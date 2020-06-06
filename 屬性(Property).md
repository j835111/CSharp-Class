# 屬性(Property)
屬性是提供彈性機制以讀取、寫入或計算私用欄位值的成員。 使用屬性時可將其視為公用資料成員，但實際上屬性是名為「存取子」** 的特殊方法。 如此可讓資料更容易存取，同時有助於提升方法的安全性和彈性。

```C#
public class Date
{
    private int _month = 7;  // Backing store

    public int Month
    {
        get => _month;
        set
        {
            if ((value > 0) && (value < 13))
            {
                _month = value;
            }
        }
    }
}
```