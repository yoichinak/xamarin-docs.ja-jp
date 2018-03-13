---
title: "Web ビュー"
description: "IOS web ビューのオプションの明確化"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 70703797792a2f667a2eb20cbc45d1736e5e6b9d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="web-views"></a>Web ビュー

IOS の有効期間は、Apple はアプリの開発者がアプリケーションに web 表示の機能を組み込む方法をいくつかの数をリリースしました。 ほとんどのユーザーは、iOS デバイスでは、組み込みの Safari web ブラウザーを使用し、その他のアプリからの web ビュー機能がこのエクスペリエンスと一致することを想定します。 同じにする基準、および機能のパフォーマンスを同じジェスチャを享受できます。

この記事の内容について学びます Apple によって提供される 3 つの Web ビューの各: `UIWebView`、 `WKWebview`、および`SFSafariViewController`、それらの類似点と相違点、および使用方法です。 

iOS 11 では、両方に新しい変更が導入されました`WKWebView`と`SFSafariViewController`です。 これらの詳細については、次を参照してください。、 [Web iOS 11 ガイド変更](~/ios/platform/introduction-to-ios11/web.md)ガイドです。

## <a name="uiwebview"></a>UIWebView

`UIWebView` アプリの web コンテンツを提示するための Apple の従来の方法です。 2.0 では、iOS でリリースされ、8.0 の時点では推奨されていません。

IOS のバージョンを 8.0 より前にサポートする場合は、使用する必要がある`UIWebView`です。 原因という事実に`UIWebView`小さいパフォーマンスが最適化されたその他の方法よりもをお勧め、ユーザーの iOS のバージョンを確認する必要があります。 場合、8.0 またはを使用して、上記のオプションのいずれかの説明の下ユーザー エクスペリエンスを向上させるが作成されます。
 
Xamarin.iOS アプリに、UIWebView を追加するには、次のコードを使用します。
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

これには、次の web ビューが生成されます。

[![](uiwebview-images/webview.png "ScalesPagesToFit の効果")](uiwebview-images/webview.png#lightbox)

使用する方法についての`UIWebView`、次のレシピを参照してください。


- [Web ページを読み込む](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [ローカル コンテンツを読み込み](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [以外の Web ドキュメントを読み込み](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` という web 閲覧モバイル Safari のと同様のインターフェイスを実装できるようにするアプリの開発者を iOS 8 で導入されました。 原因で一部をファクトを`WKWebView`Nitro Javascript エンジン、モバイルの Safari で使用されるものと同じエンジンを使用します。 `WKWebView` 常に使用する必要があります over UIWebView されたために可能な[パフォーマンスの向上](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview)、ユーザー フレンドリなジェスチャ、および web ページとアプリ間の相互作用のように簡単に構築します。
  
`WKWebView` 追加できますをアプリに UIWebView、する場合とほぼ同様の方法で開発者として、UI/ユーザー エクスペリエンスと機能をより細かく制御を持っています。 作成して、web ビュー オブジェクトを表示するページが表示されます、要求された、ただし、ビューの表示方法は、方法、ユーザーが移動することができます、およびビューを終了する方法を制御できます。  

次のコードを起動するために使用できます、 `WKWebView` Xamarin.iOS アプリで。

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

これには、次の web ビューが生成されます。

[![](uiwebview-images/wkwebview.png "ScalesPagesToFit せず、例の web ビュー")](uiwebview-images/wkwebview.png#lightbox)

重要な点は`WKWebView`WebKit 名前空間には、クラスの先頭にディレクティブを使用してこれを追加する必要がためです。

`WKWebView` Xamarin.Mac アプリ内で使用することもでき、したがってするクロスプラット フォーム Mac/iOS アプリを作成する場合は、その使用を検討する可能性があります。

[JavaScript アラートの処理](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/)レシピも WKWebView を Javascript の使用方法に関する情報を提供

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` アプリから web コンテンツを提供する最新の方法し、利用可能で、ios 9 以降。 異なり`UIWebView`または`WKWebView`、`SFSafariViewController`ビュー コント ローラーで、他のビューを使用することはできません。 存在する必要があります`SFSafariViewController`新しいビュー コント ローラーと同じ方法で紹介する任意のビュー コント ローラー。
 
 `SFSafariViewController` 本質的には、'ミニ safari' アプリに埋め込むことができます。 WKWebView と同様に、同じ Nitro Javascript エンジンを使用してを使用しますも AutoFill、リーダー、およびモバイルの Safari と cookie とデータを共有する機能などの追加の Safari 機能の範囲を提供します。 ユーザー間の相互作用と`SFSafariViewController`はアプリにアクセスできません。 アプリには、既定の Safari の機能のいずれかへのアクセスはありません。
 
既定では、実装、**完了**アプリに簡単に戻ると転送して、ユーザーが web ページの履歴内を移動できるようにする、ナビゲーション ボタンをバックアップするためにユーザーを許可する、ボタンをクリックします。 さらに、また、ユーザーは、アドレス バーに目的の web ページ上にある安心を与えることにします。 アドレス バーには、url を変更するユーザーはできません。 

これらの実装を変更できないように`SFSafariViewController`は、アプリがカスタマイズすることがなく、web ページを表示する場合は、既定のブラウザーとして使用する場合に適しています。

次のコードを起動するために使用できます、 `SFSafariViewController` Xamarin.iOS アプリで。

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

これには、次の web ビューが生成されます。

[![](uiwebview-images/sfsafariviewcontroller.png "SFSafariViewController 例 web ビュー")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内で、Safari のモバイル アプリからを開くことも。

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

これには、次の web ビューが生成されます。

[![](uiwebview-images/safari.png "Safari で提供される web ページ")](uiwebview-images/safari.png#lightbox)

Safari にアプリからユーザーを移動する一般に常に避けてください。 ほとんどのユーザーは、アプリケーションの外部でナビゲーションが予期しないアプリから移動する場合のユーザー可能性がありますしないを返すよう、エンゲージメントを本質的に強制終了します。

iOS 9 の機能強化では、Safari ページの左上隅で提供される、[戻る] ボタンを使用してアプリを簡単に取得するユーザーを許可します。

## <a name="app-transport-security"></a>アプリのトランスポート セキュリティ

アプリのトランスポート セキュリティ、または*ATS* apple iOS 9 に、すべてのインターネット通信をセキュリティで保護された接続のベスト プラクティスに準拠していることを確認してくださいで導入されました。

ATS の詳細については、アプリでは、それを実装する方法などを参照してください、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ガイドです。

## <a name="related-links"></a>関連リンク

- [Web 表示 (サンプル)](https://developer.xamarin.com/samples/monotouch/WebView/)
