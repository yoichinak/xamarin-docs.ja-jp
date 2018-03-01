---
title: SVG Path Data
description: "文字列形式を使用して、スケーラブル ベクター グラフィックス パスを定義します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: feb4c5f4c7e7ad3fc5f762786001be9aa57ae718
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="svg-path-data"></a>SVG Path Data

_文字列形式を使用して、スケーラブル ベクター グラフィックス パスを定義します。_

`SKPath`クラスは、スケーラブル ベクター グラフィックス (SVG) の仕様によって確立された形式でテキスト文字列からのパス全体でオブジェクトの定義をサポートしています。 どのテキスト文字列のようにパス全体を表すことができるこの記事で後述表示されます。

![](path-data-images/pathdatasample.png "SVG パスのデータで定義されているパスのサンプル")

SVG は、XML ベースのグラフィックス プログラミング言語の web ページです。 SVG は、一連の関数呼び出しではなく、マークアップで定義されているへのパスを許可する必要があります、ために、標準の SVG には、全体のグラフィックス パスを指定するテキスト文字列としてのため、非常に簡潔な方法が含まれています。

SkiaSharp、内でこの形式は、データとして参照"SVG パス -" Windows の XAML ベースのプログラミング環境、Windows Presentation Foundation などと呼ばれますが、ユニバーサル Windows プラットフォームで、形式がサポートされても、[パス マークアップの構文](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx)または[移動描画コマンド構文と](/windows/uwp/xaml-platform/move-draw-commands-syntax/)です。 ベクター グラフィックス イメージは、XML などのテキスト ベースのファイルで特に exchange 形式としてにも役立ちます。

SkiaSharp 言葉で 2 つのメソッドを定義する`SvgPathData`名前にします。

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

静的[ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/)メソッドに文字列に変換、`SKPath`オブジェクト、中に[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/)変換、`SKPath`オブジェクトを文字列。

ポイントを中心と、(0, 0)、radius で 100 の 5 ポイントの星型の SVG 文字列を次に示します。

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

文字が構築されるコマンド、`SKPath`オブジェクト。 `M` 示します、`MoveTo`を呼び出すと、`L`は`LineTo`、および`Z`は`Close`輪郭を閉じます。 番号の各ペアは、点の X と Y 座標を提供します。 注意して、`L`コマンドにはコンマで区切られた複数のポイントが続きます。 一連の座標とポイント、コンマと空白で同一に扱われます。 一部のプログラマは、点の間ではなく、X 座標と Y 座標の間にコンマを配置するが、あいまいさを避けるには、カンマ、スペースは必要なだけです。 これは、機能は、完全に有効です。

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG パスのデータの構文がで正式に記載されている[SVG 仕様のセクションの 8.3](http://www.w3.org/TR/SVG11/paths.html#PathData)です。 次に概要を示します。

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

これは、現在の位置を設定して、パスに新しい輪郭を開始します。 パスのデータの先頭でなければなりません常に、`M`コマンド。

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

このコマンドは、パスに垂直または水平 (または行) を追加し、最後の行の末尾に新しい現在位置を設定します。 行うことができる、`L`コマンドの複数のペアと*x*と*y*座標。

## <a name="horizontal-lineto"></a>**水平 LineTo**

```csharp
H x ...
```

このコマンドは、パスに水平な線を追加し、行の末尾に新しい現在位置を設定します。 行うことができる、`H`複数のコマンド*x*座標、ですが、無意味です。

## <a name="vertical-line"></a>**垂直線**

```csharp
V y ...
```

このコマンドは、パスにある垂直線を追加し、行の末尾に新しい現在位置を設定します。

## <a name="close"></a>**閉じる**

```csharp
Z
```

`C`コマンドは、輪郭の先頭に現在の位置から、直線を追加することによって、輪郭を閉じます。

## <a name="arcto"></a>**ArcTo**

楕円の円弧を曲線に追加するコマンドは、はるかには、全体の SVG パス データ仕様で最も複雑なコマンドです。 数値できますを表すが座標の値以外のものだけで、コマンドは。

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx*と*概要*パラメーターは、楕円の水平方向および垂直方向の半径。 *回転角度*は角度の時計回りにします。

設定、*大きな円弧フラグ*大きな円弧を 1 に、または小さい円弧を 0 にします。

設定、*スイープ フラグ*1 のまで、時計回りおよびを 0 に反時計回りにします。

ポイントに円弧が描画される (*x*、 *y*)、新しい現在位置になっています。

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

このコマンドでは、3 次ベジエ曲線を追加する現在の位置から (*x3*、 *y3*)、新しい現在位置になっています。 点 (*x1*、 *y1*) と (*x2*、 *y2*) コントロール ポイントします。

複数のベジエ曲線を 1 つで指定できる`C`コマンド。 ポイントの数は、3 の倍数である必要があります。

"Smooth"のベジエ曲線コマンドです。

```csharp
S x2 y2 x3 y3 ...
```

(ただし、ことが必要ではありません)、このコマンドは通常ベジエ コマンドに従う必要があります。 平滑ベジエ コマンドは、相互の点を中心以前ベジエの 2 番目の制御点を反映できるように、最初の制御点を計算します。 これら 3 つのポイントが同一線上に、そのため、2 つのベジエ曲線の間の接続は smooth です。

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

2 次ベジエ曲線、ポイントの数は 2 の倍数である必要があります。 コントロール ポイント (*x1*、 *y1*) と終了点 (と新しい現在位置) を (*x2*、 *y2*)

二次曲線を滑らかなコマンドもです。

```csharp
T x2 y2 ...
```

コントロール ポイントは、以前の二次曲線の制御点に基づいて計算されます。

これらすべてのコマンドも使用できます「相対」のバージョンでは、座標点は、現在の位置、場所。 これらの相対コマンドがたとえば小文字のアルファベットで始まる`c`なく`C`3 次ベジエ コマンドの相対的なバージョンです。

これは、SVG パス データ定義のエクステントです。 コマンドのグループを繰り返し、または任意の種類の計算を実行するための機能はありません。 コマンド`ConicTo`または他の種類の円弧の仕様は利用できません。

静的な[ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/)メソッドが SVG コマンドの有効な文字列が必要です。 かどうか、構文エラーが検出されると、メソッドを返します`null`です。 エラーのみを示す値です。

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/)メソッドは、既存の SVG パス データを取得するための便利な`SKPath`を別のプログラムを転送するか、XML などのテキスト ベースのファイル形式で格納するオブジェクト。 (、`ToSvgPathData`メソッドは、この記事のサンプル コードではデモンストレーションしません)。*いない*期待`ToSvgPathData`メソッドの呼び出しパスの作成を正確に対応する文字列を返します。 円弧が複数に変換される具体的には、紹介`QuadTo`コマンドから返されるパス データでの表示方法を`ToSvgPathData`です。

**パス データこんにちは**ページ魔法単語の"HELLO"SVG パス データを使用します。 両方の`SKPath`と`SKPaint`オブジェクトがフィールドのフィールドとして定義されている、 [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs)クラス。

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

テキスト文字列を定義するパスは、左上隅にある位置 (0, 0) に開始します。 各文字はワイド 50 ユニットと 100 単位の高さ、および文字で区切られます別 25 単位、つまりパス全体が 350 単位の幅。

「こんにちは」の 'H' では、'E' が 2 つの接続されている 3 次ベジエ曲線に、次の 3 つの 1 行輪郭ので構成されます。 注意して、`C`コマンドには 6 個のポイントが続くし、– 10 およびその他の文字の Y 座標の範囲外に格納される 110 の Y 座標にある 2 つの制御点。 'L' が 2 つの接続されている直線中に、' o ' でレンダリングされる楕円は、`A`コマンド。

注意して、`M`最後の輪郭を開始するコマンドは、左側の垂直方向の中央のポイント (350, 50) に位置を設定面を 'o' です。 最初の数字以下に従い、`A`コマンド、省略記号ボタンがあり、25 の水平方向の半径 50 の垂直方向の半径。 内の番号の最後のペアによって終了点が示されている、`A`コマンドでは、点 (300, 49.9) を表します。 開始ポイントから少しだけ意図的に異なるです。 エンドポイントを開始点と同じに設定されている場合、円弧はレンダリングされません。 完全な楕円を描画するには、設定すると、エンドポイントに近い (ただし、等しくない) の 2 つ以上を使用する必要があります、始点または`A`完全な楕円の一部の各コマンドします。

ページのコンス トラクターに次のステートメントを追加し、結果の文字列を確認するブレークポイントを設定する場合があります。

```csharp
string str = helloPath.ToSvgPathData();
```

長い一連の円弧が置き換えられていることに気付く`Q`2 次ベジエ曲線を使用して、円弧の段階的な部分の近似コマンド。

`PaintSurface`ハンドラーは、パスは 'E' のコントロール ポイントが含まれないの緊密な境界を取得し、' 曲線 o ' です。 3 つの変換パスの中心を (0, 0) のポイントに移動、サイズのキャンバス (もストロークの幅を考慮に入れて) へのパスを拡張、および、パスの中心をキャンバスの中央に移動します。

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

パスが横モードで表示したときより適切なキャンバスを塗りつぶします。

[![](path-data-images/pathdatahello-small.png "パス データこんにちはページのスクリーン ショットをトリプル")](path-data-images/pathdatahello-large.png "パス データこんにちはページの 3 倍のスクリーン ショット")

**パス データ Cat**ページは似ています。 パスとペイントのオブジェクトが両方のフィールドとして定義されている、 [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs)クラス。

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

Cat の先頭が円とここで、レンダリングには 2 つ`A`半円を描画するの各コマンド。 両方`A`コマンド ヘッドが 100 の水平方向および垂直方向の半径を定義します。 最初の円弧の始点 (240, 100) で終わります (240、300) で終了する 2 番目の円弧の始点が (240, 100)。

2 つ目は、2 つでは表示も`A`コマンド、猫のヘッドを使用して、2 つ目`A`コマンドが最初の開始位置として同じ位置で終わって`A`コマンド。 ただし、これらのペアの`A`コマンドは楕円を定義していません。 各円弧の 40 単位であり、radius も 40 ユニットは、これら円弧が完全 semicircles でないことを意味します。

`PaintSurface`ハンドラーの前のサンプルと同様の変換を実行できますが、1 つを設定`Scale`縦横比を維持し、cat のひげ操作画面の辺はできないために、ほとんどの余白を提供する要素。

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

3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![](path-data-images/pathdatacat-small.png "パス データ Cat ページのスクリーン ショットをトリプル")](path-data-images/pathdatacat-large.png "パス データ Cat ページのトリプル スクリーン ショット")

通常、ときに、`SKPath`オブジェクトがフィールドとして定義されている場合、コンス トラクターまたは別の方法でパスの輪郭を定義する必要があります。 ただし、SVG パス データを使用する場合、フィールド定義の完全パスを指定できることを見てきました。

以前**汚いアナログ時計**サンプルは、 [ **、回転変換**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)記事では、単純な行として、時計の針が表示されます。 **アナログ時計かなり**以下のプログラムでこれらの行を置き換えます`SKPath`でフィールドとして定義されているオブジェクト、 [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs)クラスと`SKPaint`オブジェクト。

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

時間と分の手が囲まれています、手をそれぞれ区別するために、領域、黒い枠線と灰色の塗りつぶしの使用の両方で描画されます、`handStrokePaint`と`handFillPaint`オブジェクト。

前に示した**汚いアナログ時計**サンプルについては、時間をマークされている円、もう少しと分は、ループ内で描画されました。 この**アナログ時計かなり**サンプルを完全に異なるアプローチを使用して: 時間と分のマークが点線で描画された、`minuteMarkPaint`と`hourMarkPaint`オブジェクト。

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

[**ドットし、ダッシュ**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)ガイドを使用する方法を説明した、`SKPathEffect.CreateDash`点線を作成します。 最初の引数は、`float`を一般的に 2 つの要素を持つ配列: 最初の要素はダッシュの長さと 2 番目の要素はダッシュの差。 ときに、`StrokeCap`プロパティに設定されている`SKStrokeCap.Round`、ダッシュ ボードの角の丸い端効果的に長くなる可能性がダッシュの長さダッシュの両方の側で線幅によって、します。 したがって、配列の最初の要素を 0 に設定すると、点線が作成されます。

これらのドットの間の距離は、2 番目の配列要素によって制御されます。 後ほど、これら 2 つと`SKPaint`オブジェクトを使用して、半径が 90 のユニットの円を描画します。 この円の円周あるため、180π、つまり、60 分のマークがすべて 3 π 単位を表示する必要があります、これは、2 番目の値で、`float`配列`minuteMarkPaint`です。 12 時間マーク必要がありますすべて 15π ユニット、2 番目の値である`float`配列。

`PrettyAnalogClockPage` 、16 ミリ秒ごとに、画面を無効にするタイマーを設定するクラスと`PaintSurface`速度、ハンドラーが呼び出されます。 以前の定義、`SKPath`と`SKPaint`オブジェクトでは、非常にクリーンな描画コード。

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

ただし、秒針で行わ特別なれます。 クロックが更新されるため、16 ミリ秒ごと、`Millisecond`のプロパティ、`DateTime`不連続なジャンプで移動する 1 つではなく、手動で 2 つ目、スイープをアニメーション化する値を使用する可能性が秒を 2 番目のです。 このコードでは、スムーズに移動することはできません。 代わりに、Xamarin.Forms を使用して[ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/)と[ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)アニメーションにイージング関数の移動の別の種類。 これらのイージング関数と秒針不自然になる方法 & #x 2014; に移動するには引いて、もう少しに移動し、少し過剰撮影目的地エフェクト残念ながらを再現できない静的これらスクリーン ショットでは前に。

[![](path-data-images/prettyanalogclock-small.png "アナログ時計かなりページのスクリーン ショットをトリプル")](path-data-images/prettyanalogclock-large.png "アナログ時計かなりページのトリプル スクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
