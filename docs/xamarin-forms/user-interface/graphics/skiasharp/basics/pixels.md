---
title: "デバイス非依存単位、ピクセル"
description: "SkiaSharp 座標と Xamarin.Forms の座標の違いを調べる"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 5e07377584996694aa8597af79317957c51050ec
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="pixels-and-device-independent-units"></a>デバイス非依存単位、ピクセル

_SkiaSharp 座標と Xamarin.Forms の座標の違いを調べる_

この記事では、SkiaSharp および Xamarin.Forms で使用する座標系の違いについて説明します。 2 つの座標システムの間に変換し、特定の領域を塗りつぶすグラフィックスを描画する情報を取得できます。

![](pixels-images/screenfillexample.png "画面全体を楕円")

しばらくの間 Xamarin.Forms でプログラミングされている、Xamarin.Forms 座標とサイズのフィールがあります。 前の 2 つのアーティクルで描画される円を少し小に見えるかもしれません。

これらの円*は*Xamarin.Forms サイズと比較して少ないです。 既定では、Xamarin.Forms に基づいて座標とサイズ、基になるプラットフォームによって確立されたデバイスに依存しない単位間 SkiaSharp をピクセル単位で描画します。(詳細については、Xamarin.Forms の座標系には含まれて[第 5 章します。サイズを扱う](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md)書籍の*Xamarin.Forms を使用したモバイル アプリを作成する*)。

内のページ、 [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)というプログラム**画面サイズ**SkiaSharp テキスト出力を使用して、次の 3 つの異なるソースから、ディスプレイ画面のサイズを示します。

- 通常の Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/)と[ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/)のプロパティ、`SKCanvasView`オブジェクト。
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/)のプロパティ、`SKCanvasView`オブジェクト。
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/)のプロパティ、`SKImageInfo`と一致する値、`Width`と`Height`2 つの前のページで使用されるプロパティです。

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs)クラスは、これらの値を表示する方法を示します。 コンス トラクターの保存、`SKCanvasView`オブジェクトにアクセスできるように、フィールドとして、`PaintSurface`イベントのハンドラー。

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

`SKCanvas` 6 つの異なるが含まれています`DrawText`メソッドが、この[ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/)メソッドは、最も簡単な。

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

X 座標と Y 座標を開始する、テキストがここでは、テキスト文字列を指定して、`SKPaint`オブジェクト。 X 座標は、注意が、テキストの左側にあるを配置する場所を指定: Y 座標を指定の位置を指定する、*基準*のテキスト。 これまで罫線の引かれた紙に手動で作成した、ベースラインは、どの文字 sit と下りる際にどのディセンダー (に文字 g、p、q、および y など) の下の行をします。

`SKPaint`オブジェクトでは、テキスト、フォント ファミリ、およびテキストのサイズの色を指定することができます。 既定では、 [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/)非常に小さいテキスト電話などの高解像度のデバイスで結果を 12 の値を持つプロパティです。 最も簡単なアプリケーションを除いて必要がありますも一部の情報を表示するテキストのサイズにします。 `SKPaint`クラスを定義、 [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/)プロパティは、いくつか[ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/)メソッドが、高級に設置小さいニーズに応じて、 [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/)プロパティは、テキストの連続する行の間隔の推奨値を提供します。

次`PaintSurface`ハンドラーを作成、`SKPaint`オブジェクトを`TextSize`ディセンダーの最下位にアセンダーの最上位からテキストの縦の高さの目的は、40 ピクセルです。 `FontSpacing`値、 `SKPaint` 47 ピクセルに関するをより少し大きい値は、オブジェクトを返します。

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

メソッドは、20 (左側にある小さな余白) の X 座標と Y 座標のテキストの最初の行をまず`fontSpacing`、これは、ディスプレイ画面の上部にあるテキストの最初の行の最大の高さを表示する必要はどのようなより若干高です。 呼び出すたび`DrawText`、Y 座標はの 1 つまたは 2 つずつ増加`fontSpacing`です。

3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![](pixels-images/surfacesize-small.png "画面のサイズのページのスクリーン ショットをトリプル")](pixels-images/surfacesize-large.png#lightbox "画面のサイズのページのトリプル スクリーン ショット")

わかります、`CanvasSize`のプロパティ、`SKCanvasView`と`Size`のプロパティ、`SKImageInfo`値がピクセルのサイズをレポートに一致します。 `Height`と`Width`のプロパティ、 `SKCanvasView` Xamarin.Forms のプロパティと、ビュー、プラットフォームで定義されているデバイスに依存しない単位のサイズを報告します。

左上の iOS 7 シミュレーターがデバイスに依存しない単位あたり 2 ピクセル、中央に Android Nexus 5 3 ピクセル単位では、持ち右側の Nokia Lumia 925 2.25 ピクセル単位です。 理由、単純な円、ほぼ同じサイズ、iPhone で表示されている以前の検索を Windows phone は、Android フォンでより小さい。

デバイス非依存単位でまったく作業する場合は、これを行う設定して、`IgnorePixelScaling`のプロパティ、`SKCanvasView`に`true`です。 ただし、結果を入らないかもしれません。 SkiaSharp は、ピクセル サイズがデバイスに依存しない単位のビューのサイズと等しいより小さなデバイス画面上の画像を表示します。 (たとえば、SkiaSharp は使用 360 x 512 ピクセルの表示画面 Nexus 5)。効率が改善そのイメージ サイズは、顕著なビットマップ ギザギザの結果として得られます。

同じ画像の解像度を維持するためより良いソリューションは、2 つの座標システム間で変換を独自の単純な関数を記述します。

加え、`DrawCircle`メソッド、`SKCanvas`も定義する 2 つ`DrawOval`楕円を描画するメソッド。 楕円は、半径が 1 つではなく、2 つの半径によって定義されます。 これらと呼ばれますが、*主要な radius*と*マイナー radius*です。 `DrawOval`メソッドが、X 軸と Y 軸に平行して、2 つの半径楕円を描画します。 変換またはグラフィックスへのパス (後で説明)、使用制限を克服できますが、[この`DrawOval`メソッド](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/)半径が 2 つの引数を名前`rx`と`ry`に似ていることを示すためにX と Y 軸で:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

画面の表面を塗りつぶす楕円を描画することは? **楕円の塗りつぶし**ページを示していますか。 `PaintSurface`内のイベント ハンドラー、 [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs)クラス減算から線幅の半分、`xRadius`と`yRadius`全体の楕円に合わせて値とそのデザイン画面内を示してください。

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

ここで、次の 3 つのプラットフォームで実行されています。

[![](pixels-images/ellipsefill-small.png "画面のサイズのページのスクリーン ショットをトリプル")](pixels-images/ellipsefill-large.png#lightbox "画面のサイズのページのトリプル スクリーン ショット")

[他`DrawOval`メソッド](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)が、 [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/)引数は、四角形の左上隅および右下隅の X および Y 座標の観点で定義します。 楕円を塗りつぶしますが推奨することができる可能性がありますで使用する四角形、**楕円塗りつぶし**次のようにページ。

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

ただし、4 つの辺の楕円のアウトラインのすべてのエッジを切り捨てます。 すべてを調整する必要があります、`SKRect`コンス トラクターの引数がに基づいて、`strokeWidth`右に。

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
