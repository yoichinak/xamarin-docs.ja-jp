---
title: レイアウトの選択 Xamarin.Forms
description: Xamarin.Formsレイアウトクラスを使用すると、アプリケーションに UI コントロールを配置してグループ化できます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 16a48423d05ce1cede75c0020bf18f4f398f5adc
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138503"
---
# <a name="choose-a-xamarinforms-layout"></a>レイアウトの選択 Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Xamarin.Formsレイアウトクラスを使用すると、アプリケーションに UI コントロールを配置してグループ化できます。 レイアウトクラスを選択するには、レイアウトが子要素を配置する方法と、レイアウトがその子要素をどのようにサイズ調整するかについての知識が必要です。 また、レイアウトを入れ子にして目的のレイアウトを作成することが必要になる場合もあります。

次の図は、メインレイアウトクラスを使用して実現できる一般的なレイアウトを示してい Xamarin.Forms ます。

[![のメインレイアウトクラスXamarin.Forms](images/layouts.png "[!ファンド.NO LOC (Xamarin)] レイアウトクラス")](images/layouts-large.png#lightbox "[!ファンド.NO LOC (Xamarin)] レイアウトクラス")

## <a name="stacklayout"></a>StackLayout

は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 1 次元のスタック内の要素を水平方向または垂直方向に編成します。 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)プロパティは要素の方向を指定し、既定の向きは [`Vertical`](xref:Xamarin.Forms.StackOrientation) です。 `StackLayout`は、通常、ページ上の UI のサブセクションを配置するために使用されます。

次の XAML は、3つのオブジェクトを含む垂直方向の作成方法を示してい [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label) ます。

```xaml
<StackLayout Margin="20,35,20,25">
    <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
    <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
    <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
</StackLayout>
```

では、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 要素のサイズが明示的に設定されていない場合は、使用可能な幅を埋めるように拡張されるか、 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) プロパティがに設定されている場合は height が拡張され [`Horizontal`](xref:Xamarin.Forms.StackOrientation) ます。

は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 多くの場合、他の子レイアウトを含む親レイアウトとして使用されます。 ただし、を使用して、 `StackLayout` [`Grid`](xref:Xamarin.Forms.Grid) オブジェクトの組み合わせを使用してレイアウトを再現することはできません `StackLayout` 。 次のコードは、この不適切な方法の例を示しています。

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

不要なレイアウト計算が行われるため、不経済です。 代わりに、を使用すると、目的のレイアウトをより適切に実現でき [`Grid`](xref:Xamarin.Forms.Grid) ます。

> [!TIP]
> を使用する場合 [`StackLayout`](xref:Xamarin.Forms.StackLayout) は、子要素が1つだけに設定されていることを確認して [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) ください。 このプロパティにより、指定された子は、`StackLayout` がそれに与えられる最大の領域を占有します。このような計算を複数回実行することは無駄です。

詳細については、「 [ Xamarin.Forms stacklayout](stacklayout.md)」を参照してください。

## <a name="grid"></a>グリッド

は、 [`Grid`](xref:Xamarin.Forms.Grid) 行と列に要素を表示するために使用されます。これは、比例または絶対的なサイズを持つことができます。 グリッドの行と列は、プロパティとプロパティで指定し [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) ます。

特定のセルに要素を配置するに [`Grid`](xref:Xamarin.Forms.Grid) は、 [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) および添付プロパティを使用し [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) ます。 複数の行および列にまたがる要素を作成するには、 [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) および添付プロパティを使用し [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) ます。

> [!NOTE]
> [`Grid`](xref:Xamarin.Forms.Grid)レイアウトはテーブルと混同しないようにしてください。表形式のデータを表示するためのものではありません。 HTML テーブルとは異なり、は、コンテンツをレイアウトすることを目的とし `Grid` ています。 表形式データを表示する場合は、 [ListView](~/xamarin-forms/user-interface/listview/index.md)、 [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)、または[TableView](~/xamarin-forms/user-interface/tableview.md)を使用することを検討してください。

次の XAML は、 [`Grid`](xref:Xamarin.Forms.Grid) 2 つの行と2つの列を持つを作成する方法を示しています。

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
           WidthRequest="200" />
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
- 最初の列の幅はに設定され [`Auto`](xref:Xamarin.Forms.GridLength.Auto) ます。したがって、その子に必要な幅になります。 この場合、1つ目の幅に合わせて、デバイスに依存しない単位は200です [`Label`](xref:Xamarin.Forms.Label) 。

列や行のサイズをコンテンツに合わせて調整できる自動サイズ設定を使用して、列または行の中にスペースを分散させることができます。 これを実現するには、の高さ、 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) またはの幅を [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) に設定し [`Auto`](xref:Xamarin.Forms.GridLength.Auto) ます。 比例サイズ設定を使用して、グリッドの行と列の間に使用可能な領域を重み付け比率別に配分することもできます。 これを実現するには、の高さ、 `RowDefinition` またはの幅を、演算子を `ColumnDefinition` 使用する値に設定し `*` ます。

> [!CAUTION]
> できるだけ少ない数の行と列が size に設定されるようにしてください [`Auto`](xref:Xamarin.Forms.GridLength.Auto) 。 自動サイズ調整された行または列はそれぞれ、レイアウト エンジンに追加のレイアウト計算を実行させることになります。 その代わりに可能であれば、固定サイズの行と列を使用してください。 または、行と列に対して、列挙値を使用して比例した領域を占めるように設定し [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) ます。

詳細については、「 [ Xamarin.Forms Grid](grid.md)」を参照してください。

## <a name="flexlayout"></a>FlexLayout

は [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) 、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 子要素を水平方向または垂直方向にスタックに表示するという点でに似ています。 ただし、は、 `FlexLayout` 1 つの行または列に収まらない場合に、子をラップすることもできます。また、子要素のサイズ、向き、および配置をより細かく制御できます。

次の XAML は、 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) 1 つの列にビューを表示するを作成する方法を示しています。

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

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)プロパティはに設定され `Column` ます。これにより、の子が `FlexLayout` 項目の1つの列に配置されます。
- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)プロパティはに設定されます `Center` 。これにより、各項目が水平方向に中央揃えになります。
- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)プロパティがに設定されてい `SpaceEvenly` ます。これにより、すべての項目と最初の項目の上、および最後の項目の下のすべての領域が均等に割り当てられます。

詳細については、「 [ Xamarin.Forms flexlayout](flex-layout.md)」を参照してください。

## <a name="relativelayout"></a>RelativeLayout

は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) レイアウト要素または兄弟要素のプロパティを基準として要素の位置やサイズを変更するために使用されます。 既定では、要素はレイアウトの左上隅に配置されます。 を使用すると、 `RelativeLayout` デバイスのサイズに比例してスケーリングする ui を作成できます。

内では [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 、位置とサイズは制約として指定されます。 制約 [`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor) には [`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant) プロパティとプロパティがあります。これを使用すると、他のオブジェクトのプロパティの倍数 (または小数) と定数を加算して、位置とサイズを定義できます。 また、定数には負の値を指定できます。

> [!NOTE]
> は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 要素を独自の境界の外側に配置することをサポートしています。

次の XAML は、内の要素を整列する方法を示してい [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) ます。

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

- Blue に [`BoxView`](xref:Xamarin.Forms.BoxView) は、デバイスに依存しない単位である50x50 の明示的サイズが指定されています。 レイアウトの左上隅 (既定の位置) に配置されます。
- 赤に [`BoxView`](xref:Xamarin.Forms.BoxView) は、デバイスに依存しない単位である50x50 の明示的なサイズが指定されています。 レイアウトの右上隅に配置されます。
- 灰色に [`BoxView`](xref:Xamarin.Forms.BoxView) は、デバイスに依存しない単位の明示的な幅が指定されています。高さは、その親の高さの75% に設定されています。
- 緑には、 [`BoxView`](xref:Xamarin.Forms.BoxView) 明示的なサイズが指定されていません。 その位置は、という名前のに相対的に設定され `BoxView` `pole` ます。

> [!WARNING]
> 可能であれば、`RelativeLayout` は使用しないでください。 CPU で相当な量の作業を実行しなければならなくなります。

詳細については、「 [ Xamarin.Forms RelativeLayout](relative-layout.md)」を参照してください。

## <a name="absolutelayout"></a>AbsoluteLayout

は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 明示的な値、またはレイアウトのサイズを基準とした値を使用して、要素の位置とサイズを指定するために使用されます。 位置は、の左上隅を基準として、子の左上隅によって指定され `AbsoluteLayout` ます。

は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 子にサイズを設定できる場合や、要素のサイズが他の子の位置に影響を与えない場合にのみ使用される特殊な目的のレイアウトと見なされる必要があります。 このレイアウトの標準的な用途は、他のコントロールでページをカバーするオーバーレイを作成することです。これにより、ユーザーがページ上の通常のコントロールと対話できないように保護することができます。

> [!IMPORTANT]
> `HorizontalOptions`プロパティと `VerticalOptions` プロパティは、の子には影響しません `AbsoluteLayout` 。

内で [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) は、 [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 添付プロパティを使用して、要素の水平位置、垂直位置、幅、および高さを指定します。 さらに、 [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) 添付プロパティは、レイアウトの範囲をどのように解釈するかを指定します。

次の XAML は、の要素を配置する方法を示してい [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) ます。

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

- 各 [`BoxView`](xref:Xamarin.Forms.BoxView) には 100 x 100 の明示的なサイズが与えられ、同じ位置に水平方向に中央揃えで表示されます。
- 赤は [`BoxView`](xref:Xamarin.Forms.BoxView) 30 °回転し、緑は `BoxView` 60 °回転します。
- 各で [`BoxView`](xref:Xamarin.Forms.BoxView) は、 [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) 添付プロパティがに設定されます。これは、 `PositionProportional` 幅と高さを考慮した後の残りの領域に、位置が比例していることを示します。

> [!CAUTION]
> [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize)レイアウトエンジンでは追加のレイアウト計算が実行されるため、可能な限りプロパティを使用しないでください。

詳細については、「 [ Xamarin.Forms AbsoluteLayout](absolute-layout.md)」を参照してください。

## <a name="input-transparency"></a>入力の透明度

各ビジュアル要素には、 [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) 要素が入力を受け取るかどうかを定義するために使用されるプロパティがあります。 既定値は `false` で、要素が入力を受け取ることを保証します。

このプロパティがレイアウトクラスに設定されている場合、その値は子要素に転送されます。 したがって、 [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) レイアウトクラスでプロパティをに設定する `true` と、レイアウト内のすべての要素が入力を受け取らなくなります。

## <a name="layout-performance"></a>レイアウトのパフォーマンス

レイアウトの最適なパフォーマンスを得るには、「[レイアウトのパフォーマンスを最適化](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)する」のガイドラインに従ってください。

さらに、レイアウトの圧縮を使用して、指定されたレイアウトをビジュアルツリーから削除することで、ページレンダリングのパフォーマンスを向上させることもできます。 詳細については、「[レイアウトの圧縮](layout-compression.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Xamarin.Formsレイアウト (ビデオ)](https://youtu.be/4HlLjTZQzjM)
- [Xamarin.FormsStackLayout](stacklayout.md)
- [Xamarin.Forms行列](grid.md)
- [Xamarin.FormsFlexLayout](flex-layout.md)
- [Xamarin.FormsAbsoluteLayout](absolute-layout.md)
- [Xamarin.FormsRelativeLayout](relative-layout.md)
- [レイアウトのパフォーマンスを最適化する](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)
- [レイアウトの圧縮](layout-compression.md)
