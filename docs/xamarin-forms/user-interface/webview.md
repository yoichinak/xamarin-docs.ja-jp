---
title: Xamarin.Forms の WebView
description: この記事では、ユーザーに Xamarin.Forms WebView クラスを使用して、ローカルを表示する方法、またはネットワークの web コンテンツおよびドキュメントについて説明します。
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 26fbe6af639c67a94408605ba456bb3a100d2355
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305889"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms の WebView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)は、アプリで WEB および HTML コンテンツを表示するためのビューです。

![アプリブラウザーで](webview-images/in-app-browser.png)

## <a name="content"></a>コンテンツ

`WebView` は、次の種類のコンテンツをサポートしています。

- HTML & CSS websites &ndash; web ビューでは、JavaScript のサポートを含む HTML & CSS を使用して作成された web サイトが完全にサポートされています。
- ドキュメント &ndash; web ビューは各プラットフォームでネイティブコンポーネントを使用して実装されているため、WebView は各プラットフォームで表示できるドキュメントを表示できます。 IOS と Android で PDF ファイルが動作することを意味します。
- WebView &ndash; HTML 文字列では、メモリから HTML 文字列を表示できます。
- Web ビュー &ndash; ローカルファイルでは、上に示したコンテンツの種類のいずれかをアプリに埋め込むことができます。

> [!NOTE]
> Windows の `WebView` は、そのプラットフォームの Internet Explorer でサポートされている場合でも、Silverlight、Flash、または ActiveX コントロールをサポートしていません。

### <a name="websites"></a>Web サイト

インターネットから web サイトを表示するには、`WebView`の[`Source`](xref:Xamarin.Forms.WebViewSource)プロパティを文字列の URL に設定します。

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url が指定されているプロトコルに完全形式でなければなりません (つまりが必要"http://"または先頭に"https://")。

#### <a name="ios-and-ats"></a>iOS および ATS

バージョン 9、以降、iOS は、既定でセキュリティのベスト プラクティスを実装しているサーバーと通信するアプリケーションにのみ許可はします。 セキュリティで保護されていないサーバーとの通信を可能にするには `Info.plist` で値を設定する必要があります

> [!NOTE]
> アプリケーションがセキュリティで保護されていない web サイトへの接続を必要とする場合は、常に `NSAllowsArbitraryLoads`を使用して完全に有効にするのではなく、必ず `NSExceptionDomains` を使用してドメインを例外として入力してください。 `NSAllowsArbitraryLoads` は、極端な緊急の場合にのみ使用してください。

ATS 要件をバイパスする (このケース xamarin.com) で特定のドメインを有効にする方法を次に示します。

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

ベスト プラクティスのみを信頼済みサイトを使用して、信頼されていないドメインに追加のセキュリティを活用しながら、ATS をバイパスする一部のドメインを有効にすることをお勧めします。 アプリの ATS を無効にするとの安全性の低いメソッドを次に示します。

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

コードで動的に定義された HTML の文字列を表示する場合は、 [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource)のインスタンスを作成する必要があります。

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

上記のコードでは、`@` を使用して、HTML を[逐語的文字列リテラル](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals)としてマークしています。これは、ほとんどのエスケープ文字が無視されることを意味します。

> [!NOTE]
> `WebView` が子であるレイアウトに応じて、HTML コンテンツを表示するには、 [`WebView`](xref:Xamarin.Forms.WebView)の `WidthRequest` と `HeightRequest` のプロパティを設定することが必要になる場合があります。 たとえば、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)ではこれが必須です。

### <a name="local-html-content"></a>ローカルの HTML コンテンツ

WebView は、HTML、CSS からのコンテンツを表示でき、アプリ内で Javascript が埋め込まれています。 例 :

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

CSS の場合:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

上記の CSS で指定したフォントは、すべてのプラットフォームに同じフォントをプラットフォームごとにカスタマイズする必要に注意してください。

`WebView`を使用してローカルコンテンツを表示するには、他のように HTML ファイルを開き、`HtmlWebViewSource`の `Html` プロパティに文字列として内容を読み込む必要があります。 ファイルを開く方法の詳細については、「[ファイルの操作](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

次のスクリーン ショットは、ローカル コンテンツを表示する各プラットフォームでの結果を表示します。

![Web ビューでローカルコンテンツを表示する](webview-images/local-content.png)

最初のページが読み込まれていますが、`WebView` は HTML の取得元についての情報を持っていません。 ローカル リソースを参照するページを処理するときに問題です。 ときに発生する可能性がありますの例には、それぞれにその他 ページは、ローカルのページ リンクを別の JavaScript ファイルの使用し、CSS スタイル シートにページが含まれます。  

この問題を解決するには、ファイルシステム上のファイルを検索する場所を `WebView` に指示する必要があります。 そのためには、`WebView`によって使用される `HtmlWebViewSource` の `BaseUrl` プロパティを設定します。

各オペレーティング システム上のファイル システムが異なるため、確認する必要があります。 各プラットフォームでその URL。 Xamarin は、各プラットフォームで実行時に依存関係を解決するための `DependencyService` を公開します。

`DependencyService`を使用するには、まず、各プラットフォームに実装できるインターフェイスを定義します。

```csharp
public interface IBaseUrl { string Get(); }
```

各プラットフォームで、インターフェイスを実装するまで、アプリを実行できないことに注意してください。 Common プロジェクトでは、必ず `DependencyService`を使用して `BaseUrl` を設定してください。

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

各プラットフォーム用のインターフェイスの実装が提供する必要があります。

#### <a name="ios"></a>iOS

IOS では、次に示すように、web コンテンツは、build action *BundleResource*を使用してプロジェクトのルートディレクトリまたは**リソース**ディレクトリに配置されている必要があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![IOS 上のローカルファイル](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![IOS 上のローカルファイル](webview-images/ios-xs.png)

-----

`BaseUrl` は、メインバンドルのパスに設定する必要があります。

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

Android では、`BaseUrl` を `"file:///android_asset/"`に設定する必要があります。

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

Android では、`MainActivity.Instance` プロパティによって公開されている現在の Android コンテキストを使用して、 **Assets**フォルダー内のファイルにアクセスすることもできます。

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) プロジェクトで、[ビルドアクション] を [*コンテンツ*] に設定して、HTML、CSS、およびイメージをプロジェクトルートに配置します。

`BaseUrl` を `"ms-appx-web:///"`に設定する必要があります。

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

Web ビューには、いくつかのメソッドとプロパティが使用可能なナビゲーションがサポートされています。

- **Goforward ()** &ndash; `CanGoForward` が true の場合、`GoForward` を呼び出すと、次に閲覧されたページに移動します。
- **GoBack ()** &ndash; `CanGoBack` が true の場合、`GoBack` を呼び出すと最後に閲覧されたページに移動します。
- **CanGoBack** &ndash; `true` 戻るページがある場合は、ブラウザーが開始 URL にあるかどうか `false` ます。
- ユーザーが後方に移動し、既にアクセスされているページに進むことができる場合は、 **CanGoForward** &ndash; `true` ます。

ページ内では、`WebView` はマルチタッチジェスチャをサポートしていません。 重要なことを確認するコンテンツをモバイルに最適化されたし、ズームがなくても表示されます。

アプリケーションでは、デバイスのブラウザーではなく、`WebView`内にリンクを表示するのが一般的です。 これらの状況では、通常のナビゲーションを許可すると便利ですが、ユーザーからのバックアップ開始リンク上にあるときに、アプリは、通常のアプリ ビューを返す必要があります。

このシナリオを有効にするのにには、組み込みのナビゲーション メソッドとプロパティを使用します。

ブラウザー ビューのページを作成して開始します。

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

コードの分離。

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

以上で終わりです。

![WebView ナビゲーションボタン](webview-images/in-app-browser.png)

## <a name="events"></a>イベント

Web ビューは、状態の変化に対応するために、次のイベントを発生させます。

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) – WebView が新しいページの読み込みを開始するときに発生するイベントです。
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) –ページが読み込まれ、ナビゲーションが停止したときに発生するイベントです。
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested) –現在のコンテンツを再読み込みする要求が行われたときに発生するイベントです。

[`Navigating`](xref:Xamarin.Forms.WebView.Navigating)イベントに付随する[`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)オブジェクトには、次の4つのプロパティがあります。

- `Cancel` –ナビゲーションを取り消すかどうかを示します。
- `NavigationEvent` –発生したナビゲーションイベント。
- `Source` –ナビゲーションを実行した要素。
- `Url` –ナビゲーション先。

[`Navigated`](xref:Xamarin.Forms.WebView.Navigated)イベントに付随する[`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs)オブジェクトには、次の4つのプロパティがあります。

- `NavigationEvent` –発生したナビゲーションイベント。
- `Result` – [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult)列挙メンバーを使用した、ナビゲーションの結果について説明します。 有効な値は `Cancel`、`Failure`、`Success`、および `Timeout` です。
- `Source` –ナビゲーションを実行した要素。
- `Url` –ナビゲーション先。

読み込みに長い時間がかかる web ページを使用することが予想される場合は、 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)イベントと[`Navigated`](xref:Xamarin.Forms.WebView.Navigated)イベントを使用して状態インジケーターを実装することを検討してください。 例 :

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

2 つのイベントのハンドラー:

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

これは、(読み込み)、次の出力が得られます。

![WebView ナビゲーションイベントの例](webview-images/loading-start.png)

完成した読み込み:

![WebView ナビゲーションイベントの例](webview-images/loading-end.png)

## <a name="reloading-content"></a>コンテンツを再読み込み

[`WebView`](xref:Xamarin.Forms.WebView)には、現在のコンテンツを再読み込みするために使用できる `Reload` メソッドがあります。

```csharp
var webView = new WebView();
...
webView.Reload();
```

`Reload` メソッドが呼び出されると、`ReloadRequested` イベントが発生し、現在のコンテンツを再読み込みする要求が行われたことを示します。

## <a name="performance"></a>パフォーマンス テスト

人気の web ブラウザーでは、ハードウェアアクセラレータレンダリングや JavaScript コンパイルなどのテクノロジが採用されています。 Xamarin. Forms 4.4 より前では、`UIWebView` クラスによって `WebView` が iOS に実装されていました。 ただし、この実装では、これらのテクノロジの多くを利用できませんでした。 そのため、Xamarin. 4.4 フォーム `WebView` は、より高速な参照をサポートする `WkWebView` クラスによって iOS に実装されます。

> [!NOTE]
> IOS では、`WkWebViewRenderer` には `WkWebViewConfiguration` 引数を受け取るコンストラクターオーバーロードがあります。 これにより、作成時にレンダラーを構成できます。

アプリケーションは、iOS `UIWebView` クラスを使用して、互換性上の理由から Xamarin. フォーム `WebView`を実装することができます。 これを実現するには、アプリケーションの iOS platform プロジェクトの**AssemblyInfo.cs**ファイルに次のコードを追加します。

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

既定では、Android の `WebView` は、組み込みのブラウザーほど高速です。

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view)では、Microsoft Edge レンダリングエンジンを使用します。 デスクトップ、タブレット デバイスには、Edge ブラウザー自体を使用する場合と同じパフォーマンスが表示されます。

## <a name="permissions"></a>アクセス許可

`WebView` を機能させるには、プラットフォームごとにアクセス許可が設定されていることを確認する必要があります。 一部のプラットフォームでは、`WebView` はデバッグモードで機能しますが、リリース用にビルドされる場合は動作しないことに注意してください。 Android では、インターネット アクセスと同様の一部のアクセス許可は、デバッグ モードでの Mac の Visual Studio によって既定で設定されているためにです。

- **UWP** &ndash; には、ネットワークコンテンツを表示するときにインターネット (クライアント & サーバー) 機能が必要です。
- **Android** &ndash; では、ネットワークからコンテンツを表示する場合にのみ `INTERNET` が必要です。 ローカル コンテンツには、特殊なアクセス許可は不要です。
- **iOS** &ndash; には特別なアクセス許可は必要ありません。

## <a name="layout"></a>[レイアウト]

他の多くの Xamarin 形式のビューとは異なり、`WebView` では、StackLayout または RelativeLayout に含まれるときに `HeightRequest` と `WidthRequest` を指定する必要があります。 これらのプロパティを指定しなかった場合、`WebView` は表示されません。

次の例では、`WebView`s をレンダリングするために動作するレイアウトを示します。

WidthRequest & HeightRequest StackLayout:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

WidthRequest & HeightRequest [相対レイアウト]:

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

高さ要求の*ない*グリッドは、高さ要求を & ます。 グリッドは、要求された高さと幅を指定することを必要としないいくつかのレイアウトの 1 つです。 します。

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

[`WebView`](xref:Xamarin.Forms.WebView)には、からC#JavaScript 関数を呼び出し、結果を呼び出し元C#のコードに返す機能が含まれています。 これを行うには、 [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)サンプルの次の例に示すように、 [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*)メソッドを使用します。

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*)メソッドは、引数として指定された JavaScript を評価し、すべての結果を `string`として返します。 この例では、`factorial` JavaScript 関数が呼び出され、結果として `number` の階乗が返されます。 この JavaScript 関数は、 [`WebView`](xref:Xamarin.Forms.WebView)によって読み込まれるローカルの HTML ファイルで定義されています。次の例を参照してください。

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

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>UIWebView の廃止と App Store の拒否 (ITMS-90809)

2020年4月以降、 [Apple は](https://developer.apple.com/news/?id=12232019b)非推奨の `UIWebView` API を引き続き使用するアプリを拒否します。 Xamarin. Forms が既定の `WKWebView` に切り替えられましたが、Xamarin のバイナリに古い SDK への参照が残っています。 現在の[iOS リンカー](~/ios/deploy-test/linker.md)の動作では、これは削除されません。そのため、アプリストアに送信するときに、非推奨の `UIWebView` API はアプリから参照されているように見えます。

この問題を解決するには、リンカーのプレビューバージョンを使用できます。 プレビューを有効にするには、リンカーに `--optimize=experimental-xforms-product-type` 追加の引数を指定する必要があります。 

これを行うための前提条件は次のとおりです。

- Xamarin. **forms 4.5 以降**&ndash; プレリリースバージョンの Xamarin. forms 4.5 を使用できます。
- **13.10.0.17 以降**&ndash; [Visual Studio で](~/cross-platform/troubleshooting/questions/version-logs.md#version-information)xamarin の ios バージョンを確認します。 このバージョンの Xamarin. iOS は Visual Studio for Mac dbms-guide-8.4.1 と Visual Studio 16.4.3 に含まれています。
- &ndash; **`UIWebView`への参照を削除**します。コードには、`UIWebView` または `UIWebView`を使用するクラスへの参照を含めることはできません。

### <a name="configure-the-linker-preview"></a>リンカープレビューの構成

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

リンカーで `UIWebView` 参照を削除するには、次の手順に従います。

1. Ios**プロジェクトのプロパティを開き**&ndash; ios プロジェクトを右クリックし、 **[プロパティ]** を選択します。
1. [ **Ios ビルド] セクションに移動**し、 **[ios ビルド]** セクション &ndash; 選択します。
1. 追加の mtouch 引数に &ndash; 追加の**mtouch 引数を更新**します。このフラグは、既に存在している可能性がある値に加え**て、このフラグ `--optimize=experimental-xforms-product-type` 追加し**ます。 注: このフラグは、SDK に対して**のみ**設定される**リンカーの動作**、またはすべてを**リンク**します。 何らかの理由で、リンカーの動作を All に設定したときにエラーが発生した場合は、おそらく、アプリケーションコードまたはリンカーセーフではないサードパーティ製のライブラリ内で問題が発生している可能性があります。 リンカーの詳細については、「 [Xamarin IOS アプリのリンク](~/ios/deploy-test/linker.md)」を参照してください。
1. **すべてのビルド構成を更新**し &ndash; ウィンドウの上部にある **[構成]** と **[プラットフォーム]** の一覧を使用して、すべてのビルド構成を更新します。 更新する最も重要な構成は、**リリース/iPhone**の構成です。通常、これは、App Store の送信用にビルドを作成するために使用されるためです。

このスクリーンショットでは、新しいフラグが設定されたウィンドウが表示されます。

[iOS ビルドセクションでフラグを設定 ![](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

リンカーで `UIWebView` 参照を削除するには、次の手順に従います。

1. Ios**プロジェクトオプション &ndash; 開き**、ios プロジェクトを右クリックして、 **[オプション]** を選択します。
1. [ **Ios ビルド] セクションに移動**し、 **[ios ビルド]** セクション &ndash; 選択します。
1. 追加の mtouch 引数に &ndash; 追加の mtouch 引数を更新します。このフラグは、既に存在している可能性がある値に加え**て、このフラグ `--optimize=experimental-xforms-product-type` 追加し** ます。 注: このフラグは、SDK に対して**のみ**設定される**リンカーの動作**、またはすべてを**リンク**します。 何らかの理由で、リンカーの動作を All に設定したときにエラーが発生した場合は、おそらく、アプリケーションコードまたはリンカーセーフではないサードパーティ製のライブラリ内で問題が発生している可能性があります。 リンカーの詳細については、「 [Xamarin IOS アプリのリンク](~/ios/deploy-test/linker.md)」を参照してください。
1. **すべてのビルド構成を更新**し &ndash; ウィンドウの上部にある **[構成]** と **[プラットフォーム]** の一覧を使用して、すべてのビルド構成を更新します。 更新する最も重要な構成は、**リリース/iPhone**の構成です。通常、これは、App Store の送信用にビルドを作成するために使用されるためです。

このスクリーンショットでは、新しいフラグが設定されたウィンドウが表示されます。

[iOS ビルドセクションでフラグを設定 ![](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

新しい (リリース) ビルドを作成して App Store に送信すると、非推奨の API に関する警告は表示されなくなります。

## <a name="related-links"></a>関連リンク

- [WebView の操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
