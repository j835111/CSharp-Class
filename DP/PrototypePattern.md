# 原型模式 (Prototype Pattern)
複製一個已存在的物件，來產生新的物件。  
- 淺複製(shallow clone)：僅複製舊物件內的屬性，舊物件內的物件不複製。所以新、舊物件會共用這些其他物件。  
- 深複製(deep clone)：舊物件內的屬性、參考到的其他物件，都會複製一份。

```C#
public class Person
    {
        public int Age;
        public string Name;
        public int Id;

        public Person ShallowCopy()
        {
            return (Person)this.MemberwiseClone();
        }

        public Person DeepCopy()
        {
            Person other = (Person)this.MemberwiseClone();
            other.Id = Id;
            other.Name = Name.Clone().ToString();
            return other;
        }
    }
```