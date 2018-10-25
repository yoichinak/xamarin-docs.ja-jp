---
title: SkiaSharp のパスの効果
description: この記事では、さまざまな SkiaSharp パス効果を描画し、入力に使用するパスを許可して、サンプル コードでこのについて説明します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: davidbritch
ms.author: dabritch
ms.date: 07/29/2017
ms.openlocfilehash: bd865471e3efe42c44a8996a8e364b1c478b69e7
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615679"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp のパスの効果

_入力し、線の描画に使用するパスを許可するパスのさまざまな効果を検出します。_

A*パス効果*のインスタンスである、 [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect)クラスによって定義されている 8 つの静的な作成方法のいずれかで作成されるクラス。 `SKPathEffect`オブジェクトに設定し、 [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect)のプロパティ、 [ `SKPaint` ](xref:SkiaSharp.SKPaint)小規模のレプリケートされたパスの線の描画などの興味深い効果のさまざまなオブジェクト:

![](effects-images/patheffectsample.png "リンクされたチェーンのサンプル")

パスの効果を使用します。

- ストロークの行をドットとダッシュ
- 任意の塗りつぶされたパスの線の境界線の描画します。
- ハッチ線で領域を塗りつぶす
- 並べて表示されたパスを使用して領域を塗りつぶす
- 鋭い角丸めを行う
- 直線と曲線をランダムな「ジッター」を追加します。

さらに、2 つまたは複数のパスの効果を組み合わせることができます。

この記事では使用する方法も示します、 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath*)メソッドの`SKPaint`のプロパティを適用することで、1 つのパスを別のパスに変換する`SKPaint`など、`StrokeWidth`と`PathEffect`します。 これにより、別のパスのアウトライン パスを取得するなど、いくつかの興味深い手法。 `GetFillPath` パスの効果に関連便利です。

## <a name="dots-and-dashes"></a>ドットとダッシュ

使用、 [ `PathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single))メソッドは、この記事で説明した[**ドットし、ダッシュ**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)します。 メソッドの最初の引数は、偶数のダッシュの長さと、ダッシュの間隔の長さの間で切り替えると、2 つ以上の値を格納する配列。

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

これらの値は*いない*ストロークの幅を基準とします。 ストロークの幅が 10 で、行を四角形の破線と空白の正方形で構成する場合の設定など、`intervals`配列 {10, 10}。 `phase`引数は、破線パターン内の行の開始位置を示します。 この例では、正方形の間隔で始まる行を実行する場合が設定`phase`を 10 にします。

ダッシュの両端が受ける、`StrokeCap`プロパティの`SKPaint`します。 幅のストロークの幅は、このプロパティを設定する非常に一般的な`SKStrokeCap.Round`ダッシュの両端に丸める。 この場合、値、`intervals`配列*いない*丸め処理に起因する余分な長さが含まれます。 この事実は、循環ドットは、0 の幅を指定する必要がありますを意味します。 10 のストロークの幅の点と同じ大きさの点の間のギャップには、行を作成、使用、`intervals`の配列 {0, 20}。

**ドット形式のテキストをアニメーション化**に似ている、**中抜きの文字列を**、情報の記事で説明されているページ[**統合テキストとグラフィックス**](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)で設定してテキストの文字が表示される説明されている、`Style`のプロパティ、`SKPaint`オブジェクトを`SKPaintStyle.Stroke`します。 さらに、**ドット形式のテキストをアニメーション化**使用`SKPathEffect.CreateDash`点線の外観をアウトラインこの付与して、プログラムをアニメーション化も、`phase`の引数、`SKPathEffect.CreateDash`旅行のテキストを囲む点を作成するメソッド文字。 横モードでページを示します。

[![](effects-images/animateddottedtext-small.png "ドット形式のテキストをアニメーション化されるページのスクリーン ショットをトリプル")](effects-images/animateddottedtext-large.png#lightbox "ドット形式のテキストをアニメーション化されるページの 3 倍になるスクリーン ショット")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)クラスがいくつかの定数を定義することで開始しもオーバーライド、`OnAppearing`と`OnDisappearing`アニメーションのメソッド。

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface`ハンドラーを作成して開始、`SKPaint`テキストを表示するオブジェクト。 `TextSize`プロパティは、画面の幅に基づいて調整されます。

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds = new SKRect();
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

メソッドの最後の方、`SKPathEffect.CreateDash`を使用してメソッドを呼び出す、`dashArray`フィールド、およびアニメーション化されたとして定義されている`phase`値。 `SKPathEffect`インスタンスに設定されて、`PathEffect`のプロパティ、`SKPaint`テキストを表示するオブジェクト。

また、設定、`SKPathEffect`オブジェクトを`SKPaint`テキストを計測し、ページの中央に配置する前にオブジェクト。 その場合は、ただし、アニメーション ドットとダッシュ レンダリングされたテキストのサイズに若干のバリエーションが発生して、テキストを少しバイブレーション傾向があります。 (試してみる)

テキスト文字を囲む点をアニメーション化された円としてが特定の時点で各閉じた曲線ドットのポップ表示が存在すると思われる場所もわかります。 これは、文字アウトラインを定義するパスの開始位置し、終了位置。 破線パターン (ここで 20 ピクセル単位) の長さの整数倍でないパスの長さ場合、そのパターンの一部だけは、パスの最後に適合できます。

パスの長さに合わせてダッシュ パターンの長さを調整することができますが、記事の対象となる手法は、パスの長さを確認する必要がありますを[**パス情報と列挙**](information.md).

**ドット/ダッシュ Morph**ダッシュがドットで、もう一度フォーム ダッシュを結合に分割すると思われるように、プログラムが、dash パターン自体をアニメーション化します。

[![](effects-images/dotdashmorph-small.png "ドット Dash Morph ページのスクリーン ショットをトリプル")](effects-images/dotdashmorph-large.png#lightbox "ドット Dash Morph ページの 3 倍になるスクリーン ショット")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)オーバーライド、`OnAppearing`と`OnDisappearing`メソッドは前のプログラムが、クラス定義と同じように、`SKPaint`フィールドとしてのオブジェクト。

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

`PaintSurface`ハンドラーが、ページのサイズに基づいて、楕円のモーション パスを作成して、長を設定するコードのセクションを実行し、`dashArray`と`phase`変数。 アニメーション変数として`t`0 ~ 1 の範囲、`if`ブロックがその時間に 4 つの四半期とそれらの四半期の各分割`tsub`も 0 から 1 の範囲します。 最後に、プログラムを作成、`SKPathEffect`に設定し、`SKPaint`描画オブジェクト。

## <a name="from-path-to-path"></a>パスへのパスから

[ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single))メソッドの`SKPaint`の設定に基づいて別に 1 つのパスを変換、`SKPaint`オブジェクト。 この動作を確認するには置き換えます、`canvas.DrawPath`を次のコードは前のプログラムで呼び出します。

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

この新しいコードで、`GetFillPath`に変換を呼び出し、 `ellipsePath` (楕円だけです) に`newPath`を表示する`newPaint`します。 `newPaint`オブジェクトがすべての既定のプロパティ設定で作成されたことを除いて、`Style`プロパティをベースに設定されてから値を返すブール値`GetFillPath`。

ビジュアルに設定されているこの色は除いて同一である`ellipsePaint`なく`newPaint`します。 単純な楕円で定義されているのではなく`ellipsePath`、`newPath`ドットとダッシュの系列を定義する多数のパスの輪郭が含まれています。 これは、さまざまなプロパティを適用した結果`ellipsePaint`(具体的には、 `StrokeWidth`、`StrokeCap`と`PathEffect`) に`ellipsePath`に結果のパスを配置することと`newPath`。 `GetFillPath`メソッドは変換先のパスを入力するかどうかを示すブール値を返します。 この例では、戻り値は`true`のパスを入力します。

変更してみてください、`Style`設定`newPaint`に`SKPaintStyle.Stroke`とピクセル幅の 1 つの行に記載されている個々 のパスの輪郭が表示されます。

## <a name="stroking-with-a-path"></a>パスを使用して線の描画

[ `SKPathEffect.Create1DPath` ](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle))メソッドは、概念的に似ています`SKPathEffect.CreateDash`破線と空白のパターンではなく、パスを指定する点が異なります。 このパスは、線、線や曲線を複数回にレプリケートされます。

構文は次のとおりです。

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

一般に、パスに渡す`Create1DPath`中小ポイント (0, 0) を中央揃えになります。 `advance`パラメーターを行にパスがレプリケートされると、パスのセンターの間の距離を示します。 通常、この引数は、パスのおおよその幅を設定します。 `phase`引数再生されるのと同じロールは、ここには、`CreateDash`メソッド。

[ `SKPath1DPathEffectStyle` ](xref:SkiaSharp.SKPath1DPathEffectStyle)に 3 つのメンバーがあります。

- `Translate`
- `Rotate`
- `Morph`

`Translate`メンバーがそのまま同じ方向でレプリケートする直線または曲線に沿ってへのパス。 `Rotate`パスは、曲線の接線をに基づいて回転されます。 パスが、本の横線の通常の方向。 `Morph` ような`Rotate`パス自体が描画される線の曲率を一致するように曲線も点が異なります。

**1 D 効果**ページは、これら 3 つのオプションを示します。 [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml)ファイルが列挙体の 3 つのメンバーに対応する 3 つの項目を含むピッカーを定義します。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Translate</x:String>
                    <x:String>Rotate</x:String>
                    <x:String>Morph</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs)分離コード ファイルでは、3 つを定義します`SKPathEffect`フィールドとしてのオブジェクト。 これらはすべてを使用して作成`SKPathEffect.Create1DPath`で`SKPath`を使用して作成されたオブジェクト`SKPath.ParseSvgPathData`します。 単純なボックス 1 つ目は、2 つ目は、菱形および 3 番目が四角形。 これらは、3 つの効果のスタイルを示すために使用されます。

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch ((string)effectStylePicker.SelectedItem))
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

`PaintSurface`ハンドラー自体でループ処理を判断するために、ピッカーにアクセスするベジエ曲線を作成する`PathEffect`に線を使用する必要があります。 3 つのオプション- `Translate`、 `Rotate`、および`Morph`-左から右に表示されます。

[![](effects-images/1dpatheffect-small.png "1 D 効果 ページのスクリーン ショットをトリプル")](effects-images/1dpatheffect-large.png#lightbox "1 D 効果 ページの 3 倍になるスクリーン ショット")

指定されたパス、`SKPathEffect.Create1DPath`メソッドが必ず塗りつぶされます。 指定されたパス、`DrawPath`メソッドは常に線を付ける場合、`SKPaint`オブジェクトがその`PathEffect`プロパティ 1 D パスの効果を設定します。 注意して、`pathPaint`オブジェクトにない`Style`設定で、既定では通常`Fill`パスに線を付けるかに関係なく、します。

ボックスで使用される、`Translate`例は、20 ピクセルの正方形、および`advance`引数は 24 に設定されます。 この相違では、水平方向または垂直方向に行が約ですが、ボックスの対角線が 28.3 ピクセルであるため、行を斜めのボックスが少し重複のボックス間のギャップが原因です。

菱形、`Rotate`例は 20 ピクセル幅でも。 `advance`点は、ダイヤモンドが線の曲率と一緒に回転させるようにタッチを続けることは 20 に設定します。

内の四角形、`Morph`例は、50 ピクセルで、 `advance` 55 ベジエ曲線の周りが曲がっている四角形の間の小さな間隔に設定します。

場合、`advance`引数は、パスのサイズより小さい、レプリケートされたパスが重複することができます。 これは、結果、いくつかの興味深い効果。 **リンク チェーン**ページには、一連の重複を catenary の特徴的な形状にハングする、リンクされたチェーンのようにすると思われる円が表示されます。

[![](effects-images/linkedchain-small.png "リンクされたチェーンのページのスクリーン ショットをトリプル")](effects-images/linkedchain-large.png#lightbox "リンク チェーン ページの 3 倍になるスクリーン ショット")

とても近接検索し、実際に円をものがないことを確認します。 チェーン内の各リンクは、2 つの円弧、サイズし、置か隣接リンクで接続しているようです。

チェーンまたは uniform 重みの配分のケーブル、catenary の形式でハングします。 反転されたコピーの catenary の形式で構築された、arch アーチの重み付けによる負荷を均等に配分からメリットがあります。 Catenary では、一見単純な数学的な説明があります。

y を押しを =cosh(x/a)

*Cosh*ハイパーボリック コサイン関数です。 *X*を 0 に等しい*cosh*ゼロと*y* equals *、*。 Catenary の中心です。 ように、*コサイン*関数、 *cosh*と呼ばれます*でも*、ですつまり*cosh(–x)* equals *cosh(x)*。、正または負の値の引数を高めるための値を増やすとします。 これらの値には、曲線、catenary の面を形成するについて説明します。

適切な値の検索 *、* に合わせて、電話のページの寸法を catenary を利用する直接計算はありません。 場合*w*と*h*の最適値、四角形の高さと幅は *、* 次の式を満たします。

cosh (w/2/、) = 1 + h、/

次のメソッド、 [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs)クラスには、左側のおよびと等号の右側の 2 つの式を参照して、等しいかどうかが組み込まれています`left`と`right`します。 値が小さい *、*、`left`がより大きい`right`; の値が大きい *、*、`left`がより小さい`right`します。 `while`の最適な値でループを絞り込みます *、*:

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

`SKPath`オブジェクト クラスのコンス トラクターと、結果で、リンクが作成されたの`SKPathEffect`オブジェクトに設定し、`PathEffect`のプロパティ、`SKPaint`フィールドとして格納されているオブジェクト。

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

主な仕事、`PaintSurface`ハンドラーは、catenary 自体のパスを作成します。 最適な決定した後 *、* を格納することで、`optA`変数もする必要があるウィンドウの上部からのオフセットを計算します。 コレクションを蓄積できますし、 `SKPoint` 、catenary の値は、パスに有効にして、以前に作成したを使用してパスを描画`SKPaint`オブジェクト。

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

このプログラムで使用されるパスを定義する`Create1DPath`がその (0, 0)、センターのポイントします。 これが妥当に見えますため、(0, 0)、パスのポイントが直線または曲線を装飾に揃えられます。 ただし、中央揃え以外を使用できます (0, 0) 特殊効果をいくつかのポイントします。

**コンベヤ ベルト**ページは、曲線の上端と下端のサイズは、ウィンドウのサイズをしっぽコンベヤ ベルトのようなパスを作成します。 そのパスには、単純なを付けた`SKPaint`20 ピクセルの幅と色の灰色のオブジェクトし、別にもう一度描画`SKPaint`オブジェクトを`SKPathEffect`ほとんどバケットのようなパスを参照するオブジェクト。

[![](effects-images/conveyorbelt-small.png "コンベヤ ベルトのページのスクリーン ショットをトリプル")](effects-images/conveyorbelt-large.png#lightbox "コンベヤ ベルトのページの 3 倍になるスクリーン ショット")

(0, 0) バケット パスのポイントは、ハンドル、したがって、`phase`引数がアニメーション化、バケットは、コンベヤ ベルト、おそらく、下部に水をスコープとアウトの上部にあるダンプに焦点を絞っているようです。

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs)クラス実装のオーバーライドでアニメーションを`OnAppearing`と`OnDisappearing`メソッド。 バケットのパスは、ページのコンス トラクターで定義されます。

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

バケットの作成コードは、2 つの変換を少し拡大のバケットを横向きにするには完了します。 これらの変換を適用することは、前のコードのすべての座標を調整するよりも簡単でした。

`PaintSurface`自体コンベヤ ベルトのパスを定義することでハンドラーを開始します。 これは、単に行のペアと 20 ピクセル幅灰色の線で描画するセミコロンの円のペア。

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect =
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase,
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

コンベヤ ベルトを描画するためのロジックは、横モードでは機能しません。

バケットは、200 ピクセル コンベヤ ベルトの間隔について等間隔に配置する必要があります。 ただし、コンベヤ ベルト倍数でない可能性があります 200 ピクセルの長い、としているので、`phase`の引数`SKPathEffect.Create1DPath`がアニメーションが存在して、バケットは表示されます。

このため、最初に計算という値を`length`コンベヤ ベルトの長さは。 コンベヤ ベルトは、直線と円をセミコロンで構成され、ため、これは単純な計算になります。 次に、バケットの数を割ることによって計算`length`200 でします。 これは、最も近い整数に丸められます、分割数が、そのこと`length`します。 結果は、バケット数は整数の間隔です。 `phase`引数は、その一部だけです。

## <a name="from-path-to-path-again"></a>もう一度パスへのパスから

下部にある、`DrawSurface`ハンドラー**コンベヤ ベルト**、コメント アウト、`canvas.DrawPath`を呼び出すし、次のコードに置き換えます。

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

前の例と同様`GetFillPath`結果は、色を除けば同じことを確認します。 実行した後`GetFillPath`、`newPath`オブジェクトには、バケットのパスの複数のコピーが含まれています、アニメーションの配置を呼び出し時に、同じ場所に配置の各します。

## <a name="hatching-an-area"></a>区分の陰影

[ `SKPathEffect.Create2DLines` ](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix))メソッドは、多くの場合と呼ばれる平行線がある領域を塗りつぶします*ハッチ線*します。 メソッドでは、次の構文があります。

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width`ハッチ線のストロークの幅を指定します。 `matrix`パラメーターはスケーリングと省略可能な回転を組み合わせたものです。 スケール ファクターでは、ピクセル インクリメント Skia はハッチ線のスペースを使用していることを示します。 線の間の分離はマイナスのスケール ファクターでは、`width`引数。 スケール ファクターと等しいまたはそれよりも小さいかどうか、`width`値は、スペースを入れないでハッチ線の間および格納する領域が表示されます。 水平および垂直方向のスケーリングの同じ値を指定します。

既定では、ハッチ線が水平方向です。 場合、`matrix`パラメーターには、回転が含まれています、ハッチ線が時計回りに回転します。

**ハッチの塗りつぶし**ページは、このパスの効果を示します。 [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs)クラスでは、次の 3 つのパスの効果を定義フィールドとして、いることを示すスケール ファクターで 3 つのピクセルの幅と水平方向のハッチ線の最初の 6 ピクセル離れた間隔。 線の間の分離は、そのため、次の 3 つピクセルです。 2 番目のパスの効果は、6 つのピクセル幅の垂直ハッチ線間隔 (そのため、分離が 18 ピクセル)、離れている 24 ピクセルの対角線の陰影の 12 ピクセル幅間隔 36 ピクセル離れた行について、3 番目、およびします。

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6,
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12,
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

マトリックスに注意してください`Multiply`メソッド。 水平および垂直方向のスケーリング因子は同じであるために、拡大縮小および回転行列の乗算に使用される順序は関係ありません。

`PaintSurface`ハンドラーでは、これら 3 つのパスの効果を使用して、次の 3 つの異なる色と組み合わせてで`fillPaint`角丸四角形に合わせてページ サイズを入力します。 `Style`プロパティ セットを`fillPaint`無視する場合はときに、`SKPaint`オブジェクトにはから作成されたパスの効果が含まれています`SKPathEffect.Create2DLine`に関係なく、領域が塗りつぶされます。

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

結果を慎重に確認すると、赤、青のハッチ線を角丸四角形に正確に制限されないことが表示されます。 (これは一見、基になる Skia コードの特性。)別のアプローチが緑色で斜線行に対して表示される満足のいく場合: 角丸四角形はクリッピング パスとして使用され、ページ全体にハッチ線が描画されます。

`PaintSurface`赤、青のハッチ線に不一致が確認できるように、角の丸い四角形を単にストロークを描画する呼び出しでハンドラーの終了します。

[![](effects-images/hatchfill-small.png "ハッチの塗りつぶし ページのスクリーン ショットをトリプル")](effects-images/hatchfill-large.png#lightbox "ハッチの塗りつぶし ページの 3 倍になるスクリーン ショット")

Android の画面がそのような検索しない実際: シン赤色の線とシン広くする一見赤色の線に統合するおよびより多くのスペースが原因のスクリーン ショットのスケーリングします。

## <a name="filling-with-a-path"></a>パスの入力

[ `SKPathEffect.Create2DPath` ](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath))領域を有効に並べて表示領域を水平および垂直方向にレプリケートされるパスを入力できます。

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix`スケール ファクターは、レプリケートされたパスの水平および垂直方向の間隔を示します。 これを使用してパスを回転することはできませんが、`matrix`引数は、回転、パスを作成するかどうか、パス自体の回転を使用して、`Transform`メソッドによって定義された`SKPath`します。

レプリケートされたパスは、塗りつぶし対象の領域ではなく、画面の左と上端に揃え通常られます。 0 と左と上の各辺の水平および垂直方向のオフセットを指定する倍率変換要素を提供することで、この動作をオーバーライドできます。

**パス タイル入力**ページは、このパスの効果を示します。 領域を並べて表示するために使用されるパスがフィールドとして定義されている、 [ `PathTileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs)クラス。 水平および垂直座標 – 40 ~ 範囲 40、つまりこのパスは 80 ピクセルの正方形:

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " +
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " +
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50),
                    100, 100, paint);
            }
        }
    }
}
```

`PaintSurface`ハンドラー、`SKPathEffect.Create2DPath`呼び出しが重複する 80 ピクセルの四角形のタイルを 64 に水平および垂直方向の間隔を設定します。 さいわい、パスには、タイルを隣接と適切にメッシュ、パズルのピースがようになります。

[![](effects-images/pathtilefill-small.png "パスのタイルの入力 ページのスクリーン ショットをトリプル")](effects-images/pathtilefill-large.png#lightbox "パス タイルの入力 ページの 3 倍になるスクリーン ショット")

Android の画面で特に、歪みを元のスクリーン ショットからスケーリングします。

これらのタイルが常に全体表示されが切り詰められていないことに注意してください。 最初の 2 つのスクリーン ショットでは、上も明らかに指定されている領域が角丸四角形であることはできません。 これらのタイルには、特定の領域を切り捨てる場合は、クリッピング パスを使用します。

設定を試みてください、`Style`のプロパティ、`SKPaint`オブジェクトを`Stroke`と入力するのではなく、記載されている個々 のタイルが表示されます。

この記事で示すようには、並べて表示されるビットマップに領域がいっぱいになるも[ **SkiaSharp ビットマップ タイル**](../effects/shaders/bitmap-tiling.md)。

## <a name="rounding-sharp-corners"></a>シャープな角を丸める

**丸め七角形**でプログラムが表示されます、 [**円弧を描画する方法は 3 つ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)記事が 7 両面の図のポイントを曲線に接線の円弧を使用します。 **別の丸め七角形**ページから作成されたパスの効果を使用量をより簡単な方法を示しています、 [ `SKPathEffect.CreateCorner` ](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single))メソッド。

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

1 つの引数が名前付き`radius`、目的の角の半径を半分に設定する必要があります。 (これは、基になる Skia コードの特性です)。

ここでは、`PaintSurface`ハンドラーで、 [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs)クラス。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

この効果を使用するには、線の描画またはに基づいていっぱいになると、`Style`のプロパティ、`SKPaint`オブジェクト。 ここでは 3 つすべてのプラットフォームには。

[![](effects-images/anotherroundedheptagon-small.png "別の丸め七角形ページのスクリーン ショットをトリプル")](effects-images/anotherroundedheptagon-large.png#lightbox "別の丸め七角形ページの 3 倍になるスクリーン ショット")

この丸められた七角形が以前のバージョンのプログラムと同じことを確認します。 詳細納得させる必要がある場合、角の半径は 100 ではなくで指定された、50、`SKPathEffect.CreateCorner`呼び出しでコメントを解除できます上に重なって表示される最後のステートメントで、プログラムと 100 半径の円を参照してください。

## <a name="random-jitter"></a>ランダム ジッター

場合によってコンピューター_グラフィックスの完璧な直線は非常に目的の動作であり少しランダム性が必要な。 お試しくださいしたい場合は、 [ `SKPathEffect.CreateDiscrete` ](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32))メソッド。

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

このパスの効果は、線の描画または入力のいずれかを使用できます。 接続されたセグメントに線が分かれています — のおおよその長さがで指定された`segLength`およびさまざまな方向に拡張します。 元の行からの偏差の範囲がで指定された`deviation`します。

最後の引数には、効果のために使用する擬似乱数シーケンスの生成に使用されるシードです。 ジッターの効果の外観は、異なるシードを少し異なります。 引数が、既定値は 0 で、効果が同じであるプログラムを実行するたびにです。 シードを設定するにはさまざまなジッターが必要なは、画面が再描画されるたびに場合、`Millisecond`のプロパティを`DataTime.Now`値 (たとえば)。


**実験ジッター**  ページでは、さまざまな値で四角形を描画できます。

[![](effects-images/jitterexperiment-small.png "トリプル ジッター実験ページのスクリーン ショット")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

プログラムは簡単です。 [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml)ファイルでは、2 つのインスタンス化します`Slider`要素と`SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`PaintSurface`ハンドラーで、 [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs)分離コード ファイルと呼ばれますたびに、`Slider`値の変更。 呼び出す`SKPathEffect.CreateDiscrete`、2 つを使用して`Slider`プロパティと値を使用して四角形のストロークを。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

この効果は、この場合、塗りつぶされた領域のアウトラインはこれらのランダムな誤差の対象は同様に、入力の使用できます。 **ジッター テキスト**ページは、このパスの効果を使用して、テキストを表示するを示します。 コードのほとんどは、`PaintSurface`のハンドラー、 [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs)サイズ変更や、テキストを中央揃えにクラスを使用します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

ここで 3 つすべてのプラットフォームで横モードで実行されています。

[![](effects-images/jittertext-small.png "トリプル ジッターのテキスト ページのスクリーン ショット")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>パスのアウトライン表示

ほとんどの 2 つの例は既に説明した、 [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath*)メソッドの`SKPaint`、2 つのバージョンが存在します。

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

最初の 2 つの引数のみが必要です。 メソッドによって参照されているパスにアクセスする、`src`引数、ストロークのプロパティに基づいてパスのデータを変更する、`SKPaint`オブジェクト (など、`PathEffect`プロパティ)、しに結果を書き込みます、`dst`パス。 `resScale`パラメーターは、小規模な移行先パスを作成する桁数を減らすことができます、`cullRect`引数は、四角形の外側の輪郭を排除できます。

このメソッドの 1 つの基本的な使用がパスの効果をまったく関係しない: 場合、`SKPaint`オブジェクトがその`Style`プロパティに設定`SKPaintStyle.Stroke`と*いない*がその`PathEffect`設定し、`GetFillPath`を作成、パスを表す、*アウトライン*ソース パスの描画したペイント プロパティであるかのようです。

たとえば場合、`src`パスは、単純な円の半径、500 の`SKPaint`オブジェクトが 100 のストロークの幅を指定します、`dst`半径 550 の半径が 450 および、その他の 1 つ、2 つの同心円をパスになります。 メソッドは`GetFillPath`ため、この入力`dst`ストロークと同じパスでは、`src`パス。 ストロークを描画することができますもが、`dst`パス、パスのアウトラインを参照してください。

**パスをタップすると、アウトライン**この例を示します。 `SKCanvasView`と`TapGestureRecognizer`でインスタンス化される、 [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml)ファイル。 [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs)分離コード ファイルでは、3 つを定義します`SKPaint`で線の描画に 2 つのフィールドとしてオブジェクトのストローク幅 100 と 20、および塗りつぶしの 3 つ目の。

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

画面がタップされたされていない場合、`PaintSurface`ハンドラーを使用して、`blueFill`と`redThickStroke`の円形パスを表示するためにオブジェクトの塗りつぶし。

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) -
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

円は入力し、期待どおりに線を付けます。

[![](effects-images/taptooutlinethepathnormal-small.png "タップするアウトライン、パスの通常のページのスクリーン ショットをトリプル")](effects-images/taptooutlinethepathnormal-large.png#lightbox "タップにアウトライン、パスの通常のページの 3 倍になるスクリーン ショット")

画面をタップすると`outlineThePath`に設定されている`true`、および`PaintSurface`ハンドラーは、新しいを作成します。`SKPath`オブジェクトへの呼び出しで移行先パスとしてを使用して`GetFillPath`で、`redThickStroke`ペイント オブジェクト。 移行先パスの塗りつぶしや線に適用したし`redThinStroke`次の結果として得られる。

[![](effects-images/taptooutlinethepathoutlined-small.png "中抜きタップにアウトラインのパス ページのスクリーン ショットをトリプル")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "中抜きタップにアウトラインのパス ページの 3 倍になるスクリーン ショット")

2 つの赤い円では、元の円形パスが 2 つの循環輪郭に変換されたことを明確に表示します。

この方法は、開発に使用するパスで非常に便利な`SKPathEffect.Create1DPath`メソッド。 パスがレプリケートされるときに、これらのメソッドで指定したパスが必ず塗りつぶされます。 格納するパス全体をしたくない場合、アウトラインを慎重に定義する必要があります。

など、**リンク チェーン**サンプルでは、各ペアの 2 つの半径を入力するパスの領域の説明がに基づいて、4 つの円弧の一連のリンクが定義されています。 内のコードを置換することは、`LinkedChainPage`が少し異なる方法で実行するためにします。

最初に、再定義したいが、`linkRadius`定数。

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath`が目的でその 1 つの半径に基づく 2 つの円弧の角度を開始し、角度をスイープします。

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

`outlinePath`オブジェクトは、受信者のアウトラインの`linkPath`ときを付けたで指定したプロパティ`strokePaint`します。

この手法を使用して別の例は次のメソッドで使用されるパスの予定です。

## <a name="combining-path-effects"></a>パスの効果を組み合わせること

2 つの最終的な静的な作成方法`SKPathEffect`は[ `SKPathEffect.CreateSum` ](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect))と[ `SKPathEffect.CreateCompose` ](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

これら両方のメソッドは、複合パスの効果を作成する 2 つのパスの効果を結合します。 `CreateSum`メソッドは、個別に適用される 2 つのパスの効果を次のようなパスの効果を作成中に`CreateCompose`パスの 1 つの効果を適用 (、 `inner`) し、適用、`outer`です。

既に説明した方法、`GetFillPath`メソッドの`SKPaint`に基づいて別のパスを 1 つのパスを変換できる`SKPaint`プロパティ (を含む`PathEffect`) ではありませんので*すぎる*方法、不可解な`SKPaint`で指定された 2 つのパスの効果を 2 回、オブジェクトがその操作の実行、`CreateSum`または`CreateCompose`メソッド。

明確な用途の 1 つ`CreateSum`を定義するには、`SKPaint`オブジェクトの影響は 1 つのパス、パスを入力し、別のパスの効果を使用してパスの線です。 これは、方法については、**フレームで猫**サンプルは、キャプションの境界が付いたフレーム内で猫の配列を表示します。

[![](effects-images/catsinframe-small.png "猫のフレームのページのスクリーン ショットをトリプル")](effects-images/catsinframe-large.png#lightbox "猫のフレームのページの 3 倍になるスクリーン ショット")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs)クラスがいくつかのフィールドを定義することで開始します。 最初のフィールドを認識する可能性があります、 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs)クラスから、 [ **SVG パス データ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)記事。 2 番目のパスは、行と円弧のフレームのひだパターンに基づいています。

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath =
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath`で使用できる、`SKPathEffect.Create2DPath`メソッド場合、`SKPaint`オブジェクト`Style`プロパティに設定されて`Stroke`します。 ただし場合、`catPath`このプログラムで、猫の全体の頭がいっぱいになるし、表示されていることすらひげ直接に使用されます。 (試してみる)パスのアウトラインを取得してそのアウトラインでを使用する必要がある、`SKPathEffect.Create2DPath`メソッド。

このジョブでは、コンス トラクター。 最初に 2 つの変換が適用されます`catPath`を移動する、(0, 0) の中央をポイントし、サイズにスケール ダウンします。 `GetFillPath` 輪郭のすべてのアウトラインを取得します`outlinedCatPath`でそのオブジェクトが使用して、`SKPathEffect.Create2DPath`呼び出します。 の要素、スケーリング、`SKMatrix`値が水平よりも少し大きめと翻訳の要因が、タイルの間の小さなバッファーを提供する cat の縦のサイズが多少を派生完全 cat に表示されるように経験的、フレームの左上隅。

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect =
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

コンス トラクターを呼び出して`SKPathEffect.Create1DPath`のキャプションのフレーム。 パスの幅が 100 ピクセルは、レプリケートされたパスが枠の周りにオーバー ラップしたように、事前は 75 ピクセルことに注目してください。 コンス トラクターの呼び出しの最後のステートメント`SKPathEffect.CreateSum`2 つのパスの効果を結合し、結果を設定する、`SKPaint`オブジェクト。

により、この作業はすべて、`PaintSurface`非常に簡単にするハンドラー。 四角形を定義しを使用して描画するだけで済みます`framePaint`:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

パスの効果の背後にあるアルゴリズムは、いくつかのビジュアルに四角形の外側に表示される可能性がある、表示されるストロークまたは塗りつぶしに使用全体パスを常に発生します。 `ClipRect`より前のバージョンを呼び出す、`DrawRect`呼び出しが大幅に簡潔にするビジュアルを使用します。 (お試しください、クリッピングなし。)

使用するが一般的`SKPathEffect.CreateCompose`別のパスの効果をいくつかのジッターを追加します。 確かに、自分で試すことができますが、若干異なる例を次に示します。

**破線ハッチ**破線はハッチ線で楕円を塗りつぶします。 作業のほとんどの[ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs)クラスが実行されるフィールドの定義で右。 これらのフィールドは、dash 効果と陰影効果を定義します。 として定義されている`static`で、参照されているため、`SKPathEffect.CreateCompose`呼び出し、`SKPaint`定義。

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect =
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60),
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface`ハンドラーは、標準のオーバーヘッドとに 1 回の呼び出しのみを含める必要があります`DrawOval`:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2,
                        0.45f * info.Width, 0.45f * info.Height,
                        paint);
    }
    ...
}
```

まだ探索したハッチ線は、領域の内側に正確に制限されていると、この例では、常に開始全体ダッシュを左にあります。

[![](effects-images/dashedhatchlines-small.png "ハッチの破線のページのスクリーン ショットをトリプル")](effects-images/dashedhatchlines-large.png#lightbox "ハッチ破線ページの 3 倍になるスクリーン ショット")

奇妙な組み合わせに単純なドットとダッシュからその範囲は、パスの効果を確認したら、想像して作成することができますを参照してください。



## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
