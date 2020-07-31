# 事件 (Event)
.NET 中的事件是以委派模型為基礎。事件發送者會推播事件已發生的通知，而事件接收器會收到該通告並定義對它的回應。 

- `EventHandler`  
`public delegate void EventHandler(object sender, EventArgs e);`
- `EventHandler<TEventArgs>`  
`public delegate void EventHandler<TEventArgs>(object sender, TEventArgs e);`
---
## EventHandler
```C#
static void Main(string[] args)
        {
            Class1 class1 = new Class1();
            class1.ToDoEvent += ToDo;

            Console.WriteLine();
            Console.ReadLine();
        }

        private static void ToDo(object sender,EventArgs e)
        {
            //TODO
        }

public class Class1
    {
        public EventHandler ToDoEvent;

        public Class1()
        {
            Timer timer = new Timer(10000)
            {
                AutoReset = true
            };

            timer.Elapsed += (sender, e) =>
            {
                ToDoEvent?.Invoke(this, EventArgs.Empty);
            };
        }
    }
```
---
## `EventHandler<TEventArgs>`
用在需要傳參數時。
```C#
class Program
    {
        static void Main(string[] args)
        {
            Class1 class1 = new Class1();
            class1.ToDoEvent += ToDo;

            Console.WriteLine();
            Console.ReadLine();
        }

        private static void ToDo(object sender, CountEventArgs e)
        {
            //TODO
            Console.WriteLine(e.Count);
        }
    }

    public class Class1
    {
        public EventHandler<CountEventArgs> ToDoEvent;

        public Class1()
        {
            Timer timer = new Timer(10000)
            {
                AutoReset = true
            };

            int count = 0;

            timer.Elapsed += (sender, e) =>
            {
                ToDoEvent?.Invoke(this, new CountEventArgs(count));
            };
        }
    }

    public class CountEventArgs : EventArgs
    {
        public int Count { get; }

        public CountEventArgs(int count)
        {
            Count = count;
        }
    }
```