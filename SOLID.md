# SOLID
在前文物件導向的特性中，提到物件導向設計本身具有封裝、繼承、多型、抽象這些特性。

知道物件導向的特性，就可以寫出具備閱讀性、維謢性、擴充性的程式碼？答案是肯定的，但卻非常的困難。之所以困難，常見的因素列表如下。

為了方便，類別函數全部設為 Public。(未有效使用封裝的特性。)
單一類別中，混雜了許多功能，導至要修改特定功能時，相關程式碼的變動量過大。(高耦合、不符合單一職責)
資料與商業邏輯混雜在一起。(高耦合)
當出現特定需求變更時，直接變更原本程式碼。除了可能改壞原本程式功能外，也會增加維護上的麻煩。
當然還有許多因素，是筆者沒有想到或是沒有列出來的。

使用物件導向開發軟體的過程中，如果能配上Robert C. Martin提出的物件導向設計的五個原則(SOLID)：單一職責、開放封閉、里氏替換、接口隔離以及依賴反轉。這樣會更容易開發出易維護與擴展的系統。

## 物件導向設計的五原則 SOLID
- **單一職責原則(Single responsibility principle, SRP)**  
每個物件，不管是類別、函數，負責的功能，都應該只做一件事。  
對函數而言，一個函數內，同時做了兩件以上的事情。當發生錯誤時，很難快速定位錯誤的原因。另外，也容易間接導至程式碼的可閱讀性降低。

- **開放封閉原則(Open-Close principle, OCP)**  
藉由增加新的程式碼來擴充系統的功能，而不是藉由修改原本已經存在的程式碼來擴充系統的功能。  
當需求有異動時，要如何在不變動現在正常運行的程式碼，藉由繼承、相依性注入等方式，增加新的程式碼，以實作新的需求。  
假若為了新需求，去修改了原本的程式中的某一個函數，可能會造成其他呼叫使用該函數的的功能，出現非預期的錯誤。  

- **里氏替換原則(Liskov substitution principle, LSP)**
Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it.  
簡單來說，當實作繼承了 interface 或 base-class的 sub-class，那麼在程式中，只要出現該 interface 或 base-class 的部份，都可以用 sub-class 替換。

- **接口隔離原則(Interface segregation principle, ISP)**  
針對不同需求的用戶，開放其對應需求的介面，提拱使用。可避免不相關的需求介面異動，造成被強迫一同面對異動的情況。

- **依賴反轉原則(Dependency inversion principle, DIP)**  
A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
B. Abstractions should not depend on details. Details should depend on abstractions.  
當 A 模組在內部使用 B 模組的情況下，我們稱 A 為高階模組，B 為低階模組。高階模組不應該依賴於低階模組，兩者都該依賴抽象介面。

補充:
- **迪米特法則(Law of Demeter, LoD)**  
最少知識原則 Principle of Least Knowledge
只和自己眼前的朋友交談 Only talk to your immediate friends
低耦合  
[例如]  
郵差送來掛號信，須要蓋收件人印章。  
一般人不會叫郵差自己進屋找印章，既浪費時間也不安全。  
正常都是自己進屋拿，或是請其他家人幫忙拿。  
因為不應該給郵差進屋找東西的權限、郵差也不須要知道印章放在屋內何處。  

- **合成/聚合重覆使用原則(Composite/Aggregate Reuse Principle, CARP)**  
多用合成/聚合，少用繼承。  
在兩個物件有 has-a (has-parts、is-part-of)關係時 => 合成/聚合 (A has a B)  
當兩個物件有 is-a (is-a-kind-of)關係時 => 繼承 (Superman is a kind of Person)  
合成 (Composite)：A、B兩物件有合成關係時，表示其中一個物件消失(ex:書本)，另一個物件也會消失(ex:章節)。  
聚合 (Aggregate)：A、B兩物件有聚合關係時，表示其中一個物件消失(ex:球隊)，另一個物件不會消失(ex:球員)。  