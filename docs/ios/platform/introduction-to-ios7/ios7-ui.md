---
title: iOS 7 ユーザー インターフェイスの概要
description: iOS 7 では、大量のユーザーインターフェイスの変更が導入されています。 この記事では、コントロールの外観と新しいデザインをサポートする Api の両方において、大きな変更点をいくつか取り上げます。
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4731be58c1fadae0bba6768570ecfd181b071dd2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031864"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 ユーザー インターフェイスの概要

_iOS 7 では、大量のユーザーインターフェイスの変更が導入されています。この記事では、コントロールの外観と新しいデザインをサポートする Api の両方において、大きな変更点をいくつか取り上げます。_

iOS 7 では、chrome 上のコンテンツに焦点を当てています。 IOS 7 のユーザーインターフェイス要素は、不要な境界線、ステータスバー、ナビゲーションバーなどの属性を削除することで chrome を強調します。これにより、コンテンツビューで使用される画面領域の量が減ります。 IOS 7 では、コンテンツは画面全体を使用するように設計されています。

iOS 7 では、他にもいくつかの変更が導入されています。 color は、ボタンの境界線などの属性の代わりに、ユーザーインターフェイス要素を区別するために使用されます。 ナビゲーションバーやステータスバーなどの多くの要素は、ぼやけて半透明または透明になっています。コンテンツビューでは、その下に領域があります。 これらのコンテンツビューは、ユーザーインターフェイスの奥行を伝える、ぼやけたバーに表示されます。

この記事では、iOS 7 のユーザーインターフェイス要素に加えられたいくつかの変更と、新しいユーザーインターフェイス設計に関連するさまざまな Api について説明します。

## <a name="view-and-control-changes"></a>変更の表示と制御

UIKit のすべてのビューは、iOS 7 の新しいルックアンドフィールに準拠しています。 このセクションでは、これらのビューに加えられた変更の一部と、新しい UI をサポートするように変更された関連の Api について説明します。

### <a name="uibutton"></a>UIButton

`UIButton` クラスから作成されたボタンは、次に示すように、既定では非表示になります。

 ![](ios7-ui-images/button.png "Sample UIButton")

`UIButtonType.RoundedRect` スタイルは非推奨とされました。 IOS 7 で使用した場合、`UIButtonType.RoundedRect` によって `UIButtonType.System` が使用されます。これにより、上に示すように、背景または表示されていない既定のボタンスタイルが生成されます。

### <a name="uibarbuttonitem"></a>UIBarButtonItem

`UIButton`と同様に、バーボタンも、次に示す新しい `UIBarButtonItemStyle.Plain` スタイルを既定として表示されます。

 ![](ios7-ui-images/barbuttonplain.png "Sample UIBarButtonItem")

また、`UIBarButtonItemStyle.Bordered` スタイルは非推奨とされます。 IOS 7 で `UIBarButtonItemStyle.Bordered` を設定すると、`UIBarButtonItemStyle.Plain` スタイルが使用されます。

`UIBarButtonItemStyle.Done` スタイルは非推奨とされます。 ただし、次のように太字のテキストスタイルでのみ、境界線なしのボタンが作成されます。

 ![](ios7-ui-images/barbuttondone.png "Sample UIBarButtonItem in the Done style")

### <a name="uialertview"></a>UIAlertView

新しい iOS 7 のルックアンドフィールのスタイル変更に加え、アラートビューではサブビューを使用したカスタマイズがサポートされなくなりました。 `UIAlertView` が `UIView`から継承していても、`UIAlertView` で `AddSubview` を呼び出すと、効果はありません。 次に例を示します。

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

次のように、標準のアラートビューが生成されます。サブビューは無視されます。

 ![](ios7-ui-images/alert.png "Sample UIAlertView")

 注: UIAlertView は、iOS 8 では非推奨となりました。 IOS 8 以降のアラートビューを使用して、[警告コントローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)のレシピを表示します。

### <a name="uisegmentedcontrol"></a>UISegmentedControl

IOS 7 のセグメント化されたコントロールは透明で、着色色をサポートします。 濃淡の色は、テキストと境界線の色に使用されます。 セグメントを選択すると、次に示すように、選択したセグメントを強調表示するために使用する濃淡の色で、背景とテキストの色が入れ替わります。

 ![](ios7-ui-images/segmentedcontrol.png "Sample UISegmentedControl")

また、`UISegmentedControlStyle` は iOS 7 では非推奨となりました。

### <a name="picker-views"></a>ピッカービュー

ピッカービューの API はほとんど変更されていません。ただし、iOS 7 の設計ガイドラインは、前の iOS バージョンと同様に、画面の下部から、またはナビゲーションコントローラーのスタックにプッシュされた新しいコントローラーを使用して、入力ビューとしてではなくインラインで表示する必要があります。 これは、システムカレンダーアプリで見ることができます。

 ![](ios7-ui-images/inlinepicker.png "This can be seen in the system calendar app")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

`UISearchDisplayController.DisplaysSearchBarInNavigationBar` プロパティが true に設定されている場合、ナビゲーションバー内に検索バーが表示されるようになりました。 False に設定した場合-既定では、検索コントローラーが表示されるときにナビゲーションバーが非表示になります。

次のスクリーンショットは、`UISearchDisplayController`内の検索バーを示しています。

 ![](ios7-ui-images/searchbar.png "Sample UISearchDisplayController")

### <a name="uitableview"></a>UITableView

`UITableView` に関する Api は主に変更されていません。ただし、新しいユーザーインターフェイスの設計に合わせてスタイルが大幅に変更されています。 内部ビュー階層も多少異なります。 この変更はほとんどのアプリには影響しませんが、注意すべき点です。

#### <a name="grouped-table-style"></a>グループ化されたテーブルのスタイル

グループ化されたスタイル変更が更新され、次に示すように、コンテンツが画面の端に拡張されました。

 ![](ios7-ui-images/table1.png "Sample Grouped Table Style")

#### <a name="separatorinset"></a>SeparatorInset

`UITableVIewCell.SeparatorInset` プロパティを設定することによって、行の区切り記号をインデントできるようになりました。 たとえば、次のコードは、左端からセルにインデントを設定するために使用されます。

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

これにより、次のようにインデントされたセルを含むテーブルビューが生成されます。

 ![](ios7-ui-images/separatorinset.png "Sample UITableView SeparatorInset")

#### <a name="table-button-styles"></a>テーブルボタンのスタイル

テーブルビューで使用されるさまざまなボタンはすべて変更されています。 次のスクリーンショットは、編集モードのテーブルビューを示しています。

 ![](ios7-ui-images/table2.png "This screenshot presents a table view in editing mode")

### <a name="additional-control-changes"></a>追加の制御変更

他の UIKit コントロールも、スライダー、スイッチ、steppers など、変更されています。 これらの変更は純粋にビジュアルです。 詳細については、「Apple の[iOS 7 UI 遷移ガイド」](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)を参照してください。

## <a name="general-user-interface-changes"></a>一般的なユーザーインターフェイスの変更

UIKit に加えられた変更に加え、iOS 7 では、次のようなさまざまな視覚変更が UI に導入されています。

- 全画面コンテンツ
- バーの外観
- 濃淡の色

<a name="fullscreen" />

### <a name="full-screen-content"></a>全画面コンテンツ

iOS 7 は、アプリケーションで画面全体を利用できるように設計されています。 ビューコントローラーがステータスバーとナビゲーションバーに重なって表示されるようになりました。ステータスバーとナビゲーションバーがある場合は、ステータスバーとナビゲーションバーの下に表示されません。

IOS 7 用にアプリケーションを準備するときに、 *Interface Builder*または*Xamarin iOS Designer*を使用して、サブビューを視覚的に再調整できます。 新しい Api の1つを使用して、全画面表示のコンテンツをプログラムで処理することもできます。 これらの Api は次のように導入されています。

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide と下端の Layoutguide

 `TopLayoutGuide` と `BottomLayoutGuide` は、次の例のように、コンテンツが半透明の `UIKit` バーと重ならないように、ビューの開始位置または終了位置の参照として機能します。

 [![](ios7-ui-images/clipped.png "Sample content not overlapped by a translucent UIKit bar")](ios7-ui-images/clipped.png#lightbox)

これらの Api を使用して、画面の上部または下部からビューの移動距離を計算し、それに応じてコンテンツの配置を調整できます。

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

上に計算した値を使用して、画面の上部から `ImageView`の移動を設定し、イメージ全体を表示できます。

 [![](ios7-ui-images/good2.png "Example ImageViews displacement from the top of the screen")](ios7-ui-images/good2.png#lightbox)

実際のサンプルについては、 [Imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/)を参照してください。

ディスプレイスメント値は、ビューが階層に追加された後に動的に生成されるため、`TopLayoutGuide` を読み取り、`ViewDidLoad` の値を `BottomLayoutGuide` すると、0が返されます。 たとえば、`ViewDidLayoutSubviews`で、ビューが読み込まれた後の値を計算します。

> [!IMPORTANT]
> `TopLayoutGuide` と `BottomLayoutGuide` は、新しいセーフエリアレイアウトを優先するため、iOS 11 では非推奨とされます。 この安全領域の使用は、iOS 11 より前の iOS バージョンと互換性があることが Apple によって示されています。 詳細については、「 [iOS 11 用アプリの更新](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen)」ガイドを参照してください。

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

この API は、バーの透明度に関係なく、ビューの端を全画面表示に拡張する必要があることを指定します。 IOS 7 では、以前の iOS バージョンとは異なり、ナビゲーションバーとツールバーがコントローラーのビューの上に表示されます。これは、同じ領域を占有していない場合です。 IOS 7 Photos アプリケーションは、既定の `UIViewController.EdgesForExtendedLayout` 値 `UIRectEdge.All`を示しています。 この設定により、ビュー内の4つのすべてのエッジがコンテンツと共に塗りつぶされ、重なった全画面効果が作成されます。

 [![](ios7-ui-images/photos.png "Sample EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

イメージをタップするとバーが削除され、イメージが全画面に表示されます。

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout with the bars removed")](ios7-ui-images/photos2.png#lightbox)

全画面コンテンツが既定であるため、iOS 6 用に構成されたアプリケーションには、次のスクリーンショットのように、ビューの一部がクリップされます。

 [![](ios7-ui-images/clipped.png "Apps configured for iOS 6 will have part of the view clipped, as in this screenshot")](ios7-ui-images/clipped.png#lightbox)

`UIViewController.EdgesForExtendedLayout` プロパティを変更すると、この動作が調整されます。 ビューが端にならないように指定できます。そのため、ビューでは、ナビゲーションまたはツールバーによって占有される領域にコンテンツが表示されないようにします (すべての向き)。

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

アプリでは、ビューが再度配置されていることがわかります。そのため、イメージ全体が表示されます。

 [![](ios7-ui-images/good.png "Example with whole image visible")](ios7-ui-images/good.png#lightbox)

`TopLayoutGuide/BottomLayoutGuide` Api と `EdgesForExtendedLayout` Api の効果は似ていますが、異なる目標を設定することを意図しています。 既定値から `EdgesForExtendedLayout` 設定を変更すると、iOS 6 用に設計されたアプリケーションでは、クリップされたビューが修正される場合がありますが、適切な iOS 7 設計では全画面表示を実現し、`TopLayoutGuide` に依存していて、適切に `BottomLayoutGuide` する必要があります。ユーザーのために快適に操作されるようにコンテンツを配置します。

実際のサンプルについては、 [Imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/)を参照してください。

### <a name="status-and-navigation-bars"></a>ステータスバーとナビゲーションバー

ステータスバーとナビゲーションバーは、透明度を使用してレンダリングされます。 ステータスバーは透明になっていますが、ツールバーとナビゲーションバーは半透明で、ユーザーインターフェイスの奥行を伝えるためにぼやけています。 次のスクリーンショットは、このぼかしと透明度を示しています。コレクションビューの青の背景色は、ステータスバーとナビゲーションバーの両方を通じて表示され、薄い青の外観になっています。

 ![](ios7-ui-images/transparent-navbar.png "Sample Status and Navigation Bar blurring")

#### <a name="status-bar-styles"></a>ステータスバーのスタイル

ぼかしと透明度と共に、ステータスバーの前景は明るいまたは暗い (既定値はダーク) のいずれかになります。 ステータスバーのスタイルは、ビューコントローラーから設定できます。 ビューコントローラーは、ステータスバーを非表示にするか表示するかを設定することもできます。

たとえば、次のコードは、ビューコントローラーの `PreferredStatusBarStyle` メソッドをオーバーライドして、ステータスバーが明るい前景を表示するようにします。

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

これにより、ステータスバーが次のように表示されます。

 ![](ios7-ui-images/light-status-bar.png "Sample Status Bar")

ビューコントローラーのコードからステータスバーを非表示にするには、次のように `PrefersStatusBarHidden`をオーバーライドします。

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

これにより、ステータスバーが非表示になります。

 ![](ios7-ui-images/status-bar-hidden.png "Status Bar hidden")

### <a name="tint-color"></a>濃淡の色

ボタンが chrome レステキストとして表示されるようになりました。 テキストの色は、`UIView`の新しい `TintColor` プロパティを使用して制御できます。 `TintColor` を設定すると、ビューを設定するビューのビュー階層全体に色が適用されます。 アプリ全体で `TintColor`を適用するには、`Window`に設定します。 `UIView.TintColorDidChange` メソッドを使用して、濃淡の色が変化するタイミングを検出することもできます。

たとえば、次のスクリーンショットは、ナビゲーションコントローラーのビューの濃淡の色を紫に変更した場合の効果を示しています。

 ![](ios7-ui-images/tint-color.png "Purple tint color on a navigation controllers view")

`RenderingMode` が `UIImageRenderingMode.AlwaysTemplate`に設定されている場合は、イメージに濃淡の色を適用することもできます。

> [!IMPORTANT]
> `UIAppearance`を使用して濃淡の色を設定することはできません。

### <a name="dynamic-type"></a>動的な型

IOS 7 では、ユーザーはシステム設定でテキストのサイズを指定できます。 動的な型の場合、フォントはサイズに関係なく、適切に表示されるように動的に調整されます。 ユーザーによって制御されるサイズに合わせて最適化されたフォントを取得するには、`UIFont.PreferredFontForTextStyle` を使用する必要があります。

## <a name="summary"></a>まとめ

この記事では、iOS 7 のユーザーインターフェイス要素に加えられた変更について説明します。 ここでは、UIKit のビューとコントロールに加えられたいくつかの変更を調べ、ビジュアルの変更と関連する Api の変更の両方を強調表示しています。 最後に、全画面コンテンツ、新しい着色色のサポート、および動的な型を操作するための新しい Api が導入されました。

## <a name="related-links"></a>関連リンク

- [ImageViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates)
