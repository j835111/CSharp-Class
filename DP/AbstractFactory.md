# 抽象工廠模式 (Abstract Factory Pattern)

說明：
抽象工廠類別，可返回同類型的工廠。這些返回的工廠，有多個相同的方法，做類似的事。

範例：
有 A、B 兩個品牌，都有生產 玩具車、玩具狗 這兩個商品。
當然 A、B 品牌生產出來的 玩具車、玩具狗，是不一樣的。

希望達成目標:
```C#
static void Main(string[] args)
{
    AbstractFactory xx; // 抽像工廠
    ToyCar Car; // 製造玩具車的工廠
    ToyDog Dog; // 製造玩具狗的工廠
 
    // A 品牌
    xx = new Factory_A();
    Car = xx.CreateToyCar();
    Dog = xx.CreateToyDog();
 
    Car.MakeToyCar();
    Dog.MakeToyDog();
 
    Console.WriteLine("----------");
 
    // B 品牌
    xx = new Factory_B();
    Car = xx.CreateToyCar();
    Dog = xx.CreateToyDog();
 
    Car.MakeToyCar();
    Dog.MakeToyDog();
 
    Console.ReadLine();
}
```

TODO:
```C#
//生產玩具車的抽像類別
abstract class ToyCar
{
    public abstract void MakeToyCar();
}
//生產玩具狗的抽像類別
abstract class ToyDog
{
    public abstract void MakeToyDog();
}
 
//A 品牌玩具車
class ToyCar_A : ToyCar
{
    public override void MakeToyCar()
    {
        Console.WriteLine("A 品牌 製造的 玩具車");
    }
}
 
//A 品牌玩具狗
class ToyDog_A : ToyDog
{
    public override void MakeToyDog()
    {
        Console.WriteLine("A 品牌 製造的 玩具狗");
    }
}
 
//B 品牌玩具車
class ToyCar_B : ToyCar
{
    public override void MakeToyCar()
    {
        Console.WriteLine("B 品牌 製造的 玩具車");
    }
}
 
//B 品牌玩具狗
class ToyDog_B : ToyDog
{
    public override void MakeToyDog()
    {
        Console.WriteLine("B 品牌 製造的 玩具狗");
    }
}
 
// 抽象工廠
interface AbstractFactory
{
    ToyCar CreateToyCar();
    ToyDog CreateToyDog();
}
 
// 回傳 A 品牌，實做的物件
class Factory_A : AbstractFactory
{
    public ToyCar CreateToyCar()
    {
        return new ToyCar_A();
    }
 
    public ToyDog CreateToyDog()
    {
        return new ToyDog_A();
    }
}
 
// 回傳 B 品牌，實做的物件
class Factory_B : AbstractFactory
{
    public ToyCar CreateToyCar()
    {
        return new ToyCar_B();
    }
 
    public ToyDog CreateToyDog()
    {
        return new ToyDog_B();
    }
}
```