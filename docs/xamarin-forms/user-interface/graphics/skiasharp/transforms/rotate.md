---
title: 回転変換
description: この記事では、SkiaSharp rotate 変換で可能な効果とアニメーションについて説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eb0c8bd01c9cab8a4048c0ee3deacad7afd56899
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373893"
---
# <a name="the-rotate-transform"></a>回転変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp rotate 変換で可能な効果とアニメーションを調べる_

回転変換を使用すると、SkiaSharp グラフィックスオブジェクトは、水平軸と垂直軸を使用した配置の制約を解除します。

![中心を中心に回転したテキスト](rotate-images/rotateexample.png)

ポイント (0, 0) を中心にしてグラフィカルオブジェクトを回転させる場合、SkiaSharp ではメソッドとメソッドの両方がサポートされ [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) ます。

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

360°の円は2πラジアンと同じであるため、2つの単位の間で簡単に変換できます。 任意の便利なを使用します。 .NET クラスのすべての三角関数は、 [`Math`](xref:System.Math) ラジアン単位を使用します。

回転角度は時計回りに回転します。 (デカルト座標系の回転は慣例によって反時計回りにされていますが、時計回りの回転は Y 座標と同じであり、SkiaSharp のようになります)。360°を超える負の角度と角度が許可されます。

回転の変換式は、平行移動および小数点以下桁数よりも複雑です。 Αの角度の場合、変換式は次のようになります。

x ' = x • cos (α) – y • sin (α)   

y ' = x • sin (α) + y • cos (α)

[ **基本的な回転** ] ページでは、メソッドを示し `RotateDegrees` ます。 [**BasicRotate.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs)ファイルには、ページの中央にベースラインを持つテキストがいくつか表示され、-360 ~ 360 の範囲のを基にして回転し `Slider` ます。 ハンドラーの関連する部分を次に示し `PaintSurface` ます。

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

回転はキャンバスの左上隅を中心としているため、このプログラムで設定されているほとんどの角度では、テキストは画面から回転します。

[![基本的な回転ページのトリプルスクリーンショット](rotate-images/basicrotate-small.png)](rotate-images/basicrotate-large.png#lightbox "基本的な回転ページのトリプルスクリーンショット")

これらのバージョンのメソッドとメソッドを使用して、指定されたピボットポイントを中心に回転することがよくあり [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) ます。

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**中央回転** のページは **基本的な回転** と同じですが、展開されたのバージョンを使用して、 `RotateDegrees` 回転の中心が、テキストの配置に使用されるのと同じポイントに設定されている点が異なります。

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

テキストは、テキストの配置に使用した点を中心に回転します。これは、テキストのベースラインの水平方向の中心です。

[![中央回転ページのトリプルスクリーンショット](rotate-images/centeredrotate-small.png)](rotate-images/centeredrotate-large.png#lightbox "中央回転ページのトリプルスクリーンショット")

中心となるバージョンのメソッドと同様 `Scale` に、呼び出しの中心と `RotateDegrees` なるバージョンはショートカットです。 メソッドを次に示します。

```csharp
RotateDegrees (degrees, px, py);
```

この呼び出しは、次の場合と同じです。

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

呼び出しと呼び出しを組み合わせることもでき `Translate` `Rotate` ます。 たとえば、次に示すのは、 `RotateDegrees` `DrawText` 中央の **回転** ページのとです。

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

この `RotateDegrees` 呼び出しは、2つ `Translate` の呼び出しと、中央ではないに相当し `RotateDegrees` ます。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText`特定の場所でテキストを表示する呼び出しは、 `Translate` その場所の呼び出しと、その後に `DrawText` ポイント (0, 0) を指定した場合と同じです。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

2つの連続した呼び出しは、互いにキャンセルされ `Translate` ます。

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

概念的には、2つの変換は、コード内での表示方法と逆の順序で適用されます。 この呼び出しによって、 `DrawText` キャンバスの左上隅にテキストが表示されます。 この `RotateDegrees` 呼び出しは、左上隅に対して相対的にテキストを回転させます。 その後、呼び出しによって `Translate` テキストがキャンバスの中央に移動します。

通常、回転と平行移動を組み合わせるにはいくつかの方法があります。 回転した **テキスト** ページでは、次の表示が作成されます。

[![回転したテキストページのトリプルスクリーンショット](rotate-images/rotatedtext-small.png)](rotate-images/rotatedtext-large.png#lightbox "回転したテキストページのトリプルスクリーンショット")

`PaintSurface`クラスのハンドラーを次に示し [`RotatedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) ます。

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

との値は、 `xCenter` `yCenter` キャンバスの中心を示します。 `yText`値は、そのからの小さなオフセットです。 この値は、ページ上で垂直方向に中央揃えになるようにテキストを配置するために必要な Y 座標です。 次に、このループでは、 `for` キャンバスの中央に基づいて回転を設定します。 回転は30°単位で計算されます。 テキストは、値を使用して描画され `yText` ます。 値の "ROTATE" という単語の前にある空白の数は、 `text` これらの12個のテキスト文字列間の接続を dodecagon として表示するために経験的によって決定されました。

このコードを簡略化する方法の1つとして、呼び出しの後にループを実行するたびに、回転角度を30度ずつインクリメントする方法が `DrawText` あります。 これにより、およびを呼び出す必要がなくなり `Save` `Restore` ます。 `degrees`ブロックの本体内で変数が使用されなくなったことに注意して `for` ください。

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

また、単純な形式のを使用することもできます。これを行うには、 `RotateDegrees` を呼び出して、 `Translate` キャンバスの中央にすべて移動します。

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

変更された `yText` 計算は組み込まれなくなりました `yCenter` 。 これで、 `DrawText` テキストがキャンバスの上部に中央揃えで配置されます。

変換は、コード内での表示方法とは逆に適用されるため、多くの場合、よりグローバルな変換を行い、その後により多くのローカル変換を行うことができます。 これは、多くの場合、回転と平行移動を組み合わせる最も簡単な方法です。

たとえば、中心を中心に回転するグラフィカルオブジェクトを、その軸で回転する地球のように描画するとします。 しかし、このオブジェクトは、太陽の周りにある地球のように、画面の中央を中心にして回転させることもできます。

これを行うには、キャンバスの左上隅にオブジェクトを配置し、アニメーションを使用してそのコーナーを回転します。 次に、回転 radius のようにオブジェクトを水平方向に変換します。 次に、原点を中心とした2番目のアニメーションの回転を適用します。 これにより、オブジェクトがコーナーを中心に回転します。 次に、キャンバスの中央に平行移動します。

`PaintSurface`これらの変換呼び出しを逆の順序で含むハンドラーを次に示します。

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

`revolveDegrees`フィールドと `rotateDegrees` フィールドはアニメーション化されます。 このプログラムでは、クラスに基づくさまざまなアニメーション手法を使用 Xamarin.Forms [`Animation`](xref:Xamarin.Forms.Animation) します。 (このクラスについては、「 [*を使用 Xamarin.Forms した Mobile Apps の作成* 」の22章](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)で説明されてい `OnAppearing` ます)。オーバーライドは、コールバックメソッドを持つ2つのオブジェクトを作成し、 `Animation` それらのオブジェクトに対してアニメーションの `Commit` 継続時間を

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

最初の `Animation` オブジェクトは、 `revolveDegrees` 0 ~ 360 °の10秒間をアニメーション化します。 2番目のアニメーションは、 `rotateDegrees` 0 °から360度に1秒ごとにアニメーション化され、さらに、ハンドラーへの別の呼び出しを生成するためのサーフェイスも無効に `PaintSurface` なります。 オーバーライドは、 `OnDisappearing` 次の2つのアニメーションをキャンセルします。

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

見 **づらいアナログクロック** プログラム (これは、より魅力的なアナログクロックが後の記事で説明されています) では、回転を使用して時計の分と時間のマークを描画し、ハンドを回転させます。 このプログラムは、ポイント (0, 0) の中心が100である円に基づいて、任意の座標系を使用して時計を描画します。 平行移動と拡大縮小を使用して、ページ上の円を拡大し、中央に配置します。

との呼び出しは、 `Translate` `Scale` クロックにグローバルに適用されるため、オブジェクトの初期化に従って最初に呼び出されるものです `SKPaint` 。

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

時計の周りに円を描く必要がある2つの異なるサイズのマークが60あります。 この `DrawCircle` 呼び出しは、クロックの中心に対して相対的に12:00 に対応する点 (0, – 90) に円を描画します。 この呼び出しでは、 `RotateDegrees` すべての目盛りの後に、回転角度が6度ずつインクリメントされます。 変数は、 `angle` 大きな円または小さい円が描画されるかどうかを判断するためだけに使用されます。

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

最後に、 `PaintSurface` ハンドラーは現在の時刻を取得し、時間、分、および秒の針の回転角度を計算します。 各針は12:00 の位置に描画されるため、回転角度は次のようになります。

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

時計は確かに機能しますが、実際にはむしろ、

[![見づらいアナログ時計のテキストページのトリプルスクリーンショット](rotate-images/uglyanalogclock-small.png)](rotate-images/uglyanalogclock-large.png#lightbox "見づらいアナログページのトリプルスクリーンショット")

より魅力的なクロックについては、「 [**SkiaSharp の SVG パスデータ**](../curves/path-data.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)