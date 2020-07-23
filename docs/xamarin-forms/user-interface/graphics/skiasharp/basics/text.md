---
title: テキストとグラフィックスの統合
description: この記事では、SkiaSharp グラフィックスを使用してテキストをアプリケーションに統合するためにレンダリングされるテキスト文字列のサイズを決定する方法について説明 Xamarin.Forms し、サンプルコードを使用してこれを示します。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a98c7210f2e71f6f26d53da3555f3f9b5e016952
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935981"
---
# <a name="integrating-text-and-graphics"></a>テキストとグラフィックスの統合

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_テキストを SkiaSharp グラフィックスと統合するために表示されるテキスト文字列のサイズを確認する方法を参照してください。_

この記事では、テキストを測定し、テキストを特定のサイズにスケーリングし、テキストを他のグラフィックスと統合する方法について説明します。

![四角形で囲まれるテキスト](text-images/textandgraphicsexample.png)

このイメージには、角丸四角形も含まれます。 SkiaSharp クラスには、四角形を描画し、 `Canvas` [`DrawRect`](xref:SkiaSharp.SKCanvas.DrawRect*) [`DrawRoundRect`](xref:SkiaSharp.SKCanvas.DrawRoundRect*) 角が丸い四角形を描画するメソッドが含まれています。 これらのメソッドを使用すると、四角形を値として、 `SKRect` またはその他の方法で定義できます。

[フレーム**テキスト**] ページでは、ページに短いテキスト文字列が中央揃えで表示され、角丸四角形のペアで構成されるフレームで囲まれます。 クラスは、 [`FramedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) その方法を示しています。

SkiaSharp では、クラスを使用して `SKPaint` テキストとフォントの属性を設定しますが、テキストの表示サイズを取得することもできます。 次のイベントハンドラーの先頭は、 `PaintSurface` 2 つの異なるメソッドを呼び出し `MeasureText` ます。 最初の [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) 呼び出しには単純な引数があり、 `string` 現在のフォント属性に基づいて、テキストのピクセル幅が返されます。 次に、レンダリングされ `TextSize` `SKPaint` た幅、現在の `TextSize` プロパティ、および表示領域の幅に基づいて、オブジェクトの新しいプロパティを計算します。 この計算は、 `TextSize` テキスト文字列が画面の幅の90% で表示されるように設定することを目的としています。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

2番目の [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String,SkiaSharp.SKRect@)) 呼び出しには引数があるため、表示される `SKRect` テキストの幅と高さの両方を取得します。 `Height`この値のプロパティは、 `SKRect` テキスト文字列に大文字、アセンダー、およびディセンダーが存在するかどうかによって異なります。 `Height`たとえば、テキスト文字列 "mom"、"cat"、および "dog" に対して異なる値が報告されます。

`Left` `Top` 構造体のプロパティとプロパティは、 `SKRect` テキストが `DrawText` X および Y 位置が0の呼び出しによって表示される場合に、表示されるテキストの左上隅の座標を示します。 たとえば、このプログラムが iPhone 7 シミュレーターで実行されている場合、の `TextSize` 最初の呼び出しに続く計算の結果、には値90.6254 が割り当てられ `MeasureText` ます。 の `SKRect` 2 回目の呼び出しから取得された値に `MeasureText` は、次のプロパティ値があります。

- `Left`= 6
- `Top` = &ndash;68
- `Width`= 664.8214
- `Height`= 88;

メソッドに渡す X 座標と Y 座標は、 `DrawText` ベースラインでのテキストの左側を指定することに注意してください。 `Top`88 この値は、テキストがそのベースラインの上の68ピクセルまで拡張されていることを示します。また、ベースラインの下の20ピクセル下に68を引いたです。 `Left`値6は、呼び出しの X 値の右側にある6ピクセルのテキストを開始することを示し `DrawText` ます。 これにより、通常の文字間隔を使用できます。 ディスプレイの左上隅にテキストを表示する場合は、これらの `Left` 値と値の負の `Top` 値をの X 座標と Y 座標として渡し `DrawText` ます (この例では &ndash; 6 と 68)。

`SKRect`構造体は、いくつかの便利なプロパティとメソッドを定義します。その一部は、ハンドラーの残りの部分で使用され `PaintSurface` ます。 との値は、 `MidX` `MidY` 四角形の中心の座標を示します。 (IPhone 7 の例では、これらの値は338.4107 と &ndash; 24 です)。次のコードでは、これらの値を使用して、画面上にテキストを中央揃えで表示する最も簡単な座標を計算します。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`SKImageInfo`Info 構造体は、型のプロパティも定義するので、次 [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) `SKRect` のように計算することもでき `xText` `yText` ます。

```csharp
float xText = info.Rect.MidX - textBounds.MidX;
float yText = info.Rect.MidY - textBounds.MidY;
```

ハンドラーはの2つの呼び出しを終了し `PaintSurface` `DrawRoundRect` ます。どちらもの引数を必要と `SKRect` します。 この `SKRect` 値は、メソッドから取得した値に基づいていますが、同じにすることはでき `SKRect` `MeasureText` ません。 まず、丸みのある四角形がテキストの端に描画されないように、少し大きめにする必要があります。 また、との値が、 `Left` `Top` 四角形が配置される左上隅に対応するように、スペースでシフトする必要があります。 これらの2つのジョブは、 [`Offset`](xref:SkiaSharp.SKRect.Offset*) によって定義されたメソッドとメソッドによって実現され [`Inflate`](xref:SkiaSharp.SKRect.Inflate*) `SKRect` ます。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

その後、メソッドの残りの部分はストレートになります。 この例で `SKPaint` は、境界に対して別のオブジェクトを作成し、を `DrawRoundRect` 2 回呼び出します。 2番目の呼び出しでは、別の10ピクセルで拡大された四角形を使用します。 最初の呼び出しでは、角の半径として20ピクセルが指定されています。 2番目の角の半径は30ピクセルであるため、並列であるように見えます。

 [![フレームテキストページのトリプルスクリーンショット](text-images/framedtext-small.png)](text-images/framedtext-large.png#lightbox "フレームテキストページのトリプルスクリーンショット")

電話またはシミュレーターを横向きにして、テキストとフレームのサイズの増加を確認することができます。

画面上で一部のテキストを中央揃えにする必要があるだけの場合は、テキストを測定せずに、おおよその操作を行うことができます。 代わりに、 [`TextAlign`](xref:SkiaSharp.SKPaint.TextAlign) のプロパティを `SKPaint` 列挙体のメンバーに設定し [`SKTextAlign.Center`](xref:SkiaSharp.SKTextAlign) ます。 メソッドで指定した X 座標は、 `DrawText` テキストの水平方向の位置を示します。 画面の中間点をメソッドに渡すと、ベースラインが垂直方向に中央揃えになるため、テキストが中央揃えになり、 `DrawText` 垂直方向に中央揃えで*nearly*配置されます。

テキストは、他のグラフィカルオブジェクトと同様に扱うことができます。 簡単な方法の1つとして、テキスト文字のアウトラインを表示する方法があります。

[![アウトライン表示されたテキストページのトリプルスクリーンショット](text-images/outlinedtext-small.png)](text-images/outlinedtext-large.png#lightbox "アウトライン表示されたテキストページのトリプルスクリーンショット")

これを実現するには、 `Style` オブジェクトの通常のプロパティを `SKPaint` の既定の設定から `SKPaintStyle.Fill` に変更し、ストロークの幅を指定し `SKPaintStyle.Stroke` ます。 アウトライン表示された `PaintSurface` **テキスト**ページのハンドラーは、その方法を示しています。

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

もう1つの一般的なグラフィックオブジェクトはビットマップです。 「 [**SkiaSharp**](../bitmaps/index.md)Bitmap」セクションで詳しく説明されている大きなトピックですが、次の記事「 [**SkiaSharp でのビットマップの基礎**](bitmaps.md)」ではより簡単の概要を説明しています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
