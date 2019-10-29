---
title: Xamarin Android でのマルチタッチ指の追跡
description: このトピックでは、複数の指からタッチイベントを追跡する方法について説明します。
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: f960c3cec90bd331f5a1433a869c7720b40c9680
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024275"
---
# <a name="multi-touch-finger-tracking"></a>複数タッチの指トラッキング

_このトピックでは、複数の指からタッチイベントを追跡する方法について説明します。_

マルチタッチアプリケーションでは、画面上で同時に移動するたびに個々の指を追跡する必要がある場合があります。 1つの一般的なアプリケーションは、フィンガーペイントプログラムです。 ユーザーが1本の指で描画できるようにし、同時に複数の指で描画することもできます。 複数のタッチイベントを処理するプログラムでは、各指に対応するイベントを識別する必要があります。 Android にはこの目的のための ID コードが用意されていますが、そのコードを取得して処理するのは少し厄介です。

特定の指に関連付けられているすべてのイベントについて、ID コードは変わりません。 ID コードは、指が最初に画面に触れたときに割り当てられ、指が画面から離した後は無効になります。
これらの ID コードは一般に非常に小さな整数であり、Android はこれらを後のタッチイベントに再利用します。

ほとんどの場合、個々の指を追跡するプログラムは、タッチ追跡用のディクショナリを保持します。 Dictionary キーは、特定の指を識別する ID コードです。 ディクショナリ値は、アプリケーションによって異なります。 [FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)プログラムでは、各指ストローク (タッチからリリースまで) が、その指で描画された直線を描画するために必要なすべての情報を含むオブジェクトに関連付けられています。 このプログラムでは、この目的のために小さい `FingerPaintPolyline` クラスを定義します。

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

各ポリラインには、色、ストロークの幅、および Android グラフィックス[`Path`](xref:Android.Graphics.Path)オブジェクトがあり、描画中に線の複数の点を蓄積してレンダリングします。

次に示すコードの残りの部分は、`FingerPaintCanvasView`という `View` の派生物に含まれています。 このクラスは、1つまたは複数の指によってアクティブに描画されている間に `FingerPaintPolyline` 型のオブジェクトのディクショナリを保持します。

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

このディクショナリは、ビューが特定の指に関連付けられている `FingerPaintPolyline` 情報をすばやく取得できるようにします。

`FingerPaintCanvasView` クラスは、完成したポリラインの `List` オブジェクトも保持します。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

この `List` のオブジェクトは、描画された順序と同じ順序になっています。

`View`で定義されている2つのメソッドをオーバーライドする `FingerPaintCanvasView`: [`OnDraw`](xref:Android.Views.View.OnDraw*)
および[`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*)ます。
`OnDraw` のオーバーライドでは、ビューは完成したポリラインを描画し、進行中のポリラインを描画します。

`OnTouchEvent` メソッドのオーバーライドは、`ActionIndex` プロパティから `pointerIndex` 値を取得することによって開始されます。 この `ActionIndex` 値は複数の指を区別しますが、複数のイベント間では一貫していません。 そのため、`pointerIndex` を使用して、`GetPointerId` メソッドからポインター `id` 値を取得します。 この ID*は*、複数のイベントにわたって一貫しています。

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

オーバーライドでは、`Action` プロパティではなく、`switch` ステートメントの `ActionMasked` プロパティが使用されていることに注意してください。 その理由を説明します。

マルチタッチを扱う場合、`Action` プロパティの値は、最初の指が画面に触れるときに `MotionEventsAction.Down` になります。2番目と3本目の指と同じように `Pointer2Down` と `Pointer3Down` の値も画面に接します。 4番目と5番目の指が接触すると、`Action` プロパティには、`MotionEventsAction` 列挙体のメンバーに対応していない数値が含まれます。 値のビットフラグの値を調べて、それらの意味を解釈する必要があります。

同様に、指が画面との接点を離れると、`Action` プロパティの値は、2番目と3本の `Pointer2Up` と `Pointer3Up` の値になり、最初の指の `Up` ます。

`ActionMasked` プロパティは、複数の指を区別するために `ActionIndex` プロパティと組み合わせて使用することを意図しているため、より多くの値を受け取ります。 指が画面をタッチすると、プロパティは最初の指の `MotionEventActions.Down` と同じになり、それ以降の指では `PointerDown` できます。 指が画面から離れると、`ActionMasked` には、その後の指と最初の指の `Up` に対して `Pointer1Up` の値が与えられます。

`ActionMasked`を使用する場合、`ActionIndex` は、タッチするために後続の指を区別して画面を離れることができますが、通常は `MotionEvent` オブジェクト内の他のメソッドの引数として使用する以外に、その値を使用する必要はありません。 マルチタッチの場合、上記のコードでは、これらのメソッドのうち最も重要なものの1つが `GetPointerId` 呼び出されます。 このメソッドは、特定のイベントを指に関連付けるためにディクショナリキーに使用できる値を返します。

[FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)プログラムの `OnTouchEvent` オーバーライドは、新しい `FingerPaintPolyline` オブジェクトを作成し、それをディクショナリに追加することで、`MotionEventActions.Down` と `PointerDown` イベントを同じように処理します。

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

`pointerIndex` がビュー内の指の位置を取得するためにも使用されていることに注意してください。 すべてのタッチ情報は、`pointerIndex` 値に関連付けられています。 `id` は、複数のメッセージ間で指を一意に識別するため、を使用してディクショナリエントリを作成します。

同様に、`OnTouchEvent` のオーバーライドは、完了したポリラインを `completedPolylines` コレクションに転送することによって、`MotionEventActions.Up` と `Pointer1Up` 同じように処理します。これにより、`OnDraw` のオーバーライド中に描画できるようになります。 このコードでは、ディクショナリからも `id` エントリが削除されます。

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

Down イベントと up イベントの間には、通常、多数の `MotionEventActions.Move` イベントがあります。 これらは `OnTouchEvent`の1回の呼び出しにバンドルされ、`Down` イベントと `Up` イベントとは異なる方法で処理する必要があります。 `ActionIndex` プロパティから取得した `pointerIndex` 値は無視する必要があります。 代わりに、メソッドは、0と `PointerCount` プロパティの間をループして複数の `pointerIndex` 値を取得し、それらの `pointerIndex` 値ごとに `id` を取得する必要があります。

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

この種の処理では、 [FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)プログラムが個々の指を追跡し、画面に結果を描画できます。

[![サンプルのスクリーンショット FingerPaint の例](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々の指を追跡し、それらを区別する方法がわかりました。

## <a name="related-links"></a>関連リンク

- [同等の Xamarin iOS ガイド](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
