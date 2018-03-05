---
title: "回転変換"
description: "影響と SkiaSharp 回転変換で実行できるアニメーションを調査します。"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: c87f9a561ac2f7a8c3da1c1e4ab839431073fcb9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="the-rotate-transform"></a>回転変換

_影響と SkiaSharp 回転変換で実行できるアニメーションを調査します。_

回転変換、SkiaSharp グラフィックス オブジェクト中断水平および垂直の軸を持つアラインメントの制約を含まない。

![](rotate-images/rotateexample.png "中央のに沿って回転したテキスト")

両方をサポートする SkiaSharp ポイント (0, 0) の周囲のグラフィカル オブジェクトを回転させる、 [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/)メソッドおよび[ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/)メソッド。

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

360 ° の円は、同じ 2 π ラジアンとしてため、2 つのユニット間で変換するは簡単です。 方が便利なを使用します。 すべての三角関数を静的に[ `Math` ](https://developer.xamarin.com/api/type/System.Math/)クラスは、ラジアン単位を使用します。

回転角度は時計回りの角度を高めるためです。 (デカルト座標システムでの回転は、慣例に反時計回りには、時計回りの回転は下方向を増やすと Y 座標と一致)。角度と角度が 360 度が許可されているよりも大きい負を値します。

回転を変換式があるの翻訳と小数点以下桁数よりも複雑です。 Α の角度での変換式があります。

x' = x•cos(α) – y•sin(α)   

y` = x•sin(α) + y•cos(α)

**基本的な回転**ページを示しています、`RotateDegrees`メソッドです。 [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs)ファイルは、ページの中央にベースラインがいくつかのテキストを表示し、基にして回転、 `Slider` –360 360 の範囲とします。 ここで、関連の一部である、`PaintSurface`ハンドラー。

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

回転の中心のほとんどの角度にこのプログラムで設定をキャンバスの左上隅にあるのため、画面をオフにテキストの回転します。

[![](rotate-images/basicrotate-small.png "トリプル ページのスクリーン ショット、基本的な回転")](rotate-images/basicrotate-large.png "トリプル ページのスクリーン ショット、基本的な回転")

ほとんどの場合、回転のこれらのバージョンを使用して、指定のピボット ポイントを中心としたものにしておく、 [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/)と[ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/)メソッド。

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**回転中央揃え**ページは、同じように、**基本的な回転**する点を除いて、拡張されたバージョンの`RotateDegrees`テキストを配置するために使用する同じポイントに回転の中心を設定するため。

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

今すぐテキストは、テキストのベースラインの水平方向の中央のテキストを配置するために使用する点を中心として回転します。

[![](rotate-images/centeredrotate-small.png "トリプル ページのスクリーン ショット、回転の中心")](rotate-images/centeredrotate-large.png "ページの回転の中心のトリプル スクリーン ショット")

中央のバージョンのと同様に、`Scale`メソッド、中央のバージョン、`RotateDegrees`呼び出しは、ショートカット。

```csharp
RotateDegrees (degrees, px, py);
```

これは、次の指定と同じです。

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

組み合わせることができますも紹介`Translate`と呼び出し`Rotate`呼び出しです。 たとえば、ここでは、`RotateDegrees`と`DrawText`で呼び出し、**回転中央揃え**ページです。

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees`呼び出しは、2 つに相当`Translate`呼び出しと非中央`RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText`を特定の場所にテキストを表示する呼び出しは等価、`Translate`後にその場所に呼び出す`DrawText`時点 (0, 0)。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

2 つの連続する`Translate`呼び出しも無効にします。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

概念的には、2 つの変換は、コードでの表示方法とは逆の順序で適用されます。 `DrawText`呼び出しでは、キャンバスの左上隅で、テキストが表示されます。 `RotateDegrees`呼び出しは、左上隅に対して相対的には、そのテキストを回転します。 続いて、`Translate`呼び出しでは、テキストをキャンバスの中央に移動します。

回転と変換を結合するいくつかの方法は通常です。 **テキストの回転**ページは次の表示を作成します。

[![](rotate-images/rotatedtext-small.png "テキストの回転 ページのスクリーン ショットをトリプル")](rotate-images/rotatedtext-large.png "テキストの回転 ページのトリプル スクリーン ショット")

ここでは、`PaintSurface`のハンドラー、 [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs)クラス。

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

`xCenter`と`yCenter`値は、キャンバスの中央を示します。 `yText`値は少しをからのオフセットします。 これには、Y 座標のページで実際に垂直方向に中央に配置されるようにテキストを配置するために必要なことを示します。 `for`ループし、キャンバスの中央を中心とする回転角度を設定します。 回転角度は、30 度ずつです。 使用して、テキストを描画、`yText`値。 "単語の前に空白の数"回転、`text`できるように、dodecagon 表示されるこれらの 12 のテキスト文字列の間の接続を作成する値が経験的に決定されます。

このコードを簡略化する方法の 1 つは 30 ° 回転角度をループの後に毎回インクリメントする、`DrawText`呼び出します。 呼び出しのために必要がある`Save`と`Restore`です。 注意して、`degrees`変数との本体内での使用されていないため、`for`ブロック。

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

単純なフォームを使用することも`RotateDegrees`への呼び出しを使用してループの先頭で`Translate`すべてをキャンバスの中央に移動します。

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

変更された`yText`計算が不要になったが組み込まれています`yCenter`です。 これで、`DrawText`呼び出しが、キャンバスの上部にある垂直方向にテキストを中央揃えです。

変換は概念的に適用されるためのコードに表示されるので、可能なまずグローバル変換、複数のローカルの変換後にでは多くの場合です。 これは、多くの場合、回転、および翻訳を結合する最も簡単な方法です。

たとえば、その軸で回転地球と同様の中心に回転するグラフィカル オブジェクトを描画するとします。 太陽の周りの回転地球と同様に、画面の中央を中心に展開するには、このオブジェクトもできます。

これは、キャンバスの左上隅にオブジェクトを配置し、その隅周りの回転アニメーションを使用して行うことができます。 次に、水平方向にはう radius のようなオブジェクトを変換します。 今すぐ原点の周囲にも、2 番目のアニメーション回転を適用します。 これにより、オブジェクト、コーナーを中心にします。 これで、キャンバスの中央に変換します。

ここでは、`PaintSurface`これらを含むハンドラーが逆の順序で呼び出しを変換します。

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

`revolveDegrees`と`rotateDegrees`フィールドがアニメーション化します。 このプログラムは、Xamarin.Forms に基づいて別のアニメーション手法`Animation`クラスです。 (このクラスについては、「[の章 22 *Xamarin.Forms を使用したモバイル アプリを作成する*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf))、`OnAppearing`オーバーライドでは、2 つ作成されます`Animation`オブジェクトをコールバック メソッドを使用し、を呼び出します`Commit`上には、アニメーションの継続時間。

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

最初の`Animation`オブジェクトをアニメーション化`revolveDegrees`0 ~ 360 ° 10 秒を超える。 2 つ目をアニメーション化`rotateDegrees`0 ~ 360 ° すべて 1 と 2 番目を無効に別の呼び出しを生成する画面、`PaintSurface`ハンドラー。 `OnDisappearing`オーバーライドは、これら 2 つのアニメーションをキャンセルします。

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**汚いアナログ時計**(と呼ばれるより魅力的なアナログ時計は、以降の記事で説明) プログラムでは使用して回転描画、分、および時間時計の針を回転させます。 プログラムでは、位置 (0, 0) にある radius 100 の中心とする円に基づいて任意の座標系を使用して、時計を描画します。 展開し、 ページで、円の中心の変換とスケーリングを使用します。

`Translate`と`Scale`呼び出しに適用されるグローバル、クロックのための初期化後に呼び出される最初のものであるもの、`SKPaint`オブジェクト。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

最後に、`PaintSurface`ハンドラーは、現在の時刻を取得し、時間、分、および 2 番目の針の回転角度を計算します。 各ハンドは 12時 00分の位置に描画される回転角度は関連するように。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

クロックは、手が粗雑ではなく、確かに機能です。

[![](rotate-images/uglyanalogclock-small.png "3 つの汚いアナログ時計のテキスト ページのスクリーン ショット")](rotate-images/uglyanalogclock-large.png "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
