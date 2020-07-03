# 泛型 (Generic)
泛型引進 .NET 類型參數的概念，讓您可以設計類別和方法，以延遲一或多個類型的規格，直到用戶端程式代碼宣告並具現化類別或方法為止。 例如，藉由使用泛型型別參數 T ，您可以撰寫其他用戶端程式代碼可以使用的單一類別，而不會產生執行時間轉換或裝箱作業的成本或風險。

```C#
List<int> list = new List<int>(){ 1, 2, 3 };
Console.Write(typeof(list[0]));

//output:System.Int32

// Declare the generic class.
public class GenericList<T>
{
    public void Add(T input) { }
}
class TestGenericList
{
    private class ExampleClass { }
    static void Main()
    {
        // Declare a list of type int.
        GenericList<int> list1 = new GenericList<int>();
        list1.Add(1);

        // Declare a list of type string.
        GenericList<string> list2 = new GenericList<string>();
        list2.Add("");

        // Declare a list of type ExampleClass.
        GenericList<ExampleClass> list3 = new GenericList<ExampleClass>();
        list3.Add(new ExampleClass());
    }
}
```
---
## 泛型總覽
- 使用泛型型別以最佳化程式碼重複使用、型別安全和效能。
- 泛型的最常見用法是建立集合類別。
- .NET 類別庫包含命名空間中的數個泛型集合類別 System.Collections.Generic 。 您應該盡可能使用這些類別，而不是使用類似在 System.Collections 命名空間中的 ArrayList 類別。
- 您可以建立自己的泛型介面、類別、方法、事件和委派。
- 泛型類別可限制為允許存取特定資料類型上的方法。
- 泛型資料類型中所使用的類型相關資訊，可在執行階段透過反映取得。
---
## 泛型型別參數
在泛型型別或方法定義中，當型別參數建立泛型型別的執行個體時，它們是用戶端指定之特定類型的預留位置。 若要使用 `GenericList<T>`，用戶端程式碼必須在角括弧內指定型別引數，宣告並具現化建構的類型。 此特定類別的型別引數可以是由編譯器辨識出的任何類型。 您可以建立任何數目的建構類型執行個體，每一個使用不同的型別引數，如下所示：
```C#
GenericList<float> list1 = new GenericList<float>();
GenericList<ExampleClass> list2 = new GenericList<ExampleClass>();
GenericList<ExampleStruct> list3 = new GenericList<ExampleStruct>();
```
---
## 型別參數命名方針
- 務必使用描述性的名稱命名泛型型別參數，除非單一字母名稱足以表明，而且描述性名稱不會新增值。
```C#
public interface ISessionChannel<TSession> { /*...*/ }
public delegate TOutput Converter<TInput, TOutput>(TInput from);
public class List<T> { /*...*/ }
```
- 單一字母型別參數的類型請考慮使用 T 做為型別參數名稱。
```C#
public int IComparer<T>() { return 0; }
public delegate bool Predicate<T>(T item);
public struct Nullable<T> where T : struct { /*...*/ }
```
- 描述性型別參數名稱前面務必加上 "T"。
```C#
public interface ISessionChannel<TSession>
{
    TSession Session { get; }
}
```
- 請考慮在參數名稱中指出放在型別參數上的條件約束。 例如，參數的條件約束為 ISession 可能稱為 TSession。

