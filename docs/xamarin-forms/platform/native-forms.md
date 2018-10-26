---
title: Xamarin Native プロジェクトで Xamarin.Forms
description: この記事では、Xamarin のネイティブ プロジェクトに直接追加される ContentPage から派生したページを使用する方法とそれらの間を移動する方法について説明します。
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 04d435b29f6f2f577df5025995fcc074ba5d9d9d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122752"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin Native プロジェクトで Xamarin.Forms

_ネイティブ フォームは、ネイティブの Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) プロジェクトで使用する Xamarin.Forms ContentPage から派生したページを使用します。ネイティブ プロジェクトは、プロジェクト、または .NET Standard ライブラリ、.NET Standard ライブラリ、または共有プロジェクトから直接追加される ContentPage から派生したページを使用できます。この記事では、ネイティブのプロジェクトに直接追加される ContentPage から派生したページを使用する方法とそれらの間を移動する方法について説明します。_

通常、Xamarin.Forms アプリケーションから派生した 1 つまたは複数のページが含まれます。 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)、これらのページは、.NET Standard ライブラリ プロジェクトまたは共有プロジェクトのすべてのプラットフォームによって共有されます。 ただし、ネイティブのフォームでは、 `ContentPage`-Xamarin.iOS、Xamarin.Android、および UWP のネイティブ アプリケーションに直接追加するページを派生します。 使用するネイティブ プロジェクトを持つと比較して`ContentPage`-派生ページから、.NET Standard ライブラリ プロジェクトまたは共有プロジェクト、ネイティブ プロジェクトに直接ページの追加の利点は、ネイティブ ビューでページを拡張することができます。 XAML でのネイティブ ビューを付けることができますし、`x:Name`分離コードから参照されているとします。 ネイティブ ビューの詳細については、次を参照してください。[ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)します。

Xamarin.Forms を使用するためのプロセス[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ネイティブ プロジェクト内の派生のページを次に示します。

1. ネイティブ プロジェクトに Xamarin.Forms NuGet パッケージを追加します。
1. 追加、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ページ、およびネイティブ プロジェクトへのすべての依存関係を派生します。
1. `Forms.Init` メソッドを呼び出します。
1. インスタンスを構築、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ページを派生し、次の拡張メソッドのいずれかを使用して、適切なネイティブ型に変換: `CreateViewController` ios、`CreateFragment`または`CreateSupportFragment`for Android、または`CreateFrameworkElement` UWP 用です。
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

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

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
- `PhonewordPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページの XAML で定義されている、作成され、変換、`UIViewController`を使用して、`CreateViewController`拡張メソッド。
- `Title`のプロパティ、`UIViewController`に表示される設定、`UINavigationBar`します。
- A`UINavigationController`階層型ナビゲーションを管理するために作成されます。 `UINavigationController`クラスはビュー コント ローラーのスタックを管理し、`UIViewController`に渡されるコンス トラクターがときに表示する最初に、`UINavigationController`が読み込まれます。
- `UINavigationController`インスタンスは、最上位レベルとして設定`UIViewController`の`UIWindow`と`UIWindow`キー、アプリケーション ウィンドウとして設定されが表示されます。

1 回、`FinishedLaunching`メソッドの実行が、UI は、Xamarin.Forms で定義されている`PhonewordPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

タップしてなど、UI との対話、 [ `Button` ](xref:Xamarin.Forms.Button)、内のイベント ハンドラーになります、`PhonewordPage`分離コードを実行します。 たとえば、ときにユーザーがタップ、**通話履歴**ボタンでは、次のイベント ハンドラーの実行します。

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance`フィールドを使用できます、`AppDelegate.NavigateToCallHistoryPage`の次のコード例に示すメソッドが呼び出されます。

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage`メソッドは、Xamarin.Forms を変換[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生にページ、`UIViewController`で、`CreateViewController`拡張メソッド、およびセット、`Title`のプロパティ、`UIViewController`です。 `UIViewController`しプッシュ`UINavigationController`によって、`PushViewController`メソッド。 そのため、UI を Xamarin.Forms で定義されている`CallHistoryPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

ときに、`CallHistoryPage`背面をタップして、表示される矢印が表示されます、`UIViewController`の`CallHistoryPage`クラスから、 `UINavigationController`、ユーザーを返す、`UIViewController`の`PhonewordPage`クラス。

## <a name="android"></a>Android

Android では、`OnCreate`で上書き、`MainActivity`クラスは、アプリケーションを実行する場所では通常、スタートアップ タスクに関連します。 次のコード例は、`MainActivity`サンプル アプリケーション内のクラス。

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
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
- `PhonewordPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページの XAML で定義されている、作成され、変換、`Fragment`を使用して、`CreateFragment`拡張メソッド。
- `FragmentManager`クラスを作成し、置換するトランザクションのコミット、`FrameLayout`インスタンス、`Fragment`の`PhonewordPage`クラス。

フラグメントの詳細については、次を参照してください。[フラグメント](~/android/platform/fragments/index.md)します。

> [!NOTE]
> 加え、`CreateFragment`拡張メソッドでは、Xamarin.Forms も含まれています、`CreateSupportFragment`メソッド。 `CreateFragment`メソッドを作成、 `Android.App.Fragment` API 11 を対象とするアプリケーションで使用およびそれ以降にあることができます。 `CreateSupportFragment`メソッドを作成、 `Android.Support.V4.App.Fragment` 11 より前の API バージョンを対象とするアプリケーションで使用することができます。

1 回、`OnCreate`メソッドの実行が、UI は、Xamarin.Forms で定義されている`PhonewordPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

タップしてなど、UI との対話、 [ `Button` ](xref:Xamarin.Forms.Button)、内のイベント ハンドラーになります、`PhonewordPage`分離コードを実行します。 たとえば、ときにユーザーがタップ、**通話履歴**ボタンでは、次のイベント ハンドラーの実行します。

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance`フィールドを使用できます、`MainActivity.NavigateToCallHistoryPage`の次のコード例に示すメソッドが呼び出されます。

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

`NavigateToCallHistoryPage`メソッドは、Xamarin.Forms を変換します。 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生にページ、`Fragment`で、`CreateFragment`拡張メソッドを追加し、`Fragment`フラグメント バック スタック。 そのため、UI を Xamarin.Forms で定義されている`CallHistoryPage`次のスクリーン ショットに示すように表示されます。

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

ときに、`CallHistoryPage`背面をタップして、表示される矢印が表示されます、`Fragment`の`CallHistoryPage`フラグメントのバック スタックからユーザーを返す、`Fragment`の`PhonewordPage`クラス。

### <a name="enabling-back-navigation-support"></a>ナビゲーションのサポートを有効にします。

`FragmentManager`クラスには、`BackStackChanged`フラグメント戻るスタックのコンテンツが変更されるたびに発生するイベントです。 `OnCreate`メソッドで、`MainActivity`クラスには、このイベントの匿名のイベント ハンドラーが含まれています。

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

このイベント ハンドラーは、1 つまたは複数があること、操作バーで [戻る] ボタンを表示`Fragment`フラグメント上のインスタンスがスタックをバックアップします。 [戻る] ボタンをタップする応答の処理、`OnOptionsItemSelected`をオーバーライドします。

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
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

### <a name="choosing-a-file"></a>ファイルを選択します。

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

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage`コンス トラクターは、次のタスクを実行します。

- ページのキャッシュが有効になっているように、新しい`MainPage`のページに戻るユーザーが移動したときに構成されていません。
- 参照、`MainPage`でクラスが格納されている、 `static` `Instance`フィールド。 これは他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`MainPage`クラス。
- `PhonewordPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページの XAML で定義されているが構築されに変換、`FrameworkElement`を使用して、`CreateFrameworkElement`拡張メソッドとのコンテンツとして設定します`MainPage`クラスです。

1 回、`MainPage`コンス トラクターが実行される、Xamarin.Forms で定義されている UI`PhonewordPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

タップしてなど、UI との対話、 [ `Button` ](xref:Xamarin.Forms.Button)、内のイベント ハンドラーになります、`PhonewordPage`分離コードを実行します。 たとえば、ときにユーザーがタップ、**通話履歴**ボタンでは、次のイベント ハンドラーの実行します。

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance`フィールドを使用できます、`MainPage.NavigateToCallHistoryPage`の次のコード例に示すメソッドが呼び出されます。

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

UWP でのナビゲーションは、通常の実行、`Frame.Navigate`を受け取るメソッドを`Page`引数。 Xamarin.Forms の定義、`Frame.Navigate`拡張メソッドを受け取る、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-派生ページ インスタンス。 したがって、ときに、`NavigateToCallHistoryPage`メソッドが実行される、Xamarin.Forms で定義されている UI`CallHistoryPage`次のスクリーン ショットに示すように表示されます。

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

ときに、`CallHistoryPage`背面をタップして、表示される矢印が表示されます、`FrameworkElement`の`CallHistoryPage`アプリ内のバック スタックからユーザーを返す、`FrameworkElement`の`PhonewordPage`クラス。

### <a name="enabling-back-navigation-support"></a>ナビゲーションのサポートを有効にします。

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

`OnBackRequested`イベント ハンドラーの呼び出し、`GoBack`セット、アプリケーションのルート フレームのメソッド、`BackRequestedEventArgs.Handled`プロパティを`true`イベントを処理済みとしてマークします。 (モバイル デバイス ファミリ) 上のアプリケーションから移動または (デスクトップ デバイス ファミリ) では、イベントを無視して、システム障害イベントを処理済みとしてマークする可能性があります。

アプリケーションは、スマート フォンで提供されるシステムの戻るボタンに依存していますが、デスクトップ デバイス上のタイトル バーに [戻る] ボタンを表示するかどうかを選択します。 これを設定することで実現されます、`AppViewBackButtonVisibility`プロパティのいずれかを`AppViewBackButtonVisibility`列挙値。

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated`への応答に実行されるイベント ハンドラー、`Navigated`イベントの発生は、ページ ナビゲーションが発生した場合に、タイトル バーの [戻る] ボタンの可視性を更新します。 これにより、タイトル バーの [戻る] ボタンが表示されるは、アプリに戻るスタックが空でない場合は、または、アプリに戻るスタックが空の場合は、タイトル バーから削除します。

UWP の戻るナビゲーション サポートの詳細については、次を参照してください。[ナビゲーション履歴内を後方に向かってと UWP アプリのナビゲーション](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)します。

## <a name="summary"></a>まとめ

ネイティブ フォームは、Xamarin.Forms を使用する[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ネイティブ Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) プロジェクトで使用するページを派生します。 ネイティブ プロジェクトで利用可能`ContentPage`-は、プロジェクト、または .NET Standard ライブラリ プロジェクトまたは共有プロジェクトから直接追加するページを派生します。 この記事では、使用する方法を説明しました。 `ContentPage`-ネイティブ プロジェクトは、およびそれらの間を移動する方法に直接追加されるページを派生します。


## <a name="related-links"></a>関連リンク

- [NativeForms (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)
