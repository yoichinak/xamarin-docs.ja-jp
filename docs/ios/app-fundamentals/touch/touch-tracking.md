---
title: マルチタッチ指が Xamarin.iOS での追跡
description: このドキュメントでは、Xamarin.iOS アプリで、マルチタッチ ジェスチャで個々 の指を追跡する方法について説明します。 これは、フィンガーペインティング アプリの例を中心として展開します。
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a1ddcda84d51b5a8a9220558ddaf9476a2321ee8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105052"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>マルチタッチ指が Xamarin.iOS での追跡

_このドキュメントは、複数の指のタッチ イベントを追跡する方法を示します_

マルチタッチ アプリケーションが同時に画面に移動する場合に、個々 の本の指を追跡する必要がある場合もあります。 1 つの一般的なアプリケーションは、finger-paint プログラムです。 1 本の指を描くには、一度に複数の指で描画するためにもできるようにユーザーを必要とします。 プログラムでは、複数のタッチ イベントを処理するこの本の指を区別する必要があります。

指で画面が最初に iOS 作成、 [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/)その本の指のオブジェクト。 このオブジェクトでは、指が画面上を移動および動かした後、画面で、この時点で、オブジェクトが破棄されたと同じままです。 これを格納する本の指を追跡するため、プログラムが避ける必要があります`UITouch`オブジェクトに直接します。 代わりに、使用できる、 [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)型のプロパティ`IntPtr`これらを一意に識別する`UITouch`オブジェクト。

ほぼ常に、個々 の本の指を追跡するプログラムは、タッチの追跡のためのディクショナリを保持します。 ディクショナリ キーは、iOS プログラムの場合、`Handle`を特定の指を識別する値。 ディクショナリの値は、アプリケーションによって異なります。 [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)プログラム、(リリースへのタッチ) から各本の指のストロークはその指で描画される直線を表示するために必要なすべての情報を格納しているオブジェクトに関連付けられています。 プログラム定義の小さな`FingerPaintPolyline`この目的のためのクラス。

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

各ポリラインが、色、線の幅、および、iOS のグラフィックス[ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/)を蓄積し、描画されている行の複数のポイントをレンダリングするオブジェクト。


次のコードのすべての残りの部分が含まれている、`UIView`という名前の導関数`FingerPaintCanvasView`します。 クラスは型のオブジェクトのディクショナリを保持する`FingerPaintPolyline`1 つ以上の指で積極的に描画されているは時間内に。

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

このディクショナリをすばやく取得するビューを使うと、`FingerPaintPolyline`それぞれの指に関連付けられている情報に基づいて、`Handle`のプロパティ、`UITouch`オブジェクト。

`FingerPaintCanvasView`クラスも保持しています、`List`完成したポリラインのオブジェクト。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

このオブジェクト`List`が描画されたことと同じ順序で。

`FingerPaintCanvasView` によって定義された 5 つのメソッドをオーバーライド`View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

さまざまな`Touches`上書きをポリラインを形成する点を蓄積します。

[`Draw`] の上書きが完了したポリラインとし、実行中のポリラインを描画します。

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

各、`Touches`上書き可能性のある 1 つまたは複数によって示される、複数の指のアクションを報告`UITouch`オブジェクトに格納されている、`touches`メソッドの引数。 `TouchesBegan`これらのオブジェクトをループ処理をオーバーライドします。 各`UITouch`オブジェクト、メソッドを作成し、新しい初期化`FingerPaintPolyline`などから取得した本の指の初期位置を格納するオブジェクト、`LocationInView`メソッド。 これは、`FingerPaintPolyline`オブジェクトに追加されます、`InProgressPolylines`ディクショナリを使用して、`Handle`のプロパティ、`UITouch`ディクショナリ キーとしてのオブジェクト。

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

メソッドは最後に呼び出すことによって`SetNeedsDisplay`への呼び出しを生成する、`Draw`をオーバーライドし、画面を更新します。

指や本の指が画面で、移動、`View`複数の呼び出しを取得します。 その`TouchesMoved`をオーバーライドします。 この上書きを同様にループ、`UITouch`オブジェクトに格納されている、`touches`引数グラフィック パスに、本の指の現在の場所を追加します。

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

`touches`コレクションに含まれるもののみ`UITouch`オブジェクトの最後の呼び出しから移動した本の指`TouchesBegan`または`TouchesMoved`します。 必要が生じた場合`UITouch`オブジェクトに対応する*すべて*現在画面と指、その情報を利用、`AllTouches`のプロパティ、`UIEvent`メソッドの引数。

`TouchesEnded`上書きが 2 つのジョブ。 グラフィックス パス、および転送を最後のポイントを追加する必要がありますが、`FingerPaintPolyline`オブジェクトから、`inProgressPolylines`するディクショナリ、`completedPolylines`一覧。

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

`TouchesCancelled`オーバーライドは単に破棄によって処理される、`FingerPaintPolyline`ディクショナリ内のオブジェクト。

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

この処理では、完全に、 [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)プログラムを個々 の本の指を追跡し、結果を画面に描画します。

[![](touch-tracking-images/image01.png "個々 の本の指を追跡し、結果を画面の描画")](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々 の本の指を追跡し、それらを区別する方法を見てきました。



## <a name="related-links"></a>関連リンク

- [同等の Xamarin Android ガイド](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
