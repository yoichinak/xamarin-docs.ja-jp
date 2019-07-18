---
title: SkiaSharp のプラットフォーム固有の注意事項
description: このドキュメントでは、SkiaSharp に関連するプラットフォーム固有の詳細について説明します。 IOS、Android、macOS、Windows、および Xamarin.Forms のサンプル コードを提供します。
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 1D90E0B3-A3A8-4286-BC54-9D67188A1C6C
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2018
ms.openlocfilehash: ed80a423506f96972ddf80075379ed6e6dccd0a1
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675128"
---
# <a name="skiasharp-platform-specific-notes"></a>SkiaSharp のプラットフォーム固有の注意事項

次の例は、イメージのバッファーを手動で割り当てる、これは、プラットフォームによって提供される既存の RGBA バッファーに描画するためにはプラットフォームの一般的なパターンを説明するために行われます。

たくない場合は、この手法を使用する必要はありません。  作成され、イメージのバックアップ用ストレージを管理するをオーバー ロードがあります。

## <a name="android"></a>Android

```csharp
var width = (float)skiaView.Width;
var height = (float)skiaView.Height;

using (var bitmap = Bitmap.CreateBitmap (canvas.Width, canvas.Height, Bitmap.Config.Argb8888)) {
  try {
    using (var surface = SKSurface.Create (canvas.Width, canvas.Height, SKColorType.Rgba_8888, SKAlphaType.Premul, bitmap.LockPixels (), canvas.Width * 4)) {
      var skcanvas = surface.Canvas;
      skcanvas.Scale (((float)canvas.Width)/width, ((float)canvas.Height)/height);
      // DoDraw (skcanvas);
    }
  } finally {
    bitmap.UnlockPixels ();
  }
  canvas.DrawBitmap (bitmap, 0, 0, null);
}
```

## <a name="ios"></a>iOS

```csharp
var screenScale = UIScreen.MainScreen.Scale;
var width = (int)(Bounds.Width * screenScale);
var height = (int)(Bounds.Height * screenScale);

IntPtr buff = System.Runtime.InteropServices.Marshal.AllocCoTaskMem (width * height * 4);
try {
  using (var surface = SKSurface.Create (width, height, SKImageInfo.PlatformColorType, SKAlphaType.Premul, buff, width * 4)) {
    var skcanvas = surface.Canvas;
    skcanvas.Scale ((float)screenScale, (float)screenScale);
    using (new SKAutoCanvasRestore (skcanvas, true)) {
      // DoDraw (skcanvas);
    }
  }
  using (var colorSpace = CGColorSpace.CreateDeviceRGB ())
  using (var bContext = new CGBitmapContext (buff, width, height, 8, width * 4, colorSpace, (CGImageAlphaInfo)bitmapInfo))
  using (var image = bContext.ToImage ())
  using (var context = UIGraphics.GetCurrentContext ()) {
    // flip the image for CGContext.DrawImage
    context.TranslateCTM (0, Frame.Height);
    context.ScaleCTM (1, -1);
    context.DrawImage (Bounds, image);
  }
} finally {
  if (buff != IntPtr.Zero) {
    System.Runtime.InteropServices.Marshal.FreeCoTaskMem (buff);
  }
}
```

## <a name="macos"></a>macOS

```csharp
var screenScale = (int)NSScreen.MainScreen.BackingScaleFactor * 2;
var width = (int)Bounds.Width * screenScale;
var height = (int)Bounds.Height * screenScale;

IntPtr buff = System.Runtime.InteropServices.Marshal.AllocCoTaskMem (width * height * 4);
try {
  using (var surface = SKSurface.Create (width, height, SKColorType.Rgba_8888, SKAlphaType.Premul, buff, width * 4)) {
    var skcanvas = surface.Canvas;
    skcanvas.Scale (screenScale, screenScale);
    // DoDraw (skcanvas);
  }
  int flag = ((int)CoreGraphics.CGBitmapFlags.ByteOrderDefault) | ((int)CoreGraphics.CGImageAlphaInfo.PremultipliedLast);
  using (var colorSpace = CoreGraphics.CGColorSpace.CreateDeviceRGB ())
  using (var bContext = new CoreGraphics.CGBitmapContext (buff, width, height, 8, width * 4, colorSpace, (CoreGraphics.CGImageAlphaInfo) flag))
  using (var image = bContext.ToImage ())
  using (var context = NSGraphicsContext.CurrentContext.GraphicsPort) {
    context.DrawImage (Bounds, image);
  }
} finally {
  if (buff != IntPtr.Zero) {
    System.Runtime.InteropServices.Marshal.FreeCoTaskMem (buff);
  }
}
```

## <a name="windows-desktop--mac-desktop"></a>Windows デスクトップ/Mac デスクトップ

```csharp
var width = Width;
var height = Height;

using (var bitmap = new Bitmap(width, height, PixelFormat.Format32bppPArgb)) {
  var data = bitmap.LockBits(new Rectangle(0, 0, width, height), ImageLockMode.WriteOnly, bitmap.PixelFormat);
  using (var surface = SKSurface.Create(width, height, SKImageInfo.PlatformColorType, SKAlphaType.Premul, data.Scan0, width * 4)) {
    var skcanvas = surface.Canvas;
    // DoDraw (skcanvas);
  }
  bitmap.UnlockBits(data);
  e.Graphics.DrawImage(bitmap, new Rectangle(0, 0, Width, Height));
}
```
