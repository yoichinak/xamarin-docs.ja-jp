---
title: Xamarin. iOS の Web ビュー
description: このドキュメントでは、Xamarin iOS アプリで web コンテンツを表示するさまざまな方法について説明します。 ここでは、UIWebView、WKWebView、SFSafariViewController、Safari、および app transport のセキュリティについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 210e59239957d3963a3d3275315a0eac14748ff8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021782"
---
# <a name="web-views-in-xamarinios"></a>Xamarin. iOS の Web ビュー

IOS Apple の有効期間中は、アプリ開発者がアプリに web ビュー機能を組み込むためのさまざまな方法がリリースされました。 ほとんどのユーザーは iOS デバイスで組み込みの Safari web ブラウザーを利用しているため、他のアプリの web ビュー機能はこのエクスペリエンスと同じであることが期待されます。 同じジェスチャが動作すること、パフォーマンスが同等であること、および機能が同じであることが期待されます。

この記事では、Apple によって提供される3つの Web ビュー (`UIWebView`、`WKWebview`、`SFSafariViewController`、その類似点と相違点、およびそれらの使用方法について説明します。 

iOS 11 では、`WKWebView` と `SFSafariViewController`の両方に新しい変更が導入されました。 これらの詳細については、「 [iOS 11 の Web 変更ガイド](~/ios/platform/introduction-to-ios11/web.md)」ガイドを参照してください。

## <a name="uiwebview"></a>UIWebView

`UIWebView` は、アプリに web コンテンツを提供するための Apple の従来の方法です。 IOS 2.0 でリリースされ、8.0 の時点で非推奨とされています。

8\.0 より前のバージョンの iOS をサポートする予定の場合は、`UIWebView`を使用する必要があります。 `UIWebView` は、代替手段よりもパフォーマンスに最適化されていないため、ユーザーの iOS バージョンを確認することをお勧めします。 8\.0 以上の場合、次のいずれかのオプションを使用すると、より優れたユーザーエクスペリエンスが得られます。

UIWebView を Xamarin iOS アプリに追加するには、次のコードを使用します。

```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/webview.png "The effect of ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

`UIWebView`の使用方法の詳細については、次のレシピを参照してください。

- [Web ページを読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [ローカルコンテンツの読み込み](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Web 以外のドキュメントを読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` は、iOS 8 で導入されました。これにより、アプリ開発者は、mobile Safari のような web 閲覧インターフェイスを実装できます。 これは、`WKWebView`、mobile Safari で使用されるのと同じエンジンである Nitro Javascript エンジンを使用していることが原因です。 [パフォーマンスが向上](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview)し、ユーザーにとって使いやすいジェスチャが組み込まれており、web ページとアプリの間の対話が容易であることが原因で、uiwebview で `WKWebView` を常に使用できるようになりました。
  
`WKWebView` は、UIWebView とほぼ同じ方法でアプリに追加できます。ただし、開発者は UI/UX と機能をはるかに細かく制御できます。 Web ビューオブジェクトを作成して表示すると、要求されたページが表示されます。ただし、ビューの表示方法、ユーザーが移動する方法、およびユーザーがビューを終了する方法を制御できます。  

次のコードを使用して、Xamarin. iOS アプリで `WKWebView` を起動できます。

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/wkwebview.png "An example web view without ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

`WKWebView` は WebKit 名前空間にあることに注意してください。したがって、この using ディレクティブをクラスの先頭に追加する必要があります。

`WKWebView` は Xamarin. Mac アプリ内でも使用できます。したがって、クロスプラットフォームの Mac/iOS アプリを作成する場合は、その使用を検討してください。

「 [Javascript アラートを処理](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)する」レシピでは、javascript での WKWebView の使用に関する情報も提供します。

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController` は、アプリから web コンテンツを提供する最新の方法であり、iOS 9 以降で使用できます。 `UIWebView` または `WKWebView`とは異なり、`SFSafariViewController` はビューコントローラーであるため、他のビューと一緒に使用することはできません。 ビューコントローラーを表示するのと同じ方法で、`SFSafariViewController` を新しいビューコントローラーとして表示する必要があります。

 `SFSafariViewController` は、基本的には "ミニ safari" で、アプリに埋め込むことができます。 WKWebView と同様に、同じ Nitro Javascript エンジンを使用しますが、オートフィル、閲覧者、モバイル Safari で cookie とデータを共有する機能など、さまざまな Safari 機能も用意されています。 ユーザーと `SFSafariViewController` の間の対話は、アプリにはアクセスできません。 アプリは、既定の Safari 機能のいずれにもアクセスできません。

また、既定では、 **[完了]** ボタンが実装されています。これにより、ユーザーはアプリに簡単に戻ることができ、ナビゲーションボタンを転送および戻ることができ、ユーザーは web ページのスタック内を移動できます。 また、ユーザーには、予期された web ページ上にあることを安心して提供するアドレスバーを提供します。 アドレスバーでは、ユーザーが url を変更することはできません。 

これらの実装は変更できないため、`SFSafariViewController` は、アプリがカスタマイズなしで web ページを表示する場合に、既定のブラウザーとして使用するのが理想的です。

次のコードを使用して、Xamarin. iOS アプリで `SFSafariViewController` を起動できます。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/sfsafariviewcontroller.png "An example web view with SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内から mobile Safari アプリを開くこともできます。

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/safari.png "A web page presented in Safari")](uiwebview-images/safari.png#lightbox)

一般に、アプリから Safari にユーザーを移動することは、常に避ける必要があります。 ほとんどのユーザーはアプリケーションの外部でのナビゲーションを想定していません。そのため、アプリから移動すると、ユーザーはそれを返すことがなく、実質的にはエンゲージメントを終了する可能性があります。

iOS 9 の機能強化により、Safari ページの左上隅にある [戻る] ボタンを使用して、ユーザーが簡単にアプリに戻ることができるようになりました。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

IOS 9 では、アプリトランスポートセキュリティ (または*ATS* ) が Apple によって導入され、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠するようになりました。

アプリでの実装方法など、ATS の詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)ガイド」を参照してください。

## <a name="related-links"></a>関連リンク

- [Webview (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
