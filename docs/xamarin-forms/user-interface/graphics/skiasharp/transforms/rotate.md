---
title: 回転変換
description: この記事では、効果とアニメーション SkiaSharp、回転変換でできることについて検討し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 1ec5c5fb1a81873d88a59eefba7652a86fc1ba4e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657212"
---
# <a name="the-rotate-transform"></a>回転変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_効果とアニメーション SkiaSharp 回転変換でできることを確認します。_

回転の変換では、SkiaSharp グラフィックス オブジェクト中断水平および垂直の軸を持つ、配置の制約の無料。

![](rotate-images/rotateexample.png "中心を回転したテキスト")

SkiaSharp 両方をサポートしている点 (0, 0) を基準としてグラフィカル オブジェクトを回転させる、 [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single))メソッドをおよび[ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single))メソッド。

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

360 度の円は 2 つのユニット間で変換しやすいように twoπ (ラジアン単位) の場合と同じです。 便利な方を使用します。 .Net では、すべての三角関数[ `Math` ](xref:System.Math)クラスは、ラジアン単位を使用します。

回転は時計回りの角度を高めるためです。 (慣例反時計回りに回転デカルト座標系にですが、時計回りの回転は SkiaSharp のように下方向に増やすと Y 座標を使用して一貫性のある)。負の角度と角度が 360 度が許可されているよりも大きい。

回転の変換式は平行移動とスケールの場合よりも複雑です。 Α の角度、変換式は。

x' = x•cos(α) – y•sin(α)   

y` = x•sin(α) + y•cos(α)

**基本的な回転**ページを示して、`RotateDegrees`メソッド。 [ **BasicRotate.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs)ファイルがページの中央にそのベースラインいくつかのテキストが表示され、に基づいてそれを回転、 `Slider` –360 360 の範囲で。 関連部分を次に示します、`PaintSurface`ハンドラー。

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

回転の中心のほとんどの角度でこのプログラムは、設定、キャンバスの左上隅にあるため、画面外にテキストの回転します。

[![](rotate-images/basicrotate-small.png "ページの基本的な回転の 3 倍になるスクリーン ショット")](rotate-images/basicrotate-large.png#lightbox "ページの基本的な回転の 3 倍になるスクリーン ショット")

これらのバージョンを使用して、指定のピボット ポイントを中心としたものを回転したい非常に多くの場合、 [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single))と[ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single))メソッド。

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**回転中心**ページと同じように、**基本的な回転**する点を除いて、拡張されたバージョンの`RotateDegrees`テキストを配置するために使用する同じ時点に回転の中心を設定するために使用します。

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

今すぐテキスト テキストのベースラインの水平方向の中心であると、テキストを配置するために使用するポイントを中心として回転します。

[![](rotate-images/centeredrotate-small.png "ページの回転の中心の 3 倍になるスクリーン ショット")](rotate-images/centeredrotate-large.png#lightbox "回転中心 ページの 3 倍になるスクリーン ショット")

中央揃えのバージョンと同様、`Scale`メソッド、中央揃えのバージョン、`RotateDegrees`呼び出しはショートカットです。 メソッドを次に示します。

```csharp
RotateDegrees (degrees, px, py);
```

その呼び出しは、次のと同等です。

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

組み合わせることができますもあることに気付く`Translate`呼び出しを`Rotate`呼び出し。 たとえば、ここでは、`RotateDegrees`と`DrawText`で呼び出し、**回転中心**ページです。

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees`呼び出しは、2 つに相当`Translate`呼び出しと非中心`RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText`特定の位置でテキストを表示する呼び出しは、`Translate`後にその場所への呼び出し`DrawText`時点 (0, 0)。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

2 つの連続する`Translate`アウト互いの呼び出しをキャンセルします。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

概念的には、2 つの変換は、コードの表示とは逆の順序で適用されます。 `DrawText`呼び出しには、キャンバスの左上隅のテキストが表示されます。 `RotateDegrees`呼び出しは、左上隅に対して相対的には、そのテキストを回転します。 次に、`Translate`呼び出しは、キャンバスの中央にテキストを移動します。

通常、回転と変換を結合するいくつかの方法は。 **テキストの回転**ページが次の表示を作成します。

[![](rotate-images/rotatedtext-small.png "テキストの回転 ページのスクリーン ショットをトリプル")](rotate-images/rotatedtext-large.png#lightbox "テキストの回転 ページの 3 倍になるスクリーン ショット")

ここでは、`PaintSurface`のハンドラー、 [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs)クラス。

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

`xCenter`と`yCenter`値は、キャンバスの中央を示します。 `yText`値は少しをオフセットします。 この値は、本当に垂直方向にページの中央に配置されるように、テキストを配置するために必要なの Y 座標です。 `for`ループし、キャンバスの中央に基づく回転を設定します。 回転は、30 度の単位です。 使用して、テキストを描画、`yText`値。 「回転」を単語の前に空白の数、`text`できるように、dodecagon 表示されるこれらの 12 のテキスト文字列の間の接続を作成する値が経験的に決定されます。

このコードを簡略化する 1 つの方法が 30 度の回転角度をした後、ループを通過するたびにインクリメントするには、`DrawText`呼び出します。 これを呼び出す必要がある`Save`と`Restore`します。 なお、`degrees`の本文内で変数が使用されなく、`for`ブロック。

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

単純なフォームを使用することも`RotateDegrees`への呼び出しを使用してループを前につける`Translate`すべてをキャンバスの中央に移動します。

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

変更された`yText`計算が不要になったが組み込まれて`yCenter`します。 これで、`DrawText`呼び出しは、キャンバスの上部にある垂直方向にテキストを中央揃え。

コードの表示とは逆の変換に適用される概念的には、ためには、多くの場合、ローカルの複数の変換後に、まず初めに多くのグローバル潜在的な変換。 これは、多くの場合、回転と変換を結合する最も簡単な方法です。

たとえば、その軸に回転する地球と同様に、その中心の周りに回転するためのグラフィカル オブジェクトを描画するとします。 太陽の周りを回転する地球と同様に、画面の中央を中心に展開するには、このオブジェクトもします。

これは、キャンバスの左上隅にオブジェクトを配置し、その隅の周りの回転をアニメーションを使用して行うことができます。 次に、回転のこぎりの radius のように水平方向にオブジェクトを変換します。 原点の周囲にも、2 番目のアニメーション化された回転を適用するようになりました。 これにより、コーナーに焦点を絞ってオブジェクトです。 キャンバスの中央に変換されるようになりました。

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

`revolveDegrees`と`rotateDegrees`フィールドがアニメーション化します。 このプログラムは、Xamarin.Forms に基づく別のアニメーションの手法を使用して[ `Animation` ](xref:Xamarin.Forms.Animation)クラス。 (このクラスについては、「[の第 22 章*Xamarin.Forms での Mobile Apps の作成*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf))、`OnAppearing`オーバーライドでは、2 つ作成されます`Animation`オブジェクトをコールバック メソッドをおよびを呼び出します`Commit`上には、アニメーションの継続時間。

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

最初の`Animation`オブジェクトをアニメーション化`revolveDegrees`から 360 度 10 秒を超える 0 度です。 2 つ目のアニメーション化`rotateDegrees`0 度 360 度からすべて 1 と 2 つ目を無効に別の呼び出しを生成する画面、`PaintSurface`ハンドラー。 `OnDisappearing`オーバーライドは、これら 2 つのアニメーションを取り消します。

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**づらいアナログ時計**クロックの分、時間のマークを描画して、手を回転するプログラム (と呼ばれるもより魅力的なアナログ時計は、今後の記事で説明) が回転を使用します。 プログラムでは、位置 (0, 0) にある半径が 100 の中心とする円に基づいて任意の座標系を使用して、クロックを描画します。 展開し、ページで、円の中心の変換とスケーリングを使用します。

`Translate`と`Scale`呼び出しは、クロックにグローバルに適用の初期化後に呼び出される最初のものであるものされるため、`SKPaint`オブジェクト。

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
```

円の中に、常に描画する必要が 2 つの異なるサイズの 60 のマークがあります。 `DrawCircle`呼び出しは、クロックの中心を基準とした 12時 00分に対応するポイント (0, –90) にある円を描画します。 `RotateDegrees`呼び出しごとに目盛りマーク後 6 度の回転角度をインクリメントします。 `angle`変数は、大きな円または小さな円が描画されるかどうかを判断するためだけに使用されます。

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

最後に、`PaintSurface`ハンドラーは、現在の時刻を取得し、時間、分、および 2 つ目の手の回転角度を計算します。 各手は 12時 00分の位置に描画される回転角度は、関連したように。

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

クロックが、手はかなりおおざっぱ確実に機能します。

[![](rotate-images/uglyanalogclock-small.png "3 倍に見づらいアナログ時計のテキスト ページのスクリーン ショット")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

魅力的な制では、記事を参照してください。 [ **SkiaSharp の SVG パス データ**](../curves/path-data.md)します。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
