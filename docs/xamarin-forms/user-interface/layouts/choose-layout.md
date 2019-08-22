---
title: Xamarin. フォームレイアウトを選択する
description: Xamarin. Forms layout クラスを使用すると、アプリケーションで UI コントロールを配置してグループ化できます。
ms.prod: xamarin
ms.assetid: 05A39752-A174-447E-A30D-3CC9EF98CB96
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: 161da8948f356fef997a411855598bc99d2f49b7
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69893999"
---
# <a name="choose-a-xamarinforms-layout"></a>Xamarin. フォームレイアウトを選択する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Xamarin. Forms layout クラスを使用すると、アプリケーションで UI コントロールを配置してグループ化できます。 レイアウトクラスを選択するには、レイアウトが子要素を配置する方法と、レイアウトがその子要素をどのようにサイズ調整するかについての知識が必要です。 また、レイアウトを入れ子にして目的のレイアウトを作成することが必要になる場合もあります。

次の図は、メインの Xamarin. フォームレイアウトクラスで実現できる典型的なレイアウトを示しています。

[Xamarin. forms(images/layouts.png "layout クラス")の![メインレイアウトクラス]](images/layouts-large.png#lightbox "Xamarin. フォームレイアウトクラス")

## <a name="stacklayout"></a>StackLayout

は[`StackLayout`](xref:Xamarin.Forms.StackLayout) 、1次元のスタック内の要素を水平方向または垂直方向に編成します。 プロパティ[`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)は要素の方向を指定し、既定の向きは[`Vertical`](xref:Xamarin.Forms.StackOrientation)です。 `StackLayout`は、通常、ページ上の UI のサブセクションを配置するために使用されます。

次の XAML は、3つ[`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label)のオブジェクトを含む垂直方向の作成方法を示しています。

```xaml
<StackLayout Margin="20,35,20,25">
    <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
    <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
    <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
</StackLayout>
```

では、要素のサイズが明示的に設定されていない場合は、使用可能な幅を埋める[`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)ように拡張さ[`Horizontal`](xref:Xamarin.Forms.StackOrientation)れるか、プロパティがに設定されている場合は height が拡張されます。 [`StackLayout`](xref:Xamarin.Forms.StackLayout)

は[`StackLayout`](xref:Xamarin.Forms.StackLayout) 、多くの場合、他の子レイアウトを含む親レイアウトとして使用されます。 ただし、を`StackLayout`使用して、オブジェクトの`StackLayout`組み合わせ[`Grid`](xref:Xamarin.Forms.Grid)を使用してレイアウトを再現することはできません。 次のコードは、この不適切な方法の例を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

不要なレイアウト計算が行われるため、不経済です。 代わりに、を使用[`Grid`](xref:Xamarin.Forms.Grid)すると、目的のレイアウトをより適切に実現できます。

> [!TIP]
> を使用[`StackLayout`](xref:Xamarin.Forms.StackLayout)する場合は、子要素が1つだけに[`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)設定されていることを確認してください。 このプロパティにより、指定された子は、`StackLayout` がそれに与えられる最大の領域を占有します。このような計算を複数回実行することは無駄です。

詳細については、「 [Xamarin の StackLayout](stack-layout.md)」を参照してください。

## <a name="grid"></a>グリッド

は[`Grid`](xref:Xamarin.Forms.Grid) 、行と列に要素を表示するために使用されます。これは、比例または絶対的なサイズを持つことができます。 グリッドの行と列は、 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)プロパティとプロパティで指定します。

特定[`Grid`](xref:Xamarin.Forms.Grid)のセルに要素を配置するに[`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty)は[`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) 、および添付プロパティを使用します。 複数の行および列にまたがる要素を作成するに[`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)は[`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 、および添付プロパティを使用します。

> [!NOTE]
> [`Grid`](xref:Xamarin.Forms.Grid)レイアウトはテーブルと混同しないようにしてください。表形式のデータを表示するためのものではありません。 HTML テーブルとは異なり`Grid` 、は、コンテンツをレイアウトすることを目的としています。 表形式データを表示する場合は、 [ListView](~/xamarin-forms/user-interface/listview/index.md)、 [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)、または[TableView](~/xamarin-forms/user-interface/tableview.md)を使用することを検討してください。

次の XAML は、2つの[`Grid`](xref:Xamarin.Forms.Grid)行と2つの列を持つを作成する方法を示しています。

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>    
    <Label Text="Column 0, Row 0"
           Width="200" />
    <Label Grid.Column="1"
           Text="Column 1, Row 0" />
    <Label Grid.Row="1"
           Text="Column 0, Row 1" />
    <Label Grid.Column="1"
           Grid.Row="1"
           Text="Column 1, Row 1" />
</Grid>
```

この例では、サイズ変更は次のように機能します。

- 各行には、50のデバイスに依存しない単位の高さが明示的に付いています。
- 最初の列の幅はに[`Auto`](xref:Xamarin.Forms.GridLength.Auto)設定されます。したがって、その子に必要な幅になります。 この場合、1つ目[`Label`](xref:Xamarin.Forms.Label)の幅に合わせて、デバイスに依存しない単位は200です。

列や行のサイズをコンテンツに合わせて調整できる自動サイズ設定を使用して、列または行の中にスペースを分散させることができます。 これを実現するには、の高さ[`RowDefinition`](xref:Xamarin.Forms.RowDefinition)、または[`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)の幅をに[`Auto`](xref:Xamarin.Forms.GridLength.Auto)設定します。 比例サイズ設定を使用して、グリッドの行と列の間に使用可能な領域を重み付け比率別に配分することもできます。 これを実現するには、の高さ`RowDefinition`、または`ColumnDefinition`の幅を、 `*`演算子を使用する値に設定します。

> [!CAUTION]
> できるだけ少ない数の行と列が size に[`Auto`](xref:Xamarin.Forms.GridLength.Auto)設定されるようにしてください。 自動サイズ調整された行または列はそれぞれ、レイアウト エンジンに追加のレイアウト計算を実行させることになります。 その代わりに可能であれば、固定サイズの行と列を使用してください。 または、行と列に対して、 [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star)列挙値を使用して比例した領域を占めるように設定します。

詳細については、「 [Xamarin. Forms Grid](grid.md)」を参照してください。

## <a name="flexlayout"></a>FlexLayout

は[、`StackLayout`](xref:Xamarin.Forms.StackLayout)子要素を水平方向または垂直方向にスタックに表示するという点でに似て[います。`FlexLayout`](xref:Xamarin.Forms.FlexLayout) ただし、は`FlexLayout` 、1つの行または列に収まらない場合に、子をラップすることもできます。また、子要素のサイズ、向き、および配置をより細かく制御できます。

次の XAML は、1つの[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)列にビューを表示するを作成する方法を示しています。

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">
    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

この例では、レイアウトは次のように機能します。

- プロパティはに`Column`設定されます。これにより、 `FlexLayout`の子が項目の1つの列に配置されます。 [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)
- プロパティはに`Center`設定されます。これにより、各項目が水平方向に中央揃えになります。 [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)
- プロパティがに設定さ`SpaceEvenly`れています。これにより、すべての項目と最初の項目の上、および最後の項目の下のすべての領域が均等に割り当てられます。 [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)

詳細については、「 [Xamarin. Forms FlexLayout](flex-layout.md)」を参照してください。

## <a name="relativelayout"></a>RelativeLayout

は[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 、レイアウト要素または兄弟要素のプロパティを基準として要素の位置やサイズを変更するために使用されます。 既定では、要素はレイアウトの左上隅に配置されます。 を`RelativeLayout`使用すると、デバイスのサイズに比例してスケーリングする ui を作成できます。

内では、位置とサイズは制約として指定されます。 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 制約に[`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor)は[`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant)プロパティとプロパティがあります。これを使用すると、他のオブジェクトのプロパティの倍数 (または小数) と定数を加算して、位置とサイズを定義できます。 また、定数には負の値を指定できます。

> [!NOTE]
> は[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 、要素を独自の境界の外側に配置することをサポートしています。

次の XAML は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)内の要素を整列する方法を示しています。

```xaml
<RelativeLayout>
    <BoxView Color="Blue"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView Color="Red"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.85}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView x:Name="pole"
             Color="Gray"
             WidthRequest="15"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.45}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.25}" />
    <BoxView Color="Green"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
             RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.2, Constant=20}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

この例では、レイアウトは次のように機能します。

- Blue [`BoxView`](xref:Xamarin.Forms.BoxView)には、デバイスに依存しない単位である50x50 の明示的サイズが指定されています。 レイアウトの左上隅 (既定の位置) に配置されます。
- 赤[`BoxView`](xref:Xamarin.Forms.BoxView)には、デバイスに依存しない単位である50x50 の明示的なサイズが指定されています。 レイアウトの右上隅に配置されます。
- 灰色[`BoxView`](xref:Xamarin.Forms.BoxView)には、デバイスに依存しない単位の明示的な幅が指定されています。高さは、その親の高さの 75% に設定されています。
- 緑[`BoxView`](xref:Xamarin.Forms.BoxView)には、明示的なサイズが指定されていません。 その位置は、という名前`BoxView` `pole`のに相対的に設定されます。

> [!WARNING]
> 可能な限り`RelativeLayout` 、を使用しないでください。 CPU で相当な量の作業を実行しなければならなくなります。

詳細については、「 [RelativeLayout](relative-layout.md)」を参照してください。

## <a name="absolutelayout"></a>AbsoluteLayout

は[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 、明示的な値、またはレイアウトのサイズを基準とした値を使用して、要素の位置とサイズを指定するために使用されます。 位置は、 `AbsoluteLayout`の左上隅を基準として、子の左上隅によって指定されます。

は[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 、子にサイズを設定できる場合や、要素のサイズが他の子の位置に影響を与えない場合にのみ使用される特殊な目的のレイアウトと見なされる必要があります。 このレイアウトの標準的な用途は、他のコントロールでページをカバーするオーバーレイを作成することです。これにより、ユーザーがページ上の通常のコントロールと対話できないように保護することができます。

> [!IMPORTANT]
> `HorizontalOptions`と`VerticalOptions`の子にプロパティの影響がない、`AbsoluteLayout`します。

内では、添付プロパティを使用して、要素の水平位置、垂直位置、幅、および高さを指定します。 [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) さらに、添付[`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)プロパティは、レイアウトの範囲をどのように解釈するかを指定します。

次の XAML は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)の要素を配置する方法を示しています。

```xaml
<AbsoluteLayout Margin="40">
    <BoxView Color="Red"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="30" />
    <BoxView Color="Green"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="60" />
    <BoxView Color="Blue"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" />
</AbsoluteLayout>
```

この例では、レイアウトは次のように機能します。

- 各[`BoxView`](xref:Xamarin.Forms.BoxView)には 100 x 100 の明示的なサイズが与えられ、同じ位置に水平方向に中央揃えで表示されます。
- 赤[`BoxView`](xref:Xamarin.Forms.BoxView)は30°回転し、緑`BoxView`は60°回転します。
- 各[`BoxView`](xref:Xamarin.Forms.BoxView) `PositionProportional`では、[添付プロパティがに設定されます。これは、幅と高さを考慮した後の残りの領域に、位置が比例していることを示します。`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)

> [!CAUTION]
> レイアウトエンジンで[`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize)は追加のレイアウト計算が実行されるため、可能な限りプロパティを使用しないでください。

詳細については、「 [AbsoluteLayout](absolute-layout.md)」を参照してください。

## <a name="input-transparency"></a>入力の透明度

各ビジュアル要素には[`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) 、要素が入力を受け取るかどうかを定義するために使用されるプロパティがあります。 既定値は`false`要素が入力を受け取ることを確認します。

このプロパティがレイアウトクラスに設定されている場合、その値は子要素に転送されます。 したがって、レイアウト[`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)クラスで`true`プロパティをに設定すると、レイアウト内のすべての要素が入力を受け取らなくなります。

## <a name="layout-performance"></a>レイアウトのパフォーマンス

レイアウトの最適なパフォーマンスを得るには、「[レイアウトのパフォーマンスを最適化](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)する」のガイドラインに従ってください。

さらに、レイアウトの圧縮を使用して、指定されたレイアウトをビジュアルツリーから削除することで、ページレンダリングのパフォーマンスを向上させることもできます。 詳細については、「[レイアウトの圧縮](layout-compression.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Xamarin. フォームレイアウト (ビデオ)](https://youtu.be/4HlLjTZQzjM)
- [Xamarin. フォーム StackLayout](stack-layout.md)
- [Xamarin. フォームグリッド](grid.md)
- [Xamarin. フォーム FlexLayout](flex-layout.md)
- [AbsoluteLayout](absolute-layout.md)
- [RelativeLayout](relative-layout.md)
- [レイアウトのパフォーマンスを最適化する](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)
- [レイアウトの圧縮](layout-compression.md)
