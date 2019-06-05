---
title: Xamarin Native プロジェクトで Xamarin.Forms
description: この記事では、Xamarin のネイティブ プロジェクトに直接追加される ContentPage から派生したページを使用する方法とそれらの間を移動する方法について説明します。
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/03/2019
ms.openlocfilehash: f0ad4e3271ac8c1f8d30a0440b38d8a46c57783e
ms.sourcegitcommit: b4a12607ca944de10fd166139765241a4501831c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687127"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin Native プロジェクトで Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)

通常、Xamarin.Forms アプリケーションから派生した 1 つまたは複数のページが含まれます。 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)、これらのページは、.NET Standard ライブラリ プロジェクトまたは共有プロジェクトのすべてのプラットフォームによって共有されます。 ただし、ネイティブのフォームでは、 `ContentPage`-Xamarin.iOS、Xamarin.Android、および UWP のネイティブ アプリケーションに直接追加するページを派生します。 使用するネイティブ プロジェクトを持つと比較して`ContentPage`-派生ページから、.NET Standard ライブラリ プロジェクトまたは共有プロジェクト、ネイティブ プロジェクトに直接ページの追加の利点は、ネイティブ ビューでページを拡張することができます。 XAML でのネイティブ ビューを付けることができますし、`x:Name`分離コードから参照されているとします。 ネイティブ ビューの詳細については、次を参照してください。[ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)します。

Xamarin.Forms を使用するためのプロセス[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ネイティブ プロジェクト内の派生のページを次に示します。

1. ネイティブ プロジェクトに Xamarin.Forms NuGet パッケージを追加します。
1. 追加、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ページ、およびネイティブ プロジェクトへのすべての依存関係を派生します。
1. `Forms.Init` メソッドを呼び出します。
1. インスタンスを構築、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ページを派生し、次の拡張メソッドのいずれかを使用して、適切なネイティブ型に変換: `CreateViewController` ios、 `CreateSupportFragment` for Android、または`CreateFrameworkElement`のUWP です。
1. ネイティブな型表現に移動し、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ネイティブ ナビゲーション API を使用してページを派生します。

Xamarin.Forms を呼び出すことによって初期化する必要があります、`Forms.Init`メソッドのネイティブ プロジェクトを作成する前に、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ページを派生します。 アプリケーション フローの最も便利なタイミングによって決まります主にこれを実行するタイミングを選択する – アプリケーションの起動時または直前に実行できます、 `ContentPage`-派生ページを構築します。 この記事と付属のサンプル アプリケーションで、`Forms.Init`メソッドは、アプリケーションの起動時に呼び出されます。

> [!NOTE]
> **NativeForms**サンプル アプリケーション ソリューションには、Xamarin.Forms プロジェクトが含まれていません。 代わりに、Xamarin.iOS プロジェクトを Xamarin.Android プロジェクトの場合、および UWP プロジェクトで構成されます。 各プロジェクトは、使用するネイティブのフォームを使用するネイティブ プロジェクト[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ページを派生します。 ただし、ネイティブのプロジェクトで利用できませんでした理由理由はありません`ContentPage`-.NET Standard ライブラリ プロジェクトまたは共有プロジェクトからページを派生します。

ネイティブ数字形式を使用して、Xamarin.Forms などの機能[ `DependencyService` ](xref:Xamarin.Forms.DependencyService)、 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)、およびデータ バインディング エンジンで、すべて引き続き使用します。 ただし、ページ ナビゲーションは、ネイティブ ナビゲーション API を使用して実行する必要があります。

## <a name="ios"></a>iOS

Ios では、`FinishedLaunching`で上書き、`AppDelegate`クラスは、アプリケーションを実行する場所では通常、スタートアップ タスクに関連します。 アプリケーションがついに発売されましたが、通常、メイン ウィンドウを構成し、コント ローラーを表示するオーバーライド後に呼び出されます。 次のコード例は、`AppDelegate`サンプル アプリケーション内のクラス。

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static string FolderPath { get; private set; }

    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

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

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching`メソッドは、次のタスクを実行します。

- Xamarin.Forms は呼び出すことによって初期化、`Forms.Init`メソッド。
- 参照、`AppDelegate`でクラスが格納されている、 `static` `Instance`フィールド。 これは他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`AppDelegate`クラス。
- `UIWindow`、これは、ネイティブの iOS アプリケーションでのビューのメイン コンテナーを作成します。
- `FolderPath`注データが格納されるデバイス上のパスにプロパティを初期化します。
- `NotesPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページの XAML で定義されている、作成され、変換、`UIViewController`を使用して、`CreateViewController`拡張メソッド。
- `Title`のプロパティ、`UIViewController`に表示される設定、`UINavigationBar`します。
- A`UINavigationController`階層型ナビゲーションを管理するために作成されます。 `UINavigationController`クラスはビュー コント ローラーのスタックを管理し、`UIViewController`に渡されるコンス トラクターがときに表示する最初に、`UINavigationController`が読み込まれます。
- `UINavigationController`インスタンスは、最上位レベルとして設定`UIViewController`の`UIWindow`と`UIWindow`キー、アプリケーション ウィンドウとして設定されが表示されます。

1 回、`FinishedLaunching`メソッドの実行が、UI は、Xamarin.Forms で定義されている`NotesPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![UI を使用する Xamarin.iOS アプリケーションのスクリーン ショットは、XAML で定義されている](native-forms-images/ios-notespage.png "XAML UI を使用した Xamarin.iOS アプリ")](native-forms-images/ios-notespage-large.png#lightbox "XAML UI を使用した xamarin ios アプリ")

タップしてなど、UI との対話、 **+** [ `Button` ](xref:Xamarin.Forms.Button)、次のイベント ハンドラーになります、`NotesPage`分離コードを実行します。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `AppDelegate.Instance`フィールドを使用できます、`AppDelegate.NavigateToNoteEntryPage`の次のコード例に示すメソッドが呼び出されます。

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    UIViewController noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

`NavigateToNoteEntryPage`メソッドは、Xamarin.Forms を変換[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生にページ、`UIViewController`で、`CreateViewController`拡張メソッド、およびセット、`Title`のプロパティ、`UIViewController`です。 `UIViewController`しプッシュ`UINavigationController`によって、`PushViewController`メソッド。 そのため、UI を Xamarin.Forms で定義されている`NoteEntryPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![UI を使用する Xamarin.iOS アプリケーションのスクリーン ショットは、XAML で定義されている](native-forms-images/ios-noteentrypage.png "XAML UI を使用した Xamarin.iOS アプリ")](native-forms-images/ios-noteentrypage-large.png#lightbox "XAML UI を使用した xamarin ios アプリ")

ときに、`NoteEntryPage`背面をタップして、表示される矢印が表示されます、`UIViewController`の`NoteEntryPage`クラスから、 `UINavigationController`、ユーザーを返す、`UIViewController`の`NotesPage`クラス。

## <a name="android"></a>Android

Android では、`OnCreate`で上書き、`MainActivity`クラスは、アプリケーションを実行する場所では通常、スタートアップ タスクに関連します。 次のコード例は、`MainActivity`サンプル アプリケーション内のクラス。

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

`OnCreate`メソッドは、次のタスクを実行します。

- Xamarin.Forms は呼び出すことによって初期化、`Forms.Init`メソッド。
- 参照、`MainActivity`でクラスが格納されている、 `static` `Instance`フィールド。 これは他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`MainActivity`クラス。
- `Activity`レイアウト リソースからコンテンツを設定します。 レイアウトは、サンプル アプリケーションで、`LinearLayout`を格納している、 `Toolbar`、および`FrameLayout`フラグメントのコンテナーとして機能します。
- `Toolbar`が取得され、設定の操作バーとして、 `Activity`、操作バーのタイトルを設定します。
- `FolderPath`注データが格納されるデバイス上のパスにプロパティを初期化します。
- `NotesPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページの XAML で定義されている、作成され、変換、`Fragment`を使用して、`CreateSupportFragment`拡張メソッド。
- `SupportFragmentManager`クラスを作成し、置換するトランザクションのコミット、`FrameLayout`インスタンス、`Fragment`の`NotesPage`クラス。

フラグメントの詳細については、次を参照してください。[フラグメント](~/android/platform/fragments/index.md)します。

1 回、`OnCreate`メソッドの実行が、UI は、Xamarin.Forms で定義されている`NotesPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![UI を使用する Xamarin.Android アプリケーションのスクリーン ショットは、XAML で定義されている](native-forms-images/android-notespage.png "XAML の UI で Xamarin.Android アプリ")](native-forms-images/android-notespage-large.png#lightbox "XAML の UI で Xamarin.Android アプリ")

タップしてなど、UI との対話、 **+** [ `Button` ](xref:Xamarin.Forms.Button)、次のイベント ハンドラーになります、`NotesPage`分離コードを実行します。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainActivity.Instance`フィールドを使用できます、`MainActivity.NavigateToNoteEntryyPage`の次のコード例に示すメソッドが呼び出されます。

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

`NavigateToNoteEntryPage`メソッドは、Xamarin.Forms を変換します。 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生にページ、`Fragment`で、`CreateSupportFragment`拡張メソッドを追加し、`Fragment`フラグメント バック スタック。 そのため、UI を Xamarin.Forms で定義されている`NoteEntryPage`次のスクリーン ショットに示すように表示されます。

[![UI を使用する Xamarin.Android アプリケーションのスクリーン ショットは、XAML で定義されている](native-forms-images/android-noteentrypage.png "XAML の UI で Xamarin.Android アプリ")](native-forms-images/android-noteentrypage-large.png#lightbox "XAML の UI で Xamarin.Android アプリ")

ときに、`NoteEntryPage`背面をタップして、表示される矢印が表示されます、`Fragment`の`NoteEntryPage`フラグメントのバック スタックからユーザーを返す、`Fragment`の`NotesPage`クラス。

### <a name="enable-back-navigation-support"></a>ナビゲーションのサポートを有効にします。

`SupportFragmentManager`クラスには、`BackStackChanged`フラグメント戻るスタックのコンテンツが変更されるたびに発生するイベントです。 `OnCreate`メソッドで、`MainActivity`クラスには、このイベントの匿名のイベント ハンドラーが含まれています。

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

このイベント ハンドラーは、1 つまたは複数があること、操作バーで [戻る] ボタンを表示`Fragment`フラグメント上のインスタンスがスタックをバックアップします。 [戻る] ボタンをタップする応答の処理、`OnOptionsItemSelected`をオーバーライドします。

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

`OnOptionsItemSelected`オプション メニュー内の項目が選択されるたびに、オーバーライドが呼び出されます。 この実装は、[戻る] ボタンが選択されており、1 つまたは複数があることに、フラグメントのバック スタックから現在のフラグメントをポップ`Fragment`フラグメント上のインスタンスがスタックをバックアップします。

### <a name="multiple-activities"></a>複数のアクティビティ

アプリケーションが複数のアクティビティで構成される場合[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページは、各アクティビティに埋め込まれることができます。 このシナリオで、`Forms.Init`メソッドでのみ呼び出す必要があります、`OnCreate`最初のオーバーライド`Activity`、Xamarin.Forms を埋め込みますを`ContentPage`します。 ただし、これに、次の影響です。

- 値`Xamarin.Forms.Color.Accent`から取得されます、`Activity`という、`Forms.Init`メソッド。
- 値`Xamarin.Forms.Application.Current`に関連付けられた、`Activity`という、`Forms.Init`メソッド。

### <a name="choose-a-file"></a>ファイルを選択します。

埋め込み時、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生を使用するページ、 [ `WebView` ](xref:Xamarin.Forms.WebView)する必要がある HTML"Choose File"をサポートするボタン、`Activity`をオーバーライドする必要があります、 `OnActivityResult`方法:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP、ネイティブの`App`クラスは、アプリケーションを実行する場所では通常、スタートアップ タスクに関連します。 Xamarin.Forms は、通常は、Xamarin.Forms UWP アプリケーションで初期化、`OnLaunched`ネイティブ オーバーライド`App`を渡すためのクラス、`LaunchActivatedEventArgs`への引数、`Forms.Init`メソッド。 この理由は、Xamarin.Forms を使用するネイティブの UWP アプリケーション[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページが最も簡単に呼び出すことができます、`Forms.Init`からメソッド、`App.OnLaunched`メソッド。

既定では、ネイティブ`App`クラスの起動、`MainPage`アプリケーションの最初のページとしてクラス。 次のコード例は、`MainPage`サンプル アプリケーション内のクラス。

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

`MainPage`コンス トラクターは、次のタスクを実行します。

- ページのキャッシュが有効になっているように、新しい`MainPage`のページに戻るユーザーが移動したときに構成されていません。
- 参照、`MainPage`でクラスが格納されている、 `static` `Instance`フィールド。 これは他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`MainPage`クラス。
- `FolderPath`注データが格納されるデバイス上のパスにプロパティを初期化します。
- `NotesPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページの XAML で定義されているが構築されに変換、`FrameworkElement`を使用して、`CreateFrameworkElement`拡張メソッドとのコンテンツとして設定します`MainPage`クラスです。

1 回、`MainPage`コンス トラクターが実行される、Xamarin.Forms で定義されている UI`NotesPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![UI を使用する UWP アプリケーションのスクリーン ショットは、Xamarin.Forms の XAML で定義されている](native-forms-images/uwp-notespage.png "Xamarin.Forms XAML の UI を使用した UWP アプリ")](native-forms-images/uwp-notespage-large.png#lightbox "Xamarin.Forms XAML の UI を使用した UWP アプリ")

タップしてなど、UI との対話、 **+** [ `Button` ](xref:Xamarin.Forms.Button)、次のイベント ハンドラーになります、`NotesPage`分離コードを実行します。

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainPage.Instance`フィールドを使用できます、`MainPage.NavigateToNoteEntryPage`の次のコード例に示すメソッドが呼び出されます。

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

UWP でのナビゲーションは、通常の実行、`Frame.Navigate`を受け取るメソッドを`Page`引数。 Xamarin.Forms の定義、`Frame.Navigate`拡張メソッドを受け取る、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページ インスタンス。 したがって、ときに、`NavigateToNoteEntryPage`メソッドが実行される、Xamarin.Forms で定義されている UI`NoteEntryPage`次のスクリーン ショットに示すように表示されます。

[![UI を使用する UWP アプリケーションのスクリーン ショットは、Xamarin.Forms の XAML で定義されている](native-forms-images/uwp-noteentrypage.png "Xamarin.Forms XAML の UI を使用した UWP アプリ")](native-forms-images/uwp-noteentrypage-large.png#lightbox "Xamarin.Forms XAML の UI を使用した UWP アプリ")

ときに、`NoteEntryPage`背面をタップして、表示される矢印が表示されます、`FrameworkElement`の`NoteEntryPage`アプリ内のバック スタックからユーザーを返す、`FrameworkElement`の`NotesPage`クラス。

### <a name="enable-back-navigation-support"></a>ナビゲーションのサポートを有効にします。

UWP では、アプリケーションが別のデバイス フォーム ファクターのすべてのハードウェアとソフトウェア戻るボタン、ナビゲーションを有効にする必要があります。 これは、イベント ハンドラーを登録することによって実現できます、`BackRequested`イベントで実行できますが、`OnLaunched`ネイティブ メソッド`App`クラス。

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

アプリケーションを起動すると、`GetForCurrentView`メソッドの取得、`SystemNavigationManager`オブジェクトは、現在のビューに関連付けられているし、イベント ハンドラーを登録します、`BackRequested`イベント。 アプリケーションは、前面のアプリケーションは、応答を呼び出す場合のみこのイベントを受け取る、`OnBackRequested`イベント ハンドラー。

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

`OnBackRequested`イベント ハンドラーの呼び出し、`GoBack`セット、アプリケーションのルート フレームのメソッド、`BackRequestedEventArgs.Handled`プロパティを`true`イベントを処理済みとしてマークします。 イベントを処理済みとしてマークする障害は、無視されているイベントのなる可能性があります。

アプリケーションでは、タイトル バーで、[戻る] ボタンを表示するかどうかを選択します。 これを設定することで実現されます、`AppViewBackButtonVisibility`プロパティのいずれかを`AppViewBackButtonVisibility`列挙値。

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated`への応答に実行されるイベント ハンドラー、`Navigated`イベントの発生は、ページ ナビゲーションが発生した場合に、タイトル バーの [戻る] ボタンの可視性を更新します。 これにより、タイトル バーの [戻る] ボタンが表示されるは、アプリに戻るスタックが空でない場合は、または、アプリに戻るスタックが空の場合は、タイトル バーから削除します。

UWP の戻るナビゲーション サポートの詳細については、次を参照してください。[ナビゲーション履歴内を後方に向かってと UWP アプリのナビゲーション](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)します。

## <a name="related-links"></a>関連リンク

- [NativeForms (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)
