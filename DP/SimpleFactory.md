## 簡單工廠模式 (Simple Factory Pattern)
使用 class 的靜態方法，依不同條件，取得不同物件，並用取得的物件，做類似的事情。  
缺點:要新增不同條件時，須修改到類別的靜態方法。

希望達成目標:
```C#
static void Main(string[] args)
{
    Toy aa;
    aa = ToyFactory.CreateToy("car");
    aa.MakeToy();//製造了 玩具車
 
    aa = ToyFactory.CreateToy("dog");
    aa.MakeToy();//製造了 玩具狗
 
    Console.ReadLine();
}
```

TODO:
```C#
abstract class Toy
{
    public abstract void MakeToy();
}
 
class ToyCar : Toy
{
    public override void MakeToy()
    {
        Console.WriteLine("製造了 玩具車");
    }
}
 
class ToyDog : Toy
{
    public override void MakeToy()
    {
        Console.WriteLine("製造了 玩具狗");
    }
}
 
class ToyFactory
{
    public static Toy CreateToy(String cc)
    {
        Toy obj = null;
        switch (cc)
        {
            case "car":
                obj = new ToyCar();
                break;
            case "dog":
                obj = new ToyDog();
                break;
            default:
                throw new Exception("沒有這個類別");
 
        }
        return obj;
    }
}
```