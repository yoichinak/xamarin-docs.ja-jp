---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f3a5a581ffb4ca2acf1d4209b8b7a744f0daa5eb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128052"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp のパス効果

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_描画と塗りつぶしにパスを使用できるようにするさまざまなパス効果を発見します。_

*パス効果*は、 [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) クラスによって定義された8つの静的作成メソッドの1つで作成されるクラスのインスタンスです。 次に、オブジェクトは、 `SKPathEffect` [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) さまざまな興味深い効果を与えるために、オブジェクトのプロパティに設定され [`SKPaint`](xref:SkiaSharp.SKPaint) ます。たとえば、小さなレプリケートパスを使用して行を描画します。

![リンクチェーンのサンプル](effects-images/patheffectsample.png)

パスの効果を使用すると、次のことができます。

- ドットとダッシュで線を描く
- 塗りつぶしパスを使用して線を描く
- ハッチ線で領域を塗りつぶす
- タイル化されたパスで領域を塗りつぶす
- 鋭い角を丸める
- 直線と曲線にランダムな "ジッター" を追加する

さらに、2つ以上のパス効果を組み合わせることもできます。

この記事では、 [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) のメソッドを使用し `SKPaint` て、やなどののプロパティを適用することによって、あるパスを別のパスに変換する方法についても説明し `SKPaint` `StrokeWidth` `PathEffect` ます。 これにより、別のパスのアウトラインであるパスを取得するなど、いくつかの興味深い手法が得られます。 `GetFillPath`パスの効果との接続にも役立ちます。

## <a name="dots-and-dashes"></a>ドットとダッシュ

メソッドの使用方法については、 [`PathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) 「[**ドットとダッシュ**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)」を参照してください。 メソッドの最初の引数は、2つ以上の値を含む配列で、ダッシュの長さとダッシュ間のギャップの長さを交互に指定します。

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

これらの値は、ストロークの幅に対して相対的では*ありません*。 たとえば、ストロークの幅が10で、角が直角で四角形のギャップがある場合は、 `intervals` 配列を {10, 10} に設定します。 引数は、 `phase` ダッシュパターン内の行の開始位置を示します。 この例では、行が四角形の間隔で始まるようにするには、を `phase` 10 に設定します。

ダッシュの端は、のプロパティの影響を受け `StrokeCap` `SKPaint` ます。 幅の広いストロークでは、このプロパティをに設定して、ダッシュの端を丸めることがよくあり `SKStrokeCap.Round` ます。 この場合、配列内の値には、 `intervals` 丸め処理によって得られる余分な長さは含ま*れません*。 これは、円形のドットでは、幅0を指定する必要があることを意味します。 ストロークの幅が10の場合は、同じ直径のドットとギャップを含む直線を作成するには、 `intervals` {0, 20} の配列を使用します。

アニメーション化された**点線のテキスト**ページは、オブジェクトのプロパティをに設定することによって、テキスト[**とグラフィックスの統合**](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)に関する記事で説明されているテキストページに似てい**Outlined Text** `Style` `SKPaint` `SKPaintStyle.Stroke` ます。 さらに、**アニメーション**化された点線のテキストはを使用して、 `SKPathEffect.CreateDash` このアウトラインを点線で表示します。また、プログラムは、メソッドの引数をアニメーション化して、 `phase` `SKPathEffect.CreateDash` ドットがテキスト文字の周りを移動するようにします。 横モードのページを次に示します。

[![アニメーション化された点線のテキストページのトリプルスクリーンショット](effects-images/animateddottedtext-small.png)](effects-images/animateddottedtext-large.png#lightbox)

クラスは、 [`AnimatedDottedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) いくつかの定数を定義することから `OnAppearing` 始め `OnDisappearing` ます。また、アニメーションのメソッドとメソッドもオーバーライドします。

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

ハンドラーは、 `PaintSurface` テキストを表示するオブジェクトを作成することから始め `SKPaint` ます。 プロパティは、 `TextSize` 画面の幅に基づいて調整されます。

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

メソッドの末尾に向かって、フィールドとして定義されたを使用し、アニメーション化された `SKPathEffect.CreateDash` 値を使用してメソッドが呼び出され `dashArray` `phase` ます。 `SKPathEffect`インスタンスは、 `PathEffect` テキストを表示するために、オブジェクトのプロパティに設定され `SKPaint` ます。

または、 `SKPathEffect` `SKPaint` テキストを測定してページを中央揃えにする前に、オブジェクトをオブジェクトに設定することもできます。 ただし、アニメーション化されたドットとダッシュは、レンダリングされたテキストのサイズに多少の変化を与え、テキストは少しバイブレーションになる傾向があります。 (試してみてください)

また、アニメーション化されたドットがテキスト文字を囲んでいると、各閉じられた曲線には、ドットが存在しているように見えます。 ここで、文字アウトラインの開始位置と終了位置を定義するパスを指定します。 パスの長さが、ダッシュパターン (この例では20ピクセル) の長さの整数倍でない場合は、そのパターンの一部だけがパスの末尾に収まります。

パスの長さに合わせてダッシュパターンの長さを調整することはできますが、それには、パスの長さを決定する必要があります。これは、記事の[**パス情報と列挙体**](information.md)で説明されている技法です。

**ドット/破線の変形**プログラムは、ダッシュがドットに分割されるようにダッシュパターンをアニメーション化します。これにより、ダッシュがもう一度ダッシュの形になります。

[![ドットダッシュの変形ページのトリプルスクリーンショット](effects-images/dotdashmorph-small.png)](effects-images/dotdashmorph-large.png#lightbox)

クラスは、 [`DotDashMorphPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) `OnAppearing` 前のプログラムと同じように、メソッドとメソッドをオーバーライドし `OnDisappearing` ますが、クラスは `SKPaint` オブジェクトをフィールドとして定義します。

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

ハンドラーは、 `PaintSurface` ページのサイズに基づいて楕円パスを作成し、変数と変数を設定するコードの長いセクションを実行し `dashArray` `phase` ます。 アニメーション化された変数の範囲が 0 ~ 1 の場合は、ブロックによって `t` `if` その時間が4四半期に分割され、これらの四半期ごとに 0 ~ 1 の範囲が設定され `tsub` ます。 最後に、プログラムはを作成し、 `SKPathEffect` オブジェクトに設定して `SKPaint` 描画します。

## <a name="from-path-to-path"></a>パスからパスへ

[`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single))のメソッドは、 `SKPaint` オブジェクトの設定に基づいて、あるパスを別のパスに変換し `SKPaint` ます。 これがどのように動作するかを確認するには、 `canvas.DrawPath` 前のプログラムの呼び出しを次のコードに置き換えます。

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

この新しいコードでは、 `GetFillPath` の呼び出しによって `ellipsePath` (楕円だけの) がに変換 `newPath` されます。これがと共に表示され `newPaint` ます。 オブジェクトは、 `newPaint` すべての既定のプロパティ設定を使用して作成され `Style` ます。ただし、からのブール値に基づいてプロパティが設定される点が異なり `GetFillPath` ます。

ビジュアルは、で設定されている色以外は同じです `ellipsePaint` `newPaint` 。 に定義されている単純な楕円ではなく `ellipsePath` 、 `newPath` 一連のドットとダッシュを定義するパスの輪郭が多数含まれています。 これは、(具体的には、、、および) のさまざまなプロパティをに適用し、結果の `ellipsePaint` `StrokeWidth` パスを `StrokeCap` `PathEffect` `ellipsePath` に配置 `newPath` した結果です。 メソッドは、 `GetFillPath` コピー先のパスを埋めるかどうかを示すブール値を返します。この例では、戻り値は `true` パスを埋めるためのものです。

`Style`の設定をに変更 `newPaint` する `SKPaintStyle.Stroke` と、1ピクセル幅の線で囲まれた個々のパスの輪郭が表示されます。

## <a name="stroking-with-a-path"></a>パスを使用した描画

[`SKPathEffect.Create1DPath`](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle))メソッドは概念的にはと似ていますが、 `SKPathEffect.CreateDash` ダッシュとギャップのパターンではなくパスを指定する点が異なります。 このパスは、直線または曲線を描画するために複数回レプリケートされます。

の構文は次のとおりです。

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

一般に、に渡すパスは小さく、 `Create1DPath` ポイント (0, 0) の中心になります。 パラメーターは、パス `advance` が行にレプリケートされるときのパスの中心との距離を示します。 通常、この引数には、パスのおおよその幅を設定します。 引数は、 `phase` メソッドの場合と同じロールをここで再生し `CreateDash` ます。

に [`SKPath1DPathEffectStyle`](xref:SkiaSharp.SKPath1DPathEffectStyle) は、次の3つのメンバーがあります。

- `Translate`
- `Rotate`
- `Morph`

`Translate`このメンバーは、直線または曲線に沿って複製されるため、パスが同じ向きのままになります。 の場合 `Rotate` 、曲線の接線に基づいてパスが回転します。 水平線の通常の向きはパスです。 `Morph`はと似ていますが、 `Rotate` パス自体も曲線化されている線の曲率に一致します。

[ **1D パスの効果**] ページでは、これら3つのオプションについて説明します。 [**OneDimensionalPathEffectPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml)ファイルは、列挙体の3つのメンバーに対応する3つの項目を含むピッカーを定義します。

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

[**OneDimensionalPathEffectPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs)分離コードファイルは、3つの `SKPathEffect` オブジェクトをフィールドとして定義します。 これらはすべて、を使用して作成されたオブジェクトを使用して作成され `SKPathEffect.Create1DPath` `SKPath` `SKPath.ParseSvgPathData` ます。 1つ目は単純なボックスで、2つ目はひし形で、3番目は四角形です。 これらは、3つの効果のスタイルを示すために使用されます。

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

ハンドラーは、 `PaintSurface` それ自体をループするベジエ曲線を作成し、ピッカーにアクセスし `PathEffect` て、ストロークを描画するために使用する必要があるものを決定します。 、、およびの3つのオプション `Translate` `Rotate` `Morph` が左から右に表示されます。

[![1D パス効果ページのトリプルスクリーンショット](effects-images/1dpatheffect-small.png)](effects-images/1dpatheffect-large.png#lightbox)

メソッドに指定されたパス `SKPathEffect.Create1DPath` は、常に塗りつぶされます。 `DrawPath` `SKPaint` オブジェクトの `PathEffect` プロパティが1d パス効果に設定されている場合、メソッドに指定されたパスは常にストロークになります。 オブジェクトには設定がないことに注意して `pathPaint` `Style` ください。通常は既定でに設定 `Fill` されますが、パスはに関係なくストロークになります。

この例で使用するボックス `Translate` は20ピクセルの四角形で、 `advance` 引数は24に設定されています。 この違いによって、線がほぼ水平または垂直になったときのボックス間の間隔がずれますが、ボックスの対角線が28.3 ピクセルであるため、線が斜めになると、ボックスの間が重なることになります。

この例のダイヤモンド図形 `Rotate` も、幅20ピクセルです。 `advance`は20に設定されています。これにより、ひし形が直線の曲率と共に回転している間、ポイントは引き続きタッチされます。

この例の四角形の形状 `Morph` は50ピクセル幅で、 `advance` 四角形がベジエ曲線の周りに曲がっている間のギャップを小さくするために55が設定されています。

`advance`引数がパスのサイズよりも小さい場合、レプリケートされたパスが重複する可能性があります。 これにより、いくつかの興味深い効果が得られます。 [**リンクチェーン**] ページには、リンクチェーンに似ているような重なり合う一連の円が表示されます。これは、catenary の特徴的な形状でハングします。

[![[リンクされたチェーン] ページのトリプルスクリーンショット](effects-images/linkedchain-small.png)](effects-images/linkedchain-large.png#lightbox)

非常に近いので、実際には円ではないことがわかります。 チェーン内の各リンクは、サイズと位置が隣接する2つの円弧であり、隣接するリンクに接続されているように見えます。

均一な重量分布のチェーンまたはケーブルが、catenary の形式でハングします。 Catenary の形で構築された arch は、arch の重みからの圧力の均等分布から得られます。 Catenary には、一見単純な数学的な説明があります。

`y = a · cosh(x / a)`

*Cosh*はハイパーボリックコサイン関数です。 *X*が0の場合、 *cosh*は0になり、 *y*は*a*になります。 Catenary の中心です。 *余弦*関数と同様に、 *cosh*は "*偶数*" と呼ばれます。つまり、 *cosh (– x)* は*cosh (x)* になり、正または負の引数を増やすと値が増加します。 これらの値は、catenary の辺を形成する曲線を表します。

スマートフォンのページの寸法に合うように、の適切*な*値を見つけることは、直接計算ではありません。 *W*と*h*が四角形の幅と高さの場合、の最適*な*値は次の式を満たすことができます。

`cosh(w / 2 / a) = 1 + h / a`

クラスの次のメソッドは、 [`LinkedChainPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) 等号 (=) の左側および右側の2つの式をとして参照することで、その等価性を組み込み `left` `right` ます。 の値が小さい*場合*、 `left` はより大きくなり `right` ます。の値が大きい*場合*、 `left` はよりも小さくなり `right` ます。 `while`ループは *、* の最適な値に対して次のように縮小します。

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

`SKPath`リンクのオブジェクトはクラスのコンストラクター内に作成され、結果の `SKPathEffect` オブジェクトは、 `PathEffect` `SKPaint` フィールドとして格納されるオブジェクトのプロパティに設定されます。

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

ハンドラーの主なジョブ `PaintSurface` は、catenary 自体のパスを作成することです。 最適*なを*決定し、変数に格納 `optA` すると、ウィンドウの上部からオフセットを計算する必要もあります。 次に、catenary の値のコレクションを累積し `SKPoint` 、それをパスに変換して、前に作成したオブジェクトを使用してパスを描画し `SKPaint` ます。

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

このプログラムは、 `Create1DPath` 中央に (0, 0) ポイントを設定するためにで使用されるパスを定義します。 これは、パスの (0, 0) ポイントが、装飾している直線または曲線に沿って整列されているため、合理的なように思われます。 ただし、一部の特殊効果には、中心外 (0, 0) のポイントを使用できます。

**コンベヤベルト**ページは、しっぽコンベヤベルトに似たパスを作成します。このパスは、上部と下部の曲線で、ウィンドウのサイズに合わせて調整されます。 このパスは、単純なオブジェクトの幅が20ピクセル、灰色で色分けされています。その後、別のオブジェクトを使用して、 `SKPaint` `SKPaint` `SKPathEffect` 小さいバケットに似たパスを参照するオブジェクトを使用して、もう一度ストロークを付けます。

[![コンベヤベルトページのトリプルスクリーンショット](effects-images/conveyorbelt-small.png)](effects-images/conveyorbelt-large.png#lightbox)

バケットパスの (0, 0) ポイントはハンドルであるため、引数がアニメーション化されている場合、 `phase` バケットはコンベヤベルトの周りに回転しているように見えます。たとえば、水を scooping、一番上にダンプします。

クラスは、 [`ConveyorBeltPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) メソッドとメソッドのオーバーライドを含むアニメーションを実装し `OnAppearing` `OnDisappearing` ます。 バケットのパスは、ページのコンストラクターで定義されます。

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

バケット作成コードは、2つの変換を使用して完了します。これにより、バケットは少し大きくなり、横方向に変わります。 これらの変換を適用することは、前のコードのすべての座標を調整するよりも簡単でした。

この `PaintSurface` ハンドラーは、コンベヤベルト自体のパスを定義することから始まります。 これは単なる直線と、20ピクセル幅の濃い灰色の線で描画される半円のペアです。

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

コンベヤベルトの描画ロジックは、横モードでは機能しません。

バケットは、コンベヤベルト上で200ピクセルの間隔で配置する必要があります。 しかし、コンベヤベルトは200ピクセルの倍数ではないと思います。つまり、の引数がアニメーション化されると `phase` `SKPathEffect.Create1DPath` 、バケットが表示され、存在しなくなります。

このため、プログラムはまず、 `length` コンベヤベルトの長さであるという名前の値を計算します。 コンベヤベルトは直線と半円で構成されているため、これは単純な計算です。 次に、バケット数は200で割って計算され `length` ます。 これは最も近い整数に丸められ、その数値がに分割され `length` ます。 結果は、整数のバケット数のスペースになります。 `phase`引数は、そのほんの一部にすぎません。

## <a name="from-path-to-path-again"></a>パスからパスへの再実行

`DrawSurface`**コンベヤベルト**のハンドラーの下部で、呼び出しをコメントアウト `canvas.DrawPath` し、次のコードに置き換えます。

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

前の例と同様に、 `GetFillPath` 結果は色を除いて同じであることがわかります。 を実行した後 `GetFillPath` 、オブジェクトには `newPath` バケットパスの複数のコピーが格納されます。各コピーは、呼び出し時にアニメーションによって配置されたのと同じ場所に配置されます。

## <a name="hatching-an-area"></a>領域の陰影

メソッドは、 [`SKPathEffect.Create2DLines`](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix)) 領域に、通常は*ハッチ線*と呼ばれる平行線を塗りつぶします。 メソッドの構文は次のとおりです。

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

引数は、 `width` ハッチ線のストロークの幅を指定します。 パラメーターは、 `matrix` スケーリングとオプションの回転を組み合わせたものです。 スケールファクターは、Skia がハッチ線にスペースを入れるために使用するピクセルインクリメントを示します。 行を分離するには、スケールファクターから引数を引いた値を減算し `width` ます。 スケールファクターが値以下の場合 `width` 、ハッチ線の間にはスペースがなく、領域は塗りつぶされて表示されます。 水平および垂直方向のスケーリングに同じ値を指定します。

既定では、ハッチ線は水平方向です。 パラメーターに `matrix` 回転が含まれている場合、ハッチ線は時計回りに回転します。

**ハッチの塗りつぶし**ページは、このパスの効果を示しています。 クラスは、 [`HatchFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) 3 つのパス効果をフィールドとして定義します。1つは水平ハッチ線、もう1つは幅3ピクセル、幅が6ピクセル離れていることを示すスケールファクターです。 このため、行を分離するのは3ピクセルです。 2番目のパスの効果は、幅が6ピクセルの水平方向のハッチ線に対して24ピクセル間隔で配置されています (したがって、分離は18ピクセル)。3番目のパスは、斜めのハッチ線が12ピクセル、幅が36ピクセルになるようにします。

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

Matrix メソッドに注意して `Multiply` ください。 水平方向と垂直方向のスケールファクターは同じであるため、スケーリング行列と回転行列が乗算される順序は関係ありません。

このハンドラーは、3つの `PaintSurface` 異なる色をと組み合わせて、この3つのパス効果を使用して、 `fillPaint` ページに収まるように角丸四角形のサイズを設定します。 に設定された `Style` プロパティ `fillPaint` は無視されます。 `SKPaint` オブジェクトにから作成されたパス効果が含まれている場合、 `SKPathEffect.Create2DLine` 領域は次のように塗りつぶされます。

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

結果をよく見ると、赤と青のハッチ線が丸みのある四角形に正確には見えないことがわかります。 (これは、基になる Skia コードの特性です)。これが不十分な場合は、左の斜線のハッチ線に対して別の方法が表示されます。角丸四角形はクリッピングパスとして使用され、ハッチ線はページ全体に描画されます。

ハンドラーは、 `PaintSurface` 角が丸い四角形を単に描くための呼び出しで終了します。これにより、赤と青のハッチ線との違いを確認できます。

[![ハッチの塗りつぶしページのトリプルスクリーンショット](effects-images/hatchfill-small.png)](effects-images/hatchfill-large.png#lightbox)

Android の画面は次のようには見えません。スクリーンショットの拡大縮小によって、細い赤の線と細いスペースが、大きな赤い線と幅の広い領域に統合されました。

## <a name="filling-with-a-path"></a>パスの入力

を使用すると、 [`SKPathEffect.Create2DPath`](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath)) 水平方向および垂直方向にレプリケートされるパスを領域に塗りつぶすことができます。

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

スケールファクターは、レプリケートされた `SKMatrix` パスの水平方向と垂直方向の間隔を示します。 ただし、この引数を使用してパスをローテーションすることはできません `matrix` 。パスを回転させるには、で定義されているメソッドを使用して、パス自体を回転 `Transform` させ `SKPath` ます。

通常、レプリケートされたパスは、塗りつぶされる領域ではなく、画面の左端と上端に揃えられます。 この動作をオーバーライドするには、0とスケールファクターの間に変換要素を指定して、左右のオフセットと垂直方向のオフセットを指定します。

**パスタイルの塗りつぶし**ページで、このパスの効果を示します。 領域のタイリングに使用されるパスは、クラスのフィールドとして定義され [`PathTileFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) ます。 水平方向と垂直方向の座標の範囲は– 40 ~ 40 です。つまり、このパスは80ピクセルの四角形です。

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

ハンドラーでは、呼び出しによって、 `PaintSurface` `SKPathEffect.Create2DPath` 水平方向と垂直方向の間隔が64に設定され、80ピクセルの正方形のタイルが重なります。 幸いなことに、パスはパズルピースに似ており、隣接するタイルを使用して適切にメッシュします。

[![パスタイルの [塗りつぶし] ページのトリプルスクリーンショット](effects-images/pathtilefill-small.png)](effects-images/pathtilefill-large.png#lightbox)

元のスクリーンショットからスケーリングすると、特に Android の画面で何らかのひずみが発生します。

これらのタイルは常に表示され、切り捨てられることはありません。 最初の2つのスクリーンショットでは、塗りつぶされている領域が角丸四角形であることは明らかではありません。 これらのタイルを特定の領域に切り捨てたい場合は、クリッピングパスを使用します。

`Style`オブジェクトのプロパティをに設定 `SKPaint` する `Stroke` と、個々のタイルが塗りつぶされずに表示されます。

また、「 [**SkiaSharp bitmap タイリング (ビットマップのタイル**](../effects/shaders/bitmap-tiling.md)化)」の記事に示されているように、タイル化されたビットマップで領域を塗りつぶすこともできます。

## <a name="rounding-sharp-corners"></a>丸み (鋭い角)

[**弧の描画に使用する3つの方法**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)である**丸い Heptagon**プログラムでは、接線円弧を使用して、両側の図形の点を曲線しています。 **もう1つの丸い Heptagon**ページには、メソッドから作成されたパス効果を使用する、より簡単な方法が示されてい [`SKPathEffect.CreateCorner`](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single)) ます。

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

1つの引数に名前が付けられますが、必要な `radius` 角の半径の半分に設定する必要があります。 (これは、基になる Skia コードの特性です)。

`PaintSurface`クラスのハンドラーを次に示し [`AnotherRoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) ます。

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

この効果は、オブジェクトのプロパティに基づいて、描画と塗りつぶしのどちらでも使用でき `Style` `SKPaint` ます。 次のように実行されています。

[![別の丸い Heptagon ページのトリプルスクリーンショット](effects-images/anotherroundedheptagon-small.png)](effects-images/anotherroundedheptagon-large.png#lightbox)

この丸い heptagon は、以前のプログラムと同じであることがわかります。 角の半径が、呼び出しで指定された50ではなく、実際には100であることをより正確に示す必要がある場合は、プログラムの最後のステートメントをコメント解除して、 `SKPathEffect.CreateCorner` 100 半径の円が角に重ねて表示されるようにします。

## <a name="random-jitter"></a>ランダムなジッター

場合によっては、コンピューターのグラフィックスが完璧に使用されることはないので、少しランダムにする必要があります。 そのような場合は、メソッドを試してみましょう [`SKPathEffect.CreateDiscrete`](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32)) 。

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

このパス効果は、描画と塗りつぶしのどちらにも使用できます。 線は、によって指定されたおおよその長さである、接続されたセグメントに分割され、 `segLength` さまざまな方向に拡張されます。 元の行からの偏差の範囲は、によって指定され `deviation` ます。

Final 引数は、効果に使用される擬似乱数シーケンスを生成するために使用されるシードです。 ジッター効果は、シードが異なると少し異なります。 引数の既定値は0です。これは、プログラムを実行するたびに効果が同じであることを意味します。 画面が再描画されるたびに異なるジッターが必要な場合は、シードを `Millisecond` 値のプロパティに設定でき `DataTime.Now` ます (たとえば、)。

[**ジッター実験**] ページでは、四角形の描画で異なる値を試すことができます。

[![JitterExperiment ページのトリプルスクリーンショット](effects-images/jitterexperiment-small.png)](effects-images/jitterexperiment-large.png#lightbox)

プログラムは簡単です。 JitterExperimentPage ファイルは、次の2つの要素とをインスタンス化し[**ます。**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) `Slider` `SKCanvasView`

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

`PaintSurface` [**JitterExperimentPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs)分離コードファイル内のハンドラーは、値が変更されるたびに呼び出され `Slider` ます。 `SKPathEffect.CreateDiscrete`2 つの値を使用してを呼び出し `Slider` 、それを使用して四角形を描画します。

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

この効果は、塗りつぶしにも使用できます。この場合、塗りつぶされた領域の輪郭は、これらのランダムな偏差の対象になります。 **ジッターテキスト**ページは、このパス効果を使用してテキストを表示する方法を示しています。 クラスのハンドラーのコードの大部分 `PaintSurface` [`JitterTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) は、テキストのサイズを変更し、中央揃えにします。

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

ここでは、横モードで実行されています。

[![JitterText ページのトリプルスクリーンショット](effects-images/jittertext-small.png)](effects-images/jittertext-large.png#lightbox)

## <a name="path-outlining"></a>パスのアウトライン

2つの [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) バージョンが存在する、のメソッドの2つの簡単な例については既に説明しました `SKPaint` 。

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

最初の2つの引数のみが必要です。 メソッドは、引数によって参照されるパスにアクセスし `src` 、オブジェクトのストロークプロパティに基づいてパスデータを変更し `SKPaint` (プロパティを含む `PathEffect` )、結果を `dst` パスに書き込みます。 `resScale`パラメーターを使用すると、精度を下げてより小さい変換先パスを作成できます。また、引数を使用すると、 `cullRect` 四角形の外側にある輪郭を除去できます。

このメソッドの基本的な使用方法の1つは、パスの効果には関係ありません。 `SKPaint` オブジェクトのプロパティがに設定されていて、が設定されていない場合 `Style` 、は `SKPaintStyle.Stroke` *not* `PathEffect` 、 `GetFillPath` 描画プロパティによってストロークが付けられているかのように、ソースパスの*アウトライン*を表すパスを作成します。

たとえば、 `src` パスが半径500の単純な円で、オブジェクトがストローク幅100を指定している場合、 `SKPaint` パスは `dst` 2 つの同心円円になります。1つは半径450、もう一方は半径が550です。 `GetFillPath`このパスの入力 `dst` がパスの描画と同じであるため、メソッドが呼び出され `src` ます。 ただし、パスをストロークし `dst` て、パスのアウトラインを表示することもできます。

ここでは、**パスのアウトラインをタップ**して説明します。 とは、 `SKCanvasView` `TapGestureRecognizer` [**TapToOutlineThePathPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml)ファイルでインスタンス化されます。 [**TapToOutlineThePathPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs)分離コードファイルでは、3つの `SKPaint` オブジェクトをフィールドとして定義します。ストロークの幅が 100 ~ 20 の描画用に2つ、3番目のオブジェクトを入力します。

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

画面がタップされていない場合、 `PaintSurface` ハンドラーは `blueFill` オブジェクトと描画オブジェクトを使用して、 `redThickStroke` 循環パスをレンダリングします。

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

円は、意図したとおりに塗りつぶされ、ストロークされます。

[![[パス] ページの概要を示す、通常のタップのトリプルスクリーンショット](effects-images/taptooutlinethepathnormal-small.png)](effects-images/taptooutlinethepathnormal-large.png#lightbox)

画面をタップすると、 `outlineThePath` がに設定され、ハンドラーによって `true` `PaintSurface` 新しいオブジェクトが作成され、 `SKPath` そのオブジェクトが描画オブジェクトのへの呼び出しでターゲットパスとして使用され `GetFillPath` `redThickStroke` ます。 その後、その移動先のパスが塗りつぶされ、と共に表示され `redThinStroke` ます。その結果、次のようになります。

[![[パス] ページの概要を示す三重スクリーンショット](effects-images/taptooutlinethepathoutlined-small.png)](effects-images/taptooutlinethepathoutlined-large.png#lightbox)

2つの赤い円は、元の円形のパスが2つの円形の輪郭に変換されたことを明確に示しています。

このメソッドは、メソッドに使用するパスを開発する場合に非常に便利です `SKPathEffect.Create1DPath` 。 これらの方法で指定するパスは、パスがレプリケートされるときに常に入力されます。 パス全体を塗りつぶす必要がない場合は、アウトラインを慎重に定義する必要があります。

たとえば、**リンクチェーン**のサンプルでは、リンクは一連の4つの円弧で定義されています。各ペアは、塗りつぶされるパスの領域をアウトラインするために2つの半径に基づいています。 クラスのコードを置き換えることで、 `LinkedChainPage` 少し異なる操作を行うことができます。

まず、定数を再定義し `linkRadius` ます。

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

は、 `linkPath` その1つの半径に基づく2つの円弧で、必要な開始角度とスイープ角度を持つようになりました。

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

`outlinePath`次に、オブジェクトは、 `linkPath` に指定されたプロパティを使用して描画されたときのアウトラインの受信者になり `strokePaint` ます。

この手法を使用するもう1つの例は、メソッドで使用されるパスの次の例です。

## <a name="combining-path-effects"></a>パスの効果の結合

の最後の2つの静的作成メソッドは、 `SKPathEffect` [`SKPathEffect.CreateSum`](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) と [`SKPathEffect.CreateCompose`](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) です。

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

どちらのメソッドも、2つのパス効果を組み合わせて、複合パス効果を作成します。 メソッドは、 `CreateSum` 個別に適用される2つのパス効果に似たパス効果を作成し、 `CreateCompose` 1 つのパス効果 () を適用して `inner` 、を `outer` それに適用します。

`GetFillPath`のメソッドがプロパティに基づいて1つのパスを別のパスに変換する方法については既に説明しました `SKPaint` `SKPaint` (を含む `PathEffect` ) *too* `SKPaint` `CreateSum` 。またはメソッドで指定した2つのパスの効果を使用して、オブジェクトがその操作を2回実行する方法を理解しすぎない `CreateCompose` ようにすることができます。

の1つの使用方法として `CreateSum` は、パスに `SKPaint` 1 つのパス効果を設定し、別のパス効果でパスをストロークするオブジェクトを定義することが挙げられます。 これについては、scalloped のエッジがあるフレーム内の猫の配列を表示する、[**フレーム内の猫**のサンプル] に示されています。

[![フレームページの猫のトリプルスクリーンショット](effects-images/catsinframe-small.png)](effects-images/catsinframe-large.png#lightbox)

クラスは、 [`CatsInFramePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) いくつかのフィールドを定義することから始まります。 [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) [**SVG パスデータ**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)の記事から、クラスの最初のフィールドを認識している場合があります。 2番目のパスは、フレームの scallop パターンの線と弧に基づいています。

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

`catPath` `SKPathEffect.Create2DPath` `SKPaint` オブジェクトの `Style` プロパティがに設定されている場合は、メソッドでを使用でき `Stroke` ます。 ただし、 `catPath` このプログラムでを直接使用した場合、cat の先頭全体が塗りつぶされ、ひげも表示されません。 (試してみてください)そのパスのアウトラインを取得し、そのアウトラインをメソッドで使用する必要が `SKPathEffect.Create2DPath` あります。

コンストラクターはこのジョブを行います。 まず、に2つの変換を適用して `catPath` 、(0, 0) ポイントを中央に移動し、サイズをスケールダウンします。 `GetFillPath`の輪郭のすべてのアウトラインを取得 `outlinedCatPath` し、そのオブジェクトを呼び出しで使用し `SKPathEffect.Create2DPath` ます。 値のスケールファクター `SKMatrix` は、タイル間に小さなバッファーを提供するために、cat の水平方向と垂直方向のサイズよりわずかに大きくなります。一方、翻訳要素は、フレームの左上隅に完全な cat が表示されるように経験的に派生しています。

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

次に、コンストラクターは `SKPathEffect.Create1DPath` scalloped フレームに対してを呼び出します。 パスの幅が100ピクセルであることに注意してください。ただし、その前には75ピクセルで、レプリケートされたパスがフレームの周りに重なっています。 コンストラクターの最後のステートメントは、 `SKPathEffect.CreateSum` を呼び出して2つのパス効果を結合し、結果をオブジェクトに設定し `SKPaint` ます。

このすべての作業により、 `PaintSurface` ハンドラーが非常に単純になります。 次のものを使用して四角形を定義し、描画するだけです `framePaint` 。

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

パス効果の背後にあるアルゴリズムでは、描画または塗りつぶしに使用されるパス全体が常に表示されるため、一部のビジュアルが四角形の外に表示されることがあります。 `ClipRect`呼び出しの前の呼び出しにより、 `DrawRect` ビジュアルをかなり簡潔にすることができます。 (クリッピングを行わずに試してみてください)

通常は、別の `SKPathEffect.CreateCompose` パス効果にいくつかのジッターを追加するために使用します。 自分で試してみることはできますが、次のような例があります。

**破線のハッチ線**は、破線のハッチ線を使用して楕円を塗りつぶします。 クラスのほとんどの作業 [`DashedHatchLinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) は、フィールド定義で直接実行されます。 これらのフィールドは、ダッシュ効果とハッチ効果を定義します。 これらは `static` 定義内の呼び出しで参照されるため、として定義され `SKPathEffect.CreateCompose` `SKPaint` ます。

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

ハンドラーには、 `PaintSurface` 標準のオーバーヘッドとの1回の呼び出しだけを含める必要があり `DrawOval` ます。

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

既に明らかになっているように、ハッチ線は領域の内部に対して厳密には制限されていません。この例では、常にダッシュ (_) で左側から開始します。

[![破線の [ハッチ線] ページのトリプルスクリーンショット](effects-images/dashedhatchlines-small.png)](effects-images/dashedhatchlines-large.png#lightbox)

これで、単純なドットとダッシュから奇妙な組み合わせまで、パスの効果がわかりました。次は、想像力を使って作成できるものを確認してください。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
