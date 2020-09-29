---
title: SkiaSharp プラットフォームに依存しない例
description: このドキュメントでは、中核となる SkiaSharp の概念について簡単に説明します。 特に、SKCanvas での取得と描画について説明します。
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2018
ms.openlocfilehash: fd8926edce6b2310271e15418f2498162723ba49
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436447"
---
# <a name="skiasharp-platform-independent-examples"></a>SkiaSharp プラットフォームに依存しない例

_これにより、SkiaSharp の背後にある概念の概要を簡単に説明することができるようになります。_

SkiaSharp には、2D バッファーにレンダリングするために使用できる、豊富で強力な2D グラフィックス API が用意されています。  これらの要素を使用して、アプリケーションに組み込むことができるカスタムユーザーインターフェイス要素と2D グラフィックスを実装できます。 SkiaSharp は [Skia](https://skia.org) ライブラリへの .net バインドで、このライブラリの機能と機能を継承します。

ライブラリは、現在、クロスプラットフォーム [Nuget パッケージ](https://www.nuget.org/packages/SkiaSharp)として使用できます。 nuget 参照を追加することで、このライブラリをプロジェクトに追加できます。

描画するには、コードでを作成し `SkCanvas` ます。これにより、描画操作が行われるサーフェイスが記述されます。

## <a name="obtaining-an-skcanvas"></a>SKCanvas の取得

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>SKCanvas での描画

では、 `SKCanvas` 使い慣れた他の描画モデルに似たような描画モデルを使用しており、オプションの透明度チャネルを使用して色を使用し、線、円弧、テキスト、およびイメージを描画できます。

次に、SkiaSharp で実行できるさまざまな項目をいくつか紹介します。  次の例では、変数 `canvas` は SKCanvas 型です。

### <a name="drawing-xamagon"></a>Xamagon の描画

この例では、Xamarin のロゴを Xamagon に描画します。

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

### <a name="drawing-bitmaps"></a>描画 (ビットマップを)

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

### <a name="drawing-with-image-filters"></a>描画 (イメージフィルターを使用して)

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

## <a name="more-information"></a>詳細情報

SkiaSharp の使用方法の詳細については、 [API のドキュメント](/dotnet/api/skiasharp)を参照してください。