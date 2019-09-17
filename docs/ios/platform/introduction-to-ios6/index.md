---
title: iOS 6 の概要
description: このドキュメントは、iOS 6 で導入された機能について説明しているガイドにリンクしています。 コレクションビュー、Pass Kit、ソーシャルフレームワーク、および StoreKit の変更についてすべて説明します。
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: dc8f2c3a11dd283db46fd0a2530fcca435da75f9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752052"
---
# <a name="introduction-to-ios-6"></a>iOS 6 の概要

_iOS 6 には、アプリを開発するためのさまざまな新しいテクノロジが含まれてC#います。 Xamarin は、開発者を対象としています。_

[![](images/ios6-large.jpg "IOS 6 ロゴ")](images/ios6-large.jpg#lightbox)

IOS 6 と Xamarin. iOS 6 を使用すると、開発者は、iPhone 5 を対象とするアプリを含む iOS アプリケーションを作成するための豊富な機能を利用できるようになりました。
このドキュメントでは、利用可能な新機能をいくつか紹介し、各トピックの記事へのリンクを示します。 さらに、開発者が iOS 6 に移行する際に重要になるいくつかの変更と、iPhone 5 の新しい解決策についても触れています。

## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[コレクションビューの概要](~/ios/user-interface/controls/uicollectionview.md)

コレクションビューでは、任意のレイアウトを使用してコンテンツを表示できます。 カスタムレイアウトをサポートすると同時に、グリッドに似たレイアウトをすぐに作成できます。 詳細については、「[コレクションビューの概要](~/ios/user-interface/controls/uicollectionview.md) [](~/ios/user-interface/controls/uicollectionview.md)」ガイドを参照してください。

## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[PassKit の概要](~/ios/platform/passkit.md)

Pass Kit フレームワークを使用すると、アプリケーションは、Pass Book アプリで管理されているデジタルパスと対話できます。 詳細については、「 [Pass Kit guide (パススルーキットガイド](~/ios/platform/passkit.md))」を参照してください。

## <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[EventKit の概要](~/ios/platform/eventkit.md)

EventKit フレームワークを使用すると、Calendar データベースに格納されているカレンダー、カレンダーイベント、およびアラームデータにアクセスできます。 IOS 4 以降、カレンダーとカレンダーイベントへのアクセスが利用可能になりましたが、iOS 6 ではリマインダーデータへのアクセスが公開されるようになりました。 詳細につい[ては、](~/ios/platform/eventkit.md) 「 [eventkit](~/ios/platform/eventkit.md)の使用」ガイドを参照してください。

## <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[ソーシャルフレームワークの概要](~/ios/platform/social-framework.md)

ソーシャルフレームワークは、Twitter や Facebook などのソーシャルネットワークや中国のユーザー向けの SinaWeibo と対話するための統一された API を提供します。 詳細については、[ソーシャルフレームワークの概要に](~/ios/platform/social-framework.md)関するガイドを参照してください。

## <a name="changes-to-storekitchanges-to-storekitmd"></a>[StoreKit の変更点](changes-to-storekit.md)

Apple では、ストアキットに2つの新機能が導入されています。アプリ内から iTunes または App Store のコンテンツを購入してダウンロードし、アプリ内購入用にコンテンツファイルをホストします。 詳細については、「 [Store Kit に対する変更](changes-to-storekit.md)」ガイドを参照してください。

## <a name="other-changes"></a>その他の変更

### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload と ViewDidUnload は非推奨とされます

の`ViewWillUnload` `ViewDidUnload`メソッドとメソッドは、iOS 6 では呼び出されなくなりました。 `UIViewController` 以前のバージョンの iOS では、これらのメソッドは、ビューがアンロードされる前に状態を保存するためにアプリケーションによって使用されている可能性があります。

たとえば、次のようにという`ReleaseDesignerOutlets`メソッドが作成されます。このメソッドは、から呼び出されます。次に Visual Studio for Mac 例を示します。このメソッドは、から`ViewDidUnload`呼び出されます。

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

ただし、iOS 6 では、を呼び出す`ReleaseDesignerOutlets`必要がなくなりました。   

クリーンアップコードでは、iOS 6 アプリケーションで`DidReceiveMemoryWarning`を使用する必要があります。 ただし、を呼び出す`Dispose`コードは、以下に示すように、メモリを集中的に使用するオブジェクトに対してのみ使用してください。

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

ここでも`Dispose` 、上記のようにを呼び出す必要はほとんどありません。 一般に、ほとんどのアプリケーションでは、イベントハンドラーを削除する必要があります。

状態を保存する場合、アプリケーションはの`ViewWillDisappear` `ViewWillUnload`代わりにと`ViewDidDisappear`を実行できます。

### <a name="iphone-5-resolution"></a>iPhone 5 の解決策

iPhone 5 デバイスには、640x1136 解決策があります。 以前のバージョンの iOS を対象とするアプリケーションは、次に示すように、iPhone 5 で実行すると letterboxed と表示されます。

 [![](images/01-letterboxed.png "以前のバージョンの iOS を対象とするアプリケーションが iPhone 5 で実行されると、letterboxed が表示される")](images/01-letterboxed.png#lightbox)

IPhone 5 でアプリケーションが全画面表示されるようにするには、"640x1136" という名前`Default-568h@2x.png`のイメージを追加するだけです。 次のスクリーンショットは、このイメージを含めた後に実行中のアプリケーションを示しています。

 [![](images/02-fullscreen.png "このスクリーンショットは、このイメージを含めた後に実行中のアプリケーションを示しています。")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar のサブクラス化

IOS 6 `UINavigationBar`では、サブクラス化することができます。 これにより、 `UINavigationBar`のルックアンドフィールをさらに制御できます。 たとえば、アプリケーションでサブビューを追加し、それらのビューをアニメーション化し、の`UINavigationBar`境界を変更することができます。

次のコードは、を`UINavigationBar` `UIImageView`追加するサブクラスの例を示しています。

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

`UINavigationBar`サブクラス`UINavigationController` `UINavigationBar` `UIToolbar`をに追加するには、次に示すように、との型を受け取るコンストラクターを使用します。 `UINavigationController`

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

この`UINavigationBar`サブクラスを使用すると、次のスクリーンショットに示すように、イメージビューが表示されます。

 [![](images/03-navbar.png "この UINavigationBar サブクラスを使用すると、次のスクリーンショットに示すようにイメージビューが表示されます。")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>インターフェイスの向き

IOS 6 アプリケーションより前`ShouldAutorotateToInterfaceOrientation`では、特定のコントローラーがサポートされている方向に対して true を返すことができました。 たとえば、次のコードは、縦方向のみをサポートするために使用されます。

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

IOS 6 `ShouldAutorotateToInterfaceOrientation`のは非推奨とされます。
代わりに、次に`GetSupportedInterfaceOrientations`示すように、アプリケーションはルートビューコントローラーでオーバーライドできます。

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

IPad では、が実装されてい`GetSupportedInterfaceOrientation`ない場合、これは既定で4つの方向になります。 IPhone と iPod Touch の既定値は、を除く`PortraitUpsideDown`すべての向きです。
