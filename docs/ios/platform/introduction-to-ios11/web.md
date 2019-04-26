---
title: iOS 11 における Web の変更点
description: このドキュメントでは、WebKit と iOS 11 で Safari サービス フレームワークに加えられた変更について説明します。 これには SFSafariViewController で更新プログラムと WKWebView の新機能をスタイル設定を操作する方法について説明します。
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/12/2017
ms.openlocfilehash: ba691a6605dcf7e86a76ed13d4c8ef5f0984ff6e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61399738"
---
# <a name="web-changes-in-ios-11"></a>iOS 11 における Web の変更点

iOS 11 では、WebKit と SafariServices への変更が含まれています、Safari web ブラウザー – Safari 11.0 – の新しいバージョンが導入されています。 このガイドでは、これらの変更について説明します。

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` web コンテンツを表示するか、アプリからユーザーを認証するためのオプションとして、iOS 9 で導入されました。 その機能の詳細についてで参照できる、 [Web ビュー](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller)ガイド。

iOS 11 では、アプリと web の間でシームレスなエクスペリエンスをユーザーに与え、Safari ビュー コント ローラー スタイルの更新プログラムを導入されています。 たとえば、アドレス バーにここではミニ ブラウザーではなく、アプリでブラウザーの外観である Safari ビュー コント ローラーの削除。 設定して、アプリの配色でサインイン合わせて配色をカスタマイズすることも、`preferredBarTintColor`と`PreferredControlTintColor`プロパティ。

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

次のコード スニペットは、次の図に表示される、紫と白のバーを表示します。

![紫と白でレンダリング SFSafariViewController バー](web-images/image1.png)

Safari のビュー コント ローラーに表示される閉じるボタンを設定して変更することも、`DismissButtonStyle`プロパティを`Done`、 `Close`、または`Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![ボタン テキストの変更を無視します。](web-images/image2.png)

この値を変更するときに`SFSafariViewController`が表示されます。


Safari のビュー コント ローラー内で表示されるコンテンツによっては、ユーザーがスクロール メニュー バーを縮小しないことを確認する必要があります。 これは、新しい設定で有効になって`BarCollapsedEnabled`プロパティを`false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![無効になっている折りたたみバー](web-images/image3.png)

Apple では、iOS 11 では、Safari ビュー コント ローラーでプライバシーへの更新プログラムが確立もします。 Safari のビュー コント ローラーのすべてのインスタンスではなく、アプリごとの存在のみ cookie、ローカル ストレージなどに、データを参照しています。 これにより、ユーザーの閲覧活動は、アプリ内でプライベートです。

追加の機能やなどのドラッグと Url のサポートをドロップのサポート`window.open()`にも追加されている`SFSafariViewController`iOS 11 でします。 これらの新機能の詳細については見つかります[Apple の SFSafariViewController ドキュメント](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)します。


## <a name="webkit"></a>WebKit

`WKWebView` ユーザーに web コンテンツを表示するための手段として iOS 8 で WebKit の一部として導入されました。 これがよりもはるかにカスタマイズ可能な`SFSafariViewController`、独自のナビゲーションとユーザー インターフェイスを作成することができます。

Apple での 3 つの主な機能強化が導入`WKWebView`iOS 11 で。 

- Cookie を管理する機能
- コンテンツのフィルター処理
- カスタム リソースの読み込み。 

新しいクッキー管理が行われます[ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore)クラスを追加およびの変更を保存する cookie を観察して、WKWebView に格納されているすべてのクッキーを取得するのには、cookie を削除することができます。

コンテンツのフィルター処理、ユーザーが表示されるコンテンツの種類を管理することができますがセキュリティで保護された、ファミリのわかりやすいかどうかを確認することができ、必要に応じて、ユーザーのグループにのみ使用できます。 これには、実装は、新しい[ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist)トリガーおよびアクションの JSON のペアを入力して、クラス。 これらのトリガーとアクションの詳細については、apple の見つかんだできます[コンテンツ ブロック規則](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html)ガイド。

iOS 11 をカスタマイズできるようになりました`WKWebView`カスタム リソースが、web コンテンツの読み込みとします。 これには、実装を通じて、`IWKUrlSchemeHandler`インターフェイスで、Web のキットをネイティブでない URL スキームを処理することができます。 このインターフェイスは、開始、停止メソッドを実装する必要があります。

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

ハンドラーが実装されると、設定を使用、`SetUrlSchemeHandler`プロパティを`WKWebViewConfiguration`します。 次に、カスタムのスキームを使用するものの URL を読み込みます。

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

