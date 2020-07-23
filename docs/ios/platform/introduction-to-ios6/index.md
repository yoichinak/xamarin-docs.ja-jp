---
title: iOS 6 の概要
description: このドキュメントは、iOS 6 で導入された機能について説明しているガイドにリンクしています。 コレクションビュー、Pass Kit、ソーシャルフレームワーク、および StoreKit の変更についてすべて説明します。
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: be78e76e2a52fb6e924fd67e3f0de9e0890ee25b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933433"
---
# <a name="introduction-to-ios-6"></a>iOS 6 の概要

_iOS 6 には、アプリを開発するためのさまざまな新しいテクノロジが含まれています。 Xamarin は、C# 開発者を対象としています。_

[![IOS 6 ロゴ](images/ios6-large.jpg)](images/ios6-large.jpg#lightbox)

IOS 6 と Xamarin. iOS 6 を使用すると、開発者は、iPhone 5 を対象とするアプリを含む iOS アプリケーションを作成するための豊富な機能を利用できるようになりました。
このドキュメントでは、利用可能な新機能をいくつか紹介し、各トピックの記事へのリンクを示します。 さらに、開発者が iOS 6 に移行する際に重要になるいくつかの変更と、iPhone 5 の新しい解決策についても触れています。

## <a name="introduction-to-collection-views"></a>[コレクションビューの概要](~/ios/user-interface/controls/uicollectionview.md)

コレクションビューでは、任意のレイアウトを使用してコンテンツを表示できます。 カスタムレイアウトをサポートすると同時に、グリッドに似たレイアウトをすぐに作成できます。 詳細については、「[コレクションビューの概要](~/ios/user-interface/controls/uicollectionview.md)」ガイドを参照してください [](~/ios/user-interface/controls/uicollectionview.md) 。

## <a name="introduction-to-passkit"></a>[PassKit の概要](~/ios/platform/passkit.md)

Pass Kit フレームワークを使用すると、アプリケーションは、Pass Book アプリで管理されているデジタルパスと対話できます。 詳細については、「 [Pass Kit guide (パススルーキットガイド](~/ios/platform/passkit.md))」を参照してください。

## <a name="introduction-to-eventkit"></a>[EventKit の概要](~/ios/platform/eventkit.md)

EventKit フレームワークを使用すると、Calendar データベースに格納されているカレンダー、カレンダーイベント、およびアラームデータにアクセスできます。 IOS 4 以降、カレンダーとカレンダーイベントへのアクセスが利用可能になりましたが、iOS 6 ではリマインダーデータへのアクセスが公開されるようになりました。 詳細につい[ては、](~/ios/platform/eventkit.md) 「 [eventkit](~/ios/platform/eventkit.md)の使用」ガイドを参照してください。

## <a name="introduction-to-the-social-framework"></a>[ソーシャルフレームワークの概要](~/ios/platform/social-framework.md)

ソーシャルフレームワークは、Twitter や Facebook などのソーシャルネットワークや中国のユーザー向けの SinaWeibo と対話するための統一された API を提供します。 詳細については、[ソーシャルフレームワークの概要に](~/ios/platform/social-framework.md)関するガイドを参照してください。

## <a name="changes-to-storekit"></a>[StoreKit の変更点](changes-to-storekit.md)

Apple では、ストアキットに2つの新機能が導入されています。アプリ内から iTunes または App Store のコンテンツを購入してダウンロードし、アプリ内購入用にコンテンツファイルをホストします。 詳細については、「 [Store Kit に対する変更](changes-to-storekit.md)」ガイドを参照してください。

## <a name="other-changes"></a>その他の変更

### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload と ViewDidUnload は非推奨とされます

`ViewWillUnload` `ViewDidUnload` のメソッドとメソッド `UIViewController` は、iOS 6 では呼び出されなくなりました。 以前のバージョンの iOS では、これらのメソッドは、ビューがアンロードされる前に状態を保存するためにアプリケーションによって使用されている可能性があります。

たとえば、次のようにというメソッドが作成されます。このメソッドは、から呼び出されます。次に Visual Studio for Mac 例を示します。このメソッドは、 `ReleaseDesignerOutlets` から呼び出され `ViewDidUnload` ます。

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

ただし、iOS 6 では、を呼び出す必要がなくなりました `ReleaseDesignerOutlets` 。   

クリーンアップコードでは、iOS 6 アプリケーションでを使用する必要があり `DidReceiveMemoryWarning` ます。 ただし、を呼び出すコードは、 `Dispose` 以下に示すように、メモリを集中的に使用するオブジェクトに対してのみ使用してください。

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

ここでも、上記のようにを呼び出す `Dispose` 必要はほとんどありません。 一般に、ほとんどのアプリケーションでは、イベントハンドラーを削除する必要があります。

状態を保存する場合、アプリケーションはの代わりにとを実行でき `ViewWillDisappear` `ViewDidDisappear` `ViewWillUnload` ます。

### <a name="iphone-5-resolution"></a>iPhone 5 の解決策

iPhone 5 デバイスには、640x1136 解決策があります。 以前のバージョンの iOS を対象とするアプリケーションは、次に示すように、iPhone 5 で実行すると letterboxed と表示されます。

 [![以前のバージョンの iOS を対象とするアプリケーションが iPhone 5 で実行されると、letterboxed が表示される](images/01-letterboxed.png)](images/01-letterboxed.png#lightbox)

IPhone 5 でアプリケーションが全画面表示されるようにするには、"640x1136" という名前のイメージを追加するだけです `Default-568h@2x.png` 。 次のスクリーンショットは、このイメージを含めた後に実行中のアプリケーションを示しています。

 [![このスクリーンショットは、このイメージを含めた後に実行中のアプリケーションを示しています。](images/02-fullscreen.png)](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar のサブクラス化

IOS 6 では `UINavigationBar` 、サブクラス化することができます。 これにより、のルックアンドフィールをさらに制御でき `UINavigationBar` ます。 たとえば、アプリケーションでサブビューを追加し、それらのビューをアニメーション化し、の境界を変更することができ `UINavigationBar` ます。

次のコードは、を追加するサブクラスの例を示してい `UINavigationBar` `UIImageView` ます。

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

サブクラスをに追加するには、 `UINavigationBar` `UINavigationController` `UINavigationController` `UINavigationBar` 次に示すように、との型を受け取るコンストラクターを使用し `UIToolbar` ます。

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

このサブクラスを使用する `UINavigationBar` と、次のスクリーンショットに示すように、イメージビューが表示されます。

 [![この UINavigationBar サブクラスを使用すると、次のスクリーンショットに示すようにイメージビューが表示されます。](images/03-navbar.png)](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>インターフェイスの向き

IOS 6 アプリケーションより前では `ShouldAutorotateToInterfaceOrientation` 、特定のコントローラーがサポートされている方向に対して true を返すことができました。 たとえば、次のコードは、縦方向のみをサポートするために使用されます。

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

IOS 6 の `ShouldAutorotateToInterfaceOrientation` は非推奨とされます。
代わりに、 `GetSupportedInterfaceOrientations` 次に示すように、アプリケーションはルートビューコントローラーでオーバーライドできます。

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

IPad で `GetSupportedInterfaceOrientation` は、が実装されていない場合、これは既定で4つの方向になります。 IPhone と iPod Touch の既定値は、を除くすべての向きです `PortraitUpsideDown` 。
