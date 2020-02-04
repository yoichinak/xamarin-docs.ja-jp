---
title: Xamarin. iOS の Web ビュー
description: このドキュメントでは、Xamarin iOS アプリで web コンテンツを表示するさまざまな方法について説明します。 WKWebView、SFSafariViewController、Safari、およびアプリトランスポートのセキュリティについて説明します。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 8640800717a88e800503e93c339eeb080707374e
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725432"
---
# <a name="web-views-in-xamarinios"></a>Xamarin. iOS の Web ビュー

IOS Apple の有効期間中は、アプリ開発者がアプリに web ビュー機能を組み込むためのさまざまな方法がリリースされました。 ほとんどのユーザーは iOS デバイスで組み込みの Safari web ブラウザーを利用しているため、他のアプリの web ビュー機能はこのエクスペリエンスと同じであることが期待されます。 同じジェスチャが動作すること、パフォーマンスが同等であること、および機能が同じであることが期待されます。

iOS 11 では、`WKWebView` と `SFSafariViewController`の両方に新しい変更が導入されました。 これらの詳細については、「 [iOS 11 の Web 変更ガイド](~/ios/platform/introduction-to-ios11/web.md)」ガイドを参照してください。

## <a name="wkwebview"></a>WKWebView

`WKWebView` は、iOS 8 で導入されました。これにより、アプリ開発者は、mobile Safari のような web 閲覧インターフェイスを実装できます。 これは、`WKWebView`、mobile Safari で使用されるのと同じエンジンである Nitro Javascript エンジンを使用していることが原因です。 `WKWebView` は、パフォーマンスの向上、ユーザーにとって便利なジェスチャの構築、web ページとアプリ間の相互作用の容易さによって、可能な限り UIWebView で常に使用する必要があります。

`WKWebView` は、UIWebView とほぼ同じ方法でアプリに追加できます。ただし、開発者は UI/UX と機能をはるかに細かく制御できます。 Web ビューオブジェクトを作成して表示すると、要求されたページが表示されます。ただし、ビューの表示方法、ユーザーが移動する方法、およびユーザーがビューを終了する方法を制御できます。  

次のコードを使用して、Xamarin. iOS アプリで `WKWebView` を起動できます。

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

`WKWebView` が `WebKit` 名前空間にあることに注意することが重要です。そのため、この using ディレクティブをクラスの先頭に追加する必要があります。

`WKWebView` は、Xamarin. Mac アプリでも使用できます。クロスプラットフォームの Mac/iOS アプリを作成する場合は、このアプリを使用する必要があります。

「 [Javascript アラートを処理](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)する」レシピでは、javascript での WKWebView の使用に関する情報も提供します。

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

[![SFSafariViewController を使用した web ビューの例の](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

次のコードを使用して、アプリ内から mobile Safari アプリを開くこともできます。

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

これにより、次の web ビューが生成されます。

[![Safari に表示される web ページを する](webview-images/safari.png)](webview-images/safari.png#lightbox)

一般に、アプリから Safari にユーザーを移動することは、常に避ける必要があります。 ほとんどのユーザーはアプリケーションの外部でのナビゲーションを想定していません。そのため、アプリから移動すると、ユーザーはそれを返すことがなく、実質的にはエンゲージメントを終了する可能性があります。

iOS 9 の機能強化により、Safari ページの左上隅にある [戻る] ボタンを使用して、ユーザーが簡単にアプリに戻ることができるようになりました。

## <a name="app-transport-security"></a>アプリケーション トランスポート セキュリティ

IOS 9 では、アプリトランスポートセキュリティ (または*ATS* ) が Apple によって導入され、すべてのインターネット通信がセキュリティで保護された接続のベストプラクティスに準拠するようになりました。

アプリでの実装方法など、ATS の詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)ガイド」を参照してください。

## <a name="uiwebview-deprecated"></a>UIWebView (非推奨)

> [!IMPORTANT]
> `UIWebView` は非推奨とされます。 2020年4月の時点では、このコントロールを使用するアプリは[App Store には受け入れられず、既存のアプリは2020年12月までに削除する必要があり](https://developer.apple.com/news/?id=12232019b)ます。
>
> [Apple の `UIWebView` ドキュメント](https://developer.apple.com/documentation/uikit/uiwebview)では、アプリで[`WKWebView`](#wkwebview)を使用することを提案します。

> [!IMPORTANT]
> Xamarin.Forms の使用時に `UIWebView` の非推奨の警告 (ITMS-90809) についてリソースを探している場合は、[Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) のドキュメントを参照してください。

`UIWebView` は、アプリに web コンテンツを提供するための Apple の従来の方法です。 IOS 2.0 でリリースされ、8.0 の時点で非推奨とされています。

UIWebView を Xamarin iOS アプリに追加するには、次のコードを使用します。

```csharp
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://docs.microsoft.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

これにより、次の web ビューが生成されます。

[![ScalesPagesToFit の効果を します。](webview-images/webview.png)](webview-images/webview.png#lightbox)

## <a name="related-links"></a>関連リンク

- [Webview (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
