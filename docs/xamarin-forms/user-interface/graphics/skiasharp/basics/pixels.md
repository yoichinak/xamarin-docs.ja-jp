---
title: ピクセル、およびデバイスに依存しない単位
description: この記事では、SkiaSharp 座標と座標の違いに Xamarin.Forms ついて説明し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c1ae9e05b6671d45d8df485a89cfc0dea86632d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937307"
---
# <a name="pixels-and-device-independent-units"></a>ピクセル、およびデバイスに依存しない単位

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_SkiaSharp の座標と座標の違いを調べる Xamarin.Forms_

この記事では、SkiaSharp とで使用される座標系の違いについて説明し Xamarin.Forms ます。 2つの座標系の間で変換を行うための情報を取得し、特定の領域を埋めるグラフィックを描画できます。

![画面を塗りつぶす楕円](pixels-images/screenfillexample.png)

しばらくの間にプログラミングしている場合は Xamarin.Forms 、座標とサイズの感覚がある可能性があり Xamarin.Forms ます。 前の2つの記事で描画した円は、少し小さいように見えます。

これらの円*は*、サイズと比較して小さく Xamarin.Forms なります。 既定では、SkiaSharp は、 Xamarin.Forms 基になるプラットフォームによって確立されたデバイスに依存しない単位に基づいて座標とサイズをピクセル単位で描画します。 (座標系の詳細については、「 Xamarin.Forms 5 章」を参照[してください。](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md)*を使用して Mobile Apps Xamarin.Forms を作成する*本のサイズを処理します)。

[**SkewSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)プログラムで使用されている**画面サイズ**のページでは、SkiaSharp テキスト出力を使用して、3つの異なるソースからのディスプレイ画面のサイズを示しています。

- Xamarin.Forms [`Width`](xref:Xamarin.Forms.VisualElement.Width) [`Height`](xref:Xamarin.Forms.VisualElement.Height) オブジェクトの通常のプロパティとプロパティ `SKCanvasView` 。
- [`CanvasSize`](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize)オブジェクトのプロパティ `SKCanvasView` です。
- [`Size`](xref:SkiaSharp.SKImageInfo.Size)値のプロパティ `SKImageInfo` 。これは、 `Width` `Height` 前の2つのページで使用されるプロパティとプロパティと一致します。

クラスは、 [`SurfaceSizePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) これらの値を表示する方法を示しています。 コンストラクターは、 `SKCanvasView` オブジェクトをフィールドとして保存します。これにより、イベントハンドラーでアクセスできるようになり `PaintSurface` ます。

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

`SKCanvas`には6つの異なるメソッドが含まれてい `DrawText` ますが、この [`DrawText`](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint)) 方法が最も簡単です。

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

テキスト文字列、テキストの開始位置を示す X 座標と Y 座標、およびオブジェクトを指定し `SKPaint` ます。 X 座標は、テキストの左側が配置される場所を指定します。ただし、Y 座標はテキストの*ベースライン*の位置を指定します。 手書きの用紙を手に入れた場合、ベースラインは文字が置かれている行であり、ディセンダー (g、p、q、および y の文字など) が降下します。

オブジェクトを使用すると、 `SKPaint` テキスト、フォントファミリ、およびテキストサイズの色を指定できます。 既定では、この [`TextSize`](xref:SkiaSharp.SKPaint.TextSize) プロパティの値は12であるため、電話などの高解像度のデバイスではテキストがわずかになります。 最も単純なアプリケーションであれば、表示するテキストのサイズに関する情報も必要になります。 クラスでは、 `SKPaint` [`FontMetrics`](xref:SkiaSharp.SKPaint.FontMetrics) プロパティといくつかのメソッドが定義されて [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) いますが、あまり凝っていない場合は、プロパティによって、 [`FontSpacing`](xref:SkiaSharp.SKPaint.FontSpacing) 連続するテキスト行に対して推奨される値が提供されます。

次の `PaintSurface` ハンドラーは `SKPaint` 40 ピクセルのに対するオブジェクトを作成します `TextSize` 。これは、アセンダーの先頭からディセンダーの一番下までのテキストの垂直方向の高さです。 `FontSpacing`オブジェクトから返される値 `SKPaint` は、それよりも少し大きくなります (約47ピクセル)。

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

メソッドは、X 座標が 20 (左側の小さな余白) との Y 座標でテキストの最初の行を開始し `fontSpacing` ます。これは、表示サーフェイスの上部にあるテキストの最初の行の高さを完全に表示するために必要な部分よりもわずかです。 を呼び出すたびに `DrawText` 、Y 座標がの1つまたは2つのインクリメントで増加し `fontSpacing` ます。

実行中のプログラムを次に示します。

[![[画面サイズ] ページのトリプルスクリーンショット](pixels-images/surfacesize-small.png)](pixels-images/surfacesize-large.png#lightbox "[画面サイズ] ページのトリプルスクリーンショット")

ご覧のように、の `CanvasSize` プロパティ `SKCanvasView` と `Size` 値のプロパティは、 `SKImageInfo` ピクセルディメンションをレポートするときに一貫しています。 `Height`のプロパティと `Width` プロパティ `SKCanvasView` はプロパティであり、 Xamarin.Forms プラットフォームで定義されているデバイスに依存しない単位でビューのサイズを報告します。

左側の iOS 7 シミュレーターは、デバイスに依存しないユニットごとに2ピクセルです。また、中央の Android の場合は、1ユニットあたり3ピクセルです。 これは、前に示した単純な円のサイズがプラットフォームによって異なるためです。

デバイスに依存しない単位で完全に作業する場合は `IgnorePixelScaling` 、のプロパティ `SKCanvasView` をに設定し `true` ます。 しかし、結果に満足できない場合もあります。 SkiaSharp は、デバイスに依存しない単位で、ビューのサイズと同じピクセルサイズを使用して、小さいデバイス画面でグラフィックスをレンダリングします。 (たとえば、SkiaSharp では、1つの 360 x 512 ピクセルの表示画面を使用します。その後、そのイメージのサイズが大きくなり、ビットマップの jaggies が目立つようになります。

同じイメージの解像度を維持するには、2つの座標系の間で変換するための単純な関数を作成する方が適しています。

`DrawCircle`では、メソッドに加え `SKCanvas` て、楕円を描画する2つのメソッドも定義されて `DrawOval` います。 楕円は、1つの半径ではなく2つの半径によって定義されます。 これらは、*主要半径*と*補助半径*と呼ばれます。 この `DrawOval` メソッドは、X 軸と Y 軸に平行する2つの半径を持つ楕円を描画します。 (X 軸と Y 軸に平行になっていない軸を持つ楕円を描画する必要がある場合は、「[**円弧を描画する3つの方法**](../curves/arcs.md)」の記事で説明されているように、[**回転変換**](../transforms/rotate.md)またはグラフィックスパスに関する記事で説明されている回転変換を使用できます)。 このメソッドのオーバーロード [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) は、2つの半径のパラメーターに名前を `rx` 指定し、 `ry` それらが X 軸と Y 軸に対して平行であることを示します。

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

表示サーフェイスを塗りつぶす楕円を描画することはできますか。 **楕円の塗りつぶし**ページは、その方法を示しています。 `PaintSurface` [**EllipseFillPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs)クラスのイベントハンドラーは、 `xRadius` `yRadius` 楕円全体とその輪郭を表示サーフェイス内に収まるように、との値からストロークの幅の半分を減算します。

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

次のように実行されています。

[![[画面サイズ] ページのトリプルスクリーンショット](pixels-images/ellipsefill-small.png)](pixels-images/ellipsefill-large.png#lightbox "[画面サイズ] ページのトリプルスクリーンショット")

もう1つのメソッドには、 [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint)) 引数があります [`SKRect`](xref:SkiaSharp.SKRect) 。これは、左上隅と右下隅の X 座標と Y 座標の観点で定義された四角形です。 楕円はその四角形を塗りつぶします。これは、次のように楕円の**塗りつぶし**ページで使用できることを示しています。

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

ただし、4辺の楕円の輪郭のすべての端が切り捨てられます。 `SKRect` `strokeWidth` この作業を適切に行うには、に基づいてすべてのコンストラクター引数を調整する必要があります。

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
