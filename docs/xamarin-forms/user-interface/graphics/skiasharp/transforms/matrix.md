---
title: "SkiaSharp のマトリックス変換" の説明: "この記事では、多用途変換行列を使用した SkiaSharp 変換について詳しく説明し、サンプルコードを使用してこれを示します。"
ms. 製品: xamarin skiasharp: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4 author: davidbritch ms. author: dabritch ms. date: 04/12/2017 no loc: [ Xamarin.Forms ,]」を実行します。 Xamarin.Essentials
---

# <a name="matrix-transforms-in-skiasharp"></a>SkiaSharp のマトリックス変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_汎用性のある変換マトリックスを使用した SkiaSharp 変換について詳しく知る_

オブジェクトに適用されたすべての変換 `SKCanvas` は、構造体の1つのインスタンスに統合され [`SKMatrix`](xref:SkiaSharp.SKMatrix) ます。 これは、すべての最新の2D グラフィックスシステムの場合と同様に、標準の3対3変換行列です。

既に説明したように、変換マトリックスについて理解していなくても、SkiaSharp で変換を使用できますが、変換行列は理論的な観点から重要であり、変換を使用してパスを変更したり複雑なタッチ入力を処理したりするときは、この記事で説明されているようにしてください。

![](matrix-images/matrixtransformexample.png "A bitmap subjected to an affine transform")

に適用されている現在の変換行列 `SKCanvas` は、読み取り専用プロパティにアクセスすることによっていつでも使用でき [`TotalMatrix`](xref:SkiaSharp.SKCanvas.TotalMatrix) ます。 メソッドを使用して新しい変換行列を設定でき [`SetMatrix`](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix)) ます。を呼び出すことによって、変換行列を既定値に戻すことができ [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) ます。

`SKCanvas`キャンバスの行列変換を直接操作する唯一のメンバーは、 [`Concat`](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@)) 2 つの行列を乗算することで連結します。

既定の変換行列は、id 行列であり、対角線のセルには1が、それ以外の場合は0で構成されます。

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

静的メソッドを使用して、id 行列を作成でき [`SKMatrix.MakeIdentity`](xref:SkiaSharp.SKMatrix.MakeIdentity) ます。

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

既定のコンストラクターは、 `SKMatrix` id 行列を返し*ません*。 すべてのセルが0に設定されたマトリックスが返されます。 `SKMatrix`これらのセルを手動で設定する予定がない限り、コンストラクターは使用しないでください。

SkiaSharp がグラフィカルオブジェクトをレンダリングするとき、各点 (x、y) は、次の3番目の列の1を含む1対3の行列に効果的に変換されます。

<pre>
| x  y  1 |
</pre>

この1対3の行列は、Z 座標が1に設定された3次元の点を表します。 2次元の行列変換が3つの次元で動作する必要がある理由は、後で説明します。 この1対3の行列は、3D 座標系の点を表していると考えることができますが、常に Z が1と等しい2D 平面上にあります。

この1対3の行列は変換行列で乗算され、結果はキャンバスにレンダリングされる点になります。

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

標準の行列乗算を使用する場合、変換されたポイントは次のようになります。

`x' = x`

`y' = y`

`z' = 1`

これが既定の変換です。

`Translate`オブジェクトでメソッドが呼び出されると、 `SKCanvas` `tx` `ty` メソッドの引数と引数が、 `Translate` 変換行列の3番目の行の最初の2つのセルになります。

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

乗算は次のようになります。

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

変換式を次に示します。

`x' = x + tx`

`y' = y + ty`

スケールファクターの既定値は1です。 新しいオブジェクトに対してメソッドを呼び出すと、結果として得た `Scale` `SKCanvas` 変換行列に、 `sx` 斜めのセルの引数と引数が含まれ `sy` ます。

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

変換式は次のとおりです。

`x' = sx · x`

`y' = sy · y`

を呼び出した後の変換行列には、 `Skew` スケールファクターに隣接するマトリックスセル内の2つの引数が含まれています。

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

変換式は次のとおりです。

`x' = x + xSkew · y`

`y' = ySkew · x + y`

`RotateDegrees`Αの角度に対するまたはの呼び出しの場合 `RotateRadians` 、変換行列は次のようになります。

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

変換式を次に示します。

`x' = cos(α) · x - sin(α) · y`

`y' = sin(α) · x - cos(α) · y`

Αが0度の場合は、id 行列になります。 Αが180°の場合、変換行列は次のようになります。

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180度の回転は、オブジェクトを水平方向および垂直方向に反転することに相当します。これは、-1 のスケールファクターを設定することによっても実現されます。

これらのすべての種類の変換は、*アフィン*変換として分類されます。 アフィン変換には、マトリックスの3番目の列は含まれません。これは、既定値の0、0、および1のままです。 非[**アフィン変換**](non-affine.md)については、非アフィン変換に関する記事をご覧ください。

## <a name="matrix-multiplication"></a>行列乗算

変換行列を使用する場合の大きな利点の1つは、複合変換を行列乗算によって取得できることです。これは、SkiaSharp ドキュメントで*連結*と呼ばれることがよくあります。 の変換関連のメソッドの多く `SKCanvas` は、"プリ連結" または "事前連結" を指しています。 これは乗算の順序を表します。これは、行列乗算が可換ではないために重要です。

たとえば、メソッドのドキュメントでは、 [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) "現在のマトリックスを指定された翻訳に事前に concats しています" という説明がありますが、メソッドのドキュメントでは、 [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) "現在のマトリックスを指定されたスケールで事前に concats する" ことが示されています。

これは、メソッド呼び出しで指定された変換が乗数 (左側のオペランド) であり、現在の変換行列が被乗数 (右側のオペランド) であることを意味します。

の `Translate` 後にが呼び出されるとし `Scale` ます。

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

変換は、 `Scale` `Translate` 複合変換行列の変換によって乗算されます。

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale`次のように、を呼び出すことができ `Translate` ます。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

その場合は、乗算の順序が逆になり、スケーリングファクターが変換係数に効果的に適用されます。

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

`Scale`ピボットポイントを使用するメソッドを次に示します。

```csharp
canvas.Scale(sx, sy, px, py);
```

これは、次の変換とスケールの呼び出しに相当します。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

3つの変換行列は、メソッドがコードにどのように表示されるかを逆の順序で乗算します。

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>SKMatrix 構造体

`SKMatrix`構造体は、 `float` 変換行列の9つのセルに対応する型の9つの読み取り/書き込みプロパティを定義します。

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix`では、型のという名前のプロパティも定義さ [`Values`](xref:SkiaSharp.SKMatrix.Values) `float[]` れています。 このプロパティを使用して、1回のショットで、、、、、、、、およびの順に9つの値を設定または取得でき `ScaleX` `SkewX` `TransX` `SkewY` `ScaleY` `TransY` `Persp0` `Persp1` `Persp2` ます。

、、およびの各セルについては、 `Persp0` `Persp1` `Persp2` 「[**非アフィン変換**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)」をご覧ください。 これらのセルの既定値が0、0、および1の場合、変換には次のような座標点が乗算されます。

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

`x' = ScaleX · x + SkewX · y + TransX`

`y' = SkewX · x + ScaleY · y + TransY`

`z' = 1`

これは、完全な2次元のアフィン変換です。 アフィン変換は、平行線を保持します。これは、四角形が平行四辺形以外に変換されないことを意味します。

`SKMatrix`構造体は、値を作成するためのいくつかの静的メソッドを定義し `SKMatrix` ます。 これらのすべての戻り `SKMatrix` 値:

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single))ピボットポイントを使用する
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single))ラジアン単位の角度の場合
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single))ピボットポイントを使用したラジアンの角度の場合
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single))ピボットポイントを使用する
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix`では、2つの行列を連結するいくつかの静的メソッドも定義されています。 これらのメソッドには、、、およびという名前が付けられ、 [`Concat`](xref:SkiaSharp.SKMatrix.Concat*) [`PostConcat`](xref:SkiaSharp.SKMatrix.PostConcat*) それぞれに [`PreConcat`](xref:SkiaSharp.SKMatrix.PreConcat*) 2 つのバージョンがあります。 これらのメソッドには戻り値がありません。代わりに、引数を使用して既存の値を参照し `SKMatrix` `ref` ます。 次の例では、、 `A` `B` 、および `R` ("result" の場合) がすべての `SKMatrix` 値です。

この2つの `Concat` メソッドは次のように呼び出されます。

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

これらは次の乗算を実行します。

`R = B × A`

他のメソッドにはパラメーターが2つだけあります。 最初のパラメーターが変更され、メソッド呼び出しから戻ったときに、2つの行列の積が格納されます。 この2つの `PostConcat` メソッドは次のように呼び出されます。

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

これらの呼び出しは、次の操作を実行します。

`A = A × B`

2つの `PreConcat` 方法は似ています。

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

これらの呼び出しは、次の操作を実行します。

`A = B × A`

これらのメソッドのうち、すべての引数を持つバージョンは、 `ref` 基になる実装を呼び出すと少し効率的になりますが、コードを読み取っているユーザーが混乱し、引数を持つもの `ref` がメソッドによって変更されることを前提としています。 さらに、メソッドの1つの結果である引数を渡すと便利な場合がよくあり `Make` ます。次に例を示します。

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

これにより、次のマトリックスが作成されます。

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

これは、変換変換によって乗算されたスケール変換です。 この場合、構造体には、 `SKMatrix` という名前のメソッドを含むショートカットが用意されてい [`SetScaleTranslate`](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)) ます。

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

これは、コンストラクターを安全に使用するための、数回のうちの1つです `SKMatrix` 。 メソッドは、 `SetScaleTranslate` マトリックスの9つのセルをすべて設定します。 また、静的メソッドとメソッドを使用してコンストラクターを安全に使用することもでき `SKMatrix` `Rotate` `RotateDegrees` ます。

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

これらのメソッドは、回転変換を既存の変換に連結し*ません*。 これらのメソッドは、マトリックスのすべてのセルを設定します。 これらのメソッドは、 `MakeRotation` 値を `MakeRotationDegrees` インスタンス化しない点を除いて、およびメソッドと機能的には同じです `SKMatrix` 。

表示するオブジェクトがあるとし `SKPath` ますが、別の向き、または別の中心点を使用することをお勧めします。 引数を指定してのメソッドを呼び出すことにより、そのパスのすべての座標を変更でき [`Transform`](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix)) `SKPath` `SKMatrix` ます。 [**パスの変換**] ページでは、これを行う方法を示しています。 [`PathTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs)クラスはフィールド内のオブジェクトを参照し `HendecagramPath` ますが、そのコンストラクターを使用して、そのパスに変換を適用します。

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath`オブジェクトの中心は (0, 0) で、星の11点は、その中心から100単位までのすべての方向に拡張されます。 これは、パスに正と負の両方の座標があることを意味します。 [**パスの変換**] ページでは、星の3倍の大きさと、すべての正の座標が使用されます。 さらに、星の1つのポイントをまっすぐポイントさせる必要はありません。 代わりに、星の1つのポイントが真下を指すようにしたいと考えています。 (星が11点であるため、両方を持つことはできません)。これには、星を22で割った360°の回転が必要です。

コンストラクターは、 `SKMatrix` メソッドを使用して、3つの個別の変換からオブジェクトを構築し `PostConcat` ます。このパターンでは、a、B、C はのインスタンスです `SKMatrix` 。

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

これは連続した一連の乗算であるため、結果は次のようになります。

`A × B × C`

連続する乗算は、各変換の動作を理解するのに役立ちます。 スケール変換は、パス座標のサイズを3倍に増やします。そのため、座標の範囲は– 300 ~ 300 です。 回転変換は、原点を中心として回転します。 その後、平行移動変換によって、この値は300ピクセル右と下にシフトされるため、すべての座標が正の値になります。

同じマトリックスを生成する他のシーケンスがあります。 もう1つの例を次に示します。

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

これにより、最初に中心を中心としたパスが回転し、次に100ピクセルが右と下に変換され、すべての座標が正になります。 次に、星のサイズが新しい左上隅 (ポイント (0, 0)) に対して大きくなります。

ハンドラーは、次の `PaintSurface` パスを単にレンダリングできます。

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

キャンバスの左上隅に表示されます。

[![](matrix-images/pathtransform-small.png "Triple screenshot of the Path Transform page")](matrix-images/pathtransform-large.png#lightbox "Triple screenshot of the Path Transform page")

このプログラムのコンストラクターは、次の呼び出しを使用して、マトリックスをパスに適用します。

```csharp
transformedPath.Transform(matrix);
```

このパスは、このマトリックスをプロパティとして保持し*ません*。 代わりに、パスのすべての座標に変換が適用されます。 が再度呼び出された場合は、 `Transform` 変換が再度適用され、元に戻すことができるのは、変換を元に戻す別の行列を適用することだけです。 さいわい、 `SKMatrix` 構造体は、 [`TryInvert`](xref:SkiaSharp.SKMatrix.TryInvert*) 指定された行列を反転する行列を取得するメソッドを定義します。

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

`TryInverse`すべてのマトリックスが反転できるされているわけではありませんが、反転できる行列がグラフィックス変換に使用されない可能性があるため、メソッドが呼び出されました。

また、マトリックス変換を `SKPoint` 値、点の配列、 `SKRect` またはプログラム内の単なる数値に適用することもできます。 構造体は、次のように、と `SKMatrix` いう単語で始まるメソッドのコレクションを使用して、これらの操作をサポートし `Map` ます。

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

その最後のメソッドを使用する場合は、この `SKRect` 構造体が回転した四角形を表すことができないことに注意してください。 メソッドは、 `SKMatrix` 変換とスケーリングを表す値に対してのみ意味を持ちます。

## <a name="interactive-experimentation"></a>対話型実験

アフィン変換の感覚を得る方法の1つとして、画面の周りでビットマップの3つのコーナーを対話形式で移動し、変換結果を確認する方法があります。 これは、[**アフィン行列の表示**] ページの背後にある概念です。 このページでは、他のデモンストレーションでも使用されている2つのクラスが必要です。

クラスには、 [`TouchPoint`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) 画面の周囲にドラッグできる半透明な円が表示されます。 `TouchPoint`またはの親である要素にがアタッチされている必要があり `SKCanvasView` `SKCanvasView` [`TouchEffect`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) ます。 `Capture` プロパティを `true`に設定します。 `TouchAction`イベントハンドラーでは、プログラムは `ProcessTouchEvent` `TouchPoint` 各インスタンスに対してメソッドを呼び出す必要があり `TouchPoint` ます。 `true`タッチイベントによってタッチポイントが移動された場合、メソッドはを返します。 また、ハンドラーは、 `PaintSurface` `Paint` 各インスタンスでメソッドを呼び出し `TouchPoint` て、オブジェクトに渡す必要があり `SKCanvas` ます。

`TouchPoint`SkiaSharp ビジュアルを別のクラスにカプセル化する一般的な方法を示します。 クラスでは、ビジュアルの特性を指定するためのプロパティを定義できます。また、という名前のメソッドが `Paint` 引数を使用し `SKCanvas` てレンダリングできます。

`Center`のプロパティは、 `TouchPoint` オブジェクトの場所を示します。 このプロパティは、場所を初期化するように設定できます。プロパティは、ユーザーがキャンバスの周りに円をドラッグしたときに変更されます。

[**アフィン行列の表示] ページ**には、クラスも必要です [`MatrixDisplay`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) 。 このクラスは、オブジェクトのセルを表示 `SKMatrix` します。 2つのパブリックメソッドがあります。表示さ `Measure` れる行列の次元を取得し、 `Paint` それを表示します。 クラスには、 `MatrixPaint` `SKPaint` 別のフォントサイズまたは色で置き換えることができる型のプロパティが含まれています。

[**ShowAffineMatrixPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml)ファイルは、をインスタンス化 `SKCanvasView` し、をアタッチし `TouchEffect` ます。 [**ShowAffineMatrixPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs)分離コードファイルは、3つのオブジェクトを作成し、 `TouchPoint` 埋め込みリソースから読み込むビットマップの3つのコーナーに対応する位置に設定します。

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

アフィン行列は、3つの点によって一意に定義されます。 3つのオブジェクトは、 `TouchPoint` ビットマップの左上、右上、左下の各隅に対応します。 アフィン行列は四角形を平行四辺形に変換できるだけなので、4番目の点は他の3つの要素によって暗黙的に示されます。 コンストラクターは `ComputeMatrix` 、 `SKMatrix` この3つの点からオブジェクトのセルを計算するの呼び出しで終了します。

ハンドラーは、 `TouchAction` `ProcessTouchEvent` 各のメソッドを呼び出し `TouchPoint` ます。 `scale`値は座標から Xamarin.Forms ピクセルに変換されます。

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

いずれかが移動された場合 `TouchPoint` 、メソッドはを再度呼び出し、 `ComputeMatrix` サーフェイスを無効にします。

メソッドは、 `ComputeMatrix` 3 つの点によって示される行列を決定します。 という行列は、3つの `A` 点に基づいて1ピクセルの正方形の四角形を平行四辺形に変換します。一方、呼び出されたスケール変換は、 `S` ビットマップを1ピクセルの四角形の四角形にスケーリングします。 複合行列は、 `S` 次のようになり `A` ます。

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
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

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

最後に、このメソッドは、 `PaintSurface` そのマトリックスに基づいてビットマップをレンダリングし、画面の下部にマトリックスを表示し、ビットマップの3つの隅にタッチポイントをレンダリングします。

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

次の iOS 画面は、ページが最初に読み込まれたときのビットマップを示しています。また、操作の後に、他の2つの画面が表示されます。

[![](matrix-images/showaffinematrix-small.png "Triple screenshot of the Show Affine Matrix page")](matrix-images/showaffinematrix-large.png#lightbox "Triple screenshot of the Show Affine Matrix page")

タッチポイントはビットマップの角をドラッグするように見えますが、これは錯覚です。 タッチポイントから計算された行列は、角がタッチポイントと重なるようにビットマップを変換します。

ユーザーは、角をドラッグするのではなく、ビットマップの移動、サイズ変更、および回転を行うことができますが、オブジェクト上で1つまたは2本の指を使用してドラッグ、ピンチ、回転することができます。 これについては、次の「[**タッチ操作**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)」で説明されています。

## <a name="the-reason-for-the-3-by-3-matrix"></a>3-3 の行列の理由

2次元のグラフィックスシステムでは2つの変換行列が必要になることが予想される場合があります。

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

これは、スケーリング、回転、および傾斜にも使用できますが、変換の最も基本的な変換ではありません。

問題は、2バイバイ2行列が2次元の*線形*変換を表すことです。 線形変換ではいくつかの基本的な算術演算が保持されますが、影響の1つは、線形変換が点 (0, 0) を変更しないことです。 線形変換を使用すると、変換できません。

3次元では、線形変換行列は次のようになります。

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

というラベルのセルは、 `SkewXY` 値が Y の値に基づいて x 座標を傾斜させることを意味します。セルは、 `SkewXZ` 値が Z の値に基づいて x 座標を傾斜させることを意味し、値は他のセルと同様に傾斜し `Skew` ます。

この3D 変換行列を2次元平面に制限するには、とを0に、を1に設定し `SkewZX` `SkewZY` `ScaleZ` ます。

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

2次元のグラフィックスが3D 空間内の平面全体に描画される場合 (Z が1の場合)、変換乗算は次のようになります。

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Z が1と等しい2次元平面にとどまりますが、との `SkewXZ` 各 `SkewYZ` セルは効果的に2次元の平行移動因子になります。

これは、3次元線形変換が2次元非線形変換として機能する方法です。 (たとえると、3D グラフィックスの変換は4×4の行列に基づいています)。

`SKMatrix`SkiaSharp の構造体は、3番目の行のプロパティを定義します。

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

およびの0以外の値は `Persp0` 、 `Persp1` Z が1の2次元平面からオブジェクトを移動する変換になります。 これらのオブジェクトがその平面に戻されるとどうなるかは、[**非アフィン変換**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)に関する記事で説明されています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
