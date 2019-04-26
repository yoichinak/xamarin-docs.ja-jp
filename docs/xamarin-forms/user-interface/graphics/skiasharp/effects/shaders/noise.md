---
title: SkiaSharp のノイズやを作成します。
description: パーリン ノイズ シェーダーを生成し、他のシェーダーと結合します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 90C2D00A-2876-43EA-A836-538C3318CF93
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 4801aa12acf8eca2384cc5b41d677f7cb0bdd90d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61292942"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp のノイズやを作成します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

単純なベクター グラフィックスは、不自然に傾向があります。 直線、滑らかな曲線、および純色のない現実世界のオブジェクトの不完全ようになります。 1982 ムービーのコンピューターで生成されたグラフィックスに取り組んでいる_トロン_、コンピューター科学者 Ken Perlin がこれらのイメージをより現実的なテクスチャに与えるためにランダムなプロセスを使用するアルゴリズムの開発を開始します。 1997 年、Ken Perlin Academy に対して賞技術的知識を習得します。 パーリン ノイズと呼ばれる作業が来たし、SkiaSharp ではサポートされてです。 次に例を示します。

![パーリン ノイズ サンプル](noise-images/NoiseSample.png "パーリン ノイズのサンプル")

ご覧のように、各ピクセルはいないランダムな色の値にします。 ピクセルをピクセルからの継続性は、ランダムな図形で発生します。

パーリン ノイズ Skia でのサポートは CSS と SVG の W3C 仕様に基づきます。 セクション 8.20 [**フィルター効果モジュール レベル 1** ](http://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) C コードでは、基になるパーリン ノイズ アルゴリズムが含まれています。

## <a name="exploring-perlin-noise"></a>パーリン ノイズの調査

[ `SKShader` ](xref:SkiaSharp.SKShader)クラス パーリン ノイズを生成するための 2 つの異なる静的メソッドを定義します。 [ `CreatePerlinNoiseFractalNoise` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*)と[ `CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*)します。 パラメーターは同じです。

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

どちらの方法が追加されたオーバー ロードされたバージョンにも存在`SKPointI`パラメーター。 セクション[**タイル パーリン ノイズ**](#tiling-perlin-noise)これらのオーバー ロードをについて説明します。

2 つ`baseFrequency`引数が正の値として 0 から 1 まで SkiaSharp ドキュメントで定義されているより高い値に設定することができます。 値が大きいほど、水平および垂直方向にランダムなイメージの変更は大きくなります。

`numOctaves` 1 以上の整数値です。 アルゴリズムで、イテレーション要素に関連します。 各追加 octave 前 octave の半分であるため、効果は、octave 値を大きく減少する効果を提供します。

`seed`パラメーターは、乱数ジェネレーターの開始点。 浮動小数点値として指定して、前に、これを使用すると、および 0 は 1 と同じ分数が切り捨てられます。

**パーリン ノイズ**ページで、 [ **SkiaSharpFormsDemos**)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)のさまざまな値を試すことができます、`baseFrequency`と`numOctaves`引数。 XAML ファイルを次に示します。

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

使用して 2 つ`Slider`、2 つのビュー`baseFrequency`引数。 値と低い値の範囲を展開するには、スライダーは対数です。 分離コード ファイルに引数が計算されます、`SKShader`の累乗のメソッド、`Slider`値。 `Label`ビューは、計算値を表示します。

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

A `Slider` 1 の値を 0.001 に対応、 `Slider` os 2 値 0.01 に対応、 `Slider` 0.1 に対応する 3 の値と`Slider`4 の値が 1 に対応します。

そのコードを含む分離コード ファイルを次に示します。

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

ここでは、デバイスを iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されているプログラムです。 フラクタル ノイズ、キャンバスの上半分に表示されます。 乱気流ノイズ下半分です。

[![パーリン ノイズ](noise-images/PerlinNoise.png "パーリン ノイズ")](noise-images/PerlinNoise-Large.png#lightbox)

同じ引数は、常に左上隅にあるを開始するのと同じパターンを生成します。 UWP のウィンドウの高さと幅を調整すると、この一貫性は明らかです。 Windows 10 では、画面が再描画と、キャンバスの上半分にパターンは変わりません。

ノイズのパターンには、さまざまな段階での透過性が組み込まれています。 透明度が色を設定する場合は、当然、`canvas.Clear()`呼び出します。 パターンの目立つ色になります。 セクションでは、この効果もわかります[**複数のシェーダーを組み合わせて**](#combining-multiple-shaders)します。

パーリン ノイズのこれらのパターンはほとんど単独で使用されません。 多くの場合、blend モードおよびそれ以降の記事で説明したカラー フィルターにさらされます。

## <a name="tiling-perlin-noise"></a>パーリン ノイズを並べて表示

2 つの静的な`SKShader`パーリン ノイズを作成するためのメソッドはオーバー ロード バージョンにも存在します。 [ `CreatePerlinNoiseFractalNoise` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI))と[ `CreatePerlinNoiseTurbulence` ](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI))オーバー ロードがある追加`SKPointI`パラメーター。

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

[ `SKPointI` ](xref:SkiaSharp.SKPointI)構造体は、使い慣れたの整数バージョン[ `SKPoint` ](xref:SkiaSharp.SKPoint)構造体。 `SKPointI` 定義`X`と`Y`型のプロパティ`int`なく`float`します。

これらのメソッドは、指定したサイズの繰り返しパターンを作成します。 各タイルで、右端は、左端と同じと、上端、下端と同じです。 このような特性の説明については、**パーリン ノイズを並べて表示**ページ。 XAML ファイルは、前の例に似ていますが、のみが、`Stepper`ビューを変更するため、`seed`引数。

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

分離コード ファイルは、タイルのサイズを表す定数を定義します。 `PaintSurface`ハンドラーは、そのサイズのビットマップを作成し、`SKCanvas`そのビットマップを描画します。 `SKShader.CreatePerlinNoiseTurbulence`メソッドは、そのタイルのサイズをシェーダーを作成します。 このシェーダーは、ビットマップに描画されます。

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

ビットマップが作成された後、別`SKPaint`オブジェクトが呼び出すことによって、ビットマップが並べて表示されたパターンを作成するために使用`SKShader.CreateBitmap`します。 2 つの引数に注意してください`SKShaderTileMode.Repeat`:

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

このシェーダーを使用して、キャンバスをについて説明します。 最後に、もう 1 つ`SKPaint`オブジェクトは、元のビットマップのサイズを示す四角形のストロークを描画するために使用します。

のみ、`seed`パラメーターは、ユーザー インターフェイスから選択可能です。 場合同じ`seed`パターンが各プラットフォームで使用される、同じパターンが表示されます。 異なる`seed`値をさまざまなパターン。

[![パーリン ノイズを並べて表示](noise-images/TiledPerlinNoise.png "パーリン ノイズを並べて表示")](noise-images/TiledPerlinNoise-Large.png#lightbox)

左上隅にある 200 ピクセルの正方形のパターンは、その他のタイルにシームレスに渡されます。

## <a name="combining-multiple-shaders"></a>複数のシェーダーの結合

`SKShader`クラスが含まれています、 [ `CreateColor` ](xref:SkiaSharp.SKShader.CreateColor*)指定の純色でシェーダーを作成するメソッド。 単にその色を設定できるため、このシェーダーを単独で非常に便利なことはできません、`Color`のプロパティ、`SKPaint`オブジェクトし、設定、`Shader`プロパティを null にします。

これは、`CreateColor`メソッドに別のメソッドで有用です`SKShader`を定義します。 このメソッドは[ `CreateCompose` ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader))、結合する 2 つのシェーダー。 構文を次に示します。

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

`srcShader` (シェーダーのソース) がの上に効果的に描画される、 `dstShader` (変換先のシェーダー)。 ソース シェーダーが、単色ブラシやグラデーションの透過性なしの場合は、移行先のシェーダーは完全に表示します。

パーリン ノイズのシェーダーには、透過性が含まれています。 そのシェーダーがソースの場合は、変換先のシェーダーが透明な領域で表示されます。

**で構成されるパーリン ノイズ**ページには、最初と実質的に同じである XAML ファイル**パーリン ノイズ**ページ。 分離コード ファイルは似ています。 元の**パーリン ノイズ**ページ セット、`Shader`プロパティの`SKPaint`を静的から返されたシェーダー`CreatePerlinNoiseFractalNoise`と`CreatePerlinNoiseTurbulence`メソッド。 これは、**で構成されるパーリン ノイズ**呼び出しをページ`CreateCompose`組み合わせシェーダー。 変換先は、solid の青いシェーダーを使用して作成`CreateColor`です。 パーリン ノイズのシェーダーをソースには。

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

フラクタルのノイズ シェーダーは、一番上には下にある乱気流シェーダー。

[![パーリン ノイズで構成される](noise-images/ComposedPerlinNoise.png "パーリン ノイズで構成されています")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

によって表示されるものよりも通知をどの程度寒色これらのシェーダーの**パーリン ノイズ**ページ。 違いは、ノイズのシェーダーの透明度の量を示しています。

オーバー ロードはまた、 [ `CreateCompose` ](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode))メソッド。

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

最後のパラメーターのメンバーである、`SKBlendMode`列挙体、列挙体で、次の一連の記事で説明されているメンバーが 29 と[ **SkiaSharp 合成と blend モード**](../blend-modes/index.md)します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
