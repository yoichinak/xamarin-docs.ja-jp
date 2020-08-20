---
title: Xamarin.Forms RelativeLayout
description: Xamarin.FormsRelativeLayout は、レイアウトまたは兄弟要素のプロパティと比較して、子の位置とサイズを使用します。
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2020
ms.custom: contperfq1
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 77b1837fb58d5743fd887b9f636f7f7311b807d3
ms.sourcegitcommit: 9bd6b1b20d126b3f837c4cf859b25895c242e54e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2020
ms.locfileid: "88648175"
---
# <a name="no-locxamarinforms-relativelayout"></a>Xamarin.Forms RelativeLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-relativelayoutdemos)

[![::: no-loc (Xamarin. Forms)::: RelativeLayout](relativelayout-images/layouts.png)](relativelayout-images/layouts-large.png#lightbox)

は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) レイアウトまたは兄弟要素のプロパティを基準として、子の位置やサイズを変更するために使用されます。 これにより、デバイスのサイズに比例してスケールする Ui を作成できます。 また、他のいくつかのレイアウトクラスとは異なり、で `RelativeLayout` は、重なり合うように子を配置できます。

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)クラスは、次のプロパティを定義します。

- [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty)型の [`Constraint`](xref:Xamarin.Forms.Constraint) 。これは、子の X 位置の制約を表す添付プロパティです。
- [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty)型の。 [`Constraint`](xref:Xamarin.Forms.Constraint) これは、子の Y 位置の制約を表す添付プロパティです。
- [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty)型の [`Constraint`](xref:Xamarin.Forms.Constraint) 。これは、子の幅に対する制約を表す添付プロパティです。
- [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty)型の [`Constraint`](xref:Xamarin.Forms.Constraint) 。これは、子の高さに対する制約を表す添付プロパティです。
- [`BoundsConstraint`](xref:Xamarin.Forms.RelativeLayout.BoundsConstraintProperty)型の。 [`BoundsConstraint`](xref:Xamarin.Forms.BoundsConstraint) これは、子の位置とサイズに関する制約を表す添付プロパティです。 このプロパティは、XAML から簡単に使用することはできません。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、プロパティは、データバインディングのターゲットとスタイルを設定できます。 添付プロパティの詳細については、「 [ Xamarin.Forms 添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください。

> [!NOTE]
> 内の子の幅と高さは、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) `WidthRequest` および添付プロパティではなく、子のプロパティとプロパティを使用して指定することもでき `HeightRequest` [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) ます。

クラスは、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) `Layout<T>` 型のプロパティを定義するクラスから派生 `Children` `IList<T>` します。 `Children`プロパティは `ContentProperty` クラスのである `Layout<T>` ため、XAML から明示的に設定する必要はありません。

> [!TIP]
> 可能であれば、[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) は使用しないでください。 CPU で相当な量の作業を実行しなければならなくなります。

## <a name="constraints"></a>制約

内では [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 、子の位置とサイズは、絶対値または相対値を使用して制約として指定されます。 制約が指定されていない場合、子はレイアウトの左上隅に配置されます。

次の表は、XAML と C# で制約を指定する方法を示しています。

|     | XAML | C# |
| --- | ---- | -- |
| **絶対値** | 絶対制約は、添付プロパティを値に設定することによって指定し [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) `double` ます。 | 絶対制約は、メソッドによって指定される [`Constraint.Constant`](xref:Xamarin.Forms.Constraint.Constant*) か、引数を必要とするオーバーロードを使用して指定され `Children.Add` `Func<Rectangle>` ます。 |
| **相対値** | 相対制約は [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`Constraint`](xref:Xamarin.Forms.Constraint) 、 `ConstraintExpression` マークアップ拡張機能によって返されるオブジェクトに添付プロパティを設定することによって指定します。 | 相対制約は [`Constraint`](xref:Xamarin.Forms.Constraint) 、クラスのメソッドによって返されるオブジェクトによって指定され `Constraint` ます。 |

絶対値を使用して制約を指定する方法の詳細については、「 [絶対位置とサイズ](#absolute-positioning-and-sizing)設定」を参照してください。 相対値を使用して制約を指定する方法の詳細については、「 [相対位置とサイズ](#relative-positioning-and-sizing)設定」を参照してください。

C# では、3つのオーバーロードによってに子を追加でき [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) `Add` ます。 最初のオーバーロードでは、 `Expression<Func<Rectangle>>` 子の位置とサイズを指定するためにが必要です。 2番目のオーバーロードでは、、、 `Expression<Func<double>>` `x` `y` 、およびの各引数にオプションのオブジェクトが必要です `width` `height` 。 3番目のオーバーロードでは、、、 `Constraint` `x` `y` 、およびの各引数にオプションのオブジェクトが必要です `width` `height` 。

、、、およびの各メソッドを使用して、の子の位置とサイズを変更することができ [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`SetXConstraint`](xref:Xamarin.Forms.RelativeLayout.SetXConstraint*) [`SetYConstraint`](xref:Xamarin.Forms.RelativeLayout.SetYConstraint*) [`SetWidthConstraint`](xref:Xamarin.Forms.RelativeLayout.SetWidthConstraint*) [`SetHeightConstraint`](xref:Xamarin.Forms.RelativeLayout.SetHeightConstraint*) ます。 これらの各メソッドの最初の引数は子であり、2番目の引数は [`Constraint`](xref:Xamarin.Forms.Constraint) オブジェクトです。 また、メソッドを使用して、 [`SetBoundsConstraint`](xref:Xamarin.Forms.RelativeLayout.SetBoundsConstraint*) 子の位置とサイズを変更することもできます。 このメソッドの最初の引数は子であり、2番目の引数は [`BoundsConstraint`](xref:Xamarin.Forms.BoundsConstraint) オブジェクトです。

## <a name="absolute-positioning-and-sizing"></a>絶対位置とサイズ設定

では、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) デバイスに依存しない単位で指定される絶対値を使用して、子の位置とサイズを設定できます。これにより、子をレイアウトに配置する場所を明示的に定義します。 これを実現するには、子を `Children` のコレクションに追加し、 `RelativeLayout` [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) 各子の、、、およびの [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) 各添付プロパティを絶対位置またはサイズ値に設定します。

> [!WARNING]
> さまざまなデバイスの画面サイズと解像度が異なるため、子の位置やサイズ設定に絶対値を使用すると問題が発生する可能性があります。 そのため、あるデバイス上の画面中央の座標は、他のデバイスでオフセットされる場合があります。

次の XAML は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 絶対値を使用して位置する子を持つを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="RelativeLayoutDemos.Views.StylishHeaderDemoPage"
             Title="Stylish header demo">
    <RelativeLayout Margin="20">
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="0"
                 RelativeLayout.YConstraint="10"
                 RelativeLayout.WidthConstraint="200"
                 RelativeLayout.HeightConstraint="5" />
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="0"
                 RelativeLayout.YConstraint="20"
                 RelativeLayout.WidthConstraint="200"
                 RelativeLayout.HeightConstraint="5" />
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="10"
                 RelativeLayout.YConstraint="0"
                 RelativeLayout.WidthConstraint="5"
                 RelativeLayout.HeightConstraint="65" />
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="20"
                 RelativeLayout.YConstraint="0"
                 RelativeLayout.WidthConstraint="5"
                 RelativeLayout.HeightConstraint="65" />
        <Label Text="Stylish header"
               FontSize="24"
               RelativeLayout.XConstraint="30"
               RelativeLayout.YConstraint="25" />
    </RelativeLayout>
</ContentPage>
```

この例で [`BoxView`](xref:Xamarin.Forms.BoxView) は、および添付プロパティで指定された値を使用して、各オブジェクトの位置が定義されてい [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) ます。 各のサイズは `BoxView` 、および添付プロパティで指定された値を使用して定義され [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) ます。 オブジェクトの位置は [`Label`](xref:Xamarin.Forms.Label) 、および添付プロパティで指定された値を使用しても定義され `XConstraint` `YConstraint` ます。 ただし、サイズの値はに対して指定されていないため、制約はありません `Label` 。 いずれの場合も、絶対値はデバイスに依存しない単位を表します。

次のスクリーンショットは、結果のレイアウトです。

![絶対値を使用して RelativeLayout に配置された子](relativelayout-images/absolute-values.png)

同等の C# コードを次に示します。

```csharp
public class StylishHeaderDemoPageCS : ContentPage
{
    public StylishHeaderDemoPageCS()
    {
        RelativeLayout relativeLayout = new RelativeLayout
        {
            Margin = new Thickness(20)
        };

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(0, 10, 200, 5));

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(0, 20, 200, 5));

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(10, 0, 5, 65));

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(20, 0, 5, 65));

        relativeLayout.Children.Add(new Label
        {
            Text = "Stylish Header",
            FontSize = 24
        }, Constraint.Constant(30), Constraint.Constant(25));

        Title = "Stylish header demo";
        Content = relativeLayout;
    }
}
```

この例では、 [`BoxView`](xref:Xamarin.Forms.BoxView) オブジェクトは、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) `Add` `Expression<Func<Rectangle>>` 各子の位置とサイズを指定するためにを必要とするオーバーロードを使用してに追加されます。 の位置は、 [`Label`](xref:Xamarin.Forms.Label) `Add` オプションの [`Constraint`](xref:Xamarin.Forms.Constraint) オブジェクト (この場合はメソッドによって作成される) を必要とするオーバーロードを使用して定義され [`Constraint.Constant`](xref:Xamarin.Forms.Constraint.Constant*) ます。

> [!NOTE]
> 絶対値を使用するでは、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 子の位置やサイズを指定して、レイアウトの境界内に収まるようにすることができます。

## <a name="relative-positioning-and-sizing"></a>相対位置とサイズ設定

では、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) レイアウトのプロパティまたは兄弟要素を基準とした値を使用して、子の位置とサイズを指定できます。 これを実現するには、子をのコレクションに追加 `Children` `RelativeLayout` し、 [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) 各子の、、 [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) 、およびの [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) 各添付プロパティを、オブジェクトを使用して相対値に設定し [`Constraint`](xref:Xamarin.Forms.Constraint) ます。

制約には、定数、親を基準とした相対値、または兄弟を基準とした相対値を指定できます。 制約の種類は、 [`ConstraintType`](xref:Xamarin.Forms.ConstraintType) 次のメンバーを定義する列挙体によって表されます。

- `RelativeToParent`。親に対して相対的な制約を示します。
- `RelativeToView`。ビュー (または兄弟) に対して相対的な制約を示します。
- `Constant`。定数制約を示します。

### <a name="constraint-markup-extension"></a>制約マークアップ拡張機能

XAML では、 [`Constraint`](xref:Xamarin.Forms.Constraint) マークアップ拡張機能によってオブジェクトを作成でき [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) ます。 このマークアップ拡張機能は、通常、内の子の位置とサイズを [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) その親または兄弟に関連付けるために使用されます。

[`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)クラスは、次のプロパティを定義します。

- [`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant)`double`制約定数値を表す型の。
- [`ElementName`](xref:Xamarin.Forms.ConstraintExpression.ElementName)`string`制約を計算するソース要素の名前を表す、型の。
- [`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor)型の。 `double` これは、ソース要素を基準として、制約されたディメンションをスケーリングする係数を表します。 このプロパティの既定値は1です。
- [`Property`](xref:Xamarin.Forms.ConstraintExpression.Property)`string`制約の計算で使用するソース要素のプロパティの名前を表す、型の。
- [`Type`](xref:Xamarin.Forms.ConstraintExpression.Type)[`ConstraintType`](xref:Xamarin.Forms.ConstraintType)制約の型を表す型の。

Xamarin.Forms マークアップ拡張機能の詳細については、「[XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/index.md)」を参照してください。

次の XAML は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) マークアップ拡張機能によって制約された子を持つを示してい [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="RelativeLayoutDemos.Views.RelativePositioningAndSizingDemoPage"
             Title="RelativeLayout demo">
    <RelativeLayout>
        <BoxView Color="Red"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=Constant, Constant=0}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=Constant, Constant=0}" />
        <BoxView Color="Green"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Constant=-40}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=Constant, Constant=0}" />
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=Constant, Constant=0}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Constant=-40}" />
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Constant=-40}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Silver"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.33}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0.33}"
                 RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.33}"
                 RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0.33}" />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Black"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=X}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=Y}"
                 RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=Width, Factor=0.33}"
                 RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=Height, Factor=0.33}" />
    </RelativeLayout>
</ContentPage>
```

この例で [`BoxView`](xref:Xamarin.Forms.BoxView) は、 [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) プロパティと添付プロパティを設定することによって、各オブジェクトの位置を定義し [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) ます。 最初のは、 `BoxView` `XConstraint` `YConstraint` プロパティと添付プロパティを定数に設定します。これは絶対値です。 残りの `BoxView` オブジェクトはすべて、少なくとも1つの相対値を使用して位置を設定します。 たとえば、黄色のオブジェクトは、 `BoxView` `XConstraint` 添付プロパティをその親の幅 () から40を引いた値に設定し [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) ます。 同様に、これにより、 `BoxView` `YConstraint` 添付プロパティが親の高さから40を引いた値に設定されます。 これにより、 `BoxView` 画面の右下隅に黄色が表示されます。

> [!NOTE]
> [`BoxView`](xref:Xamarin.Forms.BoxView) サイズを指定していないオブジェクトは、自動的に 40 x 40 にサイズ変更され Xamarin.Forms ます。

という名前のシルバーは、 [`BoxView`](xref:Xamarin.Forms.BoxView) `oneThird` 親を基準にして中央揃えで配置されます。 また、親を基準としてサイズが調整され、幅と高さの3分の1になります。 これを実現するには [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) 、 [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) プロパティと添付プロパティを、親の幅 () に0.33 を乗算して設定し [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) ます。 同様に、 [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) との [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) 添付プロパティは、親の高さに0.33 を乗算して設定されます。

黒 [`BoxView`](xref:Xamarin.Forms.BoxView) は、に対して相対的に配置され、サイズが変更され `oneThird` `BoxView` ます。 これを実現するには [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) 、 [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) プロパティと添付プロパティを、 `X` `Y` それぞれ兄弟要素の値と値に設定します。 同様に、そのサイズは、兄弟要素の幅と高さの3分の1に設定されます。 これを実現するには [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) 、 [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) プロパティと添付プロパティ `Width` を `Height` それぞれ兄弟要素の値と値に設定します。このとき、それぞれに0.33 が乗算されます。

次のスクリーンショットは、結果のレイアウトを示しています。

![相対値を使用して RelativeLayout に配置された子](relativelayout-images/relative-values.png)

### <a name="constraint-objects"></a>制約オブジェクト

クラスは、 [`Constraint`](xref:Xamarin.Forms.Constraint) オブジェクトを返す次のパブリック静的メソッドを定義し `Constraint` ます。

- [`Constant`](xref:Xamarin.Forms.Constraint.Constant*)。子をで指定されたサイズに制限 `double` します。
- [`FromExpression`](xref:Xamarin.Forms.Constraint.FromExpression*)。ラムダ式を使用して子を制約します。
- [`RelativeToParent`](xref:Xamarin.Forms.Constraint.RelativeToParent*)。親のサイズに対して相対的に子を制限します。
- [`RelativeToView`](xref:Xamarin.Forms.Constraint.RelativeToView*)。ビューのサイズを基準として子を制限します。

さらに、クラスは、 [`BoundsConstraint`](xref:Xamarin.Forms.BoundsConstraint) 1 つのメソッドを定義し [`FromExpression`](xref:Xamarin.Forms.BoundsConstraint.FromExpression*) ます。これは、 `BoundsConstraint` 子の位置とサイズをで制約するを返し `Expression<Func<Rectangle>>` ます。 このメソッドは、添付プロパティを設定するために使用でき [`BoundsConstraint`](xref:Xamarin.Forms.RelativeLayout.BoundsConstraintProperty) ます。

次の C# コードは、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) オブジェクトによって制約される子を持つを示してい [`Constraint`](xref:Xamarin.Forms.Constraint) ます。

```csharp
public class RelativePositioningAndSizingDemoPageCS : ContentPage
{
    public RelativePositioningAndSizingDemoPageCS()
    {
        RelativeLayout relativeLayout = new RelativeLayout();

        // Four BoxView's
        relativeLayout.Children.Add(
            new BoxView { Color = Color.Red },
            Constraint.Constant(0),
            Constraint.Constant(0));

        relativeLayout.Children.Add(
            new BoxView { Color = Color.Green },
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width - 40;
            }), Constraint.Constant(0));

        relativeLayout.Children.Add(
            new BoxView { Color = Color.Blue },
            Constraint.Constant(0),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height - 40;
            }));

        relativeLayout.Children.Add(
            new BoxView { Color = Color.Yellow },
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width - 40;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height - 40;
            }));

        // Centered and 1/3 width and height of parent
        BoxView silverBoxView = new BoxView { Color = Color.Silver };
        relativeLayout.Children.Add(
            silverBoxView,
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width * 0.33;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height * 0.33;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width * 0.33;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height * 0.33;
            }));

        // 1/3 width and height of previous
        relativeLayout.Children.Add(
            new BoxView { Color = Color.Black },
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.X;
            }),
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.Y;
            }),
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.Width * 0.33;
            }),
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.Height * 0.33;
            }));

        Title = "RelativeLayout demo";
        Content = relativeLayout;
    }
}
```

この例では、、、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 、およびの各 `Add` 引数に対してオプションのオブジェクトを必要とするオーバーロードを使用して、に子を追加し `Constraint` `x` `y` `width` `height` ます。

> [!NOTE]
> [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)相対値を使用するは、子の位置やサイズを指定して、レイアウトの境界内に収まるようにすることができます。

## <a name="related-links"></a>関連リンク

- [RelativeLayout デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-relativelayoutdemos)
- [Xamarin.Forms 添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
- [レイアウトの選択 Xamarin.Forms](choose-layout.md)
- [アプリのパフォーマンスを向上させる Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
