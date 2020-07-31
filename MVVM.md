# MVVM架構(Model-View-ViewModel)
MVVM是Model-View-ViewModel的簡稱，三者扮演的角色為：
![](2020-06-06-10-28-55.png)
- Model：管理資料來源如API和本地資料庫
- View：顯示UI和接收使用者動作
- ViewModel：從Model取得View所需的資料
---
## MVVM 架構有幾個主要的優點：
- 資料驅動：  
事件都透過資料的變化來觸發，資料成為最關鍵的因素。
下層元件不需要知道上層元件：
在 MVC / MVP 中，都需要有 View 的引用來更新 UI。但在 MVVM 中，由View 主動觀察資料，在資料變化後收到通知，而自動更新。 ViewModel 不需要知道 View 是誰。
- 職責分離：  
ViewModel 中可以減少大量通知 View 的程式碼，專心的管理流程。
Activity 和 Fragment 不用儲存資料狀態，可以專心管理介面的操作和顯示，並妥善控制生命週期。  

---
## 實例:  
https://github.com/MarkWithall/worlds-simplest-csharp-wpf-mvvm-example/tree/C%236.0  

---
## 結構
- Services  
這下面的分類隨意，與畫面無關的邏輯統一放這或是共用方法
- Views (主要放置畫面的地方)
我也會把控制項、Template也放在這下面
- ViewModels
    - Base
        - BindableBase.cs (實作INotifyPropertyChanged)
        - Command.cs (實作ICommand)
        - ViewModelBase.cs (ViewModel繼承的基底) 主要把常用的service初始化好，每個畫面都有的物件，都需要初始化的內容放裡面
- Models (資料模型)
- Helper (延伸工具)
    - Extensions.cs (擴充功能)
- Theme (專門放跟XAML有關的東西)
    - Constants.xaml (常數類，字型等)
    - Images.xaml (圖片)
    - Styles.xaml (控制項樣式)
- Images (就是放圖片)
---
## MVVM原理
- Binding(繫結)

ViewModel實作`INotifyPropertyChanged`
