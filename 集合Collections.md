# 集合 (Collections)

## 在開始之前:IEnumerable<T>、Enumerator
- IEnumerable<T>
```C#
public interface IEnumerable<[NullableAttribute(2)] out T> : IEnumerable
    {
        IEnumerator<T> GetEnumerator();
    }
```
可以看到 .Net 對「是否可被列舉」這件事的定義上，就是認定是否具備取得「列舉器」(Enumerator) 的能力。
- 、Enumerator   
負責將它所附屬的資料集合中的元素，一個一個的取出並回傳。  
**是可以foreach的關鍵**
---
C#中的集合總共有以下幾種:

## 簡明清單的集合類型 
- Array  
最基本的陣列
```C#
int[] a = new int[2]; //array必須是固定大小，相比List<T>可以擴充(非固定長度(fixed-size))
int[] a = new int[] { 1, 2 }; //有支援列舉，所以可以直接賦予內容
foreach (var item in a) //有實作列舉器，所以可以foreach
{
     Console.WriteLine(item);
}
```

- Stack<T>  
表示物件的後進先出 (LIFO) 集合。
    - Push() 將元素插入至Stack<T>頂端。
    - Pop() 從Stack<T>頂端移除元素 。
    - Peek() 傳回位於頂端的元素，但不會將元素從Stack<T>移除。
```C#
Stack<string> numbers = new Stack<string>();
        numbers.Push("one");
        numbers.Push("two");
        numbers.Push("three");
        numbers.Push("four");
        numbers.Push("five");

        //有實作Enumerator 所以可以foreach
        foreach( string number in numbers )
        {
            Console.WriteLine(number);
        }

        Console.WriteLine("Peek at next item to destack: {0}",
            numbers.Peek()); //拿出最晚放進去的元素，而不移除
        Console.WriteLine("Popping '{0}'", numbers.Pop()); //拿出最晚的元素，但是會從序列中移除它
```

- Queue<T>  
表示物件的先進先出 (FIFO) 集合。
    - Enqueue() 將元素加入至Queue<T>的結尾。
    - Dequeue() 從Queue<T>的開頭移除最舊的元素。
    - Peek() 會傳回位於 Queue<T>開頭最舊的元素，但不會將它從Queue<T>移除。
```C#
Queue<string> numbers = new Queue<string>();
        numbers.Enqueue("one");
        numbers.Enqueue("two");
        numbers.Enqueue("three");
        numbers.Enqueue("four");
        numbers.Enqueue("five");

        //有實作Enumerator 所以可以foreach
        foreach( string number in numbers )
        {
            Console.WriteLine(number);
        }

        Console.WriteLine("Peek at next item to dequeue: {0}",
            numbers.Peek()); //拿出最早放進去的元素，而不移除
        Console.WriteLine("Dequeuing '{0}'", numbers.Dequeue()); //拿出最早的元素，但是會從序列中移除它
```

- HashSet<T>  
物件不可重複的集合
```C#
HashSet<int> evenNumbers = new HashSet<int>();
            bool result =  evenNumbers.Add(1); //從result值可知加入集合是否成功，如已經有一樣的內容就會加入失敗
```

- List<T>  
最常用的物件集合，可自由延展長度

## 字典種類的集合類型
- Dictionary<TKey,TValue>  
表示索引鍵和值的集合。  
`TKey不能重複也不能為null`
```C#
Dictionary<string, string> openWith = new Dictionary<string, string>();

openWith.Add("txt", "notepad.exe");
openWith.Add("bmp", "paint.exe");
openWith.Add("dib", "paint.exe");
openWith.Add("rtf", "wordpad.exe");

openWith.Add("txt", "winword.exe"); //會丟出ArgumentException例外，因為key:text已經存在了。

//Dic最大特色就是可以用key來存取value
Console.Write($"value of key is:{openWith["txt"]}");
//value of key is:notepad.exe

//有實作索引器，所以也可以foreach
foreach(var item in openWith)
            {
                Console.WriteLine($"Key:{item.Key} Value:{item.Value}");
            }