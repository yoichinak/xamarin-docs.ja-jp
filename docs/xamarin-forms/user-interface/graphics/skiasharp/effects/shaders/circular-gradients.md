---
title: "SkiaSharp の円形のグラデーション" の説明: "円に基づくさまざまな種類のグラデーションについて説明します。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732 author: davidbritch dabritch: ms. date: 08/23/2018 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="the-skiasharp-circular-gradients"></a>SkiaSharp の円形のグラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

クラスは、 [`SKShader`](xref:SkiaSharp.SKShader) 4 つの異なる種類のグラデーションを作成するための静的メソッドを定義します。 [**SkiaSharp 線状グラデーション**](linear-gradient.md)の記事では、メソッドについて説明し [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) ます。 この記事では、他の3種類のグラデーションについて説明します。これらはすべて円に基づいています。

メソッドは、 [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*) 円の中心から長年たグラデーションを作成します。

![放射状グラデーションのサンプル](circular-gradients-images/RadialGradientSample.png)

メソッドは、 [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*) 円の中心を中心とするグラデーションを作成します。

![スイープグラデーションのサンプル](circular-gradients-images/SweepGradientSample.png)

3番目の種類のグラデーションは非常にまれです。 これは2点の円錐グラデーションと呼ばれ、メソッドによって定義され [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) ます。 グラデーションは、1つの円から別の円に向かって拡張されます。

![円錐グラデーションのサンプル](circular-gradients-images/ConicalGradientSample.png)

2つの円のサイズが異なる場合、グラデーションは円錐の形になります。

この記事では、これらのグラデーションについて詳しく説明します。

## <a name="the-radial-gradient"></a>放射状グラデーション

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))メソッドの構文は次のとおりです。

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

オーバーロードには、 [`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 変換行列パラメーターも含まれています。

最初の2つの引数は、円の中心と半径を指定します。 グラデーションはその中心から開始し、ピクセルに対して外側に拡張され `radius` ます。 それ以上の処理 `radius` は引数によって異なり [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) ます。 パラメーターは、 `colors` 2 つ以上の色の配列であり (線形グラデーションメソッドの場合と同じように)、 `colorPos` は 0 ~ 1 の範囲の整数の配列です。 これらの整数は、その線に沿った色の相対位置を示し `radius` ます。 この引数をに設定すると `null` 、色が均等にスペースになります。

を使用して円の塗りつぶしを行う場合は、 `CreateRadialGradient` グラデーションの中心を円の中心に、グラデーションの半径を円の半径に設定できます。 その場合、引数は `SKShaderTileMode` グラデーションのレンダリングに影響しません。 ただし、グラデーションによって塗りつぶされる領域がグラデーションで定義された円より大きい場合、 `SKShaderTileMode` 引数は円の外側で何が起こるかに大きな影響を与えます。

の効果 `SKShaderMode` は、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの [**放射状グラデーション**] ページで説明されています。 このページの XAML ファイルは、 `Picker` 列挙体の3つのメンバーのいずれかを選択できるようにするをインスタンス化し `SKShaderTileMode` ます。

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

分離コードファイルは、キャンバス全体に放射状グラデーションで色を付けます。 グラデーションの中心はキャンバスの中央に設定され、半径は100ピクセルに設定されます。 グラデーションは、黒と白の2つの色で構成されます。

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

このコードは、中央に黒のグラデーションを作成し、中央から白の100ピクセルに徐々にフェードします。 この半径を超えると、次のような引数に依存することに `SKShaderTileMode` なります。

[![放射状グラデーション](circular-gradients-images/RadialGradient.png "放射状グラデーション")](circular-gradients-images/RadialGradient-Large.png#lightbox)

3つのすべてのケースで、グラデーションはキャンバスを塗りつぶします。 左側の [iOS] 画面では、半径を超えたグラデーションが最後の色 (白) で続きます。 これがの結果 `SKShaderTileMode.Clamp` です。 Android の画面には、次の効果が示されてい `SKShaderTileMode.Repeat` ます: 中央から100ピクセルで、グラデーションが最初の色 (黒) で始まります。 グラデーションは100ピクセルごとに繰り返されます。 

右側の [ユニバーサル Windows プラットフォーム] 画面に、に `SKShaderTileMode.Mirror` よってグラデーションの方向が交互に示されます。 最初のグラデーションは、中央の黒から100ピクセルの半径の白までです。 次のは、200ピクセル半径の場合は100ピクセル半径から黒になり、次のグラデーションは再び反転されます。

放射状グラデーションでは、3つ以上の色を使用できます。 **レインボー弧のグラデーション**サンプルでは、レインボーの色に対応する8色の配列を作成し、赤で終了します。また、次の8つの位置値の配列も作成します。

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

キャンバスの幅と高さの最小値が1000であるとします。これは、値が250であることを意味し `rainbowWidth` ます。 `outerRadius`との `innerRadius` 値はそれぞれ1000と750に設定されます。 これらの値は、配列の計算に使用されます。 `positions` 8 つの値の範囲は 0.75 f ~ 1 です。 `radius`値は、円の描画に使用されます。 875の値は、250ピクセルのストローク幅が750ピクセルの半径と1000ピクセルの半径の間にあることを意味します。

[![レインボー弧 (グラデーション)](circular-gradients-images/RainbowArcGradient.png "レインボー弧 (グラデーション)")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

このグラデーションでキャンバス全体を塗りつぶすと、内側の半径内に赤色で表示されます。 これは、 `positions` 配列の先頭が0ではないためです。 最初の色は、0から1番目の配列値までのオフセットに使用されます。 グラデーションも、外側の半径を超えて赤になります。 これが、タイルモードの結果です `Clamp` 。 太線を描画するためにグラデーションが使用されるため、これらの赤い領域は表示されません。

## <a name="radial-gradients-for-masking"></a>マスクの放射状グラデーション

線状グラデーションと同様に、放射状グラデーションには透明色または部分的に透明な色を組み込むことができます。 この機能は、イメージの一部を非表示にしてイメージの別の部分を強調する_マスキング_と呼ばれるプロセスに便利です。

[**放射状グラデーションマスク**] ページに例が表示されます。 プログラムによって、いずれかのリソースビットマップが読み込まれます。 `CENTER`およびフィールドは、 `RADIUS` ビットマップの検査によって決定され、強調表示する必要がある領域を参照します。 ハンドラーは、 `PaintSurface` ビットマップを表示する四角形を計算してから、その四角形に表示します。

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

ビットマップを描画した後、いくつかの単純なコードは `CENTER` と `RADIUS` をとに変換し `center` `radius` ます。これは、ビットマップ内で強調表示されている領域を参照します。 これらの値は、その中心と半径を持つ放射状グラデーションを作成するために使用されます。 2色は、中央では透明で始まり、半径の最初の60% になります。 次に、グラデーションが白にフェードします。

[![放射状グラデーションマスク](circular-gradients-images/RadialGradientMask.png "放射状グラデーションマスク")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

この方法は、ビットマップをマスクするのに最適な方法ではありません。 問題として、マスクには、キャンバスの背景と一致するように選択された白の色が使用されます。 背景が他の色の場合、またはグラデーション自体の場合は、その背景が &mdash; &mdash; 一致しません。 マスクに対するより優れたアプローチについては、 [SkiaSharp Porter の blend モード](../blend-modes/porter-duff.md)に関する記事をご覧ください。

## <a name="radial-gradients-for-specular-highlights"></a>反射の光源の放射状グラデーション

光が丸い表面になると、さまざまな方向に光が反射しますが、光の一部はビューアーの目に直接バウンスされます。 これにより、多くの場合、_反射の強調表示_と呼ばれる、表面上のあいまいな白い領域の外観が作成されます。

3次元グラフィックスでは、光のパスと網掛けを決定するために使用されるアルゴリズムによって、反射の光源が強調表示されることがよくあります。 2次元グラフィックスでは、3D サーフェイスの外観を提案するために反射の光源が追加されることがあります。 反射の光源を使用すると、赤の平らな円を丸い赤いボールに変換できます。

**放射状反射の強調表示**ページでは、そのために放射状グラデーションが使用されます。 この `PaintSurface` ハンドラーは、円の半径を計算することによって、2つの `SKPoint` 値 &mdash; a と、 `center` `offCenter` 中央と円の左上隅の中間にあるを計算します。

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

この呼び出しでは、 `CreateRadialGradient` その地点から開始し、 `offCenter` 半径の半分の距離で赤で終了するグラデーションを作成します。 次のように表示されます。

[![放射状反射の強調表示](circular-gradients-images/RadialSpecularHighlight.png "放射状反射の強調表示")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

このグラデーションをよく見ると、欠点があると判断する場合があります。 グラデーションは特定の点を中心にしています。また、丸みのある表面を反射するために、少し対称ではないようにしたい場合もあります。 そのような場合は、次に示す反射の強調表示を、[**反射の光源の円錐のグラデーション**](#conical-gradients-for-specular-highlights)のセクションで使用することをお勧めします。

## <a name="the-sweep-gradient"></a>スイープグラデーション

メソッドには、 [`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[])) すべてのグラデーション作成メソッドの最も単純な構文があります。

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

これは、中心、色の配列、および色の位置にすぎません。 グラデーションは、中心点の右側にあり、中央の約360°スイープされます。 パラメーターがないことに注意 `SKShaderTileMode` してください。

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix))マトリックス変換パラメーターを使用したオーバーロードを使用することもできます。 グラデーションに回転変換を適用して、開始点を変更することができます。 また、スケール変換を適用して、方向を時計回りから反時計回りに変更することもできます。

**スイープグラデーション**ページではスイープグラデーションを使用して、ストロークの幅が50ピクセルの円に色を付けます。

[![スイープグラデーション](circular-gradients-images/SweepGradient.png "スイープグラデーション")](circular-gradients-images/SweepGradient-Large.png#lightbox)

クラスは、 `SweepGradientPage` 異なる色相値を持つ8色の配列を定義します。 配列の先頭と末尾が赤 (色合いの値が0または 360) であることに注意してください。これは、スクリーンショットの右端に表示されます。

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

このプログラムでは、 `TapGestureRecognizer` ハンドラーの末尾で一部のコードを有効にするを実装することもでき `PaintSurface` ます。 このコードでは、同じグラデーションを使用してキャンバスを塗りつぶします。

[![スイープグラデーションがいっぱいです](circular-gradients-images/SweepGradientFull.png "スイープグラデーションがいっぱいです")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

これらのスクリーンショットは、グラデーションによって色分けされた領域を塗りつぶすことを示しています。 グラデーションの先頭と末尾が同じ色でない場合は、中心点の右側に不連続性があります。

## <a name="the-two-point-conical-gradient"></a>2点の円錐のグラデーション

[`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))メソッドの構文は次のとおりです。

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

これらのパラメーターは、_開始_円と_終了_円と呼ばれる2つの円の中心点と半径で始まります。 残りの3つのパラメーターは、およびの場合と同じです `CreateLinearGradient` `CreateRadialGradient` 。 オーバーロードには、 [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) マトリックス変換が含まれます。

グラデーションは開始円から始まり、最後の円で終わります。 パラメーターは、 `SKShaderTileMode` 2 つの円の後の動作を制御します。 2点の円錐のグラデーションは、領域を完全に塗りつぶす唯一のグラデーションです。 2つの円の半径が同じ場合、グラデーションは、円の直径と同じ幅の四角形に制限されます。 2つの円の半径が異なる場合、グラデーションはコーンを形成します。

2点の円錐グラデーションを試してみることをお勧めします。これにより、**円錐のグラデーション**ページがから派生して、2つ `InteractivePage` の丸い半径に2つのタッチポイントを移動できるようになります。

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

分離コードファイルは、 `TouchPoint` 50 と100の固定半径を持つ2つのオブジェクトを定義します。

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

`colors`配列は、赤、緑、および青です。 ハンドラーの下部のコードは、 `PaintSurface` 2 つのタッチポイントを黒の円として描画し、グラデーションが妨げされないようにします。

呼び出しでは `DrawRect` 、グラデーションを使用してキャンバス全体の色が設定されることに注意してください。 ただし、一般的なケースでは、キャンバスの多くはグラデーションによって色分けされていません。 考えられる3つの構成を示したプログラムを次に示します。

[![円錐のグラデーション](circular-gradients-images/ConicalGradient.png "円錐のグラデーション")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

左側の iOS 画面は、の設定の効果を示して `SKShaderTileMode` `Clamp` います。 グラデーションは、2番目の円の近くにある小さい円の端の内側に赤で始まります。 また、値を指定すると、 `Clamp` 赤が円錐のポイントまで続きます。 グラデーションは、最初の円の一番近くにある大きな円の外縁では青で終わりますが、その円の内側では青で続きます。

Android の画面は似ていますが、は `SKShaderTileMode` `Repeat` です。 ここで、グラデーションが最初の円の内側から始まり、2番目の円の外側にあることがわかりやすくなっています。 この `Repeat` 設定により、グラデーションが、大きな円の内側に赤で繰り返されます。

UWP 画面には、小さい円が大きな円の内側に全体を移動したときの動作が表示されます。 グラデーションは、コーンではなく、領域全体を占めるようになります。 この効果は放射状グラデーションに似ていますが、小さい円が大きな円の中で正確に中央揃えになっていない場合は、非対称です。

ある円が別の円で入れ子になっている場合、グラデーションの実用的な有用性がわからないかもしれませんが、反射の強調表示に適しています。

## <a name="conical-gradients-for-specular-highlights"></a>反射の光源の円錐のグラデーション

この記事の前半では、放射状グラデーションを使用して反射の強調表示を作成する方法を説明しました。 また、この目的に2点の円錐グラデーションを使用することもできます。

[![円錐の反射の強調表示](circular-gradients-images/ConicalSpecularHighlight.png "円錐の反射の強調表示")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

非対称の外観では、オブジェクトの丸いサーフェイスがわかりやすくなります。 

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

2つの円には、との中心があり `offCenter` `center` ます。 中央の円は、 `center` ボール全体を含む半径に関連付けられていますが、中央にある円の `offCenter` 半径は1ピクセルだけです。 グラデーションは、その時点から効果的に始まり、ボールの端で終了します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
