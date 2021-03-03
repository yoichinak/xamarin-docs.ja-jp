---
title: との統合 Xamarin.Forms
description: この記事では、タッチ要素と要素に応答する SkiaSharp グラフィックスを作成する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8288ad3238babe1ce6abb6d9517cdae71373d27c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366621"
---
# <a name="integrating-with-xamarinforms"></a>との統合 Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_タッチおよび要素に応答する SkiaSharp グラフィックスを作成する Xamarin.Forms_

SkiaSharp グラフィックスは、いくつかの方法で、の残りの部分と統合でき Xamarin.Forms ます。 同じページ上の SkiaSharp キャンバスと Xamarin.Forms 要素を結合し、SkiaSharp canvas の上に要素を配置することもでき Xamarin.Forms ます。

![スライダーを使用して色を選択する](integration-images/integrationexample.png)

で対話型の SkiaSharp グラフィックスを作成するもう1つの方法は、タッチを使用すること Xamarin.Forms です。
[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムの2番目のページには、[**塗りつぶし** の設定/解除] があります。 この例では、塗りつぶしのない2つの方法で、 &mdash; &mdash; タップによって塗りつぶしが切り替わります。 クラスは、 [`TapToggleFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) ユーザー入力に応じて SkiaSharp グラフィックスを変更する方法を示しています。

このページでは、 `SKCanvasView` クラスは [TapToggleFill](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) ファイルでインスタンス化されます。このファイルは Xamarin.Forms ビューにもを設定します [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

XML 名前空間の宣言に注意して `skia` ください。

`Tapped`オブジェクトのハンドラーは、 `TapGestureRecognizer` ブール型フィールドの値を単純に切り替え、 [`InvalidateSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface) のメソッドを呼び出し `SKCanvasView` ます。

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

を呼び出すと、 `InvalidateSurface` ハンドラーへの呼び出しが実質的に生成されます。このハンドラーは、 `PaintSurface` 次のようにフィールドを使用して円を塗りつぶし `showFill` ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth`差分を強調するには、プロパティが50に設定されています。 線の幅全体を表示するには、最初に内部を描き、次にアウトラインを描画します。 既定では、イベントハンドラーの後の方で描画されるグラフィックスの数値は、ハンドラーの前に描画された `PaintSurface` ものに見えません。

[ **カラー探索** ] ページでは、SkiaSharp グラフィックスを他の要素と統合する方法についても説明 Xamarin.Forms します。また、SkiaSharp で色を定義するための2つの代替方法の違いについても説明します。 静的メソッドは、 [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl(System.Single,System.Single,System.Single,System.Byte)) `SKColor` 色合いと鮮やかさの輝度モデルに基づいて値を作成します。

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

静的メソッドは、 [`SKColor.FromHsv`](xref:SkiaSharp.SKColor.FromHsv(System.Single,System.Single,System.Single,System.Byte)) 同様の色合いと鮮やかさの値の `SKColor` モデルに基づいて値を作成します。

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

どちらの場合も、 `h` 引数の範囲は 0 ~ 360 です。 `s`、、およびの各引数は、0 ~ 100 の `l` `v` 範囲です。 `a`(アルファまたは不透明度) 引数は 0 ~ 255 の範囲です。

[**ColorExplorePage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml)ファイルは、 `SKCanvasView` `StackLayout` `Slider` `Label` ユーザーが HSL と HSV の色の値を選択できるようにするために、とビューを使用して、2つのオブジェクトをサイドバイサイドで作成します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

2つの要素は、結果として得られる `SKCanvasView` `Grid` RGB カラー値を表示するために、上にあるを持つ1つのセルにあり `Label` ます。

[**ColorExplorePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs)分離コードファイルは比較的単純です。 `ValueChanged`3 つの要素の共有ハンドラーは、両方の要素を無効にする `Slider` だけ `SKCanvasView` です。 これらのハンドラーは、 `PaintSurface` 要素によって示される色でキャンバスをクリア `Slider` し、要素の上にあるを設定し `Label` `SKCanvasView` ます。

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

HSL と HSV の両方のカラーモデルでは、色合いの値は 0 ~ 360 の範囲で、最も重要な色の色合いを示します。 これらは従来のレインボー色です。赤、オレンジ、黄、緑、青、indigo、紫、赤の円に戻ります。

HSL モデルでは、明るさに0を指定した場合は常に黒、100の値は常に白になります。 鮮やかさの値が0の場合、0 ~ 100 の明るさの値は灰色の網掛けになります。 鮮やかさを増やすと、より多くの色が追加されます。 純粋な色 (1 つのコンポーネントが255と等しい RGB 値、もう1つは0に等しい、3番目の値は0から 255) は、鮮やかさが100、輝度が50の場合に発生します。

HSV モデルでは、鮮やかさと値の両方が100の場合、純粋な色が生成されます。 値が0の場合、他の設定に関係なく、色は黒です。 灰色の網掛けは、鮮やかさが0で、値の範囲が 0 ~ 100 の場合に発生します。

しかし、2つのモデルの感覚を得る最良の方法は、自分で試してみることです。

[![色の調査ページのトリプルスクリーンショット](integration-images/colorexplore-large.png)](integration-images/colorexplore-small.png#lightbox "色の調査ページのトリプルスクリーンショット")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)