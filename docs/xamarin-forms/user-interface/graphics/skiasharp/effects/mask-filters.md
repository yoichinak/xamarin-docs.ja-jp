---
title: SkiaSharp mask フィルター
description: マスクフィルターを使用して、ぼかしやその他のアルファ効果を作成する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 940422A1-8BC0-4039-8AD7-26C61320F858
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 827b5618dce019e2dedb773f270fe1090da5d616
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562510"
---
# <a name="skiasharp-mask-filters"></a>SkiaSharp mask フィルター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

マスクフィルターは、グラフィカルオブジェクトのジオメトリおよびアルファチャネルを操作する効果です。 マスクフィルターを使用するには、 [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) のプロパティを、 `SKPaint` [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) いずれかの静的メソッドを呼び出すことによって作成した型のオブジェクトに設定し `SKMaskFilter` ます。

マスクフィルターを理解する最善の方法は、これらの静的メソッドを試してみることです。 最も役に立つマスクフィルターは、ぼかしを作成します。

![ぼかしの例](mask-filters-images/MaskFilterExample.png "ぼかしの例")

これは、この記事で説明されている唯一のマスクフィルター機能です。 また、 [**SkiaSharp イメージフィルター**](image-filters.md) に関する次の記事でも、このようなぼかし効果について説明します。 

静的 [`SKMaskFilter.CreateBlur`](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single)) メソッドの構文は次のとおりです。

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

オーバーロードでは、ぼかしの作成に使用されるアルゴリズムにフラグを指定したり、他のグラフィカルオブジェクトで覆われる領域のぼかしを避けるための四角形を使用したりできます。

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle) は、次のメンバーを持つ列挙体です。

- `Normal`
- `Solid`
- `Outer`
- `Inner`

これらのスタイルの効果を次の例に示します。 パラメーターは、 `sigma` ぼかしの範囲を指定します。 以前のバージョンの Skia では、ぼかしの範囲は半径値で示されていました。 Radius 値がアプリケーションに適している場合は、 [`SKMaskFilter.ConvertRadiusToSigma`](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*) 一方を別のに変換できる静的メソッドがあります。 このメソッドは、半径を0.57735 で乗算し、0.5 を追加します。

[**SkiaSharpFormsDemos**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**マスクのぼかし実験**ページでは、ぼかしのスタイルとシグマの値を試すことができます。 XAML ファイルは、 `Picker` 4 つの `SKBlurStyle` 列挙体メンバーと、シグマ値を指定するを使用してをインスタンス化し `Slider` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.MaskBlurExperimentPage"
             Title="Mask Blur Experiment">
    
    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blurStylePicker" 
                Title="Filter Blur Style" 
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlurStyle}">
                    <x:Static Member="skia:SKBlurStyle.Normal" />
                    <x:Static Member="skia:SKBlurStyle.Solid" />
                    <x:Static Member="skia:SKBlurStyle.Outer" />
                    <x:Static Member="skia:SKBlurStyle.Inner" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="sigmaSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaSlider},
                              Path=Value,
                              StringFormat='Sigma = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

分離コードファイルでは、これらの値を使用してオブジェクトを作成し、オブジェクト `SKMaskFilter` のプロパティに設定し `MaskFilter` `SKPaint` ます。 この `SKPaint` オブジェクトは、テキスト文字列とビットマップの両方を描画するために使用されます。

```csharp
public partial class MaskBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage), 
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public MaskBlurExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Pink);

        // Get values from XAML controls
        SKBlurStyle blurStyle =
            (SKBlurStyle)(blurStylePicker.SelectedIndex == -1 ?
                                        0 : blurStylePicker.SelectedItem);

        float sigma = (float)sigmaSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);

            // Get text bounds and calculate display rectangle
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);
            SKRect textRect = new SKRect(0, 0, info.Width, textBounds.Height + 50);

            // Center the text in the display rectangle
            float xText = textRect.Width / 2 - textBounds.MidX;
            float yText = textRect.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);

            // Calculate rectangle for bitmap
            SKRect bitmapRect = new SKRect(0, textRect.Bottom, info.Width, info.Height);
            bitmapRect.Inflate(-50, -50);

            canvas.DrawBitmap(bitmap, bitmapRect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

次の例では、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で、ぼかしスタイルを使用して、レベルを上げるプログラムを実行してい `Normal` `sigma` ます。

[![マスクのぼかし実験-標準](mask-filters-images/MaskBlurExperiment-Normal.png "マスクのぼかし実験-標準")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

ビットマップの端だけがぼかしの影響を受けていることがわかります。 `SKMaskFilter`ビットマップイメージ全体をぼかす必要がある場合、クラスは正しい効果を持ちません。 そのためには、 [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) [**SkiaSharp イメージフィルター**](image-filters.md)に関する次の記事で説明されているように、クラスを使用します。

引数の値を大きくすると、テキストがぼやけて表示され `sigma` ます。 このプログラムを試してみると、特定の値について `sigma` は、Windows 10 デスクトップでのぼかしがより極端に増えていることがわかります。 この違いが発生するのは、ピクセル密度がモバイルデバイスよりもデスクトップモニターで低いため、テキストの高さ (ピクセル単位) が低いためです。 `sigma`値はぼかしの範囲 (ピクセル単位) に比例します。したがって、特定の値に対しては、 `sigma` 解像度が低いほど高い効果が得られます。 実稼働アプリケーションでは、 `sigma` グラフィックのサイズに比例した値を計算することが必要になる場合があります。 

アプリケーションに最適なぼかしレベルを使用する前に、いくつかの値を試してください。 たとえば、[ぼかしの **マスクの実験** ] ページで、次のように設定し `sigma` ます。

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

では `Slider` 効果はありませんが、ぼかしの程度はプラットフォーム間で一貫しています。

[![マスクのぼかし実験-整合](mask-filters-images/MaskBlurExperiment-Consistent.png "マスクのぼかし実験-整合")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

これまでのすべてのスクリーンショットには、列挙メンバーを使用して作成されたぼかしが示されてい `SKBlurStyle.Normal` ます。 次のスクリーンショットは `Solid` 、、、ぼかしスタイルの効果を示してい `Outer` `Inner` ます。

[![マスクのぼかし実験](mask-filters-images/MaskBlurExperiment.png "マスクのぼかし実験")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

IOS のスクリーンショットにはスタイルが表示されて `Solid` います。テキスト文字はまだ黒の実線で表示され、ぼかしはこれらのテキスト文字の外側に追加されます。 

中央の Android スクリーンショットは、スタイルを示し `Outer` ています。 (ビットマップと同様に) 文字ストローク自体は削除され、ぼかしはテキストが1回出現した場所の空の領域を囲みます。 

右側の UWP スクリーンショットにスタイルが表示され `Inner` ます。 ぼかしは、テキスト文字が通常占有する領域に制限されます。

[**SkiaSharp 線形グラデーション**](shaders/linear-gradient.md#transparency-and-gradients)に関する記事では、線形グラデーションと変換を使用してテキスト文字列の反射を模倣した**反射グラデーション**プログラムについて説明しています。

[![反射のグラデーション](shaders/linear-gradient-images/ReflectionGradient.png "反射のグラデーション")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

**ぼやけた反射**のページでは、そのコードに1つのステートメントが追加されます。

```csharp
public class BlurryReflectionPage : ContentPage
{
    const string TEXT = "Reflection";

    public BlurryReflectionPage()
    {
        Title = "Blurry Reflection";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Set text color to blue
            paint.Color = SKColors.Blue;

            // Set text size to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to position text above center
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2;

            // Draw unreflected text
            canvas.DrawText(TEXT, xText, yText, paint);

            // Shift textBounds to match displayed text
            textBounds.Offset(xText, yText);

            // Use those offsets to create a gradient for the reflected text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, textBounds.Top),
                                new SKPoint(0, textBounds.Bottom),
                                new SKColor[] { paint.Color.WithAlpha(0),
                                                paint.Color.WithAlpha(0x80) },
                                null,
                                SKShaderTileMode.Clamp);

            // Create a blur mask filter
            paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

新しいステートメントでは、テキストサイズに基づいて、反射されたテキストのぼかしフィルターを追加します。

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

このぼかしフィルターを適用すると、リフレクションがより現実的になります。

[![反射がぼやけています](mask-filters-images/BlurryReflection.png "反射がぼやけています")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)