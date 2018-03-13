---
title: "行列変換"
description: "汎用性の変換行列と SkiaSharp 変換をさらに深く"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 9d5e65abe675ded48e9239f2cd10ceed4a7c3a52
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="matrix-transforms"></a>行列変換

_汎用性の変換行列と SkiaSharp 変換をさらに深く_

適用されたすべての変換、`SKCanvas`オブジェクトは、単一のインスタンスで統合されて、 [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/)構造体。 これは、標準的な 3-3 での変換行列の最新の 2 次元グラフィックスのすべてのシステムと同様です。

、既に説明したよう変換で使える SkiaSharp、マトリックスが変換行列は理論上の観点から重要なと変換を使用してパスを変更することが重要な変換について知ることのない、または両方の複雑なタッチ入力を処理するためこの記事で、次へは、これが示されています。

![](matrix-images/matrixtransformexample.png "対象となるアフィン変換では、ビットマップ")

現在の変換行列に適用される、`SKCanvas`はいつでも、読み取り専用にアクセスして[ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/)プロパティです。 新しい変換行列を使用して、設定することができます、 [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/)メソッド、ことができますを復元して、変換行列の既定値を呼び出して[ `ResetMatrix`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/)です。

唯一の他の`SKCanvas`キャンバスの行列変換と直接連動するメンバーは[ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/)一緒に乗算することによって 2 つの行列を連結します。

既定の変換行列は単位行列であり、1 のセル内の斜め 0 それ以外の場所で構成されます。

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

静的なを使用する単位行列を作成する[ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/)メソッド。

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix`既定のコンス トラクターは*いない*単位行列を返します。 すべてゼロに設定するセルの行列を返します。 使用しないで、`SKMatrix`コンス トラクターを手動でそれらのセルを設定する予定がない限りです。

SkiaSharp は、グラフィカル オブジェクトをレンダリングするとき (x, y) の各ポイントは効果的に 3 番目の列では 1、1 ~ 3 でマトリックスに変換されます。

<pre>
| x  y  1 |
</pre>

この 1-3 でマトリックスは、Z 座標を 1 にセットを 3 次元時点を表します。 数学上の理由から (後述) があるため、2 次元行列変換では、3 つのディメンションでの作業が必要です。 位置 1 に等しい Z は内の 3D の座標システムでは、常に 2D 平面のポイントを表すものとしてこの 1-3 でマトリックスを検討することができます。

この 1-3 でマトリックスが変換行列で乗算し、結果は、キャンバス上にレンダリングされるポイント。

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

標準の行列の乗算を使用して、変換されたポイントとおりです。

x' = x

y' = y

z' = 1

既定の変換です。

ときに、`Translate`でメソッドが呼び出さ、`SKCanvas`オブジェクト、`tx`と`ty`への引数、`Translate`メソッドが変換行列の 3 番目の行の最初の 2 つのセルになります。

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

乗算のとおりですようになりました。

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

変換の数式を次に示します。

x' = x + テキサス州

y' = y + ty

スケール ファクターは、1 の既定値を設定します。 呼び出すと、`Scale`上の新しいメソッド`SKCanvas`オブジェクト、結果の変換行列に含まれています、`sx`と`sy`対角線方向のセル内の引数。

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

変換の数式は次のとおりです。

x' sx · を =x

y' sy · を =y

呼び出し後の変換行列`Skew`スケール ファクターの横にあるマトリックス セルに 2 つの引数が含まれています。

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

変換の数式は次のとおりです。

x' = x + xSkew ·y

y' ySkew · を =x + y

呼び出すのため`RotateDegrees`または`RotateRadians`α の角度での変換行列のとおりです。

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

変換の数式を次に示します。

x' cos(α) · を =x-sin(α) ·y

y' = sin(α) · x - cos(α) · y

Α 0 ° がある場合は、単位行列。 Α が 180 度の場合は、変換行列のとおりです。

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 度回転はオブジェクトの水平方向に反転および垂直方向には、-1 のスケール ファクターを設定することもできます。

このような変換として分類される*アフィン*を変換します。 アフィン変換には、決して、マトリックスは、0、0、および 1 の既定値のままの 3 番目の列が含まれます。 アーティクル[非アフィン変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)非アフィン変換について説明します。

## <a name="matrix-multiplication"></a>行列乗算

変換行列を使用して 1 つの大きな利点は、複合変換を SkiaSharp ドキュメントで多くの場合、参照されている行列乗算によって取得できます*連結*です。 内の関連する変換するメソッドの多くは`SKCanvas`「事前連結」や「より前の concat」を参照してください。 これは、乗算、行列の乗算では可換性のために重要になっている順序です。

たとえば、ドキュメントを[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/)メソッドのドキュメントの中に"Pre concats 現在の行列に指定の平行移動では、"it の質問、 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/)メソッドとその it"Pre-concats 小数点以下桁数が指定された現在の行列"

これは、メソッドの呼び出しで指定された変換は乗数 (左のオペランド) と現在の変換行列は被乗数 (右側のオペランド) を示します。

たとえば次に示す`Translate`続いてと呼ばれる`Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale`トランス フォームを掛けた、`Translate`複合変換行列に変換します。

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` 前に呼び出すことが`Translate`次のようにします。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

この場合は、乗算の順序を反転され、スケール ファクターが平行移動要素に適用される効果的に。

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

ここでは、`Scale`ピボット ポイントを持つメソッド。

```csharp
canvas.Scale(sx, sy, px, py);
```

これは、次の翻訳とスケールの呼び出しに相当します。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

逆の順序で 3 つの変換行列の乗算をコード内のメソッドの表示から。

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>SKMatrix 構造体

`SKMatrix`構造型の 9 つの読み取り/書き込みプロパティを定義する`float`変換行列の 9 つのセルに対応します。

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` という名前のプロパティも定義されて[ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/)型の`float[]`します。 このプロパティは、の順序で 1 つのショット 9 個の値を取得または設定を使用することができます`ScaleX`、 `SkewX`、 `TransX`、 `SkewY`、 `ScaleY`、 `TransY`、 `Persp0`、 `Persp1`、および`Persp2`です。

`Persp0`、 `Persp1`、および`Persp2`セルの記事で説明した[非アフィン変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)です。 これらのセルの既定値 0、0、および 1 の場合は、トランス フォームは、次のような座標点によって乗算されます。

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' ScaleX · を =x + SkewX ·y + x 方向

y' SkewX · を =x + ScaleY ·y + 直線移動

z' = 1

これは、完全な 2 次元アフィン変換です。 アフィン変換では、四角形が指定した平行四辺形以外のものに決してに変換されることを意味する並列の行を保持します。

`SKMatrix`構造を作成する複数の静的メソッドを定義する`SKMatrix`値。 これらのすべてのリターン`SKMatrix`値。

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) ピボット ポイントと
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) 角度をラジアン単位での
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) ピボット ポイントのラジアンで表した角度の
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) ピボット ポイントと
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` 定義いくつかの静的メソッド、2 つの行列の連結を乗算することを意味します。 これらのメソッドの名前は`Concat`、 `PostConcat`、および`PreConcat`、それぞれの 2 つのバージョンがあるとします。 これらのメソッドの戻り値であるなし代わりに、参照している既存の`SKMatrix`を通じて値`ref`引数。 次の例では、 `A`、 `B`、および`R`(「結果」) はすべて`SKMatrix`値。

2 つ`Concat`メソッドは次のように呼び出されます。

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

これらは、次の乗算を実行します。

R = B × A

他の方法では、2 つのパラメーターがあります。 最初のパラメーターは変更済み、およびメソッドの呼び出しから制御が戻るときに、2 つの行列の積を含むです。 2 つ`PostConcat`メソッドは次のように呼び出されます。

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

これらの呼び出しは、次の操作を実行します。

A × B を =

2 つ`PreConcat`メソッドは同様です。

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

これらの呼び出しは、次の操作を実行します。

A = B × A

これらのメソッド呼び出しですべてのバージョンの`ref`引数が若干効率的に基になる実装を呼び出すことでは、コードを読むとものであると仮定して他の人に混乱を招く場合があります、`ref`引数は、メソッドによって変更します。 さらのいずれかの結果では引数を渡すと便利は多くの場合、`Make`例については、メソッド。

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

これには、次のマトリックスを作成します。

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

これは、スケール変換を平行移動の変換を掛けた値です。 この特定のケースで、`SKMatrix`構造では、という名前のメソッドのショートカット[ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

これは 1 つである場合に安全に使用する、いくつかの回数、`SKMatrix`コンス トラクターです。 `SetScaleTranslate`メソッドは、行列のすべての 9 つのセルを設定します。 安全に使用でも、 `SKMatrix` 、静的コンス トラクター`Rotate`と`RotateDegrees`メソッド。

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

これらのメソッドは*いない*既存の変換を回転変換を連結します。 メソッドは、マトリックスのすべてのセルを設定します。 機能的に同じで、`MakeRotation`と`MakeRotationDegrees`メソッドがインスタンス化されないことを除き、`SKMatrix`値。

あると、`SKPath`表示する場合は、若干異なる向き、または別の中心点があることを希望するオブジェクト。 パスのすべての座標を変更するには呼び出すことによって、 [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/)メソッドの`SKPath`で、`SKMatrix`引数。 **パス変換**ページは、これを行う方法を示します。 [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs)クラスが参照、`HendecagramPath`フィールド内のオブジェクトは、そのパスに変換を適用するそのコンス トラクターを使用するがします。

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

`HendecagramPath`オブジェクトには、センター (0, 0) と、星の 11 個のポイントがそのセンターによってすべての方向に 100 単位から外側に向かって拡張します。 つまり、パスが正と負の値の両方の座標であります。 **パス変換**ページ優先的に正の値のすべての座標や 3 回として大きな、星型を処理します。 さらに、まっすぐ上のポイントにある星型の 1 つのポイントを望まないです。 代わりに、星の 1 つのポイントのまっすぐポイントを行います。 (星には、11 個のポイントがあるため、されていない両方。)これは、星の 360 ° 回転 22 で割った値が必要です。

コンス トラクターは、ビルド、`SKMatrix`オブジェクトを使用して 3 つの個別の変換から、 `PostConcat` 、A、B、および C のインスタンスは、次のパターンを持つメソッド`SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

これは、ので、結果が次のように、一連の連続する乗算です。

× B × C

連続する乗算は、各変換の実行内容を理解するうえで役立ちます。 スケール変換では、座標の範囲は –300 300 ~ ために、3 倍パス座標のサイズを増やします。 回転変換の原点の周囲の星を回転します。 平行移動の変換 300 ピクセル右とダウン、正の値になるすべての座標にそれがし、シフトします。

同じマトリックスを生成するその他のシーケンスがあります。 もう 1 つを次に示します。

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

これは、パスの中心を最初に、回転し、右側に 100 ピクセルに変換およびすべてダウン座標は正の値。 星は、の新しい左上隅に対して相対的は、ポイント (0, 0) のサイズが増加します。

`PaintSurface`ハンドラーは、このパスを表示できる単に。

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

これは、キャンバスの左上隅に表示されます。

[![](matrix-images/pathtransform-small.png "パスの変換 ページのスクリーン ショットをトリプル")](matrix-images/pathtransform-large.png#lightbox "トリプル ページのスクリーン ショット、パスの変換")

このプログラムのコンス トラクターには、次の呼び出しを使用してパスに、マトリックスが適用されます。

```csharp
transformedPath.Transform(matrix);
```

パスは*いない*プロパティとしては、このマトリックスを保持します。 代わりにのすべてのパスの座標に変換が適用されます。 場合`Transform`と呼ばれる、もう一度、トランス フォームが再び適用されること、および戻ることが唯一の方法が変換を元に戻す別の行列を適用することではします。 さいわい、`SKMatrix`構造体を定義、 [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix@/)行列を取得するメソッドは、指定された行列を反転させます。

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

メソッドは`TryInverse`すべて行列は反転できる場合は、非反転できるマトリックスされますがない可能性がグラフィックス変換に使用するためです。

行列変換を適用することも、`SKPoint`値は、点の配列、 `SKRect`、または、プログラム内で 1 つの数値だけです。 `SKMatrix`構造がという単語で始まるメソッドのコレクションを使用してこれらの操作をサポートしている`Map`、次のよう。

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

場合はその最後のメソッドを使用することに注意してください、`SKRect`構造体は、回転の四角形を表すことはできません。 メソッドは、のみ`SKMatrix`翻訳を表すとスケーリングの値。

### <a name="interactive-experimentation"></a>対話型の実験

アフィン変換用大まかに 1 つの方法は、画面の周りのビットマップの 3 つの角を移動して、どのような変換の結果が表示される対話形式でです。 これは、概念、**表示のアフィン行列**ページ。 このページには、他のデモで使用されるその他の 2 つのクラスが必要です。

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs)クラスは、画面の周りにドラッグできる半透明円を表示します。 `TouchPoint` いる必要があります、`SKCanvasView`または要素の親である、`SKCanvasView`が、 [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs)添付します。 `Capture` プロパティを `true` に設定します。 `TouchAction`プログラムで呼び出す必要があります、イベント ハンドラー、`ProcessTouchEvent`メソッド`TouchPoint`各`TouchPoint`インスタンス。 このメソッドを返します`true`タッチ ポイントを移動、タッチ イベントが発生した場合。 また、`PaintSurface`ハンドラーを呼び出す必要があります、`Paint`で各メソッド`TouchPoint`を渡す、インスタンス、`SKCanvas`オブジェクト。

`TouchPoint` 一般的に使われるを示して 方法 SkiaSharp ビジュアルを別のクラスにカプセル化できます。 このクラスは、ビジュアルの特性を指定するプロパティを定義でき、という名前のメソッド`Paint`で、`SKCanvas`引数が表示できます。

`Center`プロパティ`TouchPoint`オブジェクトの場所を示します。 このプロパティは、場所を初期化するために設定できます。プロパティは、ユーザーが、キャンバスの周囲 circle をドラッグしたときに変更します。

**アフィン変換行列のページを表示**も必要です、 [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs)クラスです。 このクラスは、のセルを表示、`SKMatrix`オブジェクト。 2 つのパブリック メソッドがある:`Measure`マトリックス形式表示のサイズを取得して`Paint`を表示します。 クラスが含まれています、`MatrixPaint`型のプロパティ`SKPaint`のフォント サイズや色を置き換えることができます。

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml)ファイルをインスタンス化、`SKCanvasView`アタッチし、`TouchEffect`です。 [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs)分離コード ファイルでは、3 つが作成されます`TouchPoint`オブジェクトし、埋め込みから読み込むビットマップの 3 つの角に対応する位置に設定し、リソース:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

アフィン行列は、3 つの点によって一意に定義されます。 3 つ`TouchPoint`オブジェクトは、ビットマップの左、右、上および左下隅に対応します。 アフィン行列はの四角形をした平行四辺形に変換を行うだけであるため、4 番目のポイントは、その他の 3 つによって暗黙的に指定します。 コンス トラクターは、最後の呼び出しに`ComputeMatrix`のセルを計算する、`SKMatrix`これら 3 つの点からのオブジェクト。

`TouchAction`ハンドラーの呼び出し、`ProcessTouchEvent`それぞれの`TouchPoint`します。 `scale` Xamarin.Forms 座標からピクセルに値を変換します。

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

存在する場合`TouchPoint`、移動、メソッドを呼び出して、`ComputeMatrix`再度し、画面を無効にします。

`ComputeMatrix`メソッドは、これら 3 つのポイントが含まれる、マトリックスを決定します。 マトリックスと呼ばれる`A`中と呼ばれるスケール変換を平行四辺形に 1 ピクセルの四角形四角形が 3 つの点に基づく変換`S`1 ピクセルの四角形の四角形にビットマップのスケールを設定します。 複合のマトリックスは`S`× `A`:

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

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

最後に、`PaintSurface`メソッドは、そのマトリックスに基づくビットマップを描画、画面の下部にあるマトリックスを表示し、ビットマップの 3 つのコーナーのタッチ ポイントを表示します。

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

次の iOS の画面は、ページが最初に読み込まれるときに、他の 2 つの画面にいくつかの操作後にこれを表示中に、ビットマップを示しています。

[![](matrix-images/showaffinematrix-small.png "アフィン変換行列の表示 ページのスクリーン ショットをトリプル")](matrix-images/showaffinematrix-large.png#lightbox "アフィン変換行列の表示 ページのトリプル スクリーン ショット")

これは、タッチ ポイントがフィルターの種類のみであると、ビットマップの角をドラッグするかのように見えます。 タッチ ポイントから計算されたマトリックスは、角がタッチ ポイントと一致するように、ビットマップを変換します。

ユーザーの移動、サイズ変更、およびコーナーをドラッグすることではなくビットマップを回転するより自然なを使用して 1 つまたは 2 本の指直接ドラッグするには、オブジェクトのピンチ、および回転します。 については、次の記事「[タッチ操作](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)です。

### <a name="the-reason-for-the-3-by-3-matrix"></a>3-3 でマトリックスの理由

2-2 での変換行列のみを 2 次元グラフィックス システムが必要する必要があることが予想される場合があります。

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

これは拡大/縮小、回転、およびでもがゆがめられるのではありませんの対応する最も基本的な変換、変換であります。

問題は、2-2 でマトリックスを表す、*線形*2 つのディメンションに変換します。 線形変換には、いくつかの基本的な算術演算が保持されますが、線形変換は、ポイント (0, 0) を決して変更による影響の 1 つです。 線形変換では、翻訳を不可能にします。

3 次元では、次のよう線形の変換行列。

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

ラベルの付いたセル`SkewXY`値が Y の値に基づいて、X 座標を傾斜することを意味; セル`SkewXZ`ことを示し、値のずれ Z; の値に基づいて、X 座標値は、他の同様に傾斜`Skew`セル。

この 3D 変換行列を設定して、2 次元平面を制限することは`SkewZX`と`SkewZY`を 0 にし、 `ScaleZ` 1。

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

場所 1 に等しい Z 3D スペースで平面に完全に、2 次元グラフィックスが描画されると場合、トランス フォーム乗算以下に示します。

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

すべてのままになる 2 次元平面上に Z が 1 と等しいですが、`SkewXZ`と`SkewYZ`セルは 2 次元の翻訳の要因になります。

これは、2 次元の非線形変換としての 3 次元の線形変換の機能です。 (たとえて、3 D グラフィックスの変換はに基づいて 4-4 でマトリックス。)

`SKMatrix` SkiaSharp 内の構造は、その 3 つ目の行のプロパティを定義します。

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

値を 0 以外を`Persp0`と`Persp1`2 次元平面オフ オブジェクトを移動するには、Z は 1 に等しい場所の変換が発生します。 それらのオブジェクトはその平面に戻しているときの動作については、アーティクルに[非アフィン変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)です。


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
