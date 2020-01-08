---
title: IOS 11 での WebKit と Safari の変更点
description: このドキュメントでは、iOS 11 の WebKit および Safari サービスフレームワークに加えられた変更について説明します。 SFSafariViewController のスタイル更新と WKWebView の新機能について説明します。
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/12/2017
ms.openlocfilehash: c52ac3c0f06d58ab5fff8228ca3bdf722056b5b6
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663448"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>IOS 11 での WebKit と Safari の変更点

iOS 11 では、Safari web ブラウザー (Safari 11.0) の新しいバージョンが導入されています。これには、WebKit と Saf サービスの変更が含まれています。 このガイドでは、これらの変更について説明します。

## <a name="safariservices"></a>Saf サービス

`SFSafariViewController` は、web コンテンツを表示したり、アプリからユーザーを認証したりするためのオプションとして、iOS 9 で導入されました。 機能の詳細については、 [Web ビュー](~/ios/user-interface/controls/webview.md#sfsafariviewcontroller)のガイドを参照してください。

iOS 11 では、Safari ビューコントローラーのスタイルの更新が導入されており、アプリと web の間でシームレスなエクスペリエンスを提供しています。 たとえば、アドレスバーを削除すると、Safari ビューコントローラーは、ミニブラウザーではなく、アプリ内ブラウザーを使用できるようになります。 また、`preferredBarTintColor` と `PreferredControlTintColor` のプロパティを設定して、アプリの配色に合わせて配色をカスタマイズすることもできます。

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

次の図に示すように、次のコードスニペットは、紫色と白でバーを表示します。

![紫と白でレンダリングされる SFSafariViewController バー](web-images/image1.png)

Safari ビューコントローラーに表示される [閉じる] ボタンは、`DismissButtonStyle` プロパティを `Done`、`Close`、`Cancel`のいずれかに設定することによっても変更できます。

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![ボタンのテキストの変更を閉じる](web-images/image2.png)

この値は `SFSafariViewController` が表示されている間に変更できます。

Safari ビューコントローラー内に表示されるコンテンツによっては、ユーザーがスクロールするときにメニューバーが折りたたまれないようにする必要がある場合があります。 これを有効にするには、新しい `BarCollapsedEnabled` プロパティを `false`に設定します。

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![バーの折りたたみが無効です](web-images/image3.png)

Apple では、iOS 11 の Safari ビューコントローラーのプライバシーに関する更新も行っています。 現時点では、cookie やローカルストレージなどのデータの参照は、Safari ビューコントローラーのすべてのインスタンスに対してではなく、アプリごとに存在します。 これにより、ユーザーの閲覧アクティビティがアプリ内でプライベートになります。

Url のドラッグアンドドロップのサポート、`window.open()` のサポートなどの追加機能も、iOS 11 の `SFSafariViewController` に追加されました。 これらの新機能の詳細については、 [Apple の SFSafariViewController のドキュメント](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)を参照してください。

## <a name="webkit"></a>WebKit

`WKWebView` は、ユーザーに web コンテンツを表示する手段として、iOS 8 の WebKit の一部として導入されました。 これは `SFSafariViewController`よりもカスタマイズが非常に簡単であり、独自のナビゲーションとユーザーインターフェイスを作成できます。

Apple では、iOS 11 の `WKWebView` に関して主に3つの機能強化が導入されています。 

- Cookie を管理する機能
- コンテンツのフィルター処理
- カスタムリソースの読み込み

Cookie の管理は、新しい[`WKHttpCookieStore`](https://developer.apple.com/documentation/webkit/wkhttpcookiestore)クラスを介して行われます。これにより、cookie の追加と削除、WKWebView に格納されているすべてのクッキーの取得、およびクッキーストアに対する変更の監視を行うことができます。

コンテンツフィルターを使用すると、ユーザーに表示されるコンテンツの種類を管理できます。これにより、セキュリティで保護された家族が使用できるようになります。また、必要に応じて、選択したユーザーグループのみが利用できます。 これは、新しい[`WKContentRuleList`](https://developer.apple.com/documentation/webkit/wkcontentrulelist)クラスを通じて実装されます。これは、JSON でトリガーとアクションのペアを提供することによって行われます。 これらのトリガーとアクションの詳細については、「Apple の[コンテンツブロッキングルール](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html)ガイド」を参照してください。

iOS 11 では、web コンテンツのカスタムリソース読み込みを使用して `WKWebView` をカスタマイズできるようになりました。 これは `IWKUrlSchemeHandler` インターフェイスを通じて実装されます。これにより、Web キットにネイティブではない URL スキームを処理できます。 このインターフェイスには、次のように実装する必要がある開始および停止メソッドがあります。

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

ハンドラーが実装されたら、それを使用して、`WKWebViewConfiguration`の `SetUrlSchemeHandler` プロパティを設定します。 次に、カスタムスキームを使用するものの URL を読み込みます。

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```
