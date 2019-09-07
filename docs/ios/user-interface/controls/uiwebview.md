---
title: Xamarin. iOS の Web ビュー
description: このドキュメントでは、Xamarin iOS アプリで web コンテンツを表示するさまざまな方法について説明します。 ここでは、UIWebView、WKWebView、SFSafariViewController、Safari、および app transport のセキュリティについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: bc97f14066456a07ee7ce62131985194bbe83811
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768369"
---
# <a name="web-views-in-xamarinios"></a>Xamarin. iOS の Web ビュー

IOS Apple の有効期間中は、アプリ開発者がアプリに web ビュー機能を組み込むためのさまざまな方法がリリースされました。 ほとんどのユーザーは iOS デバイスで組み込みの Safari web ブラウザーを利用しているため、他のアプリの web ビュー機能はこのエクスペリエンスと同じであることが期待されます。 同じジェスチャが動作すること、パフォーマンスが同等であること、および機能が同じであることが期待されます。

この記事では`UIWebView`、Apple によって提供される3つの Web ビュー (、 `WKWebview`、 `SFSafariViewController`、類似点と相違点、およびそれらを使用する方法) について説明します。 

iOS 11 では、と`WKWebView` `SFSafariViewController`の両方に新しい変更が導入されました。 これらの詳細については、「 [iOS 11 の Web 変更ガイド](~/ios/platform/introduction-to-ios11/web.md)」ガイドを参照してください。

## <a name="uiwebview"></a>UIWebView

`UIWebView`は、アプリに web コンテンツを提供する従来の方法です。 IOS 2.0 でリリースされ、8.0 の時点で非推奨とされています。

8\.0 より前の iOS バージョンをサポートする予定の場合は、を使用`UIWebView`する必要があります。 パフォーマンスに対して最適化`UIWebView`されるのは代替手段よりも低いため、ユーザーの iOS バージョンを確認することをお勧めします。 8\.0 以上の場合、次のいずれかのオプションを使用すると、より優れたユーザーエクスペリエンスが得られます。

UIWebView を Xamarin iOS アプリに追加するには、次のコードを使用します。

```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/webview.png "ScalesPagesToFit の効果")](uiwebview-images/webview.png#lightbox)

の使用方法`UIWebView`の詳細については、次のレシピを参照してください。

- [Web ページを読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [ローカルコンテンツの読み込み](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Web 以外のドキュメントを読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView`は、iOS 8 で導入されました。これにより、アプリ開発者は、mobile Safari と同様の web 閲覧インターフェイスを実装できます。 これは、Nitro Javascript エンジンを使用すること`WKWebView`によるものです。これは、モバイル Safari で使用されるのと同じエンジンです。 `WKWebView`[パフォーマンスが向上](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview)し、ユーザーにわかりやすいジェスチャが組み込まれており、web ページとアプリの間の対話が容易であることが原因で、uiwebview 経由で常に使用する必要がありました。
  
`WKWebView`は、UIWebView とほぼ同じ方法でアプリに追加できます。ただし、開発者は UI/UX と機能をはるかに細かく制御できます。 Web ビューオブジェクトを作成して表示すると、要求されたページが表示されます。ただし、ビューの表示方法、ユーザーが移動する方法、およびユーザーがビューを終了する方法を制御できます。  

次のコードを使用して、Xamarin `WKWebView` . iOS アプリでを起動できます。

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/wkwebview.png "ScalesPagesToFit のない web ビューの例")](uiwebview-images/wkwebview.png#lightbox)

は WebKit 名前空間にある`WKWebView`ことに注意する必要があるため、この using ディレクティブをクラスの先頭に追加する必要があります。

`WKWebView`は、Xamarin. Mac アプリ内でも使用できます。そのため、クロスプラットフォームの Mac/iOS アプリを作成する場合は、これを使用することを検討してください。

「 [Javascript アラートを処理](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)する」レシピでは、javascript での WKWebView の使用に関する情報も提供します。

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController`は、アプリから web コンテンツを提供するための最新の方法であり、iOS 9 以降で使用できます。 また`UIWebView`は`WKWebView`と`SFSafariViewController`は異なり、はビューコントローラーであるため、他のビューと一緒に使用することはできません。 ビューコントローラーを`SFSafariViewController`表示するのと同じように、新しいビューコントローラーとして表示する必要があります。

 `SFSafariViewController`は、基本的には ' ミニ safari ' であり、アプリに埋め込むことができます。 WKWebView と同様に、同じ Nitro Javascript エンジンを使用しますが、オートフィル、閲覧者、モバイル Safari で cookie とデータを共有する機能など、さまざまな Safari 機能も用意されています。 ユーザーとの`SFSafariViewController`間の対話は、アプリにはアクセスできません。 アプリは、既定の Safari 機能のいずれにもアクセスできません。

また、既定では、 **[完了]** ボタンが実装されています。これにより、ユーザーはアプリに簡単に戻ることができ、ナビゲーションボタンを転送および戻ることができ、ユーザーは web ページのスタック内を移動できます。 また、ユーザーには、予期された web ページ上にあることを安心して提供するアドレスバーを提供します。 アドレスバーでは、ユーザーが url を変更することはできません。 

これらの実装は変更できない`SFSafariViewController`ため、アプリがカスタマイズなしで web ページを表示する必要がある場合は、既定のブラウザーとしてを使用することをお勧めします。

次のコードを使用して、Xamarin `SFSafariViewController` . iOS アプリでを起動できます。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/sfsafariviewcontroller.png "SFSafariViewController を使用した web ビューの例")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内から mobile Safari アプリを開くこともできます。

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

これにより、次の web ビューが生成されます。

[![](uiwebview-images/safari.png "Safari に表示される web ページ")](uiwebview-images/safari.png#lightbox)

一般に、アプリから Safari にユーザーを移動することは、常に避ける必要があります。 ほとんどのユーザーはアプリケーションの外部でのナビゲーションを想定していません。そのため、アプリから移動すると、ユーザーはそれを返すことがなく、実質的にはエンゲージメントを終了する可能性があります。

iOS 9 の機能強化により、Safari ページの左上隅にある [戻る] ボタンを使用して、ユーザーが簡単にアプリに戻ることができるようになりました。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

IOS 9 では、アプリトランスポートセキュリティ (または*ATS* ) が Apple によって導入され、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠するようになりました。

アプリでの実装方法など、ATS の詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)ガイド」を参照してください。

## <a name="related-links"></a>関連リンク

- [Webview (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
