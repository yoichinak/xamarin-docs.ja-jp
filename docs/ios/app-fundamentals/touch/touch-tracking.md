---
title: Xamarin のマルチタッチ指トラッキング
description: このドキュメントでは、Xamarin iOS アプリでマルチタッチジェスチャの個々の指を追跡する方法について説明します。 フィンガーペイントアプリの例を中心にしています。
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: c3998424c8f4e9482a41e2891e65f0d13d8ac2f3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009186"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Xamarin のマルチタッチ指トラッキング

_このドキュメントでは、複数の指からタッチイベントを追跡する方法を示します。_

マルチタッチアプリケーションでは、画面上で同時に移動するたびに個々の指を追跡する必要がある場合があります。 1つの一般的なアプリケーションは、フィンガーペイントプログラムです。 ユーザーが1本の指で描画できるようにし、同時に複数の指で描画することもできます。 複数のタッチイベントを処理するプログラムでは、これらの指を区別する必要があります。

指が画面に触れると、iOS はその指の[`UITouch`](xref:UIKit.UITouch)オブジェクトを作成します。 このオブジェクトは、指が画面上で移動し、画面から離れると、オブジェクトが破棄されたときと同じ状態になります。 指を追跡するには、プログラムでこの `UITouch` オブジェクトを直接格納しないようにする必要があります。 代わりに、型 `IntPtr` の[`Handle`](xref:Foundation.NSObject.Handle)プロパティを使用して、これらの `UITouch` オブジェクトを一意に識別できます。

ほとんどの場合、個々の指を追跡するプログラムは、タッチ追跡用のディクショナリを保持します。 IOS プログラムの場合、ディクショナリキーは、特定の指を識別する `Handle` の値です。 ディクショナリ値は、アプリケーションによって異なります。 [FingerPaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)プログラムでは、各指ストローク (タッチからリリースまで) が、その指で描画された直線を描画するために必要なすべての情報を含むオブジェクトに関連付けられています。 このプログラムでは、この目的のために小さい `FingerPaintPolyline` クラスを定義します。

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

各ポリラインには、色、ストロークの幅、および iOS グラフィックス[`CGPath`](xref:CoreGraphics.CGPath)オブジェクトがあり、描画中に線の複数の点を蓄積してレンダリングします。

次に示すコードの残りの部分はすべて、`FingerPaintCanvasView`という `UIView` の派生物に含まれています。 このクラスは、1つまたは複数の指によってアクティブに描画されている間に `FingerPaintPolyline` 型のオブジェクトのディクショナリを保持します。

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

このディクショナリを使用すると、ビューは `UITouch` オブジェクトの `Handle` プロパティに基づいて、各指に関連付けられている `FingerPaintPolyline` 情報をすばやく取得できます。

`FingerPaintCanvasView` クラスは、完成したポリラインの `List` オブジェクトも保持します。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

この `List` のオブジェクトは、描画された順序と同じ順序になっています。

`FingerPaintCanvasView` は、`View`によって定義された5つのメソッドをオーバーライドします。

- [`TouchesBegan`](xref:UIKit.UIResponder.TouchesBegan(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesMoved`](xref:UIKit.UIResponder.TouchesMoved(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesEnded`](xref:UIKit.UIResponder.TouchesEnded(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesCancelled`](xref:UIKit.UIResponder.TouchesCancelled(Foundation.NSSet,UIKit.UIEvent))
- [`Draw`](xref:UIKit.UIView.Draw(CoreGraphics.CGRect))

さまざまな `Touches` オーバーライドは、ポリラインを構成する点を累積します。

[`Draw`] のオーバーライドは、完成したポリラインと進行中のポリラインを描画します。

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

`Touches` の各オーバーライドでは、`touches` 引数に格納されている1つ以上の `UITouch` オブジェクトによって示される複数の指のアクションがメソッドに報告される可能性があります。 `TouchesBegan` は、これらのオブジェクトのループをオーバーライドします。 メソッドは、`UITouch` オブジェクトごとに、`LocationInView` メソッドから取得した指の初期位置を格納するなど、新しい `FingerPaintPolyline` オブジェクトを作成して初期化します。 この `FingerPaintPolyline` オブジェクトは、`UITouch` オブジェクトの `Handle` プロパティをディクショナリキーとして使用して、`InProgressPolylines` ディクショナリに追加されます。

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

メソッドは、`SetNeedsDisplay` を呼び出して、オーバーライド `Draw` の呼び出しを生成し、画面を更新します。

指または指が画面上を移動すると、`View` は `TouchesMoved` オーバーライドの複数の呼び出しを取得します。 このオーバーライドは同様に、`touches` 引数に格納されている `UITouch` オブジェクトをループし、指の現在位置をグラフィックパスに追加します。

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches` コレクションには、`TouchesBegan` または `TouchesMoved`への最後の呼び出し以降に移動した指の `UITouch` オブジェクトのみが含まれます。 現在画面と接触している*すべて*の指に対応する `UITouch` オブジェクトが必要な場合は、メソッドの `UIEvent` 引数の `AllTouches` プロパティを使用してその情報を取得できます。

`TouchesEnded` のオーバーライドには2つのジョブがあります。 グラフィックスパスに最後の点を追加し、`FingerPaintPolyline` オブジェクトを `inProgressPolylines` ディクショナリから `completedPolylines` リストに転送する必要があります。

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled` のオーバーライドは、ディクショナリ内の `FingerPaintPolyline` オブジェクトを破棄するだけで処理されます。

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

この処理によって、 [FingerPaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)プログラムは個々の指を追跡し、画面に結果を描画できます。

[![](touch-tracking-images/image01.png "Tracking individual fingers and drawing the results on the screen")](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々の指を追跡し、それらを区別する方法がわかりました。

## <a name="related-links"></a>関連リンク

- [同等の Xamarin Android ガイド](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
