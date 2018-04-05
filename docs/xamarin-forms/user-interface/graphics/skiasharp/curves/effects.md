---
title: パスの効果
description: 線の描画と、入力に使用するパスを許可するさまざまなパス効果を検出します。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 47f5a6fdcfb6ee795f84ca8e19c0954b68a2fae9
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/05/2018
---
# <a name="path-effects"></a>パスの効果

_線の描画と、入力に使用するパスを許可するさまざまなパス効果を検出します。_

A*パス効果*のインスタンス、 [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/)を 8 つの静的のいずれかで作成したクラス`Create`メソッドです。 `SKPathEffect`オブジェクトに設定し、 [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/)のプロパティ、`SKPaint`小規模のレプリケートされたパスの線を描画興味深い効果のさまざまなオブジェクト。

![](effects-images/patheffectsample.png "リンクされたチェーン サンプル")

パスの効果を使用します。

- ドットとダッシュを使用した直線を描く
- 塗りつぶされた任意のパスを使用した直線を描く
- 領域を塗りつぶす縞模様です。
- タイル化されたパス、領域を塗りつぶす
- 鋭い角の丸めを行う
- 直線と曲線をランダム「ジッター」を追加します。

さらに、2 つ以上のパスの効果を組み合わせることができます。

この資料を使用する方法も示します、`GetFillPath`メソッドの`SKPaint`のプロパティを適用することで、1 つのパスを別のパスに変換する`SKPaint`など、`StrokeWidth`と`PathEffect`です。 これにより、別のパスの概要ではパスを取得するなど、いくつかの興味深い方法。 `GetFillPath` パス効果関連便利です。

## <a name="dots-and-dashes"></a>ドットとダッシュ

使用、 [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/)メソッドは、記事で説明した[**点線および破線**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)です。 メソッドの最初の引数は、偶数個のダッシュの長さとダッシュの間隔の長さの間で切り替えると、2 つ以上の値を格納する配列を示します。

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

これらの値は*いない*ストロークの幅に対して相対的です。 たとえば、ストロークの幅は 10 ですが、行と正方形の空白の四角形の破線で構成する場合は、設定、`intervals`配列 {10, 10}。 `phase`破線パターン内の行の始点の引数を示します。 この例では、四角形の間隔で始まる行を実行する場合に、設定`phase`を 10 にします。

破線の両端が影響を受けました、`StrokeCap`プロパティ`SKPaint`です。 幅の広い線の幅は、このプロパティを設定非常に一般的な`SKStrokeCap.Round`破線の両端に丸める。 この場合、内の値、`intervals`配列は*されません*から丸め、結果として得られる循環ドットがゼロの幅を指定する必要があることを意味する追加の長さが含まれます。 、10 のストロークの幅を循環ドットと同じ形の直径のドットの間のギャップには、行を作成、使用、`intervals`の配列 {0, 20}。

**ドットで区切られたテキストのアニメーション**ページがに似ていますが、**記載されているテキスト**、資料に記載されたページ[**を統合するテキストとグラフィックス**](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)で設定してテキストの文字が表示される説明されている、`Style`のプロパティ、`SKPaint`オブジェクトを`SKPaintStyle.Stroke`です。 さらに、**ドットで区切られたテキストのアニメーション**使用`SKPathEffect.CreateDash`点線の外観を説明これに付与し、プログラムをアニメーション化も、`phase`の引数、`SKPathEffect.CreateDash`を旅行のテキストを囲むドットを作成するメソッド文字があります。 横モードでページを次に示します。

[![](effects-images/animateddottedtext-small.png "ドットで区切られたテキストのアニメーションをページのスクリーン ショットをトリプル")](effects-images/animateddottedtext-large.png#lightbox "ドットで区切られたテキストのアニメーションをページのトリプル スクリーン ショット")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)クラスは、一部の定数を定義することで開始しもオーバーライド、`OnAppearing`と`OnDisappearing`アニメーションのメソッド。

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

`PaintSurface`ハンドラーが作成した後、`SKPaint`テキストを表示するオブジェクト。 `TextSize`プロパティを画面の幅に基づいてに調整されます。

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
            SKRect textBounds;
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

メソッドの末尾にかけて、`SKPathEffect.CreateDash`を使用してメソッドを呼び出す、`dashArray`フィールド、およびアニメーション化されたとして定義されている`phase`値。 `SKPathEffect`インスタンスに設定されて、`PathEffect`のプロパティ、`SKPaint`テキストを表示するオブジェクト。

また、設定することができます、`SKPathEffect`オブジェクトを`SKPaint`テキストを測定し、ページで、中央に配置する前にオブジェクト。 その場合は、ただし、アニメーション ドットとダッシュ若干のバリエーションをレンダリングするテキストのサイズとテキストは、少し振動させられる傾向があります。 (試してみる)

テキストの文字を囲むアニメーション ドット円としてがある特定の時点で各閉じた曲線の存在との間のポップをドットが短い場合にも注目します。 これは、文字のアウトラインを定義するパスの開始位置し、終了位置。 パスの長さが、ダッシュのパターン (ここでは 20 ピクセル単位) の長さの整数倍ではない場合、そのパターンの一部だけは、パスの最後に適合できます。

破線パターンで、パスの長さに合わせてまでの長さを調整することはできますが、ある手法、パスの長さを決定するを必要とする今後の記事で説明します。

**ドット (.)/ダッシュ Morph**ダッシュように点線で、もう一度を組み合わせてフォーム ダッシュを分割できるように、プログラムが、dash パターン自体をアニメーション化します。

[![](effects-images/dotdashmorph-small.png "ドット Dash Morph ページのスクリーン ショットをトリプル")](effects-images/dotdashmorph-large.png#lightbox "ドット Dash Morph ページのトリプル スクリーン ショット")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)クラスのオーバーライド、`OnAppearing`と`OnDisappearing`メソッドの前のプログラムがでしたが、定義されているクラスと同様、`SKPaint`フィールドとしてオブジェクト。

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

`PaintSurface`ハンドラーは、ページのサイズに基づく楕円パスを作成し、長いセクションを設定するコードの実行、`dashArray`と`phase`変数。 アニメーション変数として`t`範囲は 1、0、`if`ブロックがその時点でその四半期の各および 4 つの四半期に分割`tsub`も 0 の範囲を 1 にします。 最後に、プログラムを作成、`SKPathEffect`設定し、`SKPaint`描画オブジェクトです。

## <a name="from-path-to-path"></a>パスへのパスから

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/)メソッドの`SKPaint`に別の設定に基づいて 1 つのパスをオンに、`SKPaint`オブジェクト。 このしくみを表示するには、置換、`canvas.DrawPath`を次のコードは、前のプログラムで呼び出します。

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

この新しいコードで、`GetFillPath`変換を呼び出し、 `ellipsePath` (楕円だけです) に`newPath`に表示される、`newPaint`です。 `newPaint`オブジェクトがすべての既定のプロパティ設定で作成された点を除いて、`Style`プロパティに基づいて、ブール値から値を返す`GetFillPath`です。

ビジュアルに設定されている色を除いて同一`ellipsePaint`ではなく`newPaint`です。 単純な楕円で定義されているのではなく`ellipsePath`、`newPath`ドットとダッシュの系列を定義する多数のパスの輪郭が含まれています。 さまざまなプロパティを適用した結果は次の`ellipsePaint`— `StrokeWidth`、 `StrokeCap`、および`PathEffect`— に`ellipsePath`結果のパスに配置することと`newPath`です。 `GetFillPath`メソッドが転送先のパスを入力するかどうかを示すブール値を返します。 戻り値は、この例では`true`のパスを入力します。

変更してみて、`Style`設定`newPaint`に`SKPaintStyle.Stroke`と、1 ピクセル幅線が記載されている個々 のパスの輪郭が表示されます。

## <a name="stroking-with-a-path"></a>パスの描画

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/)メソッドは、概念的に似ています`SKPathEffect.CreateDash`破線のパターンではなく、パスを指定する点が異なります。 このパスは、線、行または曲線に複数回がレプリケートされます。

構文は次のとおりです。

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> 注意: のオーバー ロードがある`Create1DPath`列挙型の引数で定義されている`SkPath1DPathEffect`を小文字 'k' この名前は、エラーと、その列挙型とメソッドは推奨されなくなりましたが、非常に簡単に、コードの一部にするメソッドは推奨されませんしする内容を正確に表示することは困難が間違っています。

一般に、渡されたパス`Create1DPath`小規模で中央に配置します (0, 0) のポイントを中心になります。 `advance`パラメーターが行にパスがレプリケートされると、パス センターからの距離を示します。 通常、この引数は、パスのおおよその幅を設定します。 `phase`引数アプローチでは、同じとしての役割は、ここに、`CreateDash`メソッドです。

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/)は 3 つのメンバーがあります。

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate`メンバーにより、パスを直線または曲線に沿ってレプリケートされるように、同じ方向に保持します。 `Rotate`パスが、曲線の接線に基づいて回転します。 パスには、水平の線の通常の方向がします。 `Morph` ような`Rotate`パス自体が描画される線の曲率を一致するように曲線も点が異なります。

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
            <Picker.Items>
                <x:String>Translate</x:String>
                <x:String>Rotate</x:String>
                <x:String>Morph</x:String>
            </Picker.Items>
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

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs)分離コード ファイルでは、3 つを定義します`SKPathEffect`フィールドとしてオブジェクト。 これらはすべてを使用して作成`SKPathEffect.Create1DPath`で`SKPath`を使用して作成されたオブジェクト`SKPath.ParseSvgPathData`です。 最初は簡単なボックス、2 番目はダイヤモンド形、3 番目は四角形。 これらは、次の 3 つの効果のスタイルを示すために使用されます。

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

            switch (effectStylePicker.Items[effectStylePicker.SelectedIndex])
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

`PaintSurface`ハンドラー自体でループ処理を決定するため、ピッカーにアクセスするベジエ曲線を作成する`PathEffect`に線を使用する必要があります。 3 つのオプション- `Translate`、 `Rotate`、および`Morph`-左から右に表示されます。

[![](effects-images/1dpatheffect-small.png "トリプル ページのスクリーン ショットの 1 D パス効果")](effects-images/1dpatheffect-large.png#lightbox "トリプル ページのスクリーン ショットの 1 D パスの効果")

指定されたパス、`SKPathEffect.Create1DPath`メソッドは常に入力します。 指定されたパス、`DrawPath`メソッド常にストロークを付ける場合、`SKPaint`オブジェクトがその`PathEffect`プロパティの 1 D パス効果に設定します。 注意して、`pathPaint`オブジェクトを持たない`Style`設定は、通常の既定値は`Fill`、パス ストロークを付けるかに関係なく、します。

使用される、ボックス、`Translate`の例は、20 ピクセルの四角形、および`advance`引数 24 に設定されています。 この違いは、水平または垂直方向、行はほぼ同じですが、ボックスの対角線が 28.3 ピクセルであるため、行を斜めのボックスとは少し重複ボックス間のギャップをによりします。 

菱形、`Rotate`例も 20 ピクセルです。 `advance`ひし形が線の曲率と一緒に回転させるようにタッチするポイントが続行するために、20 に設定します。

内の四角形、`Morph`の例で幅 50 ピクセル、 `advance` 55 のベジエ曲線周囲が曲がっている四角形の差を小規模に設定します。

場合、`advance`パスのサイズより小さい引数は、レプリケートされたパスが重なることができます。 これにより、いくつかおもしろい効果。 **リンク チェーン**ページには、一連の重複を catenary の個性的な形でハングするリンクのチェーンのように見える円が表示されます。

[![](effects-images/linkedchain-small.png "リンクされたチェーン ページのスクリーン ショットをトリプル")](effects-images/linkedchain-large.png#lightbox "リンク チェーン ページのトリプル スクリーン ショット")

非常に見るし、実際には円をされないものが表示されます。 チェーン内の各リンクは、2 つの円弧、サイズし、置か隣接リンクで接続しているようです。

チェーンまたは uniform 重み付けの分散のケーブル、catenary の形式でハングします。 反転された catenary の形式で構築された、arch メリットがある、アーキテクチャの重みの負荷を均等に配分します。 Catenary には、一見単純な数学的な説明があります。

y、· を =cosh(x/a)

*Cosh*ハイパーボリック コサイン関数です。 *X*を 0 に等しい*cosh*ゼロと*y* equals *、*です。 Catenary の中心です。 同様に、*コサイン*関数、 *cosh*と呼ばれます*でも*、つまり*cosh(–x)* equals *cosh(x)*、正または負の値の引数を高めるために値が大きくなるとします。 これらの値では、曲線、catenary の辺を形成するについて説明します。

適切な値の検索*、* catenary 電話のページの寸法に合わせてが直接計算します。 場合*w*と*h*幅と高さの四角形の最適な値は、 *、*が次の式を満たします。

cosh (w/2、/) 1 + h を =、/

次の方法で、 [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs)として等号 (=) の右側と左側の 2 つの式を参照して、等しいかどうかがクラスに組み込まれています`left`と`right`です。 値が小さい*、*、`left`がより大きい`right`; の値が大きい*、*、`left`はより小さい`right`です。 `while`ループを内の最適な値を絞り込みます*、*:

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

`SKPath`オブジェクトのクラスのコンス トラクターと、結果内のリンクが作成`SKPathEffect`オブジェクトに設定し、`PathEffect`のプロパティ、`SKPaint`フィールドとして格納されているオブジェクト。

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

メインのジョブ、`PaintSurface`ハンドラーは catenary 自体のパスを作成します。 最適なを決定した後*、*で格納して、`optA`変数もする必要があるウィンドウの上部からのオフセットを計算します。 コレクションを累積できるし、 `SKPoint` 、catenary の値が、パスに有効にして、以前に作成して、パスの描画`SKPaint`オブジェクト。

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

このプログラムで使用されるパスを定義する`Create1DPath`がその (0, 0) 中央にポイントします。 これは、必然的にあるため、(0, 0) パスのポイントは、行または装飾曲線に揃えられます。 中央揃え以外を使用するただし、(0, 0) 特殊効果をポイントします。

**コンベヤ ベルト**ページは、長方形コンベヤ ベルト曲線の上部と下部のウィンドウの次元のサイズがこのようなパスを作成します。 シンプルなそのパスのストロークを付ける`SKPaint`20 ピクセル幅の広いと、灰色のオブジェクトを別の再描画`SKPaint`オブジェクトを`SKPathEffect`ほとんどバケットのようなパスを参照するオブジェクト。

[![](effects-images/conveyorbelt-small.png "コンベヤ ベルトのページのスクリーン ショットをトリプル")](effects-images/conveyorbelt-large.png#lightbox "コンベヤ ベルトのページのトリプル スクリーン ショット")

(0, 0) のバケットのパスのポイントは、ハンドル、ため、`phase`引数はアニメーション、コンベヤ ベルト、おそらく下部水をスコープとアウトを上部にあるダンプ中心となるように、バケット。

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs)クラスのオーバーライドによるアニメーション、`OnAppearing`と`OnDisappearing`メソッドです。 バケットのパスは、ページのコンス トラクターで定義されます。

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

バケット作成コードを少し大きくのバケットを横向きにするに次の 2 つの変換を完了します。 これらの変換を適用することは、前のコードですべての座標を調整するよりも簡単でした。 

`PaintSurface`自体コンベヤ ベルトのパスを定義することによってハンドラーを開始します。 これは、行のペアだけとは、20-ピクセル幅灰色の線で描画を円のセミコロンのペア。

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

バケットは、200 ピクセル、コンベヤ ベルトの上に離れてに関する等間隔に配置する必要があります。 ただし、コンベヤ ベルトはおそらくありません 200 ピクセルの長さとしてつまり複数、`phase`の引数`SKPathEffect.Create1DPath`は、アニメーション化バケット ポップアップの存在の出入り。 

このため、最初に計算という名前の値`length`コンベヤ ベルトの長さはします。 コンベヤ ベルトは、直線とセミコロンの円は、これは、単純な計算。 バケットの数を割ることによって計算は、次に、 `length` 200 でします。 これは、最も近い整数に丸められます、分割数が、そのこと`length`です。 バケット数は整数の間隔になります。 `phase`引数は、その一部だけです。

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

前の例と同様に`GetFillPath`結果が異なる色を除くが表示されます。 実行した後`GetFillPath`、`newPath`アニメーション配置されていることに、呼び出し時に同じ場所に配置の各、オブジェクトには、バケットのパスの複数のコピーが含まれています。

## <a name="hatching-an-area"></a>陰影の領域

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/)メソッドは、平行線とも呼ばを使用して領域を塗りつぶします*行をハッチ*です。 メソッドには、次の構文。

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width`ハッチ線の太さを指定します。 `matrix`パラメーターはスケーリングと省略可能な回転の組み合わせ。 スケール ファクターでは、Skia は陰影の行間を使用してピクセル インクリメントを示します。 線の間の分離は、スケール ファクター マイナス、`width`引数。 かどうか、スケール ファクターは以下に、`width`値は、スペースを入れないで陰影線の間および格納する領域が表示されます。 水平および垂直方向のスケーリングに同じ値を指定します。 

既定では、ハッチ行は、水平方向です。 場合、`matrix`パラメーターには、回転が含まれています、ハッチ行が時計回りに回転します。

**ハッチ塗りつぶし**ページは、このパスの効果を示します。 [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs)クラスでは、次の 3 つのパスの効果を定義フィールドとして、されている最初の幅を示すスケール ファクターを 3 ピクセルの水平線ハッチ線間隔 6 ピクセルの各要素を区別します。 線の間の分離は、そのため、3 ピクセルです。 2 番目のパスの効果はピクセル単位の間隔 (そのため、分離が 18 ピクセルの位置)、24 の間隔の 6 ピクセル幅で行を垂直方向の陰影の斜線行 12 ピクセル幅スペース区切り 36 ピクセル間隔について、3 番目、およびです。 

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
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

マトリックスに注意してください`Multiply`メソッドです。 水平および垂直方向のスケール ファクターが同じであるためにスケールおよび回転の行列の乗算に使用される順序は重要でありません。

`PaintSurface`ハンドラーでは、これら 3 つのパスの効果を使用と組み合わせて、次の 3 つの異なる色で`fillPaint`角丸四角形に合わせてページ サイズを入力します。 `Style`プロパティに設定`fillPaint`は無視されます。 ときに、`SKPaint`オブジェクトから作成されたパスの効果が含まれています`SKPathEffect.Create2DLine`、かに関係なく、領域を塗りつぶします。

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

結果を注意深く確認する場合、角の丸い四角形に正確に赤と青の陰影の行が制限されないことが表示されます。 (これは一見、基になる Skia コードの特性です。)その他の方法が緑色で斜線の陰影の行に対して表示されるこのが不十分な場合は、: 角の丸い四角形はクリッピング パスとして使用され、ページ全体にハッチ線が描画されます。

`PaintSurface`ハンドラーの呼び出しだけを赤と青の陰影の行では、不一致を確認できるように角の丸い四角形の境界線の描画を終了します。

[![](effects-images/hatchfill-small.png "トリプル ページのスクリーン ショット、ハッチ塗りつぶし")](effects-images/hatchfill-large.png#lightbox "陰影の塗りつぶし ページのトリプル スクリーン ショット")

Android の画面に表示されない本当にそのような: スクリーン ショットのスケーリング、赤の細い線とシン広くするように見える赤色の線に統合するおよび幅のスペースが原因です。

## <a name="filling-with-a-path"></a>パスにデータを読み込む

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/)領域を並べて表示有効にすると、水平方向および垂直方向にレプリケートされるパスを持つ領域を埋めることができます。

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix`スケール ファクターは、レプリケートされたパスの水平方向および垂直のスペースを指定します。 これを使用してパスを回転することはできませんが、`matrix`引数以外の場合は、回転、パス、パス自体の回転を使用して、`Transform`によって定義されたメソッド`SKPath`です。 

レプリケートされたパスは通常塗りつぶし対象の領域ではなく、画面の左と上端に揃えられます。 この動作をオーバーライドするには、0 と左と上の辺から水平および垂直方向のオフセットを指定するスケール ファクターの間での平行移動の係数を提供します。

**パス タイル塗りつぶし**ページは、このパスの効果を示します。 領域を並べて表示するために使用されるパスがフィールドとして定義されている、 [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs)クラスです。 水平および垂直方向の座標の範囲 – 40 を 40、つまりこのパスは 80 ピクセルの四角形: 

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

`PaintSurface`ハンドラー、`SKPathEffect.Create2DPath`呼び出しがオーバー ラップする 80 ピクセルの四角形タイルが 64 水平および垂直方向の間隔を設定します。 さいわい、パスには、部分のピース、隣接するタイルで適切にメッシュがようになります。

[![](effects-images/pathtilefill-small.png "パスのタイルの塗りつぶし ページのスクリーン ショットをトリプル")](effects-images/pathtilefill-large.png#lightbox "パス タイルの塗りつぶし ページのトリプル スクリーン ショット")

元のスクリーン ショットからのスケーリングと、Android の画面上特に、ゆがみが行われます。

これらのタイルが常に全体表示され、切り捨てられなければことに注意してください。 ただし、[Windows 10 Mobile] 画面で、でもわかりにくい塗りつぶし対象の領域が丸い四角形であることはできません。 特定の領域にこれらのタイルを切り捨てるする場合は、クリッピング パスを使用します。

設定を試みてください、`Style`のプロパティ、`SKPaint`オブジェクトを`Stroke`と入力するのではなく、記載されている個々 のタイルが表示されます。

## <a name="rounding-sharp-corners"></a>とがった角を丸める

**丸め七角形**プログラムに表示される、 [**円弧を描画する方法は 3 つ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)記事が 7 つの辺の図のポイントを曲線に正接円弧を使用します。 **別丸め七角形**ページから作成されたパスの効果を使用する程度簡単な解決方法を示しています、 [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/)メソッド。

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

1 つの引数が名前付き`radius`目的角の半径を半分に設定する必要があります。 (これは、基になる Skia コードの特性です)。

ここでは、`PaintSurface`ハンドラー、 [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs)クラス。

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

この特殊効果を使用するには、線の描画またはに基づいていっぱいになると、`Style`のプロパティ、`SKPaint`オブジェクト。 ここで 3 つすべてのプラットフォームには。

[![](effects-images/anotherroundedheptagon-small.png "別の丸め七角形ページのスクリーン ショットをトリプル")](effects-images/anotherroundedheptagon-large.png#lightbox "別丸め七角形ページのトリプル スクリーン ショット")

この丸められた七角形が以前のバージョンのプログラムと同じことが表示されます。 詳細納得させる必要がある場合、角の半径は 100 では完全ではなくで指定された、50、`SKPathEffect.CreateCorner`呼び出しできますコメントを解除する、コーナーに重ね合わせて示す最後のステートメントで、プログラムと 100 radius 円を参照してください。

## <a name="random-jitter"></a>ランダムなジッター

場合がありますコンピューター_グラフィックスの完璧な直線は非常に目的の動作であり、少しランダム性が必要な。 その場合を実行する、 [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/)メソッド。

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

このパスの効果は、線の描画または入力のいずれかを使用できます。 行が接続されているセグメントに分割されます — のおおよその長さがで指定された`segLength`— とさまざまな方向に拡張します。 元の行からの偏差の範囲が指定された`deviation`です。 

最後の引数は、結果を得るのために使用される擬似乱数シーケンスの生成に使用されるシードです。 ジッター効果は、異なるシードを少し異なる外観されます。 引数には、という効果は同じプログラムを実行するたびに、ゼロの既定値があります。 する場合は異なるジッター画面が再描画されるたびシードを設定することができます、`Millisecond`のプロパティ、`DataTime.Now`値 (たとえば)。


**実験ジッター**  ページでは、四角形を描画のさまざまな値をテストすることができます。

[![](effects-images/jitterexperiment-small.png "3 つのジッター実験ページのスクリーン ショット")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

プログラムは、直接的なです。 [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml)ファイルでは、2 つをインスタンス化`Slider`要素および`SKCanvasView`:

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

`PaintSurface`ハンドラーに、 [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs)分離コード ファイルを呼び出すたびに、`Slider`値の変更。 呼び出す`SKPathEffect.CreateDiscrete`、2 つを使用して`Slider`値を四角形の境界線の描画に使用しています。

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

この効果は、このランダム偏差に従って塗りつぶす領域のアウトラインがの場合は同様に、入力を使用できます。 **ジッター テキスト** ページでは、このパスの効果を使用してテキストを表示するを示しています。 内のコードのほとんど、`PaintSurface`のハンドラー、 [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs)クラスのためのサイズ変更と、テキストを中央揃えにします。

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
        SKRect textBounds;
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

ここで 3 つすべてのプラットフォームで横モードで実行されています。

[![](effects-images/jittertext-small.png "3 つのテキストのジッターのページのスクリーン ショット")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>パスのアウトライン表示

既に見たようほとんどの 2 つの例、 [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/)メソッドの`SKPaint`にも存在している、[オーバー ロード](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

最初の 2 つの引数のみが必要です。 メソッドによって参照されているパスにアクセスする、`src`引数、ストロークのプロパティに基づくパスのデータの変更、`SKPaint`オブジェクト (など、`PathEffect`プロパティ)、し、結果を書き込みます、`dst`パス。 `resScale`パラメーターを使用して、小規模な移行先パスを作成する有効桁数を減らすこと、および`cullRect`引数は、四角形の外側の輪郭を除去できます。

このメソッドの 1 つの基本的な使い方では、パスの効果をまったく関与しません。 場合、`SKPaint`オブジェクトがその`Style`プロパティに設定`SKPaintStyle.Stroke`の場合は*いない*がその`PathEffect`設定、`GetFillPath`を表すパスを作成、*アウトライン*ソース パスのペイントのプロパティで、描画されていた場合と同様です。

たとえば場合、`src`パスは、単純な円の半径、500 のおよび`SKPaint`オブジェクトは、100 の線幅を指定します。、`dst`半径 550 の 450 および、その他の radius で 1 つ、2 つの同心円をパスになります。 メソッドを呼び出した`GetFillPath`これを埋めるため`dst`パスは、線の描画と同じ、`src`パス。 境界線を描くことができますが、`dst`パス枠線が表示へのパス。

**パスをタップすると、アウトライン**を次に示します。 `SKCanvasView`と`TapGestureRecognizer`でインスタンス化される、 [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml)ファイル。 [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs)分離コード ファイルでは、3 つを定義します`SKPaint`と線の描画に 2 つのフィールドとしてオブジェクトのストローク 100 と 20、および塗りつぶしの 3 つ目の幅。

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

画面がタップされていない場合、`PaintSurface`ハンドラーを使用して、`blueFill`と`redThickStroke`の円形パスを表示するためにオブジェクトの塗りつぶし。

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

円が入力し、想定されるように描画します。

[![](effects-images/taptooutlinethepathnormal-small.png "通常、タップをアウトライン Path ページのトリプル スクリーン ショット")](effects-images/taptooutlinethepathnormal-large.png#lightbox "標準をタップするアウトライン Path ページのトリプル スクリーン ショット")

画面をタップしたときに`outlineThePath`に設定されている`true`、および`PaintSurface`ハンドラーを作成新規`SKPath`オブジェクトおよびへの呼び出しで、移行先パスとして使用する`GetFillPath`上、`redThickStroke`ペイント オブジェクト。 移行先パスを入力し、線に適用した`redThinStroke`、その結果、次に。

[![](effects-images/taptooutlinethepathoutlined-small.png "アウトライン表示 をタップするアウトライン Path ページのトリプル スクリーン ショット")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "アウトライン表示 をタップするアウトライン Path ページのトリプル スクリーン ショット")

2 つの赤い円では、明確に元の円形パスが 2 つの循環配分に変換されたことを示します。

このメソッドが開発に使用するパスに非常に役に立ちます、`SKPathEffect.Create1DPath`メソッドです。 パスがレプリケートされるときに、これらのメソッドで指定したパスが必ず塗りつぶされます。 入力へのパス全体しない場合は、慎重にアウトラインを定義する必要があります。

たとえば、**リンク チェーン**サンプルでは、リンクの各ペアを格納するパスの領域のアウトラインの 2 つの半径に基づく 4 つの円弧の一連の定義がします。 内のコードを交換することは、`LinkedChainPage`は少し異なる方法でこれを実行するクラス。

最初を再定義する、`linkRadius`定数。

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath`目的で、その単一 radius に基づいて 2 つの円弧開始角度および掃引角度には。

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

`outlinePath`オブジェクトが、受信者の輪郭の`linkPath`場合、ストロークを付けるで指定されたプロパティを持つ`strokePaint`します。 

この手法を使用して別の例では、直近の見通しで使用されるパスには、次へ、`SKPathEffect.Create2DPath`メソッドです。 

## <a name="combining-path-effects"></a>結合パス効果

2 つの最後の静的な作成方法`SKPathEffect`は[ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/)と[ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

これら両方の方法では、複合パスの効果を作成する 2 つのパスの効果を結合します。 `CreateSum`メソッドは、次の 2 つのパス効果とは別に、次のようなパス効果を作成中に`CreateCompose`パスの 1 つの効果を適用 (、 `inner`) を適用してから、`outer`です。

既に見たよう方法、`GetFillPath`メソッドの`SKPaint`に基づいて別のパスを 1 つのパスを変換できます`SKPaint`プロパティ (含む`PathEffect`)、これ*すぎます*方法、不可解な`SKPaint`オブジェクトで指定された 2 つのパスの効果を 2 回の操作を実行できる、`CreateSum`または`CreateCompose`メソッドです。

明らかな用途の 1 つ`CreateSum`を定義するのには、`SKPaint`効果が 1 つのパス、パスを入力し、別のパスの効果を使用してパスの線にするオブジェクト。 これに示されている、**フレーム Cats**サンプルについては、キャプション、縁 cats フレーム内の配列を表示します。

[![](effects-images/catsinframe-small.png "Cats のフレームのページのスクリーン ショットをトリプル")](effects-images/catsinframe-large.png#lightbox "Cats のフレームのページのトリプル スクリーン ショット")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs)クラスがいくつかのフィールドを定義することで開始します。 最初のフィールドを認識する可能性があります、 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs)クラス、 [ **SVG パス データ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)資料です。 2 番目のパスは、直線およびフレームのひだパターンを円弧に基づきます。

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

`catPath`で使用できる、`SKPathEffect.Create2DPath`メソッド場合、`SKPaint`オブジェクト`Style`プロパティに設定されている`Stroke`です。 ただし場合、`catPath`このプログラムにし、cat の全体の先頭がいっぱいになるし、ひげは表示されませんでも直接は使用できます。 (試してみる)パスのアウトラインを取得しでそのアウトラインを使用する必要は、`SKPathEffect.Create2DPath`メソッドです。

このジョブでは、コンス トラクター。 最初に 2 つの変換を適用`catPath`を移動する、(0, 0) の中央をポイントし、サイズにスケール ダウンします。 `GetFillPath` 輪郭のすべてのアウトラインを取得`outlinedCatPath`でそのオブジェクトを使用して、`SKPathEffect.Create2DPath`呼び出します。 の要素のスケーリング、`SKMatrix`値が水平よりも少し多めと翻訳要因が、タイルの間の小さなバッファーを提供する、cat の縦のサイズが多少を派生経験的表示できるように完全 cat で、フレームの左上隅:

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

コンス トラクターを呼び出します`SKPathEffect.Create1DPath`キャプション フレーム。 パスの幅が 100 (ピクセル単位) が事前 75 ピクセル枠、レプリケートされたパスがオーバー ラップされるように注意してください。 コンス トラクターの呼び出しの最後のステートメント`SKPathEffect.CreateSum`を 2 つのパスの効果を結合し、結果に設定する、`SKPaint`オブジェクト。

により、この作業はすべて、`PaintSurface`ハンドラーを非常に単純なを指定します。 四角形を定義しを使用して描画するだけで済みます`framePaint`:

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

パスの効果の背後にあるアルゴリズムには、常に、全体で使用されるパス線の描画または塗りつぶし、表示される四角形の外側に表示するいくつかのビジュアルが発生することができますが発生します。 `ClipRect`より前のバージョンを呼び出す、`DrawRect`呼び出しがかなりクリーナーするビジュアルを許可します。 (みるクリッピングなし)

使用するが一般的`SKPathEffect.CreateCompose`別のパスの効果をいくつかのジッターを追加します。 確かに自分で試すことができますが、若干異なる例を次に示します。

**破線ハッチ**破線でハッチ行で楕円を塗りつぶします。 ほとんどの作業の[ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs)クラスが実行されるフィールド定義の右側にします。 これらのフィールドは、dash 効果と陰影効果を定義します。 として定義されている`static`にし、参照されているため、`SKPathEffect.CreateCompose`で呼び出し、`SKPaint`定義。

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
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface`標準的なオーバーヘッドおよび 1 回の呼び出しのみを格納するハンドラーの必要性`DrawOval`:

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

した既に説明したように、ハッチ行が、領域の内部に正確に制限はなく、この例では、常に始まる左側全体をダッシュに。

[![](effects-images/dashedhatchlines-small.png "陰影の破線のページのスクリーン ショットをトリプル")](effects-images/dashedhatchlines-large.png#lightbox "ハッチ破線ページのトリプル スクリーン ショット")

これで、単純なパスの効果を確認したドットと、予想外の組み合わせにダッシュを想像して作成することができますを参照してください。



## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
