---
title: "SkiaSharp の概要"
description: "これにより、SkiaSharp 概念の概要"
ms.topic: article
ms.prod: xamarin
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 747c158460b23b91e4c2986d212802528cdd3979
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2018
---
# <a name="an-introduction-to-skiasharp"></a>SkiaSharp の概要

_これにより、SkiaSharp 概念の概要_

SkiaSharp 豊富で強力な 2 次元グラフィックス 2D バッファーに表示するために使用できる API を提供します。  これらを使用して、カスタム ユーザー インターフェイス要素と、アプリケーションに組み込むことが 2 次元グラフィックスを実装することができます。  SkiaSharp は、.NET バインディングを[Skia](https://skia.org)ライブラリ機能とこのライブラリの電源を継承します。

ライブラリは現在、クロス プラットフォームとして使用[NuGet パッケージ](https://www.nuget.org/packages/SkiaSharp)NuGet の参照を追加することで、プロジェクトに追加できます。

を描画する、コードは、作成、 `SkCanvas` 、画面の描画操作を行う場所を記述します。

## <a name="obtaining-an-skcanvas"></a>SKCanvas を取得します。

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>SKCanvas の描画

`SKCanvas`に精通している可能性がありますを使用して基本の他の図のよう図面モデルをモデル化、省略可能な透過性チャネルで色を使用し、線、円弧、テキストおよびイメージを描画できます。

SkiaSharp で実行できる多くのさまざまな処理の一部だけを次に示します。  変数の下の例で`canvas`SKCanvas の種類です。

### <a name="drawing-xamagon"></a>描画 Xamagon

この例では、Xamarin のロゴ、Xamagon 描画します。

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>テキストの描画

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>ビットマップを描画

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>イメージ フィルターを使用した描画

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

# <a name="more-information"></a>詳細情報

SkiaSharp の使用に関する詳細についてにあります、 [API のドキュメントをオンライン](https://developer.xamarin.com/api/namespace/SkiaSharp/)


## <a name="related-links"></a>関連リンク

- [SkiaSharp iOS ブック](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
