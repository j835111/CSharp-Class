# 屬性 (Attribute)
Attribute 是用於在運行時傳遞程序中各種元素（比如類別、方法、結構、枚舉、組件等）的行為訊息的聲明性標籤。您可以通過使用Attribute向程式添加聲明性訊息。一個聲明性標籤是通過放置在它所應用的元素前面的方括號（[ ]）來描述的。

.Net框架提供了兩種類型的特性：預定義特性和自定義特性。

-> 想像成標籤(tag)

## 預定義 Attribute
.Net 框架提供了三種預定義Attribute：

- AttributeUsage
- Conditional
- Obsolete

### AttributeUsage
指定另一個Attribute類別的使用方式。  
AttributeUsageAttribute 的三個屬性是藉由定義下列參數來設定：
- ValidOn  
這個位置參數會指定可放置指定屬性的程式元素。 您可以放置屬性的所有可能專案集合都會列在 AttributeTargets 列舉中。 您可以使用位 OR 運算結合數個 AttributeTargets 值，以取得所需的有效程式元素組合。
- AllowMultiple  
這個指名的參數會指定指定的程式專案是否可以多次指定指示的屬性。
- Inherited  
這個具名引數會指定衍生類別是否可以繼承指定的屬性，以及覆寫成員。

### Conditional
這個預定義Attribute標記了一個條件方法，其執行依賴於指定的預處理指示詞。

它會引起方法調用的條件編譯，取決於指定的值，比如Debug或Trace。例如，當調試代碼時顯示變量的值。

```C#
#define DEBUG
using System ;
using System.Diagnostics ;
public class Myclass
{
    [ Conditional ( "DEBUG" ) ]
    public static void Message ( string msg )
    {
        Console . WriteLine ( msg ) ;
    }
}
class Test
{
    static void function1 ( )
    {
        Myclass . Message ("In Function 1." ) ;
        function2 ( ) ;
    }
    static void function2 ( )
    {
        Myclass . Message ( "In Function 2." ) ;
    }
    public static void Main ( )
    {
        Myclass . Message ( "In Main function." ) ;
        function1 ( ) ;
        Console . ReadKey ( ) ;
    }
}

//output:
//In Main function Main function 
//In Function 1In Function 1  
//In Function 2In Function 2  
```

### Obsolete
這個預定義Attribute標記了不應被使用的程式實體。它可以讓您通知編譯器丟棄某個特定的目標元素。例如，當一個新方法被用在一個類中，但是您仍然想要保持類中的舊方法，您可以通過顯示一個應該使用新方法，而不是舊方法的消息，來把它標記為obsolete（過時的）。

規定該Attribute的語法如下：

[Obsolete(message)]  
[Obsolete(message, iserror)]  

- message，是一個string，描述項目為什麼過時以及該替代使用什麼。  
-  iserror，是一個布林值。如果該值為true，編譯器應把該項目的使用當作一個錯誤。默認值是false（編譯器生成一個警告）。

```C#
using System ;
public class MyClass
{
   [Obsolete("Don't use OldMethod, use NewMethod instead" , true)]
   static void OldMethod ( )
   {
      Console . WriteLine ( "It is the old method" ) ;
   }
   static void NewMethod ( )
   {
      Console . WriteLine ( "It is the new method" ) ;
   }
   public static voidMain ( )
   {
      OldMethod ( ) ;
   }
}
```
## 自定義Attribute
```C#
[AttributeUsage(AttributeTargets.All)]
public class DeveloperAttribute : Attribute
{
    // Private fields.
    private string name;
    private string level;
    private bool reviewed;

    // This constructor defines two required parameters: name and level.

    public DeveloperAttribute(string name, string level)
    {
        this.name = name;
        this.level = level;
        this.reviewed = false;
    }

    // Define Name property.
    // This is a read-only attribute.

    public string Name
    {
        get => name;
    }

    // Define Level property.
    // This is a read-only attribute.

    public string Level
    {
        get => level;
    }

    // Define Reviewed property.
    // This is a read/write attribute.

    public bool Reviewed
    {
        get => reviewed
        set 
        {
            reviewed = value;
        }
    }
}

[Developer("Joan Smith", "1")]

-or-

[Developer("Joan Smith", "1", Reviewed = true)]
```

取得Attribute的方式:
- ``` Attribute.GetCustomAttribute()``` (正規方式，但很少用)
- 反射(Reflection)
