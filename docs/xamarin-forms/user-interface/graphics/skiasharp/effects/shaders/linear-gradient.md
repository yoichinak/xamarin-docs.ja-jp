---
title: SkiaSharp の線形グラデーション
description: 線のストロークを描画する方法を説明またはグラデーションによる塗りつぶし領域は、2 つの色の段階的なブレンドで構成されます。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 290e533e54b2ee150b94d9fb6b0f5119324f9cf0
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306501"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp の線形グラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKPaint`](xref:SkiaSharp.SKPaint)クラスは、線を描画したり、純色で塗りつぶしたりするために使用される[`Color`](xref:SkiaSharp.SKPaint.Color)プロパティを定義します。 また、色を段階的に混ぜ合わせた_グラデーション_を使用して線を描画したり、塗りつぶしたりすることができます。

![線状グラデーションのサンプル](linear-gradient-images/LinearGradientSample.png "線状グラデーションのサンプル")

最も基本的な種類のグラデーションは_線状_グラデーションです。 色のブレンドは、ある点から別の点への線 (_グラデーション線_と呼ばれます) に発生します。 グラデーションの線に対して垂直な線では、同じ色があります。 線形グラデーションを作成するには、2つの静的な[`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*)メソッドのいずれかを使用します。 2 つのオーバー ロードの違いは、行列変換を含む 1 つと、もう一方にないことができます。 

これらのメソッドは、`SKPaint`の[`Shader`](xref:SkiaSharp.SKPaint.Shader)プロパティに設定した[`SKShader`](xref:SkiaSharp.SKShader)型のオブジェクトを返します。 `Shader` プロパティが null 以外の場合は、`Color` プロパティをオーバーライドします。 線またはこの `SKPaint` オブジェクトを使用して塗りつぶされた領域は、純色ではなくグラデーションに基づいています。

> [!NOTE]
> `DrawBitmap` の呼び出しに `SKPaint` オブジェクトを含める場合、`Shader` プロパティは無視されます。 `SKPaint` の `Color` プロパティを使用して、ビットマップを表示するための透明度を設定できます ( [SkiaSharp ビットマップの表示](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)に関する記事で説明されています) が、`Shader` プロパティを使用して、グラデーション透明度のビットマップを表示することはできません。 グラデーションの ohp シートでビットマップを表示するためのその他の手法もあります。これらの方法については、 [SkiaSharp の丸いグラデーション](circular-gradients.md#radial-gradients-for-masking)と[SkiaSharp の合成と blend の各モード](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)に関する記事を参照してください。

## <a name="corner-to-corner-gradients"></a>角のグラデーション

多くの場合、線形グラデーションは四角形の 1 つ上にある間に拡張します。 開始ポイントが四角形の左上隅の場合は、グラデーションを拡張できます。

- 左上隅にある垂直方向に
- 右上隅の水平方向に
- 右下隅に斜め

対角線方向の線形グラデーションは、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**SkiaSharp シェーダーとその他の効果**セクションの最初のページで示されています。 **コーナーとコーナーのグラデーション**ページでは、コンストラクターに `SKCanvasView` が作成されます。 `PaintSurface` ハンドラーは `using` ステートメントで `SKPaint` オブジェクトを作成し、キャンバスの中央に300ピクセルの四角形の四角形を定義します。

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    ···
    public CornerToCornerGradientPage ()
    {
        Title = "Corner-to-Corner Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ···
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Create 300-pixel square centered rectangle
            float x = (info.Width - 300) / 2;
            float y = (info.Height - 300) / 2;
            SKRect rect = new SKRect(x, y, x + 300, y + 300);

            // Create linear gradient from upper-left to lower-right
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { SKColors.Red, SKColors.Blue },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Repeat);

            // Draw the gradient on the rectangle
            canvas.DrawRect(rect, paint);
            ···
        }
    }
}
```

`SKPaint` の `Shader` プロパティには、静的な `SKShader.CreateLinearGradient` メソッドからの戻り値 `SKShader` が割り当てられます。 5 つの引数は次のとおりです。

- グラデーションの開始点は、ここでは、四角形の左上隅に設定
- グラデーションの終了点は四角形の右上隅にここで設定
- 2 つ以上の色グラデーションに影響を与えるの配列
- グラデーション線内の色の相対位置を示す `float` 値の配列。
- グラデーションがグラデーション線の両端を越えてどのように動作するかを示す[`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode)列挙体のメンバー

グラデーションオブジェクトが作成された後、`DrawRect` メソッドは、シェーダーを含む `SKPaint` オブジェクトを使用して、300ピクセルの四角形の四角形を描画します。 ここで、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されています。

[![コーナーとコーナーのグラデーション](linear-gradient-images/CornerToCornerGradient.png "コーナーとコーナーのグラデーション")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

グラデーション ラインは、最初の 2 つの引数として指定した 2 点によって定義されます。 これらの点は、グラデーションと共に表示されるグラフィカルオブジェクトでは_なく_、_キャンバス_に対して相対的であることに注意してください。 グラデーションの線に沿った色は右下にある青色の左上にある赤から徐々 に移行します。 グラデーションの線に対して垂直な任意の行が、一定の色。

4番目の引数として指定された `float` 値の配列は、色の配列と一対一で対応しています。 値は、その色が発生するグラデーション ラインに沿って相対位置を示します。 ここで、0は、`Red` がグラデーション線の先頭に出現することを意味します。1は、`Blue` が行の末尾にあることを示します。 数字は、昇順である必要があります、および 0 ~ 1 の範囲内で指定する必要があります。 その範囲にそうでない場合は、その範囲内に調整されます。

配列内の 2 つの値は、0 と 1 以外のものに設定できます。 次のことを試してみてください。

```csharp
new float[] { 0.25f, 0.75f }
```

グラデーション ライン全体の第 1 四半期は純粋な赤、および最後の四半期が純粋な青。 赤と青の組み合わせは、グラデーションの線の中央の半分に制限されています。

一般に、スペースを 1 に均等に 0 からこれらの位置値にします。 その場合は、`CreateLinearGradient`の4番目の引数として `null` を指定するだけです。

このグラデーションは 300 ピクセルの正方形の四角形の 2 つの角の間で定義されて、その四角形の入力に制限はありません。 **[コーナーとコーナーのグラデーション]** ページには、ページ上のタップまたはマウスクリックに応答する追加のコードが含まれています。 `drawBackground` フィールドは、各タップで `true` と `false` の間で切り替えられます。 値が `true`場合、`PaintSurface` ハンドラーは同じ `SKPaint` オブジェクトを使用してキャンバス全体を塗りつぶし、次に、小さい四角形を示す黒い四角形を描画します。 

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    bool drawBackground;

    public CornerToCornerGradientPage ()
    {
        ···
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
        ···
        using (SKPaint paint = new SKPaint())
        {
            ···
            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                canvas.DrawRect(info.Rect, paint);

                // Outline the smaller rectangle
                paint.Shader = null;
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                canvas.DrawRect(rect, paint);
            }
        }
    }
}
```

ここでは、画面をタップした後は表示されます。

[![コーナーとコーナーのグラデーションがいっぱいです](linear-gradient-images/CornerToCornerGradientFull.png "コーナーとコーナーのグラデーションがいっぱいです")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

あるグラデーション ラインを定義するポイントを超えたと同じパターンで繰り返さグラデーションに注意してください。 この繰り返しが発生するのは、`CreateLinearGradient` の最後の引数が `SKShaderTileMode.Repeat`ためです。 (するオプションが表示されます、他の直後です。)

またを使用するグラデーションの線を指定する点が一意でないことに注意してください。 グラデーションの線に対して垂直な線はあるので、同じ効果を指定できるグラデーションの線の無限の数が、同じの色です。 たとえば、四角形を水平方向のグラデーションを使用して入力するときは、左と右上隅で、左および右下隅で、またはであっても、それらの行を並列である 2 つのポイントを指定できます。

## <a name="interactively-experiment"></a>対話形式で実験します。

**対話型の線形グラデーション**ページで線形グラデーションを対話的に試すことができます。 このページでは、 [**3 つの方法で円弧を描画する方法**](../../curves/arcs.md)について、記事で紹介した `InteractivePage` クラスを使用します。`InteractivePage` は、指やマウスで移動できる `TouchPoint` オブジェクトのコレクションを維持するために[`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)イベントを処理します。

XAML ファイルは、`TouchEffect` を `SKCanvasView` の親にアタッチします。また、 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode)列挙体の3つのメンバーのいずれかを選択できる `Picker` も含まれています。

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.InteractiveLinearGradientPage"
                       Title="Interactive Linear Gradient">
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

分離コードファイル内のコンストラクターは、線形グラデーションの始点と終点の2つの `TouchPoint` オブジェクトを作成します。 `PaintSurface` ハンドラーは、次の3つの色の配列を定義します (グラデーションの場合、赤から緑から青)。 `Picker`から現在の `SKShaderTileMode` を取得します。

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    public InteractiveLinearGradientPage ()
    {
        InitializeComponent ();

        touchPoints = new TouchPoint[2];

        for (int i = 0; i < 2; i++)
        { 
            touchPoints[i] = new TouchPoint
            {
                Center = new SKPoint(100 + i * 200, 100 + i * 200)
            };
        }

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
            paint.Shader = SKShader.CreateLinearGradient(touchPoints[0].Center,
                                                         touchPoints[1].Center,
                                                         colors,
                                                         null,
                                                         tileMode);
            canvas.DrawRect(info.Rect, paint);
        }
        ···
    }
}
```

`PaintSurface` ハンドラーは、すべての情報から `SKShader` オブジェクトを作成し、それを使用してキャンバス全体の色を設定します。 `float` 値の配列は `null`に設定されます。 それ以外の場合、3 つの色を均等にスペースを場合は、0、0.5 および 1 の値を持つ配列にはそのパラメーターを設定します。

`PaintSurface` ハンドラーの大部分は、いくつかのオブジェクトを表示するために用意されています。タッチポイントは、輪郭の円、グラデーションの線、およびタッチポイントのグラデーション線に垂直線です。

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
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

            // Draw gradient line connecting touchpoints
            canvas.DrawLine(touchPoints[0].Center, touchPoints[1].Center, paint);

            // Draw lines perpendicular to the gradient line
            SKPoint vector = touchPoints[1].Center - touchPoints[0].Center;
            float length = (float)Math.Sqrt(Math.Pow(vector.X, 2) +
                                            Math.Pow(vector.Y, 2));
            vector.X /= length;
            vector.Y /= length;
            SKPoint rotate90 = new SKPoint(-vector.Y, vector.X);
            rotate90.X *= 200;
            rotate90.Y *= 200;

            canvas.DrawLine(touchPoints[0].Center, 
                            touchPoints[0].Center + rotate90, 
                            paint);

            canvas.DrawLine(touchPoints[0].Center,
                            touchPoints[0].Center - rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center + rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center - rotate90,
                            paint);
        }
    }
}
```

グラデーションに 2 つの配慮を結ぶ線は、描画するために簡単ですが、垂直線が特定の作業が必要です。 グラデーション ラインは、ベクトルに変換および 1 つの単位の長さに正規化されて、90 度回転し。 そのベクトルは 200 ピクセルの長さが付与されます。 垂直グラデーション ラインになるように、タッチ ポイントから拡張する 4 つの行を描画するために使用されます。

垂直の線は、先頭とグラデーションの終了と一致します。 これらの行を超えて行われる処理は、`SKShaderTileMode` 列挙の設定によって異なります。

[![対話型線形グラデーション](linear-gradient-images/InteractiveLinearGradient.png "対話型線形グラデーション")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

3つのスクリーンショットは、 [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode)の3つの異なる値の結果を示しています。 IOS のスクリーンショットに `SKShaderTileMode.Clamp`が表示されます。これにより、グラデーションの境界線の色だけが拡張されます。 Android のスクリーンショットの `SKShaderTileMode.Repeat` オプションは、グラデーションパターンがどのように繰り返されるかを示しています。 UWP スクリーンショットの `SKShaderTileMode.Mirror` オプションもパターンを繰り返しますが、パターンは毎回反転されます。その結果、カラーの不連続化は行われません。

## <a name="gradients-on-gradients"></a>グラデーションのグラデーション

`SKShader` クラスは、`Dispose`以外のパブリックプロパティまたはメソッドを定義しません。 そのため、静的メソッドによって作成された `SKShader` オブジェクトは変更できません。 2 つのオブジェクトの同じグラデーションを使用する場合でもは、グラデーションを多少する可能性があります。 これを行うには、新しい `SKShader` オブジェクトを作成する必要があります。

**[グラデーションのテキスト]** ページには、テキストと、似たグラデーションで色分けされたバックグラウンドが表示されます。

[![グラデーションのテキスト](linear-gradient-images/GradientText.png "グラデーションのテキスト")](linear-gradient-images/GradientText-Large.png#lightbox)

グラデーションで唯一の違いは、始点と終点は。 テキストを表示するために使用するグラデーションは、テキストの外接する四角形の角の丸みの 2 つの点に基づいています。 バック グラウンドでの 2 つの点は、キャンバス全体に基づいています。 コードは次のとおりです。

```csharp
public class GradientTextPage : ContentPage
{
    const string TEXT = "GRADIENT";

    public GradientTextPage ()
    {
        Title = "Gradient Text";

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
            // Create gradient for background
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                new SKPoint(info.Width, info.Height),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw background
            canvas.DrawRect(info.Rect, paint);

            // Set TextSize to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Shift textBounds by that amount
            textBounds.Offset(xText, yText);

            // Create gradient for text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(textBounds.Left, textBounds.Top),
                                new SKPoint(textBounds.Right, textBounds.Bottom),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

`SKPaint` オブジェクトの `Shader` プロパティは、背景を覆うようにグラデーションを表示するために最初に設定されます。 グラデーションのポイントは、キャンバスの左上隅および右下隅に設定されます。

このコードは `SKPaint` オブジェクトの `TextSize` プロパティを設定して、キャンバスの幅の90% にテキストが表示されるようにします。 テキストの境界は、テキストを中央に配置するために `DrawText` メソッドに渡す `xText` と `yText` の値を計算するために使用されます。

ただし、2番目の `CreateLinearGradient` 呼び出しのグラデーションポイントは、表示されているキャンバスを基準として、テキストの左上および右下隅を参照する必要があります。 これは、`textBounds` 四角形を同じ `xText` と `yText` 値でシフトすることで実現されます。

```csharp
textBounds.Offset(xText, yText);
```

今すぐ開始し、グラデーションの始点と終点を設定する四角形の左上隅および右下隅を使用できます。

## <a name="animating-a-gradient"></a>グラデーションをアニメーション化

グラデーションをアニメーション化するいくつかの方法はあります。 1 つの方法では、始点と終点をアニメーション化します。 **グラデーションアニメーション**ページは、キャンバスの中央にある円内の2つの点を移動します。 この円の半径が半分の幅または高さのキャンバス、小さい方です。 この円では始点と終点が逆になっており、グラデーションは `Mirror` タイルモードで白から黒になります。

[![グラデーションアニメーション](linear-gradient-images/GradientAnimation.png "グラデーションアニメーション")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

コンストラクターは `SKCanvasView`を作成します。 `OnAppearing` メソッドと `OnDisappearing` メソッドは、アニメーションのロジックを処理します。

```csharp
public class GradientAnimationPage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    double angle;
    Stopwatch stopwatch = new Stopwatch();

    public GradientAnimationPage()
    {
        Title = "Gradient Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 3000;
        angle = 2 * Math.PI * (stopwatch.ElapsedMilliseconds % duration) / duration;
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

`OnTimerTick` メソッドは、3秒ごとに0から2πのアニメーション化された `angle` 値を計算します。 

グラデーションの 2 つの点を計算する方法の 1 つを次に示します。 `vector` という名前の `SKPoint` 値は、キャンバスの中心から円の半径のポイントまで拡張されるように計算されます。 このベクトルの方向は、角度の正弦と余弦の値に基づきます。 2 つの反対側のグラデーション ポイントが計算されます。 1 つのポイントが、中心点からベクトルを減算することで計算され、その他のポイントが中心点に、ベクターを加算して計算されます。

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
            int radius = Math.Min(info.Width, info.Height) / 2;
            SKPoint vector = new SKPoint((float)(radius * Math.Cos(angle)),
                                         (float)(radius * Math.Sin(angle)));

            paint.Shader = SKShader.CreateLinearGradient(
                                center - vector,
                                center + vector,
                                new SKColor[] { SKColors.White, SKColors.Black },
                                null,
                                SKShaderTileMode.Mirror);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

少し異なる方法では、少ないコードが必要です。 この方法では、最後の引数としてマトリックス変換を使用して[`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))のオーバーロードメソッドを使用します。 この方法は、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルのバージョンです。

```csharp
public class GradientAnimationPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                info.Width < info.Height ? new SKPoint(info.Width, 0) : 
                                                           new SKPoint(0, info.Height),
                                new SKColor[] { SKColors.White, SKColors.Black },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Mirror,
                                SKMatrix.MakeRotation((float)angle, info.Rect.MidX, info.Rect.MidY));

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

キャンバスの幅が高さよりも小さい場合、2つのグラデーションポイントは (0, 0) と (`info.Width`, 0) に設定されます。 `CreateLinearGradient` に最後の引数として渡された回転変換は、画面の中央を中心に2つの点を効果的に回転します。

場合の角度は 0、回転はありませんは、キャンバスの左上隅および右の角のグラデーションの 2 点に注意します。 これらの点は、前の `CreateLinearGradient` の呼び出しに示されているように計算されたグラデーションポイントと同じではありません。 ただし、これらの点は、キャンバスの中心を bisects する横線に_平行_しています。その結果、グラデーションは同じになります。

**レインボーグラデーション**

**レインボーグラデーション**ページでは、キャンバスの左上隅から右下隅にレインボーが描画されます。 ただし実際虹のようにこの虹グラデーションがありません。 カーブしてではなく、直線ですが、0 から 360 色相値間で循環によって決定される 8 の HSL (色合い-鮮やかさ-明るさ) の色に基づいて設定されます。

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

このコードは、次に示す `PaintSurface` ハンドラーの一部です。 ハンドラーは、キャンバスの左上隅から右下隅まで 6 面のポリゴンを定義するパスを作成して開始します。

```csharp
public class RainbowGradientPage : ContentPage
{
    public RainbowGradientPage ()
    {
        Title = "Rainbow Gradient";

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

        using (SKPath path = new SKPath())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 2f;

            // Create path from upper-left to lower-right corner
            path.MoveTo(0, 0);
            path.LineTo(rainbowWidth / 2, 0);
            path.LineTo(info.Width, info.Height - rainbowWidth / 2);
            path.LineTo(info.Width, info.Height);
            path.LineTo(info.Width - rainbowWidth / 2, info.Height);
            path.LineTo(0, rainbowWidth / 2);
            path.Close();

            using (SKPaint paint = new SKPaint())
            {
                SKColor[] colors = new SKColor[8];

                for (int i = 0; i < colors.Length; i++)
                {
                    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
                }

                paint.Shader = SKShader.CreateLinearGradient(
                                    new SKPoint(0, rainbowWidth / 2), 
                                    new SKPoint(rainbowWidth / 2, 0),
                                    colors,
                                    null,
                                    SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

`CreateLinearGradient` メソッドの2つのグラデーションポイントは、このパスを定義する2つの点に基づいています。両方の点が左上隅の近くにあります。 キャンバスの上端に 1 つ目は、2 番目は、キャンバスの左端にします。 結果は次のとおりです。

[![レインボーグラデーションの不具合](linear-gradient-images/RainbowGradientFaulty.png "レインボーグラデーションの不具合")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

これは、興味深いイメージが目的では非常にではありません。 問題は、線形グラデーションを作成するときに、一定の色の行があるグラデーションの線に対して垂直です。 グラデーション ラインは、図は、上および左の辺に触れて、その行がない、図の右下隅に拡張する端に垂直な通常のポイントに基づいています。 この方法は、キャンバスが正方形の場合にのみ機能します。

適切な虹のグラデーションを作成するには、グラデーション ラインを虹の端に垂直でなければなりません。 これより複雑な計算です。 図の長い側に並列なベクトルを定義する必要があります。 ベクターは 90 度回転が側に直角になるようにします。 次に、`rainbowWidth`で乗算して、図形の幅として長くなります。 さらに、ベクターを指すおよび図では、横にある点に基づいて 2 つのグラデーションのポイントが計算します。 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**レインボーグラデーション**ページに表示されるコードを次に示します。

```csharp
public class RainbowGradientPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPath path = new SKPath())
        {
            ···
            using (SKPaint paint = new SKPaint())
            {
                ···
                // Vector on lower-left edge, from top to bottom 
                SKPoint edgeVector = new SKPoint(info.Width - rainbowWidth / 2, info.Height) - 
                                     new SKPoint(0, rainbowWidth / 2);

                // Rotate 90 degrees counter-clockwise:
                SKPoint gradientVector = new SKPoint(edgeVector.Y, -edgeVector.X);

                // Normalize
                float length = (float)Math.Sqrt(Math.Pow(gradientVector.X, 2) +
                                                Math.Pow(gradientVector.Y, 2));
                gradientVector.X /= length;
                gradientVector.Y /= length;

                // Make it the width of the rainbow
                gradientVector.X *= rainbowWidth;
                gradientVector.Y *= rainbowWidth;

                // Calculate the two points
                SKPoint point1 = new SKPoint(0, rainbowWidth / 2);
                SKPoint point2 = point1 + gradientVector;

                paint.Shader = SKShader.CreateLinearGradient(point1,
                                                             point2,
                                                             colors,
                                                             null,
                                                             SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

今すぐ虹色は、図に揃えて配置されます。

[![レインボーグラデーション](linear-gradient-images/RainbowGradient.png "レインボーグラデーション")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**無限大色**

**[無限大色]** ページでは、レインボーグラデーションも使用されます。 このページでは、記事「 [**3 種類のベジエ曲線**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)」で説明されているパスオブジェクトを使用して無限大記号を描画します。 継続的に、イメージ全体をスイープするアニメーション虹グラデーションでは、イメージの色が、設定します。

コンストラクターは、無限大記号を記述する `SKPath` オブジェクトを作成します。 パスが作成されると、コンス トラクターは、パスの四角形の境界も取得できます。 次に、`gradientCycleLength`という値を計算します。 グラデーションが `pathBounds` 四角形の左上隅と右下隅に基づいている場合、この `gradientCycleLength` 値はグラデーションパターンの横幅の合計です。

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    SKCanvasView canvasView;

    // Path information 
    SKPath infinityPath;
    SKRect pathBounds;
    float gradientCycleLength;

    // Gradient information
    SKColor[] colors = new SKColor[8];
    ···

    public InfinityColorsPage ()
    {
        Title = "Infinity Colors";

        // Create path for infinity sign
        infinityPath = new SKPath();
        infinityPath.MoveTo(0, 0);                                  // Center
        infinityPath.CubicTo(  50,  -50,   95, -100,  150, -100);   // To top of right loop
        infinityPath.CubicTo( 205, -100,  250,  -55,  250,    0);   // To far right of right loop
        infinityPath.CubicTo( 250,   55,  205,  100,  150,  100);   // To bottom of right loop
        infinityPath.CubicTo(  95,  100,   50,   50,    0,    0);   // Back to center  
        infinityPath.CubicTo( -50,  -50,  -95, -100, -150, -100);   // To top of left loop
        infinityPath.CubicTo(-205, -100, -250,  -55, -250,    0);   // To far left of left loop
        infinityPath.CubicTo(-250,   55, -205,  100, -150,  100);   // To bottom of left loop
        infinityPath.CubicTo( -95,  100, - 50,   50,    0,    0);   // Back to center
        infinityPath.Close();

        // Calculate path information 
        pathBounds = infinityPath.Bounds;
        gradientCycleLength = pathBounds.Width +
            pathBounds.Height * pathBounds.Height / pathBounds.Width;

        // Create SKColor array for gradient
        for (int i = 0; i < colors.Length; i++)
        {
            colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
        }

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

また、このコンストラクターは、レインボーの `colors` 配列と `SKCanvasView` オブジェクトを作成します。

`OnAppearing` メソッドと `OnDisappearing` メソッドのオーバーライドにより、アニメーションのオーバーヘッドが発生します。 `OnTimerTick` メソッドは、`offset` フィールドを0から2秒ごと `gradientCycleLength` にアニメーション化します。

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    float offset;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 2;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        offset = (float)(gradientCycleLength * progress);
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

最後に、`PaintSurface` ハンドラーによって無限大記号がレンダリングされます。 パスには、(0, 0) の中心点を囲む負の座標と正の座標が含まれているので、キャンバス上の `Translate` 変換を使用して、中心にシフトします。 変換変換の後に `Scale` 変換が適用されます。スケールファクターを適用すると、キャンバスの幅と高さの95% 以内に無限大記号を可能な限り大きくすることができます。 

`STROKE_WIDTH` 定数が、パス外の四角形の幅と高さに追加されることに注意してください。 パスは、レンダリングされた無限大サイズのサイズが 4 辺すべてにつきその幅の半分だけ増加は、この幅の行で線は。

```csharp
public class InfinityColorsPage : ContentPage
{
    const int STROKE_WIDTH = 50;
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transforms to shift path to center and scale to canvas size
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.95f * 
            Math.Min(info.Width / (pathBounds.Width + STROKE_WIDTH),
                     info.Height / (pathBounds.Height + STROKE_WIDTH)));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = STROKE_WIDTH;
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(pathBounds.Left, pathBounds.Top),
                                new SKPoint(pathBounds.Right, pathBounds.Bottom),
                                colors,
                                null,
                                SKShaderTileMode.Repeat,
                                SKMatrix.MakeTranslation(offset, 0));

            canvas.DrawPath(infinityPath, paint);
        }
    }
}
```

`SKShader.CreateLinearGradient`の最初の2つの引数として渡された点を確認します。 これらのポイントは、外接する四角形の元のパスに基づいています。 最初のポイントは (&ndash;250、&ndash;100) で、2番目のポイントは (250、100) です。 SkiaSharp の内部では、それらのポイントの対象で、現在のキャンバス変換揃えてアライン正しく表示されている無限大記号。

`CreateLinearGradient`の最後の引数がない場合は、無限大記号の左上から右下までのレインボーグラデーションが表示されます。 (実際には、グラデーションまで左上隅から外接する四角形の右上隅にあります。 表示される無限大記号は、すべての辺の `STROKE_WIDTH` 値の半分で外接する四角形よりも大きくなります。 グラデーションは先頭と末尾の両方で赤で、グラデーションは `SKShaderTileMode.Repeat`で作成されるため、この違いは顕著ではありません)。

`CreateLinearGradient`の最後の引数を使用すると、グラデーションパターンがイメージ全体を連続的にスイープします。

[![無限大色](linear-gradient-images/InfinityColors.png "無限大色")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>透明度とグラデーション

色のグラデーションに影響を与えるには、透過性を組み込むことができます。 別に 1 つの色からフェードするグラデーションの代わりにグラデーションは透過的に色からにフェードことができます。 

この手法は、いくつかの興味深い効果を使用できます。 典型的な例の 1 つは、リフレクションを使用してグラフィカル オブジェクトを示しています。

[![反射のグラデーション](linear-gradient-images/ReflectionGradient.png "反射のグラデーション")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

テキストを上下の色は 50% の上部の下部にある完全に透過的に透明なグラデーションを使用して設定します。 これらのレベルの透過性は、および 0x80 のアルファ値を 0 に関連付けられます。

**[反射のグラデーション]** ページの `PaintSurface` ハンドラーは、テキストのサイズをキャンバスの幅の90% に拡大します。 次に、`xText` と `yText` 値を計算して、テキストを中央揃えに配置しますが、ページの垂直方向中央に対応するベースライン上に配置します。

```csharp
public class ReflectionGradientPage : ContentPage
{
    const string TEXT = "Reflection";

    public ReflectionGradientPage ()
    {
        Title = "Reflection Gradient";

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

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

これらの `xText` と `yText` の値は、`PaintSurface` ハンドラーの下部にある `DrawText` の呼び出しに反映されたテキストを表示するために使用される値と同じです。 ただし、このコードの直前には、`SKCanvas`の `Scale` メソッドの呼び出しが表示されます。 この `Scale` メソッドは、水平方向に 1 (何もしない) でスケールしますが、垂直方向に1を &ndash;します。これにより、すべての要素が反転されます。 回転の中心は、ポイント (0, `yText`) に設定されます。 `yText` はキャンバスの垂直方向であり、もともとは2で割った `info.Height` として計算されます。

Skia がキャンバスの変換は、前に、グラフィカル オブジェクトに色をグラデーションを使用することに留意してください。 未反映のテキストが描画されると、表示されるテキストに対応するように `textBounds` 四角形がシフトされます。

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient` の呼び出しは、その四角形の上部から下部までのグラデーションを定義します。 グラデーションは、完全に透明な青 (`paint.Color.WithAlpha(0)`) から50% の透明な青 (`paint.Color.WithAlpha(0x80)`) です。 Canvas の変換は、透過的な青色の 50% が、ベースラインから開始し、テキストの上部にある透明になりますので、テキストの上下を反転します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
