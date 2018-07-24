---
title: マルチタッチ指 Xamarin.Android での追跡
description: このトピック複数の指からタッチ イベントを追跡する方法を示します
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 97e848e74a4513c55c0bf50258621c010b347fcd
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436607"
---
# <a name="multi-touch-finger-tracking"></a>マルチタッチ本の指の追跡

_このトピック複数の指からタッチ イベントを追跡する方法を示します_

マルチタッチ アプリケーションが画面に同時に移動する個々 の本の指を追跡する必要がある場合もあります。 1 つの一般的なアプリケーションは、finger-paint プログラムです。 ユーザー 1 本の指を使用して描画するが、一度に複数の指を使用して描画するかどうかをできるようにします。 プログラムは、複数のタッチ イベントを処理する各指に対応するどのイベントを区別する必要があります。 Android は、この目的で ID コードを指定されているのにを取得して、そのコードの処理を少し複雑になることができます。

特定の指に関連付けられているすべてのイベント ID コードは変わりません。 ID のコードには、指は最初、画面に触れるし、指を画面から解除した後は無効になりますが割り当てられます。
これらの ID コードは一般に非常に小さい整数で、Android により再利用後タッチ イベントの。

ほとんどの場合、個々 の本の指を追跡するプログラムは追跡タッチのディクショナリを保持します。 ディクショナリのキーは、特定の指を識別する ID コードです。 ディクショナリの値は、アプリケーションによって異なります。 [フィンガー ペイント](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)プログラム (から解放するタッチ) 各本の指ストロークは、その指で描画される直線を表示するために必要なすべての情報を格納しているオブジェクトに関連付けられています。 プログラム定義の小さな`FingerPaintPolyline`この目的のためのクラス。

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

各多角形は、色、線の幅、および Android のグラフィックス[ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/)蓄積してが描画中に、行の複数のポイントを描画するオブジェクト。

次のコードの残りの部分が含まれている、`View`という名前から派生した`FingerPaintCanvasView`です。 クラスは型のオブジェクトのディクショナリを保持する`FingerPaintPolyline`時の 1 つまたは複数の指で描画されるがアクティブにします。

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

このディクショナリを迅速に取得するビューを使うと、`FingerPaintPolyline`特定指で関連付けられている情報。

`FingerPaintCanvasView`クラスも保持、`List`完了している多角形のオブジェクト。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

このオブジェクト`List`が描画された同じ順序で。

`FingerPaintCanvasView` によって定義された 2 つのメソッドをオーバーライド`View`: [ `OnDraw` ](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/)と[ `OnTouchEvent`](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/)です。
その`OnDraw`上書き、ビューが完了した多角形を描画し、実行中の多角形を描画します。

オーバーライド、`OnTouchEvent`メソッドを取得することによって開始、`pointerIndex`値から、`ActionIndex`プロパティです。 これは、`ActionIndex`値が、複数の指の間で区別は、一貫性のある複数のイベント間です。 そのため、使用する、`pointerIndex`ポインターを取得`id`値から、`GetPointerId`メソッドです。 この ID*は*複数のイベント間で一貫性のあります。

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

上書きを使用する通知、`ActionMasked`プロパティに、`switch`ステートメントではなく、`Action`プロパティです。 その理由を説明します。

、マルチタッチを扱うときに、`Action`プロパティの値を持つ`MotionEventsAction.Down`タッチ画面、およびしの値を、最初の指の`Pointer2Down`と`Pointer3Down`指が 2 番目と 3 番目はまた、画面をタッチするとします。 4 番目と 5 番目の指のように、連絡先、`Action`プロパティは、数値のメンバーにも対応していない、`MotionEventsAction`列挙! それぞれの意味を解釈する値のビット フラグの値を確認する必要があります。

同様に、ウィンドウで、連絡先、指と、`Action`プロパティの値は、`Pointer2Up`と`Pointer3Up`2 番目と 3 番目の指のおよび`Up`人差し指をします。

`ActionMasked`プロパティはより少ない数の値と組み合わせて使用するためのものではそのため、`ActionIndex`複数指を区別するためにプロパティです。 プロパティを指定できますのみ指が画面にタッチ、ときに`MotionEventActions.Down`、最初の指のおよび`PointerDown`後続の指のです。 画面のままにして、指と`ActionMasked`の値を持つ`Pointer1Up`後続の指のと`Up`最初の指の。

使用する場合`ActionMasked`、`ActionIndex`をタッチしその値以外の他のメソッドに渡す引数として使用する必要はありませんが、通常、画面のままにして後続の指の区別、`MotionEvent`オブジェクト。 マルチタッチ、最も重要なのいずれかのこれらのメソッドは`GetPointerId`上記のコードで呼び出されます。 メソッドを返す値を使用できます辞書のキーの本の指に特定のイベントを関連付けます。

`OnTouchEvent`内の上書き、[フィンガー ペイント](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)プログラムのプロセス、`MotionEventActions.Down`と`PointerDown`新たに作成する同じイベント`FingerPaintPolyline`オブジェクトとディクショナリに追加します。

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

注意して、`pointerIndex`はビュー内にある本の指の位置を取得するも使用します。 すべてのタッチ情報に関連付けられている、`pointerIndex`値。 `id`の辞書のエントリを作成するために使用できるように、複数のメッセージで指を一意に識別します。

同様に、`OnTouchEvent`オーバーライドもハンドル、`MotionEventActions.Up`と`Pointer1Up`に完了した多角形を転送することによって同じ、`completedPolylines`コレクション中に、描画できるように、`OnDraw`をオーバーライドします。 またコード、削除、`id`ディクショナリからのエントリ。

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

わかりにくい部分のようになりました。

下向きの間、イベントを一般に多`MotionEventActions.Move`イベント。 これらは、1 回の呼び出しにまとめられます。 `OnTouchEvent`、から異なる方法で処理する必要があります、`Down`と`Up`イベント。 `pointerIndex`から得た値、`ActionIndex`プロパティを無視する必要があります。 代わりに、メソッドが複数を取得する必要があります`pointerIndex`0 間でのループの値と`PointerCount`プロパティ、し、取得、`id`それらのすべての`pointerIndex`値。

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

この種の処理により、[フィンガー ペイント](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)個々 の本の指を追跡し、画面上の結果の描画プログラム。

[![フィンガー ペイント例からサンプルのスクリーン ショット](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

画面上の個々 の本の指を追跡し、それらを区別する方法を見てきましたようになりました。


## <a name="related-links"></a>関連リンク

- [等価の Xamarin iOS ガイド](~/ios/app-fundamentals/touch/touch-tracking.md)
- [フィンガー ペイント (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
