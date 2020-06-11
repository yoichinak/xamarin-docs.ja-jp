---
title: " Xamarin.Forms Xamarin ネイティブプロジェクト内" の説明: "この記事では、xamarin のネイティブプロジェクトに直接追加された ContentPage の派生ページを使用する方法と、それらの間を移動する方法について説明します。"
ms. 製品: xamarin ms. assetid: f343fc21-dfb1-4364-a332-9da6705d36bc: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 08/19/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.FormsXamarin ネイティブプロジェクトで

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

通常、アプリケーションには Xamarin.Forms 、から派生した1つ以上のページが含まれ [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。これらのページは、.NET Standard ライブラリプロジェクトまたは共有プロジェクトのすべてのプラットフォームで共有されます。 ただし、ネイティブ形式では、 `ContentPage` ネイティブの xamarin、iOS、xamarin、および UWP アプリケーションに、の派生ページを直接追加できます。 ネイティブプロジェクトが `ContentPage` .NET Standard ライブラリプロジェクトまたは共有プロジェクトから派生したページを使用する場合と比較して、ネイティブプロジェクトにページを直接追加する利点は、ネイティブビューでページを拡張できることです。 その後、ネイティブビューを XAML で名前付けし `x:Name` 、分離コードから参照できます。 ネイティブビューの詳細については、「[ネイティブビュー](~/xamarin-forms/platform/native-views/index.md)」を参照してください。

ネイティブプロジェクトでを派生したページを使用するプロセスは、次のとおり Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) です。

1. Xamarin.FormsNuGet パッケージをネイティブプロジェクトに追加します。
1. [`ContentPage`](xref:Xamarin.Forms.ContentPage)ネイティブプロジェクトに、から派生したページと依存関係を追加します。
1. `Forms.Init` メソッドを呼び出します。
1. から派生したページのインスタンスを作成 [`ContentPage`](xref:Xamarin.Forms.ContentPage) し、次の拡張メソッドのいずれかを使用して、適切なネイティブ型に変換します: `CreateViewController` IOS、 `CreateSupportFragment` Android、または `CreateFrameworkElement` UWP。
1. [`ContentPage`](xref:Xamarin.Forms.ContentPage)ネイティブナビゲーション API を使用して、の派生ページのネイティブ型表現に移動します。

Xamarin.Forms`Forms.Init`ネイティブプロジェクトが派生ページを構築するには、メソッドを呼び出すことによって初期化する必要があり [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。 この作業を行うタイミングの選択は、主にアプリケーションフローで最も便利なタイミングに依存します。これは、アプリケーションの起動時、または `ContentPage` の派生ページが構築される直前に実行されます。 この記事および付属のサンプルアプリケーションでは、 `Forms.Init` アプリケーションの起動時にメソッドが呼び出されます。

> [!NOTE]
> この**サンプルアプリケーション**ソリューションには、プロジェクトが含まれていません Xamarin.Forms 。 代わりに、Xamarin の iOS プロジェクト、Xamarin Android プロジェクト、および UWP プロジェクトで構成されます。 各プロジェクトは、ネイティブなフォームを使用して [`ContentPage`](xref:Xamarin.Forms.ContentPage) 、の派生ページを使用するネイティブプロジェクトです。 ただし、ネイティブプロジェクトが `ContentPage` .NET Standard ライブラリプロジェクトまたは共有プロジェクトから派生したページを使用できない理由はありません。

、、およびデータバインディングエンジンなどのネイティブ形式を使用する場合は、 Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) すべてが引き続き機能 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) します。 ただし、ページナビゲーションは、ネイティブナビゲーション API を使用して実行する必要があります。

## <a name="ios"></a>iOS

IOS では、 `FinishedLaunching` クラスのオーバーライドは、 `AppDelegate` 通常、アプリケーションの起動に関連するタスクを実行する場所です。 これは、アプリケーションが起動した後に呼び出され、通常はメインウィンドウおよびビューコントローラーを構成するためにオーバーライドされます。 次のコード例は、サンプルアプリケーションのクラスを示してい `AppDelegate` ます。

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;
    UIWindow _window;
    AppNavigationController _navigation;

    public static string FolderPath { get; private set; }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
        UIViewController mainPage = new NotesPage().CreateViewController();
        mainPage.Title = "Notes";

        _navigation = new AppNavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    // ...
}
```

`FinishedLaunching` メソッドは、次のタスクを実行します。

- Xamarin.Formsは、メソッドを呼び出すことによって初期化され `Forms.Init` ます。
- クラスへの参照 `AppDelegate` は、フィールドに格納され `static` `Instance` ます。 これは、クラスで定義されているメソッドを他のクラスが呼び出すメカニズムを提供するためのものです `AppDelegate` 。
- `UIWindow`ネイティブ iOS アプリケーションのビューのメインコンテナーであるが作成されます。
- プロパティは、 `FolderPath` ノートデータが格納されるデバイス上のパスに初期化されます。
- クラスは、 `NotesPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML で定義されている派生ページであり、 `UIViewController` 拡張メソッドを使用して構築され、に変換され `CreateViewController` ます。
- の `Title` プロパティが設定され、 `UIViewController` に表示され `UINavigationBar` ます。
- `AppNavigationController`階層ナビゲーションを管理するために、が作成されます。 これは、から派生するカスタムナビゲーションコントローラークラスです `UINavigationController` 。 `AppNavigationController`オブジェクトはビューコントローラーのスタックを管理し、 `UIViewController` コンストラクターに渡されたは、が読み込まれるときに最初に表示され `AppNavigationController` ます。
- `AppNavigationController`オブジェクトがの最上位レベルとして設定され、 `UIViewController` `UIWindow` `UIWindow` がアプリケーションのキーウィンドウとして設定され、が表示されるようになります。

`FinishedLaunching`メソッドが実行されると、 Xamarin.Forms `NotesPage` 次のスクリーンショットに示すように、クラスで定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin iOS アプリケーションのスクリーンショット](native-forms-images/ios-notespage.png "XAML UI を使用した Xamarin iOS アプリ")](native-forms-images/ios-notespage-large.png#lightbox "XAML UI を使用した Xamarin iOS アプリ")

たとえばをタップするなど、UI と対話すると、実行中の **+** [`Button`](xref:Xamarin.Forms.Button) 分離コードに次のイベントハンドラーが生成され `NotesPage` ます。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `AppDelegate.Instance` フィールドを使用 `AppDelegate.NavigateToNoteEntryPage` すると、メソッドを呼び出すことができます。これを次のコード例に示します。

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    var noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

メソッドは、の `NavigateToNoteEntryPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) 派生ページを拡張メソッドを使用してに変換 `UIViewController` し、 `CreateViewController` のプロパティを設定し `Title` `UIViewController` ます。 次に、 `UIViewController` `AppNavigationController` メソッドによってがプッシュされ `PushViewController` ます。 したがって、 Xamarin.Forms 次のスクリーンショットに示すように、クラスで定義されている UI が `NoteEntryPage` 表示されます。

[![XAML で定義された UI を使用する Xamarin iOS アプリケーションのスクリーンショット](native-forms-images/ios-noteentrypage.png "XAML UI を使用した Xamarin iOS アプリ")](native-forms-images/ios-noteentrypage-large.png#lightbox "XAML UI を使用した Xamarin iOS アプリ")

が表示されると、戻るナビゲーションによって `NoteEntryPage` からクラスのがポップされ、 `UIViewController` `NoteEntryPage` `AppNavigationController` ユーザーがクラスのに返され `UIViewController` `NotesPage` ます。 ただし、 `UIViewController` iOS ネイティブナビゲーションスタックからをポップしても、 `UIViewController` アタッチされたオブジェクトとアタッチされたオブジェクトは自動的に破棄されません `Page` 。 したがって、クラスはメソッドをオーバーライドして、 `AppNavigationController` `PopViewController` 後方ナビゲーション時にビューコントローラーを破棄します。

```csharp
public class AppNavigationController : UINavigationController
{
    //...
    public override UIViewController PopViewController(bool animated)
    {
        UIViewController topView = TopViewController;
        if (topView != null)
        {
            // Dispose of ViewController on back navigation.
            topView.Dispose();
        }
        return base.PopViewController(animated);
    }
}
```

オーバーライドは、 `PopViewController` `Dispose` `UIViewController` iOS ネイティブナビゲーションスタックからポップされたオブジェクトに対してメソッドを呼び出します。 この操作を行わない `UIViewController` と、オブジェクトとアタッチされたオブジェクトが `Page` 孤立します。

> [!IMPORTANT]
> 孤立したオブジェクトはガベージコレクションできないため、メモリリークが発生します。

## <a name="android"></a>Android

Android では、 `OnCreate` クラスのオーバーライドは、 `MainActivity` 通常、アプリケーションの起動に関連するタスクを実行する場所です。 次のコード例は、サンプルアプリケーションのクラスを示してい `MainActivity` ます。

```csharp
public class MainActivity : AppCompatActivity
{
    public static string FolderPath { get; private set; }

    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Notes";

        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        Android.Support.V4.App.Fragment mainPage = new NotesPage().CreateSupportFragment(this);
        SupportFragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` メソッドは、次のタスクを実行します。

- Xamarin.Formsは、メソッドを呼び出すことによって初期化され `Forms.Init` ます。
- クラスへの参照 `MainActivity` は、フィールドに格納され `static` `Instance` ます。 これは、クラスで定義されているメソッドを他のクラスが呼び出すメカニズムを提供するためのものです `MainActivity` 。
- `Activity`コンテンツはレイアウトリソースから設定されます。 サンプルアプリケーションでは、レイアウトは、、、 `LinearLayout` `Toolbar` および `FrameLayout` フラグメントコンテナーとして機能するを含むで構成されます。
- `Toolbar`が取得され、の操作バーとして設定され、 `Activity` 操作バーのタイトルが設定されます。
- プロパティは、 `FolderPath` ノートデータが格納されるデバイス上のパスに初期化されます。
- クラスは、 `NotesPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML で定義されている派生ページであり、 `Fragment` 拡張メソッドを使用して構築され、に変換され `CreateSupportFragment` ます。
- クラスは、 `SupportFragmentManager` インスタンスをクラスのに置き換えるトランザクションを作成し、コミットし `FrameLayout` `Fragment` `NotesPage` ます。

フラグメントの詳細については、「[フラグメント](~/android/platform/fragments/index.md)」を参照してください。

`OnCreate`メソッドが実行されると、 Xamarin.Forms `NotesPage` 次のスクリーンショットに示すように、クラスで定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin Android アプリケーションのスクリーンショット](native-forms-images/android-notespage.png "XAML UI を使用した Xamarin Android アプリ")](native-forms-images/android-notespage-large.png#lightbox "XAML UI を使用した Xamarin Android アプリ")

たとえばをタップするなど、UI と対話すると、実行中の **+** [`Button`](xref:Xamarin.Forms.Button) 分離コードに次のイベントハンドラーが生成され `NotesPage` ます。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainActivity.Instance` フィールドを使用 `MainActivity.NavigateToNoteEntryPage` すると、メソッドを呼び出すことができます。これを次のコード例に示します。

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    Android.Support.V4.App.Fragment noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateSupportFragment(this);
    SupportFragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, noteEntryPage)
        .Commit();
}
```

メソッドは、 `NavigateToNoteEntryPage` から派生したページを拡張メソッドを使用してに変換し、を Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) `Fragment` `CreateSupportFragment` `Fragment` フラグメントバックスタックに追加します。 したがって、 Xamarin.Forms `NoteEntryPage` 次のスクリーンショットに示すように、で定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin Android アプリケーションのスクリーンショット](native-forms-images/android-noteentrypage.png "XAML UI を使用した Xamarin Android アプリ")](native-forms-images/android-noteentrypage-large.png#lightbox "XAML UI を使用した Xamarin Android アプリ")

が表示されると、戻る矢印をタップすると、 `NoteEntryPage` `Fragment` `NoteEntryPage` フラグメントバックスタックからのがポップされ、クラスのにユーザーが返され `Fragment` `NotesPage` ます。

### <a name="enable-back-navigation-support"></a>バックナビゲーションサポートを有効にする

`SupportFragmentManager`クラスには、 `BackStackChanged` フラグメントバックスタックの内容が変更されるたびに発生するイベントがあります。 クラスのメソッドには、 `OnCreate` `MainActivity` このイベントの匿名イベントハンドラーが含まれています。

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

このイベントハンドラーは、フラグメントバックスタックに1つ以上のインスタンスがある場合に、アクションバーに [戻る] ボタンを表示し `Fragment` ます。 [戻る] ボタンをタップする応答は、オーバーライドによって処理され `OnOptionsItemSelected` ます。

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && SupportFragmentManager.BackStackEntryCount > 0)
    {
        SupportFragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

[ `OnOptionsItemSelected` オプション] メニューの項目が選択されるたびに、オーバーライドが呼び出されます。 この実装では、[戻る] ボタンが選択されていて、フラグメントバックスタックに1つ以上のインスタンスが存在する場合、フラグメントバックスタックから現在のフラグメントをポップし `Fragment` ます。

### <a name="multiple-activities"></a>複数のアクティビティ

アプリケーションが複数のアクティビティで構成されている場合、 [`ContentPage`](xref:Xamarin.Forms.ContentPage) から派生したページを各アクティビティに埋め込むことができます。 このシナリオでは、 `Forms.Init` メソッドは `OnCreate` 、を埋め込む最初ののオーバーライドでのみ呼び出す必要があり `Activity` Xamarin.Forms `ContentPage` ます。 ただし、次のような影響があります。

- の値は `Xamarin.Forms.Color.Accent` 、メソッドを呼び出したから取得され `Activity` `Forms.Init` ます。
- の値は、 `Xamarin.Forms.Application.Current` メソッドを呼び出したに関連付けられ `Activity` `Forms.Init` ます。

### <a name="choose-a-file"></a>ファイルの選択

[`ContentPage`](xref:Xamarin.Forms.ContentPage)HTML の [ファイルの選択] ボタンをサポートする必要があるを使用するから派生したページを埋め込む場合 [`WebView`](xref:Xamarin.Forms.WebView) 、は `Activity` メソッドをオーバーライドする必要があり `OnActivityResult` ます。

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP の場合、ネイティブ `App` クラスは、通常、アプリケーションの起動に関連するタスクを実行する場所です。 Xamarin.Formsは通常、UWP アプリケーションでは、 Xamarin.Forms `OnLaunched` ネイティブクラスのオーバーライドで、 `App` メソッドに引数を渡すために初期化され `LaunchActivatedEventArgs` `Forms.Init` ます。 このため、から派生したページを使用するネイティブ UWP アプリケーション Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、メソッドからメソッドを呼び出すと最も簡単に呼び出すことができ `Forms.Init` `App.OnLaunched` ます。

既定では、ネイティブクラスによって、 `App` `MainPage` クラスがアプリケーションの最初のページとして起動されます。 次のコード例は、サンプルアプリケーションのクラスを示してい `MainPage` ます。

```csharp
public sealed partial class MainPage : Page
{
    NotesPage notesPage;
    NoteEntryPage noteEntryPage;
    
    public static MainPage Instance;
    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        notesPage = new Notes.UWP.Views.NotesPage();
        this.Content = notesPage.CreateFrameworkElement();
        // ...        
    } 
    // ...
}
```

`MainPage`コンストラクターは、次のタスクを実行します。

- ページに対してキャッシュが有効になっているので、 `MainPage` ユーザーがページに戻ったときに新しいが構築されることはありません。
- クラスへの参照 `MainPage` は、フィールドに格納され `static` `Instance` ます。 これは、クラスで定義されているメソッドを他のクラスが呼び出すメカニズムを提供するためのものです `MainPage` 。
- プロパティは、 `FolderPath` ノートデータが格納されるデバイス上のパスに初期化されます。
- クラスは、 `NotesPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML で定義されている派生ページであり、拡張メソッドを使用して構築され、に変換された `FrameworkElement` 後、 `CreateFrameworkElement` クラスのコンテンツとして設定され `MainPage` ます。

コンストラクターが実行されると `MainPage` 、 Xamarin.Forms `NotesPage` 次のスクリーンショットに示すように、クラスで定義されている UI が表示されます。

[![XAML で定義された UI を使用する UWP アプリケーションのスクリーンショット Xamarin.Forms](native-forms-images/uwp-notespage.png "を使用する UWP アプリファンド.NO LOC (Xamarin)] XAML UI")](native-forms-images/uwp-notespage-large.png#lightbox "を使用する UWP アプリファンド.NO LOC (Xamarin)] XAML UI")

たとえばをタップするなど、UI と対話すると、実行中の **+** [`Button`](xref:Xamarin.Forms.Button) 分離コードに次のイベントハンドラーが生成され `NotesPage` ます。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainPage.Instance` フィールドを使用 `MainPage.NavigateToNoteEntryPage` すると、メソッドを呼び出すことができます。これを次のコード例に示します。

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    noteEntryPage = new Notes.UWP.Views.NoteEntryPage
    {
        BindingContext = note
    };
    this.Frame.Navigate(noteEntryPage);
}
```

UWP でのナビゲーションは、通常、引数を受け取るメソッドを使用して実行され `Frame.Navigate` `Page` ます。 Xamarin.Forms`Frame.Navigate`派生ページインスタンスを受け取る拡張メソッドを定義 [`ContentPage`](xref:Xamarin.Forms.ContentPage) します。 このため、メソッドを実行すると、 `NavigateToNoteEntryPage` Xamarin.Forms `NoteEntryPage` 次のスクリーンショットに示すように、に定義されている UI が表示されます。

[![XAML で定義された UI を使用する UWP アプリケーションのスクリーンショット Xamarin.Forms](native-forms-images/uwp-noteentrypage.png "を使用する UWP アプリファンド.NO LOC (Xamarin)] XAML UI")](native-forms-images/uwp-noteentrypage-large.png#lightbox "を使用する UWP アプリファンド.NO LOC (Xamarin)] XAML UI")

が表示されると、戻る矢印をタップすると、 `NoteEntryPage` アプリ内のバックスタックからのがポップされ、その `FrameworkElement` `NoteEntryPage` ユーザーがクラスのに返され `FrameworkElement` `NotesPage` ます。

### <a name="enable-page-resizing-support"></a>ページサイズ変更のサポートを有効にする

UWP アプリケーションウィンドウのサイズを変更する場合は、 Xamarin.Forms コンテンツのサイズも変更する必要があります。 これは、イベントのイベントハンドラーをコンストラクターに登録することで実現され `Loaded` `MainPage` ます。

```csharp
public MainPage()
{
    // ...
    this.Loaded += OnMainPageLoaded;
    // ...
}
```

イベントは、 `Loaded` ページがレイアウトされ、レンダリングされ、対話できる状態になったときに発生し、応答としてメソッドを実行し `OnMainPageLoaded` ます。

```csharp
void OnMainPageLoaded(object sender, RoutedEventArgs e)
{
    this.Frame.SizeChanged += (o, args) =>
    {
        if (noteEntryPage != null)
            noteEntryPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
        else
            notesPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
    };
}
```

メソッドは、 `OnMainPageLoaded` イベントの匿名イベントハンドラーを登録し `Frame.SizeChanged` ます。これは、のプロパティまたはプロパティのいずれかがによって変更された場合に発生 `ActualHeight` `ActualWidth` `Frame` します。 応答とし Xamarin.Forms て、アクティブページのコンテンツは、メソッドを呼び出すことによってサイズが変更され `Layout` ます。

### <a name="enable-back-navigation-support"></a>バックナビゲーションサポートを有効にする

UWP では、アプリケーションは、さまざまなデバイスフォームファクターにわたって、すべてのハードウェアおよびソフトウェアの [戻る] ボタンのナビゲーションを有効にする必要があります。 これは、イベントのイベントハンドラーを登録することによって実現できます。これは、 `BackRequested` コンストラクターで実行でき `MainPage` ます。

```csharp
public MainPage()
{
    // ...
    SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
}
```

アプリケーションが起動されると、 `GetForCurrentView` メソッドは、 `SystemNavigationManager` 現在のビューに関連付けられているオブジェクトを取得し、イベントのイベントハンドラーを登録し `BackRequested` ます。 アプリケーションは、フォアグラウンドアプリケーションの場合にのみこのイベントを受信し、応答として `OnBackRequested` イベントハンドラーを呼び出します。

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
        noteEntryPage = null;
    }
}
```

`OnBackRequested`イベントハンドラーは、 `GoBack` アプリケーションのルートフレームに対してメソッドを呼び出し、 `BackRequestedEventArgs.Handled` プロパティをに設定して、イベントを処理済み `true` としてマークします。 イベントを処理済みとしてマークしないと、イベントは無視されます。

アプリケーションでは、タイトルバーに [戻る] ボタンを表示するかどうかを選択します。 これを実現するには、 `AppViewBackButtonVisibility` プロパティをクラスの列挙値のいずれかに設定し `AppViewBackButtonVisibility` `App` ます。

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

イベントの `OnNavigated` 発生に応答して実行されるイベントハンドラーは `Navigated` 、ページナビゲーションが発生したときにタイトルバーの [戻る] ボタンの表示を更新します。 これにより、アプリ内バックスタックが空でない場合はタイトルバーの [戻る] ボタンが表示され、アプリ内バックスタックが空の場合はタイトルバーから削除されます。

UWP でのバックナビゲーションサポートの詳細については、「 [uwp アプリのナビゲーション履歴と後方ナビゲーション](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)」を参照してください。

## <a name="related-links"></a>関連リンク

- [種類 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)
