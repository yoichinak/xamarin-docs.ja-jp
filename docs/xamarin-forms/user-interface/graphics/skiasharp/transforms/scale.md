---
title: スケール変換
description: Thhis 記事では、オブジェクトをさまざまな規模を拡張するための SkiaSharp スケール変換について説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 9bc320273df192f9daf2520f451601335731e7b0
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061353"
---
# <a name="the-scale-transform"></a>スケール変換

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_SkiaSharp のスケール変換オブジェクトをさまざまなサイズを拡張するための検出します。_

説明したように[ **、変換の変換**](translate.md)記事では、平行移動変換は、1 つの場所からグラフィカル オブジェクトを別に移動できます。 これに対し、スケール変換は、グラフィカル オブジェクトのサイズを変更します。

![](scale-images/scaleexample.png "サイズにスケーリング (縦) word")

スケール変換には、大規模な実行時に移動するグラフィックス座標も多くの場合とします。

要素の変換の効果を記述する 2 つの変換式は前述`dx`と`dy`:

x' = x + dx

y' = y + dy

スケール ファクターの`sx`と`sy`加法ではなく乗算されます。

x' = sx ・ x

y' = sy ・ y

翻訳の要素の既定値は 0 です。スケール ファクターの既定値は、1 です。

`SKCanvas`クラスは、4 つ定義`Scale`メソッド。 最初の[ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single))メソッドは、同じ水平および垂直スケーリングする場合を考慮します。

```csharp
public void Scale (Single s)
```

呼ばれます*アイソトロ ピック*スケーリング&mdash;でのスケーリングは、同じ双方向。 アイソトロ ピック スケーリングには、オブジェクトの縦横比が保持されます。

2 番目の[ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single))メソッドを使用して、水平および垂直方向のスケーリングに別の値を指定できます。

```csharp
public void Scale (Single sx, Single sy)
```

これは、結果、*異方性*スケーリングします。
3 番目[ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint))メソッドは、1 つの 2 つのスケール ファクターを組み合わせた`SKPoint`値。

```csharp
public void Scale (SKPoint size)
```

4 番目`Scale`メソッドはまもなく説明します。

**基本的なスケール**ページを示して、`Scale`メソッド。 [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml)ファイルには 2 つ`Slider`できる要素が 0 から 10 までの水平および垂直方向のスケール ファクターを選択します。 [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs)分離コード ファイルでは、これらの値を使用して、呼び出す`Scale`破線が付いている線を付けるし、サイズを左上でいくつかのテキストに合わせて角丸四角形を表示する前にキャンバスの下隅。

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

疑問に思うかもしれません。 はどのようにスケール ファクター影響から返される値、`MeasureText`メソッドの`SKPaint`でしょうか。 答え: いいえ、違います。 `Scale` メソッドは、`SKCanvas`します。 行う何らかの処理は影響しません、`SKPaint`キャンバスで何かを表示するためにそのオブジェクトを使用するまでのオブジェクトします。

ご覧のとおり、すべての後に描画、`Scale`比例して増加を呼び出します。

[![](scale-images/basicscale-small.png "基本的なスケール ページのスクリーン ショットをトリプル")](scale-images/basicscale-large.png#lightbox "基本的なスケール ページの 3 倍になるスクリーン ショット")

テキスト、破線、隅と、キャンバスの左と上の端と角丸四角形の間の 10 ピクセルの余白の丸め、その行にダッシュの長さの幅はすべて同じスケール ファクター。

> [!IMPORTANT]
> ユニバーサル Windows プラットフォームは anisotropicly 拡大縮小されたテキストを正しく表示されません。

水平および垂直の軸に揃えになる別の線のストロークの幅をエラーの原因をスケーリング異方性。 (これはもこのページの最初の図からわかります) です。スケーリングの要因によって影響を受けるストロークの幅をしたくない場合は 0 に設定しに関係なく 1 ピクセルは常に、`Scale`設定します。

スケーリングは、キャンバスの左上隅に対して相対的です。 これは厳密にあります目的の動作ができない可能性があります。 キャンバスで、テキストと四角形の別の場所に移動して、その中心を基準としたサイズを変更するとします。 4 番目のバージョンを使用する場合、 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single))メソッドで、スケーリングの中心を指定する 2 つのパラメーターが含まれています。

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px`と`py`パラメーターとも呼ばれるポイントを定義する、*センターをスケーリング*、SkiaSharp でドキュメントと呼びますが、*ピボット ポイント*します。 これは、スケーリングして影響はありませんが、キャンバスの左上隅に対する相対ポイントです。 すべてのスケーリングは、その中心を基準に発生します。

[**スケールの中央に**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs)  ページでは、この動作を示します。 `PaintSurface`ハンドラーと似ています、**基本的なスケール**プログラムのことを除いて、`margin`縦モードでプログラムが最適に動作することを意味するテキストを水平方向に中央値が計算されます。

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

角丸四角形の左上隅が配置されている`margin`、キャンバスの左側からのピクセルと`margin`上部 (ピクセル)。 最後の 2 つの引数、`Scale`メソッドは、これらの値と幅との角の丸い四角形の幅と高さにもには、テキストの高さに設定されます。 つまり、すべてのスケーリングが四角形の中央を基準。

[![](scale-images/centeredscale-small.png "[スケールの中央に配置] ページのスクリーン ショットをトリプル")](scale-images/centeredscale-large.png#lightbox "スケールの中央に配置 ページの 3 倍になるスクリーン ショット")

`Slider`このプログラム内の要素の範囲がある&ndash;10 ~ 10。 ご覧のように、負の値の垂直スケール (画面の中央で、android など) が発生するスケーリングの中心を通過する水平方向の軸を中心に反転するオブジェクト。 水平スケーリング (右側の UWP 画面など) の負の値では、スケーリングの中心を通過する垂直軸を中心に反転するオブジェクトが発生します。

バージョン、 [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single))ピボット ポイントを持つメソッドが 3 つの系列のショートカット`Translate`と`Scale`呼び出し。 置き換えることで、この動作を確認したい場合があります、`Scale`メソッドで、**スケールの中央に**を次のページ。

```csharp
canvas.Translate(-px, -py);
```

これらは、ピボット ポイントの座標の符号を反転します。

そして、再びプログラムを実行します。 中央がキャンバスの左上隅になるよう、四角形やテキストがシフトしたことを確認します。 ほんの少し確認できます。 スライダーは、プログラムがすべての拡張できないようになりましたので、もちろん機能しません。

基本的なを今すぐ追加`Scale`(スケーリング センター) せずに呼び出す*する前に*を`Translate`呼び出し。

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

慣れている場合にその他のグラフィックスのシステムをプログラミングで、問題のあるを考える場合がありますが、この演習はありません。 Skia 連続する変換の呼び出し、少し異なる方法で処理からどのような場合があります理解してでしょう。

連続する`Scale`と`Translate`左上隅で呼び出し、角丸四角形の中心は、角丸四角形の中心でもあると、キャンバスの左上隅に対する相対スケールすることができますようになりました。

その前に、`Scale`呼び出し、もう 1 つ追加`Translate`中央値を使用して呼び出します。

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

これは、スケールの結果に移動元の位置。 これら 3 つの呼び出しと同等のとおりです。

```csharp
canvas.Scale(sx, sy, px, py);
```

合計変換式が実行されるよう、個々 の変換が複雑化します。

 x' = sx ・ (x ? px) + px

 y' = sy ・ (y ? py) + py

注意の既定値`sx`と`sy`は 1。 これらの数式、ピボット ポイント (px、py) が変換されませんをたどりやすいになります。 これは、キャンバスの基準と同じ場所に残ります。

結合すると`Translate`と`Scale`呼び出し、順序は重要です。 場合、`Translate`の後に続く、`Scale`翻訳の要因はスケール ファクターで効果的に拡大/縮小されます。 場合、`Translate`の前に、`Scale`翻訳の要因をスケールできません。 このプロセスが少し明確になります (ただしより数学的な) の変換行列のサブジェクトを導入したとき。

`SKPath`クラス定義の読み取り専用[ `Bounds` ](xref:SkiaSharp.SKPath.Bounds)プロパティを返す、`SKRect`パスで、座標の範囲を定義します。 たとえば、`Bounds`プロパティが以前に作成した hendecagram パスから取得した、`Left`と`Top`四角形のプロパティは、約 –100、`Right`と`Bottom`プロパティは、約 100 と`Width`と`Height`プロパティは、約 200 です。 (ほとんどの実際の値はほとんどない星のポイントが 100 の半径の円で定義されているが、上のポイントのみが水平または垂直の軸を持つ並列ため)。

この情報の可用性は、スケールを派生し、キャンバスのサイズへのパスを拡張するための適切な要素を変換することができます、ことを意味します。 [**異方性スケーリング**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) 11 星とこのページを示します。 *異方性*スケールではないこと、水平および垂直方向に等しい星が元の縦横比を保持されないことを意味することを意味します。 関連するコードをここでは、`PaintSurface`ハンドラー。

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

`pathBounds`四角形が、このコードの先頭付近にある取得され、キャンバスの高さと幅で後で使用される、`Scale`呼び出します。 呼び出し自体はによって表示される場合は、パスの座標をスケールは、`DrawPath`の呼び出しが星されますが、キャンバスの右上隅で中央揃えにします。 下と左にシフトが必要です。 これは、ジョブの`Translate`呼び出します。 これら 2 つのプロパティの`pathBounds`は約 –100、平行移動の係数は約 100。 `Translate`後の呼び出しは、`Scale`を呼び出すには、キャンバスの中央に、星の中央を移動できるように、これらの値をスケーリングの要因によってスケール効果的に。

[![](scale-images/anisotropicscaling-small.png "スケーリングの異方性ページのスクリーン ショットをトリプル")](scale-images/anisotropicscaling-large.png#lightbox "異方性スケール ページの 3 倍になるスクリーン ショット")

別の方法を考えることができます、`Scale`と`Translate`呼び出しは、逆の順序で効果を確認する:`Translate`呼び出しは、キャンバスの左上隅に指向が完全に可視状態になるようにパスをシフトします。 `Scale`メソッドからは、そのスター左上隅に対して相対的に大きくします。

実際には、星がキャンバスより少し大きいでことが表示されます。 問題は、ストロークの幅です。 `Bounds`プロパティの`SKPath`パスでエンコードされた座標のディメンションとは、プログラムの使用を拡張することを示します。 特定のストロークの幅と、パスが表示されると、表示されたパスは、キャンバスを超えています。

この問題を解決するには、補正する必要があります。 このプログラムでは、1 つの簡単なアプローチは、前に次のステートメント権限を追加する、`Scale`呼び出し。

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

これにより、 `pathBounds` 4 辺すべてにつき 1.5 単位で四角形。 これは、ストロークの結合が丸められた場合にのみ、妥当なソリューションです。 マイター結合は長くなり、計算することは困難です。

テキスト、同様の手法として使用することもできます、**異方性テキスト**ページを示します。 関連部分を次に示します、`PaintSurface`からハンドラー、 [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs)クラス。

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

同様のロジック、テキストがテキストの境界の四角形から返されるに基づいてページのサイズに拡大`MeasureText`(実際のテキストより少し大きい値です)。

[![](scale-images/anisotropictext-small.png "異方性テスト ページのスクリーン ショットをトリプル")](scale-images/anisotropictext-large.png#lightbox "異方性テスト ページの 3 倍になるスクリーン ショット")

グラフィカル オブジェクトの縦横比を保持する必要がある場合にアイソトロ ピック スケーリングを使用します。 **アイソトロ ピック スケーリング**11 星のこのページを示します。 概念的には、グラフィカル オブジェクトをアイソトロ ピック スケーリングとページの中央に表示するための手順です。

- 左上隅にグラフィカル オブジェクトの中心に変換します。
- 水平および垂直方向のページの寸法のグラフィカル オブジェクトのサイズで割った値の最小値に基づくオブジェクトを拡張します。
- ページの中央にスケーリングされたオブジェクトの中心に変換します。

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs)星を表示する前に、逆の順序で次の手順を実行します。

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

によって 10% と赤から青に徐々 に色を変更するたびに、スケーリングの減少を考慮、コードが他にもスター 10 に表示されます。

[![](scale-images/isotropicscaling-small.png "アイソトロ ピック スケーリング ページのスクリーン ショットをトリプル")](scale-images/isotropicscaling-large.png#lightbox "アイソトロ ピック スケール ページの 3 倍になるスクリーン ショット")


## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
