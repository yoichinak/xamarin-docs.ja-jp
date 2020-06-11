---
title: "SkiaSharp 線状グラデーション" の説明: "2 色の緩やかなブレンドで構成されるグラデーションで線または塗りつぶし領域を描画する方法を発見します。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A author: davidbritch dabritch: ms. date: 08/23/2018 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="the-skiasharp-linear-gradient"></a>SkiaSharp 線状グラデーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

クラスは、 [`SKPaint`](xref:SkiaSharp.SKPaint) [`Color`](xref:SkiaSharp.SKPaint.Color) 線を描画したり、純色で塗りつぶしたりするために使用されるプロパティを定義します。 また、色を段階的に混ぜ合わせた_グラデーション_を使用して線を描画したり、塗りつぶしたりすることができます。

![線状グラデーションのサンプル](linear-gradient-images/LinearGradientSample.png "線状グラデーションのサンプル")

最も基本的な種類のグラデーションは_線状_グラデーションです。 色のブレンドは、ある点から別の点への線 (_グラデーション線_と呼ばれます) に発生します。 グラデーション線に垂直な線の色は同じです。 線形グラデーションを作成するには、2つの静的メソッドのいずれかを使用し [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) ます。 2つのオーバーロードの違いは、一方にマトリックス変換が含まれていて、もう一方のオーバーロードが含まれていないことです。 

これらのメソッドは [`SKShader`](xref:SkiaSharp.SKShader) 、のプロパティに設定した型のオブジェクトを返し [`Shader`](xref:SkiaSharp.SKPaint.Shader) `SKPaint` ます。 `Shader`プロパティが null 以外の場合は、プロパティをオーバーライドし `Color` ます。 線またはこのオブジェクトを使用して塗りつぶされた領域は、 `SKPaint` 純色ではなくグラデーションに基づいています。

> [!NOTE]
> `Shader`呼び出しにオブジェクトを含める場合、プロパティは無視され `SKPaint` `DrawBitmap` ます。 のプロパティを使用し `Color` `SKPaint` て、ビットマップを表示するための透明度を設定できます ( [SkiaSharp ビットマップの表示](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)に関する記事で説明されています)。ただし、プロパティを使用して、 `Shader` グラデーション透明度のビットマップを表示することはできません。 グラデーションの ohp シートでビットマップを表示するためのその他の手法もあります。これらの方法については、 [SkiaSharp の丸いグラデーション](circular-gradients.md#radial-gradients-for-masking)と[SkiaSharp の合成と blend の各モード](../blend-modes/porter-duff.md#gradient-transparency-and-transitions)に関する記事を参照してください。

## <a name="corner-to-corner-gradients"></a>コーナーとコーナーのグラデーション

多くの場合、線形グラデーションは四角形の1つの隅から別の角に向かって拡張されます。 開始点が四角形の左上隅にある場合、グラデーションは次のように拡張できます。

- 左下隅に垂直方向に移動
- 右上隅に水平方向に配置
- 右下隅に斜めになる

対角線方向の線形グラデーションは、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**SkiaSharp シェーダーとその他の効果**セクションの最初のページで示されています。 **コーナーとコーナーのグラデーション**ページでは、コンストラクターにが作成され `SKCanvasView` ます。 ハンドラーは、 `PaintSurface` `SKPaint` ステートメント内にオブジェクトを作成 `using` し、キャンバスの中央に300ピクセルの四角形の四角形を定義します。

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

`Shader`のプロパティに `SKPaint` は、 `SKShader` 静的メソッドからの戻り値が割り当てられ `SKShader.CreateLinearGradient` ます。 5つの引数は次のとおりです。

- グラデーションの始点。ここで、四角形の左上隅に設定します。
- グラデーションの終点。ここで、四角形の右下隅に設定します。
- グラデーションに寄与する2つ以上の色の配列
- `float`グラデーション線内の色の相対位置を示す値の配列。
- グラデーション [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) がグラデーション線の両端を越えてどのように動作するかを示す列挙体のメンバー

グラデーションオブジェクトが作成された後、メソッドは、 `DrawRect` シェーダーを含むオブジェクトを使用して、300ピクセルの四角形の四角形を描画し `SKPaint` ます。 ここでは、iOS、Android、ユニバーサル Windows プラットフォーム (UWP) で実行されています。

[![コーナーとコーナーのグラデーション](linear-gradient-images/CornerToCornerGradient.png "コーナーとコーナーのグラデーション")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

グラデーション線は、最初の2つの引数として指定された2つの点によって定義されます。 これらの点は、グラデーションと共に表示されるグラフィカルオブジェクトでは_なく_、_キャンバス_に対して相対的であることに注意してください。 グラデーション線に沿って、色は、左上から右下にある赤から青に徐々に変化します。 グラデーション線に垂直な線は、一定の色になります。

`float`4 番目の引数として指定された値の配列には、色の配列との1対1の対応があります。 値は、その色が出現するグラデーション線に沿った相対的な位置を示します。 ここで、0は、が `Red` グラデーション線の開始時に発生することを意味し、1は `Blue` 行の最後に発生することを意味します。 数値は昇順にする必要があり、0 ~ 1 の範囲で指定する必要があります。 範囲内にない場合は、その範囲内になるように調整されます。

配列内の2つの値は、0と1以外の値に設定できます。 次のコードを実行してみてください。

```csharp
new float[] { 0.25f, 0.75f }
```

これで、グラデーション線の最初の四半期全体が純粋に赤になり、最後の四半期は純粋な青になります。 赤と青の組み合わせは、グラデーション線の中央半分に制限されています。

通常は、これらの位置の値を0から1に均等にスペースに配置します。 その場合は、の `null` 4 番目の引数としてを指定するだけ `CreateLinearGradient` です。

このグラデーションは300ピクセルの四角形の四角形の2つの角の間に定義されていますが、その四角形の塗りつぶしに限定されていません。 [**コーナーとコーナーのグラデーション**] ページには、ページ上のタップまたはマウスクリックに応答する追加のコードが含まれています。 `drawBackground`フィールドは `true` 、タップごとにとの間で切り替えられ `false` ます。 値がの場合 `true` 、ハンドラーは `PaintSurface` 同じオブジェクトを使用して `SKPaint` キャンバス全体を塗りつぶし、小さい四角形を示す黒い四角形を描画します。 

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

画面をタップした後に表示される内容を次に示します。

[![コーナーとコーナーのグラデーションがいっぱいです](linear-gradient-images/CornerToCornerGradientFull.png "コーナーとコーナーのグラデーションがいっぱいです")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

グラデーションが、グラデーション線を定義する点を超える同じパターンで繰り返されることに注意してください。 この繰り返しは、の最後の引数がであるために発生し `CreateLinearGradient` `SKShaderTileMode.Repeat` ます。 (他のオプションはすぐに表示されます)。

また、グラデーション線を指定するために使用するポイントが一意ではないことにも注意してください。 グラデーション線に垂直な線は同じ色であるため、同じ効果に対して指定できるグラデーション線の数は無限です。 たとえば、水平方向のグラデーションで四角形を塗りつぶす場合、左上と右上のコーナー、または左下と右下のコーナー、またはそれらの線と並行して並列である2つのポイントを指定できます。

## <a name="interactively-experiment"></a>対話的に実験する

**対話型の線形グラデーション**ページで線形グラデーションを対話的に試すことができます。 このページでは、この記事で説明したクラスを使用 `InteractivePage` して[**円弧を描画**](../../curves/arcs.md)します。 `InteractivePage` は、 [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) `TouchPoint` 指やマウスで移動できるオブジェクトのコレクションを保持するイベントを処理します。

XAML ファイルは、をの `TouchEffect` 親にアタッチ `SKCanvasView` します。また、 `Picker` 列挙体の3つのメンバーの1つを選択できるを含み [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) ます。

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

分離コードファイル内のコンストラクターは、 `TouchPoint` 線形グラデーションの始点と終点の2つのオブジェクトを作成します。 この `PaintSurface` ハンドラーは、次の3色の配列を定義します (グラデーションの場合、赤から緑から青)。また、から現在のを取得し `SKShaderTileMode` `Picker` ます。

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

ハンドラーは、 `PaintSurface` `SKShader` すべての情報からオブジェクトを作成し、それを使用してキャンバス全体の色を設定します。 値の配列 `float` はに設定され `null` ます。 それ以外の場合、3色に均等にスペースを割り当てるには、そのパラメーターを値0、0.5、および1を持つ配列に設定します。

ハンドラーの大部分 `PaintSurface` は、複数のオブジェクトを表示するために用意されています。タッチポイントは、輪郭の円、グラデーションの線、およびタッチポイントのグラデーション線に垂直線です。

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

2つの個々を接続するグラデーション線は簡単に描画できますが、垂直線にはいくつかの作業が必要です。 グラデーション線はベクトルに変換され、1つの単位の長さに正規化された後、90°回転します。 その後、ベクターの長さは200ピクセルになります。 タッチポイントから線を引いてグラデーション線に垂直になる4本の線を描画するために使用されます。

グラデーションの先頭および末尾と一致する垂直線。 これらの行を超えて行われる処理は、列挙の設定によって異なり `SKShaderTileMode` ます。

[![対話型線形グラデーション](linear-gradient-images/InteractiveLinearGradient.png "対話型線形グラデーション")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

3つのスクリーンショットは、の3つの異なる値の結果を示して [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) います。 IOS のスクリーンショットが表示され `SKShaderTileMode.Clamp` 、グラデーションの境界線の色だけが拡張されます。 `SKShaderTileMode.Repeat`Android のスクリーンショットのオプションは、グラデーションパターンがどのように繰り返されるかを示しています。 `SKShaderTileMode.Mirror`UWP スクリーンショットのオプションもパターンを繰り返しますが、パターンは毎回反転されるため、色の不連続化は行われません。

## <a name="gradients-on-gradients"></a>グラデーションのグラデーション

クラスは、 `SKShader` 以外のパブリックプロパティまたはメソッドを定義 `Dispose` します。 その `SKShader` ため、静的メソッドによって作成されたオブジェクトは変更できません。 2つの異なるオブジェクトに対して同じグラデーションを使用する場合でも、グラデーションを少し変更することが必要になる可能性があります。 そのためには、新しいオブジェクトを作成する必要があり `SKShader` ます。

[**グラデーションのテキスト**] ページには、テキストと、似たグラデーションで色分けされたバックグラウンドが表示されます。

[![グラデーションのテキスト](linear-gradient-images/GradientText.png "グラデーションのテキスト")](linear-gradient-images/GradientText-Large.png#lightbox)

グラデーションの唯一の違いは、開始点と終了点です。 テキストの表示に使用されるグラデーションは、テキストの外接する四角形の角にある2つの点に基づいています。 背景の場合、2つの点はキャンバス全体に基づいています。 コードは次のとおりです。

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

`Shader`オブジェクトのプロパティ `SKPaint` は、背景を覆うようにグラデーションを表示するために最初に設定されます。 グラデーションポイントは、キャンバスの左上隅と右下隅に設定されます。

このコードは、 `TextSize` `SKPaint` テキストがキャンバスの幅の90% に表示されるように、オブジェクトのプロパティを設定します。 テキストの境界を使用し `xText` て、 `yText` メソッドに渡す値と値を計算し `DrawText` 、テキストを中央揃えで表示します。

ただし、2番目の呼び出しのグラデーションポイントは、 `CreateLinearGradient` 表示されているキャンバスを基準として、テキストの左上および右下隅を参照する必要があります。 これは、 `textBounds` 四角形を同じ値と値でシフトすることで実現され `xText` `yText` ます。

```csharp
textBounds.Offset(xText, yText);
```

ここで、四角形の左上隅と右下隅を使用して、グラデーションの始点と終点を設定できます。

## <a name="animating-a-gradient"></a>グラデーションのアニメーション化

グラデーションをアニメーション化するには、いくつかの方法があります。 1つの方法は、開始点と終了点をアニメーション化することです。 **グラデーションアニメーション**ページは、キャンバスの中央にある円内の2つの点を移動します。 この円の半径は、キャンバスの幅または高さの半分です (どちらか小さい方)。 この円では始点と終点が逆になっており、グラデーションはタイルモードで白から黒に `Mirror` なります。

[![グラデーションアニメーション](linear-gradient-images/GradientAnimation.png "グラデーションアニメーション")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

コンストラクターは、を作成し `SKCanvasView` ます。 `OnAppearing`メソッドと `OnDisappearing` メソッドは、アニメーションロジックを処理します。

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

メソッドは、 `OnTimerTick` `angle` 3 秒ごとに0から2πのアニメーション化された値を計算します。 

2つのグラデーションポイントを計算する方法の1つを次に示します。 `SKPoint`という名前の値 `vector` が計算され、キャンバスの中心から円の半径のポイントに拡張されます。 このベクトルの方向は、角度の正弦とコサインの値に基づいています。 次に、2つの反対のグラデーション点が計算されます。1つは中心点からベクトルを引いて計算され、もう一方の点は中心点にベクターを追加することによって計算されます。

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

少し異なる方法では、コードが少なくて済みます。 この方法では、 [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) 最後の引数としてマトリックス変換を使用して、オーバーロードメソッドを使用します。 この方法は、 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルのバージョンです。

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

キャンバスの幅が高さよりも小さい場合、2つのグラデーションポイントは (0, 0) と (, 0) に設定され `info.Width` ます。 回転変換は最後の引数として渡され、 `CreateLinearGradient` 画面の中央を中心に2つの点を効果的に回転します。

角度が0の場合は回転が行われず、2つのグラデーションポイントがキャンバスの左上隅と右上隅になります。 これらの点は、前の呼び出しで示されているように計算されたグラデーションポイントと同じではありません `CreateLinearGradient` 。 ただし、これらの点は、キャンバスの中心を bisects する横線に_平行_しています。その結果、グラデーションは同じになります。

**レインボーグラデーション**

**レインボーグラデーション**ページでは、キャンバスの左上隅から右下隅にレインボーが描画されます。 しかし、このレインボーグラデーションは実際のレインボーとは似ていません。 これは曲線ではなく、直線ですが、0 ~ 360 の色相値を循環することによって決定される8つの HSL (色合いと鮮やかさの明るさ) の色に基づいています。

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

このコードは、 `PaintSurface` 次に示すハンドラーの一部です。 ハンドラーは、キャンバスの左上隅から右下隅までを拡張する6つの辺を定義するパスを作成することから始めます。

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

メソッド内の2つのグラデーションポイント `CreateLinearGradient` は、このパスを定義する2つの点に基づいています。両方の点が左上隅の近くにあります。 1つ目はキャンバスの上端にあり、2つ目はキャンバスの左端にあります。 結果は次のようになります。

[![レインボーグラデーションの不具合](linear-gradient-images/RainbowGradientFaulty.png "レインボーグラデーションの不具合")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

これは興味深いイメージですが、意図したものではありません。 問題は、線状グラデーションを作成するときに、グラデーションの線がグラデーション線に垂直になることです。 グラデーション線は、図が上端と左側に接している点に基づいています。この線は、通常、右下隅に拡張される図形の端に垂直方向ではありません。 この方法は、キャンバスが正方形の場合にのみ機能します。

適切なレインボーグラデーションを作成するには、グラデーション線がレインボーの端に垂直になっている必要があります。 これは、より複雑な計算です。 ベクターは、図形の長い側に平行に定義されている必要があります。 ベクターは、その辺に対して垂直になるように90°回転されます。 次に、を乗算することによって、図形の幅として長くなり `rainbowWidth` ます。 2つのグラデーションポイントは、図形の横にある点に基づいて計算され、その点とベクターを加算したものになります。 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの**レインボーグラデーション**ページに表示されるコードを次に示します。

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

これで、レインボー色は図に合わせて調整されました。

[![レインボーグラデーション](linear-gradient-images/RainbowGradient.png "レインボーグラデーション")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**無限大色**

[**無限大色**] ページでは、レインボーグラデーションも使用されます。 このページでは、記事「 [**3 種類のベジエ曲線**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs)」で説明されているパスオブジェクトを使用して無限大記号を描画します。 画像は、画像全体を連続的にスイープする、アニメーション化されたレインボーグラデーションで色付けされます。

コンストラクターは、 `SKPath` 無限大記号を記述するオブジェクトを作成します。 パスが作成された後、コンストラクターはパスの四角形の境界を取得することもできます。 次に、という値を計算し `gradientCycleLength` ます。 グラデーションが四角形の左上隅と右下隅に基づいている場合 `pathBounds` 、この `gradientCycleLength` 値はグラデーションパターンの横幅の合計になります。

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

また、このコンストラクターは、 `colors` レインボーの配列とオブジェクトを作成し `SKCanvasView` ます。

`OnAppearing`メソッドとメソッドをオーバーライドすると、 `OnDisappearing` アニメーションのオーバーヘッドが発生します。 メソッドは、 `OnTimerTick` `offset` フィールドを0から2秒ごとにアニメーション化し `gradientCycleLength` ます。

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

最後に、この `PaintSurface` ハンドラーは無限大記号をレンダリングします。 パスには、(0, 0) の中心点を囲む負の座標と正の座標が含まれているので、 `Translate` キャンバス上の変換を使用して中央に移動します。 変換変換の後に、 `Scale` キャンバスの幅と高さの95% 以内に無限大記号を可能な限り大きくするスケールファクターを適用する変換が行われます。 

`STROKE_WIDTH`パス外の四角形の幅と高さに定数が追加されていることに注意してください。 パスには、この幅の線が表示されます。したがって、表示される無限大サイズのサイズは、4辺すべての幅の半分に増加します。

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

の最初の2つの引数として渡された点を確認 `SKShader.CreateLinearGradient` します。 これらのポイントは、元のパス外接する四角形に基づいています。 最初のポイントは ( &ndash; 250, &ndash; 100) で、2番目のポイントは (250, 100) です。 SkiaSharp の内部では、これらのポイントは、表示されている無限大記号と適切に配置されるように、現在のキャンバス変換を対象としています。

の最後の引数がない場合 `CreateLinearGradient` 、無限大記号の左上から右下へのレインボーグラデーションが表示されます。 (実際には、グラデーションは、左上隅から外接する四角形の右下隅に向かっています。 表示される無限大記号は、すべての辺の値の半分によって外接する四角形よりも大きくなり `STROKE_WIDTH` ます。 グラデーションは先頭と末尾の両方で赤で、グラデーションはで作成されるため、この `SKShaderTileMode.Repeat` 違いは顕著ではありません)。

この最後の引数をに指定すると `CreateLinearGradient` 、グラデーションパターンがイメージ全体を連続的にスイープします。

[![無限大色](linear-gradient-images/InfinityColors.png "無限大色")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>透明度とグラデーション

グラデーションに寄与する色には、透明度を組み込むことができます。 ある色から別の色にフェードするグラデーションではなく、グラデーションを色から透明にフェードできます。 

この手法は、いくつかの興味深い効果に使用できます。 従来の例の1つは、そのリフレクションを使用したグラフィカルオブジェクトを示しています。

[![反射のグラデーション](linear-gradient-images/ReflectionGradient.png "反射のグラデーション")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

反転されたテキストは、一番上に透明色が50% のグラデーションで色付けされます。 これらのレベルの透明度は、0x80 と0のアルファ値に関連付けられています。

`PaintSurface`**リフレクションのグラデーション**ページのハンドラーは、テキストのサイズをキャンバスの幅の90% に拡大します。 次に `xText` 、との `yText` 値を計算し、水平方向にテキストを配置しますが、ページの垂直方向中央に対応するベースライン上に配置します。

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

これら `xText` の `yText` 値と値は、 `DrawText` ハンドラーの下部にある呼び出しでリフレクションされたテキストを表示するために使用される値と同じです `PaintSurface` 。 ただし、このコードの直前には、のメソッドの呼び出しが表示さ `Scale` `SKCanvas` れます。 この `Scale` メソッドは、水平方向に 1 (何も実行しない) で、垂直方向に1ずつ拡大縮小 &ndash; します。 回転の中心は、ポイント (0,) に設定され `yText` ます。ここで、 `yText` はキャンバスの垂直方向の中心であり、当初は2で割った値として計算され `info.Height` ます。

Skia はグラデーションを使用して、キャンバス変換の前にグラフィカルオブジェクトの色を設定することに注意してください。 反射されていないテキストが描画されると、 `textBounds` 四角形がシフトされ、表示されるテキストに対応するようになります。

```csharp
textBounds.Offset(xText, yText);
```

この `CreateLinearGradient` 呼び出しは、四角形の上部から下部までのグラデーションを定義します。 グラデーションは、完全に透明な青 ( `paint.Color.WithAlpha(0)` ) から50% の透明な青 () までです `paint.Color.WithAlpha(0x80)` 。 キャンバスの変換では、テキストの反転が反転されるため、50% の透明な青色がベースラインで開始され、テキストの上部で透明になります。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
