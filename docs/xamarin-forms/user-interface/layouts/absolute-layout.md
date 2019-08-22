---
title: Xamarin.Forms AbsoluteLayout
description: この記事では、Xamarin.Forms AbsoluteLayout クラスを使用して、ピクセル単位で正確 Ui を作成する方法について説明します。 このクラスの位置し、サイズの子要素のサイズと位置、または絶対値によってに比例します。
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 0389587b2abffa751349a66eedc4a3800aa2a99d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888532"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms AbsoluteLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 位置し、サイズ子要素のサイズと位置、または絶対値によってに比例します。 位置指定と相対値または静的な値を使用してサイズと比例子ビューがあり、静的な値を混在させることができます。

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms のレイアウト")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms のレイアウト")

この記事では説明します。

- **[目的](#Purpose)** &ndash;の一般的な使用`AbsoluteLayout`します。
- **[使用状況](#Usage)** &ndash;を使用する`AbsoluteLayout`目的の設計を実現するためにします。
  - **[比例レイアウト](#Proportional_Layouts)** &ndash;比例する方法の値を理解で作業を`AbsoluteLayout`します。
  - **[値を指定する](#Specifying_Values)** &ndash;プロポーショナルと絶対値が指定する方法を理解します。
  - **[比例値](#Proportional_Values)** &ndash;比例する方法の値を理解する機能します。
    - **[絶対値](#Absolute_Values)** &ndash;絶対値のしくみを理解します。

<a name="Purpose" />

## <a name="purpose"></a>目的

配置モデルであるため`AbsoluteLayout`レイアウトでは、比較的簡単に、レイアウトの任意の辺と揃えるか中央揃えで要素を配置します。 比例サイズと位置、内の要素、`AbsoluteLayout`任意のビュー サイズに自動的にスケールできます。 項目の位置のみがサイズではなくをスケールする必要があります、絶対と比例して値を混在させることができます。

`AbsoluteLayout` でした使用 anywhere 要素は、ビュー内で配置する必要があり、要素の端に揃えて配置する場合に特に便利です。

<a name="Usage" />

## <a name="usage"></a>使用法

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>比例レイアウト

`AbsoluteLayout` 要素のアンカーがその要素に対する相対的な配置されているように比例して配置を使用するときに、レイアウトを基準とした、要素が配置されているという一意のアンカー モデルがあります。 絶対配置を使用すると、アンカーは、ビュー内 (0, 0) には。 これは、2 つの重要な影響があります。

- 要素は比例値を使用して画面をオフに配置することはできません。
- 要素は、任意の辺のレイアウトまたはレイアウトまたはデバイスのサイズに関係なく、中央に確実に配置できます。

`AbsoluteLayout`、のような`RelativeLayout`、重なるように要素を配置することができます。

次のスクリーン ショットで、ボックスのアンカーのメモは、白点です。 アンカーと、ボックス間のリレーションシップのことを確認して、レイアウト内を移動します。

![](absolute-layout-images/anchor-start.png "開始時のアンカー")
![](absolute-layout-images/anchor-center.png "センター アンカー")
![](absolute-layout-images/anchor-end.png "最後のアンカー")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>値を指定します。

内にあるビュー、 `AbsoluteLayout` 4 つの値を使用して配置されます。

- **X** &ndash;ビューのアンカーの x (水平) 座標
- **Y** &ndash;ビューのアンカーの y (垂直) の位置
- **幅**&ndash;ビューの幅
- **高さ**&ndash;ビューの高さ

値として設定できる、[比例](#Proportional_Values)値または[絶対](#Absolute_Values)値。

値は、境界とフラグの組み合わせとして指定されます。 `LayoutBounds` [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) 4 つの値から成る: `x`、 `y`、 `width`、`height`します。

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) 値を解釈する方法を指定し、次の定義済みのオプションがあります。

- **None** &ndash;絶対値としてすべての値を解釈します。 これは、レイアウトのフラグが指定されていない場合、既定値です。
- **すべて**&ndash;比例としてすべての値を解釈します。
- **WidthProportional** &ndash;解釈、`Width`絶対値としてプロポーショナルとその他のすべての値として値。
- **HeightProportional** &ndash;のみ高さの値として比例絶対他のすべての値を解釈します。
- **XProportional** &ndash;解釈、`X`絶対値として他のすべての値を扱っているときに、比例として値します。
- **YProportional** &ndash;解釈、`Y`絶対値として他のすべての値を扱っているときに、比例として値します。
- **PositionProportional** &ndash;解釈、`X`と`Y`比例、として値をサイズの値が絶対値として解釈されます。
- **SizeProportional** &ndash;解釈、`Width`と`Height`位置の値は絶対に比例してとして値します。

XAML では、ビュー、レイアウト内の定義の一部としての境界とフラグを設定します。 を使用して、`AbsoluteLayout.LayoutBounds`プロパティ。 境界は、値のコンマ区切りリストとして設定されます。 `X`、 `Y`、 `Width`、および`Height`点で、注文します。 レイアウトを使用して、ビューの宣言でフラグが指定されても、`AbsoluteLayout.LayoutFlags`プロパティ。 フラグは、コンマ区切りのリストを使用して XAML で組み合わせることができますに注意してください。 次に例を示します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "AbsoluteLayout の例")

次の点に注意してください。

- 絶対サイズと位置の値を使用して、中央にラベルが配置されていること。 そのため、iPhone 4 s を中心とした、以下で、大型のデバイスでない中央揃えに表示されます。
- 比例サイズと位置の値を使用して、レイアウトの下部にあるテキストを配置します。 常に、レイアウトの下部中央に表示されますが、そのサイズがレイアウト サイズの大きな拡張されます。
- 次の 3 つの色`BoxView`s は比例位置と絶対サイズを使用して、画面の上、左、および右のエッジに配置されます。

次は、c# で同じレイアウトを実現します。

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```

<a name="Proportional_Values" />

### <a name="proportional-values"></a>比例値

比例した値は、レイアウトとビュー間のリレーションシップを定義します。 この関係は、親のレイアウトの対応する値の比率として子ビューの位置またはスケール値を定義します。 これらの値として表現されます`double`を 0 ~ 1 の値。

位置とサイズ ビュー、レイアウト内に比例した値が使用されます。 そのため、ビューの幅に対する比率として設定すると場合、に、結果の幅の値は乗算割合、`AbsoluteLayout`の幅。 たとえば、`AbsoluteLayout`幅の`500`と比例幅.5、ビューの描画時の幅の設定ビューを 250 (500 x.5 ハンドルとなります

比例した値を使用するには、次のように設定します。 `LayoutBounds` (x, y) を使用して設定し、縦横比と比例サイズは、`LayoutFlags`に`All`します。

で XAML:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

C# の場合:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>絶対値

絶対値では、レイアウト内でビューを配置する場所を明示的に定義します。 プロポーショナルの値とは異なりは絶対値が位置およびサイズ、レイアウトの境界内に適合しない表示可能です。

絶対値を使用して位置を指定できます危険なレイアウトのサイズは認識されていない場合です。 絶対位置を使用する場合、要素 1 つのサイズで画面の中央には他の任意のサイズにオフセットでした。 サポートされているデバイスのさまざまな画面サイズ全体でアプリのテストは重要です。

絶対レイアウトの値を使用する設定`LayoutBounds`(x, y) を使用して座標と、明示的なサイズを設定し、`LayoutFlags`に`None`します。

で XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

C# の場合:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>複雑なレイアウトの調査

レイアウトの各は、特定のレイアウトを作成する長所と短所があります。 この一連のレイアウトの記事では、全体で同じページ レイアウトを次の 3 つの異なるレイアウトを使用して実装されているサンプル アプリが作成されました。

次の XAML を検討してください。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

上記のコードは、次のレイアウトが得られます。

![](absolute-layout-images/abs.png "複雑な AbsoluteLayout")

注意`AbsoluteLayout`s が入れ子になった場合によってはレイアウトを入れ子できるので、同じレイアウト内のすべての要素を表示するよりも簡単です。

## <a name="related-links"></a>関連リンク

- [第 14 章、Xamarin.Forms でモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
