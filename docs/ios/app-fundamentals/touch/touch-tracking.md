---
title: "マルチタッチ本の指の追跡"
description: "このドキュメントが複数の指からタッチ イベントを追跡する方法を示します"
ms.topic: article
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: a9e3842611aab86d23a2b0c2a832efce18c22465
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="multi-touch-finger-tracking"></a>マルチタッチ本の指の追跡

_このドキュメントが複数の指からタッチ イベントを追跡する方法を示します_

マルチタッチ アプリケーションが画面に同時に移動する個々 の本の指を追跡する必要がある場合もあります。 1 つの一般的なアプリケーションは、finger-paint プログラムです。 ユーザー 1 本の指を使用して描画するが、一度に複数の指を使用して描画するかどうかをできるようにします。 プログラムでは、複数のタッチ イベントを処理、指がこれらを区別する必要があります。

指では、画面が最初に、iOS 作成、 [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/)その指のオブジェクト。 このオブジェクトは、同じように、指が画面上の移動し、画面で、この時点で、オブジェクトが破棄されてから離したです。 指を入力するプログラムが格納しないようにこの`UITouch`オブジェクトに直接できます。 代わりに、使用できる、 [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)型のプロパティ`IntPtr`これらを一意に識別する`UITouch`オブジェクト。

ほとんどの場合、個々 の本の指を追跡するプログラムは追跡タッチのディクショナリを保持します。 IOS プログラムのディクショナリのキーは、`Handle`特定指を識別する値。 ディクショナリの値は、アプリケーションによって異なります。 [フィンガー ペイント](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)プログラム (から解放するタッチ) 各本の指ストロークは、その指で描画される直線を表示するために必要なすべての情報を格納しているオブジェクトに関連付けられています。 プログラム定義の小さな`FingerPaintPolyline`この目的のためのクラス。

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

各多角形は、色、線の幅、および iOS グラフィックス[ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/)蓄積してが描画中に、行の複数のポイントを描画するオブジェクト。


次のコードのすべての残りの部分が含まれている、`UIView`という名前から派生した`FingerPaintCanvasView`です。 クラスは型のオブジェクトのディクショナリを保持する`FingerPaintPolyline`時の 1 つまたは複数の指で描画されるがアクティブにします。

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

このディクショナリを迅速に取得するビューを使うと、`FingerPaintPolyline`各本の指で関連付けられている情報に基づいて、`Handle`のプロパティ、`UITouch`オブジェクト。

`FingerPaintCanvasView`クラスも保持、`List`完了している多角形のオブジェクト。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

このオブジェクト`List`が描画された同じ順序で。

`FingerPaintCanvasView` によって定義された 5 つのメソッドをオーバーライド`View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

さまざまな`Touches`上書きは、多角形を構成する点を蓄積します。

[`Draw`] の上書きは、完了した多角形とし、実行中の多角形を描画します。

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

各、`Touches`上書き可能性のある 1 つまたは複数によって示される、複数の指のアクションを報告`UITouch`オブジェクトに格納されている、`touches`メソッドの引数。 `TouchesBegan`これらのオブジェクトをループ処理をオーバーライドします。 各`UITouch`オブジェクト、メソッドを作成して、新しい初期化`FingerPaintPolyline`オブジェクトから取得した指の初期位置を格納するなど、`LocationInView`メソッドです。 これは、`FingerPaintPolyline`にオブジェクトが追加、`InProgressPolylines`ディクショナリを使用して、`Handle`のプロパティ、`UITouch`辞書のキーとしてオブジェクト。

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

メソッドを呼び出すことによって終了`SetNeedsDisplay`への呼び出しを生成する、`Draw`をオーバーライドし、画面を更新します。

本の指または指が画面で、移動したとき、`View`が複数の呼び出しを取得、`TouchesMoved`をオーバーライドします。 このオーバーライドが同様にループ、`UITouch`オブジェクトに格納されている、`touches`引数グラフィックス パスを本の指の現在の場所を追加。

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

`touches`コレクションにはのみが含まれます`UITouch`オブジェクト、最後の呼び出し以降に移行された本の指の`TouchesBegan`または`TouchesMoved`です。 必要が生じた場合`UITouch`オブジェクトに対応する*すべて*指が画面に現在接続には、その情報があるを通じて、`AllTouches`のプロパティ、`UIEvent`メソッドの引数。

`TouchesEnded`オーバーライドが 2 つのジョブです。 グラフィックス パス、および転送に、最後のポイントを追加する必要があります、`FingerPaintPolyline`オブジェクトから、`inProgressPolylines`辞書、`completedPolylines`一覧。

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

`TouchesCancelled`上書きは単純に放棄によって処理される、`FingerPaintPolyline`ディクショナリ内のオブジェクト。

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

この処理により、完全に、[フィンガー ペイント](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)プログラムを個々 の本の指を追跡し、画面に結果を描画します。

[![](touch-tracking-images/image01.png "個々 の本の指の追跡と画面に結果の絵")](touch-tracking-images/image01.png#lightbox)

画面上の個々 の本の指を追跡し、それらを区別する方法を見てきましたようになりました。



## <a name="related-links"></a>関連リンク

- [等価の Xamarin Android ガイド](~/android/app-fundamentals/touch/touch-tracking.md)
- [フィンガー ペイント (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
