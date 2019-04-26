---
title: マルチタッチ指が Xamarin.Android での追跡
description: このトピックでは、複数の指のタッチ イベントを追跡する方法を示します
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 34a9d2d9b8acb05a1b978a70e85038507032faaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61011798"
---
# <a name="multi-touch-finger-tracking"></a>指のマルチタッチ追跡

_このトピックでは、複数の指のタッチ イベントを追跡する方法を示します_

マルチタッチ アプリケーションが同時に画面に移動する場合に、個々 の本の指を追跡する必要がある場合もあります。 1 つの一般的なアプリケーションは、finger-paint プログラムです。 1 本の指を描くには、一度に複数の指で描画するためにもできるようにユーザーを必要とします。 プログラムでは、複数のタッチ イベントを処理するそれぞれの指に対応しているイベントを区別する必要があります。 Android は、このため、ID コードを提供しますが、取得および処理するコードは多少難しくなることができます。

特定の指に関連付けられているすべてのイベント ID コードは変わりません。 ID のコードには、指は最初、画面に触れるし、画面から指を離した後は無効になりますが割り当てられます。
これらの ID コードは非常に小さくの整数で、Android がそれらのタッチ イベントの後で再利用されます。

ほぼ常に、個々 の本の指を追跡するプログラムは、タッチの追跡のためのディクショナリを保持します。 ディクショナリのキーは、特定の指を識別する ID コードです。 ディクショナリの値は、アプリケーションによって異なります。 [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)プログラム、(リリースへのタッチ) から各本の指のストロークはその指で描画される直線を表示するために必要なすべての情報を格納しているオブジェクトに関連付けられています。 プログラム定義の小さな`FingerPaintPolyline`この目的のためのクラス。

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

各ポリラインが、色、線の幅、および Android のグラフィックス[ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/)を蓄積し、描画されている行の複数のポイントをレンダリングするオブジェクト。

次のコードの残りの部分が含まれている、`View`という名前の導関数`FingerPaintCanvasView`します。 クラスは型のオブジェクトのディクショナリを保持する`FingerPaintPolyline`1 つ以上の指で積極的に描画されているは時間内に。

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

このディクショナリをすばやく取得するビューを使うと、`FingerPaintPolyline`特定の指に関連付けられた情報。

`FingerPaintCanvasView`クラスも保持しています、`List`完成したポリラインのオブジェクト。

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

このオブジェクト`List`が描画されたことと同じ順序で。

`FingerPaintCanvasView` によって定義された 2 つのメソッドをオーバーライド`View`: [`OnDraw`](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/)
[ `OnTouchEvent`](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/)します。
その`OnDraw`のオーバーライドでは、ビューが完了したポリラインを描画し、実行中のポリラインを描画します。

オーバーライド、`OnTouchEvent`メソッドの取得ではまず、`pointerIndex`値から、`ActionIndex`プロパティ。 これは、`ActionIndex`値は、複数の指の間で区別されますが、矛盾がある複数のイベント間。 そのため、使用する、`pointerIndex`ポインターを取得する`id`値から、`GetPointerId`メソッド。 この ID*は*複数のイベントの間で一貫性のあります。

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

上書きを使用する通知、`ActionMasked`プロパティ、`switch`ステートメントではなく、`Action`プロパティ。 その理由を説明します。

、マルチタッチを扱っているときに、`Action`プロパティの値を持つ`MotionEventsAction.Down`タッチ画面、およびしの値を 1 つ目の指の`Pointer2Down`と`Pointer3Down`ように、2 番目と 3 つ目の指が画面をタッチもできます。 4 番目と 5 番目の本の指での連絡先、`Action`プロパティが数値のメンバーにも対応していない、`MotionEventsAction`列挙! どのような意味を解釈する値のビット フラグの値を確認する必要があります。

同様に、指がウィンドウで、連絡先を残す、`Action`プロパティの値を持つ`Pointer2Up`と`Pointer3Up`、2 番目と 3 つ目の指と`Up`最初の指の。

`ActionMasked`プロパティがより少ないと組み合わせて使用するためのものがあるため、値の数、`ActionIndex`プロパティを複数の指を区別します。 プロパティが等しいことができますのみ本の指が画面をタッチと`MotionEventActions.Down`、最初の指のおよび`PointerDown`後続本の指の。 指が画面を残す`ActionMasked`の値を持つ`Pointer1Up`、後続の指と`Up`最初の指の。

使用する場合`ActionMasked`、`ActionIndex`後続の指タッチの他のメソッドに引数として以外は、その値を使用する必要はありませんが、通常は、画面のままにして区別、`MotionEvent`オブジェクト。 1 つの最も重要なマルチタッチのこれらのメソッドは`GetPointerId`上記のコードで呼び出されます。 メソッドを返す値を使用できます、ディクショナリ キーの本の指に特定のイベントを関連付けます。

`OnTouchEvent`で上書き、 [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)プログラムのプロセス、`MotionEventActions.Down`と`PointerDown`新しいを作成することで同じイベント`FingerPaintPolyline`オブジェクトと辞書に追加します。

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

なお、`pointerIndex`ビュー内で指の位置を取得することにも使用されます。 関連付けられたすべてのタッチ情報、`pointerIndex`値。 `id`のディクショナリ エントリを作成するために使用されるように、複数のメッセージで本の指を一意に識別します。

同様に、`OnTouchEvent`ハンドルをオーバーライドも、`MotionEventActions.Up`と`Pointer1Up`に完了したポリラインを転送することによって同じ、`completedPolylines`コレクション中に描画できるように、`OnDraw`をオーバーライドします。 コードも削除されます、`id`ディクショナリからエントリ。

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

難しいのようになりました。

リスト間と、イベントには、通常は多数あります`MotionEventActions.Move`イベント。 これらは、1 回の呼び出しにまとめられます。 `OnTouchEvent`、から異なる方法で処理する必要があります、`Down`と`Up`イベント。 `pointerIndex`から先ほど取得した値、`ActionIndex`プロパティを無視する必要があります。 代わりに、メソッドが複数を取得する必要があります`pointerIndex`値 0 の間でのループによって、`PointerCount`プロパティ、し、取得、`id`これらの各`pointerIndex`値。

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

この種の処理により、 [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)を個々 の本の指を追跡し、結果を画面に描画プログラム。

[![FingerPaint 例からサンプルのスクリーン ショット](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

これで、画面上の個々 の本の指を追跡し、それらを区別する方法を見てきました。


## <a name="related-links"></a>関連リンク

- [同等の Xamarin iOS」ガイドします。](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
