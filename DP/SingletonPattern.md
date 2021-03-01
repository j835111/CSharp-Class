# 單例模式 (Singleton Pattern)

讓一個類別只能有一個實例(Instance)的方法。
產生單一實例的方式：
- 懶漢方式(Lazy initialization)：第一次使用時，才產生實例。
- 餓漢方式(Eager initialization)：class 載入時就產生實例，不管後面會不會用到。

希望達成目標:
```C#
static void Main(string[] args)
        {
            SomeService someService1 = SomeService.Instance;
            someService1.Value = 1;

            SomeService someService2 = SomeService.Instance;
            Console.WriteLine(someService1.Value == someService2.Value);
        }

        
//True
```

## 餓漢
```C#
public class SomeService
    {
        private static SomeService _this = new SomeService();

        public static SomeService Instance => _this;

        public int Value { get; set; }

        private SomeService()
        {
        }

        public void DoSomething()
        {
            //TODO
        }
    }
```
---

## 懶漢
```C#
//法1
public class SomeService
    {
        private readonly static object _lockObj = new object();

        private static SomeService _this;

        public static SomeService Instance
        {
            get
            {
                if(_this == null)
                {
                    lock (_lockObj)
                    {
                        if (_this == null)
                        {
                            _this = new SomeService();
                        }
                    }
                }

                return _this;
            }
        }

        public int Value { get; set; }

        private SomeService()
        {
        }

        public void DoSomething()
        {
            //TODO
        }
    }

//法2
public class SomeService
    {
        private static Lazy<SomeService> _lazy = new Lazy<SomeService>();

        public static SomeService Instance => _lazy.Value;

        public int Value { get; set; }

        private SomeService()
        {
        }

        public void DoSomething()
        {
            //TODO
        }
    }
```