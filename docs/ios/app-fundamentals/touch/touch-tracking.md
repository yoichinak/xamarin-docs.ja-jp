---
title: Xamarin のマルチタッチ指トラッキング
description: このドキュメントでは、Xamarin iOS アプリでマルチタッチジェスチャの個々の指を追跡する方法について説明します。 フィンガーペイントアプリの例を中心にしています。
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: b1ba548135cedd951d7f0a349f273b29182839d1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928680"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Xamarin のマルチタッチ指トラッキング

_このドキュメントでは、複数の指からタッチイベントを追跡する方法を示します。_

マルチタッチアプリケーションでは、画面上で同時に移動するたびに個々の指を追跡する必要がある場合があります。 1つの一般的なアプリケーションは、フィンガーペイントプログラムです。 ユーザーが1本の指で描画できるようにし、同時に複数の指で描画することもできます。 複数のタッチイベントを処理するプログラムでは、これらの指を区別する必要があります。

指が画面に触れると、iOS によって [`UITouch`](xref:UIKit.UITouch) その指のオブジェクトが作成されます。 このオブジェクトは、指が画面上で移動し、画面から離れると、オブジェクトが破棄されたときと同じ状態になります。 指を追跡するには、プログラムがこのオブジェクトを直接格納しないようにする必要があり `UITouch` ます。 代わりに、型のプロパティを使用して、 [`Handle`](xref:Foundation.NSObject.Handle) `IntPtr` これらのオブジェクトを一意に識別でき `UITouch` ます。

ほとんどの場合、個々の指を追跡するプログラムは、タッチ追跡用のディクショナリを保持します。 IOS プログラムの場合、ディクショナリキーは特定の `Handle` 指を識別する値です。 ディクショナリ値は、アプリケーションによって異なります。 [FingerPaint](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)プログラムでは、各指ストローク (タッチからリリースまで) が、その指で描画された直線を描画するために必要なすべての情報を含むオブジェクトに関連付けられています。 プログラムは、 `FingerPaintPolyline` この目的のための小さいクラスを定義します。

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

各ポリラインには、色、ストロークの幅、および、 [`CGPath`](xref:CoreGraphics.CGPath) 描画中の線の複数のポイントを蓄積してレンダリングするための iOS グラフィックスオブジェクトがあります。

次に示すコードの残りの部分はすべて、という派生物に含まれてい `UIView` `FingerPaintCanvasView` ます。 このクラスは、 `FingerPaintPolyline` 1 つまたは複数の指によってアクティブに描画されているときに、型のオブジェクトのディクショナリを保持します。

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

このディクショナリを使用すると、ビューは、 `FingerPaintPolyline` オブジェクトのプロパティに基づいて、各指に関連付けられている情報をすばやく取得でき `Handle` `UITouch` ます。

`FingerPaintCanvasView`また、クラスは、 `List` 完成したポリラインのオブジェクトも保持します。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

この内のオブジェクト `List` は、描画された順序と同じ順序になります。

`FingerPaintCanvasView`によって定義された5つのメソッドをオーバーライドし `View` ます。

- [`TouchesBegan`](xref:UIKit.UIResponder.TouchesBegan(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesMoved`](xref:UIKit.UIResponder.TouchesMoved(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesEnded`](xref:UIKit.UIResponder.TouchesEnded(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesCancelled`](xref:UIKit.UIResponder.TouchesCancelled(Foundation.NSSet,UIKit.UIEvent))
- [`Draw`](xref:UIKit.UIView.Draw(CoreGraphics.CGRect))

さまざまな `Touches` オーバーライドは、ポリラインを構成する点を累積します。

[] のオーバーライドは、 `Draw` 完成したポリラインと進行中のポリラインを描画します。

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

各オーバーライドは、 `Touches` `UITouch` メソッドの引数に格納されている1つ以上のオブジェクトによって示される複数の指のアクションを報告する可能性があり `touches` ます。 `TouchesBegan`これらのオブジェクトをオーバーライドします。 メソッドは、オブジェクトごとに、 `UITouch` `FingerPaintPolyline` メソッドから取得した指の初期位置を格納するなど、新しいオブジェクトを作成して初期化し `LocationInView` ます。 この `FingerPaintPolyline` オブジェクトは、ディクショナリ `InProgressPolylines` `Handle` キーとしてオブジェクトのプロパティを使用してディクショナリに追加され `UITouch` ます。

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

メソッドは、を呼び出して `SetNeedsDisplay` 、オーバーライドの呼び出しを生成 `Draw` し、画面を更新します。

指や指が画面上を移動すると、は `View` そのオーバーライドの複数の呼び出しを取得し `TouchesMoved` ます。 このオーバーライド `UITouch` は同様に、引数に格納されているオブジェクトをループ `touches` し、指の現在の位置をグラフィックパスに追加します。

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

`touches`コレクションに `UITouch` は、またはへの最後の呼び出し以降に移動した指のオブジェクトだけが格納され `TouchesBegan` `TouchesMoved` ます。 `UITouch`現在画面と接触している*すべて*の指に対応するオブジェクトが必要な場合は、 `AllTouches` メソッドの引数のプロパティを使用してその情報を取得でき `UIEvent` ます。

`TouchesEnded`オーバーライドには2つのジョブがあります。 このメソッドは、グラフィックスパスに最後の点を追加し、 `FingerPaintPolyline` ディクショナリから一覧にオブジェクトを転送する必要があり `inProgressPolylines` `completedPolylines` ます。

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

この `TouchesCancelled` オーバーライドは、ディクショナリ内のオブジェクトを破棄するだけで処理され `FingerPaintPolyline` ます。

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

[![個々の指を追跡して結果を画面に描画する](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々の指を追跡し、それらを区別する方法がわかりました。

## <a name="related-links"></a>関連リンク

- [同等の Xamarin Android ガイド](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
