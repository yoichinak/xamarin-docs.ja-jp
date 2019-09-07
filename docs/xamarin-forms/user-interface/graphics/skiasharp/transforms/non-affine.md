---
title: 非アフィン変換
description: この記事では、パースペクティブとテーパ効果、変換行列の 3 番目の列を作成する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
ms.openlocfilehash: eb7057d40e6ff0c48c6dc1b5dc38af2eb92de2e0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772779"
---
# <a name="non-affine-transforms"></a>非アフィン変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_変換行列の 3 番目の列でのテーパ効果とパースペクティブを作成します。_

平行移動、拡大縮小、回転、および傾斜はすべてな分類*アフィン*を変換します。 アフィン変換では、本の平行線が保持されます。 2 つの行が、変換する前に並列の場合は、引き続き並列、変換した後。 四角形は、常に parallelograms に変換されます。

ただし、SkiaSharp が非アフィン変換で、四角形を凸の四角形に変換する機能がありますの対応もです。

![](non-affine-images/nonaffinetransformexample.png "凸の四角形に変換されたビットマップ")

凸の四角形は、常に 180 度や互いに交差しない辺よりも少ない内部角度の 4 辺の図です。

非アフィンに変換の変換行列の 3 番目の行が 0、0、および 1 以外の値に設定されている場合の結果をします。 完全な`SKMatrix`乗算は。

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

結果の変換式は次のとおりです。

x' = ScaleX·x + SkewX·y + x 方向

y' SkewY·x + ScaleY·y 直線移動を =

z' Persp0·x + Persp1·y Persp2 を =

3-3 でマトリックスを使用して、2 次元変換のための基本的なルールは、維持に努めます平面上に Z が 1 です。 しない限り、`Persp0`と`Persp1`は 0、および`Persp2`1 と等しい、変換には、その面を Z 座標は移動しました。

2 次元変換には、これを復元するには座標する必要がありますに再び移動その平面。 別の手順が必要です。 X'、y'、z '値必要がありますで除算 z' とします。

'x = x'/z'

'y = y'/z'

z"z ='/z' = 1

これらと呼ばれます。*同次座標*Möbius ストリップちなんで年 8 月 Ferdinand Möbius、かなりよく知られている、トポロジの奇妙な点のによって開発されたとします。

場合 z' は 0、無限の座標での除算の結果です。 実際には、有限数で無限の値を表現できるため、同次座標を開発するための Möbius の動機の 1 つでした。

グラフィックスを表示するときに、しないようにする無限の値に変換する座標を使用して何かを表示します。 これらの座標は表示されません。 これらの座標ポインターの近くにあるものは非常に大きな、視覚的にわかりやすいではない可能性があります。

この数式でたくない z の値 ' 0 になります。

z' Persp0·x + Persp1·y Persp2 を =

その結果、これらの値には、いくつか実際的な制限があります。 

`Persp2`かに、セルに 0 または 0 を指定できます。 場合`Persp2`が 0、z' ポイント (0, 0) のゼロおよびするが、通常、望ましくない点が 2 次元グラフィックスで非常に一般的なためです。 場合`Persp2`場合、汎用性の損失はありませんが、0 と等しくない`Persp2`1 に固定されます。 たとえば、あると判断した場合`Persp2`することだけです、マトリックス内のすべてのセルで除算 5、これにより、5、する必要があります`Persp2`を 1 に等しくなり、結果は同じになります。

これらの理由から、`Persp2`恒等行列の値が同じである、1 に固定されて多くの場合。

一般に、`Persp0`と`Persp1`は少数です。 たとえば、恒等行列が、セットを開始する`Persp0`0.01 に。

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

変換式は次のとおりです。

x` = x / (0.01·x + 1)

y' y =/(0.01·x + 1)

この変換を使用して、原点に配置されている 100 ピクセルの四角形のボックスを表示するようになりました。 4 つの角を変換する方法を次に示します。

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

X が 100 で、次に、z の場合 ' 分母は、2 x および y 座標が半分に効果的にします。 ボックスの右側には、左側にあるよりも短いのようになります。

![](non-affine-images/nonaffinetransform.png "非アフィン変換の対象 ボックス")

`Persp`ビューアーから、右側にあると、ボックスが傾いているようになりましたことを提案する、短縮するために、これらのセルの名前の部分が「パースペクティブ」を指します。

**テスト パースペクティブ** ページでは、実験の値を使用できます。`Persp0`と`Pers1`そのしくみを理解します。 これらのマトリックスのセルの妥当な値が小さいためを`Slider`ユニバーサル Windows プラットフォームで正しく処理できませんに。 UWP の問題を 2 つの対応するために`Slider`内の要素、 [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) 1 – 1 の範囲を初期化する必要があります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
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
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

スライダーのイベント ハンドラー、 [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) –0.01 と 0.01 の範囲がように分離コード ファイルが 100 を値を分割します。 さらに、コンス トラクターは、ビットマップに読み込みます。

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface`ハンドラーの計算、`SKMatrix`という名前の値`perspectiveMatrix`100 で除算これら 2 つのスライダーの値に基づきます。 これと組み合わせると、2 つの変換、ビットマップの中央に、この変換の中心を配置する変換。

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

一部のサンプル イメージを次に示します。

[![](non-affine-images/testperspective-small.png "テスト パースペクティブのページのスクリーン ショットをトリプル")](non-affine-images/testperspective-large.png#lightbox "テスト パースペクティブのページの 3 倍になるスクリーン ショット")

スライダーで実験するとき、–0.0066 以下 0.0066 以外の値が発生するイメージを分割し、統一性がないことになることがあります。 変換されているビットマップは、300 ピクセルの正方形です。 150 –150 からビットマップの座標の範囲であるため、中央を基準と変換されます。 Z の値 ' は。

z' = Persp0·x + Persp1·y + 1

場合`Persp0`または`Persp1`0.0066 より大きいか –0.0066、下が常にいくつかの座標、z で結果のビットマップの ' 0 の値。 0 による除算の原因となると、レンダリング、厄介になります。 0 による除算が発生する座標で何かを表示しないようにする非アフィン変換を使用する場合。

一般を設定しません`Persp0`と`Persp1`分離します。 多くの場合、特定の種類の非アフィン変換を実現するために、マトリックス内の他のセルを設定する必要がもあります。

このような 1 つの非アフィン変換は、*テーパ変換*します。 この種の非アフィン変換では、全体的な四角形の大きさは保持されますが、一方の側が分散します。

![](non-affine-images/tapertransform.png "テーパ変換の対象 ボックス")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs)クラスは、これらのパラメーターに基づいて非アフィン変換の汎用化された計算を実行します。

- 変換されているイメージの四角形のサイズ
- 分散、四角形の側を示す列挙体
- 分散がその方法を示すもう 1 つの列挙体と
- 描の範囲。

次のコードに示します。

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

このクラスが使用されて、**テーパ変換**ページ。 XAML ファイルでは、2 つのインスタンス化します`Picker`列挙の値を選択する要素と`Slider`テーパ分数を選択するためです。 [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55)ハンドラー結合の 2 つのテーパ変換の変換、変換、ビットマップの左上隅に対して相対的に変換します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedItem;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedItem;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

次にいくつかの例を示します。

[![](non-affine-images/tapertransform-small.png "テーパ変換ページのスクリーン ショットをトリプル")](non-affine-images/tapertransform-large.png#lightbox "テーパ変換ページの 3 倍になるスクリーン ショット")

別の種類の汎用化された非アフィン変換は、3 D の回転は、次の記事で示されていますが、 [ **3D 回転**](3d-rotation.md)します。

非アフィン変換では、任意の凸四角形に四角形を変換できます。 これを示します、**非アフィン行列を表示する**ページ。 よく似ていますが、**アフィン行列を表示する**ページから、 [**行列変換**](matrix.md)点が異なりますがある、4 つ目の記事`TouchPoint`オブジェクトを 4 つ目の操作ビットマップの隅。

[![](non-affine-images/shownonaffinematrix-small.png "非アフィン行列を表示するページのスクリーン ショットをトリプル")](non-affine-images/shownonaffinematrix-large.png#lightbox "非アフィン行列を表示するページの 3 倍になるスクリーン ショット")

プログラムが正常にからこのメソッドを使用して変換を計算のビットマップ四隅のいずれかの内部角度する 180 度より大きいか、2 つの辺を互いに交差しようとするはありません、限り、 [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs)クラス。

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
{
    // Scale transform
    SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

    // Affine transform
    SKMatrix A = new SKMatrix
    {
        ScaleX = ptUR.X - ptUL.X,
        SkewY = ptUR.Y - ptUL.Y,
        SkewX = ptLL.X - ptUL.X,
        ScaleY = ptLL.Y - ptUL.Y,
        TransX = ptUL.X,
        TransY = ptUL.Y,
        Persp2 = 1
    };

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

容易にするための計算では、このメソッドは、ここでこれらの変換がビットマップの 4 つの角を変更する方法を示す矢印の付いたされるは 3 つの個別の変換の製品として合計変換を取得します。

(0, 0) (0, 0) の作成] → [(0, 0) の作成] → [→ (0, y0) x (左)

(0、H) → (0, 1) (0, 1) の作成] → [→ (x1, y1) (左)

(W、0) (1, 0) の作成] → [(1, 0) の作成] → [→ (x2, y2) (右)

(W、H) (1, 1) の作成] → [→ (a、b) → (3、y3) x (右下)

右側にある最後の座標は、4 つの 4 つのタッチ ポイントに関連付けられている点です。 これらは、最終的なビットマップの四隅の座標です。

幅と高さ、幅とビットマップの高さを表します。 最初の変換`S`単に 1 ピクセルの正方形にビットマップをスケールします。 2 番目の変換が非アフィン変換`N`、3 番目はアフィン変換`A`します。 先ほどアフィンようでは、アフィン変換が 3 つの点に基づくは[ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68)メソッドと 4 番目の行が関与しないと、(a、b) ポイント。

`a`と`b`3 番目の変換がアフィン変換できるように、値が計算されます。 コードでは、アフィン変換の逆元を取得し、を使用して、右下隅にマップします。 (A、b) のポイントです。

非アフィン変換の別の使用は、3 次元グラフィックスを模倣するためです。 次の記事で[ **3D 回転**](3d-rotation.md) 3D 空間で 2 次元グラフィックを回転させる方法を参照してください。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
