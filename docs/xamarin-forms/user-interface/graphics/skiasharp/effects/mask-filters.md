---
title: SkiaSharp マスク フィルター
description: マスクのフィルタを使用して、ぼかしやその他のアルファのエフェクトを作成する方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 940422A1-8BC0-4039-8AD7-26C61320F858
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
ms.openlocfilehash: 36a8b5c32261d4f508c82feea1e6127574db6a20
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198249"
---
# <a name="skiasharp-mask-filters"></a>SkiaSharp マスク フィルター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

フィルターのマスクは、ジオメトリとグラフィカル オブジェクトのアルファ チャネルを操作する効果。 マスクのフィルタを使用する設定、 [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter)プロパティの`SKPaint`型のオブジェクトに[ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter)のいずれかを呼び出すことで作成したこと、`SKMaskFilter`静的メソッド。

フィルターのマスクを理解する最善の方法では、これらの静的メソッドを使って試してみるです。 マスクのフィルタを最も役に立つには、ぼかしが作成されます。

![例のぼかし](mask-filters-images/MaskFilterExample.png "ぼかしの例")

この記事で説明されている唯一のマスク フィルター機能です。 次の記事[ **SkiaSharp イメージ フィルター** ](image-filters.md)に望ましいと考えられるぼかし効果についても説明します。 

静的な[ `SKMaskFilter.CreateBlur` ](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single))メソッドには、次の構文。

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

ブラ ―、およびその他のグラフィカル オブジェクトでカバーされます領域にぼかし効果を回避するために四角形を作成するために使用するアルゴリズムのフラグを指定することができるオーバー ロードします。

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle) 次のメンバーを持つ列挙型を示します。

- `Normal`
- `Solid`
- `Outer`
- `Inner`

次の例では、これらのスタイルの効果が表示されます。 `sigma`ぼかしの範囲を指定します。 Skia の以前のバージョンでは、ぼかしのエクステントは、半径の値で示されていました。 静的ながある場合は、半径の値が、アプリケーションの推奨[ `SKMaskFilter.ConvertRadiusToSigma` ](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*)メソッドを他のいずれかから変換できます。 メソッドは、0.57735 半径を乗算し、0.5 を追加します。

**マスクぼかし実験**ページで、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)ぼかしスタイル シグマ値で実験することができます。 XAML ファイルのインスタンスを作成、 `Picker` 4 つの`SKBlurStyle`列挙型メンバー、`Slider`シグマ値を指定します。

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

分離コード ファイルでは、これらの値を使用して、作成、`SKMaskFilter`オブジェクトし、設定、`MaskFilter`のプロパティ、`SKPaint`オブジェクト。 これは、`SKPaint`オブジェクトは、テキスト文字列とビットマップの両方を描画するために使用します。

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

ここでは、iOS、Android、およびとユニバーサル Windows プラットフォーム (UWP) で実行されているプログラム、`Normal`スタイル、および増加のぼかし`sigma`レベル。

[![マスクのぼかし実験 - Normal](mask-filters-images/MaskBlurExperiment-Normal.png "マスクぼかし実験 - 標準")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

ビットマップの端だけがぼかしによって影響を受けることがわかります。 `SKMaskFilter`クラスは、ビットマップ全体のイメージがぼかしたい場合に使用する適切な効果はありません。 そのを使用する、 [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter)クラスでは、次の記事で説明されている[ **SkiaSharp イメージ フィルター**](image-filters.md)します。

複数の値を増やすことで、テキストがあいまい、`sigma`引数。 このプログラムをみることがわかります特定の`sigma`値、ぼかしは、Windows 10 デスクトップより極端な。 この違いは、ピクセル密度は、モバイル デバイスでよりもデスクトップ モニターの下部のテキストの高さ (ピクセル単位) が低いためために発生します。 `sigma`値はピクセル単位でぼかしエクステントに比例ため、指定された`sigma`値、効果は極端な解像度。 運用環境のアプリケーションでおそらくたい計算、`sigma`グラフィックのサイズに比例した値。 

ぼかし、アプリケーションの最適なを検索する前に、いくつかの値をお試しください。 たとえば、**マスクぼかし実験** ページで、設定を試みてください`sigma`次のように。

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

これで、`Slider`影響を与えませんが、ぼかしの度合いは、プラットフォーム間で一貫性のあります。

[![マスクのぼかし実験 - 一貫性のある](mask-filters-images/MaskBlurExperiment-Consistent.png "ぼかし実験 - 一貫性のあるマスク")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

すべてのスクリーン ショットはこれまでにで作成されたぼかしを示すが、`SKBlurStyle.Normal`列挙型のメンバー。 次のスクリーン ショットの効果を表示する、 `Solid`、 `Outer`、および`Inner`ぼかしスタイル。

[![ぼかしの実験をマスク](mask-filters-images/MaskBlurExperiment.png "ぼかし実験のマスク")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

IOS のスクリーンショットに`Solid`は、次のスタイルが示されています。テキスト文字は塗りつぶされた黒いストロークとして引き続き存在し、ぼかしはこれらのテキスト文字の外側に追加されます。 

中央の Android スクリーンショットには、 `Outer`次のスタイルが表示されます。文字ストローク自体は (ビットマップと同様に) 削除され、ぼかしはテキスト文字が1回出現した空の領域を囲みます。 

適切な番組で UWP スクリーン ショット、`Inner`スタイル。 ぼかしは、通常、テキスト文字が占める領域に限定されます。

[ **SkiaSharp 線状グラデーション**](shaders/linear-gradient.md#transparency-and-gradients)記事で説明されている、**リフレクション グラデーション**線形グラデーションと変換をテキスト文字列の反射を模倣するために使用するプログラム。

[![リフレクション グラデーション](shaders/linear-gradient-images/ReflectionGradient.png "リフレクション グラデーション")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

**ぼやけてリフレクション**ページは、そのコードを 1 つのステートメントを追加します。

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

新しいステートメントは、テキストのサイズに基づいているテキストの blur フィルターを追加します。

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

この blur フィルターより現実的なようにリフレクションが発生します。

[![画像がぼやけるリフレクション](mask-filters-images/BlurryReflection.png "ぼやけてリフレクション")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
