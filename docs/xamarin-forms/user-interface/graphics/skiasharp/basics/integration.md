---
title: "Xamarin.Forms との統合"
description: "タッチや Xamarin.Forms 要素への対応 SkiaSharp グラフィックを作成します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: c938e5ef836904c42f3349c66d48a9b13cb335ca
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="integrating-with-xamarinforms"></a>Xamarin.Forms との統合

_タッチや Xamarin.Forms 要素への対応 SkiaSharp グラフィックを作成します。_

SkiaSharp グラフィックスは、Xamarin.Forms のさまざまな方法で残りの部分と統合できます。 SkiaSharp キャンバスと同じ ページで、Xamarin.Forms 要素、SkiaSharp キャンバスの上にあっても位置 Xamarin.Forms 要素を組み合わせることができます。

![](integration-images/integrationexample.png "スライダーの色を選択します。")

Xamarin.Forms で対話型の SkiaSharp グラフィックスを作成するための別の方法では、タッチ使用します。
2 番目のページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)権利があるプログラム**トグル塗りつぶしをタップして**です。 描画、単純な 2 つの方法は & #x 2014; の円塗りつぶしと塗りつぶし & #x 2014 です。タップで切り替えをします。 [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs)クラスは、ユーザー入力に応答 SkiaSharp グラフィックスを変更する方法を示しています。

このページの`SKCanvasView`でクラスをインスタンス化、 [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml)も、Xamarin.Forms を設定するファイル[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)ビュー。

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

通知、 `skia` XML 名前空間宣言します。

`Tapped`のハンドラーは、`TapGestureRecognizer`オブジェクトでは、ブール値フィールドと呼び出しの値だけを切り替えます、 [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/)メソッドの`SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

呼び出し`InvalidateSurface`への呼び出しを効果的に生成、 `PaintSurface` 、ハンドラーを使用して、`showFill`フィールドを入力するか、または円は入力しません。

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

`StrokeWidth`プロパティが、違いを強調したり 50 に設定されています。 全体の線の幅は、まず内部を描画し、アウトラインも確認できます。 既定では、グラフィックスの図の描画、`PaintSurface`イベント ハンドラーでは、ハンドラーの前で描画されるものが見えにくきます。

**色探索**ページと他の Xamarin.Forms 要素 SkiaSharp グラフィックスも統合する方法を示しています、SkiaSharp で色を定義するための 2 つの代替方法の違いについても示します。 静的な[ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/)メソッドを作成、`SKColor`色合い、鮮やかさの明るさモデルに基づいて、値。

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

静的な[ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/)メソッドを作成、`SKColor`類似の色合いの値の彩度モデルに基づいて、値。

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

どちらの場合、`h`引数の範囲は 0 ~ 360 です。 `s`、 `l`、および`v`引数の範囲は 0 ~ 100 です。 `a`アルファ (透明度) 引数範囲は 0 ~ 255 です。

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml)ファイルには、2 つ作成されます`SKCanvasView`内のオブジェクト、`StackLayout`サイド バイ サイドで`Slider`と`Label`HSL を選択するユーザーに許可するビューとHSV カラー値:

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

2 つ`SKCanvasView`要素が 1 つのセルに`Grid`で、`Label`結果、RGB 色の値を表示するための上部に座ってです。

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs)は比較的簡単に分離コード ファイル。 共有`ValueChanged`の 3 つのハンドラー`Slider`要素だけが無効化両方`SKCanvasView`要素。 `PaintSurface`ハンドラーによって示される色で、キャンバスのオフ、`Slider`要素、およびも設定、`Label`の上に座って、`SKCanvasView`要素。

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

HSL と HSV の両方の色のモデルでは、色合いの値は 0 ~ 360 の範囲を基準となる、色の色合いを示します。 これらは、虹の従来の色: 赤、オレンジ色、黄、緑、青、indigo、紫、赤い円で囲まれた戻る。

モデルでは、HSL、明るさの値 0 は常に黒、および 100 の値が空白では常にします。 鮮やかさの値が 0 の場合は、灰色の網掛けは 0 ~ 100 の間の明るさの値です。 彩度を増やすと、その他の色を追加します。 鮮やかさが 100 で、明るさは 50 (される 1 つのコンポーネントを別の 3 番目、0 ~ 255 0 に等しい 255 と同じで、RGB 値)、純粋な色が発生します。

HSV モデルでは、純粋な色は、彩度と値の両方が 100 を発生します。 値が 0、その他の設定に関係なく、色は黒です。 鮮やかさが 0 と値の範囲は 0 ~ 100 の場合は、灰色の網かけにあります。

2 つのモデルの把握する最善の方法を試してみるが自分で。

[![](integration-images/colorexplore-large.png "トリプル ページのスクリーン ショット、色の調査")](integration-images/colorexplore-small.png#lightbox "トリプル ページのスクリーン ショット、色の表示")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
