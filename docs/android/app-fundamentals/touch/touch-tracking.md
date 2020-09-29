---
title: Xamarin Android でのマルチタッチ指の追跡
description: このトピックでは、複数の指からタッチイベントを追跡する方法について説明します。
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 8c2e06d2700cd7b61c16fc993d807ca4d042a063
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455119"
---
# <a name="multi-touch-finger-tracking"></a>複数タッチの指トラッキング

_このトピックでは、複数の指からタッチイベントを追跡する方法について説明します。_

マルチタッチアプリケーションでは、画面上で同時に移動するたびに個々の指を追跡する必要がある場合があります。 1つの一般的なアプリケーションは、フィンガーペイントプログラムです。 ユーザーが1本の指で描画できるようにし、同時に複数の指で描画することもできます。 複数のタッチイベントを処理するプログラムでは、各指に対応するイベントを識別する必要があります。 Android にはこの目的のための ID コードが用意されていますが、そのコードを取得して処理するのは少し厄介です。

特定の指に関連付けられているすべてのイベントについて、ID コードは変わりません。 ID コードは、指が最初に画面に触れたときに割り当てられ、指が画面から離した後は無効になります。
これらの ID コードは一般に非常に小さな整数であり、Android はこれらを後のタッチイベントに再利用します。

ほとんどの場合、個々の指を追跡するプログラムは、タッチ追跡用のディクショナリを保持します。 Dictionary キーは、特定の指を識別する ID コードです。 ディクショナリ値は、アプリケーションによって異なります。 [FingerPaint](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)プログラムでは、各指ストローク (タッチからリリースまで) が、その指で描画された直線を描画するために必要なすべての情報を含むオブジェクトに関連付けられています。 プログラムは、 `FingerPaintPolyline` この目的のための小さいクラスを定義します。

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

各ポリラインには、色、ストロークの幅、および [`Path`](xref:Android.Graphics.Path) 描画されるときに線の複数の点を累積して表示するための Android グラフィックスオブジェクトがあります。

次に示すコードの残りの部分は、という派生物に含まれてい `View` `FingerPaintCanvasView` ます。 このクラスは、 `FingerPaintPolyline` 1 つまたは複数の指によってアクティブに描画されているときに、型のオブジェクトのディクショナリを保持します。

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

このディクショナリは、ビューが `FingerPaintPolyline` 特定の指に関連付けられている情報をすばやく取得できるようにします。

`FingerPaintCanvasView`また、クラスは、 `List` 完成したポリラインのオブジェクトも保持します。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

この内のオブジェクト `List` は、描画された順序と同じ順序になります。

`FingerPaintCanvasView` によって定義される2つのメソッドをオーバーライドし `View` ます。 [`OnDraw`](xref:Android.Views.View.OnDraw*)
および [`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*) に対して多くの機能強化が行われました。
そのオーバーライドでは `OnDraw` 、ビューは完成したポリラインを描画し、進行中のポリラインを描画します。

メソッドのオーバーライドは、 `OnTouchEvent` プロパティから値を取得することによって開始され `pointerIndex` `ActionIndex` ます。 この `ActionIndex` 値は複数の指を区別しますが、複数のイベント間では一貫していません。 そのため、を使用して、 `pointerIndex` メソッドからポインター値を取得し `id` `GetPointerId` ます。 この ID *は* 、複数のイベントにわたって一貫しています。

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

オーバーライドでは、プロパティではなく、ステートメントのプロパティが使用されていることに注意して `ActionMasked` `switch` `Action` ください。 理由は次のとおりです。

マルチタッチを扱う場合、プロパティは最初の `Action` 指が画面をタッチするための値を持ち、2番目と3番目の指が `MotionEventsAction.Down` `Pointer2Down` 画面に触れるように、との値を持ち `Pointer3Down` ます。 4番目と5番目の指が接触すると、 `Action` プロパティには、列挙体のメンバーに対応していない数値が含まれます `MotionEventsAction` 。 値のビットフラグの値を調べて、それらの意味を解釈する必要があります。

同様に、指が画面に接続したままである場合、 `Action` プロパティの値は、 `Pointer2Up` `Pointer3Up` 2 番目と3番目の指、および最初の指の値に `Up` なります。

プロパティは、 `ActionMasked` 複数の指を区別するためにプロパティと組み合わせて使用することを意図しているため、より少数の値を受け取り `ActionIndex` ます。 指が画面をタッチすると、プロパティは `MotionEventActions.Down` 最初の指とそれ以降の指でのみ同じに `PointerDown` なります。 指が画面から離れると、は、後続の指 `ActionMasked` `Pointer1Up` と最初の指の値を持ち `Up` ます。

を使用する場合、 `ActionMasked` は `ActionIndex` 後続の指を区別して画面を離れるようにしますが、通常は、オブジェクト内の他のメソッドの引数として、その値を使用する必要はありません `MotionEvent` 。 マルチタッチの場合、上記のコードでは、これらのメソッドの中で最も重要なものの1つが `GetPointerId` 呼び出されます。 このメソッドは、特定のイベントを指に関連付けるためにディクショナリキーに使用できる値を返します。

`OnTouchEvent` [FingerPaint](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)プログラムのオーバーライドは、 `MotionEventActions.Down` `PointerDown` 新しいオブジェクトを作成 `FingerPaintPolyline` し、それをディクショナリに追加することで、イベントとイベントをまったく同じように処理します。

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

は、 `pointerIndex` ビュー内の指の位置を取得するためにも使用されていることに注意してください。 すべてのタッチ情報は、値に関連付けられてい `pointerIndex` ます。 は、 `id` 複数のメッセージ間で指を一意に識別するため、を使用してディクショナリエントリを作成します。

同様に、オーバーライドは、 `OnTouchEvent` `MotionEventActions.Up` 完了したポリラインをコレクションに転送することで、との両方を同じように処理し、 `Pointer1Up` `completedPolylines` オーバーライド中に描画できるようにし `OnDraw` ます。 また、このコードは `id` ディクショナリからエントリを削除します。

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

では、注意を要する部分です。

ダウンイベントと上位イベントの間には、通常、多数のイベントがあり `MotionEventActions.Move` ます。 これらは、の1回の呼び出しでバンドルされ、 `OnTouchEvent` イベントとイベントとは異なる方法で処理する必要があり `Down` `Up` ます。 `pointerIndex`プロパティから取得した値は `ActionIndex` 無視する必要があります。 代わりに、メソッドは、 `pointerIndex` 0 とプロパティの間でループして複数の値を取得 `PointerCount` し、その値ごとにを取得する必要があり `id` `pointerIndex` ます。

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

この種の処理では、 [FingerPaint](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) プログラムが個々の指を追跡し、画面に結果を描画できます。

[![FingerPaint の例のスクリーンショットの例](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々の指を追跡し、それらを区別する方法がわかりました。

## <a name="related-links"></a>関連リンク

- [同等の Xamarin iOS ガイド](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)