---
title: Xamarin.Forms のレイアウト
description: Xamarin.Forms がいくつかのレイアウトと、画面上のコンテンツを整理するための機能と、この記事では、それらについて説明します。
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 5bd232293c979566faed2856de7287903da94054
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831769"
---
# <a name="layouts-in-xamarinforms"></a>Xamarin.Forms のレイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

Xamarin.Forms は、いくつかのレイアウトと画面上のコンテンツを整理するための機能があります。

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Xamarin.Forms のレイアウトにより、 [Xamarin University](https://university.xamarin.com/)**

各レイアウト コントロールは画面の向きの変更を処理する方法の詳細と、以下について説明します。

* **[StackLayout](stack-layout.md)**  – ビューを直線的に、配置を水平方向または垂直方向にします。 ビュー、StackLayout 内では、左または右のレイアウトの中央に配置できます。
* **[AbsoluteLayout](absolute-layout.md)**  – 座標を設定してビューを配置およびサイズを絶対値または比率の観点からするために使用します。 ビューのレイヤーだけでなく、左、右または中央に固定する AbsoluteLayout を使用できます。
* **[[相対レイアウト]](relative-layout.md)**  – 親のサイズと位置を基準とした制約を設定してビューを配置するために使用されます。
* **[グリッド](grid.md)** – グリッド内のビューを配置するために使用されます。 絶対値または比率の観点からは、行と列を指定できます。
* **[FlexLayout](flex-layout.md)**  – の折り返しビューを水平方向または垂直方向に配置するために使用されます。
* **[ScrollView](scroll-view.md)**  – をスクロールすると表示されますが、画面の境界内に完全に収めることはできませんを提供します。
* **[LayoutOptions](layout-options.md)**  – アラインメントとその親に対する相対的な表示は、拡張を定義します。
* **[透明度を入力](#input_transparency)** – 要素が入力を受け取るかどうかを指定します。
* **[余白とパディング](margin-and-padding.md)** – 要素は、ユーザー インターフェイスに表示されるときに、レイアウトの動作を制御する方法を示します。
* **[デバイスの向き](device-orientation.md)** – デバイスの向きの変更を処理する方法について説明します。
* **[タブレットとデスクトップ デバイス上のレイアウト](tablet.md)** – 各プラットフォームでの大きい画面を最適化する方法を示しています。
* **[バインド可能なレイアウト](bindable-layouts.md)** – 項目のコレクションにバインドして、コンテンツを生成するレイアウト クラスを有効にします。
* **[カスタム レイアウトを作成する](custom.md)** – カスタム レイアウトのクラスを作成する方法について説明します。
* **[レイアウト圧縮](layout-compression.md)** – ページのレンダリング パフォーマンスを向上させるために指定したビジュアル ツリーからレイアウトを削除します。

プラットフォームのコントロールと Xamarin.Forms のレイアウトで直接も使用できます[**ネイティブな埋め込み**](~/xamarin-forms/platform/native-views/index.md) (Xamarin.Forms 2.2 で新規)、し[ **のカスタムレイアウトを作成します。**](custom.md)を特定の要件を満たすようにします。

次の図は、レイアウト コントロールを視覚化します。

[![](images/layouts-sml.png "Xamarin.Forms のレイアウト")](images/layouts.png#lightbox "Xamarin.Forms のレイアウト")

## <a name="choosing-the-right-layout"></a>適切なレイアウトを選択します。

アプリで選択したレイアウトするヘルプか不都合は魅力的で使用可能な Xamarin.Forms アプリを作成するとします。 各レイアウト機能が役立つクリーナー スケーラブルな UI コードを記述する方法を検討するまでに時間がかかっています。 画面には、特定のデザインを実現するためにさまざまなレイアウトの組み合わせを持つことができます。

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout`に沿って水平または垂直のいずれかである行ビューを表示するためです。 位置とサイズ、レイアウト内、に基づいて決定ビューの`HeightRequest`、 `WidthRequest`、`HorizontalOptions`と`VerticalOptions`します。 `StackLayout` 基本のレイアウトでは、画面上の他のレイアウトの配置としてよく使用されます。

タイミングの例については`StackLayout`適していますが、ボタンと左揃えのラベルと右揃え ボタン、ラベルを表示する必要があるアプリを検討してください。

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="flexlayoutflex-layoutmd"></a>[FlexLayout](flex-layout.md)

`FlexLayout`のような`StackLayout`水平方向または垂直方向には、子ビューを表示します。

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">

    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

ただし入りますを 1 つの行に収まるように多数の子が存在する場合は、`FlexLayout`はこれらのビューをラップすることもできます。 `FlexLayout` CSS フレキシブル ボックス レイアウト モジュールに基づいており、配置とその子のアラインメントの同じの組み込みオプションの多くが。

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout`サイズと位置のいずれかに指定されている明示的な値またはレイアウトのサイズに対して相対的な値としてのビューを表示するためです。 異なり`StackLayout`と`Grid`、`AbsoluteLayout`子が重複するビューを使用します。 異なり`RelativeLayout`、`AbsoluteLayout`が画面外の要素を配置することができます。

タイミングの例については`AbsoluteLayout`適していますが、スタックとしてオブジェクトのコレクションを提示する必要があるアプリを検討してください。 これは多くの場合、写真や音楽のアルバムを表示する場合に発生します。 次のコードでは、積みの内容をヒントに回転して要素を持つ、山の外観を与えます。

で XAML:

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

上記のコードの以下の点に注意してください。

- 各`Image`(水平方向の領域) の途中で同じ位置に表示されます。
- `Padding`でと見なされます`AbsoluteLayout`とは異なり、`RelativeLayout`を無視します。
- `AbsoluteLayout.LayoutFlags` レイアウトの境界を解釈する方法を指定します。 ここで`PositionProportional`サイズは、明示的なサイズとして解釈されます中に、座標が、レイアウトのサイズの比率をことを示します。
- `AbsoluteLayout.Layoutbounds` この順序で水平方向の位置、垂直方向の位置、幅と高さを指定します。

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout`サイズとレイアウトまたは別のビューの値に対して相対的な値として指定された位置でのビューを表示するためです。 相対値は、関連するビューの値を対応する彼と一致する必要はありません。 例については、ビューを設定することが`Width`プロパティを別のビューに比例して`X`プロパティ。

デバイスのサイズに比例して Ui を作成するのには、[相対レイアウト] を使用できます。 次の XAML は、センターのフラグの flagpole で最上位の角にあるボックスを使用してデザインを実装します。

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

上記のコードの以下の点に注意してください。

- 位置とサイズの両方が、制約として指定されます。
- 名前は、flagpole ようにフラグの (緑色のボックスの)、flagpole に対する相対位置を設定することができます。
- 制約の式が`Factor`と`Constant`プロパティは、位置とサイズの倍数として定義するために使用できます (または端数) の定数、他のオブジェクトのプロパティ。 定数は負の値にすることはできます。

### <a name="gridgridmd"></a>[グリッド](grid.md)

`Grid`行と列の要素を表示するためです。 グリッド テーブルではありません、セル、行のヘッダーとフッター、または行と列の間の罫線の概念があるないように注意してください。 一般に、グリッドでは、表形式のデータを表示するには適していません。 使用するを検討してください、 [ListView](~/xamarin-forms/user-interface/listview/index.md)または[テーブル](~/xamarin-forms/user-interface/tableview.md)します。

タイミングの例については、`Grid`が右のレイアウトを使用するには、電卓の数値の入力を検討してください。 電卓の数値の入力が 4 つの行とボタンを 3 つの列で構成されます。 次のコードは、この設計を実装します。

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

上記のコードの以下の点に注意してください。

- グリッドと列が明示的に指定されて、コンテンツから推論できません。
- `Height` `Width`値に設定できますスター、つまり、グリッドが空き領域の塗りつぶしにそれらの値を設定します。
- 各ボタンの位置が指定された`Grid.Row`  &  `Grid.Column`プロパティ。

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)配置とその親に対する相対的な表示は、拡張を定義する構造体を使用できます。

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[余白とスペース](margin-and-padding.md)

[ `Margin` ](xref:Xamarin.Forms.View.Margin)と[ `Padding` ](xref:Xamarin.Forms.Layout.Padding)プロパティが、ユーザー インターフェイス要素が表示されるときに、レイアウトの動作を制御します。

<a name="input_transparency" />

### <a name="input-transparency"></a>入力の透過性

各要素には、 [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent)要素が入力を受け取るかどうかを定義するために使用するプロパティ。 既定値は`false`要素が入力を受け取ることを確認します。

レイアウト クラスであり、その子要素に値の転送などのコンテナー クラスでこのプロパティを設定するとします。 そのため、設定、 [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent)プロパティを`true`レイアウト上の入力を受信していないレイアウト内の要素のクラスが発生します。

### <a name="device-orientationdevice-orientationmd"></a>[デバイスの向き](device-orientation.md)

Xamarin.Forms と、組み込みのレイアウトは、デバイスの向きの変更を処理できます。 アプリがサポートされているにも方法をどの向きを検討してください。 横、縦モードで表示されたスペースを使用します。

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[タブレットとデスクトップ アプリのレイアウト](tablet.md)

iOS、Android、およびユニバーサル Windows プラットフォームでより大きな画面サイズをサポートしてすべてのタブレット デバイス (ラップトップと Windows のデスクトップ) と同様です。 Xamarin.Forms を使用して、デバイスの種類と、ページ レイアウトを調整するかを検出するか、大きい画面の完全、まったく違うページを使用して、大きい画面のアプリを最適化できます。

### <a name="bindable-layoutsbindable-layoutsmd"></a>[バインド可能なレイアウト](bindable-layouts.md)

`BindableLayout`クラスから派生した任意のレイアウト クラスを使用できます、 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)で各項目の外観を設定することも、項目のコレクションにバインドするには、そのコンテンツを生成するクラス、 [ `DataTemplate`](xref:Xamarin.Forms.DataTemplate).

### <a name="creating-a-custom-layoutcustommd"></a>[カスタム レイアウトの作成](custom.md)

Xamarin.Forms の 4 つのレイアウト クラスを定義する[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、 [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout)、 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout)、および[ `Grid` ](xref:Xamarin.Forms.Grid)、し、その子を別の方法で配置の各します。 Xamarin.Forms によって、必要なレイアウトを使用していないページ コンテンツの編成が提供される場合がありますただし、します。 この記事では、カスタム レイアウト クラスを作成する方法について説明し、区別の向きを示します`WrapLayout`クラスを追加の行に後続の子の表示をラップし、ページ間で、その子を水平方向に整列します。

### <a name="layout-compressionlayout-compressionmd"></a>[レイアウトの圧縮](layout-compression.md)

レイアウト圧縮は、ページのレンダリング パフォーマンスを向上させるために、ビジュアル ツリーから指定したレイアウトを削除します。 この機能が提供するパフォーマンスの効果は、ページの複雑さ、使用しているオペレーティング システムのバージョン、およびアプリケーションを実行しているデバイスによって異なります。 しかし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。

## <a name="making-your-choice"></a>選択を行う

ほとんどの場合、1 つ以上のレイアウトの選択した使用できること、必要な設計を実装するためにあります。 複数の有効な選択肢が存在する場合、ユーザーの状況に最も簡単なため、どのアプローチを検討してください。
入れ子のレイアウトとしてより複雑なデザインを作成するために必要なため、1 つのレイアウトとほとんどの設計を認識できません。

## <a name="related-links"></a>関連リンク

- [Apple のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android のデザインの web サイト](https://developer.android.com/design/index.html)
- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
