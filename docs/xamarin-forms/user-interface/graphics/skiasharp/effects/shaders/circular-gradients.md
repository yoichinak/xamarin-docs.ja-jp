---
title: SkiaSharp の円形グラデーション
description: グラデーション円に基づくのさまざまな種類について説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: a17ddf438856600870c9bb3da60a5f4667128d57
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056046"
---
# <a name="the-skiasharp-circular-gradients"></a>SkiaSharp の円形グラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

[ `SKShader` ](xref:SkiaSharp.SKShader)クラスは、4 つの異なる種類のグラデーションを作成する静的メソッドを定義します。 [ **SkiaSharp 線状グラデーション**](linear-gradient.md)説明、 [ `CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient*)メソッド。 この記事では、その他の 3 つの種類のグラデーション、円をに基づいてこれらはすべてについて説明します。

[ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient*)メソッドは、円の中心から放射されるグラデーションを作成します。

![放射状グラデーションのサンプル](circular-gradients-images/RadialGradientSample.png)

[ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient*)メソッドは、円の中心をスイープするグラデーションを作成します。

![スイープのグラデーションのサンプル](circular-gradients-images/SweepGradientSample.png)

3 番目のグラデーションの種類はまったく通常ではありません。 2 つの点の円錐グラデーションと呼びます、によって定義されますが、 [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*)メソッド。 グラデーションは、別に 1 つの円から拡張します。

![グラデーションの円錐のサンプル](circular-gradients-images/ConicalGradientSample.png)

2 つの円がさまざまなサイズの場合、グラデーションは、円すいの形式をとります。

この記事では、さらに詳しくこれらグラデーションについて説明します。

## <a name="the-radial-gradient"></a>放射状グラデーション

[ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))メソッドには、次の構文。

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

A [ `CreateRadialGradient` ](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))オーバー ロードには、変換行列のパラメーターも含まれています。

最初の 2 つの引数は、円、半径の中心を指定します。 グラデーションは、そのデータ センターの開始しの外側を拡張`radius`(ピクセル)。 超える起こる`radius`によって異なります、 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode)引数。 `colors`パラメーターは、(同様、線形グラデーション メソッド)、2 つまたは複数の色の配列と`colorPos`は 0 ~ 1 の範囲の整数の配列です。 これらの整数をに沿って色の相対位置を示す`radius`行。 引数を設定する`null`に均等に領域の色。

使用する場合`CreateRadialGradient`円を入力する、円の中心に半径の円の半径へのグラデーションのグラデーションの中心を設定することができます。 その場合は、`SKShaderTileMode`引数は、グラデーションのレンダリングに影響を与えません。 グラデーションで塗りつぶす領域が、グラデーション、によって定義される円を超える場合は、`SKShaderTileMode`引数が円の外側の動作に大きな影響します。

効果`SKShaderMode`方法については、**放射状グラデーション**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)サンプル。 このページの XAML ファイルのインスタンスを作成、`Picker`の 3 つのメンバーのいずれかを選択することができます、`SKShaderTileMode`列挙体。

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

このコードでは、中心から 100 ピクセルを白に徐々 にフェードの中心に黒でグラデーションを作成します。 半径の外の動作によって異なります、`SKShaderTileMode`引数。

[![放射状グラデーション](circular-gradients-images/RadialGradient.png "放射状グラデーション")](circular-gradients-images/RadialGradient-Large.png#lightbox)

すべての 3 つのケースでは、グラデーションは、キャンバスを塗りつぶします。 左側にある iOS の画面では、半径を超えるグラデーションは、最後の色は白を続行します。 結果である`SKShaderTileMode.Clamp`します。 Android の画面は、の効果を示しています。 `SKShaderTileMode.Repeat`: センターからに 100 ピクセル、グラデーションが再び開始の最初の色は黒です。 グラデーションは、radius のすべての 100 ピクセルが繰り返されます。 

ユニバーサル Windows プラットフォーム画面の右に示す方法`SKShaderTileMode.Mirror`代替方向にグラデーションをによりします。 最初のグラデーションは、中央の黒から白に半径が 100 ピクセルのです。 次は、200 ピクセルの半径を黒に 100 ピクセル radius から白し、もう一度 [次へ] のグラデーションを反転します。

放射状グラデーションでは、複数の 2 つの色を使用できます。 **虹円弧グラデーション**サンプル虹と末尾に、赤の色に対応する 8 つの色の配列とも、8 つの位置の値の配列を作成します。

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

たとえば、幅の最小値と、キャンバスの高さは 1000 です。 つまり、`rainbowWidth`値は 250 です。 `outerRadius`と`innerRadius`値は 1000 に設定および 750、それぞれします。 これらの値は計算に使用、`positions`配列; 0.75f 1 ~ 8 個の値の範囲。 `radius`値は、線、円の描画に使用されます。 値は 750 ピクセルの半径と 1000 ピクセルの半径の 250 ピクセルのストロークの幅を拡張 875 ことを意味します。

[![虹円弧グラデーション](circular-gradients-images/RainbowArcGradient.png "虹円弧グラデーション")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

このグラデーションを使用してキャンバス全体を入力している場合、内側の半径内で赤であるが表示されます。 これは、ため、`positions`配列が 0 で開始しません。 0 の配列の最初の値からのオフセットの最初の色が使用されます。 グラデーションは、外側の半径を超える赤もあります。 結果である、`Clamp`タイル モード。 線の太い線の描画にグラデーションを使用しているために、これらの赤い領域は表示されません。

## <a name="radial-gradients-for-masking"></a>マスクの放射状グラデーション

線形グラデーションは、ような放射状グラデーションは透明または部分的に透明色を組み込むことができます。 この機能は役立ちますと呼ばれるプロセスの_マスク_イメージの別の部分を強調したり、イメージの一部が隠ぺいされます。

**放射状のグラデーション マスク**ページ例を示しています。 リソース ビットマップのプログラムによって読み込まれます。 `CENTER`と`RADIUS`フィールドを調べる場合は、ビットマップから判断し、強調表示されている必要があります領域を参照します。 `PaintSurface`ハンドラーは、ビットマップを表示する四角形を計算することで開始し、その四角形で表示します。

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

ビットマップを描画した後は、単純なコードが変換されます`CENTER`と`RADIUS`に`center`と`radius`、拡大縮小および表示するためにシフトされたビットマップで強調表示された領域を参照します。 これらの値は、その中心と radius の放射状グラデーションの作成に使用されます。 2 つの色が始まると半径の最初の 60% の中央に透過的な。 グラデーションが白にフェードします。

[![放射状グラデーション マスク](circular-gradients-images/RadialGradientMask.png "放射状グラデーションのマスク")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

この方法は、ビットマップをマスクする最善の方法ではありません。 問題は、マスクは、キャンバスの背景として選択された、白のカラーをほとんどの場合です。 背景がその他のいくつかの色である場合&mdash;または自体グラデーションおそらく&mdash;に一致しません。 マスクにより優れたアプローチは、記事に記載[SkiaSharp の Porter Duff ブレンド モード](../blend-modes/porter-duff.md)します。

## <a name="radial-gradients-for-specular-highlights"></a>反射の光源の放射状グラデーション

ライト丸みのあるサーフェイスが発生したときに、多くの方向に光が反映されますが、者の目に直接 bounces 光のいくつか。 これと呼ばれる画面に多くの場合、あいまいの空白領域の外観が作成、_鏡面ハイライト_します。

3 次元グラフィックは、反射の光源が明るいパス、網掛けを判断するために使用するアルゴリズムの結果として多くの場合にします。 2 次元グラフィックスは、反射の光源は 3D サーフェイスの外観を提案する追加場合があります。 反射ハイライトは、round 赤いボールにフラットな赤い円を変換できます。

**放射状鏡面ハイライト**ページでは、放射状グラデーションを使用して、その正確にします。 `PaintSurface`ハンドラーいるので、円と 2 つの半径を計算することで`SKPoint`値&mdash;、`center`と`offCenter`円の左端と中央の中間です。

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

`CreateRadialGradient`呼び出しをで始まるグラデーションを作成します`offCenter`白、半分の半径の距離にある赤で終わるをポイントします。 次のように表示されます。

[![放射状の反射ハイライト](circular-gradients-images/RadialSpecularHighlight.png "放射状の反射ハイライト")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

このグラデーションをよく見る場合、これは欠陥ことことがあります。 グラデーションが特定のポイントを中心としてが少し減ります丸みのある画面を反映するように対称的なをすることもできます。 その場合は、セクションの下に表示の反射ハイライトで望ましいと考え[**反射の光源の円錐のグラデーション**](#conical-gradients-for-specular-highlights)します。

## <a name="the-sweep-gradient"></a>スイープのグラデーション

[ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[]))メソッドすべてのグラデーションの作成方法の最も簡単な構文には。

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Center だけ、色の配列および色の位置になります。 グラデーションは、中心点の右側にある開始し、360 度を中心に時計回りにスイープします。 あることに注意してくださいありません`SKShaderTileMode`パラメーター。

A [ `CreateSweepGradient` ](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix))マトリックス変換パラメーターを持つオーバー ロードも使用します。 グラデーションを開始点を変更するには、回転変換を適用できます。 時計回りにから反時計回りに方向を変更するスケール変換を適用することもできます。

**スイープ グラデーション** ページでは、スイープ グラデーションを使用して、円のストローク幅 50 ピクセルの色します。

[![グラデーションのスイープ](circular-gradients-images/SweepGradient.png "グラデーションをスイープ")](circular-gradients-images/SweepGradient-Large.png#lightbox)

`SweepGradientPage`クラスは、異なる色合いの値の 8 つの色の配列を定義します。 配列が開始され、スクリーン ショットでは右端に表示される赤 (0 または 360 の色相値)、終了ことに注意してください。

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

プログラムを実装しても、`TapGestureRecognizer`の最後にいくつかのコードをできるようにする、`PaintSurface`ハンドラー。 このコードでは、同じのグラデーションを使用して、キャンバスが埋まります。

[![グラデーションの全体をスイープ](circular-gradients-images/SweepGradientFull.png "グラデーション全体をスイープ")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

これらのスクリーン ショットでは、こと、グラデーション塗りつぶしどのような領域に色をについて説明します。 グラデーションの開始し、同じ色の終了はしない、中心点の右側に不連続性があります。

## <a name="the-two-point-conical-gradient"></a>2 つの点の円錐グラデーション

[ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))メソッドには、次の構文。

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

Center 点と呼ばれる 2 つの円の半径を持つパラメーターの開始、_開始_円と_エンド_円。 残りの 3 つのパラメーターが同じである`CreateLinearGradient`と`CreateRadialGradient`します。 A [ `CreateTwoPointConicalGradient` ](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))オーバー ロードには、行列変換が含まれます。

グラデーションがスタート円で始まり末尾の円で終了します。 `SKShaderTileMode`パラメーターは 2 つの円を超えるの動作を制御します。 2 つの点の円錐グラデーションでは、領域を入力していない完全のみのグラデーションです。 2 つの円の半径を同じ場合は、グラデーションは、四角形の円の直径と同じ幅に制限されます。 2 つの円の半径が異なる場合は、グラデーションは円錐を形成します。

2 点の円錐グラデーションで実験する可能性がありますので、**円錐グラデーション**から派生したページ`InteractivePage`2 つの円の半径のに移動する 2 つのタッチ ポイントを許可します。

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

分離コード ファイルでは、2 つを定義します。 `TouchPoint` 50 と 100 の固定の半径を持つオブジェクト。

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

`colors`配列は赤、緑、および青。 下部のコード、`PaintSurface`黒の円はグラデーションの妨げにならないように、ハンドラーが 2 つのタッチ ポイントを描画します。

注意`DrawRect`呼び出しにキャンバス全体の色グラデーションを使用します。 一般的なケースでは、ただし、キャンバスの多くは、グラデーションを四角いです。 次の 3 つの可能な構成を示すプログラムを次に示します。

[![グラデーションの円錐](circular-gradients-images/ConicalGradient.png "円錐グラデーション")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

左側にある iOS の画面の効果を示しています、`SKShaderTileMode`設定`Clamp`します。 グラデーションは、赤は反対側の 2 番目の円に最も近い小さな円の内側で開始します。 `Clamp`値も赤の円錐のポイントに続行すると、します。 グラデーションは、青を円の内部以降では最初の円に最も近いが、引き続き大きく円の外側のエッジが青で終わります。

Android の画面に似ていますが、`SKShaderTileMode`の`Repeat`します。 これで、グラデーションが最初の円の内部に開始され、2 つ目の円の外側の終了を明確になります。 `Repeat`設定により、グラデーションをより大きな円の内部に赤でもう一度繰り返します。

UWP の画面では、小さな円が大きい円の内部に完全に移動したときの動作を示します。 グラデーションは、される円錐を停止し、代わりに全体の領域を塗りつぶします。 効果が放射状のグラデーションのようには、小さな円が必ずしも大きな円の中央に配置する場合非対称です。

グラデーションの実用的な有用性は、別の 1 つの円が入れ子になったが、鏡面ハイライトに最適です疑い可能性があります。

## <a name="conical-gradients-for-specular-highlights"></a>反射の光源の円錐のグラデーション

この記事の前半で放射状グラデーションを使用して、反射ハイライトを作成する方法を説明しました。 このため、2 つの点の円錐のグラデーションを使用することもでき、外観したい場合があります。

[![円錐の反射ハイライト](circular-gradients-images/ConicalSpecularHighlight.png "円錐の反射ハイライト")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

非対称の外観には、オブジェクトの丸みのあるサーフェスよりお勧めします。 

描画コード、**円錐鏡面ハイライト**ページは同じ、**放射状鏡面ハイライト**シェーダーを除くページ。

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

2 つの円のセンターがある`offCenter`と`center`します。 円の中心`center`に関連付けられた、円の中心が、全体のボールを包含する radius`offCenter`の 1 つだけのピクセルの半径します。 グラデーションは実質的にその時点で開始し、ボールの端で終了します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
