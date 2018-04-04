---
title: IOS 6 の概要
description: iOS 6 には、さまざまな Xamarin.iOS 6 が c# 開発者を表示するアプリを開発するための新しいテクノロジが含まれています。
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8f3be80ffb8156c24c96b03fda8eac3907ca88bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-6"></a>IOS 6 の概要

_iOS 6 には、さまざまな Xamarin.iOS 6 が c# 開発者を表示するアプリを開発するための新しいテクノロジが含まれています。_

[ ![](images/ios6-large.jpg "IOS 6 ロゴ")](images/ios6-large.jpg#lightbox)

IOS 6 と Xamarin.iOS 6 では、開発者は、自由にそのターゲット iPhone 5 ものなど、iOS アプリケーションを作成する機能豊富なを今すぐがあります。
このドキュメントでは、使用可能なより興味深いの新機能および各トピックの記事へのリンクの一部を一覧表示します。 さらに、いくつかの変更が開発者は、iOS 6 および iPhone 5 の新しい解像度に移動したときに重要になる点に触れるです。


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[コレクション ビューの概要](~/ios/user-interface/controls/uicollectionview.md)

コレクション ビューは、任意のレイアウトを使用して表示するコンテンツを許可します。 これらは、同様のカスタム レイアウトをサポートしながら、ボックス外のグリッドのようなレイアウトを容易に作成できるようにします。 詳細については、次を参照してください。、[コレクション ビューの概要](~/ios/user-interface/controls/uicollectionview.md) [](~/ios/user-interface/controls/uicollectionview.md)ガイドです。


## <a name="introduction-to-pass-kitiosplatformpasskitmd"></a>[キットを渡すの概要](~/ios/platform/passkit.md)

キットを渡すために、フレームワークにより、Passbook アプリで管理されているデジタルのパスと対話するアプリケーションです。 詳細については、次を参照してください。、[渡すキット ガイドの概要](~/ios/platform/passkit.md)です。


##  <a name="introduction-to-event-kitiosplatformeventkitmd"></a>[イベントのキットの概要](~/ios/platform/eventkit.md)

EventKit フレームワークでは、予定表、カレンダー イベント、およびカレンダーのデータベースを格納するアラーム データにアクセスする手段を提供します。 アクセス カレンダーと予定表へのイベントは以来、使用可能な iOS 4 が iOS 6 は今すぐアラーム データへのアクセスを公開します。 詳細については、次を参照してください。、[すれば](~/ios/platform/eventkit.md) [EventKit に ntroduction](~/ios/platform/eventkit.md)ガイドです。


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[ソーシャル フレームワークの概要](~/ios/platform/social-framework.md)

ソーシャル フレームワークでは、Twitter、Facebook、とともに SinaWeibo を中国でのユーザーなどのソーシャル ネットワークと対話するため、統合 API を提供します。 詳細については、次を参照してください。、[ソーシャル Framework に導入](~/ios/platform/social-framework.md)ガイドです。


##  <a name="changes-to-store-kitchanges-to-storekitmd"></a>[キットを格納する変更](changes-to-storekit.md)

Apple がストア キットで 2 つの新しい機能を導入しました。 購入と iTunes や、アプリからアプリ ストアのコンテンツをダウンロードすると、アプリ内購入のコンテンツ ファイルをホストしているです。 詳細については、次を参照してください。、[ストア キット変更](changes-to-storekit.md)ガイドです。


## <a name="other-changes"></a>その他の変更


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload と非推奨 ViewDidUnload

`ViewWillUnload`と`ViewDidUnload`メソッドの`UIViewController`6、iOS で不要になったと呼ばれます。 以前のバージョンの iOS、これらのメソッドがアプリケーションで使用されてそれぞれビューをアンロードする前に、の状態とクリーンアップ コードを保存するためです。

Visual Studio for Mac が呼び出されるメソッドを作成するなど、 `ReleaseDesignerOutlets`、これがから呼び出されますし、次に示す`ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

ただし、6、iOS では不要になった呼び出しに必要な`ReleaseDesignerOutlets`します。   
   
   
   
クリーンアップ コードは、iOS 6 アプリケーションを使用する必要があります`DidReceiveMemoryWarning`です。 ただし、コードを呼び出す`Dispose`慎重に使用し、メモリ集中型オブジェクトとして表示されている下でのみ。

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

もう一度呼び出して`Dispose`上記と同じことはほとんどありません必要です。 一般に、ほとんどのアプリケーションが行う必要がありますはイベント ハンドラーを削除します。

状態を保存中の場合、アプリケーションを実行できますでこの`ViewWillDisappear`と`ViewDidDisappear`の代わりに`ViewWillUnload`です。


### <a name="iphone-5-resolution"></a>iPhone 5 の解決

iPhone 5 デバイスでは、640 x 1136 解像度があります。 IOS の以前のバージョンを対象とするアプリケーションが表示されます letterboxed iPhone 5 で実行すると次のように。

 [![](images/01-letterboxed.png "IOS の以前のバージョンを対象とするアプリケーションが letterboxed の iPhone 5 の実行時に表示されます。")](images/01-letterboxed.png#lightbox)

アプリケーションを表示するために全画面表示している iPhone 5 で単に追加という名前のイメージ`Default-568h@2x.png`640 x 1136 の解像度を持ちます。 次のスクリーン ショットは、このイメージが含まれています後に実行されるアプリケーションを示しています。

 [![](images/02-fullscreen.png "このスクリーン ショットは、このイメージが含まれています後に実行されるアプリケーションを示しています。")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar をサブクラス化

Ios 6`UINavigationBar`サブクラス化されたことができます。 これによりのルック アンド フィールの他の制御、`UINavigationBar`です。 たとえば、アプリケーションがサブクラス サブビューを追加、それらのビューをアニメーション化およびの境界を変更することができます、`UINavigationBar`です。

次のコードは、サブクラスの例を示します`UINavigationBar`を追加、 `UIImageView`:

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

サブクラス化を追加する`UINavigationBar`を`UINavigationController`を使用して、`UINavigationController`の型を受け取るコンス トラクター、`UINavigationBar`と`UIToolbar`、次のように。

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

これを使用して`UINavigationBar`サブクラス結果の次のスクリーン ショットに示すように表示されるイメージの表示にします。

 [![](images/03-navbar.png "このスクリーン ショットに示すように表示されるイメージのビューでこの UINavigationBar サブクラス結果の使用")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>インターフェイスの向き

IOS の前に 6 つのアプリケーションを上書きでした`ShouldAutorotateToInterfaceOrientation`、サポートされている特定のコント ローラーの向きについて true を返します。 たとえば、次のコードを使用して、縦向きのみをサポートするためには。

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

Ios 6`ShouldAutorotateToInterfaceOrientation`は推奨されなくなりました。
代わりにアプリケーションを上書きできます`GetSupportedInterfaceOrientations`ルート ビュー コント ローラーで次のようにします。

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

IPad での既定値は 4 つすべての向き場合`GetSupportedInterfaceOrientation`は実装されていません。 IPhone と iPod Touch では、既定値はすべての向きを除く`PortraitUpsideDown`です。
