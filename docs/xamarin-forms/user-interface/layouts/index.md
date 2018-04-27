---
title: レイアウト
description: 画面上のビューをレイアウトします。
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 864e81b6955fd5138c4055a3f202695803139ac6
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="layouts"></a>レイアウト

Xamarin.Forms では、いくつかのレイアウトと画面上のコンテンツを整理するための機能があります。 

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Xamarin.Forms レイアウトにより、 [Xamarin 大学](https://university.xamarin.com/)**

各レイアウト コントロールは画面の向きの変更を処理する方法の詳細だけでなく、以下について説明します。

* **[StackLayout](stack-layout.md)**  &ndash;直線的に、ビューを配置に使用される、水平方向または垂直方向にします。 StackLayout 内のビューは、左または右のレイアウトの中央に配置できます。
* **[AbsoluteLayout](absolute-layout.md)**  &ndash;座標の設定によってビューを整列 & 絶対値または比率の単位でサイズを変更するために使用します。 レイヤーのビューだけでなく左、右、または center に固定するには、AbsoluteLayout を使用できます。
* **[[相対レイアウト]](relative-layout.md)**  &ndash;親のサイズと位置を基準とした制約を設定してビューを配置するために使用します。
* **[グリッド](grid.md)** &ndash;をグリッドに表示を配置するために使用します。 絶対値または比率の観点からは、行と列を指定できます。
* **[ScrollView](scroll-view.md)**  &ndash;ビューが、画面の境界内に完全に収めることができなかったときにスクロールを提供するために使用します。
* **[LayoutOptions](layout-options.md)**  &ndash;アラインメントとその親のビューでの展開を定義します。
* **[透明度を入力](#input_transparency)** &ndash;要素が入力を受け取るかどうかを指定します。
* **[マージンと埋め込み](margin-and-padding.md)** &ndash;要素がユーザー インターフェイスに表示される場合は、レイアウトの動作を制御する方法を示します。
* **[デバイスの向き](device-orientation.md)** &ndash;デバイスの向きの変更を処理する方法について説明します。
* **[タブレットおよびデスクトップ デバイスでレイアウト](tablet.md)** &ndash;各プラットフォームでより大きな画面を最適化する方法を示しています。
* **[カスタム レイアウトの作成](custom.md)** &ndash;カスタム レイアウトのクラスを作成する方法について説明します。
* **[レイアウト圧縮](layout-compression.md)** &ndash;では、ページのレンダリング パフォーマンスを向上させるために、特定のビジュアル ツリーからレイアウトを削除します。

プラットフォーム コントロールこともできますで Xamarin.Forms レイアウト内で直接[**ネイティブ埋め込み**](~/xamarin-forms/platform/native-views/index.md) (Xamarin.Forms 2.2 で新規)、することができます、 [ **のカスタムレイアウトを作成**](custom.md)を特定の要件を満たすようにします。

次の図は、レイアウト コントロールを視覚化します。

[![](images/layouts-sml.png "Xamarin.Forms レイアウト")](images/layouts.png#lightbox "Xamarin.Forms レイアウト")

## <a name="choosing-the-right-layout"></a>右のレイアウトを選択します。

アプリで選択したレイアウト ヘルプか不都合は魅力的で使用可能な Xamarin.Forms アプリを作成するいるとします。 クリーナーと拡張性の UI コードを記述する各レイアウト動作がどのように役立つ検討するまでに時間を取得しています。 画面には、特定の設計を実現するためにさまざまなレイアウトの組み合わせを持つことができます。

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout`水平または垂直の線に沿ったビューを表示するためです。 位置とサイズをレイアウト内、に基づいて決定ビューの`HeightRequest`、 `WidthRequest`、`HorizontalOptions`と`VerticalOptions`です。 `StackLayout` 基本のレイアウトでは、画面上の他のレイアウトの配置としてよく使用されます。

場合の例については`StackLayout`適切な選択をすると、ボタンと左揃えのラベルと右揃えボタン付きのラベルを表示する必要があるアプリを検討してください。

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout`サイズと位置のいずれかに指定されている明示的な値またはレイアウトのサイズに対して相対的な値としてのビューを表示するためです。 異なり`StackLayout`と`Grid`、`AbsoluteLayout`子ビューの重複を許可します。 異なり`RelativeLayout`、`AbsoluteLayout`画面外の要素を配置することはできます。

場合の例については`AbsoluteLayout`適切な選択をすると、スタックとして、オブジェクトのコレクションを提示する必要があるアプリを検討してください。 これは、写真や音楽のアルバムを表示する場合、多くの場合、表示されます。 次のコードは、積み上がりの内容をヒントに回転して要素を持つ山の外観を与えます。

XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

上記のコードの次の点に注意してください。

- 各`Image`(水平方向の領域) の途中で同じ位置に表示されます。
- `Padding`でと見なされます`AbsoluteLayout`とは異なり、 `RelativeLayout`、これは無視されます。
- `AbsoluteLayout.LayoutFlags` レイアウトの境界を解釈する方法を指定します。 ここでは`PositionProportional`サイズは明示的なサイズとして解釈するときに、座標が、レイアウトのサイズの比率をことを示します。
- `AbsoluteLayout.Layoutbounds` その注文で水平方向の位置、垂直方向の位置、幅と高さを指定します。

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout`サイズとレイアウトや別のビューの値に対して相対的な値として指定された位置でのビューを表示するためです。 相対値は、彼は対応する、関連するビューの値と一致する必要はありません。 例として、可能であれば、ビューを設定する`Width`プロパティを別のビューに比例する`X`プロパティです。

デバイスのサイズに比例して Ui を作成するのには、[相対レイアウト] を使用できます。 次の XAML では、中央にフラグを使って flagpole での一番上の隅にあるボックスを使用してデザインを実装します。

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

上記のコードの次の点に注意してください。

- 位置とサイズの両方が、制約として指定されます。
- 名前は、flagpole ようにフラグの (緑色のボックスの)、flagpole に対する相対位置を設定することができます。
- 制約の式が`Factor`と`Constant`位置とサイズの倍数として定義するために使用するプロパティ (または分数)、他のオブジェクトと定数のプロパティのです。 定数は負の値にすることができます。

### <a name="gridgridmd"></a>[グリッド](grid.md)

`Grid`行と列の要素を表示するためです。 グリッド テーブルではありません、セル、ヘッダーとフッター行、または行と列の間の罫線の概念があるないように注意してください。 一般に、グリッドでは、表形式のデータを表示する適していません。 その使用を検討してください、 [ListView](~/xamarin-forms/user-interface/listview/index.md)または[テーブル](~/xamarin-forms/user-interface/tableview.md)です。

場合の例については、`Grid`が右のレイアウトを使用して、電卓の数値の入力を検討してください。 電卓の数値の入力は、4 つの行とボタン 3 つの列で構成可能性があります。 次のコードでは、この設計を実装します。

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

上記のコードの次の点に注意してください。

- グリッドと列が明示的に指定されて、コンテンツから推論できません。
- `Height` および`Width`値に設定できますスター、グリッドが使用可能な領域がいっぱいにそれらの値を設定することを意味します。
- 各ボタンの位置が指定された`Grid.Row`  &  `Grid.Column`プロパティです。

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)アラインメントとその親のビューでの展開を定義する構造体を使用することができます。

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[余白とスペース](margin-and-padding.md)

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)と[ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)要素がユーザー インターフェイスに表示される場合、プロパティがレイアウトの動作を制御します。

<a name="input_transparency" />

### <a name="input-transparency"></a>入力の透過性

各要素には、 [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/)プロパティ要素が入力を受け取るかどうかを定義するために使用します。 既定値は`false`要素が入力を受信することを確認します。

レイアウトのクラスでは、子要素に値を転送するなどのコンテナー クラスのこのプロパティを設定するとします。 そのため、設定、 [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/)プロパティを`true`レイアウトのクラスと、入力を受け取っていませんレイアウト内の要素。

### <a name="device-orientationdevice-orientationmd"></a>[デバイスの向き](device-orientation.md)

Xamarin.Forms と、組み込みのレイアウトをデバイスの向きの変更を処理します。 アプリがサポートされているとすれば、どのようにどの向きの考慮の横と縦置きモードで表示されたスペースを使用します。

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[タブレットおよびデスクトップ アプリのレイアウト](tablet.md)

iOS、Android、およびユニバーサル Windows プラットフォームでより大きな画面サイズをサポートしてすべてのタブレット デバイス (ラップトップと Windows のデスクトップ) およびします。 Xamarin.Forms を使用して、デバイスの種類とページ レイアウトを調整するかを検出するか、まったく別のページをより大きな画面の完全を使用してより大きな画面用のアプリを最適化できます。

### <a name="creating-a-custom-layoutcustommd"></a>[カスタム レイアウトの作成](custom.md)

Xamarin.Forms の 4 つのレイアウト クラスを定義する[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)、 [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)、 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)、および[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)、別の方法でその子を整列各とします。 ただし、場合がありますいないレイアウトを使用してページの内容を整理する、必要なによる Xamarin.Forms です。 この記事は、カスタム レイアウト クラスを作成する方法について説明し、印刷の向き依存型を示しています`WrapLayout`ページの上、その子を水平方向に整列し、追加の行に後続の子の表示をラップするクラス。

### <a name="layout-compressionlayout-compressionmd"></a>[レイアウトの圧縮](layout-compression.md)

レイアウトの圧縮は、ページのレンダリング パフォーマンスを向上させるためにでは、ビジュアル ツリーから指定したレイアウトを削除します。 この操作によって得られるパフォーマンスのメリットは、ページの複雑さ、使用しているオペレーティング システムのバージョン、このアプリケーションが実行されているデバイスによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。

## <a name="making-your-choice"></a>選択を行う

ほとんどの場合、複数のレイアウト オプション使用できることを目的の設計を実装するのには注意してください。 複数の有効な選択肢がある場合は、どちらの方法は、簡単に、状況に応じたを検討してください。
として入れ子レイアウトがより複雑なデザインの作成に必要なために、1 つのレイアウトを持つほとんどの設計を認識できません。


## <a name="related-links"></a>関連リンク

- [Apple のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android デザインの web サイト](https://developer.android.com/design/index.html)
- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
