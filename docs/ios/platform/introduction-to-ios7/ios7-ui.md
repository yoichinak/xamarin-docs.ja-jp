---
title: iOS 7 ユーザー インターフェイスの概要
description: iOS 7 では、多くのユーザー インターフェイスの変更について説明します。 この記事では、いくつかのコントロールの外観と新しいデザインをサポートする Api の両方は、大規模な変更の強調表示されます。
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 132265c27e1d1ba3b8f3fc8db10d7b3cfa746197
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109017"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 ユーザー インターフェイスの概要

_iOS 7 では、多くのユーザー インターフェイスの変更について説明します。この記事では、いくつかのコントロールの外観と新しいデザインをサポートする Api の両方は、大規模な変更の強調表示されます。_

iOS 7 では、chrome 経由でコンテンツについて説明します。 IOS 7 のユーザー インターフェイス要素の強調 chrome 余分な枠線、ステータス バー、およびコンテンツ ビューで使用される画面領域の量を減らすのナビゲーション バーなどの属性を削除することで。 Ios 7 で全画面を使用するコンテンツが設計されています。

iOS 7 の他のいくつかの変更が導入されています。 色がボタンの境界線などの属性の代わりのユーザー インターフェイス要素を区別するために使用されます。 ナビゲーション バーやステータス バーなどの多くの要素整いましたぼかしたして半透明または透過的のいずれかのコンテンツ ビューを使用して、その下にある領域を取得します。 これらのコンテンツ ビューは、ユーザー インターフェイスで奥行き感を伝える、ぼかしバーをレンダリングします。

この記事では、iOS 7 とさまざまな Api デザインに関連する、新しいユーザー インターフェイスでのユーザー インターフェイス要素への変更のいくつかについて説明します。

## <a name="view-and-control-changes"></a>ビューとコントロールの変更

すべての UIKit でビューは、iOS 7 の新しいルック アンド フィールに準拠しています。 このセクションには、いくつかの新しい UI をサポートするために変更された関連する Api と同様に、これらのビューへの変更が強調表示されます。

### <a name="uibutton"></a>UIButton

作成したボタン、`UIButton`クラスは、次に示すようボーダーレス、既定では、背景がないのではようになりました。

 ![](ios7-ui-images/button.png "サンプル UIButton")

`UIButtonType.RoundedRect`スタイルが非推奨とされました。 IOS 7 で使用する場合`UIButtonType.RoundedRect`なります`UIButtonType.System`を使用しているが生成されますなし、バック グラウンドまたは表示されているエッジは、既定のボタン スタイルの上に示すようにします。

### <a name="uibarbuttonitem"></a>UIBarButtonItem

ような`UIButton`、ボタン バーがボーダーレス、既定では、新しいも`UIBarButtonItemStyle.Plain`次に示すスタイル。

 ![](ios7-ui-images/barbuttonplain.png "サンプル UIBarButtonItem")

さらに、`UIBarButtonItemStyle.Bordered`スタイルが非推奨とされました。 設定`UIBarButtonItemStyle.Bordered`iOS 7 結果で、`UIBarButtonItemStyle.Plain`使用されているスタイルを設定します。

`UIBarButtonItemStyle.Done`スタイルが非推奨されていません。 ただしはも作成ボーダーレス ボタンに示すように、太字のテキスト スタイルでのみ。

 ![](ios7-ui-images/barbuttondone.png "元に戻すスタイルでサンプル UIBarButtonItem")

### <a name="uialertview"></a>UIAlertView

アラートの表示をに加えて新しい iOS 7 のルック アンド フィールのスタイルの変更には、サブビューを使用してカスタマイズをサポートしません。 場合でも`UIAlertView`継承`UIView`を呼び出すと、`AddSubview`上、`UIAlertView`も何も起こりません。 次に例を示します。

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

これが生成されます標準のアラートの表示が無視され、サブビューを次に示すよう。

 ![](ios7-ui-images/alert.png "サンプルの\"uialertview\"")
 
 注:"uialertview"は、iOS 8 で非推奨でした。 ビュー、[アラート コント ローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)レシピ iOS 8 以降、アラートの表示を使用します。

### <a name="uisegmentedcontrol"></a>UISegmentedControl

IOS 7 でのセグメント化されたコントロールは透過的であり、濃淡の色をサポートします。 テキストと境界線の色の濃淡の色が使用されます。 セグメントを選択すると、色が次に示すように、選択した区分を強調表示に使用する濃淡の色を背景とテキスト間スワップされます。

 ![](ios7-ui-images/segmentedcontrol.png "サンプル UISegmentedControl")

さらに、 `UISegmentedControlStyle` iOS 7 では非推奨とされます。

### <a name="picker-views"></a>ビューの選択

ピッカー ビュー用の API が大きく変更されています。ただし、iOS 7 の設計のガイドラインは、以前の iOS のように、ナビゲーション コント ローラーのスタックにプッシュされますまたは新しいコント ローラーを使用して、画面の下部にあるが、バージョンを使用するように入力ビューからアニメーションを実行するのではなく、ピッカー ビューは、インラインに表示する必要がありますを今すぐ状態します。 これは、システムの予定表アプリで確認できます。

 ![](ios7-ui-images/inlinepicker.png "これは、システムの予定表アプリで確認できます。")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

ナビゲーション内で検索バーが表示されるようになりましたバーの場合に、`UISearchDisplayController.DisplaysSearchBarInNavigationBar`プロパティの設定を true にします。 False - 既定値に設定すると、検索コント ローラーが表示される場合、ナビゲーション バーは表示されません。

次のスクリーン ショット内で検索バー、 `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "サンプル UISearchDisplayController")

### <a name="uitableview"></a>UITableView

に関する Api`UITableView`は、主に変更されません。 新しいユーザー インターフェイスの設計に準拠するように、スタイルが大幅に変更された、します。 内部のビュー階層は、若干異なるもあります。 この変更は、ほとんどのアプリに影響はありませんが、注意すべきことです。

#### <a name="grouped-table-style"></a>グループ化されたテーブルのスタイル

グループ化されたスタイルが変更されたで更新内容を次に示すように、画面の端に拡張するようになりました。

 ![](ios7-ui-images/table1.png "サンプルのグループ化されたテーブルのスタイル")

#### <a name="separatorinset"></a>SeparatorInset

行区切り記号を設定してインデントようになりましたことができます、`UITableVIewCell.SeparatorInset`プロパティ。 たとえば、次のコードは、左端からのセルをインデントする使用は。

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

次に示すインデントされたセルをテーブル ビューが生成されます。

 ![](ios7-ui-images/separatorinset.png "サンプルの UITableView SeparatorInset")

#### <a name="table-button-styles"></a>テーブルのボタンのスタイル

テーブル ビューで使用されるさまざまなボタンがすべて変更します。 次のスクリーン ショットでは、編集モードのテーブル ビューが表示されます。

 ![](ios7-ui-images/table2.png "このスクリーン ショットは、編集モードのテーブル ビューを表示します")

### <a name="additional-control-changes"></a>その他のコントロールの変更

その他の UIKit コントロールは、スライダー、スイッチ ステッパなども変更されました。 これらの変更は、純粋なビジュアルです。 詳細については、Apple を参照してください[iOS 7 UI 移行ガイド](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)します。

## <a name="general-user-interface-changes"></a>一般的なユーザー インターフェイスの変更

IOS 7 UIKit の変更に加えて、さまざまな UI を視覚的な変更が導入されていますなど。

-  全画面表示のコンテンツ
-  バーの外観
-  濃淡の色

<a name="fullscreen" />

### <a name="full-screen-content"></a>全画面表示のコンテンツ

iOS 7 は、アプリケーションの画面全体を活用できるようにする設計されています。 状態とナビゲーション バーの下に表示されるとは対照的に存在する場合、ビュー コント ローラーはこれで、ステータス バーとナビゲーション バーのオーバー ラップ表示されます。

IOS 7 のアプリケーションを準備するときサブビューを使用して視覚的に配置し直すことができます*Interface Builder*または*Xamarin iOS Designer*します。 全画面表示のコンテンツをプログラムで処理に役立つ、新しい Api のいずれかを使用できます。 以下は、これらの Api が導入されました。

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide と BottomLayoutGuide

 `TopLayoutGuide` `BottomLayoutGuide` 、半透明でコンテンツが重複しないようにする必要があります、ビューの開始または終了するには、場所の参照として使用`UIKit`バーで、次の例のように。

 [![](ios7-ui-images/clipped.png "半透明の UIKit バーによって重複しないコンテンツのサンプル")](ios7-ui-images/clipped.png#lightbox)

これらの Api は、画面の上下からビューの距離を計算に使用することができ、コンテンツ配置を調整します。

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

設定を上記で計算された値を使用できます、`ImageView`の画像全体が表示されるよう、画面の上部からの距離。

 [![](ios7-ui-images/good2.png "画面の上部から ImageViews 変位の例")](ios7-ui-images/good2.png#lightbox)

参照してください、 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates)実際のサンプルです。

ビューを読み取ろうとしたため、階層に追加した後、変位値が動的に生成される`TopLayoutGuide`と`BottomLayoutGuide`値`ViewDidLoad`は 0 を返します。 ビューが読み込まれた後の例については、値の計算、`ViewDidLayoutSubviews`します。

> [!IMPORTANT]
> `TopLayoutGuide` `BottomLayoutGuide`新しい安全領域レイアウトを優先して、iOS 11 で非推奨とされます。 Apple が安全な領域を使用してを iOS 11 より前の iOS バージョンと互換性があると述べています。 詳細については、次を参照してください。、 [iOS 11 のアプリの更新](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen)ガイド。

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

この API は、ビューのどの外枠を拡張する、全画面表示に関係なくバーの透明度を指定します。 IOS 7 でのナビゲーション バーとツールバーが表示の上に配置、コント ローラーのビュー - とは異なり以前の iOS のバージョンでは、同じ領域を行うしなかった場所。 IOS 7 写真のアプリケーションは、既定値を示しています。`UIViewController.EdgesForExtendedLayout`値、`UIRectEdge.All`します。 この設定は、重複している全画面表示の効果を作成、コンテンツ ビューですべての 4 辺を入力します。

 [![](ios7-ui-images/photos.png "サンプル EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

画像をタップして、バーを削除し、イメージの全画面を示しています。

 [![](ios7-ui-images/photos2.png "削除のバーと EdgesForExtendedLayout")](ios7-ui-images/photos2.png#lightbox)

全画面表示のコンテンツは、既定値であるために、iOS 6 用に構成されたアプリケーションは、次のスクリーン ショットのように、クリップのビューの一部が完成します。

 [![](ios7-ui-images/clipped.png "IOS 6 用に構成されたアプリは、ビューのスクリーン ショットのように、クリップの一部になります")](ios7-ui-images/clipped.png#lightbox)

変更、`UIViewController.EdgesForExtendedLayout`プロパティは、この動作を調整します。 ビューいっぱいにならないすべてのエッジでは、ナビゲーションまたは (向き) でツールバーが占有する領域のコンテンツを表示するビューを回避するように指定できます。

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

当社のアプリで思いますビューがもう一度再配置される画像全体が表示されるよう。

 [![](ios7-ui-images/good.png "表示されるイメージ全体の例")](ios7-ui-images/good.png#lightbox)

中の影響に注意してください、`TopLayoutGuide/BottomLayoutGuide`と`EdgesForExtendedLayout`Api に似ていますが、本来は異なる目標を入力します。 変更、`EdgesForExtendedLayout`から既定の設定は、6、iOS 用に設計されたアプリケーションでクリップされたビューを解決できますが、適切な iOS 7 の設計する必要があります全画面表示の見た目を優先して、全画面表示エクスペリエンスを表示、証明書利用者に提供`TopLayoutGuide`と`BottomLayoutGuide`がユーザーの位置に置いたりに操作を意図してコンテンツを正しく配置します。

参照してください、 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates)実際のサンプルです。

### <a name="status-and-navigation-bars"></a>状態とナビゲーション バー

ステータス バーとナビゲーション バーは、透明度が表示されます。 ステータス バーはツールバーやナビゲーション バーは半透明とユーザー インターフェイスで奥行き感を伝達するためにぼかし透明にします。 次のスクリーン ショットを示しますこのぼかしや透明度、コレクション ビューの青色の背景色が、状態とナビゲーション バーを薄い青の外観を与えることを示しています。

 ![](ios7-ui-images/transparent-navbar.png "サンプルの状態とナビゲーション バーのぼかし効果")

#### <a name="status-bar-styles"></a>ステータス バーのスタイル

ぼかし効果と透明度、と共に、ステータス バーの前景色は光または暗い (濃いされている既定値) のいずれかを指定できます。 ビュー コント ローラーから、ステータス バーのスタイルを設定できます。 また、ビュー コント ローラーは、ステータス バーを非表示にするか、または表示するかどうかを設定していることもできます。

たとえば、次のコードの上書き、`PreferredStatusBarStyle`して、ステータス バーのライトの前景色を表示するビュー コント ローラーのメソッド。

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

これにより、ステータス バーに表示される以下のとおり。

 ![](ios7-ui-images/light-status-bar.png "ステータス バーのサンプル")

ビュー コント ローラーのコードからのステータス バーを非表示にするにはオーバーライド`PrefersStatusBarHidden`以下に示すようにします。

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

これにより、ステータス バーを非表示になります。

 ![](ios7-ui-images/status-bar-hidden.png "ステータス バーを非表示")

### <a name="tint-color"></a>濃淡の色

ボタンは、chrome のないテキストとして表示されるようになりました。 新しいテキストの色を制御できます`TintColor`プロパティ`UIView`します。 設定、`TintColor`色を設定するビューの全体のビュー階層に適用します。 適用する、 `TintColor`、アプリ全体の設定、`Window`します。 使用して、濃淡の色が変更されたときを検出することも、`UIView.TintColorDidChange`メソッド。

たとえば、次のスクリーン ショットは、ナビゲーション コント ローラーのビューに濃淡の色を変更した効果を示しています。 紫に。

 ![](ios7-ui-images/tint-color.png "ナビゲーション コント ローラーのビューで、紫色の色")

濃淡の色を場合も画像に適用できる、`RenderingMode`に設定されている`UIImageRenderingMode.AlwaysTemplate`します。

> [!IMPORTANT]
> 使用して、濃淡の色を設定することはできません`UIAppearance`します。


### <a name="dynamic-type"></a>動的な型

IOS 7 では、ユーザーは、システムの設定でテキストのサイズを指定できます。 動的な型で、フォントは、サイズに関係なくきれいに動的に調整されます。 `UIFont.PreferredFontForTextStyle` ユーザーによって制御されたサイズの最適化されたフォントを取得するために使用する必要があります。

## <a name="summary"></a>まとめ

この記事では、iOS 7 のユーザー インターフェイス要素への変更について説明します。 ビューと、UIKit、ビジュアルの変更の両方を強調表示のコントロールに加えられた変更のいくつかの検査し、関連 Api への変更します。 最後に、全画面表示のコンテンツ、濃淡の色の新しいサポート、および動的な型を使用する新しい Api が導入されています。

## <a name="related-links"></a>関連リンク

- [ImageViewer (サンプル)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
