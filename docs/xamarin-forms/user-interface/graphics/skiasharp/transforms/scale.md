---
title: スケールの変換
description: オブジェクトをさまざまなサイズに合わせたスケーリングの SkiaSharp スケール変換を検出します。
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: b89e7472a332ed5cd518a26bc54af59f12b19c2a
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="the-scale-transform"></a>スケールの変換

_オブジェクトをさまざまなサイズに合わせたスケーリングの SkiaSharp スケール変換を検出します。_

説明したように[、変換の変換](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)資料、平行移動の変換は、1 つの場所からグラフィカル オブジェクトを別に移動できます。 これに対し、スケールの変換は、グラフィカル オブジェクトのサイズを変更します。

![](scale-images/scaleexample.png "スケール サイズの高さ word")

スケール変換では、グラフィックスの座標を拡大できるように移動する場合も多くありますさせます。

前述の翻訳要因の影響を説明する 2 つの変換数式`dx`と`dy`:

x' = x + dx

y' = y + dy

規模の要素の`sx`と`sy`加法ではなく乗算されます。

x' sx · を =x

y' sy · を =y

翻訳の要素の既定値は 0 です。スケール ファクターの既定値は 1 です。

`SKCanvas`クラスは、次の 4 つ定義`Scale`メソッドです。 最初の[ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/)メソッドは同じ水平および垂直方向のスケーリングする場合を考慮します。

```csharp
public void Scale (Single s)
```

これと呼ばれます*アイソトロ ピック*スケーリング&mdash;拡大/縮小されている同じ双方向でします。 アイソトロ ピック スケーリングには、オブジェクトの縦横比が維持されます。

2 番目[ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/)メソッドでは、水平および垂直方向のスケーリングに別の値を指定できます。

```csharp
public void Scale (Single sx, Single sy)
```

これは、結果、*異方性*スケーリングします。
3 番目[ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/)メソッドを組み合わせて 1 つの 2 つのスケール ファクター`SKPoint`値。

```csharp
public void Scale (SKPoint size)
```

4 番目`Scale`メソッドは、間もなくはについて説明します。

**基本スケール**ページを示しています、`Scale`メソッドです。 [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) XAML ファイルには 2 つ`Slider`できる要素が 0 ~ 10 の間の水平方向および垂直方向のスケール ファクターを選択します。 [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs)分離コード ファイルでは、それらの値を使用して、呼び出す`Scale`破線で線し、サイズを左上でいくつかのテキストに合わせて角の丸い四角形を表示する前にキャンバスの上隅にあります。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

でしょうか方法はスケール ファクターには影響から返される値、`MeasureText`メソッドの`SKPaint`しますか?。 回答: まったくないです。 `Scale` メソッドは、`SKCanvas`です。 行う何らかの処理は影響しません、`SKPaint`キャンバスで何かを表示するためにそのオブジェクトを使用するまでのオブジェクトします。

わかるように後に、描画された内容、`Scale`比例して増加を呼び出します。

[![](scale-images/basicscale-small.png "基本スケール ページのスクリーン ショットをトリプル")](scale-images/basicscale-large.png#lightbox "基本スケール ページのトリプル スクリーン ショット")

テキスト、破線、コーナーと、キャンバスの左と上のエッジ、および角の丸い四角形の間の 10 ピクセルの余白の丸め、その行にダッシュの長さの幅は、同じスケール ファクターのすべてが適用されます。

> [!IMPORTANT]
> ユニバーサル Windows プラットフォームは anisotropicly スケール調節されたテキストを正しく表示されません。

水平方向および垂直の軸に揃えて配置になる行の異なるストロークの幅を原因をスケーリング異方性。 (これはもこのページでの最初のイメージから明らかです。)ストロークの幅、スケールの要因の影響を受けていない場合は、0 に設定し、1 ピクセル幅に関係なくは常に、`Scale`設定します。

スケーリング、キャンバスの左上隅に対する相対パスです。 これは、あります正確に目的の動作ができない可能性があります。 キャンバス上でテキストおよび四角形の別の場所に配置して、中心の相対サイズを変更するとします。 4 番目のバージョンを使用する場合は、 [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/)メソッドで、スケールの中央を指定する 2 つのパラメーターが含まれています。

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px`と`py`パラメーター定義と呼ぶことができる点を*center をスケーリング*、SkiaSharp でドキュメントと呼びますが、*ピボット ポイント*です。 これは、スケーリングして影響はありません、キャンバスの左上隅に対する相対ポイントです。 すべてのスケーリングは、その中心を基準としたが発生します。

[**スケールの中央揃え**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs)  ページでは、このしくみを示しています。 `PaintSurface`ハンドラーがに似ていますが、**基本スケール**プログラムのことを除いて、`margin`プログラムが縦モードでは最適ですが含まれて、テキストを水平方向に中央揃えに値が計算。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

角の丸い四角形の左上隅が配置されている`margin`キャンバスの左端からピクセルと`margin`最上部からピクセルです。 最後の 2 つの引数、`Scale`メソッドは、それらの値の幅と高さの角の丸い四角形の幅と高さがテキストに設定されます。 これはすべてスケーリングが、その四角形の中心を基準として。

[![](scale-images/centeredscale-small.png "スケールの中央に配置 ページのスクリーン ショットをトリプル")](scale-images/centeredscale-large.png#lightbox "スケールの中央に配置 ページのトリプル スクリーン ショット")

`Slider`このプログラム内の要素の範囲がある&ndash;10 ~ 10 です。 わかります、垂直方向のスケーリング (画面の中央で、Android 上など) の負の値によってスケーリングの中心を通る水平軸の周りの上下を反転するオブジェクトを使用します。 負の値 (右側の Windows の画面など) のスケーリングの水平方向のスケーリングの中心を通る垂直軸の周りの上下を反転するオブジェクトが発生します。

この 4 つ目のバージョンの`Scale`メソッドは、実際のショートカットです。 置き換えることでこの動作を確認することができます、`Scale`このコードを次のメソッド。

```csharp
canvas.Translate(-px, -py);
```

これらは、ピボット ポイント座標の符号を反転します。

そして、再びプログラムを実行します。 中央がキャンバスの左上隅になるよう、四角形やテキストが移動されたことが表示されます。 ほとんど表示できます。 今すぐプログラムでは、拡張であるために、スライダーはもちろん動作しません。

基本的なを今すぐ追加`Scale`しなくても、呼び出すスケーリング center)*する前に*を`Translate`呼び出し。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

慣れている場合がその他のグラフィックス プログラミング システム、思うかもしれませんが正しく、この手順はありません。 Skia 連続する変換の呼び出しは少し異なる方法で処理からどのような場合がありますに精通しています。

連続する`Scale`と`Translate`呼び出し、角の丸い四角形の中心は引き続き、左上隅にあるが角の丸い四角形の中心でもあると、キャンバスの左上隅に対する相対スケールすることができるようになりました。

ここで、前に、その`Scale`呼び出しは、もう 1 つ追加`Translate`中央値を使用して呼び出します。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

これは、スケール調節された結果に移動元の位置。 それら 3 つの呼び出しに相当します。

```csharp
canvas.Scale(sx, sy, px, py);
```

合計変換式が実行されるように個々 の変換が複雑化します。

 x' sx · を =(x – px) + px

 y' sy · を =(y – py) + py

注意してください、既定値の`sx`と`sy`は 1 です。 ピボット ポイント (px、py) がこれらの数式によって変換されませんをたどりに簡単です。 キャンバスの基準と同じ場所に残ります。

結合すると`Translate`と`Scale`呼び出し、順序は重要です。 場合、`Translate`の後に続く、 `Scale`、翻訳要因はスケール ファクターを効果的に拡大/縮小されます。 場合、`Translate`が前に来る、 `Scale`、翻訳要因はスケールされません。 このプロセスに内容がわかりやすくなります (せよ算術演算子) 場合の変換行列のサブジェクトが導入されました。

`SKPath`クラス定義は読み取り専用[ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/)プロパティを返す、`SKRect`パスに、座標の範囲を定義します。 たとえば、`Bounds`プロパティが以前に作成された hendecagram パスから取得した、`Left`と`Top`四角形のプロパティは、約 –100、`Right`と`Bottom`プロパティは、約 100 と`Width`と`Height`プロパティは、約 200 です。 (ほとんどの実際の値はほとんどない星のポイントが 100 の半径の円によって定義されますが、上のポイントのみが水平または垂直の軸を持つ並列ため)。

この情報の可用性は、あるスケールを派生しており、キャンバスのサイズへのパスを拡張するために適切な要素に変換することを意味します。 [**異方性スケーリング**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) 11 星のこのページを示します。 *異方性*スケールでは、されていないこと、水平方向および垂直方向、等しい、星が元の縦横比を保持しないことを意味することを意味します。 関連するコードをここでは、`PaintSurface`ハンドラー。

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds`四角形がこのコードの先頭付近にある取得およびにキャンバスの高さと幅で後で使用される、`Scale`呼び出します。 単独での呼び出しは拡大縮小パスの座標によってレポートが表示される、`DrawPath`キャンバスの右上隅の中央の呼び出しがある星型。 ダウンし、左にシフトする必要があります。 これは、ジョブの`Translate`呼び出します。 これら 2 つのプロパティの`pathBounds`は約 –100、ため、変換の要因が約 100 です。 `Translate`呼び出しが後に、`Scale`を呼び出すと、キャンバスの中央にある星型の中心を移動できるようにそれらの値が、スケーリングの要因によってスケーリング効果的に。

[![](scale-images/anisotropicscaling-small.png "トリプル ページのスクリーン ショット、異方性スケーリング")](scale-images/anisotropicscaling-large.png#lightbox "トリプル ページのスクリーン ショット、異方性のスケーリング")

別の方法を考えることができます、`Scale`と`Translate`逆の順序に影響を判断するのには、呼び出し:`Translate`呼び出しは、キャンバスの左上隅で方向が完全に表示されるようにするために、パスを移動します。 `Scale`メソッドからは、そのスター左上隅に対して相対的に大きくします。

実際には、星がキャンバスより少し大きい値が表示されます。 問題は、線の幅です。 `Bounds`プロパティ`SKPath`パスが、エンコードされた座標のディメンションとは、プログラムの使用を拡張することを示します。 特定のストロークの幅とパスが表示されると、レンダリングされたパスは、キャンバスを超えています。

この問題を解決するには、補正する必要があります。 このプログラムで 1 つの簡単な方法は、前に次のステートメント権限を追加する、`Scale`呼び出し。

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

これが増加、 `pathBounds` 1.5 ユニット 4 つの側で四角形。 これは、線の結合が丸められる場合にのみ、妥当なソリューションです。 マイター結合が長くすることを計算するが困難にします。

としてテキストを含む類似の手法を使用することもできます、**異方性テキスト**ページを示しています。 ここで、関連の一部である、`PaintSurface`からハンドラー、 [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs)クラス。

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

同様のロジックは、テキストがから返されるテキスト範囲の四角形に基づくページのサイズに拡張されます`MeasureText`(実際のテキストより少し大きい値です)。

[![](scale-images/anisotropictext-small.png "異方性テスト ページのスクリーン ショットをトリプル")](scale-images/anisotropictext-large.png#lightbox "異方性テスト ページのトリプル スクリーン ショット")

グラフィカル オブジェクトの縦横比を保持する必要がある場合にアイソトロ ピック スケーリングを使用します。 **アイソトロ ピック スケーリング**11 星のこのページを示します。 概念的には、アイソトロ ピック スケーリングを使用してページの中央にグラフィカル オブジェクトを表示するための手順です。

- 左上隅にグラフィカル オブジェクトの中心を変換します。
- グラフィック オブジェクトの次元によって分割水平および垂直方向のページの寸法の最小値に基づくオブジェクトを拡張します。
- ページの中央にスケーリングされたオブジェクトの中心を変換します。

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs)星を表示する前に、逆の順序で次の手順を実行します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

により 10% と赤から青の色を段階的に変更を組み込むことのスケーリングを小さくたびに、コードの詳細でその 10 倍も星に表示されます。

[![](scale-images/isotropicscaling-small.png "トリプル ページのスクリーン ショット、アイソトロ ピック スケーリング")](scale-images/isotropicscaling-large.png#lightbox "トリプル ページのスクリーン ショット、アイソトロ ピック スケーリング")


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
