# 反射 (Reflection)
反射提供描述程式集、模組Type和類型的物件（類型）。 您可以使用反射來動態建立類型的執行個體、將類型繫結至現有的物件，或從現有的物件取得類型，並叫用其方法或存取其欄位及屬性。如果您在程式碼中使用屬性，則反射可讓您存取它們。 

下面是一個簡單的反射示例，使用GetType()方法 （由Object基類中所有類型的繼承） 來獲取變數的類型：
```C#
// Using GetType to obtain type information:
int i = 42;
Type type = i.GetType();
Console.WriteLine(type);

//output:
//System.Int32
```
下列範例使用反映以取得所載入組件的完整名稱。
```C#
// Using Reflection to get information of an Assembly:
Assembly info = typeof(int).Assembly;
Console.WriteLine(info);

//output
//System.Private.CoreLib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e
```

## 優缺點
### 優點：

- 反射提高了程序的擴展和擴展性。
- 降低重組性，提高自適應能力。
- 它允許程序創建和控制任何類的對象，無需提前硬編碼目標類。
### 缺點：

- 性能問題：使用反射基本上是一種解釋操作，用於指向和方法連接時要遠慢於直接代碼。因此反射機制主要應用在對所述和擴展性要求很高的系統框架上，普通程序不建議使用。
- 使用反射會模糊程序內部邏輯；程序員希望在源代碼中看到程序的邏輯，反射卻繞過了源代碼的技術，由此會帶來維護的問題，反射代碼比相應的直接代碼更複雜。
## 反射（Reflection）的用途

- 它允許在運行時查看特性（屬性）信息。  
- 它允許審查集合中的各種類型，以及實例化這些類型。  
- 它允許延遲綁定的方法和屬性（property）。  
- 它允許在運行時創建新類型，然後使用這些類型執行一些任務。

## 反射的用法
- 使用 Assembly 來定義和載入組件，載入列在組件資訊清單中的模組，然後從這個組件中尋找類型並建立它的執行個體。
- 使用 Module 來探索資訊，例如包含模組的組件以及模組中的類別。 您也可以取得所有全域方法，或在模組上所定義的其他特定非全域方法。
- 使用 ConstructorInfo 來探索資訊，例如名稱、參數、存取修飾詞 (例如 public 或 private)，以及建構函式的實作詳細資料 (例如 abstract 或 virtual)。 使用 Type 的 GetConstructors 或 GetConstructor 方法來叫用特定的建構函式。
- 使用 MethodInfo 來探索資訊，例如名稱、傳回類型、參數、存取修飾詞 (例如 public 或 private)，以及方法的實作詳細資料 (例如 abstract 或 virtual)。 使用 Type 的 GetMethods 或 GetMethod 方法來叫用特定的方法。
- 使用 FieldInfo 來探索資訊，例如名稱、存取修飾詞 (例如 public 或 private)，以及欄位的實作詳細資料 (例如 static)，並取得或設定欄位值。
- 使用 EventInfo 來探索資訊，例如名稱、事件處理常式資料類型、自訂屬性、宣告類型，以及事件的反映類型，並加入或移除事件處理常式。
- 使用 PropertyInfo 來探索資訊，例如名稱、資料類型、宣告類型、反映類型和唯讀或可寫入的屬性狀態，並且取得或設定屬性值。
- 使用 ParameterInfo 來探索資訊，例如參數的名稱、資料類型、參數是否為輸入或輸出參數以及方法簽章中的參數位置。
- 當您在應用程式定義域的僅限反映之內容中工作時，請使用 CustomAttributeData 來探索自訂屬性的相關資訊。 CustomAttributeData 可讓您檢查屬性而不需建立它們的執行個體。

