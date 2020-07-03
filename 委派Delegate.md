# 委派 (Delegate)
「委派型別」代表對方法的參考，其中含有特定參數清單與傳回型別。 委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。 委託類似于在其他一些語言中找到的函數指標的概念。 與函數指標不同，委託是物件導向的和型別安全的。

-> 把方法、函式(method)當作參數、變數傳遞

EX:
```C#
class Program
{
    private delegate int Function(int obj1, int obj2);

    static void Main(string[] args)
    {
        Function addFunction = new Function(Add);
        Console.Write(addFunction(1,2));
        Console.ReadLine();
    }

    private static int Add(int i,int j)
    {
        return i + j;
    }
}

//output:2
```
---
## Action