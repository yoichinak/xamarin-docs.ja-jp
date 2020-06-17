---
title: " Xamarin.Forms AbsoluteLayout" description: "この記事では、AbsoluteLayout クラスを使用して Xamarin.Forms ピクセルに最適な ui を作成する方法について説明します。 このクラスは、独自のサイズと位置または絶対値に比例して子要素を配置し、サイズを調整します。
ms. 製品: xamarin ms. assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 11/25/2015 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-absolutelayout"></a>Xamarin.FormsAbsoluteLayout

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)子要素のサイズと位置または絶対値に比例して、子要素の位置とサイズを調整します。 子ビューは、比例値または静的な値を使用して配置およびサイズ設定できます。また、比例値と静的な値を混在させることができます。

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

この記事では、次の内容を取り上げます。

- **[目的](#purpose)** &ndash;の一般的な使用方法 `AbsoluteLayout` 。
- **[使用法](#usage)** &ndash;を使用し `AbsoluteLayout` て目的のデザインを実現する方法について説明します。
  - **[プロポーショナルレイアウト](#proportional-layouts)** &ndash;での比例値の動作について説明 `AbsoluteLayout` します。
  - **[値](#specifying-values)** &ndash; の指定比例値と絶対値を指定する方法について説明します。
  - **[比例値](#proportional-values)** &ndash;比例値がどのように機能するかを理解します。
    - **[絶対値](#absolute-values)** &ndash;絶対値がどのように機能するかを理解します。

## <a name="purpose"></a>目的

の配置モデルにより、レイアウトで `AbsoluteLayout` は、レイアウトの任意の辺に合わせて、または中央揃えになるように、要素を配置することが比較的簡単になります。 比例するサイズと位置を使用すると、内の要素 `AbsoluteLayout` は任意のビューサイズに自動的にスケーリングできます。 サイズ以外の位置だけをスケールする必要がある項目の場合、絶対値と比例値を混在させることができます。

`AbsoluteLayout`任意の場所の要素をビュー内に配置する必要があり、要素を端に配置するときに特に便利です。

## <a name="usage"></a>使用

### <a name="proportional-layouts"></a>プロポーショナルレイアウト

`AbsoluteLayout`には、相対的な配置が使用されている場合に要素がレイアウトに対して相対的に配置されるため、要素のアンカーが要素の相対位置に配置される一意のアンカーモデルがあります。 絶対配置を使用する場合、アンカーはビュー内の (0, 0) の位置にあります。 これには、次の2つの重要な結果があります。

- 比例値を使用して、要素を画面外に配置することはできません。
- レイアウトまたはデバイスのサイズに関係なく、要素はレイアウトの任意の側、または中央に配置することができます。

`AbsoluteLayout`と同様に、で `RelativeLayout` は要素を重ねることができます。

次のスクリーンショットでは、ボックスのアンカーは白い点を示しています。 アンカーとボックスがレイアウト内を移動する間の関係を確認します。

![](absolute-layout-images/anchor-start.png "Anchor at Start")
![](absolute-layout-images/anchor-center.png "Anchor at Center")
![](absolute-layout-images/anchor-end.png "Anchor at End")

### <a name="specifying-values"></a>値の指定

内のビュー `AbsoluteLayout` は、次の4つの値を使用して配置されます。

- **X** &ndash; ビューのアンカーの x (水平) 位置
- **Y** &ndash; ビューのアンカーの y (垂直) 位置
- **幅** &ndash;ビューの幅
- **高さ** &ndash;ビューの高さ

これらの各値は、[比例](#proportional-values)値または[絶対値](#absolute-values)として設定できます。

値は、境界とフラグの組み合わせとして指定されます。 `LayoutBounds`は、、、 [`Rectangle`](xref:Xamarin.Forms.Rectangle) 、の4つの値で構成されるです `x` 。 `y` `width` `height`

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)値を解釈する方法を指定します。定義済みのオプションは次のとおりです。

- **なし** &ndash;すべての値を絶対値として解釈します。 これは、レイアウトフラグが指定されていない場合の既定値です。
- **すべて** &ndash;すべての値を比例して解釈します。
- **幅の比例** &ndash;値を `Width` 比例して、その他のすべての値を絶対値として解釈します。
- **高さの比例** &ndash;高さの値のみを他のすべての値と比例して解釈します。
- **Xproportional** &ndash;は、 `X` 他のすべての値を絶対値として扱いながら、値を比例して解釈します。
- **Yproportional** &ndash;は、 `Y` 他のすべての値を絶対値として扱いながら、値を比例して解釈します。
- **Positionproportional** &ndash;では、 `X` との `Y` 値は比例して解釈されますが、サイズの値は絶対値として解釈されます。
- **Sizeproportional** &ndash;位置の `Width` `Height` 値が絶対値の場合、との値は比例して解釈されます。

XAML では、境界とフラグは、プロパティを使用して、レイアウト内のビューの定義の一部として設定され `AbsoluteLayout.LayoutBounds` ます。 境界は、値、、、、およびのコンマ区切りのリストとして、この順序で設定され `X` `Y` `Width` `Height` ます。 また、プロパティを使用して、レイアウト内のビューの宣言でフラグを指定することもでき `AbsoluteLayout.LayoutFlags` ます。 フラグは、コンマ区切りのリストを使用して XAML で組み合わせることができます。 次に例を示します。

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

![](absolute-layout-images/exploration.png "AbsoluteLayout Examples")

次のことを考慮してください。

- 中央のラベルは、絶対サイズと位置の値を使用して配置されます。 そのため、iPhone 4S と lower の中央に表示されますが、大規模なデバイスには集中していません。
- レイアウトの下部にあるテキストは、比例したサイズと位置の値を使用して配置されます。 レイアウトの一番下の中央に表示されますが、サイズが大きくなるとレイアウトサイズが大きくなります。
- 3色 `BoxView` の s は、比例位置と絶対サイズを使用して、画面の上端、左端、右端に配置されます。

C# では、次のように同じレイアウトが実現されます。

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

### <a name="proportional-values"></a>比例値

比例値は、レイアウトとビューの間のリレーションシップを定義します。 このリレーションシップは、子ビューの位置またはスケール値を、親レイアウトの対応する値の比率として定義します。 これらの値は `double` 、0 ~ 1 の値を持つ s として表現されます。

プロポーショナル値は、レイアウト内のビューの位置とサイズを指定するために使用されます。 そのため、ビューの幅が比率として設定されている場合、結果の幅の値は、の幅を乗算した比率になり `AbsoluteLayout` ます。 たとえば、幅がで、 `AbsoluteLayout` `500` ビューに比例幅 .5 が設定されている場合、表示されるビューの幅は 250 (500 x 5) になります。

比例値を使用するには、 `LayoutBounds` (x, y) の比率とプロポーショナルサイズを使用して設定し、 `LayoutFlags` をに設定し `All` ます。

XAML の場合:

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

### <a name="absolute-values"></a>絶対値

絶対値は、レイアウト内のビューの位置を明示的に定義します。 比例値とは異なり、絶対値は、レイアウトの境界内に収まっていないビューを配置したり、サイズを変更したりすることができます。

レイアウトのサイズが不明な場合、配置に絶対値を使用することは危険になる可能性があります。 絶対位置を使用する場合、1つのサイズの画面の中央にある要素を他のサイズでオフセットできます。 サポートされているデバイスのさまざまな画面サイズでアプリケーションをテストすることが重要です。

絶対レイアウト値を使用するには、 `LayoutBounds` (x, y) 座標と明示的なサイズを使用してを設定し、 `LayoutFlags` をに設定し `None` ます。

XAML の場合:

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

各レイアウトには、特定のレイアウトを作成するための長所と短所があります。 この一連のレイアウト記事では、3つの異なるレイアウトを使用して実装された同じページレイアウトでサンプルアプリが作成されています。

次の XAML を考えてみましょう。

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

上記のコードでは、次のレイアウトが生成されます。

![](absolute-layout-images/abs.png "Complex AbsoluteLayout")

が入れ子になっていることに注意してください。入れ子になったレイアウトは、 `AbsoluteLayout` 同じレイアウト内のすべての要素を表示するよりも簡単な場合があるためです。

## <a name="related-links"></a>関連リンク

- [第14章で Mobile Apps を作成する Xamarin.Forms](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble の例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
