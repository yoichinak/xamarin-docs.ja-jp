---
title: " Xamarin.Forms webview" description: "この記事では、webview クラスを使用して、 Xamarin.Forms ローカルまたはネットワークの web コンテンツとドキュメントをユーザーに表示する方法について説明します。"
ms. 製品: xamarin ms. assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 05/06/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-webview"></a>Xamarin.FormsWebView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)は、アプリで web および HTML コンテンツを表示するためのビューです。

![アプリブラウザーで](webview-images/in-app-browser.png)

## <a name="content"></a>コンテンツ

`WebView`では、次の種類のコンテンツをサポートしています。

- HTML & CSS websites &ndash; WebView では、JavaScript のサポートを含む html & css を使用して作成された web サイトが完全にサポートされています。
- ドキュメント &ndash; webview は各プラットフォームのネイティブコンポーネントを使用して実装されているため、web ビューでは、各プラットフォームで表示できるドキュメントを表示できます。 つまり、PDF ファイルは iOS と Android で動作します。
- HTML 文字列 &ndash; WebView では、メモリから html 文字列を表示できます。
- ローカルファイル &ndash; WebView では、アプリに埋め込まれている任意のコンテンツの種類を表示できます。

> [!NOTE]
> `WebView`では、Silverlight、Flash、または ActiveX コントロールが、そのプラットフォームの Internet Explorer でサポートされている場合でも、Windows ではサポートされません。

### <a name="websites"></a>Websites

インターネットから web サイトを表示するには、 `WebView` の [`Source`](xref:Xamarin.Forms.WebViewSource) プロパティを文字列の URL に設定します。

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url は、指定されたプロトコルで完全に形成されている必要があります (つまり、"http://" または "https://" が付加されている必要があります)。

#### <a name="ios-and-ats"></a>iOS と ATS

バージョン9以降では、アプリケーションは、ベストプラクティスセキュリティを実装しているサーバーとの通信のみが既定で許可されます。 安全でない `Info.plist` サーバーとの通信を可能にするには、で値を設定する必要があります。

> [!NOTE]
> アプリケーションがセキュリティで保護されていない web サイトへの接続を必要とする場合は、を使用して完全にオフにするのではなく、常にを使用してドメインを例外として入力してください `NSExceptionDomains` `NSAllowsArbitraryLoads` 。 `NSAllowsArbitraryLoads`非常に緊急な状況でのみ使用してください。

次の例では、特定のドメイン (この場合は xamarin.com) を有効にして、ATS の要件を回避する方法を示しています。

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
    ...
</key>
```

信頼されていないドメインのセキュリティを強化することをドラマしながら、信頼済みサイトを使用できるようにするには、一部のドメインのみを有効にすることをお勧めします。 次の例では、アプリの ATS を無効にする安全性の低い方法を示しています。

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
    ...
</key>
```

IOS 9 のこの新機能の詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

### <a name="html-strings"></a>HTML 文字列

コードで動的に定義された HTML の文字列を表示する場合は、のインスタンスを作成する必要があり [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) ます。

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![WebView HTML 文字列を表示する](webview-images/html-string.png)

上記のコードでは、を `@` 使用して、HTML を[逐語的文字列リテラル](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)としてマークしています。これは、ほとんどのエスケープ文字が無視されることを意味します。

> [!NOTE]
> `WidthRequest` `HeightRequest` [`WebView`](xref:Xamarin.Forms.WebView) が子であるレイアウトに応じて、HTML コンテンツを表示するには、のプロパティとプロパティを設定する必要がある場合があり `WebView` ます。 たとえば、ではこれが必須です [`StackLayout`](xref:Xamarin.Forms.StackLayout) 。

### <a name="local-html-content"></a>ローカル HTML コンテンツ

WebView では、アプリ内に埋め込まれている HTML、CSS、JavaScript のコンテンツを表示できます。 例:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamarin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

スタイル

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

すべてのプラットフォームで同じフォントが使用されるわけではないため、上記の CSS で指定したフォントはプラットフォームごとにカスタマイズする必要があることに注意してください。

を使用してローカルコンテンツを表示するには `WebView` 、HTML ファイルを他のものと同様に開いてから、のプロパティに内容を文字列として読み込む必要があり `Html` `HtmlWebViewSource` ます。 ファイルを開く方法の詳細については、「[ファイルの操作](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

次のスクリーンショットは、各プラットフォームでローカルコンテンツを表示した結果を示しています。

![Web ビューでローカルコンテンツを表示する](webview-images/local-content.png)

最初のページが読み込まれていますが、は HTML の取得元について `WebView` の情報を持っていません。 これは、ローカルリソースを参照するページを処理するときに問題になります。 ローカルページが相互にリンクされている場合、ページで個別の JavaScript ファイルが使用される場合、またはページが CSS スタイルシートにリンクされる場合などに、このような状況が発生する可能性があります。  

この問題を解決するには、ファイルシステム上のファイルの検索場所を指定する必要があり `WebView` ます。 これを行うには `BaseUrl` `HtmlWebViewSource` 、によって使用されるのプロパティを設定し `WebView` ます。

各オペレーティングシステム上のファイルシステムが異なるため、各プラットフォームでその URL を確認する必要があります。 Xamarin.Formsは、 `DependencyService` 各プラットフォームで実行時に依存関係を解決するためにを公開します。

を使用するには `DependencyService` 、まず、各プラットフォームに実装できるインターフェイスを定義します。

```csharp
public interface IBaseUrl { string Get(); }
```

各プラットフォームにインターフェイスが実装されるまで、アプリは実行されません。 Common プロジェクトでは、必ずを使用してを設定してください `BaseUrl` `DependencyService` 。

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

その後、各プラットフォームのインターフェイスの実装を提供する必要があります。

#### <a name="ios"></a>iOS

IOS では、次に示すように、web コンテンツは、build action *BundleResource*を使用してプロジェクトのルートディレクトリまたは**リソース**ディレクトリに配置されている必要があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![IOS 上のローカルファイル](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![IOS 上のローカルファイル](webview-images/ios-xs.png)

-----

は、 `BaseUrl` メインバンドルのパスに設定する必要があります。

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS
{
  public class BaseUrl_iOS : IBaseUrl
  {
    public string Get()
    {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Android では、次に示すように、ビルドアクション*Androidasset*を使用して、[Assets] フォルダーに HTML、CSS、およびイメージを配置します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Android 上のローカルファイル](webview-images/android-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![Android 上のローカルファイル](webview-images/android-xs.png)

-----

Android では、 `BaseUrl` を次のように設定する必要があり `"file:///android_asset/"` ます。

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android
{
  public class BaseUrl_Android : IBaseUrl
  {
    public string Get()
    {
      return "file:///android_asset/";
    }
  }
}
```

Android では、次のようにプロパティによって公開されている現在の Android コンテキストから**Assets**フォルダー内のファイルにアクセスすることもできます `MainActivity.Instance` 。

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) プロジェクトで、[ビルドアクション] を [*コンテンツ*] に設定して、HTML、CSS、およびイメージをプロジェクトルートに配置します。

は、次のよう `BaseUrl` に設定する必要があり `"ms-appx-web:///"` ます。

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>ナビゲーション

WebView では、使用できるようにするいくつかのメソッドとプロパティによるナビゲーションがサポートされています。

- **Goforward ()** &ndash;`CanGoForward`が true の場合、を呼び出すと、 `GoForward` 次のページに移動します。
- **GoBack ()** &ndash;`CanGoBack`が true の場合、を呼び出す `GoBack` と、最後にアクセスしたページに移動します。
- **CanGoBack** &ndash;`true`ジャンプ先のページがある場合は、 `false` ブラウザーが開始 URL にある場合はです。
- **CanGoForward** &ndash;`true`ユーザーが後方に移動し、既にアクセスされているページに進むことができる場合。

ページ内で `WebView` は、はマルチタッチジェスチャをサポートしていません。 コンテンツがモバイルに最適化されており、ズームを必要とせずに表示されることを確認することが重要です。

アプリケーションでは、 `WebView` デバイスのブラウザーではなく、内のリンクを表示するのが一般的です。 このような状況では、通常のナビゲーションを許可すると便利ですが、ユーザーが開始リンクを使用しているときにユーザーが戻ると、アプリは通常のアプリビューに戻る必要があります。

組み込みのナビゲーションメソッドとプロパティを使用して、このシナリオを有効にします。

まず、ブラウザービューのページを作成します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.InAppBrowserXaml"
             Title="Browser">
    <StackLayout Margin="20">
        <StackLayout Orientation="Horizontal">
            <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="OnBackButtonClicked" />
            <Button Text="Forward" HorizontalOptions="EndAndExpand" Clicked="OnForwardButtonClicked" />
        </StackLayout>
        <!-- WebView needs to be given height and width request within layouts to render. -->
        <WebView x:Name="webView" WidthRequest="1000" HeightRequest="1000" />
    </StackLayout>
</ContentPage>
```

コードビハインドで、次のようにします。

```csharp
public partial class InAppBrowserXaml : ContentPage
{
    public InAppBrowserXaml(string URL)
    {
        InitializeComponent();
        webView.Source = URL;
    }

    async void OnBackButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoBack)
        {
            webView.GoBack();
        }
        else
        {
            await Navigation.PopAsync();
        }
    }

    void OnForwardButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoForward)
        {
            webView.GoForward();
        }
    }
}
```

これだけです。

![WebView ナビゲーションボタン](webview-images/in-app-browser.png)

## <a name="events"></a>イベント

WebView は、状態の変化に対応するために次のイベントを発生させます。

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)– WebView が新しいページの読み込みを開始するときに発生するイベントです。
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated)–ページが読み込まれ、ナビゲーションが停止したときに発生するイベントです。
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested)–現在のコンテンツを再読み込みする要求が行われたときに発生するイベントです。

[`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)イベントに付随するオブジェクトに [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) は、次の4つのプロパティがあります。

- `Cancel`–ナビゲーションを取り消すかどうかを示します。
- `NavigationEvent`–発生したナビゲーションイベント。
- `Source`–ナビゲーションを実行した要素。
- `Url`: ナビゲーション先。

[`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs)イベントに付随するオブジェクトに [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) は、次の4つのプロパティがあります。

- `NavigationEvent`–発生したナビゲーションイベント。
- `Result`–列挙メンバーを使用したナビゲーションの結果について説明し [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) ます。 有効な値は、`Cancel`、`Failure`、`Success`、`Timeout` です。
- `Source`–ナビゲーションを実行した要素。
- `Url`: ナビゲーション先。

読み込みに長い時間がかかる web ページを使用することが予想される場合は、イベントとイベントを使用して [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) 状態インジケーターを実装することを検討してください。 例:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.LoadingLabelXaml"
             Title="Loading Demo">
    <StackLayout>
        <!--Loading label should not render by default.-->
        <Label x:Name="labelLoading" Text="Loading..." IsVisible="false" />
        <WebView HeightRequest="1000" WidthRequest="1000" Source="http://www.xamarin.com" Navigated="webviewNavigated" Navigating="webviewNavigating" />
    </StackLayout>
</ContentPage>
```

2つのイベントハンドラー:

```csharp
void webviewNavigating(object sender, WebNavigatingEventArgs e)
{
    labelLoading.IsVisible = true;
}

void webviewNavigated(object sender, WebNavigatedEventArgs e)
{
    labelLoading.IsVisible = false;
}
```

この結果、次の出力 (読み込み) が行われます。

![WebView ナビゲーションイベントの例](webview-images/loading-start.png)

読み込みが完了しました:

![WebView ナビゲーションイベントの例](webview-images/loading-end.png)

## <a name="reloading-content"></a>コンテンツの再読み込み

[`WebView`](xref:Xamarin.Forms.WebView)には、 `Reload` 現在のコンテンツを再読み込みするために使用できるメソッドがあります。

```csharp
var webView = new WebView();
...
webView.Reload();
```

`Reload`メソッドが呼び出されると `ReloadRequested` 、イベントが発生し、現在のコンテンツを再読み込みする要求が行われたことを示します。

## <a name="performance"></a>パフォーマンス

人気の web ブラウザーでは、ハードウェアアクセラレータレンダリングや JavaScript コンパイルなどのテクノロジが採用されています。 4.4 より前 Xamarin.Forms では、は Xamarin.Forms `WebView` クラスによって iOS に実装されていま `UIWebView` した。 ただし、この実装では、これらのテクノロジの多くを利用できませんでした。 したがって、 Xamarin.Forms 4.4 以降、は、 Xamarin.Forms `WebView` より高速な参照をサポートするクラスによって iOS に実装され `WkWebView` ます。

> [!NOTE]
> IOS では、に `WkWebViewRenderer` 引数を受け取るコンストラクターオーバーロードがあり `WkWebViewConfiguration` ます。 これにより、作成時にレンダラーを構成できます。

アプリケーションで `UIWebView` は、 Xamarin.Forms `WebView` 互換性の理由により、iOS クラスを使用してを実装することができます。 これを実現するには、アプリケーションの iOS platform プロジェクトの**AssemblyInfo.cs**ファイルに次のコードを追加します。

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView`既定では、Android では、組み込みのブラウザーほど高速です。

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view)では、Microsoft Edge レンダリングエンジンを使用します。 デスクトップデバイスとタブレットデバイスでは、Edge ブラウザー自体を使用した場合と同じパフォーマンスが表示されます。

## <a name="permissions"></a>アクセス許可

`WebView`を機能させるには、各プラットフォームに対してアクセス許可が設定されていることを確認する必要があります。 一部のプラットフォームでは、 `WebView` はデバッグモードで機能しますが、リリース用にビルドされる場合は動作しないことに注意してください。 これは、Android でのインターネットアクセスのような一部のアクセス許可は、既定でデバッグモードのときに Visual Studio for Mac 設定されているためです。

- **UWP** &ndash;ネットワークコンテンツを表示するときに、インターネット (クライアント & サーバー) 機能が必要です。
- **Android** &ndash;`INTERNET`ネットワークのコンテンツを表示する場合にのみ必要です。 ローカルコンテンツには特別なアクセス許可は必要ありません。
- **iOS** &ndash;特別なアクセス許可は必要ありません。

## <a name="layout"></a>Layout

他のほとんどのビューとは異なり Xamarin.Forms 、で `WebView` `HeightRequest` は、 `WidthRequest` Stacklayout または RelativeLayout に含まれるときにとが指定されている必要があります。 これらのプロパティを指定しなかった場合、は `WebView` 表示されません。

次の例では、をレンダリングするためのレイアウトを示し `WebView` ます。

幅の要求 & 高さ要求の StackLayout:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout with 幅要求 & 高さ要求:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout の幅を & 要求し*ない*の高さの要求:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

高さ要求の*ない*グリッドは、高さ要求を & ます。 Grid は、要求された高さと幅の指定を必要としない、いくつかのレイアウトの1つです。

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

## <a name="invoking-javascript"></a>JavaScript の呼び出し

[`WebView`](xref:Xamarin.Forms.WebView)c# から JavaScript 関数を呼び出して、呼び出し元の C# コードに結果を返す機能が含まれています。 これは、次の [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)サンプルの例に示すように、メソッドを使用して実現されます。

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

メソッドは、 [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) 引数として指定された JavaScript を評価し、結果をとして返し `string` ます。 この例では、 `factorial` JavaScript 関数が呼び出され、結果としての階乗が返され `number` ます。 この JavaScript 関数は、によって読み込まれるローカルの HTML ファイルで定義され、次の例のようになり [`WebView`](xref:Xamarin.Forms.WebView) ます。

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="cookies"></a>クッキー

クッキーはに設定でき [`WebView`](xref:Xamarin.Forms.WebView) 、その後、web 要求と共に指定された URL に送信されます。 これを実現するには、オブジェクトをに追加 `Cookie` `CookieContainer` します。このオブジェクトは、バインド可能なプロパティの値として設定され `WebView.Cookies` ます。 この例を次のコードに示します。

```csharp
using System.Net;
using Xamarin.Forms;
// ...

CookieContainer cookieContainer = new CookieContainer();
Uri uri = new Uri("https://dotnet.microsoft.com/apps/xamarin", UriKind.RelativeOrAbsolute);

Cookie cookie = new Cookie
{
    Name = "XamarinCookie",
    Expires = DateTime.Now.AddDays(1),
    Value = "My cookie",
    Domain = uri.Host,
    Path = "/"
};
cookieContainer.Add(uri, cookie);
webView.Cookies = cookieContainer;
webView.Source = new UrlWebViewSource { Url = uri.ToString() };
```

この例では、1つの `Cookie` がオブジェクトに追加され、 `CookieContainer` その後、プロパティの値として設定され `WebView.Cookies` ます。 が [`WebView`](xref:Xamarin.Forms.WebView) 指定された URL に web 要求を送信すると、クッキーが要求と共に送信されます。

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView の廃止と App Store の拒否 (ITMS-90809)

2020年4月以降、 [Apple は](https://developer.apple.com/news/?id=12232019b)非推奨の API をまだ使用しているアプリを拒否し `UIWebView` ます。 Xamarin.Formsは `WKWebView` 既定値としてに切り替えられていますが、バイナリに以前の SDK への参照があり Xamarin.Forms ます。 現在の[iOS リンカー](~/ios/deploy-test/linker.md)の動作では、これは削除されません。そのため、アプリストアに送信するときに、非推奨の `UIWebView` API はアプリから参照されているように見えます。

この問題を解決するには、リンカーのプレビューバージョンを使用できます。 プレビューを有効にするには、リンカーに追加の引数を指定する必要があり `--optimize=experimental-xforms-product-type` ます。

これを行うための前提条件は次のとおりです。

- ** Xamarin.Forms 4.5 以上**。 Xamarin.Formsアプリで素材ビジュアルを使用する場合は、4.6 以上が必要です。
- **13.10.0.17 以上**。 [Visual Studio で](~/cross-platform/troubleshooting/questions/version-logs.md#version-information)Xamarin iOS のバージョンを確認します。 このバージョンの Xamarin. iOS は Visual Studio for Mac dbms-guide-8.4.1 と Visual Studio 16.4.3 に含まれています。
- **への参照 `UIWebView` を削除**します。 コードには、またはを使用するクラスへの参照を含めることはできません `UIWebView` `UIWebView` 。

参照の検出と削除の詳細について `UIWebView` は、「 [uiwebview の廃止](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)」を参照してください。

### <a name="configure-the-linker"></a>リンカーを構成する

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

リンカーで参照を削除するには、次の手順に従い `UIWebView` ます。

1. **IOS プロジェクトのプロパティ** &ndash; を開くIOS プロジェクトを右クリックし、[**プロパティ**] を選択します。
1. **IOS のビルドセクション** &ndash; に移動します。[ **IOS ビルド**] セクションを選択します。
1. **追加の mtouch 引数** &ndash; を更新する追加の**mtouch 引数**で、このフラグ `--optimize=experimental-xforms-product-type` を追加します (既に存在している可能性がある値に加えて)。 注: このフラグは、SDK に対して**のみ**設定される**リンカーの動作**、またはすべてを**リンク**します。 何らかの理由で、リンカーの動作を All に設定したときにエラーが発生した場合は、おそらく、アプリケーションコードまたはリンカーセーフではないサードパーティ製のライブラリ内で問題が発生している可能性があります。 リンカーの詳細については、「 [Xamarin IOS アプリのリンク](~/ios/deploy-test/linker.md)」を参照してください。
1. **すべてのビルド構成** &ndash; を更新するウィンドウの上部にある [**構成**] と [**プラットフォーム**] の一覧を使用して、すべてのビルド構成を更新します。 更新する最も重要な構成は、**リリース/iPhone**の構成です。通常、これは、App Store の送信用にビルドを作成するために使用されるためです。

このスクリーンショットでは、新しいフラグが設定されたウィンドウが表示されます。

[![IOS のビルドセクションでフラグを設定する](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

リンカーで参照を削除するには、次の手順に従い `UIWebView` ます。

1. **IOS プロジェクトオプション** &ndash; を開くIOS プロジェクトを右クリックし、[**オプション**] を選択します。
1. **IOS のビルドセクション** &ndash; に移動します。[ **IOS ビルド**] セクションを選択します。
1. **_mtouch_ **追加の Mtouch 引数の追加の mtouch 引数を更新し &ndash; ます。このフラグ** _mtouch_ ** `--optimize=experimental-xforms-product-type` は、既に存在している可能性がある値に加えて追加します。 注: このフラグは、SDK に対して**のみ**設定される**リンカーの動作**、またはすべてを**リンク**します。 何らかの理由で、リンカーの動作を All に設定したときにエラーが発生した場合は、おそらく、アプリケーションコードまたはリンカーセーフではないサードパーティ製のライブラリ内で問題が発生している可能性があります。 リンカーの詳細については、「 [Xamarin IOS アプリのリンク](~/ios/deploy-test/linker.md)」を参照してください。
1. **すべてのビルド構成** &ndash; を更新するウィンドウの上部にある [**構成**] と [**プラットフォーム**] の一覧を使用して、すべてのビルド構成を更新します。 更新する最も重要な構成は、**リリース/iPhone**の構成です。通常、これは、App Store の送信用にビルドを作成するために使用されるためです。

このスクリーンショットでは、新しいフラグが設定されたウィンドウが表示されます。

[![IOS のビルドセクションでフラグを設定する](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

新しい (リリース) ビルドを作成して App Store に送信すると、非推奨の API に関する警告は表示されなくなります。

## <a name="related-links"></a>関連リンク

- [WebView の操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
- [UIWebView の廃止](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)
