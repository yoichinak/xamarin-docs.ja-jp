---
title: SkiaSharp の円形グラデーション
description: グラデーション円に基づくのさまざまな種類について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: d56cc499112a937cd1a22664adeedd54c4397341
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305745"
---
# <a name="the-skiasharp-circular-gradients"></a>SkiaSharp の円形グラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKShader`](xref:SkiaSharp.SKShader)クラスは、4つの異なる種類のグラデーションを作成するための静的メソッドを定義します。 [**SkiaSharp 線状グラデーション**](linear-gradient.md)の記事では、 [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*)方法について説明します。 この記事では、その他の 3 つの種類のグラデーション、円をに基づいてこれらはすべてについて説明します。

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*)メソッドは、円の中心から長年たグラデーションを作成します。

![放射状グラデーションのサンプル](circular-gradients-images/RadialGradientSample.png)

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*)メソッドは、円の中心をスイープするグラデーションを作成します。

![スイープのグラデーションのサンプル](circular-gradients-images/SweepGradientSample.png)

3 番目のグラデーションの種類はまったく通常ではありません。 これは2点の円錐グラデーションと呼ばれ、 [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*)メソッドによって定義されます。 グラデーションは、別に 1 つの円から拡張します。

![グラデーションの円錐のサンプル](circular-gradients-images/ConicalGradientSample.png)

2 つの円がさまざまなサイズの場合、グラデーションは、円すいの形式をとります。

この記事では、さらに詳しくこれらグラデーションについて説明します。

## <a name="the-radial-gradient"></a>放射状グラデーション

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))メソッドには、次の構文があります。

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))のオーバーロードには、変換行列パラメーターも含まれています。

最初の 2 つの引数は、円、半径の中心を指定します。 その中心からグラデーションが開始され、`radius` ピクセルの外に向かって拡大します。 `radius` を超えて発生するのは、 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode)引数によって異なります。 `colors` パラメーターは2つ以上の色の配列です (線形グラデーションメソッドと同様)。 `colorPos` は、0 ~ 1 の範囲の整数の配列です。 これらの整数は、`radius` 線に沿った色の相対位置を示します。 この引数を `null` に設定すると、色を均等にスペースにすることができます。

`CreateRadialGradient` を使用して円の塗りつぶしを行う場合は、グラデーションの中心を円の中心に、グラデーションの半径を円の半径に設定できます。 その場合、`SKShaderTileMode` 引数はグラデーションのレンダリングに影響しません。 ただし、グラデーションによって塗りつぶされる領域がグラデーションで定義された円より大きい場合、`SKShaderTileMode` 引数は円の外側に何が起こるかに大きな影響を与えます。

`SKShaderMode` の効果は、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの **[放射状グラデーション]** ページで示されています。 このページの XAML ファイルは、`SKShaderTileMode` 列挙体の3つのメンバーのいずれかを選択できるようにする `Picker` をインスタンス化します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.RadialGradientPage"
             Title="Radial Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="0"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

分離コード ファイルは、放射状グラデーションを使用してキャンバス全体を色します。 グラデーションの中心は、キャンバスの中央に設定され、半径が 100 ピクセルに設定。 黒と白だけ 2 つの色のグラデーションで構成されます。

```csharp
public partial class RadialGradientPage : ContentPage
{
    public RadialGradientPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                new SKPoint(info.Rect.MidX, info.Rect.MidY),
                                100,
                                new SKColor[] { SKColors.Black, SKColors.White },
                                null,
                                tileMode);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

このコードでは、中心から 100 ピクセルを白に徐々 にフェードの中心に黒でグラデーションを作成します。 この半径を超えると、`SKShaderTileMode` 引数に依存します。

[![放射状グラデーション](circular-gradients-images/RadialGradient.png "放射状グラデーション")](circular-gradients-images/RadialGradient-Large.png#lightbox)

すべての 3 つのケースでは、グラデーションは、キャンバスを塗りつぶします。 左側にある iOS の画面では、半径を超えるグラデーションは、最後の色は白を続行します。 `SKShaderTileMode.Clamp`の結果です。 Android の画面には `SKShaderTileMode.Repeat`の効果が示されています。センターから100ピクセルで、グラデーションが最初の色 (黒) で再び開始されます。 グラデーションは、radius のすべての 100 ピクセルが繰り返されます。 

右側の [ユニバーサル Windows プラットフォーム] 画面には、`SKShaderTileMode.Mirror` によってグラデーションが交互に表示される方法が示されています。 最初のグラデーションは、中央の黒から白に半径が 100 ピクセルのです。 次は、200 ピクセルの半径を黒に 100 ピクセル radius から白し、もう一度 [次へ] のグラデーションを反転します。

放射状グラデーションでは、複数の 2 つの色を使用できます。 **レインボー弧のグラデーション**サンプルでは、レインボーの色に対応する8色の配列を作成し、赤で終了します。また、次の8つの位置値の配列も作成します。

```csharp
public class RainbowArcGradientPage : ContentPage
{
    public RainbowArcGradientPage ()
    {
        Title = "Rainbow Arc Gradient";

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
            float rainbowWidth = Math.Min(info.Width, info.Height) / 4f;

            // Center of arc and gradient is lower-right corner
            SKPoint center = new SKPoint(info.Width, info.Height);

            // Find outer, inner, and middle radius
            float outerRadius = Math.Min(info.Width, info.Height);
            float innerRadius = outerRadius - rainbowWidth;
            float radius = outerRadius - rainbowWidth / 2;

            // Calculate the colors and positions
            SKColor[] colors = new SKColor[8];
            float[] positions = new float[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
                positions[i] = (i + (7f - i) * innerRadius / outerRadius) / 7f;
            }

            // Create sweep gradient based on center and outer radius
            paint.Shader = SKShader.CreateRadialGradient(center, 
                                                         outerRadius, 
                                                         colors, 
                                                         positions, 
                                                         SKShaderTileMode.Clamp);
            // Draw a circle with a wide line
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = rainbowWidth;

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

キャンバスの幅と高さの最小値が1000であることを想定しています。これは、`rainbowWidth` 値が250であることを意味します。 `outerRadius` と `innerRadius` の値はそれぞれ1000と750に設定されます。 これらの値は、`positions` 配列を計算するために使用されます。8つの値の範囲は 0.75 f ~ 1 です。 `radius` 値は、円の描画に使用されます。 値は 750 ピクセルの半径と 1000 ピクセルの半径の 250 ピクセルのストロークの幅を拡張 875 ことを意味します。

[![レインボー弧 (グラデーション)](circular-gradients-images/RainbowArcGradient.png "レインボー弧 (グラデーション)")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

このグラデーションを使用してキャンバス全体を入力している場合、内側の半径内で赤であるが表示されます。 これは、`positions` 配列の先頭が0ではないためです。 0 の配列の最初の値からのオフセットの最初の色が使用されます。 グラデーションは、外側の半径を超える赤もあります。 これは、`Clamp` タイルモードの結果です。 線の太い線の描画にグラデーションを使用しているために、これらの赤い領域は表示されません。

## <a name="radial-gradients-for-masking"></a>マスクの放射状グラデーション

線形グラデーションは、ような放射状グラデーションは透明または部分的に透明色を組み込むことができます。 この機能は、イメージの一部を非表示にしてイメージの別の部分を強調する_マスキング_と呼ばれるプロセスに便利です。

**[放射状グラデーションマスク]** ページに例が表示されます。 リソース ビットマップのプログラムによって読み込まれます。 `CENTER` フィールドと `RADIUS` フィールドは、ビットマップの検査によって決定され、強調表示する必要がある領域を参照します。 `PaintSurface` ハンドラーは、ビットマップを表示する四角形を計算してから、その四角形に表示します。

```csharp
public class RadialGradientMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(RadialGradientMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public RadialGradientMaskPage ()
    {
        Title = "Radial Gradient Mask";

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

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                                (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                     scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Transparent,
                                                SKColors.White },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            // Display rectangle using that gradient
            canvas.DrawRect(rect, paint);
        }
    }
}
```

ビットマップを描画した後、一部の単純なコードでは、`CENTER` と `RADIUS` を `center` および `radius`に変換します。これは、ビットマップ内で強調表示されている領域を参照します。 これらの値は、その中心と radius の放射状グラデーションの作成に使用されます。 2 つの色が始まると半径の最初の 60% の中央に透過的な。 グラデーションが白にフェードします。

[![放射状グラデーションマスク](circular-gradients-images/RadialGradientMask.png "放射状グラデーションマスク")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

この方法は、ビットマップをマスクする最善の方法ではありません。 問題は、マスクは、キャンバスの背景として選択された、白のカラーをほとんどの場合です。 背景がその他の色 &mdash; か、またはグラデーション自体の場合、&mdash; は一致しません。 マスクに対するより優れたアプローチについては、 [SkiaSharp Porter の blend モード](../blend-modes/porter-duff.md)に関する記事をご覧ください。

## <a name="radial-gradients-for-specular-highlights"></a>反射の光源の放射状グラデーション

ライト丸みのあるサーフェイスが発生したときに、多くの方向に光が反映されますが、者の目に直接 bounces 光のいくつか。 これにより、多くの場合、_反射の強調表示_と呼ばれる、表面上のあいまいな白い領域の外観が作成されます。

3 次元グラフィックは、反射の光源が明るいパス、網掛けを判断するために使用するアルゴリズムの結果として多くの場合にします。 2 次元グラフィックスは、反射の光源は 3D サーフェイスの外観を提案する追加場合があります。 反射ハイライトは、round 赤いボールにフラットな赤い円を変換できます。

**放射状反射の強調表示**ページでは、そのために放射状グラデーションが使用されます。 `PaintSurface` ハンドラーは、円の半径を計算することによって発生します。また、2つの `SKPoint` 値 &mdash; `center` と、中央と円の左上隅の中間にある `offCenter` を計算します。

```csharp
public class RadialSpecularHighlightPage : ContentPage
{
    public RadialSpecularHighlightPage()
    {
        Title = "Radial Specular Highlight";

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

        float radius = 0.4f * Math.Min(info.Width, info.Height);
        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        SKPoint offCenter = center - new SKPoint(radius / 2, radius / 2);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                offCenter,
                                radius / 2,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

`CreateRadialGradient` の呼び出しでは、その `offCenter` ポイントから開始し、白で終了し、半径の半分の距離で終了するグラデーションを作成します。 次の図は、その例です。

[![放射状反射の強調表示](circular-gradients-images/RadialSpecularHighlight.png "放射状反射の強調表示")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

このグラデーションをよく見る場合、これは欠陥ことことがあります。 グラデーションが特定のポイントを中心としてが少し減ります丸みのある画面を反映するように対称的なをすることもできます。 そのような場合は、次に示す反射の強調表示を、[**反射の光源の円錐のグラデーション**](#conical-gradients-for-specular-highlights)のセクションで使用することをお勧めします。

## <a name="the-sweep-gradient"></a>スイープのグラデーション

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[]))メソッドには、すべてのグラデーション作成メソッドの最も単純な構文があります。

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Center だけ、色の配列および色の位置になります。 グラデーションは、中心点の右側にある開始し、360 度を中心に時計回りにスイープします。 `SKShaderTileMode` パラメーターがないことに注意してください。

マトリックス変換パラメーターを使用して[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix))オーバーロードを使用することもできます。 グラデーションを開始点を変更するには、回転変換を適用できます。 時計回りにから反時計回りに方向を変更するスケール変換を適用することもできます。

**スイープグラデーション**ページではスイープグラデーションを使用して、ストロークの幅が50ピクセルの円に色を付けます。

[![スイープグラデーション](circular-gradients-images/SweepGradient.png "スイープグラデーション")](circular-gradients-images/SweepGradient-Large.png#lightbox)

`SweepGradientPage` クラスは、異なる色相値を持つ8色の配列を定義します。 配列が開始され、スクリーン ショットでは右端に表示される赤 (0 または 360 の色相値)、終了ことに注意してください。

```csharp
public class SweepGradientPage : ContentPage
{
    bool drawBackground;

    public SweepGradientPage ()
    {
        Title = "Sweep Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            drawBackground ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Define an array of rainbow colors
            SKColor[] colors = new SKColor[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
            }

            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);

            // Create sweep gradient based on center of canvas
            paint.Shader = SKShader.CreateSweepGradient(center, colors, null);

            // Draw a circle with a wide line
            const int strokeWidth = 50;
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = strokeWidth;

            float radius = (Math.Min(info.Width, info.Height) - strokeWidth) / 2;
            canvas.DrawCircle(center, radius, paint);

            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                paint.Style = SKPaintStyle.Fill;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

このプログラムは、`PaintSurface` ハンドラーの末尾で一部のコードを有効にする `TapGestureRecognizer` も実装します。 このコードでは、同じのグラデーションを使用して、キャンバスが埋まります。

[![スイープグラデーションがいっぱいです](circular-gradients-images/SweepGradientFull.png "スイープグラデーションがいっぱいです")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

これらのスクリーン ショットでは、こと、グラデーション塗りつぶしどのような領域に色をについて説明します。 グラデーションの開始し、同じ色の終了はしない、中心点の右側に不連続性があります。

## <a name="the-two-point-conical-gradient"></a>2 つの点の円錐グラデーション

[`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))メソッドには、次の構文があります。

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

これらのパラメーターは、_開始_円と_終了_円と呼ばれる2つの円の中心点と半径で始まります。 残りの3つのパラメーターは、`CreateLinearGradient` と `CreateRadialGradient`と同じです。 [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))のオーバーロードには、マトリックス変換が含まれます。

グラデーションがスタート円で始まり末尾の円で終了します。 `SKShaderTileMode` パラメーターは、2つの円の後の動作を制御します。 2 つの点の円錐グラデーションでは、領域を入力していない完全のみのグラデーションです。 2 つの円の半径を同じ場合は、グラデーションは、四角形の円の直径と同じ幅に制限されます。 2 つの円の半径が異なる場合は、グラデーションは円錐を形成します。

2点の円錐グラデーションを試してみると、**円錐のグラデーション**ページが `InteractivePage` から派生し、2つのタッチポイントを2つの円半径に移動できるようになります。

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.ConicalGradientPage"
                       Title="Conical Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <Grid BackgroundColor="White"
              Grid.Row="0">
            <skiaforms:SKCanvasView x:Name="canvasView"
                                    PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</local:InteractivePage>
```

分離コードファイルは、50と100の固定半径を持つ2つの `TouchPoint` オブジェクトを定義します。

```csharp
public partial class ConicalGradientPage : InteractivePage
{
    const int RADIUS1 = 50;
    const int RADIUS2 = 100;

    public ConicalGradientPage ()
    {
        touchPoints = new TouchPoint[2];

        touchPoints[0] = new TouchPoint
        {
            Center = new SKPoint(100, 100),
            Radius = RADIUS1
        };

        touchPoints[1] = new TouchPoint
        {
            Center = new SKPoint(300, 300),
            Radius = RADIUS2
        };

        InitializeComponent();
        baseCanvasView = canvasView;
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKColor[] colors = { SKColors.Red, SKColors.Green, SKColors.Blue };
        SKShaderTileMode tileMode = 
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ? 
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(touchPoints[0].Center,
                                                                  RADIUS1,
                                                                  touchPoints[1].Center,
                                                                  RADIUS2,
                                                                  colors,
                                                                  null,
                                                                  tileMode);
            canvas.DrawRect(info.Rect, paint);
        }

        // Display the touch points here rather than by TouchPoint
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;

            foreach (TouchPoint touchPoint in touchPoints)
            {
                canvas.DrawCircle(touchPoint.Center, touchPoint.Radius, paint);
            }
        }
    }
}
```

`colors` 配列は、赤、緑、および青です。 `PaintSurface` ハンドラーの下部にあるコードは、2つのタッチポイントを黒の円として描画し、グラデーションが妨げされないようにします。

`DrawRect` の呼び出しは、グラデーションを使用してキャンバス全体に色を設定することに注意してください。 一般的なケースでは、ただし、キャンバスの多くは、グラデーションを四角いです。 次の 3 つの可能な構成を示すプログラムを次に示します。

[![円錐のグラデーション](circular-gradients-images/ConicalGradient.png "円錐のグラデーション")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

左側の iOS 画面は、`Clamp`の `SKShaderTileMode` 設定の効果を示しています。 グラデーションは、赤は反対側の 2 番目の円に最も近い小さな円の内側で開始します。 また、`Clamp` 値を指定すると、赤が円錐のポイントまで続きます。 グラデーションは、青を円の内部以降では最初の円に最も近いが、引き続き大きく円の外側のエッジが青で終わります。

Android の画面は似ていますが、`Repeat`の `SKShaderTileMode` があります。 これで、グラデーションが最初の円の内部に開始され、2 つ目の円の外側の終了を明確になります。 `Repeat` 設定により、グラデーションは、大きい円の内側に赤で繰り返し表示されます。

UWP の画面では、小さな円が大きい円の内部に完全に移動したときの動作を示します。 グラデーションは、される円錐を停止し、代わりに全体の領域を塗りつぶします。 効果が放射状のグラデーションのようには、小さな円が必ずしも大きな円の中央に配置する場合非対称です。

グラデーションの実用的な有用性は、別の 1 つの円が入れ子になったが、鏡面ハイライトに最適です疑い可能性があります。

## <a name="conical-gradients-for-specular-highlights"></a>反射の光源の円錐のグラデーション

この記事の前半で放射状グラデーションを使用して、反射ハイライトを作成する方法を説明しました。 このため、2 つの点の円錐のグラデーションを使用することもでき、外観したい場合があります。

[![円錐の反射の強調表示](circular-gradients-images/ConicalSpecularHighlight.png "円錐の反射の強調表示")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

非対称の外観には、オブジェクトの丸みのあるサーフェスよりお勧めします。 

**円錐反射の強調表示**ページの描画コードは、シェーダーを除き、**放射状反射の強調表示**ページと同じです。

```csharp
public class ConicalSpecularHighlightPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(
                                offCenter,
                                1,
                                center,
                                radius,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

2つの円には `offCenter` と `center`の中心があります。 `center` の中央にある円は、ボール全体を含む半径に関連付けられていますが、`offCenter` の中央にある円の半径は1ピクセルだけです。 グラデーションは実質的にその時点で開始し、ボールの端で終了します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
