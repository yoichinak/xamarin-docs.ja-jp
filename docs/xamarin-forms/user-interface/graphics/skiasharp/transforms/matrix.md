---
title: SkiaSharp の行列変換
description: この記事では、汎用性の高い変換マトリックスで SkiaSharp の変換に深く踏み込み、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2017
ms.openlocfilehash: 07b6a13a8bba1e30db1d69e49aa87420bbbdf601
ms.sourcegitcommit: a635312ffec816ba357a92b66c8c5221c8d9044c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "39615432"
---
# <a name="matrix-transforms-in-skiasharp"></a>SkiaSharp の行列変換

_SkiaSharp の変換に、汎用性の高い変換行列に詳しく調べる_

適用されたすべての変換、`SKCanvas`オブジェクトは、単一のインスタンスで統合されます、 [ `SKMatrix` ](xref:SkiaSharp.SKMatrix)構造体。 これは、すべての最新の 2D グラフィックス システムとよく似た標準 3-3 での変換行列です。

、説明したようにして変換 SkiaSharp、マトリックスが変換行列は理論上の観点から重要なと変換を使用してパスを変更することが重要ですが、変換認識せずに、または両方の複雑なタッチ入力を処理するためこの記事で、[次へ] が示されています。

![](matrix-images/matrixtransformexample.png "アフィン変換の対象にビットマップ")

現在の変換行列に適用される、 `SKCanvas` 、読み取り専用にアクセスすることはいつでも使用可能な[ `TotalMatrix` ](xref:SkiaSharp.SKCanvas.TotalMatrix)プロパティ。 新しい変換マトリックスを使用して、設定することができます、 [ `SetMatrix` ](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix))メソッドできますを復元してその変換マトリックスの既定値を呼び出して[ `ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix)します。

唯一の他の`SKCanvas`キャンバスの行列変換で直接動作するメンバーが[ `Concat` ](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@))一緒に乗算することによって 2 つの行列を連結します。

既定の変換行列恒等行列は、対角線方向のセルと 0 それ以外の場所に 1 で構成されています。

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

静的なを使用して行列を作成する[ `SKMatrix.MakeIdentity` ](xref:SkiaSharp.SKMatrix.MakeIdentity)メソッド。

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix`既定のコンス トラクターは*いない*恒等行列を返します。 すべてゼロに設定するセルの行列を返します。 使用しないでください、`SKMatrix`コンス トラクターを手動でそれらのセルを設定しない場合。

SkiaSharp のグラフィカル オブジェクトをレンダリングするとき (x, y) の各ポイントが 3 番目の列では 1 - 1-3 でマトリックスを効果的に変換されます。

<pre>
| x  y  1 |
</pre>

この 1-3 でマトリックスは、1 に設定する Z 座標を 3 次元のポイントを表します。 (後で説明します)、数学的な理由があるため、2 次元行列変換では、3 つのディメンションでの作業が必要です。 Z が 1、3 D 座標系ですが常に 2D 平面上のポイントを表すものとしてこの 1-3 でマトリックスを考えることができます。

1-3 でこの行列は変換の行列で乗算し、結果は、キャンバスにレンダリングされたポイント。

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

標準的な行列乗算を使用して、変換後のポイント次に示します。

x' = x

y' = y

z' = 1

既定の変換です。

ときに、`Translate`メソッドが、`SKCanvas`オブジェクト、`tx`と`ty`への引数、`Translate`メソッドの変換行列の 3 番目の行の最初の 2 つのセルになります。

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

変換式は、次に示します。

x' = x + tx

y' = y + ty

スケール ファクターがある既定値は 1 です。 呼び出すと、`Scale`上の新しいメソッド`SKCanvas`オブジェクト、結果の変換マトリックスが含まれて、`sx`と`sy`対角線方向のセル内の引数。

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

変換式は次のとおりです。

x' sx 押しを =x

y' sy 押しを =y

呼び出した後、変換行列`Skew`スケール ファクターの横にあるマトリックス セルに 2 つの引数が含まれています。

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

変換式は次のとおりです。

x' = x + xSkew 押しy

y' ySkew 押しを =x + y

呼び出すのため`RotateDegrees`または`RotateRadians`α の角度の変換行列のとおりです。

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

変換式は、次に示します。

x' cos(α) 押しを =x-sin(α) 押しy

y' sin(α) 押しを =x-cos(α) 押しy

Α が 0 度の場合は、恒等行列になります。 Α が 180 度と、変換行列のとおりです。

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 度回転は、-1 のスケール ファクターを設定しても実行するが水平および垂直に、オブジェクトの反転と同じです。

これらすべての種類の変換として分類される*アフィン*を変換します。 アフィン変換には、3 番目の列マトリックスは、0、0、および 1 の既定値のままのにはことはありませんが含まれます。 この記事[**非アフィン変換**](non-affine.md)非アフィン変換について説明します。

## <a name="matrix-multiplication"></a>行列乗算

変換行列を使用しての 1 つの大きな利点は、複合変換として SkiaSharp ドキュメントで多くの場合、参照は、行列乗算により得られること*連結*します。 多くの変換に関連するメソッドの`SKCanvas`「事前連結」または"プレ-concat"を参照してください これは、順序は、乗算、行列の乗算は可換的ではないために重要です。

たとえば、ドキュメントを[ `Translate` ](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single))メソッドのドキュメントの中に"Pre concats 現在の行列に指定した平行移動、"it の質問、 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single))メソッドとその it"プレ-concats 現在の行列に指定したスケールします"

これは、メソッドの呼び出しで指定された変換は、乗数 (左側のオペランド) を現在の変換マトリックスが multiplicand (右側のオペランド) を意味します。

たとえば次に示す`Translate`続くと呼ばれる`Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale`変換を乗算して、`Translate`複合変換行列に変換します。

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` 前に呼び出すことができます`Translate`次のようにします。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

その場合は、乗算の順序を逆にすると、し、スケール ファクターは、翻訳の要因に効果的に適用します。

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

ここでは、`Scale`ピボット ポイントを持つメソッド。

```csharp
canvas.Scale(sx, sy, px, py);
```

これは、次の翻訳とスケールの呼び出しと同等です。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

逆の順序で次の 3 つの変換行列の乗算をコードでメソッドの表示から。

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>SKMatrix 構造体

`SKMatrix`構造型の 9 つの読み取り/書き込みプロパティを定義する`float`変換行列の 9 つのセルに対応します。

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` という名前のプロパティも定義します[ `Values` ](xref:SkiaSharp.SKMatrix.Values)型の`float[]`します。 このプロパティを順に一度に 9 つの値を取得または設定を使用できます`ScaleX`、 `SkewX`、 `TransX`、 `SkewY`、 `ScaleY`、 `TransY`、 `Persp0`、 `Persp1`、および`Persp2`します。

`Persp0`、 `Persp1`、および`Persp2`セルは、情報の記事で説明した[**非アフィン変換**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)します。 これらのセルの既定値 0、0、および 1 の場合は、変換は、このような座標点で乗算されます。

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' ScaleX 押しを =x + SkewX 押しy + x 方向

y' SkewX 押しを =x + ScaleY 押しy + 直線移動

z' = 1

これは、完全な 2 次元アフィン変換です。 アフィン変換では、四角形を平行四辺形以外のものに変換しないことを意味する、平行線を保持します。

`SKMatrix`構造体を作成するいくつかの静的メソッドを定義する`SKMatrix`値。 これらのすべての戻り`SKMatrix`値。

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single)) ピボット ポイントを持つ
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single)) 角度 (ラジアン単位)
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single)) ピボット ポイントをラジアン単位の角度の
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single)) ピボット ポイントを持つ
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix` 定義、2 つの行列を連結するいくつかの静的メソッドに乗算することを意味します。 これらのメソッドの名前は[ `Concat` ](xref:SkiaSharp.SKMatrix.Concat*)、 [ `PostConcat` ](xref:SkiaSharp.SKMatrix.PostConcat*)、および[ `PreConcat` ](xref:SkiaSharp.SKMatrix.PreConcat*)、それぞれの 2 つのバージョンがあるとします。 これらのメソッドの戻り値であるありません。代わりに、既存の参照`SKMatrix`を通じて値`ref`引数。 次の例では、 `A`、 `B`、および`R`(「結果」) はすべて`SKMatrix`値。

2 つ`Concat`メソッドは次のように呼び出されます。

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

これらは、次の乗算を実行します。

R = B × A

他の方法では、2 つのパラメーターがあります。 最初のパラメーターは変更済み、およびメソッドの呼び出しから返された場合は、2 つの行列の積を格納します。 2 つ`PostConcat`メソッドは次のように呼び出されます。

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

これらの呼び出しでは、次の操作を実行します。

A = × B

2 つ`PreConcat`メソッドは同様です。

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

これらの呼び出しでは、次の操作を実行します。

A = B × A

これらのメソッドでは、すべてのバージョン`ref`引数は、基になる実装を呼び出すことより若干効率的ですが、コードを読むとを含むものがあると仮定して、ユーザーが混乱を招くことが考えられます、`ref`で引数を変更メソッド。 さらに、いずれかの結果では引数を渡すと便利は多くの場合、`Make`メソッド、たとえば。

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

これは、次のマトリックスを作成します。

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

これは、スケール変換を平行移動変換を乗算します。 このケースで、`SKMatrix`構造では、という名前のメソッドのショートカット[ `SetScaleTranslate` ](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

これは 1 つの数回を安全に使用すると、`SKMatrix`コンス トラクター。 `SetScaleTranslate`メソッドは、行列の 9 つすべてのセルを設定します。 安全に使用でも、 `SKMatrix` 、静的コンス トラクター`Rotate`と`RotateDegrees`メソッド。

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

これらのメソッドは*いない*既存の変換を回転変換を連結します。 メソッドは、マトリックスのすべてのセルを設定します。 まったく同じ機能は、`MakeRotation`と`MakeRotationDegrees`メソッドがインスタンス化されないことを除いて、`SKMatrix`値。

あるとします、`SKPath`オブジェクトを表示したいが、若干異なる方向の場合、または別の中心点がある場合です。 パスのすべての座標を変更するには呼び出すことによって、 [ `Transform` ](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix))メソッドの`SKPath`で、`SKMatrix`引数。 **パス変換**ページは、これを行う方法を示します。 [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs)クラスが参照、`HendecagramPath`フィールド内のオブジェクトがそのパスに変換を適用するが、コンス トラクターを使用します。

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

`HendecagramPath`オブジェクトには、中心 (0, 0) し、星の 11 点をそのセンターによってすべての方向に 100 単位から外側に向かって拡張します。 これは、パスが正と負の値の両方の座標であることを意味します。 **パス変換** ページですべての正の座標を使用して 3 回として、大規模な星型で動作するが優先されます。 さらに、そのまま上のポイントの星の 1 つのポイントは望ましくありません。 代わりに、星の 1 つのポイントのまっすぐポイントします。 (星は 11 点があるため、されていない両方。)これは、星を 360 度回転 22 で割った値が必要です。

コンス トラクターは、ビルド、`SKMatrix`オブジェクトを使用して 3 つの個別の変換から、`PostConcat`メソッド A、B、および C がのインスタンスを次のパターンで`SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

これは、結果は次のように、一連の連続する乗算は、です。

× B × C

連続する乗算は、各変換の動作を理解するうえで役立ちます。 スケール変換では、座標 –300 から 300 の範囲であるために、第 3 の倍数でパスの座標のサイズが大きくなります。 回転の変換では、星の原点の周囲を回転します。 平行移動変換を 300 ピクセル右と正の値になるすべての座標にこれが、シフトします。

同じマトリックスを生成するその他のシーケンスがあります。 もう 1 つを次に示します。

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

これは、パスの中心を最初に、回転し、右側に 100 ピクセルに変換し、座標が正の値はすべてダウンします。 星がその新しい左上隅に対して相対的は、ポイント (0, 0) のサイズが増加します。

`PaintSurface`ハンドラーは、このパスを表示できるだけです。

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

[![](matrix-images/pathtransform-small.png "パスの変換 ページのスクリーン ショットをトリプル")](matrix-images/pathtransform-large.png#lightbox "パスに変換 ページの 3 倍になるスクリーン ショット")

このプログラムのコンス トラクターでは、次の呼び出しを使用してパスに、マトリックスが適用されます。

```csharp
transformedPath.Transform(matrix);
```

パスは*いない*プロパティとしてこの行列を保持します。 代わりに、すべてのパスの座標に変換を適用します。 場合`Transform`と呼ばれますが、もう一度変換は、もう一度適用され、唯一の方法が戻ることができますが、変換元に戻し、別の行列を適用することでは。 さいわい、`SKMatrix`構造体を定義、 [ `TryInvert` ](xref:SkiaSharp.SKMatrix.TryInvert*)行列を取得するメソッドは、指定された行列を反転させます。

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

メソッドは`TryInverse`すべて行列は反転が非可逆のマトリックスがグラフィックス変換に使用する可能性があります。

行列変換を適用することも、`SKPoint`値、点の配列、 `SKRect`、または、プログラム内で 1 つの数値のみでも。 `SKMatrix`構造体は、単語で始まるメソッドのコレクションでこれらの操作をサポートしている`Map`、次のようします。

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

その最後のメソッドを使用する場合、ことに注意、`SKRect`構造が回転した四角形を表すことができます。 メソッドのみに適した、`SKMatrix`翻訳を表すとスケーリングの値。

## <a name="interactive-experimentation"></a>対話型の実験

アフィン変換をおおまかに把握する方法の 1 つは、対話的に、画面の周りのビットマップの 3 つの角を移動し、どのような変換の結果が表示されることです。 これは考え方、**アフィン行列を表示する**ページ。 このページには、その他のデモでも使用されているその他の 2 つのクラスが必要です。

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs)クラスを画面にドラッグできる半透明の円が表示されます。 `TouchPoint` 必要があります、`SKCanvasView`または要素の親である、`SKCanvasView`が、 [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs)アタッチします。 `Capture` プロパティを `true`に設定します。 `TouchAction`イベント ハンドラーでは、プログラムを呼び出す必要があります、`ProcessTouchEvent`メソッド`TouchPoint`各`TouchPoint`インスタンス。 メソッドを返します`true`タッチ ポイントの移動にタッチ イベントが発生した場合。 また、`PaintSurface`ハンドラーを呼び出す必要があります、`Paint`メソッドで各`TouchPoint`をそれに渡すインスタンス、`SKCanvas`オブジェクト。

`TouchPoint` 共通の示す方法別のクラスで SkiaSharp visual をカプセル化できます。 このクラスは、ビジュアルの特性を指定するプロパティを定義でき、という名前のメソッド`Paint`で、`SKCanvas`引数でレンダリングできます。

`Center`プロパティの`TouchPoint`オブジェクトの場所を示します。 このプロパティは、場所を初期化するために設定できます。プロパティは、ユーザーがキャンバスの周囲の円をドラッグしたときに変更します。

**アフィン行列のページを表示**も必要です、 [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs)クラス。 このクラスのセルの表示、`SKMatrix`オブジェクト。 2 つのパブリック メソッドが含まれて: `Measure` 、マトリックス形式表示の次元を取得し、`Paint`表示します。 クラスが含まれています、`MatrixPaint`型のプロパティ`SKPaint`のフォント サイズや色を置き換えることができます。

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml)ファイルをインスタンス化、`SKCanvasView`アタッチし、`TouchEffect`します。 [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs)分離コード ファイルを作成します 3`TouchPoint`オブジェクトし、埋め込まれたから読み込むビットマップの 3 つの角に対応する位置に設定し、リソース:

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

アフィン行列は、次の 3 つのポイントで一意に定義されます。 3 つ`TouchPoint`オブジェクトは、ビットマップの左、右上、左下隅に対応します。 アフィン行列は、四角形を平行四辺形に変換するための対応はのみであるために、他の 3 つが、4 番目のポイントが含まれます。 コンス トラクターの最後の呼び出しに`ComputeMatrix`のセルが計算される`SKMatrix`これら 3 つのポイントからのオブジェクト。

`TouchAction`ハンドラーの呼び出し、`ProcessTouchEvent`メソッドをそれぞれ`TouchPoint`します。 `scale` Xamarin.Forms 座標から値がピクセルに変換します。

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

存在する場合`TouchPoint`、移動、メソッドを呼び出して、`ComputeMatrix`もう一度し、画面を無効にします。

`ComputeMatrix`メソッドは、これら 3 つのポイントが含まれるマトリックスを判断します。 マトリックスと呼ばれる`A`中と呼ばれるスケール変換の変換を平行四辺形に 1 ピクセルの正方形四角形が 3 つの点に基づく`S`1 ピクセルの正方形の四角形にビットマップを拡大または縮小します。 複合のマトリックスは`S`× `A`:

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

最後に、`PaintSurface`メソッドは、そのマトリックスに基づくビットマップをレンダリングします、画面の下部にあるマトリックスを表示し、ビットマップの 3 つの角にタッチ ポイントを表示します。

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

次の iOS の画面は、ページが最初に読み込まれると、いくつかの操作の後に、その他の 2 つの画面を示して、ビットマップを示しています。

[![](matrix-images/showaffinematrix-small.png "アフィン行列を表示するページのスクリーン ショットをトリプル")](matrix-images/showaffinematrix-large.png#lightbox "アフィン行列を表示するページの 3 倍になるスクリーン ショット")

タッチ ポイントがフィルターの種類のみであると、ビットマップの角をドラッグするかのように見えます。 タッチ ポイントから計算されたマトリックスは、コーナーは、タッチ ポイントと一致するように、ビットマップを変換します。

ユーザーを移動、サイズ変更、およびコーナーをドラッグすることではなくビットマップを回転するためより自然なですを使用して 1 つまたは 2 本の指直接ドラッグするには、オブジェクトのピンチ、および回転します。 これについては、次の記事[**タッチ操作**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)します。

## <a name="the-reason-for-the-3-by-3-matrix"></a>3-3 でマトリックスの理由

2 次元グラフィックス システムが 2-2 での変換マトリックスのみを要求するようことが想定する可能性があります。

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

拡大縮小、回転、およびも傾斜では、これは機能が、対応のない最も基本的な変換、変換であります。

問題は、2-2 で行列を表すことを*線形*2 次元で変換します。 線形変換には、いくつかの基本的な算術演算が保持されますが、線形変換は、点 (0, 0) を決して変更による影響の 1 つです。 線形変換により、変換が不可能です。

3 次元では、線形変換マトリックスがようになります。

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

というラベルの付いたセル`SkewXY`値が Y の値に基づいて、X 座標を傾けますつまり; セル`SkewXZ`値 Z; の値に基づいて、X 座標を傾斜すると、その他の値が同様に傾斜`Skew`セル。

この 3D 変換行列を設定して、2 次元平面に制限することは`SkewZX`と`SkewZY`を 0 にし、 `ScaleZ` 1。

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Z が 1、3 D 空間の平面に完全には、2 次元グラフィックスが描画される、変換乗算はようになります。

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Z が 1、2 次元平面上に留まりますすべてが、`SkewXZ`と`SkewYZ`セルは 2 次元変換の要因になります。

これは、3 次元の線形変換は、2 次元の非線形変換として機能する方法です。 (たとえて、3 D グラフィック内の変換に基づく 4-4 でマトリックス。)

`SKMatrix` SkiaSharp の構造は、その 3 つ目の行のプロパティを定義します。

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

非ゼロ値`Persp0`と`Persp1`Z が 1、2 次元面をオブジェクトの移動変換が発生します。 記事で説明がそれらのオブジェクトはその平面に戻すときに起こる[**非アフィン変換**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)します。


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
