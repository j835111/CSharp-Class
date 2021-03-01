# 工廠方法模式 (Factory Method Pattern)

說明：  
為避免了簡單工廠模式，對工廠類靜態方法的修改(對修改應該是封閉的)。
實作方式，將簡單工廠的工廠類靜態方法，再拆成不同的小子類。

希望達成目標:
```C#
static void Main(string[] args)
{
    // 工廠方法模式寫法
    ToyFactory xx;
    Toy aa;
 
    xx = new ToyCarFactory();
    aa = xx.CreateToy();
    aa.MakeToy();//製造了 玩具車
 
    xx = new ToyDogFactory();
    aa = xx.CreateToy();
    aa.MakeToy();//製造了 玩具車
 
 
    /* 比較：簡單工單模式寫法
    Toy aa;
    aa = ToyFactory.CreateToy("car");
    aa.MakeToy();//製造了 玩具車
 
    aa = ToyFactory.CreateToy("dog");
    aa.MakeToy();//製造了 玩具狗
    */
    Console.ReadLine();
}
```

TODO:
```C#
abstract class Toy
{
    public abstract void MakeToy();
}

// 工廠方法模式寫法
interface ToyFactory
{
    Toy CreateToy();
}
 
class ToyCar : Toy
{
    private string _status;

    public ToyCar(string status)
    {
        _status = status;
    }

    public override void MakeToy()
    {
        Console.WriteLine("製造了 " + status + "玩具車");
    }
}
 
class ToyDog : Toy
{
    public override void MakeToy()
    {
        Console.WriteLine("製造了 玩具狗");
    }
}
 
class ToyCarFactory : ToyFactory
{
    public Toy CreateToy()
    {
        return new ToyCar();
    }

    public Toy CreateToy(string status)
    {
        return = new ToyCar(status);
    }
}
 
class ToyDogFactory : ToyFactory
{
    public Toy CreateToy()
    {
        return new ToyDog();
    }
}
 
/* 比較：簡單工單模式寫法
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
*/
```