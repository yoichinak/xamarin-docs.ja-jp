---
title: iOS 6 の概要
description: このドキュメントは、iOS 6 で導入された機能について説明するガイドにリンクしています。 コレクション ビュー、PassKit、ソーシャル フレームワーク、および StoreKit の変更内容はすべて説明します。
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 25926d82e060b91b007da9c2295b328cb049e8df
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61302263"
---
# <a name="introduction-to-ios-6"></a>iOS 6 の概要

_iOS 6 には、さまざまな Xamarin.iOS 6 を提供するには、アプリを開発するための新しいテクノロジが含まれていますC#開発者。_

[ ![](images/ios6-large.jpg "IOS 6 ロゴ")](images/ios6-large.jpg#lightbox)

開発者で、iOS 6 と Xamarin.iOS 6 を含め、iOS アプリケーションを作成するには、そのターゲット iPhone 5 を最大限豊富な機能があるようになりました。
このドキュメントでは、利用できるより魅力的な新しい機能とこのトピックでは、次の各記事へのリンクの一部を示します。 さらに重要なは、iOS 6 および iPhone 5 の新しい解像度に移行する開発者となるいくつかの変更に当てはめます。


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[コレクション ビューの概要](~/ios/user-interface/controls/uicollectionview.md)

コレクション ビューでは、任意のレイアウトで表示されるコンテンツを許可します。 カスタム レイアウトもサポートしているときにすぐに使えるグリッドのようなレイアウトを簡単に作成できます。 詳細については、次を参照してください。、[コレクション ビューの概要](~/ios/user-interface/controls/uicollectionview.md) [](~/ios/user-interface/controls/uicollectionview.md)ガイド。


## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[PassKit の概要](~/ios/platform/passkit.md)

PassKit framework Passbook アプリで管理されているデジタルのパスとの対話を可能です。 詳細については、次を参照してください。、[渡すキット ガイドの概要](~/ios/platform/passkit.md)します。


##  <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[EventKit の概要](~/ios/platform/eventkit.md)

EventKit フレームワークでは、予定表、予定表イベント、およびカレンダーのデータベースに保存されるアラーム データにアクセスする手段を提供します。 アクセスの予定表、予定表をイベントでしたが使用可能な iOS 4 以降、iOS 6 は今すぐアラーム データへのアクセスを公開します。 詳細については、次を参照してください。、[は](~/ios/platform/eventkit.md) [EventKit 概要](~/ios/platform/eventkit.md)ガイド。


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[ソーシャル フレームワークの概要](~/ios/platform/social-framework.md)

ソーシャル フレームワークは、中国でのユーザーの Twitter および Facebook、ほか SinaWeibo をなどのソーシャル ネットワークと対話するための統一された API を提供します。 詳細については、次を参照してください。、[ソーシャル フレームワークの概要](~/ios/platform/social-framework.md)ガイド。


##  <a name="changes-to-storekitchanges-to-storekitmd"></a>[StoreKit の変更点](changes-to-storekit.md)

Apple がストア キットの 2 つの新しい機能を導入しました。 購入と iTunes または、アプリ内からアプリ ストアのコンテンツをダウンロードし、アプリ内購入のコンテンツ ファイルをホストしています。 詳細については、次を参照してください。、[ストア キットへの変更](changes-to-storekit.md)ガイド。


## <a name="other-changes"></a>その他の変更


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload と ViewDidUnload 非推奨とされます。

`ViewWillUnload`と`ViewDidUnload`メソッドの`UIViewController`iOS 6 では不要になったと呼ばれます。 IOS の以前のバージョンでこれらのメソッドがアプリケーションで使用されてそれぞれビューをアンロードする前に、の状態とクリーンアップ コードを保存するためです。

Visual Studio for Mac が呼び出されるメソッドを作成するなど、`ReleaseDesignerOutlets`からを呼び出すことはし、次に示す`ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

ただし、6、iOS では不要になったを呼び出すために必要な`ReleaseDesignerOutlets`します。   
   
   
   
クリーンアップ コードでは、iOS 6 アプリケーションを使用する必要があります`DidReceiveMemoryWarning`します。 ただし、その呼び出しをコード`Dispose`控えめに使用して、メモリ集中型のオブジェクトに示すように以下に対してのみ。

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

ここでも、呼び出す`Dispose`上記と同じことはほとんどありません必要です。 一般に、ほとんどのアプリケーションが行う必要がありますイベント ハンドラーを削除することです。

状態を保存する場合は、アプリケーションを実行できますで`ViewWillDisappear`と`ViewDidDisappear`の代わりに`ViewWillUnload`します。


### <a name="iphone-5-resolution"></a>iPhone 5 の解決

iPhone 5 デバイスでは、640 x 1136 解像度があります。 以前のバージョンの iOS を対象とするアプリケーションが表示されます letterboxed iPhone 5 を実行すると次に示すよう。

 [![](images/01-letterboxed.png "以前のバージョンの iOS を対象とするアプリケーションを iPhone 5 の実行と letterboxed に表示されます。")](images/01-letterboxed.png#lightbox)

表示されるアプリケーションのために全画面表示している iPhone 5 に追加するだけでという名前のイメージ`Default-568h@2x.png`640 x 1136 の解像度を持ちます。 次のスクリーン ショットは、このイメージが含まれている後に実行されるアプリケーションを示しています。

 [![](images/02-fullscreen.png "このスクリーン ショットは、このイメージが含まれている後に実行されるアプリケーションを示しています。")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar をサブクラス化

IOS 6 で`UINavigationBar`をサブクラス化できます。 これにより、追加のコントロールの外観の`UINavigationBar`します。 たとえば、アプリケーションがサブクラス サブビューを追加、これらのビューをアニメーション化するの境界を変更することができます、`UINavigationBar`します。

次のコードは、サブクラスの例を示します`UINavigationBar`を追加する、 `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

サブクラスを追加する`UINavigationBar`を`UINavigationController`を使用して、`UINavigationController`の型を受け取るコンス トラクター、`UINavigationBar`と`UIToolbar`以下に示すように。

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

これを使用して`UINavigationBar`サブクラスの結果の次のスクリーン ショットに示すように表示されるイメージの表示。

 [![](images/03-navbar.png "このスクリーン ショットで示すように表示されているイメージ ビューでこの UINavigationBar サブクラスの結果の使用")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>インターフェイスの向き

IOS の前に、6 アプリケーションをオーバーライドできます`ShouldAutorotateToInterfaceOrientation`、特定のコント ローラーがサポートされている任意の向きについて true を返します。 たとえば、次のコードを使用して、縦向きのみをサポートするためには。

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

IOS 6 で`ShouldAutorotateToInterfaceOrientation`は非推奨とされます。
代わりにアプリケーションを上書きできます`GetSupportedInterfaceOrientations`ルート ビュー コント ローラー次に示すようにします。

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

IPad 上の既定値は 4 つすべての向き場合`GetSupportedInterfaceOrientation`は実装されていません。 IPhone と iPod Touch で、既定ではすべての向きを除く`PortraitUpsideDown`します。
