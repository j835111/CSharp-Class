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
        Function addFunction = new Function(Add); //正統寫法
        Function addFunction = Add; //語法糖
        Function addFunction = (i, j) => { return i + j; }; //匿名委派
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
用來封裝**沒有回傳值**的委派

```C# 
class Program
    {
        private delegate void Function(int obj1, int obj2);

        static void Main(string[] args)
        {
            Function function = DoSomething;
            function(1, 2);
            Console.WriteLine();
            Console.ReadLine();
        }

        private static void DoSomething(int i, int j)
        {
            Console.WriteLine("input 1 is:" + i);
            Console.WriteLine("input 2 is:" + j);
        }
    }

//Action改寫

class Program
    {
        //private delegate void Function(int obj1, int obj2);

        static void Main(string[] args)
        {
            Action<int, int> action = new Action<int, int>(DoSomething); //正統寫法
            Action<int, int> action = DoSomething; //語法糖
            Action<int, int> action = (i, j) =>
             {
                 Console.WriteLine("input 1 is:" + i);
                 Console.WriteLine("input 2 is:" + j);
             }; //匿名委派
            action(1, 2);
            Console.WriteLine();
            Console.ReadLine();
        }

        private static void DoSomething(int i, int j)
        {
            Console.WriteLine("input 1 is:" + i);
            Console.WriteLine("input 2 is:" + j);
        }
    }

//無參數的Action
class Program
    {
        //private delegate void Function(int obj1, int obj2);

        static void Main(string[] args)
        {
            Action action = new Action(DoSomething);
            Action action = DoSomething;
            Action action = () =>
             {
                 //TO DO
             };
            action();
            Console.WriteLine();
            Console.ReadLine();
        }

        private static void DoSomething()
        {
            //TO DO
        }
    }
```
---
## Func
用來封裝**有回傳值**的委派

```C#
class Program
    {
        //private delegate int Function(int obj1, int obj2);

        static void Main(string[] args)
        {
            Func<int, int, int> addFunction = new Func<int, int, int>(Add); //正統寫法
            Func<int, int, int> addFunction = Add;
            Func<int, int, int> addFunction = (i, j) => { return i + j; }; 
            Console.Write(addFunction(1, 2));
            Console.ReadLine();
        }

        private static int Add(int i, int j)
        {
            return i + j;
        }
    }

//無參數的Func
class Program
    {
        //private delegate void Function(int obj1, int obj2);

        static void Main(string[] args)
        {
            Func<int> func = new Func<int>(DoSomething); //正統寫法
            Func<int> func = DoSomething; //語法糖
            Func<int> func = () => { return 0; }; //匿名委派
            func();
            Console.WriteLine();
            Console.ReadLine();
        }

        private static int DoSomething()
        {
            return 0;
        }
    }
```
---
## 課外補充 Lamdba (=>)
Lambda是匿名函式的簡化寫法  
> `(input-parameters) => { body }` 

若body為單行可不用大括號({})
```C#
Func<int, int, int> addFunction = (i, j) => i + j; //只放運算式
//也可以放完整的陳述式
Func<int, int, int> addFunction = (i, j) => { return i + j; };
//也可以放完整的單行陳述式
Func<int, int, int> addFunction = (i, j) => Add(i, j);

Func<int, int, int> addFunction = (i, j) => 
{ 
    return i + j; //多行
};

