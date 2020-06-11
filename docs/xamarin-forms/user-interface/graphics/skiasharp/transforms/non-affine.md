---
title: "非アフィン変換" の説明: "この記事では、変換行列の3番目の列を使用してパースペクティブとテーパ効果を作成する方法について説明し、サンプルコードを使用してこれを示します。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 785F4D13-7430-492E-B24E-3B45C560E9F1 author: davidbritch dabritch: ms. date: 04/14/2017 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="non-affine-transforms"></a>非アフィン変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_変換行列の3番目の列を使用して、パースペクティブとテーパ効果を作成する_

平行移動、拡大縮小、回転、および傾斜は、すべて*アフィン*変換として分類されます。 アフィン変換は、平行線を保持します。 変換の前に2つの行が並列である場合、変換後も並列のままになります。 四角形は常に parallelograms に変換されます。

ただし、SkiaSharp は、四角形を任意の凸形に変換する機能を持つ非アフィン変換にも対応しています。

![](non-affine-images/nonaffinetransformexample.png "A bitmap transformed into a convex quadrilateral")

凸形の四角形は、内側の角度が常に180度未満であり、互いに交差していない辺を持つ4つの辺の図です。

変換行列の3番目の行が0、0、および1以外の値に設定されている場合、非アフィン変換の結果になります。 完全な `SKMatrix` 乗算は次のとおりです。

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

変換の結果の数式は次のとおりです。

x ' = ScaleX · x + SkewX · y + TransX

y ' = SkewY · x + ScaleY · y + TransY

z ' = Persp0 · x + Persp1 · y + Persp2

2次元変換に 3 x 3 の行列を使用する基本的な規則は、Z が1と等しい平面上にすべて残っているということです。 `Persp0`とが0の場合、 `Persp1` およびが `Persp2` 1 の場合を除き、変換は Z 座標をその平面から移動しました。

これを2次元変換に復元するには、座標をその平面に戻す必要があります。 もう1つの手順が必要です。 X '、y '、および z ' の値は、z ' で割る必要があります。

x "= x '/z '

y "= y '/z '

z "= z '/z ' = 1

これらは同種の*座標*として知られており、Möbius 8 月の mathematician によって開発され、Möbius ストリップのトポロジ遭遇に関してはるかによく知られています。

Z ' が0の場合、除算は無限の座標になります。 現実には、同種の座標を開発するための Möbius's 動機の1つは、有限の数値で無限値を表す機能でした。

ただし、グラフィックスを表示する場合は、無限値に変換する座標を使用して何もレンダリングしないようにする必要があります。 これらの座標はレンダリングされません。 これらの座標の近くにあるすべてのものは非常に大きくなり、見た目が統一されていない可能性があります。

この式では、z の値がゼロになることはありません。

z ' = Persp0 · x + Persp1 · y + Persp2

そのため、これらの値には実用的な制限があります。 

`Persp2`セルは0または0以外の値にすることができます。 `Persp2`が0の場合、ポイント (0, 0) に対して z ' が0になります。これは通常、2次元のグラフィックスでは非常に一般的であるため、推奨されません。 `Persp2`が0以外の場合、 `Persp2` が1で固定されている場合、一般性は失われません。 たとえば、5にする必要があると判断した場合は、 `Persp2` 単にマトリックス内のすべてのセルを5で割ることができ `Persp2` ます。これは1と等しく、結果は同じになります。

これらの理由から、は多くの場合、 `Persp2` id 行列の同じ値である1で修正されます。

通常、 `Persp0` と `Persp1` は小さい数値です。 たとえば、id 行列で始まるが0.01 に設定されているとし `Persp0` ます。

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

変換式は次のとおりです。

x ' = x/(0.01 x + 1)

y ' = y/(0.01 x + 1)

ここで、この変換を使用して、原点に位置する100ピクセルの四角形を描画します。 4つの角の変換方法を次に示します。

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100、0) → (50、0)

(100、100) → (50、50)

X が100の場合、z の分母は2であるため、x 座標と y 座標は実質的に半分になります。 ボックスの右側が左側よりも短くなっています。

![](non-affine-images/nonaffinetransform.png "A box subjected to a non-affine transform")

`Persp`これらのセル名の一部は "パースペクティブ" を意味します。これは、縮み率によって、箱がビューアーからさらに右側に傾いていることがわかるからです。

[**テストパースペクティブ**] ページを使用すると、およびの値を試して、その動作を確認でき `Persp0` `Pers1` ます。 これらのマトリックスセルの適切な値は小さいため、ユニバーサル Windows プラットフォーム内のでは `Slider` 適切に処理できません。 UWP の問題に対応するには、 `Slider` [**testperspective**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml)の2つの要素を-1 ~ 1 の範囲に初期化する必要があります。

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

分離コードファイルのスライダーのイベントハンドラーは、 [`TestPerspectivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) 値を100で除算して、値が–0.01 から0.01 の範囲になるようにします。 さらに、コンストラクターはビットマップに読み込まれます。

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

`PaintSurface`このハンドラーは、 `SKMatrix` `perspectiveMatrix` これら2つのスライダーの値を100で割った値に基づいて、という名前の値を計算します。 これは、この変換の中心をビットマップの中央に配置する2つの平行移動変換と組み合わされています。

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

いくつかのサンプルイメージを次に示します。

[![](non-affine-images/testperspective-small.png "Triple screenshot of the Test Perspective page")](non-affine-images/testperspective-large.png#lightbox "Triple screenshot of the Test Perspective page")

スライダーを試してみると、0.0066 以上の範囲の値が0.0066 を超えていることがわかります。これにより、イメージが突然フラクチャされ統一性になります。 変換されるビットマップは300ピクセルの四角形です。 この値は、中心を基準として変換されます。したがって、ビットマップの座標は-150 ~ 150 の範囲になります。 Z の値が次のようになっていることを思い出してください。

z ' = Persp0 · x + Persp1 · y + 1

`Persp0`または `Persp1` が0.0066 以上–0.0066 より大きい場合は、常に z ' 値が0になるビットマップの座標があります。 0による除算が発生し、レンダリングが混乱になります。 非アフィン変換を使用する場合は、0による除算を発生させる座標を使用して何もレンダリングしないようにします。

通常、 `Persp0` とを分離して設定することはありません `Persp1` 。 また、場合によっては、特定の種類の非アフィン変換を実現するために、マトリックス内の他のセルも設定する必要があります。

このような非アフィン変換の1つは、*テーパ変換*です。 この種類の非アフィン変換は、四角形の全体の次元を保持しますが、1辺を tapers ます。

![](non-affine-images/tapertransform.png "A box subjected to a taper transform")

クラスは、 [`TaperTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) これらのパラメーターに基づいて、非アフィン変換の一般化された計算を実行します。

- 変換されるイメージの四角形のサイズ。
- tapers する四角形の辺を示す列挙体。
- tapers 方法を示す別の列挙体
- tapering の範囲。

コードは次のとおりです。

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

このクラスは、[**テーパ変換**] ページで使用されます。 XAML ファイルは、 `Picker` 列挙値を選択する2つの要素と、テーパの割合を選択するためのをインスタンス化し `Slider` ます。 この [`PaintSurface`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) ハンドラーは、テーパ変換と2つの平行移動変換を組み合わせて、ビットマップの左上隅を基準として変換を行います。

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

次に例をいくつか示します。

[![](non-affine-images/tapertransform-small.png "Triple screenshot of the Taper Transform page")](non-affine-images/tapertransform-large.png#lightbox "Triple screenshot of the Taper Transform page")

次の記事「 [**3d**](3d-rotation.md)の回転」で説明されているように、一般的な非アフィン変換の別の型は3d ローテーションです。

非アフィン変換は、四角形を任意の凸形に変換できます。 これは、[**非アフィン行列の表示**] ページで示されています。 これは、ビットマップの4番目の隅を操作する4番目のオブジェクトがある点を除いて、[**マトリックス変換**](matrix.md)の記事の [**アフィン行列の表示**] ページとよく似てい `TouchPoint` ます。

[![](non-affine-images/shownonaffinematrix-small.png "Triple screenshot of the Show Non-Affine Matrix page")](non-affine-images/shownonaffinematrix-large.png#lightbox "Triple screenshot of the Show Non-Affine Matrix page")

ビットマップのいずれかのコーナーの内部角度を180度より大きくしようとしたり、2つの辺を相互に交差させようとしたりしない限り、プログラムはクラスからこのメソッドを使用して変換を正常に計算し [`ShowNonAffineMatrixPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) ます。

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

計算を簡単にするために、このメソッドは3つの個別の変換の積として合計変換を取得します。これは、これらの変換によってビットマップの4つの角がどのように変更されるかを示す矢印が付いています。

(0, 0) → (0, 0) → (0, 0) → (x0, y0) (左上)

(0, H) → (0, 1) → (0, 1) → (x1, y1) (左下)

(W, 0) → (1, 0) → (1, 0) → (x2, y2) (右上)

(W, H) → (1, 1) → (a, b) → (x3, y3) (右下)

右側の最後の座標は、4つのタッチポイントに関連付けられている4つの点です。 これらは、ビットマップのコーナーの最終的な座標です。

W と H は、ビットマップの幅と高さを表します。 最初の変換では、 `S` 単にビットマップを1ピクセルの正方形にスケーリングします。 2番目の変換は非アフィン変換で、 `N` 3 番目の変換はアフィン変換 `A` です。 このアフィン変換は3つの点に基づいているため、以前のアフィンメソッドと同じように、 [`ComputeMatrix`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) (a, b) ポイントを含む4番目の行は含まれません。

`a`との `b` 値は、3番目の変換がアフィンになるように計算されます。 このコードは、アフィン変換の逆のを取得し、それを使用して右下隅をマップします。 これはポイント (a, b) です。

非アフィン変換のもう1つの用途は、3次元グラフィックスを模倣することです。 次の記事「 [**3D 回転**](3d-rotation.md)」では、3d 空間で2次元のグラフィックを回転させる方法を説明しています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
