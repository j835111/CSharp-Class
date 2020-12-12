# 協程(Coroutine)

## Coroutine 與 Thread
在系統層面上，coroutine 通常和 fiber 是一樣的意思。Fiber 是一種特別的 user thread，如同一般的 thread 那樣具有自己的 call stack 與 program counter，但具備了以下的特性：

- Thread 不需自己定義中斷點（也就是呼叫 yield 的地方），而是讓 OS 或 thread library 來決定是否進行 context switch，通常由執行時間來判斷。然而 fiber 需要自行定義中斷點，context switch 只會發生在明確呼叫 yield 的地方。
- 因為 context switch 是由使用者自行控制，因此 fiber 通常 不需要 mutex 之類的東西來避免 race condition。
- Kernel thread 會進入 OS 的排程中，在多核心的 CPU 上可能會使用不同的核心同時執行許多 kernel thread。但 fiber 屬於特別種類的 user thread，它無法利用多核心進行平行處理。

## 平行處理

```C#
Parallel.For
Parallel.ForEach

Parallel.ForEach(vodPlaylist, new ParallelOptions() { MaxDegreeOfParallelism = ServicePointManager.DefaultConnectionLimit - 1 }, (part, loopState) =>
               {
                   int retryCounter = 0;

                   bool success = false;

                           using (WebClient downloadClient = new WebClient())
                           {
                               byte[] bytes = downloadClient.DownloadData(part.RemoteFile);

                               Interlocked.Increment(ref completedPartDownloads);

                               FileSystem.DeleteFile(part.LocalFile);

                               File.WriteAllBytes(part.LocalFile, bytes);

                               long completed = Interlocked.Read(ref completedPartDownloads);
                           }                 
               });
```

## 非同步模式
- 以工作為基礎的非同步模式 (TAP)  
以工作為基礎的非同步模式 (點) 是以 System.Threading.Tasks.Task System.Threading.Tasks.Task<TResult> 命名空間中的和類型為基礎，而這些類型 System.Threading.Tasks 是用來代表任意非同步作業。 TAP 是進行新的開發工作時，建議使用的非同步設計模式。

- 事件架構非同步模式 (EAP)  
要同時執行許多工作，還能繼續回應使用者互動，這樣的應用程式通常都需要可以使用多執行緒的設計。 System.Threading 命名空間提供建立高效能多執行緒應用程式的所有必要工具，但是要有效地使用這些工具，需要具備多執行緒軟體工程的豐富經驗。 對於較簡單的多執行緒應用程式，BackgroundWorker 元件提供了簡單明瞭的方案。 如果是較為複雜精細的非同步應用程式，請考慮實作遵守事件架構非同步模式的類別。

- 非同步程式設計模型 (APM)  
同時會有許多請求，每個請求分開處理(Server端的處置方式，ex:ASP.Net)

範例速覽:  
若要快速比較三個模式如何建立非同步作業的模型，請考慮 Read 方法，它會將指定的資料量讀取到所提供的緩衝區，並且在指定的位移處開始：

```C#
//normal
public class MyClass  
{  
    public int Read(byte [] buffer, int offset, int count);  
}

//此方法的 TAP 對應項會公開下列單一 ReadAsync 方法：
public class MyClass  
{  
    public Task<int> ReadAsync(byte [] buffer, int offset, int count);  
}  

//EAP 對應項會公開下列類型和成員的集合：
public class MyClass  
{  
    public void ReadAsync(byte [] buffer, int offset, int count);  
    public event ReadCompletedEventHandler ReadCompleted;  
}

//APM 對應項會公開 BeginRead 與 EndRead 方法：
public class MyClass  
{  
    public IAsyncResult BeginRead(  
        byte [] buffer, int offset, int count,
        AsyncCallback callback, object state);  
    public int EndRead(IAsyncResult asyncResult);  
}  
```

## Task詳解
Task是C#用來實作非同步方法的基礎支援類別，必須要搭配`async`和`await`來使用。

[**原理**](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/concepts/async/)

PS. ex2:
```C#
private async Task Test()
        {
            using (var httpClient = new HttpClient())
            {
                var result = await httpClient.GetAsync("https://www.google.com/");
                if (result.IsSuccessStatusCode)
                {
                    //TODO
                }
            }
        }
```

`遇到await時進行**切換**，async做為修飾詞存在，修飾方法(method)非同步方法，方法名稱須加上Async作為標示，回傳型別改為Task<T>`

使用方法:
```C#
Task.Factory.StartNew(Action action, CancellationToken cancellationToken);
Task.Factory Task StartNew(Action action);

public static Task Run(Action action);
public static Task Run(Action action, CancellationToken cancellationToken);

//using:
private async Task Test()
{
    await Task.Run(DoSomething);

    Task task = new Task(DoSomething);
    task.Start();
}

private void DoSomething()
{
    //TODO
}

//有回傳值
private int DoSomething()
{
     //TODO
    return 0;
}

private async Task Test()
{
    int result = await Task<int>.Run(DoSomething);
}

//從同步函式執行非同步函式(應用在Command中)
public ICommand ExcuteCommand
{
    get => new Command(async() => await Test());
}
```
決定結束後是否要由同一個執行序繼續執行。
```C#
public ConfiguredTaskAwaitable ConfigureAwait(bool continueOnCapturedContext);
```

取消

取消是藉由`CancellationTokenSource`Cancel
```C#
CancellationTokenSource cts = new CancellationTokenSource();

Task.Delay(1000, cts.Token);

cts.Cancel;
cts.Dispose();
```

接續

```C#
public Task ContinueWith(Action<Task> continuationAction);
```
