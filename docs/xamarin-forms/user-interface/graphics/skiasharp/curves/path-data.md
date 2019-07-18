---
title: SkiaSharp の SVG パス データ
description: この記事では、テキスト文字列を使用して、スケーラブル ベクター グラフィックス形式で SkiaSharp パスを定義する方法について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
ms.openlocfilehash: 690d3c15d7ad2aad06be5b499bae1a94107414f4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019587"
---
# <a name="svg-path-data-in-skiasharp"></a>SkiaSharp の SVG パス データ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_スケーラブル ベクター グラフィックス形式でテキスト文字列を使用してパスを定義します。_

[ `SKPath` ](xref:SkiaSharp.SKPath)クラスは、スケーラブル ベクター グラフィックス (SVG) 仕様で確立されている形式でテキスト文字列からパス全体のオブジェクトの定義をサポートしています。 文字列のようなパス全体を表現するこの記事の後半で表示されます。

![](path-data-images/pathdatasample.png "SVG パス データで定義されているパスのサンプル")

SVG では、プログラミング言語の web ページを XML ベースのグラフィックです。 SVG では、一連の関数呼び出しではなく、マークアップで定義するパスを許可する必要があります、ために、標準の SVG には、全体のグラフィックス パスを指定するテキスト文字列としての非常に簡潔な方法が含まれています。

SkiaSharp、内でこの形式は、データと呼ば"SVG パス -" 形式は Windows XAML ベースのプログラミング環境、Windows Presentation Foundation などと呼ばれますが、ユニバーサル Windows プラットフォームでサポートされても、[パス マークアップ構文](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax)または[移動描画コマンドの構文と](/windows/uwp/xaml-platform/move-draw-commands-syntax/)します。 これは、はベクター グラフィックス イメージ、XML などのテキスト ベースのファイルで特にの交換形式としても使用できます。

[ `SKPath` ](xref:SkiaSharp.SKPath)クラスは、言葉で 2 つのメソッドを定義します。 `SvgPathData` 、名前にします。

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

静的な[ `ParseSvgPathData` ](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String))メソッドへの文字列に変換、`SKPath`オブジェクト、中に[ `ToSvgPathData` ](xref:SkiaSharp.SKPath.ToSvgPathData)変換、`SKPath`オブジェクトを文字列。

ポイントを中心 (0, 0) を radius で 100 の 5 ポイントの星の SVG 文字列を次に示します。

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

文字はビルド コマンド、`SKPath`オブジェクト:`M`を示します、`MoveTo`を呼び出すと、`L`は`LineTo`、および`Z`は`Close`輪郭を閉じます。 各番号のペアは、点の X と Y 座標を提供します。 注意、`L`コマンドにはコンマで区切られた複数のポイントが続きます。 一連の座標とポイント、コンマと空白で同一に扱われます。 いくつかのプログラマは、点の間ではなく、X および Y 座標の間にコンマが挿入するが、コンマまたは空白のみのあいまいさを回避するために必要です。 これは、機能は、完全に有効です。

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG パス データの構文が正式に記載されている[SVG 仕様のセクションの 8.3](http://www.w3.org/TR/SVG11/paths.html#PathData)します。 概要を次に示します。

## <a name="moveto"></a>**MoveTo**

```
M x y
```

これには、現在の位置を設定して新しいパスの輪郭が開始されます。 パスのデータがで始める必要があります、`M`コマンド。

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

このコマンドは、直線 (または行) をパスに追加し、最後の行の末尾に新しい現在位置を設定します。 利用できる、`L`コマンドの複数のペアを*x*と*y*座標。

## <a name="horizontal-lineto"></a>**水平 LineTo**

```
H x ...
```

このコマンドは、パスに水平な線を追加し、行の末尾に新しい現在位置を設定します。 利用できる、`H`複数コマンド*x*座標が、それをあまり意味がなさない。

## <a name="vertical-line"></a>**垂直線**

```
V y ...
```

このコマンドは、パスに垂直線を追加し、行の末尾に新しい現在位置を設定します。

## <a name="close"></a>**閉じる**

```
Z
```

`C`コマンドは、現在の位置から、輪郭の先頭に直線を追加することで、輪郭を閉じます。

## <a name="arcto"></a>**ArcTo**

楕円の円弧を曲線に追加するコマンドは、全体の SVG パス データ仕様で最も複雑なコマンドでは圧倒的です。 数値できますを表すが座標の値以外のものだけで、コマンドは。

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx*と*ry*パラメーターは、楕円の水平および垂直方向の半径。 *回転角度*が時計回り (度単位)。

設定、*円弧フラグでは大きな*1 大きい円弧をしたり、小さい円弧の場合は 0。

設定、*スイープ フラグ*の 1 を 0 にし、時計回りに反時計回りにします。

ポイントに円弧が描画される (*x*、 *y*)、新しい現在位置になりますが。

## <a name="cubicto"></a>**CubicTo**

```
C x1 y1 x2 y2 x3 y3 ...
```

このコマンドは、現在の位置から 3 次ベジエ曲線を追加します (*x3*、 *y3*)、新しい現在位置になりますが。 点 (*x1*、 *y1*) と (*x2*、 *y2*) コントロール ポイントします。

1 つで複数のベジエ曲線を指定できます`C`コマンド。 ポイントの数は 3 の倍数である必要があります。

"Smooth"ベジエ曲線コマンドです。

```
S x2 y2 x3 y3 ...
```

(ただし、必要な厳密には)、このコマンドは通常ベジエ コマンドに従う必要があります。 スムーズ ベジエ コマンドは、相互の点を基準として前のベジエの 2 つ目の制御点を反映できるように、最初の制御点を計算します。 これら 3 つのポイントは同一線上に、そのためと、2 つのベジエ曲線の間の接続はスムーズです。

## <a name="quadto"></a>**QuadTo**

```
Q x1 y1 x2 y2 ...
```

2 次ベジエ曲線、ポイントの数は 2 の倍数である必要があります。 制御点は (*x1*、 *y1*) と終了ポイント (およびの新しい現在の位置) が (*x2*、 *y2*)

滑らかな二次曲線コマンドです。

```
T x2 y2 ...
```

制御点は、前の二次曲線の制御点に基づいて計算されます。

これらすべてのコマンドも利用「相対」のバージョンでは、座標の点は、現在の位置を基準と場所です。 これらの相対コマンドはたとえば、小文字で始まる`c`なく`C`3 次ベジエ コマンドの相対的なバージョン。

これは、SVG パス データの定義の範囲です。 コマンドのグループを繰り返し、または任意の種類の計算を実行するための機能はありません。 コマンドを使用して`ConicTo`または他の種類の円弧の仕様は利用できません。

静的な[ `SKPath.ParseSvgPathData` ](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String))メソッドは、SVG コマンドの有効な文字列を受け取ります。 構文エラーが検出されたかどうか、メソッドを返します`null`します。 これは、エラーのみを示す値です。

[ `ToSvgPathData` ](xref:SkiaSharp.SKPath.ToSvgPathData)メソッドは、既存の SVG パス データを取得するための便利な`SKPath`別のプログラムに転送するか、XML などのテキスト ベースのファイル形式で格納するオブジェクト。 (、`ToSvgPathData`メソッドは、この記事のサンプル コードでは実践しません)。*いない*期待`ToSvgPathData`パスを作成したメソッドの呼び出しに正確に対応する文字列を返します。 円弧が複数に変換される具体的には、気付く`QuadTo`コマンドから返されるパス データの表示です`ToSvgPathData`。

**パス データこんにちは**ページ魔法の単語を"HELLO"SVG パス データを使用します。 両方の`SKPath`と`SKPaint`オブジェクトがフィールドとして定義されている、 [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs)クラス。

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

(0, 0) の時点で左上隅にあるテキスト文字列を定義するパスを開始します。 それぞれの文字は 50 単位の幅と 100 単位の高さ、およびパス全体が 350 ユニット全体であることを意味する別の 25 単位で区切られます文字。

「こんにちは」の"H"は、'E' が接続されている 2 つの 3 次ベジエ曲線の 3 つの 1 行輪郭で構成されます。 なお、`C`の – 10 と 110 では、その他の文字の Y 座標の範囲外に配置する Y 座標を制御点の 2 つあり、6 つのポイントは、コマンドの後にします。 'L' が 2 つの接続されている直線中に、' o ' でレンダリングされる楕円は、`A`コマンド。

注意、`M`コマンドの最後の輪郭を開始する位置を左の垂直方向の中央のポイント (350, 50) に設定する面を ' o '。 最初の数値以下で示されている、`A`コマンド、楕円は水平方向の半径の 25、50 の垂直方向の半径。 数値の最後のペアで終点が示されます、`A`コマンドは、点 (300, 49.9) を表します。 開始点から意図的に少し異なるです。 エンドポイントを開始点と同じに設定されている場合、円弧はレンダリングされません。 完全な楕円を描画するには、設定すると、エンドポイントに近い (等しくない) が 2 つ以上を使用する必要があるか、開始点`A`ごとに完全な楕円の一部のコマンドします。

ページのコンス トラクターに次のステートメントを追加し、結果の文字列を確認するブレークポイントを設定する可能性があります。

```csharp
string str = helloPath.ToSvgPathData();
```

長い一連の円弧が置き換えられることがわかるでしょう`Q`2 次ベジエ曲線を使用して、円弧の段階的な部分の近似コマンド。

`PaintSurface`ハンドラーが、'E' のコントロール ポイントが含まれていないと、パスの緊密な境界を取得し、' 曲線 o '。 3 つの変換パスの中心をポイント (0, 0) に移動、サイズのキャンバス (また、ストロークの幅を考慮) へのパスを拡張、および、キャンバスの中央にパスの中心を移動します。

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

パスには、横モードで表示したときより適切なは、キャンバスが拡張されます。

[![](path-data-images/pathdatahello-small.png "パス データこんにちはページのスクリーン ショットをトリプル")](path-data-images/pathdatahello-large.png#lightbox "パス データこんにちはページの 3 つのスクリーン ショット")

**パス データ Cat**ページは似ています。 パスとペイントのオブジェクトが両方のフィールドとして定義されている、 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs)クラス。

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

猫の先頭が円とここで、2 つ表示される`A`コマンドをそれぞれ半円を描画します。 どちらも`A`先頭 100 の水平および垂直方向の半径を定義するのではコマンドします。 第 1 弧から始まります (240, 100) で終わります (240、300) で終了する 2 番目の円弧の始点になりますが (240, 100)。

2 つ目、2 つでも表示する`A`コマンド、猫の head を使用して、2 つ目`A`コマンドが 1 つ目の先頭として同じ時点で終了`A`コマンド。 ただし、これらのペアの`A`コマンドは、楕円を定義しないでください。 での各円弧が 40 単位あり半径 40 単位は、これらの円弧が完全な semicircles ではないことを意味します。

`PaintSurface`ハンドラーは、前のサンプルと同様の変換を実行しますが、1 つの設定`Scale`縦横比を維持し、猫のひげは画面の端を操作しないために、小さな余白を指定する要素。

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

[![](path-data-images/pathdatacat-small.png "パス データ Cat ページのスクリーン ショットをトリプル")](path-data-images/pathdatacat-large.png#lightbox "パス データ Cat ページの 3 倍になるスクリーン ショット")

通常、ときに、`SKPath`オブジェクトがフィールドとして定義されている場合、パスの輪郭をコンス トラクターまたは別の方法で定義する必要があります。 ただし、SVG パス データを使用する場合、フィールド定義で完全パスを指定できることを説明しました。

以前、**づらいアナログ時計**サンプルは、 [ **、Rotate Transform** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)記事では、単純な線として時計の針が表示されます。 **アナログ時計かなり**次のプログラムでこれらの行を置き換えます`SKPath`でフィールドとして定義されているオブジェクト、 [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs)クラスと`SKPaint`オブジェクト。

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

これで時間と分の手は領域を囲むが。 これらの手を互いから個別にするために、黒のアウトラインと塗りつぶしの灰色を使用しての両方で描画されます、`handStrokePaint`と`handFillPaint`オブジェクト。

前に示した**づらいアナログ時計**サンプルでは、小さな円、時間をマークして、分は、ループ内で描画されました。 この**アナログ時計かなり**サンプルでは、まったく異なるアプローチを使用: 時間と分のマークが点線で描画された、`minuteMarkPaint`と`hourMarkPaint`オブジェクト。

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

[**ドットし、ダッシュ**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)記事では、使用する方法について説明、 [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash*)破線を作成します。 最初の引数は、`float`を一般に 2 つの要素を持つ配列。最初の要素はダッシュの長さで、2 番目の要素はダッシュの間隔です。 ときに、`StrokeCap`プロパティに設定されて`SKStrokeCap.Round`、丸められた、破線の端がダッシュ ボードの両方の側でストロークの幅を効果的にダッシュの長さを長くします。 したがって、配列の最初の要素を 0 に設定は、点線を作成します。

これらのドットの間の距離は、2 番目の配列の要素によって制御されます。 後ほど、これら 2 つと`SKPaint`オブジェクトは、90 単位の半径の円を描画するために使用します。 この円の円周は 180π、つまり 60 分のマークが 3 π 単位ごとに表示する必要があります、これは、2 番目の値で、`float`配列`minuteMarkPaint`します。 12 時間マークは 15π 単位ごとを表示する必要があります、2 番目の値は`float`配列。

`PrettyAnalogClockPage` 16 ミリ秒ごとに、サーフェスを無効にするためのタイマーを設定するクラスと`PaintSurface`ペースがハンドラーが呼び出されます。 以前の定義、`SKPath`と`SKPaint`を許可するオブジェクトが非常にクリーンなコードを描画します。

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

ただし、2 番目の手札で行う特殊なは。 クロックが更新されるため、16 ミリ秒ごと、`Millisecond`のプロパティ、`DateTime`可能性のあるいずれかの不連続なジャンプで移動するのではなく 2 つ目は、手動スイープをアニメーション化する値を使用できます 2 番目の 2 番目の。 ただし、このコードでは、スムーズに移動することはできません。 Xamarin.Forms を使用する代わりに、 [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn)と[ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut)イージング関数の移動のさまざまな種類のアニメーション。 イージング関数によって、不自然になる方法で移動する 2 番目の手札&mdash;引いて、もう少し移動して、少し過剰撮影効果、変換先、残念ながらその再現できないこれらの静的なスクリーン ショットで前に。

[![](path-data-images/prettyanalogclock-small.png "かなりアナログ時計のページのスクリーン ショットをトリプル")](path-data-images/prettyanalogclock-large.png#lightbox "アナログ時計かなりページの 3 倍になるスクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
