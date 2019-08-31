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
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70197990"
---
# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp の線形グラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[ `SKPaint` ](xref:SkiaSharp.SKPaint)クラスを定義、 [ `Color` ](xref:SkiaSharp.SKPaint.Color)線のストロークまたは純色で領域の塗りつぶしに使用されるプロパティ。 または線のストロークを描画、使用領域の塗りつぶしまたは_グラデーション_色の段階的なブレンドであります。

![線形グラデーション サンプル](linear-gradient-images/LinearGradientSample.png "線形グラデーションのサンプル")

グラデーションの最も基本的な型が、_線形_グラデーションします。 行に色の組み合わせが発生します (と呼ばれる、_グラデーション ライン_) 別の 1 つのポイントから。 グラデーションの線に対して垂直な線では、同じ色があります。 2 つの静的のいずれかを使用して線形グラデーションを作成する[ `SKShader.CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient*)メソッド。 2 つのオーバー ロードの違いは、行列変換を含む 1 つと、もう一方にないことができます。 

これらのメソッドは、型のオブジェクトを返す[ `SKShader` ](xref:SkiaSharp.SKShader)に設定する、 [ `Shader` ](xref:SkiaSharp.SKPaint.Shader)プロパティの`SKPaint`します。 場合、`Shader`プロパティが null 以外で、オーバーライド、`Color`プロパティ。 すべての行に線を付けるにまたはこれを使用して塗りつぶされる領域`SKPaint`オブジェクトが純色ではなく、グラデーション基づきます。

> [!NOTE]
> `Shader`に含めるときに、プロパティは無視されます、`SKPaint`オブジェクト、`DrawBitmap`呼び出します。 使用することができます、`Color`プロパティの`SKPaint`ビットマップを表示するための透明度レベルを設定する (、記事の説明に従って[SkiaSharp を表示するビットマップ](../../bitmaps/displaying.md#displaying-in-pixel-dimensions))、使用することはできませんが、`Shader`プロパティを表示するためグラデーションの透明度のビットマップです。 他にも、グラデーション透明度のあるビットマップを表示するための方法が用意されています。これらの詳細については、SkiaSharp の「[グラデーション](circular-gradients.md#radial-gradients-for-masking)と[SkiaSharp の複合モードとブレンドモード](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)」を参照してください。

## <a name="corner-to-corner-gradients"></a>角のグラデーション

多くの場合、線形グラデーションは四角形の 1 つ上にある間に拡張します。 開始ポイントが四角形の左上隅の場合は、グラデーションを拡張できます。

- 左上隅にある垂直方向に
- 右上隅の水平方向に
- 右下隅に斜め

対角線方向の線形グラデーションの説明については、最初のページの**SkiaSharp シェーダーとその他の効果**のセクション、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプル。 **角 - グラデーション**ページを作成、`SKCanvasView`コンス トラクター内。 `PaintSurface`ハンドラーを作成、`SKPaint`オブジェクト、`using`ステートメントとし、キャンバスの中央に配置する 300 ピクセルの正方形四角形を定義します。

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

`Shader`プロパティの`SKPaint`が割り当てられている、`SKShader`静的なから値を返す`SKShader.CreateLinearGradient`メソッド。 5 つの引数は次のとおりです。

- グラデーションの開始点は、ここでは、四角形の左上隅に設定
- グラデーションの終了点は四角形の右上隅にここで設定
- 2 つ以上の色グラデーションに影響を与えるの配列
- 配列の`float`グラデーション ライン内の色の相対的な位置を示す値
- メンバー、 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode)グラデーションのグラデーションの線の端を超えて動作方法を示す列挙型

グラデーションのオブジェクトが作成された後、`DrawRect`メソッドを使用して、300 ピクセルの正方形の四角形を描画、`SKPaint`シェーダーを含むオブジェクト。 ここで、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されています。

[![角のグラデーション](linear-gradient-images/CornerToCornerGradient.png "角のグラデーション")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

グラデーション ラインは、最初の 2 つの引数として指定した 2 点によって定義されます。 これらのポイントの基準に通知、_キャンバス_と_いない_グラデーションを使用して表示されるグラフィカル オブジェクトにします。 グラデーションの線に沿った色は右下にある青色の左上にある赤から徐々 に移行します。 グラデーションの線に対して垂直な任意の行が、一定の色。

配列`float`色の配列を一対一に対応している 4 番目の引数として指定する値。 値は、その色が発生するグラデーション ラインに沿って相対位置を示します。 ここでは、0、つまり`Red`グラデーションの線の開始時に発生します 1。 つまり、`Blue`行の最後に発生します。 数字は、昇順である必要があります、および 0 ~ 1 の範囲内で指定する必要があります。 その範囲にそうでない場合は、その範囲内に調整されます。

配列内の 2 つの値は、0 と 1 以外のものに設定できます。 これを試してみます。

```csharp
new float[] { 0.25f, 0.75f }
```

グラデーション ライン全体の第 1 四半期は純粋な赤、および最後の四半期が純粋な青。 赤と青の組み合わせは、グラデーションの線の中央の半分に制限されています。

一般に、スペースを 1 に均等に 0 からこれらの位置値にします。 指定できます単純にする場合は、 `null` 4 番目の引数として`CreateLinearGradient`します。

このグラデーションは 300 ピクセルの正方形の四角形の 2 つの角の間で定義されて、その四角形の入力に制限はありません。 **角 - グラデーション**ページには、タップに応答する、余分なコードが含まれています。 または、ページ上のマウス クリックします。 `drawBackground`フィールドを切り替える`true`と`false`各タップでします。 値の場合`true`、`PaintSurface`ハンドラーを使用して同じ`SKPaint`全体のキャンバスにオブジェクトし、小さい四角形を示す黒い長方形を描画します。 

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

[![角のグラデーションの完全な](linear-gradient-images/CornerToCornerGradientFull.png "角 - グラデーションの完全な")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

あるグラデーション ラインを定義するポイントを超えたと同じパターンで繰り返さグラデーションに注意してください。 この繰り返しは、ために発生します。 最後の引数`CreateLinearGradient`は`SKShaderTileMode.Repeat`します。 (するオプションが表示されます、他の直後です。)

またを使用するグラデーションの線を指定する点が一意でないことに注意してください。 グラデーションの線に対して垂直な線はあるので、同じ効果を指定できるグラデーションの線の無限の数が、同じの色です。 たとえば、四角形を水平方向のグラデーションを使用して入力するときは、左と右上隅で、左および右下隅で、またはであっても、それらの行を並列である 2 つのポイントを指定できます。

## <a name="interactively-experiment"></a>対話形式で実験します。

線形グラデーションを対話形式で実験できます、**対話型の線形グラデーション**ページ。 このページを使用して、`InteractivePage`クラスは、情報の記事で導入された[**円弧を描画する 3 つの方法**](../../curves/arcs.md)します。`InteractivePage`ハンドル[ `TouchEffect` ](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)イベントのコレクションを維持するために`TouchPoint`指またはマウスを移動できるオブジェクト。

XAML ファイルのアタッチ、`TouchEffect`の親を`SKCanvasView`も含まれています、`Picker`の 3 つのメンバーのいずれかを選択することができます、 [ `SKShaderTileMode` ](xref:SkiaSharp.SKShaderTileMode)列挙体。

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

分離コード ファイルでコンス トラクターでは、2 つ作成されます`TouchPoint`起点と線形グラデーションの始点と終点のオブジェクト。 `PaintSurface`ハンドラーが (赤から緑、青のグラデーション) の 3 つの色の配列を定義し、現在を取得します`SKShaderTileMode`から、 `Picker`:。

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

`PaintSurface`ハンドラーを作成、`SKShader`オブジェクトからその情報は、それをキャンバス全体に色を使用しています。 配列`float`に値が設定されている`null`します。 それ以外の場合、3 つの色を均等にスペースを場合は、0、0.5 および 1 の値を持つ配列にはそのパラメーターを設定します。

一括、`PaintSurface`ハンドラーがいくつかのオブジェクトを表示するときに充てる: 輪郭の円、グラデーションの線では、タッチ ポイントでのグラデーションの線に対して垂直な線とタッチ ポイント。

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

垂直の線は、先頭とグラデーションの終了と一致します。 これらの行を超える操作結果の設定によって異なります、`SKShaderTileMode`列挙体。

[![対話型の線形グラデーション](linear-gradient-images/InteractiveLinearGradient.png "対話型の線形グラデーション")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

次の 3 つのスクリーン ショットは、の 3 つの異なる値の結果を表示する[ `SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode)します。 IOS のスクリーン ショットに示す`SKShaderTileMode.Clamp`グラデーションの境界線の色だけ延長します。 `SKShaderTileMode.Repeat`グラデーションのパターンは、次のように繰り返されますの Android のスクリーン ショットのオプションを示しています。 `SKShaderTileMode.Mirror` UWP スクリーン ショットではオプションも、パターンの繰り返しですが、パターンを反転するたびでない色の不連続性。

## <a name="gradients-on-gradients"></a>グラデーションのグラデーション

`SKShader`パブリック プロパティまたはメソッド以外のクラスを定義しない`Dispose`します。 `SKShader`オブジェクトの静的メソッドによって作成はそのため変更できません。 2 つのオブジェクトの同じグラデーションを使用する場合でもは、グラデーションを多少する可能性があります。 そのためには、新たに作成する必要があります`SKShader`オブジェクト。

**グラデーション テキスト**テキストと、両方のようなグラデーションによる色グラウンド ページが表示されます。

[![グラデーション テキスト](linear-gradient-images/GradientText.png "グラデーション テキスト")](linear-gradient-images/GradientText-Large.png#lightbox)

グラデーションで唯一の違いは、始点と終点は。 テキストを表示するために使用するグラデーションは、テキストの外接する四角形の角の丸みの 2 つの点に基づいています。 バック グラウンドでの 2 つの点は、キャンバス全体に基づいています。 次のコードに示します。

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

`Shader`のプロパティ、`SKPaint`バック グラウンドをカバーへのグラデーションを表示するオブジェクトが最初に設定します。 グラデーションのポイントは、キャンバスの左上隅および右下隅に設定されます。

コードのセット、`TextSize`のプロパティ、`SKPaint`オブジェクト、キャンバスの幅の 90% で、テキストが表示されます。 テキストの境界を使用して計算`xText`と`yText`に渡す値、`DrawText`中央揃えで表示するメソッド。

ただし、2 つ目のポイントをグラデーション`CreateLinearGradient`呼び出しが表示されたらそれをキャンバス テキストの左上隅および右下隅にあるを参照する必要があります。 これは、移行することによって実現、`textBounds`四角形によって同じ`xText`と`yText`値。

```csharp
textBounds.Offset(xText, yText);
```

今すぐ開始し、グラデーションの始点と終点を設定する四角形の左上隅および右下隅を使用できます。

## <a name="animating-a-gradient"></a>グラデーションをアニメーション化

グラデーションをアニメーション化するいくつかの方法はあります。 1 つの方法では、始点と終点をアニメーション化します。 **グラデーション アニメーション**ページ内を移動、2 つのポイント、円の中心とする、キャンバス上。 この円の半径が半分の幅または高さのキャンバス、小さい方です。 始点と終点が互いに反対側この円にあり、グラデーションが白から黒とに、`Mirror`タイル モード。

[![グラデーションのアニメーション](linear-gradient-images/GradientAnimation.png "グラデーション アニメーション")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

コンス トラクターを作成、`SKCanvasView`します。 `OnAppearing`と`OnDisappearing`メソッドは、アニメーションのロジックを処理します。

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

`OnTimerTick`メソッドは、計算、`angle`アニメーション化する 0 から 2 π を 3 秒おきの値。 

グラデーションの 2 つの点を計算する方法の 1 つを次に示します。 `SKPoint`という名前の値`vector`を円の半径のポイント、キャンバスの中央からを拡張する計算されます。 このベクトルの方向は、角度の正弦と余弦の値に基づきます。 その後、2つの反対のグラデーション点が計算されます。一方の点は、そのベクトルを中心点から減算することによって計算され、その他の点は、ベクトルを中心点に追加することによって計算されます。

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

少し異なる方法では、少ないコードが必要です。 このアプローチを利用、 [ `SKShader.CreateLinearGradient` ](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))行列変換が最後の引数としてメソッドをオーバー ロードします。 この方法は、バージョン、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプル。

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

キャンバスの幅が高さより小さいかどうかは、2 つのグラデーションのポイントに設定されます (0, 0) と (`info.Width`, 0)。 回転変換は、最後の引数として渡される`CreateLinearGradient`画面の中心では、これら 2 つのポイントを効果的に回転します。

場合の角度は 0、回転はありませんは、キャンバスの左上隅および右の角のグラデーションの 2 点に注意します。 これらのポイントは、同じグラデーション ポイントのように、前計算`CreateLinearGradient`呼び出します。 これらのポイントが_並列_を水平方向のグラデーションの行に 2 等分、キャンバスの中央と同一のグラデーションの結果します。

**虹グラデーション**

**虹グラデーション**ページがキャンバスの左上隅から右下隅に、レインボーを描画します。 ただし実際虹のようにこの虹グラデーションがありません。 カーブしてではなく、直線ですが、0 から 360 色相値間で循環によって決定される 8 の HSL (色合い-鮮やかさ-明るさ) の色に基づいて設定されます。

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

コードが含まれている、`PaintSurface`ハンドラーの下に表示します。 ハンドラーは、キャンバスの左上隅から右下隅まで 6 面のポリゴンを定義するパスを作成して開始します。

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

`CreateLinearGradient`メソッドの2つのグラデーションポイントは、このパスを定義する2つの点に基づいています。どちらの点も、左上隅の近くにあります。 キャンバスの上端に 1 つ目は、2 番目は、キャンバスの左端にします。 結果を次に示します。

[![障害のある虹グラデーション](linear-gradient-images/RainbowGradientFaulty.png "欠陥のある虹グラデーション")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

これは、興味深いイメージが目的では非常にではありません。 問題は、線形グラデーションを作成するときに、一定の色の行があるグラデーションの線に対して垂直です。 グラデーション ラインは、図は、上および左の辺に触れて、その行がない、図の右下隅に拡張する端に垂直な通常のポイントに基づいています。 この方法は、キャンバスが正方形の場合にのみ機能します。

適切な虹のグラデーションを作成するには、グラデーション ラインを虹の端に垂直でなければなりません。 これより複雑な計算です。 図の長い側に並列なベクトルを定義する必要があります。 ベクターは 90 度回転が側に直角になるようにします。 掛けることによって、図の幅にすることが長く、`rainbowWidth`します。 さらに、ベクターを指すおよび図では、横にある点に基づいて 2 つのグラデーションのポイントが計算します。 表示されるコードをここでは、**虹グラデーション**ページで、 [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプル。

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

[![虹グラデーション](linear-gradient-images/RainbowGradient.png "虹グラデーション")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**無限大の色**

虹グラデーションでも使用、**無限大色**ページ。 このページは、この記事で説明されているパス オブジェクトを使用して、無限大記号を描画します。 [**ベジエ曲線の型を次の 3 つ**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)します。 継続的に、イメージ全体をスイープするアニメーション虹グラデーションでは、イメージの色が、設定します。

コンス トラクターを作成、`SKPath`無限大記号を記述するオブジェクト。 パスが作成されると、コンス トラクターは、パスの四角形の境界も取得できます。 そしてという値を計算`gradientCycleLength`します。 左上隅および右下隅にグラデーションが基づいている場合、`pathBounds`四角形は、この`gradientCycleLength`値は、グラデーションのパターンの水平方向の幅の合計。

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

コンス トラクターを作成も、 `colors` 、虹の配列と`SKCanvasView`オブジェクト。

オーバーライド、`OnAppearing`と`OnDisappearing`メソッドは、アニメーションのオーバーヘッドを実行します。 `OnTimerTick`メソッドをアニメーション化、`offset`フィールドに 0 を`gradientCycleLength`2 秒間隔。

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

最後に、`PaintSurface`ハンドラーは、無限大記号を表示します。 パスには中心点を囲む負と正の座標が含まれています (0, 0)、`Translate`中央にシフトする、キャンバス上の変換を使用します。 平行移動変換の後に、`Scale`無限大記号の幅の 95% と、キャンバスの高さ内でも根強くが可能な限り大きく、スケール ファクターを適用する変換。 

なお、`STROKE_WIDTH`定数は、幅と高さの外接する四角形、パスに追加されます。 パスは、レンダリングされた無限大サイズのサイズが 4 辺すべてにつきその幅の半分だけ増加は、この幅の行で線は。

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

最初の 2 つの引数として渡されたポイント見て`SKShader.CreateLinearGradient`します。 これらのポイントは、外接する四角形の元のパスに基づいています。 最初のポイントは (&ndash;250、 &ndash;100)、2 番目の (250, 100)。 SkiaSharp の内部では、それらのポイントの対象で、現在のキャンバス変換揃えてアライン正しく表示されている無限大記号。

最後の引数なし`CreateLinearGradient`、右下へ、無限大記号の左上から拡張したレインボー グラデーションが表示されます。 (実際には、グラデーションまで左上隅から外接する四角形の右上隅にあります。 レンダリングされた無限大記号が半分に外接する四角形より大きい、`STROKE_WIDTH`辺すべてにつき値。 グラデーションは、最初と最後に、赤で、グラデーションが作成されたため、`SKShaderTileMode.Repeat`違いが顕著はありません)。

その最後の引数を持つ`CreateLinearGradient`、グラデーションのパターンは、イメージにまたがって継続的にスイープします。

[![無限大色](linear-gradient-images/InfinityColors.png "無限大の色")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>透明度とグラデーション

色のグラデーションに影響を与えるには、透過性を組み込むことができます。 別に 1 つの色からフェードするグラデーションの代わりにグラデーションは透過的に色からにフェードことができます。 

この手法は、いくつかの興味深い効果を使用できます。 典型的な例の 1 つは、リフレクションを使用してグラフィカル オブジェクトを示しています。

[![リフレクション グラデーション](linear-gradient-images/ReflectionGradient.png "リフレクション グラデーション")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

テキストを上下の色は 50% の上部の下部にある完全に透過的に透明なグラデーションを使用して設定します。 これらのレベルの透過性は、および 0x80 のアルファ値を 0 に関連付けられます。

`PaintSurface`ハンドラーで、**リフレクション グラデーション**ページ キャンバスの幅の 90% にテキストのサイズを拡大または縮小します。 そして計算`xText`と`yText`水平方向に中央揃えにするテキストを配置する値が、ページの垂直方向の中央に対応する基準に。

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

これは、`xText`と`yText`値は、同じ値に反映されたテキストを表示するために使用、`DrawText`の下部にある呼び出し、`PaintSurface`ハンドラー。 そのコードの直前に、表示されますへの呼び出し、`Scale`メソッドの`SKCanvas`します。 これは、`Scale`メソッドで拡大または縮小水平方向に (これは何も行いません) 1 つが垂直方向に&ndash;1 で、実質的にすべて上下を反転します。 回転の中心は、ポイントに設定されます (0、 `yText`) ここで、`yText`として最初に計算された、キャンバスの垂直方向の中心は、 `info.Height` 2 で割った値します。

Skia がキャンバスの変換は、前に、グラフィカル オブジェクトに色をグラデーションを使用することに留意してください。 Unreflected のテキストを描画した後、`textBounds`四角形が表示されるテキストに対応するためだけシフトします。

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient`呼び出しは、その四角形の上部から下へのグラデーションを定義します。 グラデーションは、完全に透明な青から (`paint.Color.WithAlpha(0)`) 50% の透過的な青から (`paint.Color.WithAlpha(0x80)`)。 Canvas の変換は、透過的な青色の 50% が、ベースラインから開始し、テキストの上部にある透明になりますので、テキストの上下を反転します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
