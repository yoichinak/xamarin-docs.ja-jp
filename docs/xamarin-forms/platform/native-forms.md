---
title: Xamarin Native プロジェクト内の Xamarin.Forms
description: この記事では、それらの間を移動する方法とは、Xamarin のネイティブ プロジェクトに直接追加するコンテンツ ページから派生したページを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: ca62b9fec3223e8da62d8e4cc6e1f69a58f335a0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243276"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin Native プロジェクト内の Xamarin.Forms

_ネイティブのフォームは、Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) のネイティブ プロジェクトが消費できるように Xamarin.Forms コンテンツ ページから派生したページを使用できます。ネイティブ プロジェクトは、プロジェクトまたは .NET 標準ライブラリ、標準的な .NET のライブラリまたは共有プロジェクトから直接追加するコンテンツ ページから派生したページを使用できます。この記事では、それらの間を移動する方法とは、ネイティブ プロジェクトに直接追加するコンテンツ ページから派生したページを使用する方法について説明します。_

通常、Xamarin.Forms アプリケーションから派生した 1 つまたは複数のページが含まれます。 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)、これらのページは、標準的な .NET のライブラリ プロジェクトまたは共有プロジェクトのすべてのプラットフォームで共有されるとします。 ただし、ネイティブのフォームでは、 `ContentPage`-ネイティブ Xamarin.iOS、Xamarin.Android、および UWP アプリケーションに直接追加するページを派生します。 使用するネイティブ プロジェクトを持つと比較して`ContentPage`-派生のページから、標準的な .NET のライブラリ プロジェクトまたは共有のプロジェクト ページに直接追加するネイティブ プロジェクトの利点は、ネイティブのビューでページを拡張することができます。 使用して XAML でネイティブのビューを付けることができますし、`x:Name`分離コードから参照されているとします。 ネイティブのビューの詳細については、次を参照してください。[ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)です。

Xamarin.Forms を使用するためのプロセス[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-ネイティブ プロジェクト内の派生のページを次に示します。

1. Xamarin.Forms NuGet パッケージをネイティブ プロジェクトに追加します。
1. 追加、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)のページ、およびネイティブ プロジェクトへの依存関係を派生します。
1. `Forms.Init` メソッドを呼び出します。
1. インスタンスを構築、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)のページを派生し、次の拡張メソッドのいずれかを使用して、適切なネイティブ型に変換: `CreateViewController` 、iOS の`CreateFragment`または`CreateSupportFragment`for Android、または`CreateFrameworkElement` UWP にします。
1. ネイティブ型表現に移動し、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-ネイティブ ナビゲーション API を使用してページを派生します。

Xamarin.Forms を呼び出すことによって初期化する必要があります、`Forms.Init`メソッドのネイティブ プロジェクトを作成、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)のページを派生します。 アプリケーション フローで最も便利な時間に依存を選択するときにこれを行う主に – アプリケーションの起動時にかされる前に実行でした、 `ContentPage`-派生ページを構築します。 この記事と付属のサンプル アプリケーションで、`Forms.Init`メソッドはアプリケーションの起動時に呼び出されます。

> [!NOTE]
> **NativeForms**サンプル アプリケーションのソリューションには、Xamarin.Forms プロジェクトが含まれていません。 代わりに、Xamarin.iOS プロジェクト、Xamarin.Android プロジェクトと UWP プロジェクトで構成されます。 各プロジェクトは、使用するネイティブのフォームを使用するネイティブ プロジェクト[ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)のページを派生します。 ただし、ネイティブ プロジェクトを使用できませんでした。 理由理由はありません`ContentPage`-.NET 標準のライブラリ プロジェクトまたはプロジェクトの共有のページに由来します。

Xamarin.Forms 機能などのネイティブ形式を使用する場合[ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)、 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)、およびデータ バインディング エンジンで、まだすべての作業です。

## <a name="ios"></a>iOS

Ios の場合、`FinishedLaunching`内の上書き、`AppDelegate`クラスはアプリケーションを実行する場所、通常の起動に関連するタスク。 アプリケーションが起動され、通常、メイン ウィンドウを構成およびコント ローラーを表示するオーバーライドが後に呼び出されます。 次のコード例は、`AppDelegate`サンプル アプリケーション内のクラス。

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

- Xamarin.Forms は呼び出すことによって初期化、`Forms.Init`メソッドです。
- 参照、`AppDelegate`でクラスが格納されている、 `static` `Instance`フィールドです。 これは、他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`AppDelegate`クラスです。
- `UIWindow`、これはネイティブの iOS アプリケーションで、ビューのメインのコンテナーを作成します。
- `PhonewordPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページの XAML で定義されている、作成され、変換、`UIViewController`を使用して、`CreateViewController`拡張メソッド。
- `Title`のプロパティ、`UIViewController`に表示されますが、設定、`UINavigationBar`です。
- A`UINavigationController`が階層的なナビゲーションを管理するために作成します。 `UINavigationController`クラスは、管理コント ローラーの表示のスタックと`UIViewController`に渡されるコンス トラクターが表示されます最初にすると、`UINavigationController`が読み込まれる。
- `UINavigationController`インスタンスは、最上位レベルとして設定`UIViewController`の`UIWindow`、および`UIWindow`キー、アプリケーションのウィンドウとして設定されているし、表示されます。 します。

1 回、`FinishedLaunching`メソッドの実行に、UI は、Xamarin.Forms で定義されている`PhonewordPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

たとえばはタップして、UI と対話する、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)、内のイベント ハンドラーになります、`PhonewordPage`分離コードを実行します。 たとえば、ときに、ユーザーがタップ、**通話履歴**ボタン、次のイベント ハンドラーの実行します。

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance`フィールドでは、`AppDelegate.NavigateToCallHistoryPage`のメソッドを呼び出して、次のコード例に示されています。

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage`メソッドは、Xamarin.Forms を変換[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生にページ、`UIViewController`で、`CreateViewController`拡張メソッド、およびセット、`Title`のプロパティ、`UIViewController`です。 `UIViewController`にプッシュし、`UINavigationController`によって、`PushViewController`メソッドです。 そのため、UI を Xamarin.Forms で定義されている`CallHistoryPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

ときに、`CallHistoryPage`背面をタップすると、表示される矢印が表示されます、`UIViewController`の`CallHistoryPage`からクラス、 `UINavigationController`、ユーザーを返す、`UIViewController`の`PhonewordPage`クラスです。

## <a name="android"></a>Android

Android で、`OnCreate`内の上書き、`MainActivity`クラスはアプリケーションを実行する場所、通常の起動に関連するタスク。 次のコード例は、`MainActivity`サンプル アプリケーション内のクラス。

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

- Xamarin.Forms は呼び出すことによって初期化、`Forms.Init`メソッドです。
- 参照、`MainActivity`でクラスが格納されている、 `static` `Instance`フィールドです。 これは、他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`MainActivity`クラスです。
- `Activity`レイアウト リソースからコンテンツを設定します。 レイアウトは、サンプル アプリケーションで、`LinearLayout`を格納している、 `Toolbar`、および`FrameLayout`フラグメント コンテナーとして機能します。
- `Toolbar`が取得され、アクションの水準として設定、 `Activity`、および操作のバーのタイトルを設定します。
- `PhonewordPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページの XAML で定義されている、作成され、変換、`Fragment`を使用して、`CreateFragment`拡張メソッド。
- `FragmentManager`クラスを作成し、トランザクションをコミットしますを置き換える、`FrameLayout`インスタンス、`Fragment`の`PhonewordPage`クラスです。

フラグメントの詳細については、次を参照してください。[フラグメント](~/android/platform/fragments/index.md)です。

> [!NOTE]
> 加え、`CreateFragment`拡張メソッドでは、Xamarin.Forms も含まれています、`CreateSupportFragment`メソッドです。 `CreateFragment`メソッドを作成、 `Android.App.Fragment` API 11 を対象とするアプリケーションで使用されていると大きい値にすることができます。 `CreateSupportFragment`メソッドを作成、 `Android.Support.V4.App.Fragment` 11 より前の API のバージョンを対象とするアプリケーションで使用できます。

1 回、`OnCreate`メソッドの実行に、UI は、Xamarin.Forms で定義されている`PhonewordPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

たとえばはタップして、UI と対話する、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)、内のイベント ハンドラーになります、`PhonewordPage`分離コードを実行します。 たとえば、ときに、ユーザーがタップ、**通話履歴**ボタン、次のイベント ハンドラーの実行します。

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance`フィールドでは、`MainActivity.NavigateToCallHistoryPage`のメソッドを呼び出して、次のコード例に示されています。

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

`NavigateToCallHistoryPage`メソッドは、Xamarin.Forms を変換します。 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生にページ、`Fragment`で、`CreateFragment`拡張メソッドを追加し、`Fragment`フラグメント バック スタック。 そのため、UI を Xamarin.Forms で定義されている`CallHistoryPage`次のスクリーン ショットに示すように表示されます。

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

ときに、`CallHistoryPage`背面をタップすると、表示される矢印が表示されます、`Fragment`の`CallHistoryPage`、フラグメントのバック スタックからユーザーを返す、`Fragment`の`PhonewordPage`クラスです。

### <a name="enabling-back-navigation-support"></a>[戻る] ナビゲーションのサポートを有効にします。

`FragmentManager`クラスには、`BackStackChanged`フラグメント バック スタックの内容が変更されるたびに発生するイベントです。 `OnCreate`メソッドで、`MainActivity`クラスには、このイベントの匿名のイベント ハンドラーが含まれています。

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

このイベント ハンドラーは、その 1 つまたは複数操作バーの [戻る] ボタンを表示`Fragment`フラグメントは、上のインスタンスがスタックをバックアップします。 [戻る] ボタンをタップする応答の処理、`OnOptionsItemSelected`をオーバーライドします。

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

`OnOptionsItemSelected`オプション メニュー内の項目が選択されていると、オーバーライドが呼び出されます。 [戻る] ボタンが選択されていて、1 つまたは複数をこの実装は、フラグメントのバック スタックから現在のフラグメントをポップ`Fragment`フラグメントは、上のインスタンスがスタックをバックアップします。

### <a name="multiple-activities"></a>複数のアクティビティ

アプリケーションが複数のアクティビティで構成されるときに[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページは、各アクティビティに埋め込むことができます。 このシナリオでは、`Forms.Init`メソッドでのみ呼び出す必要があります、`OnCreate`最初のオーバーライド`Activity`、Xamarin.Forms を埋め込みますを`ContentPage`です。 ただし、これには、以下の影響があります。

- 値`Xamarin.Forms.Color.Accent`から取得されます、`Activity`呼び出される、`Forms.Init`メソッドです。
- 値`Xamarin.Forms.Application.Current`が関連付けられる、`Activity`呼び出される、`Forms.Init`メソッドです。

### <a name="choosing-a-file"></a>ファイルを選択します。

埋め込み時、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生を使用するページ、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)する必要がある HTML"Choose File"をサポートするボタン、`Activity`をオーバーライドする必要があります、 `OnActivityResult`方法:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP、ネイティブで`App`クラスはアプリケーションを実行する場所、通常の起動に関連するタスク。 Xamarin.Forms は通常、Xamarin.Forms UWP アプリケーション内で初期化、`OnLaunched`ネイティブでオーバーライド`App`に渡す、クラス、`LaunchActivatedEventArgs`への引数、`Forms.Init`メソッドです。 このため、Xamarin.Forms を使用するネイティブの UWP アプリケーション[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページから呼び出すことが最も簡単に、`Forms.Init`メソッドから、`App.OnLaunched`メソッドです。

既定では、ネイティブ`App`クラスが起動し、`MainPage`アプリケーションの最初のページとしてクラスです。 次のコード例は、`MainPage`サンプル アプリケーション内のクラス。

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

- ページのキャッシュが有効になっているように、新しい`MainPage`いない構築時に、ユーザーがページに移動します。
- 参照、`MainPage`でクラスが格納されている、 `static` `Instance`フィールドです。 これは、他のクラスで定義されているメソッドを呼び出すためのメカニズムを提供する、`MainPage`クラスです。
- `PhonewordPage`クラスは、これは、Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページの XAML で定義されているが構築されに変換、`FrameworkElement`を使用して、`CreateFrameworkElement`拡張メソッドとのコンテンツとして設定します`MainPage`クラスです。

1 回、`MainPage`コンス トラクターが実行される、UI は、Xamarin.Forms で定義されている`PhonewordPage`次のスクリーン ショットに示すようにクラスが表示されます。

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

たとえばはタップして、UI と対話する、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)、内のイベント ハンドラーになります、`PhonewordPage`分離コードを実行します。 たとえば、ときに、ユーザーがタップ、**通話履歴**ボタン、次のイベント ハンドラーの実行します。

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance`フィールドでは、`MainPage.NavigateToCallHistoryPage`のメソッドを呼び出して、次のコード例に示されています。

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

UWP でナビゲーションは、通常の実行、`Frame.Navigate`を受け取るメソッド、`Page`引数。 Xamarin.Forms を定義、`Frame.Navigate`拡張メソッドを受け取る、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページ インスタンス。 したがって、ときに、`NavigateToCallHistoryPage`メソッドが実行される、Xamarin.Forms で定義されている UI`CallHistoryPage`次のスクリーン ショットに示すように表示されます。

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

ときに、`CallHistoryPage`背面をタップすると、表示される矢印が表示されます、`FrameworkElement`の`CallHistoryPage`、アプリ内のバック スタックからユーザーを返す、`FrameworkElement`の`PhonewordPage`クラスです。

### <a name="enabling-back-navigation-support"></a>[戻る] ナビゲーションのサポートを有効にします。

UWP では、アプリケーションが別のデバイスのフォーム ファクターの間でのすべてのハードウェアとソフトウェア戻るボタン、[戻る] ナビゲーションを有効にする必要があります。 これには、イベント ハンドラーを登録することによって、`BackRequested`イベントで、上で実行できる、`OnLaunched`でネイティブ メソッド`App`クラス。

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

アプリケーションが起動されたときに、`GetForCurrentView`メソッドの取得、`SystemNavigationManager`オブジェクトは、現在のビューに関連付けられているし、イベント ハンドラーを登録、`BackRequested`イベント。 アプリケーションは、フォア グラウンド アプリケーションは、し、応答でのみこのイベントを受け取る、`OnBackRequested`イベントのハンドラー。

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

`OnBackRequested`イベント ハンドラーの呼び出し、`GoBack`アプリケーションとセットのルート フレームのメソッド、`BackRequestedEventArgs.Handled`プロパティを`true`イベントを処理済みとしてマークします。 イベントを処理済みとしてマークする発生する可能性システム (モバイル デバイスのファミリ) 上のアプリケーションから移動または (デスクトップ デバイス ファミリ) でイベントを無視しています。

アプリケーションは、電話に用意されたシステムの戻るボタンに依存していますが、デスクトップ デバイス上のタイトル バーに [戻る] ボタンを表示するかどうかを選択します。 これを実現するには、`AppViewBackButtonVisibility`プロパティのいずれかを`AppViewBackButtonVisibility`列挙値。

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated`への応答が実行される、イベント ハンドラー、`Navigated`イベントの発生は、ページ ナビゲーションが発生した場合に、タイトル バーの [戻る] ボタンの表示を更新します。 タイトル バーの [戻る] ボタンは、アプリ内のバック スタックが空ではない場合に表示されるかアプリ内のバック スタックが空の場合は、タイトル バーから削除になります。

UWP に [戻る] ナビゲーションのサポートの詳細については、次を参照してください。[ナビゲーション履歴と backwards の UWP アプリのナビゲーション](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)です。

## <a name="summary"></a>まとめ

ネイティブのフォームは、Xamarin.Forms を許可する[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-ネイティブ Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) プロジェクトで使用されるページを派生します。 ネイティブ プロジェクトが使用できる`ContentPage`-は、プロジェクトまたは .NET 標準のライブラリ プロジェクトまたはプロジェクトの共有から直接追加するページを派生します。 この記事の内容を使用する方法を説明した`ContentPage`-ネイティブ プロジェクトは、およびそれらの間を移動する方法は、直接追加するページを派生します。


## <a name="related-links"></a>関連リンク

- [NativeForms (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [ネイティブ ビュー](~/xamarin-forms/platform/native-views/index.md)
