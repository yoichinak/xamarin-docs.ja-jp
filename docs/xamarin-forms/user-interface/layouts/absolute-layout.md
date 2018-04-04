---
title: AbsoluteLayout
description: AbsoluteLayout を使用すると、ピクセルの完全な Ui を作成できます。
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: d07026fbcc43a43a9f26d85ad15d5a4e3165e2ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) 配置し、子要素のサイズと位置または絶対値によって比例のサイズします。 静的な値を混在させることができます、位置指定と相対値または静的な値を使用してサイズおよび比例して、子ビューがあります。

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms レイアウト")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms レイアウト")

この記事を取り上げます。

- **[目的](#Purpose)** &ndash;の一般的な使用`AbsoluteLayout`です。
- **[使用状況](#Usage)** &ndash;の使用方法`AbsoluteLayout`目的の設計を実現するためにします。
  - **[比例レイアウト](#Proportional_Layouts)** &ndash;方法比例値理解で職場、`AbsoluteLayout`です。
  - **[値を指定する](#Specifying_Values)** &ndash;比例と絶対パスの値を指定する方法を理解します。
  - **[相対値を](#Proportional_Values)** &ndash;方法に比例した値を理解して動作します。
    - **[絶対値](#Absolute_Values)** &ndash;絶対値のしくみを理解します。

<a name="Purpose" />

## <a name="purpose"></a>目的

配置モデルにより`AbsoluteLayout`レイアウトでは、要素を配置できるようにして、レイアウトの任意の辺と揃えるまたは中央揃えを比較的簡単です。 プロポーショナル サイズと位置内の要素で、`AbsoluteLayout`任意のビュー サイズに自動的に拡張できます。 アイテムの位置だけがサイズではなくを拡張する必要があります、絶対と割合の値を混在させることができます。

`AbsoluteLayout` 使用できます anywhere 要素をビュー内に配置する必要があるし、要素の端に揃えて配置する場合に特に便利です。

<a name="Usage" />

## <a name="usage"></a>使用法

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>プロポーショナル レイアウト

`AbsoluteLayout` それにより、要素のアンカーが配置されているその要素に相対的な比例して配置を使用するときに、レイアウトの基準とした要素は、一意のアンカー モデルを持ちます。 絶対位置を使用すると、アンカーは、ビュー内で (0, 0) です。 これは、2 つの重要な影響があります。

- 要素は比例値を使用して画面をオフに配置することはできません。
- 要素は、レイアウトまたはデバイスのサイズに関係なく、センター、またはレイアウトの任意の辺に確実に配置できます。

`AbsoluteLayout`、のような`RelativeLayout`、が重複しているため、要素を配置することができます。

次のスクリーン ショットで、ボックスのアンカー メモは、白点です。 レイアウトを移動するときに、アンカーおよびボックス間のリレーションシップを確認してください。

![](absolute-layout-images/anchor-start.png "開始時にアンカー")
![](absolute-layout-images/anchor-center.png "センターでアンカー")
![](absolute-layout-images/anchor-end.png "最後のアンカー")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>値を指定します。

ビュー内で、 `AbsoluteLayout` 4 つの値を使用して配置します。

- **X** &ndash;ビューのアンカーの x (水平) 座標
- **Y** &ndash;ビューのアンカーの y (垂直) 位置
- **幅**&ndash;ビューの幅
- **高さ**&ndash;ビューの高さ

これらの値の各として設定できます。、[比例](#Proportional_Values)値または[絶対](#Absolute_Values)値。

値は、境界とフラグの組み合わせとして指定されます。 `LayoutBounds` [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) 4 つの値から成る: `x`、 `y`、 `width`、`height`です。

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) 値の解釈方法を指定し、次の定義済みオプションがあります。

- **None** &ndash;ですべての値は絶対パスとして解釈します。 これは、レイアウト フラグが指定されていない場合、既定値です。
- **すべて**&ndash;比例としてすべての値を解釈します。
- **WidthProportional** &ndash;解釈、`Width`比例し、その他のすべての値と絶対値としての値。
- **HeightProportional** &ndash;のみ高さの値として比例絶対他のすべての値を解釈します。
- **XProportional** &ndash;解釈、`X`比例して、他のすべての値を絶対パスとして扱いながら値します。
- **YProportional** &ndash;解釈、`Y`比例して、他のすべての値を絶対パスとして扱いながら値します。
- **PositionProportional** &ndash;解釈、`X`と`Y`比例、として値をサイズの値が絶対値として解釈されます。
- **SizeProportional** &ndash;解釈、`Width`と`Height`位置の値は絶対に値としての比例します。

Xaml では、レイアウト内のビューの定義の一部としての境界とフラグを設定します。 を使用して、`AbsoluteLayout.LayoutBounds`プロパティです。 境界は、値のコンマ区切りリストとして設定されて`X`、 `Y`、 `Width`、および`Height`点で、注文します。 使用して、レイアウト ビューの宣言でフラグが指定されても、`AbsoluteLayout.LayoutFlags`プロパティです。 フラグは、コンマ区切りのリストを使用して XAML で組み合わせることができますに注意してください。 次に例を示します。

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

![](absolute-layout-images/exploration.png "AbsoluteLayout 例")

次の点に注意してください。

- 絶対サイズと位置の値を使用して、中央にラベルが配置されます。 そのため、iPhone 4 秒を中心としたと低いが大規模なデバイスでない中央揃えに表示されます。
- レイアウトの下部にあるテキストは比例サイズと位置の値を使用して配置されます。 常に、レイアウトの下部中央に表示されますが、レイアウトのサイズの大きなサイズが増大します。
- 次の 3 つの色`BoxView`s は比例位置および絶対サイズを使用して、画面の上、左、および右のエッジに配置します。

C# の場合、同じレイアウトを次に実現します。

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

### <a name="proportional-values"></a>相対値

比例値は、レイアウトは、ビュー間のリレーションシップを定義します。 このリレーションシップは、親のレイアウトの対応する値の比率として子ビューの位置またはスケール値を定義します。 これらの値として表されます`double`0 ~ 1 の値とします。

位置とサイズ ビュー、レイアウト内に比例した値が使用されます。 そのため、ビューの幅が、割合として設定されている場合に、結果の幅の値は比率を掛けた値によって、`AbsoluteLayout`の幅。 たとえば、`AbsoluteLayout`幅の`500`.5 は、表示されるビューの幅に比例幅に設定することが可能 250 (500 x.5). と

使用するには相対値を次のように設定します。 `LayoutBounds` (x, y) を使用して縦横比比例、およびサイズを設定し、`LayoutFlags`に`All`です。

XAML:

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

絶対値では、レイアウト内でのビューの位置を明示的に定義します。 割合の値とは異なり絶対値が位置およびサイズ、レイアウトの境界内に適合しない表示可能です。

絶対値の位置を決定を使用すると危険なレイアウトのサイズは認識されていない場合。 絶対位置を使用する場合に 1 つのサイズ、画面の中央に要素が他のサイズを問わず、オフセットでした。 これが、サポートされているデバイスのさまざまな画面サイズ間でアプリをテストする重要です。

絶対レイアウトの値を使用する設定`LayoutBounds`(x, y) を使用して座標と、明示的なサイズを設定し、`LayoutFlags`に`None`です。

XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

C# の場合:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>複雑なレイアウトを調べる

レイアウトの各は、特定のレイアウトを作成するの長所と短所があります。 この一連のレイアウトの記事では、全体で同じページ レイアウトを次の 3 つの異なるレイアウトを使用して実装されているサンプル アプリケーションが作成されました。

次の XAML を考慮してください。

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

なお、Windows Phone をボタンを表示する方法の違いにより、Windows Phone のスクリーン ショットで boxviews 円のいくつか置き換えられています。

注意して`AbsoluteLayout`s が入れ子になった場合によってはレイアウトを入れ子が許容されるので、同じレイアウト内のすべての要素を表示するよりも簡単です。



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms、章 14 を使用したモバイル アプリの作成](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
