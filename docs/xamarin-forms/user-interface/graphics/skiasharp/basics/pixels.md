---
title: ピクセル、およびデバイスに依存しない単位
description: この記事では、SkiaSharp 座標と Xamarin.Forms の座標間の違いについて説明し、サンプル コードを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
ms.openlocfilehash: d6011175a735eb81f83a023f7d32fccd6feadd47
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759468"
---
# <a name="pixels-and-device-independent-units"></a>ピクセル、およびデバイスに依存しない単位

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp の座標と Xamarin.Forms の座標間の違いを詳細します。_

この記事では、SkiaSharp および Xamarin.Forms で使用される座標系の違いについて説明します。 2 つの座標システム間の変換も特定の領域に表示されるグラフィックスを描画する情報を取得できます。

![](pixels-images/screenfillexample.png "楕円の画面を")

Xamarin.Forms でしばらくの間をプログラミングした、Xamarin.Forms の座標とサイズの感じがあります。 2 つの以前の記事で描画する円を少し小さく見えるかもしれません。

これらの円*は*Xamarin.Forms サイズと比較します。 既定では、Xamarin.Forms は、基になるプラットフォームによって確立されているデバイスに依存しない単位に基づいて座標とサイズを計算中に SkiaSharp をピクセル単位で描画します。 (Xamarin.Forms の座標システムの詳細についてで参照できる[第 5 章です。サイズを扱う](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md)書籍の*を Xamarin.Forms での Mobile Apps の作成*)。

内のページ、 [ **SkewSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)というプログラム**画面サイズ**SkiaSharp テキスト出力を使用して、次の 3 つの異なるソースから表示画面のサイズを表示します。

- 通常の Xamarin.Forms [ `Width` ](xref:Xamarin.Forms.VisualElement.Width)と[ `Height` ](xref:Xamarin.Forms.VisualElement.Height)のプロパティ、`SKCanvasView`オブジェクト。
- [ `CanvasSize` ](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize)のプロパティ、`SKCanvasView`オブジェクト。
- [ `Size` ](xref:SkiaSharp.SKImageInfo.Size)のプロパティ、`SKImageInfo`と一致する値、`Width`と`Height`前の 2 つのページで使用されるプロパティ。

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs)クラスは、これらの値を表示する方法を示します。 コンス トラクターの保存、`SKCanvasView`オブジェクトにアクセスできるように、フィールドとして、`PaintSurface`イベント ハンドラー。

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` 6 つの異なるが含まれています`DrawText`メソッドが、この[ `DrawText` ](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint))メソッドは、最も簡単な。

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

X および Y 座標をテキストの開始、位置、テキスト文字列を指定して、`SKPaint`オブジェクト。 X 座標は、テキストの左側が配置される場所を指定しますが、次の点に注意してください。Y 座標は、テキストの*ベースライン*の位置を指定します。 インライン展開のホワイト ペーパーでは、手動でこれまで記述した、ベースラインにどの文字 sit と (文字 g、p、q、および y 上など) には、どのディセンダー降下の下の行をします。

`SKPaint`オブジェクトを使用すると、テキスト、フォント ファミリ、およびテキストのサイズの色を指定します。 既定で、 [ `TextSize` ](xref:SkiaSharp.SKPaint.TextSize)携帯電話などの高解像度のデバイスで最小のテキストで、12 の値を持つプロパティです。 最も簡単なアプリケーション以外も必要になりますいくつかの情報を表示しているテキストのサイズにします。 `SKPaint`クラスを定義、 [ `FontMetrics` ](xref:SkiaSharp.SKPaint.FontMetrics)プロパティは、いくつか[ `MeasureText` ](xref:SkiaSharp.SKPaint.MeasureText(System.String))メソッドが、高度な少なくニーズに応じて、 [ `FontSpacing` ](xref:SkiaSharp.SKPaint.FontSpacing)プロパティは、テキストの空白文字の連続する行の推奨値を提供します。

次`PaintSurface`ハンドラーを作成、`SKPaint`オブジェクト、`TextSize`アセンダーの先頭からディセンダーの一番下のテキストの縦の高さの目的は、40 ピクセル単位の。 `FontSpacing`値、`SKPaint`を 47 のピクセルについてより少し大きい値は、オブジェクトが返されます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

メソッドは、(左側にある小さな余白) の場合は 20 の X 座標と Y 座標のテキストの最初の行をまず`fontSpacing`は画面の上部にあるテキストの最初の行の最大の高さを表示するために必要なものよりも、もう少しであります。 各呼び出しの後`DrawText`、Y 座標の 1 つまたは 2 つずつ増加`fontSpacing`します。

実行中のプログラムを次に示します。

[![](pixels-images/surfacesize-small.png "画面のサイズのページのスクリーン ショットをトリプル")](pixels-images/surfacesize-large.png#lightbox "画面サイズのページの 3 倍になるスクリーン ショット")

ご覧のとおり、`CanvasSize`のプロパティ、`SKCanvasView`と`Size`のプロパティ、`SKImageInfo`値はレポートのピクセル寸法で一貫性のあります。 `Height`と`Width`のプロパティ、 `SKCanvasView` Xamarin.Forms のプロパティと、プラットフォームによって定義されているデバイスに依存しない単位でビューのサイズを報告します。

左上の iOS 7 シミュレーターは、デバイスに依存しない単位あたり 2 つのピクセルを備え、センターでの Android の Nexus 5 が 3 ピクセル単位。 その前に示した単純な円は、さまざまなプラットフォームでさまざまなサイズが。

デバイス非依存単位全体で作業する場合は、これを行う設定して、`IgnorePixelScaling`のプロパティ、`SKCanvasView`に`true`します。 ただし、結果に満足する可能性がありますされません。 SkiaSharp では、デバイスに依存しない単位では、ビューのサイズに等しいピクセル サイズの小さいデバイスの画面でグラフィックスをレンダリングします。 (たとえば、SkiaSharp は使用 360 x 512 ピクセルの表示画面 Nexus 5 上)。スケール アップ顕著なビットマップ ギザギザのサイズは、そのイメージ。

同じ画像の解像度を維持するより優れたソリューションは、2 つの座標システム間で変換する単純な関数を作成します。

加え、`DrawCircle`メソッド、`SKCanvas`も定義する 2 つ`DrawOval`楕円を描画するメソッド。 楕円は、半径が 1 つではなく、2 つの半径によって定義されます。 これらと呼ばれますが、*主要な radius*と*マイナー radius*します。 `DrawOval`メソッドは X と Y 軸に平行 2 つの半径を使用して楕円を描画します。 (X と Y 軸に並列でない軸を持つ楕円を描画する必要がある場合、情報の記事で説明したように、回転変換を使用できます[ **、Rotate Transform** ](../transforms/rotate.md)またはグラフィックス パスで説明したように、記事[**円弧を描画する方法は 3 つ**](../curves/arcs.md))。 このオーバー ロード、 [ `DrawOval` ](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint))メソッド名の 2 つの半径パラメーター`rx`と`ry`X と Y 軸に平行ことを示します。

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

表示画面を塗りつぶす楕円を描画することはできますか。 **楕円の塗りつぶし**ページについて説明する方法。 `PaintSurface`内のイベント ハンドラー、 [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs)クラスの減算からストローク幅の半分、`xRadius`と`yRadius`全体の楕円サイズに合わせて値とその表示画面内を示してください。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

ここでは、実行します。

[![](pixels-images/ellipsefill-small.png "画面のサイズのページのスクリーン ショットをトリプル")](pixels-images/ellipsefill-large.png#lightbox "画面サイズのページの 3 倍になるスクリーン ショット")

他の[ `DrawOval` ](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint))メソッドには、 [ `SKRect` ](xref:SkiaSharp.SKRect)引数は、四角形の左上隅および右下隅の X および Y 座標で定義されています。 楕円、四角形を塗りつぶしますで使用することができるとありますを示す、**楕円の塗りつぶし**このようなページ。

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

ただし、4 つの辺の楕円のアウトラインのすべてのエッジを切り捨てます。 すべてを調整する必要がある、`SKRect`コンス トラクター引数に基づいて、`strokeWidth`右に。

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
