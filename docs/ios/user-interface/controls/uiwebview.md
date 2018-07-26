---
title: Xamarin.iOS での web ビュー
description: このドキュメントでは、Xamarin.iOS アプリは web コンテンツを表示できるさまざまな方法について説明します。 UIWebView、WKWebView、SFSafariViewController、Safari、およびアプリのトランスポート セキュリティがについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242041"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.iOS での web ビュー

Apple は iOS の有効期間にわたってさまざまなアプリ開発者がアプリケーションに web ビューの機能を組み込む方法をリリースしました。 ほとんどのユーザーは自分の iOS デバイスでは、組み込みの Safari web ブラウザーを利用し、その他のアプリからの web ビュー機能がこのエクスペリエンスと一致することを想定します。 ために、パフォーマンス基準、および機能で同じである同じジェスチャの想定されます。

この記事で紹介 Apple によって提供される次の 3 つの Web ビューの各: `UIWebView`、 `WKWebview`、および`SFSafariViewController`、これらの類似点と相違点、および使用方法。 

iOS 11 では、両方に新しい変更が導入されました`WKWebView`と`SFSafariViewController`します。 これらの詳細については、次を参照してください。、 [iOS 11 のガイドでの変更を Web](~/ios/platform/introduction-to-ios11/web.md)ガイド。

## <a name="uiwebview"></a>UIWebView

`UIWebView` アプリで web コンテンツを提供するための Apple のレガシ方法です。 2.0 では、iOS でリリースされ、8.0 の時点では推奨されていません。

IOS のバージョンを 8.0 より前サポートを計画して使用する必要があります`UIWebView`します。 原因という事実に`UIWebView`小さいパフォーマンスが最適化されたその他の方法よりも、お勧め、ユーザーの iOS バージョンを確認する必要があります。 場合、8.0、またはを使用して、上記のオプションのいずれかの説明の下優れたユーザー エクスペリエンスが作成されます。
 
UIWebView を Xamarin.iOS アプリに追加するには、次のコードを使用します。
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

これには、次の web ビューが生成されます。

[![](uiwebview-images/webview.png "ScalesPagesToFit の効果")](uiwebview-images/webview.png#lightbox)

使用しての詳細については`UIWebView`、次のレシピを参照してください。


- [Web ページを読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [ローカル コンテンツを読み込み](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [以外の Web ドキュメントを読み込む](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` iOS 8 という web 閲覧モバイル Safari に似たインターフェイスを実装するために許可するアプリの開発者で導入されました。 これは、期限に達すると、ファクトの一部を`WKWebView`Nitro Javascript エンジン、モバイルの Safari で使用されるものと同じエンジンを使用します。 `WKWebView` 常に使用する必要があります over UIWebView されたため、[パフォーマンスが向上](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview)、わかりやすいジェスチャは、および web ページとアプリ間の相互作用の容易さで構築します。
  
`WKWebView` 追加できますをアプリに UIWebView をするのとほぼ同じ方法で、開発者は UI/UX や機能をより細かく制御を持って。 作成して、web ビュー オブジェクトを表示するは、ビューを表示する方法、ユーザーが操作できる方法と、ユーザーが、ビューがどのように終了する方法を制御できますが、要求されたページを表示するされます。  

次のコードを起動するために使用できます、 `WKWebView` Xamarin.iOS アプリで。

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

これには、次の web ビューが生成されます。

[![](uiwebview-images/wkwebview.png "ScalesPagesToFit せず web ビューの例")](uiwebview-images/wkwebview.png#lightbox)

注意することが重要`WKWebView`は WebKit 名前空間でクラスの先頭にディレクティブを使用してこれを追加する必要があるためです。

`WKWebView` Xamarin.Mac のアプリ内でも使用でき、したがってするクロスプラット フォーム対応の Mac と iOS アプリを作成する場合は、その使用を検討することがあります。

[JavaScript アラートの処理](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)レシピ Javascript で WKWebView の使用に関する情報も提供します

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` アプリから web コンテンツを提供する最新の方法ですし、利用可能で、ios 9 以降。 異なり`UIWebView`または`WKWebView`、`SFSafariViewController`ビュー コント ローラーで、他のビューを使用することはできません。 存在する必要があります`SFSafariViewController`新しいビュー コント ローラーと同じ方法で紹介する任意のビュー コント ローラー。
 
 `SFSafariViewController` 基本的に、'ミニ safari' をアプリに埋め込むことができるは。 WKWebView などは、同じの Nitro Javascript エンジンを使用してもオートコンプリート、リーダー、およびモバイルの Safari の cookie とデータを共有する機能などの追加の Safari 機能の範囲を提供します。 ユーザー間の相互作用と`SFSafariViewController`は、アプリにアクセスできません。 アプリでは、既定の Safari 機能のいずれかへのアクセスはありません。
 
また、既定では、実装、**完了**ボタン、ユーザーをアプリに簡単に返すし、転送して、ユーザーが web ページのスタック間を移動できるようにする、ナビゲーション ボタンをバックアップすることができます。 さらに、アドレス バーの予想される web ページ上にある安心を与えることも、ユーザーを提供します。 アドレス バーでは、url を変更するユーザーは許可されません。 

これらの実装を変更できないように`SFSafariViewController`は、アプリをカスタマイズすることがなく web ページを表示する必要がある場合、既定のブラウザーとして使用するに最適です。

次のコードを起動するために使用できます、 `SFSafariViewController` Xamarin.iOS アプリで。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これには、次の web ビューが生成されます。

[![](uiwebview-images/sfsafariviewcontroller.png "SFSafariViewController と web ビューの例")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリのモバイル Safari のアプリを開くこともします。

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

これには、次の web ビューが生成されます。

[![](uiwebview-images/safari.png "Safari で提供される web ページ")](uiwebview-images/safari.png#lightbox)

Safari にアプリからユーザーを移動する一般に常に避けてください。 ほとんどのユーザーは、アプリケーションの外部でナビゲーションが予期しないため、アプリから移動する場合ユーザー可能性がありますしないを返す、基本的に engagement を強制終了します。

iOS 9 の機能強化には、Safari ページの左上隅で提供されている [戻る] ボタンをアプリに簡単に返すにユーザーができるようにします。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

アプリのトランスポート セキュリティ、または*ATS* apple iOS 9 で、すべてのインターネット通信をセキュリティで保護された接続のベスト プラクティスに準拠していることを確認で導入されました。

ATS の詳細については、アプリでは、その実装方法などを参照してください、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ガイド。

## <a name="related-links"></a>関連リンク

- [Webview (サンプル)](https://developer.xamarin.com/samples/monotouch/WebView/)
