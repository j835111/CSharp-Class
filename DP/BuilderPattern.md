# 建造者模式(生成器模式) (Builder Pattern)
將某種類產品，生產步驟整理出來。
所有要生產這類產品的 class，均要實現這些標準化步驟。
另外，為避免實際生產時，遺漏某步驟，統一由一個 class，執行一系列的生產步驟。


範例：
製做大杯珍珠奶茶、小杯紅茶。

希望達成如下的效果
```C#
static void Main(string[] args)
{
    Director aa = new Director();
    Bulider bb = new 大杯珍奶();
    aa.setBulider(bb);
    aa.create();
 
    Console.WriteLine("----------");
 
    bb = new 小杯紅茶();
    aa.setBulider(bb);
    aa.create();
 
    Console.ReadLine();
}
```



實現重點在於，大杯珍奶、小杯紅茶物件，都能傳給 Director 操作。

```C#
//標準化的生產步驟
interface Bulider
{
    void 拿杯子();
 
    void 裝飲料();
 
    void 加蓋子();
 
    void 拿吸管();
}
 
//大杯奶茶生產過程，實作 Bulider 介面
class 大杯珍奶 : Bulider
{
    public void 拿杯子()
    {
        Console.WriteLine("拿大杯子");
    }
    public void 裝飲料()
    {
        Console.WriteLine("裝珍珠、裝奶茶");
    }
 
    public void 加蓋子()
    {
        Console.WriteLine("拿大蓋子加蓋");
    }
 
    public void 拿吸管()
    {
        Console.WriteLine("拿粗吸管");
    }
}
 
//小杯紅茶生產過程，實作 Bulider 介面
class 小杯紅茶 : Bulider
{
    public void 拿杯子()
    {
        Console.WriteLine("拿小杯子");
    }
    public void 裝飲料()
    {
        Console.WriteLine("裝紅茶");
    }
 
    public void 加蓋子()
    {
        Console.WriteLine("拿小蓋子加蓋");
    }
 
    public void 拿吸管()
    {
        Console.WriteLine("拿細吸管");
    }
}
 
//統一由指揮者 class 執行生產步驟
class Director
{
    private Bulider builder;
 
    public void setBulider(Bulider builder)
    {
        this.builder = builder;
    }
 
    public void create()
    {
        this.builder.拿杯子();
        this.builder.裝飲料();
        this.builder.加蓋子();
        this.builder.拿吸管();
    }
}
```