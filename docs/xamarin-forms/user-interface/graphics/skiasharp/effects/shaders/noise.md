---
title: SkiaSharp のノイズと作成
description: Perlin ノイズシェーダーを生成し、他のシェーダーと結合します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 45ec48c0b7b58e26fa47d7343e96bb49591cb339
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127765"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp のノイズと作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

単純なベクターグラフィックスは不自然に見える傾向があります。 直線、滑らかな曲線、純色は、実際のオブジェクトの傷とはよく似ていません。 コンピューター科学者 Ken Perlin は、コンピューターが生成した 1982 movie _Tron_用のグラフィックスを操作している間に、ランダムなプロセスを使用して、これらのイメージにより現実的なテクスチャを提供するアルゴリズムの開発を開始しました。 1997では、Ken Perlin は技術に関するアチーブメントを獲得しています。 彼の仕事は Perlin ノイズとして知られており、SkiaSharp でもサポートされています。 次に例を示します。

![Perlin のノイズのサンプル](noise-images/NoiseSample.png "Perlin のノイズのサンプル")

ご覧のように、各ピクセルはランダムな色の値ではありません。 ピクセルからピクセルへの連続性は、ランダムな形状になります。

Skia での Perlin ノイズのサポートは、CSS と SVG の W3C 仕様に基づいています。 フィルター効果のセクション8.20 の[**モジュールレベル 1**](https://www.w3.org/TR/filter-effects-1/#feTurbulenceElement)には、C コードの基になる Perlin ノイズアルゴリズムが含まれています。

## <a name="exploring-perlin-noise"></a>Perlin ノイズの調査

クラスは、 [`SKShader`](xref:SkiaSharp.SKShader) Perlin ノイズを生成するための2つの異なる静的メソッド (と) を定義し [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*) ます。 パラメーターは同じです。

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

また、どちらのメソッドも、追加のパラメーターを使用して、オーバーロードされたバージョンに存在し `SKPointI` ます。 「 [**Perlin ノイズのタイル**](#tiling-perlin-noise)」では、これらのオーバーロードについて説明しています。

2つの引数は、 `baseFrequency` 0 ~ 1 の範囲の SkiaSharp ドキュメントで定義されている正の値ですが、より高い値に設定することもできます。 値が大きいほど、ランダムな画像の変化が、水平方向と垂直方向に大きくなります。

`numOctaves`値は1以上の整数です。 アルゴリズムのイテレーション係数に関連しています。 1つの追加のオクターブは、前のオクターブの半分である影響を与えるので、大きな値になると効果が低下します。

パラメーターは、 `seed` 乱数ジェネレーターの開始点です。 浮動小数点値として指定されていますが、分数は使用前に切り捨てられ、0は1と同じになります。

[ **SkiaSharpFormsDemos**)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**Perlin ノイズ**ページを使用すると、引数と引数のさまざまな値を試すことができ `baseFrequency` `numOctaves` ます。 XAML ファイルを次に示します。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PerlinNoisePage"
             Title="Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="baseFrequencyXSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyXText"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="baseFrequencyYSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyYText"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference octavesStepper},
                                  Path=Value,
                                  StringFormat='Number of Octaves: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="octavesStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

2つの `Slider` 引数に2つのビューを使用 `baseFrequency` します。 小さい値の範囲を拡大するには、スライダーを対数にします。 分離コードファイルは、 `SKShader` 値の累乗からメソッドへの引数を計算し `Slider` ます。 ビューには `Label` 、次の計算値が表示されます。

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

`Slider`値1は0.001 に対応し、 `Slider` os 2 の値は0.01 に対応し、値3は `Slider` 0.1 に対応し、 `Slider` 値4は1に対応します。

そのコードを含む分離コードファイルを次に示します。

```csharp
public partial class PerlinNoisePage : ContentPage
{
    public PerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader =
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0);

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader =
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0);

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

IOS、Android、およびユニバーサル Windows プラットフォーム (UWP) デバイスで実行されているプログラムを次に示します。 フラクタルノイズは、キャンバスの上半分に表示されます。 乱気流のノイズは下半分にあります。

[![Perlin のノイズ](noise-images/PerlinNoise.png "Perlin のノイズ")](noise-images/PerlinNoise-Large.png#lightbox)

同じ引数を指定すると、左上隅から開始するのと同じパターンが常に生成されます。 この一貫性は、UWP ウィンドウの幅と高さを調整するときに明らかになります。 Windows 10 によって画面が再描画されると、キャンバスの上半分にあるパターンは変わりません。

ノイズパターンには、さまざまな透明度が組み込まれています。 呼び出しに色を設定すると、透明度が明らかになり `canvas.Clear()` ます。 パターンでは、その色が目立つようになります。 また、[**複数のシェーダーを結合**](#combining-multiple-shaders)するセクションでも、この効果が得られます。

これらの Perlin ノイズパターンは、単独で使用されることはほとんどありません。 多くの場合、これらは、後の記事で説明する blend モードとカラーフィルターに含まれています。

## <a name="tiling-perlin-noise"></a>タイル Perlin ノイズ

Perlin ノイズを作成するための2つの静的メソッドは、 `SKShader` オーバーロードバージョンにも存在します。 [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI))オーバーロードとオーバーロードには、 [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) 追加のパラメーターがあり `SKPointI` ます。

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

[`SKPointI`](xref:SkiaSharp.SKPointI)構造体は、使い慣れた構造体の整数バージョンです [`SKPoint`](xref:SkiaSharp.SKPoint) 。 `SKPointI`で `X` `Y` はなく型のプロパティとプロパティを定義し `int` `float` ます。

これらのメソッドは、指定されたサイズの繰り返しパターンを作成します。 各タイルでは、右端が左端と同じであり、上端が下端と同じになっています。 この特性は、タイル化された**Perlin ノイズ**ページで示されています。 XAML ファイルは前のサンプルと似ていますが、引数を `Stepper` 変更するためのビューしかありません `seed` 。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.TiledPerlinNoisePage"
             Title="Tiled Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference seedStepper},
                                  Path=Value,
                                  StringFormat='Seed: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="seedStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

分離コードファイルは、タイルサイズの定数を定義します。 ハンドラーは、 `PaintSurface` そのサイズのビットマップと、 `SKCanvas` そのビットマップに描画するためのを作成します。 メソッドは、 `SKShader.CreatePerlinNoiseTurbulence` そのタイルサイズを使用してシェーダーを作成します。 このシェーダーはビットマップ上に描画されます。

```csharp
public partial class TiledPerlinNoisePage : ContentPage
{
    const int TILE_SIZE = 200;

    public TiledPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get seed value from stepper
        float seed = (float)seedStepper.Value;

        SKRect tileRect = new SKRect(0, 0, TILE_SIZE, TILE_SIZE);

        using (SKBitmap bitmap = new SKBitmap(TILE_SIZE, TILE_SIZE))
        {
            using (SKCanvas bitmapCanvas = new SKCanvas(bitmap))
            {
                bitmapCanvas.Clear();

                // Draw tiled turbulence noise on bitmap
                using (SKPaint paint = new SKPaint())
                {
                    paint.Shader = SKShader.CreatePerlinNoiseTurbulence(
                                        0.02f, 0.02f, 1, seed,
                                        new SKPointI(TILE_SIZE, TILE_SIZE));

                    bitmapCanvas.DrawRect(tileRect, paint);
                }
            }

            // Draw tiled bitmap shader on canvas
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
                canvas.DrawRect(info.Rect, paint);
            }

            // Draw rectangle showing tile
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 2;

                canvas.DrawRect(tileRect, paint);
            }
        }
    }
}
```

ビットマップが作成された後、を呼び出すことによって、別のオブジェクトを使用してタイル化された `SKPaint` ビットマップパターンを作成し `SKShader.CreateBitmap` ます。 次の2つの引数に注意して `SKShaderTileMode.Repeat` ください。

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

このシェーダーは、キャンバスをカバーするために使用されます。 最後に、別のオブジェクトを使用して、 `SKPaint` 元のビットマップのサイズを示す四角形を描画します。

`seed`ユーザーインターフェイスから選択できるのは、パラメーターのみです。 `seed`各プラットフォームで同じパターンが使用されている場合は、同じパターンが表示されます。 値が異なると `seed` 、次のようなパターンになります。

[![タイル化 Perlin ノイズ](noise-images/TiledPerlinNoise.png "タイル化 Perlin ノイズ")](noise-images/TiledPerlinNoise-Large.png#lightbox)

左上隅の200ピクセルの正方形パターンは、他のタイルにシームレスに流れます。

## <a name="combining-multiple-shaders"></a>複数のシェーダーの結合

クラスには、 `SKShader` [`CreateColor`](xref:SkiaSharp.SKShader.CreateColor*) 指定された純色でシェーダーを作成するメソッドが含まれています。 このシェーダーは、単純にその色を `Color` オブジェクトのプロパティに設定 `SKPaint` し、プロパティを null に設定できるため、単独ではあまり便利ではありません `Shader` 。

この `CreateColor` メソッドは、を定義する別のメソッドで役に立ち `SKShader` ます。 このメソッドは [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader)) 、2つのシェーダーを結合します。 構文は次のとおりです。

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

`srcShader`(ソースシェーダー) は、実際には `dstShader` (変換先シェーダー) の上に描画されます。 ソースシェーダーが透明色または透明度のないグラデーションの場合、変換先シェーダーは完全に隠されます。

Perlin ノイズシェーダーには透明度が含まれています。 そのシェーダーがソースである場合、コピー先のシェーダーは透明領域に表示されます。

構成済みの**Perlin ノイズ**ページには、最初の**Perlin ノイズ**ページと実質的に同一の XAML ファイルがあります。 分離コードファイルも同様です。 ただし、元の**Perlin ノイズ**ページでは、 `Shader` のプロパティが、 `SKPaint` 静的なメソッドとメソッドから返されたシェーダーに設定され `CreatePerlinNoiseFractalNoise` `CreatePerlinNoiseTurbulence` ます。 これ**Composed Perlin Noise** `CreateCompose` は、組み合わせシェーダーの Perlin ノイズページ呼び出しを構成します。 コピー先は、を使用して作成された青いソリッドシェーダーです `CreateColor` 。 ソースは Perlin のノイズシェーダーです。

```csharp
public partial class ComposedPerlinNoisePage : ContentPage
{
    public ComposedPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0));

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0));

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

フラクタルノイズシェーダーは上にあります。乱気流シェーダーは下部にあります。

[![構成された Perlin ノイズ](noise-images/ComposedPerlinNoise.png "構成された Perlin ノイズ")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

これらのシェーダーは、 **Perlin ノイズ**ページで表示されているものよりも離れるの大きさに注意してください。 この違いは、ノイズシェーダーの透明度の大きさを示しています。

メソッドのオーバーロードもあり [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) ます。

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

最後のパラメーターは列挙体のメンバーで `SKBlendMode` 、 [**SkiaSharp 合成および blend モード**](../blend-modes/index.md)に関する次の一連の記事で説明されている、29メンバーの列挙体です。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
