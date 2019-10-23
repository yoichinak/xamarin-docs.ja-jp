---
title: Xamarin ネイティブプロジェクトでの xamarin フォーム
description: この記事では、Xamarin のネイティブプロジェクトに直接追加された ContentPage の派生ページを使用する方法と、それらの間を移動する方法について説明します。
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/19/2019
ms.openlocfilehash: 0c84b844455b8a792b8cbe2f4dac97097e5ebd97
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "69621062"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin ネイティブプロジェクトでの xamarin フォーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

通常、Xamarin アプリケーションには[`ContentPage`](xref:Xamarin.Forms.ContentPage)から派生した1つ以上のページが含まれます。これらのページは、.NET Standard ライブラリプロジェクトまたは共有プロジェクトのすべてのプラットフォームで共有されます。 ただし、ネイティブ形式では、ネイティブの Xamarin、iOS、Xamarin、および UWP アプリケーションに `ContentPage` 派生ページを直接追加することができます。 ネイティブプロジェクトで .NET Standard ライブラリプロジェクトまたは共有プロジェクトの `ContentPage` 派生ページを使用するのとは異なり、ネイティブプロジェクトにページを直接追加する利点は、ネイティブビューでページを拡張できることです。 その後、ネイティブビューは、`x:Name` で XAML で名前を付け、分離コードから参照できます。 ネイティブビューの詳細については、「[ネイティブビュー](~/xamarin-forms/platform/native-views/index.md)」を参照してください。

ネイティブプロジェクトで[`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページを使用するプロセスは次のとおりです。

1. ネイティブプロジェクトに Xamarin. Forms NuGet パッケージを追加します。
1. [@No__t_1](xref:Xamarin.Forms.ContentPage)から派生したページ、およびすべての依存関係をネイティブプロジェクトに追加します。
1. `Forms.Init` メソッドを呼び出します。
1. [@No__t_1](xref:Xamarin.Forms.ContentPage)派生ページのインスタンスを構築し、次の拡張メソッドのいずれかを使用して、適切なネイティブ型に変換します。 iOS の場合は `CreateViewController`、Android の場合は `CreateSupportFragment`、UWP の場合は `CreateFrameworkElement`。
1. ネイティブナビゲーション API を使用して、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページのネイティブ型表現に移動します。

ネイティブプロジェクトで[`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページを構築するには、`Forms.Init` メソッドを呼び出して、Xamarin を初期化する必要があります。 この作業を行うタイミングの選択は、主にアプリケーションフローで最も便利なタイミングに依存します。これは、アプリケーションの起動時、または `ContentPage` の派生ページが構築される直前に実行されます。 この記事および付属のサンプルアプリケーションでは、アプリケーションの起動時に `Forms.Init` メソッドが呼び出されます。

> [!NOTE]
> この**サンプルアプリケーションソリューションには、** Xamarin. forms プロジェクトは含まれていません。 代わりに、Xamarin の iOS プロジェクト、Xamarin Android プロジェクト、および UWP プロジェクトで構成されます。 各プロジェクトは、ネイティブフォームを使用して[`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページを使用するネイティブプロジェクトです。 ただし、ネイティブプロジェクトが .NET Standard ライブラリプロジェクトまたは共有プロジェクトから `ContentPage` 派生ページを使用できない理由はありません。

[@No__t_1](xref:Xamarin.Forms.DependencyService)、 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)、データバインディングエンジンなどのネイティブフォームを使用する場合は、すべてが引き続き機能します。 ただし、ページナビゲーションは、ネイティブナビゲーション API を使用して実行する必要があります。

## <a name="ios"></a>iOS

IOS では、`AppDelegate` クラスの `FinishedLaunching` オーバーライドは、通常、アプリケーションの起動に関連するタスクを実行する場所です。 これは、アプリケーションが起動した後に呼び出され、通常はメインウィンドウおよびビューコントローラーを構成するためにオーバーライドされます。 次のコード例は、サンプルアプリケーションの `AppDelegate` クラスを示しています。

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

@No__t_0 メソッドは、次のタスクを実行します。

- @No__t_0 メソッドを呼び出すことによって、Xamarin. フォームが初期化されます。
- @No__t_0 クラスへの参照は、`static` `Instance` フィールドに格納されます。 これは、`AppDelegate` クラスで定義されているメソッドを他のクラスが呼び出すメカニズムを提供するためのものです。
- ネイティブ iOS アプリケーションのビューのメインコンテナーである `UIWindow` が作成されます。
- @No__t_0 プロパティは、ノートデータが格納されるデバイス上のパスに初期化されます。
- @No__t_0 クラスは、XAML で定義されている Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページであり、`CreateViewController` 拡張メソッドを使用して `UIViewController` に変換されます。
- @No__t_1 の [`Title`] プロパティが設定され、`UINavigationBar` に表示されます。
- 階層ナビゲーションを管理するための `AppNavigationController` が作成されます。 これは、`UINavigationController` から派生するカスタムナビゲーションコントローラークラスです。 @No__t_0 オブジェクトはビューコントローラーのスタックを管理し、コンストラクターに渡された `UIViewController` は `AppNavigationController` が読み込まれるときに最初に表示されます。
- @No__t_0 オブジェクトは `UIWindow` の最上位の `UIViewController` として設定され、`UIWindow` はアプリケーションのキーウィンドウとして設定され、表示されるようになります。

@No__t_0 メソッドが実行されると、次のスクリーンショットに示すように、Xamarin. Forms `NotesPage` クラスに定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin iOS アプリケーションのスクリーンショット](native-forms-images/ios-notespage.png "XAML UI を使用した Xamarin iOS アプリ")](native-forms-images/ios-notespage-large.png#lightbox "XAML UI を使用した Xamarin iOS アプリ")

たとえば、 **+** [`Button`](xref:Xamarin.Forms.Button)をタップして UI と対話すると、`NotesPage` コードビハインドで次のイベントハンドラーが実行されます。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

@No__t_0 `AppDelegate.Instance` フィールドを使用すると、`AppDelegate.NavigateToNoteEntryPage` メソッドを呼び出すことができます。これを次のコード例に示します。

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

@No__t_0 メソッドは、`CreateViewController` 拡張メソッドを使用して、Xamarin [`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページを `UIViewController` に変換し、`Title` の `UIViewController` プロパティを設定します。 @No__t_0 は、`PushViewController` メソッドによって `AppNavigationController` にプッシュされます。 そのため、次のスクリーンショットに示すように、Xamarin `NoteEntryPage` クラスで定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin iOS アプリケーションのスクリーンショット](native-forms-images/ios-noteentrypage.png "XAML UI を使用した Xamarin iOS アプリ")](native-forms-images/ios-noteentrypage-large.png#lightbox "XAML UI を使用した Xamarin iOS アプリ")

@No__t_0 が表示されると、戻るナビゲーションによって `AppNavigationController` から `NoteEntryPage` クラスの `UIViewController` がポップされ、`UIViewController` クラスの `NotesPage` にユーザーが返されます。 ただし、iOS ネイティブナビゲーションスタックから `UIViewController` をポップしても、`UIViewController` およびアタッチされた `Page` オブジェクトは自動的に破棄されません。 したがって、`AppNavigationController` クラスは `PopViewController` メソッドをオーバーライドして、後方ナビゲーション時にビューコントローラーを破棄します。

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

@No__t_0 オーバーライドは、iOS ネイティブナビゲーションスタックからポップされた `UIViewController` オブジェクトの `Dispose` メソッドを呼び出します。 この操作を行わないと、`UIViewController` とアタッチされた `Page` オブジェクトが孤立します。

> [!IMPORTANT]
> 孤立したオブジェクトはガベージコレクションできないため、メモリリークが発生します。

## <a name="android"></a>Android

Android では、`MainActivity` クラスの `OnCreate` オーバーライドは、通常、アプリケーションの起動に関連するタスクを実行する場所です。 次のコード例は、サンプルアプリケーションの `MainActivity` クラスを示しています。

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

@No__t_0 メソッドは、次のタスクを実行します。

- @No__t_0 メソッドを呼び出すことによって、Xamarin. フォームが初期化されます。
- @No__t_0 クラスへの参照は、`static` `Instance` フィールドに格納されます。 これは、`MainActivity` クラスで定義されているメソッドを他のクラスが呼び出すメカニズムを提供するためのものです。
- @No__t_0 コンテンツはレイアウトリソースから設定されます。 サンプルアプリケーションでは、レイアウトは `Toolbar` を含む `LinearLayout` と、フラグメントコンテナーとして機能する `FrameLayout` で構成されます。
- @No__t_0 が取得され、`Activity` のアクションバーとして設定され、操作バーのタイトルが設定されます。
- @No__t_0 プロパティは、ノートデータが格納されるデバイス上のパスに初期化されます。
- @No__t_0 クラスは、XAML で定義されている Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページであり、`CreateSupportFragment` 拡張メソッドを使用して `Fragment` に変換されます。
- @No__t_0 クラスは、`FrameLayout` インスタンスを `NotesPage` クラスの `Fragment` に置き換えるトランザクションを作成し、コミットします。

フラグメントの詳細については、「[フラグメント](~/android/platform/fragments/index.md)」を参照してください。

@No__t_0 メソッドが実行されると、次のスクリーンショットに示すように、Xamarin. Forms `NotesPage` クラスに定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin Android アプリケーションのスクリーンショット](native-forms-images/android-notespage.png "XAML UI を使用した Xamarin Android アプリ")](native-forms-images/android-notespage-large.png#lightbox "XAML UI を使用した Xamarin Android アプリ")

たとえば、 **+** [`Button`](xref:Xamarin.Forms.Button)をタップして UI と対話すると、`NotesPage` コードビハインドで次のイベントハンドラーが実行されます。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

@No__t_0 `MainActivity.Instance` フィールドを使用すると、`MainActivity.NavigateToNoteEntryPage` メソッドを呼び出すことができます。これを次のコード例に示します。

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

@No__t_0 メソッドは、`CreateSupportFragment` 拡張メソッドを使用して、Xamarin [`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページを `Fragment` に変換し、その `Fragment` をフラグメントバックスタックに追加します。 そのため、次のスクリーンショットに示すように、Xamarin `NoteEntryPage` で定義されている UI が表示されます。

[![XAML で定義された UI を使用する Xamarin Android アプリケーションのスクリーンショット](native-forms-images/android-noteentrypage.png "XAML UI を使用した Xamarin Android アプリ")](native-forms-images/android-noteentrypage-large.png#lightbox "XAML UI を使用した Xamarin Android アプリ")

@No__t_0 が表示されると、戻る矢印をタップすると、フラグメントバックスタックから `NoteEntryPage` の `Fragment` がポップされ、ユーザーが `NotesPage` クラスの `Fragment` に返されます。

### <a name="enable-back-navigation-support"></a>バックナビゲーションサポートを有効にする

@No__t_0 クラスには、フラグメントバックスタックの内容が変更されるたびに発生する `BackStackChanged` イベントがあります。 @No__t_1 クラスの `OnCreate` メソッドには、このイベントの匿名イベントハンドラーが含まれています。

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

このイベントハンドラーは、フラグメントバックスタックに1つ以上の `Fragment` インスタンスがある場合に、アクションバーに [戻る] ボタンを表示します。 [戻る] ボタンをタップする応答は、`OnOptionsItemSelected` のオーバーライドによって処理されます。

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

@No__t_0 オーバーライドは、[オプション] メニューの項目が選択されるたびに呼び出されます。 この実装は、[戻る] ボタンが選択されていて、フラグメントバックスタックに1つ以上の `Fragment` インスタンスがある場合に、フラグメントバックスタックから現在のフラグメントをポップします。

### <a name="multiple-activities"></a>複数のアクティビティ

アプリケーションが複数のアクティビティで構成されている場合、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページを各アクティビティに埋め込むことができます。 このシナリオでは、`Forms.Init` メソッドは、Xamarin. Forms `ContentPage` を埋め込む最初の `Activity` のオーバーライド `OnCreate` でのみ呼び出す必要があります。 ただし、次のような影響があります。

- @No__t_0 の値は、`Forms.Init` メソッドを呼び出した `Activity` から取得されます。
- @No__t_0 の値は、`Forms.Init` メソッドを呼び出した `Activity` に関連付けられます。

### <a name="choose-a-file"></a>ファイルの選択

HTML の [ファイルの選択] ボタンをサポートする必要がある[`WebView`](xref:Xamarin.Forms.WebView)を使用する[`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページを埋め込む場合、`Activity` は `OnActivityResult` メソッドをオーバーライドする必要があります。

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP の場合、ネイティブ `App` クラスは、通常、アプリケーションの起動に関連するタスクを実行する場所です。 通常、xamarin は、ネイティブ `App` クラスの `OnLaunched` オーバーライドで、Xamarin. Forms UWP アプリケーションで初期化され、`LaunchActivatedEventArgs` 引数を `Forms.Init` メソッドに渡します。 このため、Xamarin [`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページを使用するネイティブ UWP アプリケーションは、`App.OnLaunched` メソッドから `Forms.Init` メソッドを最も簡単に呼び出すことができます。

既定では、ネイティブ `App` クラスは、アプリケーションの最初のページとして `MainPage` クラスを起動します。 次のコード例は、サンプルアプリケーションの `MainPage` クラスを示しています。

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        this.Content = new Notes.UWP.Views.NotesPage().CreateFrameworkElement();
    }
    ...
}
```

@No__t_0 コンストラクターは、次のタスクを実行します。

- ページに対してキャッシュが有効になっているので、ユーザーがページに戻ったときに新しい `MainPage` が構築されることはありません。
- @No__t_0 クラスへの参照は、`static` `Instance` フィールドに格納されます。 これは、`MainPage` クラスで定義されているメソッドを他のクラスが呼び出すメカニズムを提供するためのものです。
- @No__t_0 プロパティは、ノートデータが格納されるデバイス上のパスに初期化されます。
- @No__t_0 クラスは、XAML で定義されている Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページであり、`CreateFrameworkElement` 拡張メソッドを使用して `FrameworkElement` に変換し、`MainPage` クラスの内容として設定します。

@No__t_0 コンストラクターが実行されると、次のスクリーンショットに示すように、Xamarin. Forms `NotesPage` クラスに定義されている UI が表示されます。

[![Xamarin の XAML で定義された UI を使用する UWP アプリケーションのスクリーンショット](native-forms-images/uwp-notespage.png "Xamarin. Forms XAML UI を使用した UWP アプリ")](native-forms-images/uwp-notespage-large.png#lightbox "Xamarin. Forms XAML UI を使用した UWP アプリ")

たとえば、 **+** [`Button`](xref:Xamarin.Forms.Button)をタップして UI と対話すると、`NotesPage` コードビハインドで次のイベントハンドラーが実行されます。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

@No__t_0 `MainPage.Instance` フィールドを使用すると、`MainPage.NavigateToNoteEntryPage` メソッドを呼び出すことができます。これを次のコード例に示します。

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

UWP でのナビゲーションは、通常、`Page` 引数を受け取る `Frame.Navigate` メソッドを使用して実行されます。 Xamarin. Forms は、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページインスタンスを受け取る `Frame.Navigate` 拡張メソッドを定義します。 このため、`NavigateToNoteEntryPage` メソッドを実行すると、次のスクリーンショットに示すように、Xamarin. Forms `NoteEntryPage` で定義されている UI が表示されます。

[![Xamarin の XAML で定義された UI を使用する UWP アプリケーションのスクリーンショット](native-forms-images/uwp-noteentrypage.png "Xamarin. Forms XAML UI を使用した UWP アプリ")](native-forms-images/uwp-noteentrypage-large.png#lightbox "Xamarin. Forms XAML UI を使用した UWP アプリ")

@No__t_0 が表示されると、戻る矢印をタップすると、アプリ内バックスタックから `NoteEntryPage` の `FrameworkElement` がポップされ、ユーザーが `NotesPage` クラスの `FrameworkElement` に返されます。

### <a name="enable-back-navigation-support"></a>バックナビゲーションサポートを有効にする

UWP では、アプリケーションは、さまざまなデバイスフォームファクターにわたって、すべてのハードウェアおよびソフトウェアの [戻る] ボタンのナビゲーションを有効にする必要があります。 これを行うには、`BackRequested` イベントのイベントハンドラーを登録します。これは、ネイティブ `App` クラスの `OnLaunched` メソッドで実行できます。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

アプリケーションが起動されると、`GetForCurrentView` メソッドは、現在のビューに関連付けられている `SystemNavigationManager` オブジェクトを取得し、`BackRequested` イベントのイベントハンドラーを登録します。 アプリケーションは、フォアグラウンドアプリケーションの場合にのみこのイベントを受信し、応答として `OnBackRequested` イベントハンドラーを呼び出します。

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

@No__t_0 イベントハンドラーは、アプリケーションのルートフレームで `GoBack` メソッドを呼び出し、`BackRequestedEventArgs.Handled` プロパティを `true` に設定して、イベントを処理済みとしてマークします。 イベントを処理済みとしてマークしないと、イベントは無視されます。

アプリケーションでは、タイトルバーに [戻る] ボタンを表示するかどうかを選択します。 これを実現するには、`AppViewBackButtonVisibility` プロパティを `AppViewBackButtonVisibility` 列挙値のいずれかに設定します。

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

@No__t_0 イベントハンドラーは、`Navigated` イベントの発生に応答して実行され、ページナビゲーションが発生したときにタイトルバーの [戻る] ボタンの表示を更新します。 これにより、アプリ内バックスタックが空でない場合はタイトルバーの [戻る] ボタンが表示され、アプリ内バックスタックが空の場合はタイトルバーから削除されます。

UWP でのバックナビゲーションサポートの詳細については、「 [uwp アプリのナビゲーション履歴と後方ナビゲーション](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)」を参照してください。

## <a name="related-links"></a>関連リンク

- [種類 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)
