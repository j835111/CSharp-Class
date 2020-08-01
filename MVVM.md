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
- Binding (繫結)

ViewModel實作`INotifyPropertyChanged`

```C#
public abstract class BindableBase : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void SetProperty<T>(ref T storage, T value, [CallerMemberName]string propertyName = null)
        {
            if (Equals(storage, value))
            {
                return;
            }

            OnPropertyChanged(propertyName);
        }

        protected virtual void OnPropertyChanged([CallerMemberName]string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
```
更新時Call SetProperty發出PropertyChanged的event通知view更改內容

ViewModelBase繼承BindableBase，目的在於放入初始化基底函式
```C#
public virtual Task InitializeAsync(params object[] parameters)
        {
            return Task.FromResult(false);
        }
```
以及引入常用service
```C#
protected readonly INavigationService NavigationService = ViewModelLocator.Resolve<INavigationService>();
```
ViewModel再去繼承ViewModelBase實作畫面內容。
```C#
public class LoginViewModel : ViewModelBase
    {
        #region Fields

        private string _text;

        #endregion

        #region Properties

        public string Text
        {
            get => _text;
            set => SetProperty(ref _text, value);
        }

        #endregion
    }

//XAML
<Label Content="{Binding Text}"/>
```     
**BindingMode**  
在binding時有個BindingMode
- TwoWay  
每當目標屬性或來源屬性變更時，會更新目標屬性和來源屬性。
- OneWay  
只有在來源屬性變更時，才會更新目標屬性。
- OneTime  
只有在應用程式啟動或 DataContext 進行變更時，才會更新目標屬性。
- OneWayToSource  
當目標屬性變更時，更新來源屬性。
- Default
根據控制項不同會是TwoWay或OneWay。

**StringFormat**  
可以把綁定的文字做格式化顯示
```C#
<Label Content="{Binding Text,StringFormat=顯示的文字是:{0}}"/>
```
---
- Command (命令)
可操作的控制項(Button之類的)，是要如何綁定一個method上去的呢?
利用Command
```C#
public static readonly DependencyProperty CommandProperty;

public ICommand Command { get; set; }
```
先實作Icommand
```C#
public class Command : ICommand
    {
        #region Fields

        private readonly Func<object, bool> _canExecute;
        private readonly Action<object> _execute;

        #endregion

        #region Constructors

        public Command(Action<object> execute)
        {
            _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        }

        public Command(Action execute) : this(o => execute())
        {
            if (execute == null)
                throw new ArgumentNullException(nameof(execute));
        }

        public Command(Action<object> execute, Func<object, bool> canExecute) : this(execute)
        {
            if (canExecute == null)
                throw new ArgumentNullException(nameof(canExecute));

            _canExecute = canExecute;
        }

        public Command(Action execute, Func<bool> canExecute) : this(o => execute(), o => canExecute())
        {
            if (execute == null)
                throw new ArgumentNullException(nameof(execute));
            if (canExecute == null)
                throw new ArgumentNullException(nameof(canExecute));
        }

        #endregion

        #region Methods

        public bool CanExecute(object parameter)
        {
            if (_canExecute != null)
                return _canExecute(parameter);

            return true;
        }

        public void Execute(object parameter)
        {
            _execute(parameter);
        }

        #endregion

        #region Event

        public event EventHandler CanExecuteChanged;

        #endregion
    }
```
在ViewModel裡寫上function
```C#
public class LoginViewModel : ViewModelBase
    {
        #region Fields

        private ICommand _doCommand;

        #endregion

        #region Properties

        public ICommand DoCommand 
        { 
            get
            {
                if (_doCommand == null)
                    _doCommand = new Command(DoSomething);

                return _doCommand;
            }
        }

        #endregion

        #region Methods

        private void DoSomething()
        {

        }

        #endregion
    }

//XAML
<Button Command="{Binding DoCommand}"/>
```
**CommandParameter**
這是用來傳入參數的，例如說需要輸入框的文字(比較常用是把自身傳入，存取控制項自身的屬性、功能之類的)

Command是多載原因就是這個
不過實際上`CommandParameter`用到的機會不多(個人經驗)
