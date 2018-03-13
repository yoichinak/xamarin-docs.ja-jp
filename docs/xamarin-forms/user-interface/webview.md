---
title: WebView
description: "ローカルまたはネットワークの web コンテンツ、およびドキュメントを表示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 7a077a3dcc47de8416abb0c51b23dc07fc1f1f12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="webview"></a>WebView

[WebView](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) web と HTML を表示するビューは、アプリのコンテンツ。 異なり`OpenUri`、ユーザー、デバイス上の web ブラウザーを扱う`WebView`アプリ内の HTML コンテンツが表示されます。

このガイドは、次のセクションで構成されます。

- **[コンテンツ](#Content)** &ndash; WebView 埋め込み HTML、web ページ、HTML 文字列など、さまざまなコンテンツ ソースをサポートしています。
- **[ナビゲーション](#Navigation)** &ndash; WebView には、特定のページに移動して戻ってのサポートが含まれています。
- **[イベント](#Events)** &ndash;リッスンし、WebView でユーザーが実行されたアクションに応答します。
- **[パフォーマンス](#Performance)** &ndash;各プラットフォームで WebView のパフォーマンス特性について説明します。
- **[アクセス許可](#Permissions)** &ndash; WebView アプリで動作するように、アクセス許可を設定する方法について説明します。
- **[レイアウト](#Layout)** &ndash; WebView がそれを配置する方法のいくつかの非常に特定の要件です。WebView が正しく表示されるかどうかを確認する方法を学習します。

![](webview-images/in-app-browser.png "アプリのブラウザーで")

## <a name="content"></a>Content

WebView は、次の種類のコンテンツがサポートされています。

- HTML および CSS の web サイト&ndash;WebView が HTML と CSS、JavaScript のサポートなどを使用して作成された web サイトを完全にサポートします。
- ドキュメント&ndash;WebView は各プラットフォームで表示可能なドキュメントを表示できないネイティブ コンポーネントを使用して、各プラットフォームで WebView が実装されたためです。 IOS、Android、Windows Phone ではないで PDF ファイルが機能することを意味します。
- HTML 文字列&ndash;WebView がメモリから HTML 文字列を表示することができます。
- ローカル ファイル&ndash;WebView は上記のコンテンツの種類のいずれかのアプリに埋め込むを提示できます。

> [!NOTE]
> `WebView` Windows および Windows Phone ではサポートしません Silverlight、Flash またはすべての ActiveX コントロールでは、そのプラットフォームで Internet Explorer ではサポートされている場合でも。

### <a name="websites"></a>Websites

表示するには、インターネットから web サイトを設定、`WebView`の[ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/)プロパティを文字列 URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Url は、指定されたプロトコルに完全な形式でなければなりません (つまりが必要"http://"または"https://"に付加される)。

#### <a name="ios-and-ats"></a>iOS および ATS

バージョン 9、iOS のみにより、アプリケーションは既定でセキュリティのベスト プラクティスを実装しているサーバーと通信します。 値を設定する必要があります`Info.plist`安全でないサーバーとの通信を有効にします。

> [!NOTE]
> 使用して、例外として、ドメインを入力する、アプリケーションでは、安全ではないの web サイトへの接続を必要とする場合は、常にする必要があります`NSExceptionDomains`ATS を使用して完全にオフではなく`NSAllowsArbitraryLoads`です。 `NSAllowsArbitraryLoads` 緊急の極端な状況でのみ使用する必要があります。

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

のみを信頼済みサイトを使用して、信頼されていないドメインに追加のセキュリティを利用しながら、ATS をバイパスする一部のドメインを有効にすることをお勧めします。 アプリの ATS の無効化の安全性の低いメソッドを次に示します。

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

参照してください[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)iOS 9 に、この新しい機能についての詳細。

### <a name="html-strings"></a>HTML 文字列

コードで動的に定義されている HTML の文字列を表示する場合のインスタンスを作成する必要があります[ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

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

上記のコードで`@`リテラル、つまり、すべての通常のエスケープ文字は無視されますを文字列として HTML を示すために使用します。

### <a name="local-html-content"></a>ローカルの HTML コンテンツ

WebView を HTML、CSS からコンテンツを表示し、Javascript アプリに埋め込みます。 例:

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

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

上記の CSS で指定したフォント必要がある各プラットフォーム用にカスタマイズするようにすべてのプラットフォームがあるあります。 同じフォントに注意してください。

表示のローカル コンテンツを使用してに、`WebView`と同様に、他の HTML ファイルを開きに文字列として内容をロードする必要があります、`Html`のプロパティ、`HtmlWebViewSource`です。 ファイルを開くの詳細については、次を参照してください。[ファイルで作業](~/xamarin-forms/app-fundamentals/files.md)です。

次のスクリーン ショットは、ローカル コンテンツを表示する各プラットフォームでの結果を表示します。

![](webview-images/local-content.png "WebView ローカル コンテンツを表示します。")

最初のページが読み込まれていますが、 `WebView` HTML が元の情報を持たない。 ローカル リソースを参照するページを処理するときに問題があります。 ときに発生する可能性がありますの例には、別の JavaScript ファイルの各もう 1 つのページは、ローカルのページ リンクを使用したり、CSS スタイル シートにページが含まれます。  

これを解決するには指定する必要があります、`WebView`ファイルシステム上のファイルを検索する場所です。 設定によって、`BaseUrl`プロパティを`HtmlWebViewSource`によって使用される、`WebView`です。

各オペレーティング システム上のファイル システムは異なるため、確認する必要があります。 各プラットフォームでその URL。 Xamarin.Forms を公開、`DependencyService`各プラットフォーム上で実行時の依存関係を解決するためです。

使用する、 `DependencyService`、まず、各プラットフォームで実装できるインターフェイスを定義します。

```csharp
public interface IBaseUrl { string Get(); }
```

各プラットフォームで、インターフェイスを実装するまで、アプリを実行できないことに注意してください。 共通のプロジェクトを設定するように留意することを確認してください、`BaseUrl`を使用して、 `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

各プラットフォーム用のインターフェイスの実装を指定し、必要があります。

#### <a name="ios"></a>iOS

プロジェクトのルート ディレクトリに、iOS に web コンテンツを配置する必要がありますまたは**リソース**ディレクトリ ビルド アクションが*BundleResource*以下に示すように、します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "IOS 上のローカル ファイル")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/ios-xs.png "IOS 上のローカル ファイル")

-----

`BaseUrl`メインのバンドルのパスに設定する必要があります。

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

Android でビルド アクション Assets フォルダーに HTML、CSS、およびイメージを配置*AndroidAsset*以下に示すようにします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Android 上のローカル ファイル")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/android-xs.png "Android 上のローカル ファイル")

-----

Android で、`BaseUrl`に設定する必要があります`"file:///android_asset/"`:

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

Android でのファイル、**資産**フォルダーは、によって公開される現在の Android コンテキストを通じてアクセスすることも、`MainActivity.Instance`プロパティ。

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="windows-phone"></a>Windows Phone

Windows Phone、HTML、CSS、および画像の配置プロジェクトのルートに設定するビルド アクション*コンテンツ*以下に示すようにします。

![](webview-images/windows-vs.png "Windows Phone 上のローカル ファイル")

Windows Phone 上、`BaseUrl`に設定する必要があります`""`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Windows))]
namespace WorkingWithWebview.Windows {
  public class BaseUrl_Windows : IBaseUrl {
    public string Get() {
      return "";
    }
  }
}
```

#### <a name="windows-runtime-and-universal-windows-platform"></a>Windows ランタイムとユニバーサル Windows プラットフォーム

プロジェクトでは Windows ランタイムとユニバーサル Windows プラットフォーム (UWP)、HTML、CSS、およびイメージ ルートに配置するプロジェクトに設定するビルド アクション*コンテンツ*です。

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

WebView には、いくつかのメソッドと使用可能なプロパティ内の移動がサポートされています。

- **GoForward()** &ndash;場合`CanGoForward`が true の場合、呼び出す`GoForward`へ、[次へ] に表示したページに移動します。
- **GoBack()** &ndash;場合`CanGoBack`が true の場合、呼び出す`GoBack`を最後に表示したページに移動します。
- **CanGoBack** &ndash; `true`ページに戻るには場合`false`ブラウザーの開始 URL にある場合。
- **CanGoForward** &ndash; `true`ユーザーに戻っている場合とが既にアクセスしたページに進むことができます。

ページ内で`WebView`マルチタッチ ジェスチャをサポートしていません。 重要であることを確認するコンテンツをモバイル最適化は、ズームがなくても表示されます。

アプリケーション内のリンクを表示するが一般的な`WebView`デバイスのブラウザーではなくです。 ような場合、通常のナビゲーションを許可すると便利ですが、ユーザー ヒットは、開始のリンクになっている間にバックアップ、アプリは、通常のアプリのビューに返す必要があります。

このシナリオを有効にするのにには、組み込みのナビゲーション メソッドおよびプロパティを使用します。

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

この分離コードで。

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

![](webview-images/in-app-browser.png "WebView ナビゲーション ボタン")

## <a name="events"></a>イベント

Web ビューには、状態の変化に対応するための 2 つのイベントを発生させます。

- **移動する** &ndash; WebView 新しいページの読み込みの開始時に発生するイベントです。
- **移動**&ndash;ページが読み込まれ、ナビゲーションが停止したときに発生するイベントです。

読み込みに長い時間がかかるの web ページを使用してを予測している場合は、これらのイベントを使用して、状態インジケーターを実装することを検討します。 例:

この XAML では:

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
        isVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
この 2 つのイベントのハンドラー:

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

これは、結果、次の出力 (読み込み) になります。

![](webview-images/loading-start.png "WebView を移動するイベントの例")

読み込みの終了:

![](webview-images/loading-end.png "WebView 移動イベントの例")

## <a name="performance"></a>パフォーマンス

最新きたようにそれぞれの一般的な web ブラウザーのハードウェア アクセラレーション レンダリングと JavaScript のコンパイルと同様のテクノロジを採用します。 残念ながら、セキュリティ制限によりこれらの機能のほとんどで利用できなかったの iOS equaivalent `WebView`、`UIWebView`です。 Xamarin.Forms`WebView`使用`UIWebView`です。 問題がある場合は、カスタム レンダラーを使用してを記述する必要があります`WKWebView`、高速な参照をサポートします。 なお`WKWebView`は iOS 8 以降でのみサポートされます。

既定では Android で WebView はの組み込みブラウザーと同じ速度で約です。

`WebBrowser`コントロール Windows Phone 8 および Windows Phone 8.1 機能は最新の HTML5 のサポート機能ではないので、多くの場合、パフォーマンスが低下します。 Windows Phone の サイトを表示する方法に注意してください`WebView`です。 Internet Explorer でテストするのに十分ではありません。

## <a name="permissions"></a>アクセス許可

順序で`WebView`するために、各プラットフォーム用のアクセス許可が設定されていることを確認してください。 一部のプラットフォームでなお`WebView`リリース用にビルドされたときではなく、デバッグ モードでは動作します。 Android でのインターネット アクセスの場合と同様に、いくつかのアクセス許可がデバッグ モードのときに Mac を Visual Studio で既定で設定するためです。

- **Windows Phone 8.0** &ndash;が必要です`ID_CAP_WEBBROWSERCOMPONENT`コントロールと`ID_CAP_NETWORKING`インターネット アクセス。
- **Windows Phone 8.1 と UWP** &ndash;ネットワークのコンテンツを表示するときに、インターネット (クライアントとサーバー) の機能が必要です。
- **Android** &ndash;必要`INTERNET`ネットワークからコンテンツを表示する場合にのみです。 ローカル コンテンツには、特殊なアクセス許可は不要です。
- **iOS** &ndash;特殊なアクセス許可は必要ありません。

## <a name="layout"></a>レイアウト

その他のほとんどの Xamarin.Forms ビューとは異なり`WebView`いる必要があります`HeightRequest`と`WidthRequest`は StackLayout または [相対レイアウト] に含まれているときに指定します。 これらのプロパティを指定しない場合、`WebView`は表示されません。

次の例では、レイアウトを使用して作業の結果をレンダリング`WebView`s:

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

グリッド*せず*WidthRequest & HeightRequest です。 グリッドは、要求された高さと幅を指定することが不要ないくつかのレイアウトのいずれかです。 します。

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


## <a name="related-links"></a>関連リンク

- [WebView (サンプル) の操作](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
