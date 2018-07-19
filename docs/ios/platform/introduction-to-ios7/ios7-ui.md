---
title: iOS 7 ユーザー インターフェイスの概要
description: iOS 7 には、多数のユーザー インターフェイスの変更が導入されています。 この記事より大きな変更、コントロールの外観と、新しいデザインをサポートする Api の両方の一部が強調表示されます。
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3f5ea8abd41e718f9ac947c5acb290dffe400ddd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781926"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 ユーザー インターフェイスの概要

_iOS 7 には、多数のユーザー インターフェイスの変更が導入されています。この記事より大きな変更、コントロールの外観と、新しいデザインをサポートする Api の両方の一部が強調表示されます。_

iOS 7 では、chrome 経由でコンテンツについて説明します。 IOS 7 のユーザー インターフェイス要素の強調 chrome 余分な境界線、ステータス バー、およびコンテンツ ビューで使用される画面領域の量を減らすのナビゲーション バーなどの属性を削除することで。 IOS 7 では、コンテンツが画面全体を使用するように設計します。

iOS 7 に他のいくつかの変更が導入されています: 色がボタンの境界線などの属性を使用せずに、ユーザー インターフェイス要素を区別するために使用します。 ナビゲーション バー、ステータス バーなどの多くの要素は、今すぐぼかしたおよび半透明または透過でのコンテンツ ビューを使用して、その下にある領域を取得です。 これらのコンテンツ ビューは、ユーザー インターフェイスの深さの感情を伝えるぼかしたバーを表示します。

この記事では、iOS 7 とさまざまな Api デザインに関連する新しいユーザー インターフェイスのユーザー インターフェイス要素への変更のいくつかについて説明します。

## <a name="view-and-control-changes"></a>表示と管理の変更

UIKit 内のビューのすべては、iOS 7 の新しいルック アンド フィールに準拠しています。 このセクションでは、これらのビューだけでなく、新しい UI をサポートするために変更された関連する Api への変更の一部を強調表示されます。

### <a name="uibutton"></a>UIButton

作成したボタン、`UIButton`クラスは、次のように、ふちなし、既定では、背景がないのようになりました。

 ![](ios7-ui-images/button.png "サンプル UIButton")

`UIButtonType.RoundedRect`スタイルは廃止されました。 IOS 7 で使用する場合`UIButtonType.RoundedRect`なります`UIButtonType.System`使用されているが生成されますなし、バック グラウンドまたは表示エッジは、既定のボタン スタイル上記のようにします。

### <a name="uibarbuttonitem"></a>UIBarButtonItem

ような`UIButton`、ボタン バーもふちなし、既定で、新しい`UIBarButtonItemStyle.Plain`次に示すスタイル。

 ![](ios7-ui-images/barbuttonplain.png "サンプル UIBarButtonItem")

さらに、`UIBarButtonItemStyle.Bordered`スタイルは廃止されました。 設定`UIBarButtonItemStyle.Bordered`iOS 7 が生成されるために、`UIBarButtonItemStyle.Plain`使用されているスタイルを設定します。

`UIBarButtonItemStyle.Done`スタイルは推奨されていません。 ただし、これはも作成ふちなしのボタンをクリックするには、太字のテキスト スタイルを示すようにのみ使用。

 ![](ios7-ui-images/barbuttondone.png "元に戻すスタイルでサンプル UIBarButtonItem")

### <a name="uialertview"></a>UIAlertView

新しい iOS 7 のルック アンド フィールのスタイルの変更、に加えてアラートの表示は不要になったサブビュー経由でのカスタマイズをサポートします。 にもかかわらず`UIAlertView`から継承`UIView`、呼び出し元`AddSubview`上、`UIAlertView`も何も起こりません。 次に例を示します。

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

次のように標準のアラート ビューが無視され、サブビューと結果します。

 ![](ios7-ui-images/alert.png "サンプル UIAlertView")
 
 注意: UIAlertView が 8、iOS で非推奨です。 ビュー、[アラート コント ローラー](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)レシピ iOS 8 以降、アラートの表示を使用します。

### <a name="uisegmentedcontrol"></a>UISegmentedControl

IOS 7 でセグメント化されたコントロールは透過的であり色の濃淡をサポートします。 テキストと境界線の色の濃淡色が使用されます。 セグメントを選択すると、色は、次に示すように、選択したセグメントを強調表示に使用濃淡色でとの間、バック グラウンド、スワップされます。

 ![](ios7-ui-images/segmentedcontrol.png "サンプル UISegmentedControl")

さらに、 `UISegmentedControlStyle` iOS 7 では廃止されています。

### <a name="picker-views"></a>ピッカー ビュー

ピッカー ビューの API は、ほとんど変更されています。ただし、iOS 7 のデザイン ガイドラインにはここで状態の以前の iOS のように、ナビゲーション コント ローラーのスタックにプッシュされますまたは新しいコント ローラーを使用して、画面の下部にあるが、バージョンを使用するように入力ビューのアニメーションからのではなく、インライン ピッカーのビューを表示される必要があります。 これは、システム予定表アプリに表示されることができます。

 ![](ios7-ui-images/inlinepicker.png "これでわかるように、システムの予定表アプリ")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

ナビゲーションには、内部検索バーが表示されていますバーの場合、`UISearchDisplayController.DisplaysSearchBarInNavigationBar`プロパティの設定を true にします。 False - 既定値に設定すると、検索コント ローラーが表示されるときに、ナビゲーション バーが非表示されます。

次のスクリーン ショット内で検索バーを表示する、 `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "サンプル UISearchDisplayController")

### <a name="uitableview"></a>UITableView

に関する Api`UITableView`は、主にそのままです。 ただし、スタイルが変更された大幅に新しいユーザー インターフェイスの設計に準拠するようにします。 階層の内部の表示は少し異なるもできます。 この変更は、ほとんどのアプリに影響はありませんが、すべきことに注意してください。

#### <a name="grouped-table-style"></a>グループ化されたテーブルのスタイル

グループ化されたスタイルが変更されたが、更新、コンテンツ下図のように、画面の端に拡張するようになりました。

 ![](ios7-ui-images/table1.png "サンプルのグループ化されたテーブルのスタイル")

#### <a name="separatorinset"></a>SeparatorInset

行区切り記号に設定してインデントするようになりましたことができます、`UITableVIewCell.SeparatorInset`プロパティです。 たとえば、次のコードは、左端からのセルをインデントする使用は。

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

これにより、次のように、インデントされたセルを持つテーブルのビューで生成します。

 ![](ios7-ui-images/separatorinset.png "サンプル UITableView SeparatorInset")

#### <a name="table-button-styles"></a>テーブルのボタン スタイル

テーブル ビューで使用されるさまざまなボタンがすべて変更します。 次のスクリーン ショットでは、編集モードで表形式ビューが表示されます。

 ![](ios7-ui-images/table2.png "このスクリーン ショットが編集モードで表形式ビューを表示します。")

### <a name="additional-control-changes"></a>その他のコントロールの変更

その他の UIKit コントロールは、スライダー、スイッチのステッパなど同様に、変更されました。 これらの変更は、純粋なビジュアルです。 詳細については、Apple を参照してください[iOS 7 UI 移行ガイド](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)です。

## <a name="general-user-interface-changes"></a>一般的なユーザー インターフェイスの変更

IOS 7 UIKit の変更に加えてには、さまざまな UI を視覚的な変更が導入されていますなど。

-  全画面表示の内容
-  バーの外観
-  色の濃淡

<a name="fullscreen" />

### <a name="full-screen-content"></a>全画面表示の内容

iOS 7 は、アプリケーションが画面全体を活用できるように設計されています。 コント ローラーの表示に表示されます、ステータス バーとナビゲーション バーで重複した状態とナビゲーション バーの下に表示されるとは対照的に存在する場合。

IOS 7 用のアプリケーションを準備するときはサブビューを使用して視覚的に配置し直すことができます*インターフェイス ビルダー*または*Xamarin iOS デザイナー*です。 全画面のコンテンツをプログラムで処理に役立つ新しい Api の 1 つを使用できます。 以下は、これらの Api が導入されました。

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide と BottomLayoutGuide

 `TopLayoutGuide` `BottomLayoutGuide`コンテンツがない、半透明でオーバー ラップされたできるようにする必要があります、ビューの開始または終了するには、場所の参照として使用`UIKit`バーで、次の例に示すようにします。

 [![](ios7-ui-images/clipped.png "半透明 UIKit バーでいないオーバー ラップされたサンプルの内容")](ios7-ui-images/clipped.png#lightbox)

これらの Api は、画面の上下からビューの距離を計算に使用することができ、コンテンツの配置は必要に応じて調整。

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

設定する上で計算される値を使用して当社`ImageView`の画像全体が表示されるよう、画面の上部からの距離。

 [![](ios7-ui-images/good2.png "画面の最上位から例 ImageViews 変位")](ios7-ui-images/good2.png#lightbox)

参照してください、 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates)実際のサンプルです。

ビューを読み取ろうとしたため、階層に追加した後、変位値が動的に生成される`TopLayoutGuide`と`BottomLayoutGuide`値`ViewDidLoad`は 0 を返します。 ビューが読み込まれた後の例については、値の計算、`ViewDidLayoutSubviews`です。

> [!IMPORTANT]
> `TopLayoutGuide` および`BottomLayoutGuide`新しい安全な領域のレイアウトを優先するために iOS 11 では使用されなくなりました。 Apple が表記を安全な領域を使用して iOS 11 より前の iOS のバージョンと互換性のある示します。 詳細については、次を参照してください。、 [11 iOS 用アプリを更新](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen)ガイドです。

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

この API は、ビューのどの端必要がありますを拡張して、全画面表示に関係なくバーの透明度を指定します。 IOS 7 でのナビゲーション バーとツールバーが表示の上に配置 - コント ローラーのビューとは異なり以前の iOS のバージョンでは、同じ領域がありませんでした受け取りません。 IOS 7 写真アプリケーションは、既定値を示しています。`UIViewController.EdgesForExtendedLayout`値、`UIRectEdge.All`です。 この設定は、ビューでは、コンテンツ、4 つのエッジすべてを入力し、重複、全画面表示の効果を作成します。

 [![](ios7-ui-images/photos.png "サンプル EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

画像をタップすると、バーを削除し、イメージ全画面表示を表示します。

 [![](ios7-ui-images/photos2.png "削除バーと EdgesForExtendedLayout")](ios7-ui-images/photos2.png#lightbox)

全画面のコンテンツは、既定値であるために、iOS 6 用に構成されたアプリケーションは、次のスクリーン ショットのようにクリップされるビューの一部が得られます。

 [![](ios7-ui-images/clipped.png "IOS 6 を構成したアプリはこのスクリーン ショットのようにクリップされるビューの一部になります")](ios7-ui-images/clipped.png#lightbox)

変更、`UIViewController.EdgesForExtendedLayout`プロパティを調整します。 この動作をします。 ビューいないいっぱいになる、エッジのため、ビューはナビゲーションまたはツールバー (すべての向き) の位置によって占有される領域でコンテンツの表示を避けることを指定できます。

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

アプリケーション内で会いしましょうビューがもう一度再配置されるため、全体のイメージが表示されます。

 [![](ios7-ui-images/good.png "表示されるイメージ全体の例")](ios7-ui-images/good.png#lightbox)

中の影響に注意してください、`TopLayoutGuide/BottomLayoutGuide`と`EdgesForExtendedLayout`Api が似ているものである別の目標を入力します。 変更、`EdgesForExtendedLayout`既定値からの設定は、6、iOS 用に設計されたアプリケーションでクリップされたビューを解決することが、適切な iOS 7 デザインする必要があります全画面表示の見た目を優先して、全画面表示方法が、証明書利用者を提供`TopLayoutGuide`と`BottomLayoutGuide`がユーザーの位置に置いたりに操作を意図してコンテンツを正しく配置します。

参照してください、 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates)実際のサンプルです。

### <a name="status-and-navigation-bars"></a>状態とナビゲーション バー

透過性は、ステータス バーおよびナビゲーション バーがレンダリングされます。 ステータス バーが、透過的なツールバーとナビゲーション バーは半透明とぼかしたのユーザー インターフェイスの深さの感情を伝えるためにします。 次のスクリーン ショットは、コレクション ビューの青色の背景色が、状態とナビゲーション バー、明るい青色の外観を与えることを示していますこのぼかしと透明度を示しています。

 ![](ios7-ui-images/transparent-navbar.png "ぼかしバーの状態とナビゲーションをサンプルします。")

#### <a name="status-bar-styles"></a>ステータス バーのスタイル

ぼかしと透明度、と共に、ステータス バーの前景色は明るいまたは (暗いいる既定値) の濃いのいずれかを指定できます。 ビューのコント ローラーから、ステータス バーのスタイルを設定できます。 また、ビューのコント ローラーは、ステータス バーを非表示にするか、または表示するかどうかを設定していることもできます。

たとえば、次のコードの上書き、`PreferredStatusBarStyle`ライトの前景色を表示、ステータス バーを作成するビュー コント ローラーのメソッド。

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

これが原因で、ステータス バーに表示される次のよう。

 ![](ios7-ui-images/light-status-bar.png "ステータス バーのサンプル")

ビュー コント ローラーのコードからのステータス バーを非表示にするオーバーライド`PrefersStatusBarHidden`、次のようにします。

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

これには、ステータス バーが非表示にします。

 ![](ios7-ui-images/status-bar-hidden.png "ステータス バーが非表示")

### <a name="tint-color"></a>色の濃淡

ボタンは、chrome なしテキストとして表示されるようになりました。 テキストの色は、new を使用して制御できます`TintColor`プロパティ`UIView`です。 設定、`TintColor`色を設定する方法をビューをビュー全体階層に適用します。 適用する、 `TintColor`、アプリ全体の設定、`Window`です。 使用して、濃淡色が変更されたときを検出することも、`UIView.TintColorDidChange`メソッドです。

たとえば、次のスクリーン ショットはナビゲーション コント ローラーのビューで濃淡の色を変更した場合の効果を示しています。 紫色に。

 ![](ios7-ui-images/tint-color.png "ナビゲーション コント ローラーのビューで、紫色の色")

濃淡の色を場合も画像に適用できる、`RenderingMode`に設定されている`UIImageRenderingMode.AlwaysTemplate`です。

> [!IMPORTANT]
> 使用して、濃淡の色を設定することはできません`UIAppearance`です。


### <a name="dynamic-type"></a>動的な型

IOS 7 では、ユーザーは、システムの設定でテキストのサイズを指定できます。 動的な型では、適切にサイズにかかわらず表示する、フォントが動的に調整します。 `UIFont.PreferredFontForTextStyle` ユーザー制御のサイズの最適なフォントを取得するために使用する必要があります。

## <a name="summary"></a>まとめ

この記事では、iOS 7 のユーザー インターフェイス要素への変更について説明します。 これは、ビューおよび UIKit、ビジュアルの変更の両方を強調表示のコントロールへの変更のいくつかを調べますだけでなく関連 Api に変更します。 最後に、全画面表示コンテンツ、新しい濃淡色サポート、および動的な型を使用する新しい Api が導入されています。

## <a name="related-links"></a>関連リンク

- [ImageViewer (サンプル)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
