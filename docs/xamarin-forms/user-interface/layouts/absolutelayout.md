---
title: Xamarin.FormsAbsoluteLayout
description: Xamarin.FormsAbsoluteLayout は、明示的な値、またはレイアウトのサイズに比例した値を使用して要素の位置とサイズを指定するために使用されます。
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 696429e04775640d46add77ec6a4bbf6e69f675b
ms.sourcegitcommit: c3329ab25d377907d8804cdd5e26dc84a274f39c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88134158"
---
# <a name="no-locxamarinforms-absolutelayout"></a>Xamarin.FormsAbsoluteLayout

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-absolutelayoutdemos)

[![::: no-loc (Xamarin. Forms)::: AbsoluteLayout](absolutelayout-images/layouts.png)](absolutelayout-images/layouts-large.png#lightbox)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)は、明示的な値を使用して子の位置とサイズを指定するために使用されます。 位置は、デバイスに依存しない単位で、の左上隅を基準として、子の左上隅によって指定され `AbsoluteLayout` ます。 `AbsoluteLayout` では、比例の配置とサイズ変更の機能も実装されています。 また、他のいくつかのレイアウトクラスとは異なり、で `AbsoluteLayout` は、重なり合うように子を配置できます。

は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 子にサイズを設定できる場合や、要素のサイズが他の子の位置に影響を与えない場合にのみ使用される特殊な目的のレイアウトと見なされる必要があります。

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)クラスは、次のプロパティを定義します。

- [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)型の [`Rectangle`](xref:Xamarin.Forms.Rectangle) 。これは、子の位置とサイズを表す添付プロパティです。 このプロパティの既定値は、(0, 0, AutoSize, AutoSize) です。
- [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)型の。 [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) これは、子の位置とサイズを指定するために使用されるレイアウト境界のプロパティが比例して解釈されるかどうかを示す添付プロパティです。 このプロパティの既定値は `AbsoluteLayoutFlags.None` です。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、プロパティは、データバインディングのターゲットとスタイルを設定できます。 添付プロパティの詳細については、「 [ Xamarin.Forms 添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください。

クラスは、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) `Layout<T>` 型のプロパティを定義するクラスから派生 `Children` `IList<T>` します。 `Children`プロパティは `ContentProperty` クラスのである `Layout<T>` ため、XAML から明示的に設定する必要はありません。

> [!TIP]
> レイアウトの最適なパフォーマンスを得るには、「[レイアウトのパフォーマンスを最適化](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)する」のガイドラインに従ってください。

## <a name="position-and-size-children"></a>子の位置とサイズを変更する

の子の位置とサイズは、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 絶対値または比例値を使用して、各子の添付プロパティを設定することによって定義されます。 位置のスケールを設定するときに、絶対値と比例値を子に混在させることができますが、サイズは固定したままにしておく必要があります。 絶対値の詳細については、「[絶対位置とサイズ](#absolute-positioning-and-sizing)設定」を参照してください。 比例値の詳細については、「[比例配置とサイズ](#proportional-positioning-and-sizing)設定」を参照してください。

[`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)添付プロパティは、絶対値または比例値が使用されているかどうかに関係なく、2つの形式を使用して設定できます。

- `x, y`. この形式では、 `x` との値は、親を基準とした、 `y` 子の左上隅の位置を示します。 子は制約されません。
- `x, y, width, height`. この形式では、 `x` `y` 値と値は親を基準とした子の左上隅の位置を示し、との `width` 値は `height` 子のサイズを示します。

子のサイズが水平または垂直のどちらであるか、またはその両方を指定するには、 `width` プロパティに値または値を設定し `height` [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) ます。 ただし、このプロパティを使用すると、レイアウトエンジンによって追加のレイアウト計算が実行されるため、アプリケーションのパフォーマンスに悪影響を及ぼす可能性があります。

> [!IMPORTANT]
> [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティは、の子には影響しません [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 。

## <a name="absolute-positioning-and-sizing"></a>絶対位置とサイズ設定

既定では、は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) デバイスに依存しない単位で指定された絶対値を使用して、子の位置とサイズを設定します。これにより、ビューをレイアウト内に配置する場所を明示的に定義します。 これを実現するには、子を `Children` のコレクションに追加 `AbsoluteLayout` し、 [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 各子の添付プロパティを絶対位置またはサイズ値に設定します。

> [!WARNING]
> さまざまなデバイスの画面サイズと解像度が異なるため、子の位置やサイズ設定に絶対値を使用すると問題が発生する可能性があります。 そのため、あるデバイス上の画面中央の座標は、他のデバイスでオフセットされる場合があります。

次の XAML は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 絶対値を使用して位置する子を持つを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="AbsoluteLayoutDemos.Views.StylishHeaderDemoPage"
             Title="Stylish header demo">
    <AbsoluteLayout Margin="20">
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
        <Label Text="Stylish Header"
               FontSize="24"
               AbsoluteLayout.LayoutBounds="30, 25" />
    </AbsoluteLayout>
</ContentPage>
```

この例で [`BoxView`](xref:Xamarin.Forms.BoxView) は、添付プロパティで指定されている最初の2つの絶対値を使用して、各オブジェクトの位置が定義されてい [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) ます。 各のサイズは、 `BoxView` 3 番目と3番目の値を使用して定義されます。 オブジェクトの位置は、 [`Label`](xref:Xamarin.Forms.Label) 添付プロパティで指定されている2つの絶対値を使用して定義され `AbsoluteLayout.LayoutBounds` ます。 サイズの値はに対して指定されていないため、制約はありません `Label` 。 いずれの場合も、絶対値はデバイスに依存しない単位を表します。

次のスクリーンショットは、結果のレイアウトを示しています。

![絶対値を使用して AbsoluteLayout に配置された子](absolutelayout-images/absolute-values.png)

同等の C# コードを次に示します。

```csharp
public class StylishHeaderDemoPageCS : ContentPage
{
    public StylishHeaderDemoPageCS()
    {
        AbsoluteLayout absoluteLayout = new AbsoluteLayout
        {
            Margin = new Thickness(20)
        };

        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver,
        }, new Rectangle(0, 10, 200, 5));
        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, new Rectangle(0, 20, 200, 5));
        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, new Rectangle(10, 0, 5, 65));
        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, new Rectangle(20, 0, 5, 65));

        absoluteLayout.Children.Add(new Label
        {
            Text = "Stylish Header",
            FontSize = 24
        }, new Point(30,25));                    

        Title = "Stylish header demo";
        Content = absoluteLayout;
    }
}
```

この例では、各の位置とサイズ [`BoxView`](xref:Xamarin.Forms.BoxView) がオブジェクトを使用して定義されてい [`Rectangle`](xref:Xamarin.Forms.Rectangle) ます。 の位置は、 [`Label`](xref:Xamarin.Forms.Label) オブジェクトを使用して定義され [`Point`](xref:Xamarin.Forms.Point) ます。

C# では、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) `Children` メソッドを使用して、コレクションに追加された後にの子の位置とサイズを設定することもでき [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds*) ます。 このメソッドの最初の引数は子であり、2番目の引数は [`Rectangle`](xref:Xamarin.Forms.Rectangle) オブジェクトです。

> [!NOTE]
> 絶対値を使用するでは、子の位置やサイズを指定して、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) レイアウトの境界内に収まるようにすることができます。

## <a name="proportional-positioning-and-sizing"></a>比例配置とサイズ設定

では、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 比例値を使用して子の位置とサイズを指定できます。 これを実現するには、子をのコレクションに追加 `Children` `AbsoluteLayout` し、 [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 各子の添付プロパティを、0-1 の範囲の比例位置やサイズ値に設定します。 位置とサイズの値は、各子の添付プロパティを設定することによって比例 [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) します。

[`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)型の添付プロパティを使用すると、 [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) 子のレイアウト範囲の位置とサイズの値がのサイズに比例していることを示すフラグを設定でき `AbsoluteLayout` ます。 子をレイアウトするときに、 `AbsoluteLayout` 位置とサイズの値を任意のデバイスサイズに合わせてスケーリングします。

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)列挙体は、次のメンバーを定義します。

- `None`は、値が絶対として解釈されることを示します。 これは添付プロパティの既定値です [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) 。
- `XProportional`は、値が `x` 比例して解釈され、他のすべての値を絶対値として扱うことを示します。
- `YProportional`は、値が `y` 比例して解釈され、他のすべての値を絶対値として扱うことを示します。
- `WidthProportional`は、値が `width` 比例して解釈され、他のすべての値を絶対値として扱うことを示します。
- `HeightProportional`は、値が `height` 比例して解釈され、他のすべての値を絶対値として扱うことを示します。
- `PositionProportional`は、との `x` `y` 値が比例して解釈されることを示し、サイズの値は絶対値として解釈されます。
- `SizeProportional`は、との `width` `height` 値が比例して解釈され、位置の値は絶対として解釈されることを示します。
- `All`は、すべての値が比例して解釈されることを示します。

> [!TIP]
> 列挙は列挙体で、列挙体の [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) `Flags` メンバーを組み合わせることができます。 これは XAML で、コンマ区切りのリストと、ビットごとの OR 演算子を使用した C# で実現されます。

たとえば、フラグを使用 `SizeProportional` して、子の幅を0.25 に、高さを0.1 に設定した場合、子はの幅の1分の1になり、高さが 1 ~ 10 に設定され [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) ます。 `PositionProportional`フラグも似ています。 位置 (0, 0) は、子を左上隅に配置します。また、(1, 1) の位置によって子が右下隅に配置され、(0.5, 0.5) の位置が内の子を中心とし `AbsoluteLayout` ます。

次の XAML は、 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 比例値を使用して配置された子を持つを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="AbsoluteLayoutDemos.Views.ProportionalDemoPage"
             Title="Proportional demo">
    <AbsoluteLayout>
        <BoxView Color="Blue"
                 AbsoluteLayout.LayoutBounds="0.5,0,100,25"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <BoxView Color="Green"
                 AbsoluteLayout.LayoutBounds="0,0.5,25,100"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <BoxView Color="Red"
                 AbsoluteLayout.LayoutBounds="1,0.5,25,100"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <BoxView Color="Black"
                 AbsoluteLayout.LayoutBounds="0.5,1,100,25"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <Label Text="Centered text"
               AbsoluteLayout.LayoutBounds="0.5,0.5,110,25"
               AbsoluteLayout.LayoutFlags="PositionProportional" />
    </AbsoluteLayout>
</ContentPage>
```

この例では、各子は比例値を使用して配置されますが、絶対値を使用してサイズが設定されます。 これを実現するには、 [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) 各子の添付プロパティをに設定し `PositionProportional` ます。 各子について、添付プロパティで指定されている最初の2つの値は、 [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 比例値を使用して位置を定義します。 各子のサイズは、デバイスに依存しない単位を使用して、3番目と3番目の絶対値で定義されます。

次のスクリーンショットは、結果のレイアウトを示しています。

![プロポーショナル positino 値を使用して AbsoluteLayout に配置された子](absolutelayout-images/proportional-position.png)

同等の C# コードを次に示します。

```csharp
public class ProportionalDemoPageCS : ContentPage
{
    public ProportionalDemoPageCS()
    {
        BoxView blue = new BoxView { Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds(blue, new Rectangle(0.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags(blue, AbsoluteLayoutFlags.PositionProportional);

        BoxView green = new BoxView { Color = Color.Green };
        AbsoluteLayout.SetLayoutBounds(green, new Rectangle(0, 0.5, 25, 100));
        AbsoluteLayout.SetLayoutFlags(green, AbsoluteLayoutFlags.PositionProportional);

        BoxView red = new BoxView { Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds(red, new Rectangle(1, 0.5, 25, 100));
        AbsoluteLayout.SetLayoutFlags(red, AbsoluteLayoutFlags.PositionProportional);

        BoxView black = new BoxView { Color = Color.Black };
        AbsoluteLayout.SetLayoutBounds(black, new Rectangle(0.5, 1, 100, 25));
        AbsoluteLayout.SetLayoutFlags(black, AbsoluteLayoutFlags.PositionProportional);

        Label label = new Label { Text = "Centered text" };
        AbsoluteLayout.SetLayoutBounds(label, new Rectangle(0.5, 0.5, 110, 25));
        AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.PositionProportional);

        Title = "Proportional demo";
        Content = new AbsoluteLayout
        {
            Children = { blue, green, red, black, label }
        };
    }
}
```

この例では、各子の位置とサイズがメソッドで設定されてい [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds*) ます。 メソッドの最初の引数は子であり、2番目の引数は [`Rectangle`](xref:Xamarin.Forms.Rectangle) オブジェクトです。 各子の位置は比例値で設定され、各子のサイズはデバイスに依存しない単位を使用して絶対値で設定されます。

> [!NOTE]
> [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)比例値を使用するは、0-1 の範囲外の値を使用してレイアウトの境界内に収まるように、子の位置やサイズを指定できます。

## <a name="related-links"></a>関連リンク

- [AbsoluteLayout デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-absolutelayoutdemos)
- [Xamarin.Forms添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [レイアウトの選択 Xamarin.Forms](choose-layout.md)
- [アプリのパフォーマンスを向上させる Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
