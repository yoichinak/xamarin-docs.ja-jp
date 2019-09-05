---
title: IOS 11 での WebKit と Safari の変更点
description: このドキュメントでは、iOS 11 の WebKit および Safari サービスフレームワークに加えられた変更について説明します。 SFSafariViewController のスタイル更新と WKWebView の新機能について説明します。
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/12/2017
ms.openlocfilehash: b90673559d0b8a3728898b7d8dbc3207bb22520b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280079"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>IOS 11 での WebKit と Safari の変更点

iOS 11 では、Safari web ブラウザー (Safari 11.0) の新しいバージョンが導入されています。これには、WebKit と Saf サービスの変更が含まれています。 このガイドでは、これらの変更について説明します。

## <a name="safariservices"></a>Saf サービス

`SFSafariViewController`は、web コンテンツを表示したり、アプリからユーザーを認証したりするためのオプションとして、iOS 9 で導入されました。 機能の詳細については、 [Web ビュー](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller)のガイドを参照してください。

iOS 11 では、Safari ビューコントローラーのスタイルの更新が導入されており、アプリと web の間でシームレスなエクスペリエンスを提供しています。 たとえば、アドレスバーを削除すると、Safari ビューコントローラーは、ミニブラウザーではなく、アプリ内ブラウザーを使用できるようになります。 また、プロパティ`preferredBarTintColor`と`PreferredControlTintColor`プロパティを設定して、配色をアプリの配色に合わせてカスタマイズすることもできます。

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

次の図に示すように、次のコードスニペットは、紫色と白でバーを表示します。

![紫と白でレンダリングされる SFSafariViewController バー](web-images/image1.png)

Safari ビューコントローラーに表示される [閉じる`DismissButtonStyle` ] ボタンは、プロパティを、 `Done` `Close` `Cancel`、のいずれかに設定することによって変更することもできます。

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![ボタンのテキストの変更を閉じる](web-images/image2.png)

この値は、の表示`SFSafariViewController`中に変更できます。


Safari ビューコントローラー内に表示されるコンテンツによっては、ユーザーがスクロールするときにメニューバーが折りたたまれないようにする必要がある場合があります。 これは、新しい`BarCollapsedEnabled`プロパティをに設定する`false`ことによって有効になります。

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![バーの折りたたみが無効です](web-images/image3.png)

Apple では、iOS 11 の Safari ビューコントローラーのプライバシーに関する更新も行っています。 現時点では、cookie やローカルストレージなどのデータの参照は、Safari ビューコントローラーのすべてのインスタンスに対してではなく、アプリごとに存在します。 これにより、ユーザーの閲覧アクティビティがアプリ内でプライベートになります。

IOS 11 では、の`window.open()` url およびサポートに対するドラッグアンドドロップのサポートなどの追加機能もに`SFSafariViewController`追加されました。 これらの新機能の詳細については、 [Apple の SFSafariViewController のドキュメント](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)を参照してください。


## <a name="webkit"></a>WebKit

`WKWebView`は、ユーザーに web コンテンツを表示する手段として、iOS 8 の WebKit の一部として導入されました。 これは、独自のナビゲーション`SFSafariViewController`とユーザーインターフェイスを作成できるよりもはるかにカスタマイズできます。

Apple では、iOS 11 に`WKWebView`向けて主に3つの機能強化が導入されています。 

- Cookie を管理する機能
- コンテンツのフィルター処理
- カスタムリソースの読み込み。 

Cookie の管理は新しい[`WKHttpCookieStore`](https://developer.apple.com/documentation/webkit/wkhttpcookiestore)クラスによって行われます。これにより、cookie の追加と削除、WKWebView に格納されているすべてのクッキーの取得、およびクッキーストアに対する変更の監視を行うことができます。

コンテンツフィルターを使用すると、ユーザーに表示されるコンテンツの種類を管理できます。これにより、セキュリティで保護された家族が使用できるようになります。また、必要に応じて、選択したユーザーグループのみが利用できます。 これは、新しい[`WKContentRuleList`](https://developer.apple.com/documentation/webkit/wkcontentrulelist)クラスを通じて実装されます。このためには、JSON でトリガーとアクションのペアを指定します。 これらのトリガーとアクションの詳細については、「Apple の[コンテンツブロッキングルール](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html)ガイド」を参照してください。

iOS 11 では、web コンテンツ`WKWebView`のカスタムリソース読み込みを使用してカスタマイズできるようになりました。 これは`IWKUrlSchemeHandler`インターフェイスによって実装されます。これにより、Web Kit にネイティブではない URL スキームを処理できます。 このインターフェイスには、次のように実装する必要がある開始および停止メソッドがあります。

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

ハンドラーが実装されたら、それを使用して`SetUrlSchemeHandler` 、の`WKWebViewConfiguration`プロパティを設定します。 次に、カスタムスキームを使用するものの URL を読み込みます。

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

