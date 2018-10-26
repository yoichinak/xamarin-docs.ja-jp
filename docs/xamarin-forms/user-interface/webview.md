---
title: Xamarin.Forms の WebView
description: この記事では、ユーザーに Xamarin.Forms WebView クラスを使用して、ローカルを表示する方法、またはネットワークの web コンテンツおよびドキュメントについて説明します。
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2018
ms.openlocfilehash: 8d68afaf0edf178bba6f18d3071de029e111edee
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118670"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms の WebView

[`WebView`](xref:Xamarin.Forms.WebView) アプリで web および HTML コンテンツを表示するためのビューです。 異なり`OpenUri`、ユーザーをデバイス上の web ブラウザーを受け取ります`WebView`アプリ内の HTML コンテンツを表示します。

![](webview-images/in-app-browser.png "アプリのブラウザーで")

## <a name="content"></a>Content

`WebView` 次の種類のコンテンツをサポートしています。

- HTML と CSS の web サイト&ndash;WebView が HTML & CSS、JavaScript のサポートなどを使用して記述された web サイトを完全にサポートします。
- ドキュメント&ndash;WebView が各プラットフォームで表示可能なドキュメントを表示できないネイティブ コンポーネントを使用して、各プラットフォームで WebView が実装されているためです。 IOS と Android で PDF ファイルが動作することを意味します。
- HTML 文字列&ndash;WebView がメモリから HTML 文字列を表示することができます。
- ローカル ファイル&ndash;WebView がアプリに埋め込まれている上記のコンテンツの種類のいずれかを提示します。

> [!NOTE]
> `WebView` Windows をサポートしません Silverlight、Flash、またはすべての ActiveX コントロール場合でも、これらは、そのプラットフォームで Internet Explorer でサポートされます。

### <a name="websites"></a>Websites

インターネットから web サイトを表示する設定、`WebView`の[ `Source` ](xref:Xamarin.Forms.WebViewSource)プロパティに文字列 URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url が指定されているプロトコルに完全形式でなければなりません (つまりが必要"http://"または先頭に"https://")。

#### <a name="ios-and-ats"></a>iOS および ATS

バージョン 9、以降、iOS は、既定でセキュリティのベスト プラクティスを実装しているサーバーと通信するアプリケーションにのみ許可はします。 値を設定する必要があります`Info.plist`安全でないサーバーとの通信を有効にします。

> [!NOTE]
> アプリケーションでは、安全でない web サイトへの接続が必要とする場合を使用して、例外としてのドメインを入力する必要がありますは常に`NSExceptionDomains`ATS を使用して完全にオフにすることではなく`NSAllowsArbitraryLoads`します。 `NSAllowsArbitraryLoads` 極端な緊急の状況でのみ使用する必要があります。

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
```

ベスト プラクティスのみを信頼済みサイトを使用して、信頼されていないドメインに追加のセキュリティを活用しながら、ATS をバイパスする一部のドメインを有効にすることをお勧めします。 アプリの ATS を無効にするとの安全性の低いメソッドを次に示します。

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

参照してください[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)iOS 9 のこの新機能に関する詳細について。

### <a name="html-strings"></a>HTML 文字列

インスタンスを作成する必要がありますのコードで動的に定義されている HTML 文字列を表示する場合は、 [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Web ビューを表示する HTML 文字列")

上記のコードで`@`リテラル、つまりすべての通常のエスケープ文字は無視されますを文字列として、HTML を示すために使用します。

### <a name="local-html-content"></a>ローカルの HTML コンテンツ

WebView は、HTML、CSS からのコンテンツを表示でき、アプリ内で Javascript が埋め込まれています。 例えば:

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

表示のローカル コンテンツを使用してに、 `WebView`、するなど、他の HTML ファイルを開きに文字列として内容を読み込む必要があります、`Html`のプロパティ、`HtmlWebViewSource`します。 開く、ファイルの詳細については、次を参照してください。[ファイルを扱う](~/xamarin-forms/app-fundamentals/files.md)します。

次のスクリーン ショットは、ローカル コンテンツを表示する各プラットフォームでの結果を表示します。

![](webview-images/local-content.png "ローカル コンテンツを表示する WebView")

最初のページが読み込まれていますが、`WebView`の HTML の出所に関する知識を持たない。 ローカル リソースを参照するページを処理するときに問題です。 ときに発生する可能性がありますの例には、それぞれにその他 ページは、ローカルのページ リンクを別の JavaScript ファイルの使用し、CSS スタイル シートにページが含まれます。  

これを解決するには通知する必要があります、`WebView`ファイル システムにファイルを検索する場所。 設定して実行、`BaseUrl`プロパティを`HtmlWebViewSource`で使用される、`WebView`します。

各オペレーティング システム上のファイル システムが異なるため、確認する必要があります。 各プラットフォームでその URL。 Xamarin.Forms は、`DependencyService`各プラットフォーム上で実行時の依存関係を解決するためです。

使用する、 `DependencyService`、まず、各プラットフォームで実装できるインターフェイスを定義します。

```csharp
public interface IBaseUrl { string Get(); }
```

各プラットフォームで、インターフェイスを実装するまで、アプリを実行できないことに注意してください。 共通のプロジェクトで設定するを忘れないことことを確認、`BaseUrl`を使用して、 `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

各プラットフォーム用のインターフェイスの実装が提供する必要があります。

#### <a name="ios"></a>iOS

Ios では、プロジェクトのルート ディレクトリで web コンテンツを配置する必要がありますまたは**リソース**ディレクトリ ビルド アクションが*BundleResource*以下に示すように、します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](webview-images/ios-vs.png "IOS 上のローカル ファイル")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](webview-images/ios-xs.png "IOS 上のローカル ファイル")

-----

`BaseUrl`メイン バンドルのパスに設定する必要があります。

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Android では、ビルド アクションで、Assets フォルダーに HTML、CSS、およびイメージを配置*AndroidAsset*次のようにします。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](webview-images/android-vs.png "Android 上のローカル ファイル")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](webview-images/android-xs.png "Android 上のローカル ファイル")

-----

Android の場合、`BaseUrl`に設定する必要があります`"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

Android では、ファイル、**資産**フォルダーは、によって公開される現在の Android コンテキストを介してアクセスすることも、`MainActivity.Instance`プロパティ。

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) プロジェクトでは、HTML、CSS、およびイメージ プロジェクトのルートに配置するビルド アクションで*コンテンツ*します。

`BaseUrl`に設定する必要があります`"ms-appx-web:///"`:

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

- **GoForward()** &ndash;場合`CanGoForward`が true の場合、呼び出す`GoForward`[次へ]、アクセスしたページへ移動します。
- **GoBack()** &ndash;場合`CanGoBack`が true の場合、呼び出す`GoBack`を最後にアクセスしたページに移動します。
- **CanGoBack** &ndash; `true`がページに戻る場合`false`ブラウザーが開始 URL にある場合。
- **CanGoForward** &ndash; `true`ユーザーが後方移動し、既ににアクセスしたページに進むことができます。

ページ内で`WebView`マルチタッチ ジェスチャをサポートしていません。 重要なことを確認するコンテンツをモバイルに最適化されたし、ズームがなくても表示されます。

アプリケーション内のリンクを表示するが一般的な`WebView`デバイスのブラウザーではなく。 これらの状況では、通常のナビゲーションを許可すると便利ですが、ユーザーからのバックアップ開始リンク上にあるときに、アプリは、通常のアプリ ビューを返す必要があります。

このシナリオを有効にするのにには、組み込みのナビゲーション メソッドとプロパティを使用します。

ブラウザー ビューのページを作成して開始します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

で、コードの分離。

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

これで完了です。

![](webview-images/in-app-browser.png "Web ビューのナビゲーション ボタン")

## <a name="events"></a>イベント

Web ビューは、状態の変化に対応するための 2 つのイベントを発生させます。

- **移動**&ndash;新しいページを読み込む、WebView の開始時に発生するイベントです。
- **移動**&ndash;イベント ページが読み込まれ、ナビゲーションが停止したときに発生します。

読み込みに長い時間がかかるの web ページを使用する見込みがある場合は、これらのイベントを使用して、状態インジケーターを実装することを検討します。 たとえば、XAML は、次のようになります。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
  <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

2 つのイベントのハンドラー:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

これは、(読み込み)、次の出力が得られます。

![](webview-images/loading-start.png "WebView を移動するイベントの例")

完成した読み込み:

![](webview-images/loading-end.png "WebView は、イベントの例を移動します。")

## <a name="performance"></a>パフォーマンス

ハードウェア アクセラレータのレンダリングや JavaScript のコンパイルと同様に、一般的な web ブラウザーは今すぐテクノロジを採用します。 Ios では、既定では、Xamarin.Forms`WebView`によって実装される、`UIWebView`クラス、およびこれらのテクノロジの多くはこの実装では使用できません。 ただし、アプリケーション オプトインできる iOS を使用する`WkWebView`、Xamarin.Forms を実装するクラスを`WebView`、高速な参照をサポートしています。 これは、次のコードを追加することで実現できます、 **AssemblyInfo.cs** iOS プラットフォーム プロジェクトには、アプリケーションのファイル。

```csharp
// Opt-in to using WkWebView instead of UIWebView.
[assembly: ExportRenderer(typeof(WebView), typeof(Xamarin.Forms.Platform.iOS.WkWebViewRenderer))]
```

`WebView` 既定では Android では、約、組み込みのブラウザーと同じ速度の。

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view)は Microsoft Edge のレンダリング エンジンを使用します。 デスクトップ、タブレット デバイスには、Edge ブラウザー自体を使用する場合と同じパフォーマンスが表示されます。

## <a name="permissions"></a>アクセス許可

順序で`WebView`するのには、各プラットフォーム用のアクセス許可が設定されていることを確認してください。 一部のプラットフォームでなお`WebView`リリース用にビルドされたときではなく、デバッグ モードでは動作します。 Android では、インターネット アクセスと同様の一部のアクセス許可は、デバッグ モードでの Mac の Visual Studio によって既定で設定されているためにです。

- **UWP** &ndash;ネットワーク コンテンツを表示するときに、インターネット (クライアントとサーバー) の機能が必要です。
- **Android** &ndash;必要`INTERNET`ネットワークからコンテンツを表示する場合にのみです。 ローカル コンテンツには、特殊なアクセス許可は不要です。
- **iOS** &ndash;特殊なアクセス許可は必要ありません。

## <a name="layout"></a>レイアウト

その他のほとんどの Xamarin.Forms ビューとは異なり`WebView`いる必要があります`HeightRequest`と`WidthRequest`StackLayout または [相対レイアウト] に含まれるときに指定されます。 これらのプロパティを指定しなかった場合、`WebView`は表示されません。

次の例では、レイアウトを使用して作業の結果をレンダリング`WebView`: %s

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

AbsoluteLayout*せず*WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

グリッド*せず*WidthRequest & HeightRequest します。 グリッドは、要求された高さと幅を指定することを必要としないいくつかのレイアウトの 1 つです。 します。

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

[ `WebView` ](xref:Xamarin.Forms.WebView)を c# から JavaScript 関数を呼び出し、呼び出し元の c# コードを任意の結果を返す機能が含まれています。 これを行うと、 [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*)メソッドは、次の例に示した、 [WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)サンプル。

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*)メソッドは JavaScript は、引数として指定し、すべての結果として返しますが、評価、`string`します。 この例で、`factorial`の階乗を返します JavaScript 関数が呼び出される`number`のためです。 この JavaScript 関数がローカル HTML で定義されているファイルを[ `WebView` ](xref:Xamarin.Forms.WebView)が読み込まれ、次の例に示します。

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

- [WebView (サンプル) の操作](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
