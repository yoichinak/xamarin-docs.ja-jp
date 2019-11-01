---
title: SkiaSharp の SVG パスデータ
description: この記事では、スケーラブルなベクターグラフィックス形式でテキスト文字列を使用して SkiaSharp パスを定義する方法について説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
ms.openlocfilehash: 809bcd8288c4c4205b3110418aeae0e08bf21dd6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029458"
---
# <a name="svg-path-data-in-skiasharp"></a>SkiaSharp の SVG パスデータ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_スケーラブルなベクターグラフィックス形式でテキスト文字列を使用してパスを定義する_

[`SKPath`](xref:SkiaSharp.SKPath)クラスは、スケーラブルベクターグラフィックス (SVG) 仕様によって確立された形式のテキスト文字列からパスオブジェクト全体の定義をサポートします。 この記事の後半では、次のようなパス全体をテキスト文字列で表現する方法について説明します。

![](path-data-images/pathdatasample.png "A sample path defined with SVG path data")

SVG は、web ページ用の XML ベースのグラフィックスプログラミング言語です。 SVG では、一連の関数呼び出しではなくマークアップでパスを定義できる必要があるため、SVG 標準には、グラフィックスパス全体をテキスト文字列として指定する非常に簡潔な方法が用意されています。

SkiaSharp 内では、この形式は "SVG パス-データ" と呼ばれます。 この形式は、Windows XAML ベースのプログラミング環境でもサポートされています。これには、Windows Presentation Foundation およびユニバーサル Windows プラットフォームが含まれます。これは、[パスマークアップ構文](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax)または[Move コマンドと draw commands 構文](/windows/uwp/xaml-platform/move-draw-commands-syntax/)と呼ばれます。 また、ベクターグラフィックスイメージの交換形式としても機能します。特に、XML などのテキストベースのファイルで使用できます。

[`SKPath`](xref:SkiaSharp.SKPath)クラスは2つのメソッドを定義し、名前に `SvgPathData` という単語を付けます。

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

静的[`ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String))メソッドは、文字列を `SKPath` オブジェクトに変換し、 [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) `SKPath` オブジェクトを文字列に変換します。

次に示すのは、ポイント (0, 0) を中心とする5つの星が100である場合の SVG 文字列です。

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

これらの文字は `SKPath` オブジェクトを作成するコマンドです。 `M` `MoveTo` の呼び出し、`L` が `LineTo`、`Z` が輪郭を閉じるために `Close` ていることを示します。 各数値ペアは、点の X 座標と Y 座標を提供します。 `L` コマンドの後に複数のポイントがコンマで区切られていることに注意してください。 一連の座標と点では、コンマと空白は同じように扱われます。 一部のプログラマは、点の間ではなく、X 座標と Y 座標の間にコンマを配置することを希望していますが、あいまいさを避けるために必要なのはコンマまたは空白のみです。 これは完全に有効です。

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG パスデータの構文は、 [svg 仕様のセクション 8.3](https://www.w3.org/TR/SVG11/paths.html#PathData)に正式に記載されています。 次に概要を示します。

## <a name="moveto"></a>**MoveTo**

```
M x y
```

これにより、現在の位置を設定することによって、パスの新しい輪郭が開始されます。 パスデータは常に `M` コマンドで始まる必要があります。

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

このコマンドは、直線 (または線) をパスに追加し、新しい現在位置を最後の行の末尾に設定します。 *X*座標と*y*座標の複数のペアを使用して、`L` コマンドに従うことができます。

## <a name="horizontal-lineto"></a>**横方向の LineTo**

```
H x ...
```

このコマンドは、パスに水平線を追加し、新しい現在位置を行の末尾に設定します。 複数の*x*座標で `H` コマンドに従うことはできますが、あまり意味がありません。

## <a name="vertical-line"></a>**垂直線**

```
V y ...
```

このコマンドは、パスに垂直線を追加し、新しい現在位置を行の末尾に設定します。

## <a name="close"></a>**閉じる**

```
Z
```

`C` コマンドは、現在位置から輪郭の先頭に直線を追加することによって、輪郭を閉じます。

## <a name="arcto"></a>**ArcTo**

楕円の円弧を輪郭に追加するコマンドは、SVG パスデータ仕様全体で最も複雑なコマンドです。 数値が座標値以外の値を表すことができる唯一のコマンドです。

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx*との*パラメーターは*、楕円の水平方向と垂直方向の半径です。 *回転角度*は時計回りに回転します。

大きな弧の場合は、*大きい*円弧の場合は1に、小さい場合は0に設定します。

時計回りの場合は*スイープフラグ*を1、反時計回りの場合は0に設定します。

弧は、新しい現在位置になる点 (*x*, *y*) に描画されます。

## <a name="cubicto"></a>**CubicTo**

```
C x1 y1 x2 y2 x3 y3 ...
```

このコマンドは、現在の位置から (*x3*, *y3*) に3次ベジエ曲線を追加します。これは、新しい現在位置になります。 ポイント (*x1*, *y1*) と (*x2*, *y2*) は制御ポイントです。

複数のベジエ曲線は、1つの `C` コマンドで指定できます。 ポイント数は3の倍数である必要があります。

"Smooth" ベジエ曲線コマンドもあります。

```
S x2 y2 x3 y3 ...
```

このコマンドは、通常のベジエコマンドに従う必要があります (ただし、これは厳密には必須ではありません)。 Smooth ベジエコマンドは、最初の制御点を計算して、相互に関連する前のベジエの2つ目の制御点の反射を行います。 したがって、この3つの点は colinear であり、2つのベジエ曲線間の接続はスムーズです。

## <a name="quadto"></a>**QuadTo**

```
Q x1 y1 x2 y2 ...
```

2次ベジエ曲線の場合、ポイント数は2の倍数である必要があります。 制御点は (*x1*, *y1*) で、終了点 (および新しい現在の位置) は (*x2*, *y2*) です。

Smooth 2 次曲線コマンドもあります。

```
T x2 y2 ...
```

制御点は、前の2次曲線の制御点に基づいて計算されます。

これらのコマンドはすべて "相対" バージョンでも使用できます。座標点は現在の位置を基準としています。 これらの相対コマンドは小文字で始まります。たとえば、3つのベジエコマンドの相対バージョンの `C` ではなく `c` です。

これは、SVG パスデータ定義の範囲です。 コマンドのグループを繰り返したり、任意の種類の計算を実行したりするための機能はありません。 `ConicTo` またはその他の種類の arc 仕様のコマンドは使用できません。

静的[`SKPath.ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String))メソッドには、有効な SVG コマンドの文字列が必要です。 構文エラーが検出された場合、メソッドは `null`を返します。 これだけがエラーを示しています。

[`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData)メソッドは、既存の `SKPath` オブジェクトから SVG パスデータを取得して別のプログラムに転送したり、XML などのテキストベースのファイル形式で格納したりする場合に便利です。 (`ToSvgPathData` メソッドは、この記事のサンプルコードには記載されていません)。`ToSvgPathData` が、パスを作成したメソッドの呼び出しに正確に対応する文字列を返すことを想定*しない*でください。 特に、円弧は複数の `QuadTo` コマンドに変換され、`ToSvgPathData`から返されるパスデータでのそれらの表示方法がわかります。

**Path Data Hello**ページでは、SVG パスデータを使用して "hello" という語が使用されています。 `SKPath` オブジェクトと `SKPaint` オブジェクトは、どちらも[`PathDataHelloPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs)クラスのフィールドとして定義されます。

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

テキスト文字列を定義するパスは、ポイント (0, 0) の左上隅から開始します。 各文字は50ユニット幅と100単位で、文字は別の25単位で区切られています。つまり、パス全体が350単位幅になります。

"Hello" の ' H ' は 3 1 ラインの輪郭で構成されていますが、' E ' は2つの接続された3次ベジエ曲線で構成されています。 `C` コマンドの後に6つのポイントがあり、2つのコントロールポイントの Y 座標が–10と110であることに注意してください。これにより、他の文字の Y 座標の範囲外に配置されます。 ' L ' は2つの接続された線ですが、' O ' は `A` コマンドでレンダリングされる楕円です。

最後の輪郭を開始した `M` コマンドによって位置がポイント (350, 50) に設定されていることに注意してください。これは、' O ' の左辺の垂直方向の中心です。 `A` コマンドの後の最初の数字で示されているように、楕円の水平方向の半径は25、垂直方向の半径は50です。 エンドポイントは、`A` コマンドの最後の数値ペアによって示されます。これは、ポイント (300, 49.9) を表します。 これは、開始点とは少し異なります。 エンドポイントが始点と同じに設定されている場合、円弧はレンダリングされません。 完全な楕円を描画するには、エンドポイントの近くにエンドポイントを設定する必要があります。または、2つ以上の `A` コマンドを使用する必要があります。これらは完全な楕円の一部に使用します。

ページのコンストラクターに次のステートメントを追加し、結果の文字列を確認するためのブレークポイントを設定することができます。

```csharp
string str = helloPath.ToSvgPathData();
```

Arc は、2次ベジエ曲線を使用して、弧の段階的な部分に対して、`Q` の長い一連のコマンドに置き換えられていることがわかります。

`PaintSurface` ハンドラーは、パスの厳密な境界を取得します。これには、' E ' および ' O ' 曲線の制御点は含まれません。 3つの変換は、パスの中心をポイント (0, 0) に移動し、キャンバスのサイズに合わせてパスを拡大します (さらに、ストロークの幅を考慮に入れます)。次に、パスの中心をキャンバスの中央に移動します。

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

パスは、横モードで表示するとより適切に見えるキャンバスを塗りつぶします。

[![](path-data-images/pathdatahello-small.png "Triple screenshot of the Path Data Hello page")](path-data-images/pathdatahello-large.png#lightbox "Triple screenshot of the Path Data Hello page")

**[Path Data Cat]** ページは似ています。 Path オブジェクトと paint オブジェクトは、どちらも[`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs)クラスのフィールドとして定義されています。

```csharp
public class PathDataCatPage : ContentPage
{
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

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Cat の先頭は円であり、ここでは2つの `A` コマンドを使用してレンダリングされます。それぞれのコマンドで半円が描画されます。 ヘッドの `A` コマンドはどちらも、100の水平方向と垂直方向の半径を定義します。 最初の弧は (240, 100) から始まり、(240, 300) で終わります。これは、(240, 100) で終了する2番目の弧の始点になります。

2つの `A` コマンドも一緒に表示されます。また、cat のヘッドと同様に、2番目の `A` コマンドは、最初の `A` コマンドの先頭と同じ位置で終了します。 ただし、これらの `A` コマンドのペアでは、楕円は定義されません。 各弧ののは40単位であり、半径も40単位であるため、これらの円弧は完全な semicircles ではありません。

`PaintSurface` ハンドラーは、前のサンプルと同様の変換を実行しますが、1つの `Scale` ファクターを設定して縦横比を維持し、小さな余白を設定します。これにより、cat のひげが画面の側面に触れることはありません。

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

実行中のプログラムを次に示します。

[![](path-data-images/pathdatacat-small.png "Triple screenshot of the Path Data Cat page")](path-data-images/pathdatacat-large.png#lightbox "Triple screenshot of the Path Data Cat page")

通常、`SKPath` オブジェクトがフィールドとして定義されている場合、そのパスの輪郭は、コンストラクターまたは別のメソッドで定義されている必要があります。 ただし、SVG パスデータを使用する場合は、フィールド定義でパスを完全に指定できることがわかりました。

[[**変換の回転**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)」では、前に見た見**づらいアナログクロック**サンプルが、時計の針を単純な線として表示していました。 次**の例**では、これらの行を、`SKPaint` オブジェクトと共に[`PrettyAnalogClockPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs)クラスのフィールドとして定義されている `SKPath` のオブジェクトに置き換えています。

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

時間と分の針は、区分を囲みました。 これらを互いに区別するために、`handStrokePaint` オブジェクトと `handFillPaint` オブジェクトを使用して、黒のアウトラインとグレーの塗りつぶしの両方で描画されます。

以前の見**づらいアナログクロック**サンプルでは、時間と分をマークした小さな円がループで描画されました。 この**かなりのアナログクロック**サンプルでは、まったく異なるアプローチが使用されます。時間と分のマークは、`minuteMarkPaint` オブジェクトと `hourMarkPaint` オブジェクトで描画される点線です。

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

[**ドットとダッシュ**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)の記事では、 [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash*)メソッドを使用して破線を作成する方法について説明しました。 最初の引数は、通常、次の2つの要素を持つ `float` 配列です。最初の要素はダッシュの長さで、2番目の要素はダッシュの間の間隔です。 `StrokeCap` プロパティが `SKStrokeCap.Round`に設定されている場合、ダッシュの丸められた端は、ダッシュの両側のストローク幅によってダッシュの長さを効果的に長くします。 したがって、最初の配列要素を0に設定すると、点線が作成されます。

これらの点の間の距離は、2番目の配列要素によって制御されます。 すぐにわかるように、これら2つの `SKPaint` オブジェクトを使用して、半径が90単位の円を描画します。 この円の円周は180πであるため、60分のマークは3π単位ごとに表示される必要があります。これは、`minuteMarkPaint`の `float` 配列の2番目の値です。 12時間マークは、2番目の `float` 配列の値である15π単位ごとに指定する必要があります。

`PrettyAnalogClockPage` クラスは、16ミリ秒ごとにサーフェイスを無効にするタイマーを設定し、その速度で `PaintSurface` ハンドラーが呼び出されます。 `SKPath` オブジェクトと `SKPaint` オブジェクトの以前の定義では、非常にクリーンな描画コードを使用できます。

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

ただし、特別な処理は2番目の作業で行われます。 クロックは16ミリ秒ごとに更新されるため、`DateTime` 値の [`Millisecond`] プロパティを使用して、2番目から2秒目に不連続のジャンプで移動するスイープをアニメーション化することができます。 しかし、このコードでは、動きを滑らかにすることはできません。 代わりに、別の種類の移動を行うために、Xamarin. Forms [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)と[`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)アニメーションイージング関数を使用します。 これらのイージング関数を使用すると、2番目の手を jerkier な方法で移動し、移動する前に少し前に戻り、その変換先を少し &mdash; します。その結果、これらの静的なスクリーンショットでは、残念ながら再現できません。

[![](path-data-images/prettyanalogclock-small.png "Triple screenshot of the Pretty Analog Clock page")](path-data-images/prettyanalogclock-large.png#lightbox "Triple screenshot of the Pretty Analog Clock page")

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
