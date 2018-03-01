---
title: "IOS 11 で web の変更"
ms.topic: article
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: 224a64b39f9761ca520117a23dfeb7ee08cae719
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="web-changes-in-ios-11"></a>IOS 11 で web の変更

iOS 11 WebKit と SafariServices への変更が含まれています、Safari web ブラウザー – Safari 11.0 – の新しいバージョンが導入されています。 このガイドでは、これらの変更について説明します。

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` web コンテンツを表示するか、アプリからユーザーを認証するためのオプションとして、iOS 9 で導入されました。 詳細については、機能には含まれて、 [Web ビュー](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller)ガイドです。

iOS 11 には、アプリと web の間でシームレスなエクスペリエンスをユーザーに与え、Safari ビュー コント ローラーにスタイルの更新プログラムが導入されました。 たとえば、アドレス バーにここで示しています。 Safari ビュー コント ローラー ミニ ブラウザーではなく、アプリ内のブラウザーの外観の削除。 設定して、アプリの画面の配色と適合する画面の配色をカスタマイズすることも、`preferredBarTintColor`と`PreferredControlTintColor`プロパティ。

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

次のコード スニペットは、次の図に表示される、紫、白、内のバーを表示します。

![SFSafariViewController バー紫と空白で表示されます。](web-images/image1.png)

Safari ビュー コント ローラーに表示される、Dismiss ボタンを設定して変更することも、`DismissButtonStyle`プロパティを`Done`、 `Close`、または`Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![ボタンのテキスト変更の破棄します。](web-images/image2.png)

この値を変更するときに`SFSafariViewController`が表示されます。


Safari ビュー コント ローラー内に表示されるコンテンツ、によっては、メニュー バーをユーザーがスクロール折りたたむしないことを確認する必要があります。 これが有効になって、新しい`BarCollapsedEnabled`プロパティを`false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![無効になっている折りたたみバー](web-images/image3.png)

Apple では、ios 11、Safari ビュー コント ローラーでプライバシーに更新が行わもします。 Safari ビュー コント ローラーのすべてのインスタンス間ではなく、アプリごとに存在のみクッキーおよびローカル記憶域などに、データを参照しています。 これにより、ユーザーの参照アクティビティは、アプリ内でプライベートです。

追加機能などをドラッグし、Url のサポートしドロップのサポート`window.open()`にも追加されている`SFSafariViewController`iOS 11 にします。 詳細については、これらの新機能を見つけることができます[Apple の SFSafariViewController ドキュメント](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor)です。


## <a name="webkit"></a>WebKit

`WKWebView` ユーザーに web コンテンツを表示するための手段として iOS 8 の WebKit の一部として導入されました。 もはるかにカスタマイズ可能である`SFSafariViewController`ナビゲーションとユーザー インターフェイスを作成することができます。

Apple での 3 つの主な機能強化が導入`WKWebView`ios 11。 

- Cookie を管理する機能
- コンテンツのフィルター処理
- カスタム リソースを読み込みします。 

新しいクッキー管理を行う[ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore)クラスを追加し、WKWebView に格納されているすべての cookie を取得し、変更を保存する cookie を観察する cookie を削除することができます。

コンテンツのユーザーが表示されるコンテンツの種類を管理することにより、フィルター処理はセキュリティで保護された、ファミリに対応したことを確認することができ、必要に応じて、ユーザーのグループにのみ使用できます。 これには、実装は、新しい[ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist)クラス、トリガーと JSON での操作のペアを入力します。 これらのトリガーとアクションの詳細については、Apple ので参照できる[コンテンツをブロックしてルール](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html)ガイドです。

iOS 11 をカスタマイズできるようになりました`WKWebView`カスタムのリソースが、web コンテンツの読み込みを使用します。 これはによって実装され、`IWKUrlSchemeHandler`インターフェイスで、Web キットをネイティブではない URL スキームを処理することができます。 このインターフェイスでは、開始、停止メソッドを実装する必要がありますには。

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

ハンドラーが実装されるを使用して、設定、`SetUrlSchemeHandler`プロパティを`WKWebViewConfiguration`です。 次に、カスタム スキームを使用するものの URL を読み込みます。

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

