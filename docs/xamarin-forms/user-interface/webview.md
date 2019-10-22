---
title: Xamarin. Forms WebView
description: この記事では、Xamarin. Forms WebView クラスを使用して、ローカルまたはネットワークの web コンテンツとドキュメントをユーザーに表示する方法について説明します。
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2019
ms.openlocfilehash: cdb2a9f1a37dc7bca458a4291ce6fe5ac9c120aa
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696053"
---
# <a name="xamarinforms-webview"></a>Xamarin. Forms WebView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)は、アプリで WEB および HTML コンテンツを表示するためのビューです。

![アプリブラウザーで](webview-images/in-app-browser.png)

## <a name="content"></a>Content

`WebView` は、次の種類のコンテンツをサポートしています。

- HTML & CSS websites &ndash; web ビューでは、JavaScript のサポートを含む HTML & CSS を使用して作成された web サイトが完全にサポートされています。
- ドキュメント &ndash; web ビューは各プラットフォームでネイティブコンポーネントを使用して実装されているため、WebView は各プラットフォームで表示できるドキュメントを表示できます。 つまり、PDF ファイルは iOS と Android で動作します。
- WebView &ndash; HTML 文字列では、メモリから HTML 文字列を表示できます。
- Web ビュー &ndash; ローカルファイルでは、上に示したコンテンツの種類のいずれかをアプリに埋め込むことができます。

> [!NOTE]
> Windows の `WebView` は、そのプラットフォームの Internet Explorer でサポートされている場合でも、Silverlight、Flash、または ActiveX コントロールをサポートしていません。

### <a name="websites"></a>Websites

インターネットから web サイトを表示するには、`WebView` の[`Source`](xref:Xamarin.Forms.WebViewSource)プロパティを文字列の URL に設定します。

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url は、指定されたプロトコルで完全に形成されている必要があります (つまり、"http://" または "https://" が付加されている必要があります)。

#### <a name="ios-and-ats"></a>iOS と ATS

バージョン9以降では、アプリケーションは、ベストプラクティスセキュリティを実装しているサーバーとの通信のみが既定で許可されます。 セキュリティで保護されていないサーバーとの通信を可能にするには `Info.plist` で値を設定する必要があります

> [!NOTE]
> アプリケーションがセキュリティで保護されていない web サイトへの接続を必要とする場合は、常に `NSAllowsArbitraryLoads` を使用して完全に有効にするのではなく、必ず `NSExceptionDomains` を使用してドメインを例外として入力してください。 `NSAllowsArbitraryLoads` は、極端な緊急の場合にのみ使用してください。

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

上記のコードでは、`@` を使用して HTML を文字列リテラルとしてマークしています。これは、通常のすべてのエスケープ文字が無視されることを意味します。

> [!NOTE]
> @No__t_4 が子であるレイアウトに応じて、HTML コンテンツを表示するには、 [`WebView`](xref:Xamarin.Forms.WebView)の `WidthRequest` と `HeightRequest` のプロパティを設定することが必要になる場合があります。 たとえば、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)ではこれが必須です。

### <a name="local-html-content"></a>ローカル HTML コンテンツ

WebView では、アプリ内に埋め込まれている HTML、CSS、Javascript のコンテンツを表示できます。 (例:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
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

@No__t_0 を使用してローカルコンテンツを表示するには、他のように HTML ファイルを開き、`HtmlWebViewSource` の `Html` プロパティに文字列として内容を読み込む必要があります。 ファイルを開く方法の詳細については、「[ファイルの操作](~/xamarin-forms/data-cloud/data/files.md)」を参照してください。

次のスクリーンショットは、各プラットフォームでローカルコンテンツを表示した結果を示しています。

![Web ビューでローカルコンテンツを表示する](webview-images/local-content.png)

最初のページが読み込まれていますが、`WebView` は HTML の取得元についての情報を持っていません。 これは、ローカルリソースを参照するページを処理するときに問題になります。 ローカルページが相互にリンクされている場合、ページで個別の JavaScript ファイルが使用される場合、またはページが CSS スタイルシートにリンクされる場合などに、このような状況が発生する可能性があります。  

この問題を解決するには、ファイルシステム上のファイルを検索する場所を `WebView` に指示する必要があります。 そのためには、`WebView` によって使用される `HtmlWebViewSource` の `BaseUrl` プロパティを設定します。

各オペレーティングシステム上のファイルシステムが異なるため、各プラットフォームでその URL を確認する必要があります。 Xamarin は、各プラットフォームで実行時に依存関係を解決するための `DependencyService` を公開します。

@No__t_0 を使用するには、まず、各プラットフォームに実装できるインターフェイスを定義します。

```csharp
public interface IBaseUrl { string Get(); }
```

各プラットフォームにインターフェイスが実装されるまで、アプリは実行されません。 Common プロジェクトでは、必ず `DependencyService` を使用して `BaseUrl` を設定してください。

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

その後、各プラットフォームのインターフェイスの実装を提供する必要があります。

#### <a name="ios"></a>iOS

IOS では、次に示すように、web コンテンツは、build action *BundleResource*を使用してプロジェクトのルートディレクトリまたは**リソース**ディレクトリに配置されている必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![IOS 上のローカルファイル](webview-images/ios-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![IOS 上のローカルファイル](webview-images/ios-xs.png)

-----

@No__t_0 は、メインバンドルのパスに設定する必要があります。

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

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android 上のローカルファイル](webview-images/android-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android 上のローカルファイル](webview-images/android-xs.png)

-----

Android では、`BaseUrl` を `"file:///android_asset/"` に設定する必要があります。

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

@No__t_0 を `"ms-appx-web:///"` に設定する必要があります。

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

- **Goforward ()** &ndash; `CanGoForward` が true の場合、`GoForward` を呼び出すと、次に閲覧されたページに移動します。
- **GoBack ()** &ndash; `CanGoBack` が true の場合、`GoBack` を呼び出すと最後に閲覧されたページに移動します。
- **CanGoBack** &ndash; `true` 戻るページがある場合は、ブラウザーが開始 URL にあるかどうか `false` ます。
- ユーザーが後方に移動し、既にアクセスされているページに進むことができる場合は、 **CanGoForward** &ndash; `true` ます。

ページ内では、`WebView` はマルチタッチジェスチャをサポートしていません。 コンテンツがモバイルに最適化されており、ズームを必要とせずに表示されることを確認することが重要です。

アプリケーションでは、デバイスのブラウザーではなく、`WebView` 内にリンクを表示するのが一般的です。 このような状況では、通常のナビゲーションを許可すると便利ですが、ユーザーが開始リンクを使用しているときにユーザーが戻ると、アプリは通常のアプリビューに戻る必要があります。

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

これで完了です。

![WebView ナビゲーションボタン](webview-images/in-app-browser.png)

## <a name="events"></a>イベント

WebView は、状態の変化に対応するために次のイベントを発生させます。

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) – WebView が新しいページの読み込みを開始するときに発生するイベントです。
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) –ページが読み込まれ、ナビゲーションが停止したときに発生するイベントです。
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested) –現在のコンテンツを再読み込みする要求が行われたときに発生するイベントです。

[@No__t_3](xref:Xamarin.Forms.WebView.Navigating)イベントに付随する[`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)オブジェクトには、次の4つのプロパティがあります。

- `Cancel` –ナビゲーションを取り消すかどうかを示します。
- `NavigationEvent` –発生したナビゲーションイベント。
- `Source` –ナビゲーションを実行した要素。
- `Url` –ナビゲーション先。

[@No__t_3](xref:Xamarin.Forms.WebView.Navigated)イベントに付随する[`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs)オブジェクトには、次の4つのプロパティがあります。

- `NavigationEvent` –発生したナビゲーションイベント。
- `Result` – [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult)列挙メンバーを使用した、ナビゲーションの結果について説明します。 正しい値は `Cancel`、`Failure`、`Success`、`Timeout` です。
- `Source` –ナビゲーションを実行した要素。
- `Url` –ナビゲーション先。

読み込みに長い時間がかかる web ページを使用することが予想される場合は、 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)イベントと[`Navigated`](xref:Xamarin.Forms.WebView.Navigated)イベントを使用して状態インジケーターを実装することを検討してください。 (例:

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

[`WebView`](xref:Xamarin.Forms.WebView)には、現在のコンテンツを再読み込みするために使用できる `Reload` メソッドがあります。

```csharp
var webView = new WebView();
...
webView.Reload();
```

@No__t_0 メソッドが呼び出されると、`ReloadRequested` イベントが発生し、現在のコンテンツを再読み込みする要求が行われたことを示します。

## <a name="performance"></a>パフォーマンス

人気の web ブラウザーでは、ハードウェアアクセラレータレンダリングや JavaScript コンパイルなどのテクノロジが採用されています。 IOS では、既定では、Xamarin の `WebView` は `UIWebView` クラスによって実装されます。これらのテクノロジの多くは、この実装では使用できません。 ただし、アプリケーションでは、iOS `WkWebView` クラスを使用して、より高速な参照をサポートする Xamarin. Forms `WebView` を実装することをオプトインできます。 これを実現するには、アプリケーションの iOS platform プロジェクトの**AssemblyInfo.cs**ファイルに次のコードを追加します。

```csharp
// Opt-in to using WkWebView instead of UIWebView.
[assembly: ExportRenderer(typeof(WebView), typeof(Xamarin.Forms.Platform.iOS.WkWebViewRenderer))]
```

> [!NOTE]
> IOS では、`WkWebViewRenderer` には `WkWebViewConfiguration` 引数を受け取るコンストラクターオーバーロードがあります。 これにより、作成時にレンダラーを構成できます。

既定では、Android の `WebView` は、組み込みのブラウザーほど高速です。

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view)では、Microsoft Edge レンダリングエンジンを使用します。 デスクトップデバイスとタブレットデバイスでは、Edge ブラウザー自体を使用した場合と同じパフォーマンスが表示されます。

## <a name="permissions"></a>アクセス許可

@No__t_0 を機能させるには、プラットフォームごとにアクセス許可が設定されていることを確認する必要があります。 一部のプラットフォームでは、`WebView` はデバッグモードで機能しますが、リリース用にビルドされる場合は動作しないことに注意してください。 これは、Android でのインターネットアクセスのような一部のアクセス許可は、既定でデバッグモードのときに Visual Studio for Mac 設定されているためです。

- **UWP** &ndash; には、ネットワークコンテンツを表示するときにインターネット (クライアント & サーバー) 機能が必要です。
- **Android** &ndash; では、ネットワークからコンテンツを表示する場合にのみ `INTERNET` が必要です。 ローカルコンテンツには特別なアクセス許可は必要ありません。
- **iOS** &ndash; には特別なアクセス許可は必要ありません。

## <a name="layout"></a>レイアウト

他の多くの Xamarin 形式のビューとは異なり、`WebView` では、StackLayout または RelativeLayout に含まれるときに `HeightRequest` と `WidthRequest` を指定する必要があります。 これらのプロパティを指定しなかった場合、`WebView` は表示されません。

次の例では、`WebView`s をレンダリングするためのレイアウトを示します。

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

[@No__t_1](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*)メソッドは、引数として指定された JavaScript を評価し、すべての結果を `string` として返します。 この例では、`factorial` JavaScript 関数が呼び出され、結果として `number` の階乗が返されます。 この JavaScript 関数は、 [`WebView`](xref:Xamarin.Forms.WebView)によって読み込まれるローカルの HTML ファイルで定義されています。次の例を参照してください。

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

## <a name="related-links"></a>関連リンク

- [WebView の操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
