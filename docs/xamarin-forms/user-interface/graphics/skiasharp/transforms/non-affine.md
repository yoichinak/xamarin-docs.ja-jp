---
title: "非アフィン変換"
description: "変換行列の 3 番目の列を持つテーパ効果とパースペクティブを作成します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 2e2e83404bc93bd07885008b868c51eba2ff7140
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="non-affine-transforms"></a>非アフィン変換

_変換行列の 3 番目の列を持つテーパ効果とパースペクティブを作成します。_

平行移動、拡大/縮小、回転、および傾斜すべてに分類されます*アフィン*を変換します。 アフィン変換には、平行線が保持されます。 2 つの行が変換する前に平行場合は、常に並列、変換後にします。 四角形は parallelograms 常に変換されます。

ただし、SkiaSharp も非アフィン変換は、四角形を凸の四角形に変換することができます。

![](non-affine-images/nonaffinetransformexample.png "凸状の四角形に変換されたビットマップ")

凸状の四角形は、内部角度 180 度や互いに交差しない辺よりも常に以下の 4 辺の図です。

非アフィンを変換結果の変換行列の 3 番目の行が 0、0、および 1 以外の値に設定します。 完全`SKMatrix`乗算は。

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

結果の変換の数式は次のとおりです。

x' ScaleX·x + SkewX·y + x 方向を =

y' SkewY·x + ScaleY·y 直線移動を =

z` = Persp0·x + Persp1·y + Persp2

3-3 でマトリックスを使用して、2 次元の変換のための基本的なルールは、ことはすべてそのまま平面上に Z が 1 に等しい場所です。 しない限り、`Persp0`と`Persp1`0 の場合と`Persp2`が 1 と等しい、トランス フォームがその面を Z 座標を移動します。

2 次元の変換には、これを復元するに座標必要がありますに再び移動その平面。 別のステップが必要です。 X'、y'、z 'の値で分割されます z' とします。

x"= x'/z'

y" = y' / z'

z" = z' / z' = 1

これらと呼ばれます*同種座標*Möbius ストリップ年 8 月 Ferdinand Möbius をよりよく知られた彼トポロジー奇妙の数学的によって、開発されたとします。

場合は z' は、0、無限の座標で除算結果。 実際には、有限数で無限の値を表現できるため、同種の座標を開発するための Möbius の動機の 1 つでした。

グラフィックを表示するときを避けたい無限値に変換する座標で何かを表示します。 これらのコーディネートはレンダリングされません。 これらのコーディネートの近くのすべてのものはされ、非常に大きな視覚的に一貫した可能性があります。

この式にたくない z の値 ' 0 になります。

z` = Persp0·x + Persp1·y + Persp2

`Persp2`セルには、0 または 0 ではありませんかを指定できます。 場合`Persp2`が 0、z' (0, 0) のポイントの 0 は、外にある通常望ましいそのポイントが 2 次元グラフィックスのよくあるためです。 場合`Persp2`がない場合は 0 に等しい場合は、ジェネリック クラスの損失は起こりません`Persp2`は 1 に固定します。 たとえば、次のことがわかった`Persp2`単に分割できるマトリックス内のすべてのセル、5、し、5 をする必要があります`Persp2`を 1 に等しいし、同じ結果になります。

これらの理由から、`Persp2`は多くの場合、固定長で 1、単位行列で同じ値であります。

一般に、`Persp0`と`Persp1`は少数です。 たとえばが、行列はセットで開始する`Persp0`0.01 に。

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

変換の数式は次のとおりです。

x` = x / (0.01·x + 1)

y' = = y/(0.01·x + 1)

今すぐ元に配置されている 100 ピクセルの四角形のボックスを表示するためにこの変換を使用します。 4 つの角を変換する方法を次に示します。

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

ときに x が 100、z' 分母は 2、x および y 座標が実質的に半分になります。 ボックスの右側には、左側にあるよりも短いのようになります。

![](non-affine-images/nonaffinetransform.png "非アフィン変換の対象 ボックス")

`Persp`しますが、右側にあるビューアーからかけ離れたものに傾いたようになりましたことを提案する、短縮するために、これらのセルの名前の部分が「パースペクティブ」を指します。

**テスト パースペクティブ** ページでは、値をテストできます。`Persp0`と`Pers1`をどのように動作感を取得します。 マトリックス セルにこれらの妥当な値は非常に小さいこと、`Slider`ユニバーサル Windows プラットフォームで正しく処理できないにします。 UWP 問題を 2 つ、それに合わせて`Slider`内の要素、 [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) 1 – 1 の範囲に初期化する必要があります。

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

スライダーを対応するイベント ハンドラー、 [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs)分離コード ファイルに分けられ、値 100 をよう –0.01 と 0.01 間で、さまざまな種類です。 さらに、コンス トラクターは、ビットマップにロードします。

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

`PaintSurface`ハンドラーを計算する`SKMatrix`という名前の値`perspectiveMatrix`100 で割った値これら 2 つのスライダーの値に基づいて。 組み合わせてこのビットマップの中央にこの変換の中心に配置した変換が 2 つの変換します。

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

一部の画像をサンプルを次に示します。

[![](non-affine-images/testperspective-small.png "テスト パースペクティブのページのスクリーン ショットをトリプル")](non-affine-images/testperspective-large.png#lightbox "テスト パースペクティブのページのトリプル スクリーン ショット")

スライダーを試すとき 0.0066 を超えてまたは –0.0066 以下の値が発生するイメージを分割し、一貫性のないことになることがあります。 変換されているビットマップは、300 ピクセルの四角形です。 ビットマップの座標の範囲は –150 150 ~ ようにその中心を基準とした変換されます。 取り消しを z の値 ' は。

z` = Persp0·x + Persp1·y + 1

場合`Persp0`または`Persp1`0.0066 より大きい –0.0066、下は常に、z で結果のビットマップのいくつかの座標または ' 値は 0 です。 0 による除算の原因となると、レンダリング乱雑になります。 0 による除算が発生する座標のすべてのものを表示しないようにする非アフィン変換を使用する場合。

一般を設定しない`Persp0`と`Persp1`分離します。 多くの場合、特定の種類の非アフィン変換を実現するために、マトリックス内の他のセルを設定するために必要です。

このような 1 つの非アフィン変換とは、*テーパ変換*です。 この種の非アフィン変換では、四角形の全体的な寸法は保持されますが、一方の側が分散します。

![](non-affine-images/tapertransform.png "対象となるテーパ変換ボックス")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs)クラスは、これらのパラメーターに基づいて非アフィン変換の一般的な計算を実行します。

- 変換されているイメージの四角形のサイズ
- 分散する四角形の側を示す列挙体
- 分散がその方法を示す他の列挙と
- 描の範囲。

コードを次に示します。

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

このクラスが使用されて、**テーパ変換**ページ。 XAML ファイルの 2 つのインスタンスを作成`Picker`列挙値を選択する要素と`Slider`のテーパ分数を選択します。 [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55)ハンドラーを組み合わせたもので 2 つのテーパ変換の変換、ビットマップの左上隅に対する相対変換を作成するための変換。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
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

[![](non-affine-images/tapertransform-small.png "トリプル ページのスクリーン ショット、テーパ変換")](non-affine-images/tapertransform-large.png#lightbox "トリプル ページのスクリーン ショット、テーパ変換")

汎用化された非アフィン変換の別の型は次の記事で示したようには、3D 回転[3D 回転](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)です。

非アフィン変換では、任意の凸四角形に四角形を変換できます。 示されるこれは、**表示の非アフィン行列**ページ。 非常に似ています、**表示のアフィン行列**ページから、[行列変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)記事がある 4 つ目のする点を除いて`TouchPoint`ビットマップの 4 番目の隅を操作するオブジェクト。

[![](non-affine-images/shownonaffinematrix-small.png "非アフィン行列の表示 ページのスクリーン ショットをトリプル")](non-affine-images/shownonaffinematrix-large.png#lightbox "非アフィン行列の表示 ページのトリプル スクリーン ショット")

ビットマップの四隅のいずれかの内部角度を 180 度よりも大きい値に加えるか互いに交差する 2 つの辺を加えるしようとしていない、限り正常に計算からこのメソッドを使用して変換を[ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs)クラス。

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

容易にするための計算は、このメソッドは、これらの変換ファイルが、ビットマップの四隅を変更する方法を示す矢印の付いたここされるは、3 つの個別の変換の積として合計変換を取得します。

(0, 0) (0, 0) → → (0, 0) → (0, y0) x (左)

(0、H) → (0, 1) (0, 1) → → (x1, y1) (左下)

(W、0) (1, 0) → → (1, 0) → (x2, y2) (右)

(W, H) (1, 1) → → (a、b) → (3、y3) x (右下)

右側にある最後の座標は、次の 4 つのタッチ ポイントに関連付けられている 4 つのポイントです。 これらは、最終的なビットマップの角の座標です。

幅と高さは、ビットマップの高さと幅を表します。 最初の変換 (`S`) だけで 1 ピクセルの四角形にビットマップのスケールを設定します。 2 番目の変換が非アフィン変換`N`、3 番目はアフィン変換`A`です。 アフィン変換は次の 3 つ、ポイントに基づいて以前アフィンようでは、 [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68)メソッドと 4 番目の行が含まれる、(a、b) ポイント。

`a`と`b`値が 3 番目の変換がアフィンになるように計算されます。 コードでは、アフィン変換の逆を取得し、右下隅のマップを使用しています。 ポイント (a、b) です。

非アフィン変換の別の使用は、3 次元グラフィックスを模倣するためにです。 次の記事で[3D 回転](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)3D スペースで 2 次元グラフィックを回転させる方法を参照してください。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
