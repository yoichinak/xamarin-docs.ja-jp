---
title: 呼び出し元のイベントの効果を適用
description: 特殊効果では、定義でき、基になるネイティブ ビュー内の変更の通知、イベントを起動することができます。 この記事では、追跡、低レベルのマルチタッチ指を実装する方法とタッチのアクティビティを通知するイベントを生成する方法を示します。
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: e363cae4dd72a25e4768395410d4e56a8db30eba
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="invoking-events-from-effects"></a>呼び出し元のイベントの効果を適用

_特殊効果では、定義でき、基になるネイティブ ビュー内の変更の通知、イベントを起動することができます。この記事では、追跡、低レベルのマルチタッチ指を実装する方法とタッチのアクティビティを通知するイベントを生成する方法を示します。_

この記事で説明されている効果は、低レベルのタッチ イベントへのアクセスを提供します。 これらの低レベルのイベントは、既存で使用できない`GestureRecognizer`クラスが、いくつかの種類のアプリケーションに不可欠なです。 たとえば、画面上を移動すると、個々 の本の指を追跡するためにアプリケーションのニーズを finger-paint です。 音楽キーボードでは、タップを検出する必要があり、個々 のキーだけでなく、glissando 内の別の 1 つのキーから滑空指で解放します。

効果は、Xamarin.Forms の任意の要素に関連付けることができますのでマルチタッチ指が追跡に最適です。

## <a name="platform-touch-events"></a>プラットフォーム タッチ イベント

IOS、Android、およびユニバーサル Windows プラットフォームがすべて検出するためにアプリケーションを使用する低水準の API が含まれて、アクティビティを修正します。 これらの 3 つの基本的な種類の区別のすべてのプラットフォームはタッチ イベントです。

- *押された*指が画面をタッチすると、
- *移動*画面に触れて指を動かしたときに、
- *リリース*指が画面から離れるときに、

マルチタッチ環境で複数の指は、同時に、画面をタッチすることができます。 さまざまなプラットフォームには、アプリケーションが複数の指を区別するために使用できる識別 (ID) 番号が含まれます。

Ios では、`UIView`クラスは、次の 3 つのオーバーライド可能なメソッドを定義`TouchesBegan`、 `TouchesMoved`、および`TouchesEnded`これら 3 つの基本的なイベントに対応します。 アーティクル[マルチタッチ指追跡](~/ios/app-fundamentals/touch/touch-tracking.md)これらのメソッドを使用する方法について説明します。 ただし、iOS プログラムがから派生するクラスをオーバーライドする必要ありません`UIView`これらのメソッドを使用します。 IOS`UIGestureRecognizer`も定義するこれらと同じ 3 つのメソッド、およびするがから派生したクラスのインスタンスにアタッチできる`UIGestureRecognizer`に`UIView`オブジェクト。

Android で、`View`クラスという名前のオーバーライド可能なメソッドを定義する`OnTouchEvent`タッチ アクティビティのすべてを処理します。 タッチ アクティビティの型が列挙体メンバーで定義されている`Down`、 `PointerDown`、 `Move`、`Up`と`PointerUp`資料」の説明に従って[マルチタッチ指追跡](~/android/app-fundamentals/touch/touch-tracking.md)です。 Android`View`もという名前のイベントを定義`Touch`いずれかにアタッチされるイベント ハンドラーを利用できる`View`オブジェクト。

ユニバーサル Windows プラットフォーム (UWP) で、`UIElement`クラスという名前のイベントを定義する`PointerPressed`、 `PointerMoved`、および`PointerReleased`です。 これらについては、記事[ポインター入力の処理についての MSDN](/windows/uwp/input-and-devices/handle-pointer-input/)と API のドキュメントを[ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/)クラスです。

`Pointer`マウス、タッチ、ペン入力を統一するユニバーサル Windows プラットフォームの API が対象としています。 そのため、`PointerMoved`マウス ボタンが押されていない場合でも、要素間で、マウスを動かしたときにイベントが呼び出されます。 `PointerRoutedEventArgs`これらのイベントに付属しているオブジェクトという名前のプロパティには`Pointer`という名前のプロパティを持つ`IsInContact`マウス ボタンが押されたか、指が画面に接続がのかどうかを示します。

さらに、2 つ以上のイベントという名前の定義、UWP`PointerEntered`と`PointerExited`です。 これらは、移動すると、マウスや指 1 つの要素から別に示します。 たとえば、A と B という 2 つの隣接する要素両方の要素ポインター イベントのハンドラーがインストールされました。 A 上で指を押したとき、`PointerPressed`イベントが呼び出されます。 指が移動するとき、A を呼び出す`PointerMoved`イベント。 A が呼び出す指は A から B に移動した場合、`PointerExited`イベントと B を呼び出す、`PointerEntered`イベント。 指を解放し、場合 B を呼び出して、`PointerReleased`イベント。

IOS および Android プラットフォームは、UWP と異なる: 最初の呼び出しを取得するビュー`TouchesBegan`または`OnTouchEvent`に触れると、ビュー、引き続き指が別のビューへ移動した場合、タッチ アクティビティでも場合、すべてを取得します。 アプリケーションは、ポインターをキャプチャする場合に、UWP によって同様に動作: で、`PointerEntered`イベント ハンドラー、要素のコール`CapturePointer`し、その指からタッチのすべてのアクティビティを取得します。

UWP アプローチは、アプリケーション、音楽キーボードなどの種類によっては非常に役に立つに証明します。 各キーはそのキーのタッチ イベントの処理し、指を使用して 1 つのキーからスライドが検出、`PointerEntered`と`PointerExited`イベント。

そのため、この記事で説明されているタッチ追跡効果は、UWP アプローチを実装します。

## <a name="the-touch-tracking-effect-api"></a>タッチ追跡効果 API

[**追跡効果デモのタッチ**](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)サンプルには、低レベルのタッチの追跡を実装するクラス (および列挙) が含まれています。 これらの型が、名前空間に属する`TouchTracking`という単語で始まる`Touch`です。 **TouchTrackingEffectDemos** .NET 標準のライブラリ プロジェクトに含まれる、`TouchActionType`タッチ イベントの種類を列挙します。

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

すべてのプラットフォームでは、タッチ イベントが取り消されましたことを示すイベントも含まれます。

`TouchEffect` .NET 標準ライブラリ内のクラスから派生`RoutingEffect`という名前のイベントを定義および`TouchAction`という名前のメソッドと`OnTouchAction`を呼び出す、`TouchAction`イベント。

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

また、`Capture`プロパティです。 タッチ イベントをキャプチャするアプリケーションこのプロパティを設定する必要があります`true`より前のバージョン、`Pressed`イベント。 それ以外の場合、タッチ イベントは、ユニバーサル Windows プラットフォームのように動作します。

`TouchActionEventArgs` .NET 標準ライブラリ内のクラスには、各イベントに付属しているすべての情報が含まれています。

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

アプリケーションで使用できます、`Id`個々 の本の指を追跡するためのプロパティです。 通知、`IsInContact`プロパティです。 このプロパティは常に`true`の`Pressed`イベントと`false`の`Released`イベント。 常に`true`の`Moved`iOS および Android 上のイベントです。 `IsInContact`プロパティがあります`false`の`Moved`デスクトップでプログラムが実行され、ボタンなくマウス ポインターを動かしたときに、ユニバーサル Windows プラットフォーム上のイベントが押されました。

使用することができます、`TouchEffect`にインスタンスを追加して、ソリューションの標準の .NET ライブラリ プロジェクトで、ファイルを含むによって独自のアプリケーション内のクラス、 `Effects` Xamarin.Forms の任意の要素のコレクション。 ハンドラーをアタッチ、`TouchAction`タッチ イベントを取得するイベントです。

使用する`TouchEffect`独自のアプリケーションに含まれるプラットフォームの実装を必要がありますも**TouchTrackingEffectDemos**ソリューションです。

## <a name="the-touch-tracking-effect-implementations"></a>タッチの追跡の効果の実装

IOS、Android、および UWP の実装、`TouchEffect`最も簡単な実装 (UWP) で始まると、他のユーザーよりもより構造的に複雑になっているために、iOS の実装で終わる次に示します。

### <a name="the-uwp-implementation"></a>UWP 実装では、

UWP 実装`TouchEffect`が、最も簡単です。 通常どおり、クラスの派生元`PlatformEffect`と 2 つのアセンブリ属性が含まれています。

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

`OnAttached`上書きがすべてのポインター イベントへのフィールドとアタッチのハンドラーとしていくつかの情報を保存します。

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

`OnPointerPressed`ハンドラーが呼び出すことによって効果イベントを呼び出す、`onTouchAction`フィールドで、`CommonHandler`メソッド。

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed` またの値を調べて、 `Capture` .NET 標準ライブラリの呼び出しで効果クラスのプロパティの`CapturePointer`である場合`true`です。

 その他の UWP イベント ハンドラーよりシンプルになりました。

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Android の実装

Android および iOS の実装は、必ずしも複雑に実装する必要がありますので、`Exited`と`Entered`指が 1 つの要素を移動する場合のイベントです。 どちらの実装は、同様に構成されています。

Android`TouchEffect`クラスのハンドラーをインストールする、`Touch`イベント。

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

クラスは、2 つの静的ディクショナリも定義します。

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

`viewDictionary`たびに新しいエントリを取得、`OnAttached`オーバーライドが呼び出されます。

```csharp
viewDictionary.Add(view, this);
```

辞書からエントリを削除`OnDetached`です。 すべてのインスタンス`TouchEffect`に効果が関連付けられている特定のビューに関連付けられています。 いずれかにより、静的ディクショナリ`TouchEffect`他のすべてのビューとそれに対応する順に列挙するインスタンス`TouchEffect`インスタンス。 これは、別の 1 つのビューからイベントを転送するために必要です。

Android では、個々 の本の指を追跡するためにアプリケーションを許可するタッチ イベントの ID コードを割り当てます。 `idToEffectDictionary`でこの ID のコードを関連付ける、`TouchEffect`インスタンス。 項目がこのディクショナリに追加されたときに、`Touch`ハンドラーが呼び出された本の指のキーを押します。

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

項目が削除された、`idToEffectDictionary`指を画面から解放する場合。 `FireEvent`メソッドを呼び出すために必要なすべての情報を蓄積するだけ、`OnTouchAction`メソッド。

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

タッチに関するその他のすべての種類は、2 つの方法で処理されます。 場合、`Capture`プロパティは`true`、タッチ イベントは、非常に単純に平行移動を、`TouchEffect`情報。 取得より複雑な場合に`Capture`は`false`タッチ イベントの 1 つのビューから別に移動するが必要になるためです。 これは、責任において、`CheckForBoundaryHop`移動イベント中に呼び出されるメソッド。 このメソッドの両方の静的ディクショナリを使用します。 これを列挙、`viewDictionary`指を変更することは現在、これを使用してビューの決定`idToEffectDictionary`現在を格納する`TouchEffect`インスタンス (および現在のビューがそのため、) 特定の ID に関連付けられています。

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

変更があった場合、 `idToEffectionDictionary`、メソッドを呼び出す可能性のある`FireEvent`の`Exited`と`Entered`別に送信する 1 つのビューからにします。 ただし、指が移動された可能性がないビュー添付によって占有されている領域に`TouchEffect`、または領域をビュー、効果が接続されているからです。

通知、`try`と`catch`ビューのアクセスをブロックします。 移動し、次に、ホーム ページに戻ります ページで、`OnDetached`メソッドが呼び出されないと、アイテム、 `viewDictionary` Android では、破棄両者と見なされるが、します。

### <a name="the-ios-implementation"></a>IOS の実装

IOS 実装は点を除いて実装では、Android、iOS`TouchEffect`クラスが派生物をインスタンス化する必要があります`UIGestureRecognizer`です。 これは、という名前の iOS プロジェクトでクラス`TouchRecognizer`です。 このクラスを格納する 2 つの静的ディクショナリを保持する`TouchRecognizer`インスタンス。

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

この構造体の程度`TouchRecognizer`クラスは、Android のような`TouchEffect`クラスです。

## <a name="putting-the-touch-effect-to-work"></a>作業をするタッチ効果を配置します。

[ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)プログラムに影響をテストするタッチの追跡の一般的なタスクの 5 つのページが含まれています。

**BoxView ドラッグ** ページでは、追加できます。`BoxView`要素を、`AbsoluteLayout`し、それらを画面にドラッグします。 [XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml)2 つのインスタンスを作成`Button`を追加するためのビュー`BoxView`要素を`AbsoluteLayout`オフにして、`AbsoluteLayout`です。

メソッドで、[分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs)加算した新しい`BoxView`に、`AbsoluteLayout`も追加されて、`TouchEffect`オブジェクトを`BoxView`効果にイベント ハンドラーをアタッチします。

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

`TouchAction`イベント ハンドラーは、すべてのすべてのタッチ イベントを処理、`BoxView`要素がいくつか注意する必要があります: 1 つの 2 本の指を許可することはできません、`BoxView`プログラムがのみドラッグすると、を実装し、2 本の指がため。互いに干渉します。 このため、ページには現在追跡されている各本の指の埋め込みクラスを定義します。

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

`dragDictionary`のエントリを含むすべて`BoxView`現在ドラッグされています。

`Pressed`タッチ操作がこのディクショナリに項目を追加し、`Released`アクションは削除されます。 `Pressed`ロジックが既に存在する項目をディクショナリ内をチェックする必要があります`BoxView`です。 場合は、`BoxView`は既にドラッグされると、新しいイベントは、1 秒あたりにお問い合わせに同じ`BoxView`です。 `Moved`と`Released`アクション、イベント ハンドラーの必要がありますチェックにディクショナリのエントリがあるかどうかは`BoxView`ことと、タッチ`Id`プロパティをドラッグ`BoxView`ディクショナリ エントリの構造と一致します。

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

`Pressed`ロジック セット、`Capture`のプロパティ、`TouchEffect`オブジェクトを`true`です。 これにより、その指の後続のすべてのイベントを同じイベント ハンドラーに配信する効果があります。

`Moved`ロジックの移動、`BoxView`変更することにより、`LayoutBounds`添付プロパティ。 `Location`イベント引数のプロパティが基準には常に、`BoxView`ドラッグされている場合は、`BoxView`が一定の割合でドラッグされている、`Location`連続したイベントのプロパティはほぼ同じになります。 指を押した場合など、 `BoxView` 、中央で、`Pressed`アクション ストア、`PressPoint`のプロパティ (50, 50) を変換後の後続のイベントの同じです。 場合、`BoxView`が、その後一定の割合で斜めにドラッグされた`Location`中にプロパティ、`Moved`アクションの値があります (55、55)、その場合、`Moved`ロジック 5 水平および垂直方向の位置に追加、の`BoxView`. これにより、移動、`BoxView`中心がもう一度ように指の直下にします。

複数を移動する`BoxView`要素を同時に異なる指を使用します。

[![](touch-tracking-images/boxviewdragging-small.png "トリプル ページのスクリーン ショット、BoxView ドラッグ")](touch-tracking-images/boxviewdragging-large.png#lightbox "トリプル ページのスクリーン ショット、BoxView をドラッグします。")

### <a name="subclassing-the-view"></a>ビューをサブクラス化

多くの場合、独自のタッチ イベントを処理する Xamarin.Forms 要素に簡単です。 **ドラッグ BoxView ドラッグ**ページの機能と同じ、 **BoxView ドラッグ** ページが要素のインスタンスでは、ユーザーのドラッグが、 [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs)派生したクラス`BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

コンス トラクターを作成し、アタッチ、 `TouchEffect`、設定と、`Capture`プロパティがそのオブジェクトが最初にインスタンス化されるときです。 ディクショナリは必要ありませんので、このクラス自体は格納`isBeingDragged`、 `pressPoint`、および`touchId`各本の指で関連付けられている値。 `Moved`処理を変更、`TranslationX`と`TranslationY`プロパティのためのロジックが機能場合でもの親、`DraggableBoxView`されませんが、`AbsoluteLayout`です。

### <a name="integrating-with-skiasharp"></a>SkiaSharp との統合

次の 2 つのデモがグラフィックスを必要し、この目的のため SkiaSharp を使用します。 について学習することができます[xamarin.forms を使用して SkiaSharp](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)これらの例を調査する前にします。 (「SkiaSharp 図面基本」と「SkiaSharp 行、およびパス」) は、最初の 2 つのアーティクルでは、ここで必要となるすべてのものについて説明します。

**楕円の描画** ページでは、画面に指をスワイプして楕円を描画することができます。 左から右、下からまたは反対側の隅にその他のすべてのコーナーからは、指を移動する方法によっては、楕円を描画できます。 ランダムな色の不透明度と楕円が描画されます。

[![](touch-tracking-images/ellipsedrawing-small.png "楕円の描画のページのスクリーン ショットをトリプル")](touch-tracking-images/ellipsedrawing-large.png#lightbox "楕円の描画のページのトリプル スクリーン ショット")

省略記号のいずれかをタッチする場合、は、別の場所にドラッグできます。 これには、「ヒット テスト、」特定の時点でグラフィック オブジェクトの検索が行われると呼ばれる手法が必要です。 SkiaSharp 省略記号は、Xamarin.Forms 要素ため実行できず、独自に`TouchEffect`を処理します。 `TouchEffect`全体に適用する必要があります`SKCanvasView`オブジェクト。

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml)ファイルをインスタンス化、 `SKCanvasView` 1 つのセルで`Grid`です。 `TouchEffect`にオブジェクトがアタッチされる`Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Android およびユニバーサル Windows プラットフォームで、`TouchEffect`に直接関連付けることができます、`SKCanvasView`が、それでも iOS でします。 注意して、`Capture`プロパティに設定されている`true`です。

SkiaSharp をレンダリングするそれぞれの楕円は、型のオブジェクトによって表される`EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

`StartPoint`と`EndPoint`プログラムがタッチ入力を処理するときにプロパティが使用される、`Rectangle`プロパティは、楕円を描画するために使用します。 `LastFingerLocation`プロパティ活躍楕円がドラッグされているときに、`IsInEllipse`ヒット テスト メソッドを支援します。 このメソッドを返します`true`楕円の内部のポイントがある場合。

[分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs)3 つのコレクションが保持されます。

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure`ディクショナリにはサブセットが含まれています、`completedFigures`コレクション。 SkiaSharp`PaintSurface`イベント ハンドラーは、これらのオブジェクトを単にレンダリング、`completedFigures`と`inProgressFigures`コレクション。

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

タッチ処理最も難しいは、`Pressed`を処理します。 ここでは、ヒット テストが実行されますが、コードは、検出された場合、ユーザーの本の指の下にある省略記号、その楕円のみドラッグできる別の指がされていないドラッグされている場合。 ユーザーの本の指の下にある省略記号がない場合、コードは新しい楕円を描画のプロセスを開始します。

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

その他の SkiaSharp 例は、**指ペイント**ページ。 線の色および線の幅を 2 つから選択できます`Picker`のビューし、し、1 つまたは複数の指を使用して描画します。

[![](touch-tracking-images/fingerpaint-small.png "指ペイント ページのスクリーン ショットをトリプル")](touch-tracking-images/fingerpaint-large.png#lightbox "指ペイント ページのトリプル スクリーン ショット")

この例では、画面に描画された各行を表す別のクラスも必要です。

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

`SKPath`オブジェクトがそれぞれの線を表示するために使用します。 [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs)ファイルは、これらのオブジェクト、現在描画されているこれらの多角形の 1 つと完了した多角形の 2 つのコレクションを管理します。

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed`処理が新たに作成`FingerPaintPolyline`、呼び出し`MoveTo`最初のポイントを格納するパス オブジェクトでそのオブジェクトを追加し、`inProgressPolylines`ディクショナリ。 `Moved`呼び出しの処理`LineTo`新しいの本の指の位置と、パス オブジェクトで、`Released`処理から完成した多角形の転送`inProgressPolylines`に`completedPolylines`です。 もう一度、実際の SkiaSharp 描画コードが比較的単純です。

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>追跡ビューをタッチ

前の例をすべて設定、`Capture`のプロパティ、`TouchEffect`に`true`、いずれかの場合、`TouchEffect`が作成された場合や、`Pressed`イベントが発生しました。 これにより、同じ要素が指を最初に押されたビューに関連付けられているすべてのイベントを受信します。 最後のサンプルでは、*いない*設定`Capture`に`true`です。 これにより、接続画面に指を 1 つの要素から移動する際に、異なる動作が原因です。 指を移動する要素を持つイベントを受信する、`Type`プロパティに設定`TouchActionType.Exited`2 番目の要素を持つイベントを受信して、`Type`設定`TouchActionType.Entered`です。

この種のタッチ処理は、音楽のキーボードで非常に便利です。 キーが押されたときも指が別の 1 つのキーからスライドするときを検出するためにできる必要があります。

**サイレント キーボード**ページを小さな定義[ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs)と[ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs)から派生したクラス[ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs)から派生した`BoxView`です。

`Key`クラスは、実際の音楽プログラムで使用できるようにします。 という名前のパブリック プロパティを定義`IsPressed`と`KeyNumber`、これは、MIDI 標準によって確立されたキー コードに設定するためのものです。 `Key`クラスもという名前のイベントを定義`StatusChanged`、ときに呼び出される、`IsPressed`プロパティが変更されました。

複数の指は、各キーで許可されます。 このため、`Key`クラスを保持、`List`現在そのキーと接触しているすべての指タッチ ID の数値の。

```csharp
List<long> ids = new List<long>();
```

`TouchAction`イベント ハンドラーを追加するための ID、`ids`両方の一覧、`Pressed`イベントの種類と`Entered`型であり、場合にのみ、`IsInContact`プロパティは`true`の`Entered`イベント。 ID はから削除、`List`の`Released`または`Exited`イベント。

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

`AddToList`と`RemoveFromList`両方のメソッドかどうか確認、`List`空および空以外の場合、間で変更された場合を呼び出すと、`StatusChanged`イベント。

さまざまな`WhiteKey`と`BlackKey`要素をページの 配置[XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml)電話が横モードで保持されている場合に最適ななります。

[![](touch-tracking-images/silentkeyboard-small.png "サイレント [キーボード] ページのスクリーン ショットをトリプル")](touch-tracking-images/silentkeyboard-large.png#lightbox "サイレント [キーボード] ページのトリプル スクリーン ショット")

場合は、キーに指をスイープするはずの色に若干の変更によって別に 1 つのキーからタッチ イベントが転送されることです。

## <a name="summary"></a>まとめ

この記事には、特殊効果でイベントを起動する方法と記述および低レベルのマルチタッチ処理を実装する効果を使用する方法を説明しました。


## <a name="related-links"></a>関連リンク

- [IOS でマルチタッチ本の指の追跡](~/ios/app-fundamentals/touch/touch-tracking.md)
- [マルチタッチ指が Android での追跡](~/android/app-fundamentals/touch/touch-tracking.md)
- [タッチ追跡効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
