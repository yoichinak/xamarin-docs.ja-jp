---
title: Xamarin Android でのマルチタッチ指の追跡
description: このトピックでは、複数の指からタッチイベントを追跡する方法について説明します。
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 26dfc4f9327f12d6854d72349dc46e0b4427fa72
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643926"
---
# <a name="multi-touch-finger-tracking"></a>複数タッチの指トラッキング

_このトピックでは、複数の指からタッチイベントを追跡する方法について説明します。_

マルチタッチアプリケーションでは、画面上で同時に移動するたびに個々の指を追跡する必要がある場合があります。 1つの一般的なアプリケーションは、フィンガーペイントプログラムです。 ユーザーが1本の指で描画できるようにし、同時に複数の指で描画することもできます。 複数のタッチイベントを処理するプログラムでは、各指に対応するイベントを識別する必要があります。 Android にはこの目的のための ID コードが用意されていますが、そのコードを取得して処理するのは少し厄介です。

特定の指に関連付けられているすべてのイベントについて、ID コードは変わりません。 ID コードは、指が最初に画面に触れたときに割り当てられ、指が画面から離した後は無効になります。
これらの ID コードは一般に非常に小さな整数であり、Android はこれらを後のタッチイベントに再利用します。

ほとんどの場合、個々の指を追跡するプログラムは、タッチ追跡用のディクショナリを保持します。 Dictionary キーは、特定の指を識別する ID コードです。 ディクショナリ値は、アプリケーションによって異なります。 [FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)プログラムでは、各指ストローク (タッチからリリースまで) が、その指で描画された直線を描画するために必要なすべての情報を含むオブジェクトに関連付けられています。 プログラムは、この目的`FingerPaintPolyline`のための小さいクラスを定義します。

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

各ポリラインには、色、ストロークの幅、および描画さ[`Path`](xref:Android.Graphics.Path)れるときに線の複数の点を累積して表示するための Android グラフィックスオブジェクトがあります。

次に示すコードの残りの部分は、と`View`いう`FingerPaintCanvasView`派生物に含まれています。 このクラスは、1つまたは複数`FingerPaintPolyline`の指によってアクティブに描画されているときに、型のオブジェクトのディクショナリを保持します。

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

このディクショナリは、ビューが特定の指`FingerPaintPolyline`に関連付けられている情報をすばやく取得できるようにします。

また`FingerPaintCanvasView` 、クラスは、 `List`完成したポリラインのオブジェクトも保持します。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

この`List`内のオブジェクトは、描画された順序と同じ順序になります。

`FingerPaintCanvasView`によって定義さ`View`れる2つのメソッドをオーバーライドします。[`OnDraw`](xref:Android.Views.View.OnDraw*)
および[`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*)。
その`OnDraw`オーバーライドでは、ビューは完成したポリラインを描画し、進行中のポリラインを描画します。

`OnTouchEvent`メソッドのオーバーライドは、 `ActionIndex`プロパティから値を`pointerIndex`取得することによって開始されます。 この`ActionIndex`値は複数の指を区別しますが、複数のイベント間では一貫していません。 そのため、を使用`pointerIndex`して、 `GetPointerId`メソッドからポインター `id`値を取得します。 この ID*は*、複数のイベントにわたって一貫しています。

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

オーバーライドでは、 `ActionMasked` `Action`プロパティではなく、 `switch`ステートメントのプロパティが使用されていることに注意してください。 その理由を説明します。

マルチタッチを扱う場合`Action` 、プロパティは最初の指が画面をタッチする`MotionEventsAction.Down`ための値を持ち、2番目と3番目`Pointer3Down`の指が画面に触れるように、との`Pointer2Down`値を持ちます。 4番目と5番目の指が`Action`接触すると、プロパティには、 `MotionEventsAction`列挙体のメンバーに対応していない数値が含まれます。 値のビットフラグの値を調べて、それらの意味を解釈する必要があります。

同様に、指が画面に接続したままで`Action`ある場合、プロパティ`Pointer2Up`の`Pointer3Up`値は、2番目と 3 `Up`番目の指、および最初の指の値になります。

プロパティは、複数の指を区別するために`ActionIndex`プロパティと組み合わせて使用することを意図しているため、より少数の値を受け取ります。 `ActionMasked` 指が画面をタッチすると、プロパティは最初`MotionEventActions.Down`の指と`PointerDown`それ以降の指でのみ同じになります。 指が画面から離れると、 `ActionMasked`は、後続`Pointer1Up`の指と`Up`最初の指の値を持ちます。

を使用`ActionMasked`する場合`ActionIndex` 、は後続の指を区別して画面を離れるようにしますが、通常は、 `MotionEvent`オブジェクト内の他のメソッドの引数として、その値を使用する必要はありません。 マルチタッチの場合、上記のコードでは、これらのメソッド`GetPointerId`の中で最も重要なものの1つが呼び出されます。 このメソッドは、特定のイベントを指に関連付けるためにディクショナリキーに使用できる値を返します。

[FingerPaint](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) プログラムの `OnTouchEvent` オーバーライドは、新しい `FingerPaintPolyline` オブジェクトを作成し、それをディクショナリに追加することで、`MotionEventActions.Down` イベントと `PointerDown` イベントをまったく同じように処理します。

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

は、 `pointerIndex`ビュー内の指の位置を取得するためにも使用されていることに注意してください。 すべてのタッチ情報は、 `pointerIndex`値に関連付けられています。 は`id` 、複数のメッセージ間で指を一意に識別するため、を使用してディクショナリエントリを作成します。

同様に、 `OnTouchEvent`オーバーライドは、完了`MotionEventActions.Up`し`Pointer1Up`たポリラインを`completedPolylines`コレクションに転送することで、との両方を同じよう`OnDraw`に処理し、オーバーライド中に描画できるようにします。 また、このコードは`id`ディクショナリからエントリを削除します。

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

ダウンイベントと上位イベントの間には、通常`MotionEventActions.Move` 、多数のイベントがあります。 これらは`OnTouchEvent`、の1回の呼び出しでバンドルされ、イベント`Down`とイベントと`Up`は異なる方法で処理する必要があります。 プロパティから取得した値は`pointerIndex`無視する必要があります。 `ActionIndex` 代わりに、 `pointerIndex`メソッドは、0 `PointerCount`とプロパティの間でループして複数の値を取得し`id` 、その`pointerIndex`値ごとにを取得する必要があります。

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

[![FingerPaint の例のスクリーンショットの例](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々の指を追跡し、それらを区別する方法がわかりました。


## <a name="related-links"></a>関連リンク

- [同等の Xamarin iOS ガイド](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
