---
title: 効果からのイベントの呼び出し
description: 効果を定義し、基になるネイティブ ビューの変更をシグナル化イベントを呼び出します。 この記事では、追跡、低レベルのマルチタッチ指を実装する方法とタッチのアクティビティを通知するイベントを生成する方法を示します。
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 0a5e2c1a7a7807da91fd98e617467ea251a25bc0
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527405"
---
# <a name="invoking-events-from-effects"></a>効果からのイベントの呼び出し

_効果を定義し、基になるネイティブ ビューの変更をシグナル化イベントを呼び出します。この記事では、追跡、低レベルのマルチタッチ指を実装する方法とタッチのアクティビティを通知するイベントを生成する方法を示します。_

この記事で説明されている効果では、低レベルのタッチ イベントへのアクセスを提供します。 これらの低レベルのイベントでは、既存の使用できない`GestureRecognizer`いますが、クラスのいくつかの種類のアプリケーションに不可欠です。 たとえば、画面上を移動すると、個々 の本の指を追跡するためにアプリケーションのニーズを finger-paint します。 音楽のキーボードでは、タップを検出する必要があり、個々 のキーと同様に、glissando 内の別の 1 つのキーから滑空指を解放します。

効果は、Xamarin.Forms の任意の要素にアタッチできるため、マルチタッチの指の追跡に最適です。

## <a name="platform-touch-events"></a>プラットフォームのタッチ イベント

IOS、Android、およびユニバーサル Windows プラットフォームがすべて検出するためにアプリケーションを使用する低レベルの API が含まれて、アクティビティをタッチします。 これらの 3 つの基本的な種類の区別をすべてのプラットフォームはタッチ イベントです。

- *押された*指が画面をタッチすると、
- *移動*が画面に触れる指を移動すると、
- *リリース*指が画面から離れるときに、

マルチタッチ環境で複数の指は、同時に画面をタッチすることができます。 さまざまなプラットフォームには、アプリケーションが複数の指を区別するために使用できる識別 (ID) 番号が含まれます。

Ios で、`UIView`クラスは、次の 3 つのオーバーライド可能なメソッドを定義します。 `TouchesBegan`、 `TouchesMoved`、と`TouchesEnded`これら 3 つの基本的なイベントに対応します。 この記事[マルチタッチ指追跡](~/ios/app-fundamentals/touch/touch-tracking.md)これらのメソッドを使用する方法について説明します。 ただし、iOS、プログラムがから派生したクラスをオーバーライドする必要ありません`UIView`これらのメソッドを使用します。 IOS`UIGestureRecognizer`もこれらと同じを 3 つの定義から派生したクラスのインスタンスをアタッチできるメソッド、およびする`UIGestureRecognizer`いずれかに`UIView`オブジェクト。

Android では、`View`クラスという名前のオーバーライド可能なメソッドを定義する`OnTouchEvent`タッチのすべてのアクティビティを処理します。 タッチ アクティビティの種類が列挙体メンバーで定義されている`Down`、 `PointerDown`、 `Move`、`Up`と`PointerUp`、記事の説明に従って[マルチタッチ指追跡](~/android/app-fundamentals/touch/touch-tracking.md)します。 Android`View`という名前のイベントも定義して`Touch`いずれかに接続するイベント ハンドラーを利用できる`View`オブジェクト。

ユニバーサル Windows プラットフォーム (UWP) で、`UIElement`クラスという名前のイベントを定義する`PointerPressed`、 `PointerMoved`、および`PointerReleased`します。 これらについては、記事[ポインター入力の処理の MSDN 記事「](/windows/uwp/input-and-devices/handle-pointer-input/)と API のドキュメントを[ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/)クラス。

`Pointer`マウス、タッチ、およびペン入力を統一するユニバーサル Windows プラットフォームでの API が対象としています。 そのため、`PointerMoved`マウス ボタンが押されていない場合でも、マウスが要素の間で移動したときにイベントが呼び出されます。 `PointerRoutedEventArgs`これらのイベントに付属しているオブジェクトがという名前のプロパティを持つ`Pointer`という名前のプロパティを持つ`IsInContact`マウス ボタンが押されたかどうか、または画面と指を示します。

また、UWP はという名前の 2 つのより多くのイベントを定義します。`PointerEntered`と`PointerExited`します。 これらは、移動したときに、マウスまたは指 1 つの要素からを示しています。 たとえば、A と B という 2 つの隣接する要素両方の要素は、ポインター イベントのハンドラーをインストールします。 指が a でキーを押したときに、`PointerPressed`イベントが呼び出されます。 A が呼び出す、指を移動、`PointerMoved`イベント。 A が呼び出す、指が A から B に移動した場合、`PointerExited`イベントと B を呼び出す、`PointerEntered`イベント。 B を呼び出す場合は、指を解放し、`PointerReleased`イベント。

IOS および Android プラットフォームは、UWP と異なる: 最初の呼び出しを取得するビュー`TouchesBegan`または`OnTouchEvent`に触れると、ビューは引き続き、指をさまざまなビューに移動する場合でも、すべてのタッチ アクティビティを取得します。 ポインターをキャプチャした場合、UWP が同様に動作できます: で、`PointerEntered`イベント ハンドラー、要素の呼び出し`CapturePointer`し、その指からタッチのすべてのアクティビティを取得します。

UWP のアプローチに音楽キーボードなどのアプリケーションの種類によっては非常に便利ですが。 各キーはそのキーのタッチ イベントを処理でき、もう 1 つを使用する 1 つのキーから指をスライドが検出、`PointerEntered`と`PointerExited`イベント。

そのため、この記事で説明されているタッチの追跡の効果は UWP のアプローチを実装します。

## <a name="the-touch-tracking-effect-api"></a>タッチの追跡の有効 API

[**追跡効果デモのタッチ**](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)サンプルには、低レベルのタッチの追跡を実装するクラス (および列挙体) が含まれています。 これらの型は、名前空間に属している`TouchTracking`単語で始まると`Touch`します。 **TouchTrackingEffectDemos** .NET Standard ライブラリ プロジェクトが含まれています、`TouchActionType`タッチ イベントの種類の列挙体。

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

すべてのプラットフォームには、タッチ イベントがキャンセルされたことを示すイベントも含まれます。

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

また、`Capture`プロパティ。 タッチ イベントをキャプチャするには、アプリケーションこのプロパティを設定する必要があります`true`より前のバージョン、`Pressed`イベント。 それ以外の場合、タッチ イベントは、ユニバーサル Windows プラットフォームのように動作します。

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

アプリケーションで使用できます、`Id`個々 の本の指を追跡するためのプロパティ。 通知、`IsInContact`プロパティ。 このプロパティは常に`true`の`Pressed`イベントと`false`の`Released`イベント。 常に`true`の`Moved`iOS と Android でのイベント。 `IsInContact`プロパティがあります`false`の`Moved`ユニバーサル Windows プラットフォーム、デスクトップで、プログラムが実行されていると、マウス ポインターがボタンなしでイベントが押されました。

使用することができます、`TouchEffect`とするインスタンスを追加することで、ソリューションの .NET Standard ライブラリ プロジェクトでは、ファイルを含めることによって、独自のアプリケーション内のクラス、 `Effects` Xamarin.Forms の任意の要素のコレクション。 ハンドラーを結び付ける、`TouchAction`タッチ イベントを取得するイベントです。

使用する`TouchEffect`独自のアプリケーションにも必要になりますプラットフォームの実装に含まれる**TouchTrackingEffectDemos**ソリューション。

## <a name="the-touch-tracking-effect-implementations"></a>タッチ追跡効果の実装

IOS、Android、および UWP の実装、`TouchEffect`は最も簡単な実装 (UWP) で始まると、他よりもより構造的に複雑では、iOS のカスタム実装で終わる次のとおりです。

### <a name="the-uwp-implementation"></a>UWP の実装

UWP 実装`TouchEffect`は最も簡単です。 通常どおり、クラスの派生元`PlatformEffect`2 つのアセンブリ属性が含まれています。

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

`OnAttached`オーバーライドは、すべてのポインター イベントへのフィールドとアタッチのハンドラーとしていくつかの情報を保存します。

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

`OnPointerPressed`ハンドラーが呼び出すことによって有効イベントを呼び出す、`onTouchAction`フィールドに、`CommonHandler`メソッド。

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

`OnPointerPressed` 値も確認、 `Capture` .NET Standard ライブラリの呼び出しで効果のクラスのプロパティ`CapturePointer`場合は`true`します。

 その他の UWP イベント ハンドラーがさらに簡単です。

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

Android および iOS の実装は、それらを実装する必要がありますので、必ずしも複雑、`Exited`と`Entered`1 つの要素から指を移動するときにイベント。 どちらの実装は、同様に構成されています。

Android`TouchEffect`クラスのハンドラーをインストールする、`Touch`イベント。

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

クラスは、2 つの静的ディクショナリにも定義します。

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

そのエントリはでディクショナリから削除`OnDetached`します。 すべてのインスタンス`TouchEffect`に効果がアタッチされている特定のビューに関連付けられています。 いずれかにより、静的ディクショナリ`TouchEffect`インスタンス、その他のすべてのビューとそれに対応する順に列挙する`TouchEffect`インスタンス。 これは、間の 1 つのビューのイベントを転送するために必要です。

Android では、個々 の本の指を追跡するためにアプリケーションを可能にするタッチ イベントの ID コードが割り当てられます。 `idToEffectDictionary`でこの ID コードを関連付けます、`TouchEffect`インスタンス。 このディクショナリに項目が追加されたときに、`Touch`本の指のキーを押してのハンドラーが呼び出されます。

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

項目を削除、`idToEffectDictionary`指が画面から解放されるときにします。 `FireEvent`メソッドを呼び出すために必要なすべての情報を蓄積単に、`OnTouchAction`メソッド。

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

タッチに関するその他のすべての種類は、2 つの方法で処理されます。 場合、`Capture`プロパティは`true`、タッチ イベントを非常に単純な変換は、、`TouchEffect`情報。 取得より複雑な場合に`Capture`は`false`のため、タッチ イベントは別に 1 つのビューから移動する必要があります。 これは、役割、`CheckForBoundaryHop`移動イベント中に呼び出されるメソッド。 このメソッドの両方の静的ディクショナリを使用します。 これを列挙、`viewDictionary`ビューを使用して、本の指が接触現在を決定する`idToEffectDictionary`現在を格納する`TouchEffect`インスタンス (および現在のビューがそのため、) 特定の ID に関連付けられています。

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

変更があった場合、 `idToEffectDictionary`、可能性のあるメソッドを呼び出して`FireEvent`の`Exited`と`Entered`別に 1 つのビューからの転送にします。 ただし、指が移動された可能性が添付なしのビューによって占有されている領域に`TouchEffect`、またはその領域にアタッチされている効果では、ビューから。

通知、`try`と`catch`ビューにアクセスするときにブロックします。 ナビゲートし、ホーム ページにナビゲートするページで、`OnDetached`メソッドを呼び出さないし、アイテム、 `viewDictionary` Android では、破棄考えますが、します。

### <a name="the-ios-implementation"></a>IOS の実装

IOS のカスタム実装に点を除いては、Android の実装に似ていますが、iOS`TouchEffect`クラスの派生クラスのインスタンスを作成する必要があります`UIGestureRecognizer`します。 これは、名前付きの iOS プロジェクトでクラス`TouchRecognizer`します。 このクラスを格納する 2 つの静的ディクショナリを保持する`TouchRecognizer`インスタンス。

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

この構造体の`TouchRecognizer`クラスは、Android と同様に`TouchEffect`クラス。

## <a name="putting-the-touch-effect-to-work"></a>タッチの効果を作業に配置します。

[ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)プログラムには、一般的なタスクのタッチの追跡の効果をテストする 5 つのページが含まれています。

**BoxView ドラッグ** ページでは、追加できます。`BoxView`要素を、`AbsoluteLayout`し、それらを画面にドラッグします。 [XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml)2 つのインスタンスを作成`Button`を追加するためのビュー`BoxView`要素を`AbsoluteLayout`オフにして、`AbsoluteLayout`します。

メソッドで、[分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs)新しいを追加する`BoxView`を`AbsoluteLayout`も追加、`TouchEffect`オブジェクトを`BoxView`効果にイベント ハンドラーをアタッチします。

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

`TouchAction`イベント ハンドラーは、すべてのすべてのタッチ イベントを処理、`BoxView`要素がいくつか注意する必要があります: 1 つで 2 本の指を許可することはできません`BoxView`プログラムがのみ、ドラッグを実装し、2 本の指がため互いに干渉します。 このため、ページは、現在追跡されているそれぞれの指の埋め込みのクラスを定義します。

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

`dragDictionary`エントリすべて`BoxView`現在ドラッグ中します。

`Pressed`タッチ操作では、このディクショナリに項目を追加し、`Released`アクションで削除します。 `Pressed`かどうかは、項目をディクショナリにロジックを確認する必要があります`BoxView`します。 そうである場合、`BoxView`既にドラッグされていると、新しいイベントが同じに指を 2 つ目は、`BoxView`します。 `Moved`と`Released`アクション、イベント ハンドラーの必要がありますチェックにディクショナリのエントリがあるかどうか`BoxView`ことと、タッチ`Id`プロパティをドラッグ`BoxView`辞書エントリと一致します。

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

`Pressed`ロジック セット、`Capture`のプロパティ、`TouchEffect`オブジェクトを`true`します。 同じイベント ハンドラーに、その指の後続のすべてのイベントを提供する効果があります。

`Moved`ロジックの移動、`BoxView`変更することによって、`LayoutBounds`添付プロパティ。 `Location`イベント引数のプロパティが基準には常に、`BoxView`ドラッグされている場合に、`BoxView`が一定の率でドラッグされている、`Location`連続するイベントのプロパティはほぼ同じになります。 指がキーを押した場合など、 `BoxView` 、中央で、`Pressed`アクション ストア、`PressPoint`のプロパティ (50, 50) は、後続のイベントの同じです。 場合、`BoxView`がそれに続く一定の率で対角線方向にドラッグされる`Location`中にプロパティ、`Moved`アクションの値があります (55, 55)、その場合、`Moved`ロジック、の水平および垂直方向の位置に5を追加します`BoxView`. これにより、移動、`BoxView`中心が再びように指の直下にあります。

複数を移動する`BoxView`要素が同時に別の指を使用します。

[![](touch-tracking-images/boxviewdragging-small.png "BoxView ドラッグ ページのスクリーン ショットをトリプル")](touch-tracking-images/boxviewdragging-large.png#lightbox "BoxView をドラッグするページの 3 倍になるスクリーン ショット")

### <a name="subclassing-the-view"></a>ビューのサブクラス化

多くの場合、独自のタッチ イベントを処理するために Xamarin.Forms 要素に簡単です。 **ドラッグ BoxView ドラッグ**ページの機能と同じ、 **BoxView ドラッグ** ページが要素のインスタンスでは、ユーザーのドラッグが、 [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs)派生したクラス`BoxView`:

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

コンス トラクターを作成し、アタッチ、 `TouchEffect`、設定と、`Capture`そのオブジェクトが最初にインスタンス化されるプロパティ。 ディクショナリは必要ありません、クラス自体に格納されるため`isBeingDragged`、 `pressPoint`、および`touchId`それぞれの指に関連付けられた値。 `Moved`処理の変更、`TranslationX`と`TranslationY`プロパティは、ロジックの動作に場合でもの親、`DraggableBoxView`でない、`AbsoluteLayout`します。

### <a name="integrating-with-skiasharp"></a>SkiaSharp との統合

次の 2 つのデモは、グラフィックを必要とし、この目的で SkiaSharp を使用します。 について学習する必要あります[Xamarin.Forms で SkiaSharp を使用して](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)前に、これらの例を学習します。 (「SkiaSharp 描画の基礎」および「SkiaSharp の線とパス」) は、最初の 2 つの記事では、ここで必要となるすべてのものについて説明します。

**楕円の描画** ページでは、画面に指をスワイプすると、楕円を描画できます。 左から右、下とは反対側の隅に、その他の隅からは、指を移動する方法によっては、楕円を描画できます。 ランダムな色と不透明度、楕円が描画されます。

[![](touch-tracking-images/ellipsedrawing-small.png "楕円の描画のページのスクリーン ショットをトリプル")](touch-tracking-images/ellipsedrawing-large.png#lightbox "楕円の描画のページの 3 倍になるスクリーン ショット")

省略記号のいずれかをタッチ、別の場所にドラッグできます。 これには、「ヒット テスト、」特定の時点で、グラフィカル オブジェクトを検索すると呼ばれる手法が必要です。 SkiaSharp の省略記号でない Xamarin.Forms 要素では、独自に実行することはできませんので`TouchEffect`処理します。 `TouchEffect`全体に適用する必要があります`SKCanvasView`オブジェクト。

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml)ファイルをインスタンス化、 `SKCanvasView` 1 つのセルで`Grid`します。 `TouchEffect`にオブジェクトがアタッチされる`Grid`:

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

Android およびユニバーサル Windows プラットフォームで、`TouchEffect`に直接関連付けることができます、`SKCanvasView`が動作しない iOS でします。 なお、`Capture`プロパティに設定されて`true`します。

SkiaSharp をレンダリングするそれぞれの楕円がの型のオブジェクトによって表される`EllipseDrawingFigure`:

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

`StartPoint`と`EndPoint`プロパティを使用して、プログラムがタッチ入力を処理するときに、`Rectangle`プロパティは、楕円の描画に使用します。 `LastFingerLocation`プロパティが、省略記号ボタンをドラッグされているときに、`IsInEllipse`ヒット テスト メソッドを支援します。 メソッドを返します`true`ポイントが、ellipse の内部にある場合。

[分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs)3 つのコレクションを保持します。

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure`ディクショナリにはサブセットが含まれています、`completedFigures`コレクション。 SkiaSharp、`PaintSurface`イベント ハンドラーは単にこれらのオブジェクトを表示、`completedFigures`と`inProgressFigures`コレクション。

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

タッチ処理の最大の見所は、`Pressed`を処理します。 これは、ような場合、ヒット テストを実行するが、コードがユーザーの指の下で楕円を検出した場合、楕円のみドラッグすることは現在別の指でドラッグされている場合。 ユーザーの指の下の楕円がない場合は、コードは新しい楕円を描画プロセスを開始します。

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

SkiaSharp の他の例は、**指ペイント**ページ。 2 つのストロークの色と太さを選択できます`Picker`ビューし、1 つ以上の指を描画します。

[![](touch-tracking-images/fingerpaint-small.png "本の指ペイント ページのスクリーン ショットをトリプル")](touch-tracking-images/fingerpaint-large.png#lightbox "指ペイント ページの 3 倍になるスクリーン ショット")

この例では、画面上に描画される各行を表す別のクラスも必要です。

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

`SKPath`オブジェクトをそれぞれの行を表示するために使用します。 [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs)ファイルは、これらのオブジェクト、これらのポリラインを描画されている現在の 1 つと完了したポリラインの 2 つのコレクションを保持します。

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed`処理が新たに作成`FingerPaintPolyline`、呼び出し`MoveTo`最初のポイントを格納するパス オブジェクトにし、そのオブジェクトを追加、`inProgressPolylines`ディクショナリ。 `Moved`呼び出しの処理`LineTo`本の指の新しい位置のパス オブジェクトで、`Released`処理から完成したポリラインを転送する`inProgressPolylines`に`completedPolylines`します。 繰り返しますが、実際の SkiaSharp 描画コードは比較的単純なは。

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

### <a name="tracking-view-to-view-touch"></a>追跡ビューからビューへのタッチ

前の例をすべて設定、`Capture`のプロパティ、`TouchEffect`に`true`、いずれかの場合、`TouchEffect`が作成された場合や、`Pressed`イベントが発生しました。 これにより、同じ要素が最初に、ビュー、押された本の指に関連付けられているすべてのイベントを受信します。 最後のサンプルでは、*いない*設定`Capture`に`true`します。 これにより、1 つの要素間、画面と指を移動するときに、動作が異なります。 要素から指を移動するとイベントの受信、`Type`プロパティに設定`TouchActionType.Exited`2 番目の要素を持つイベントを受信して、`Type`の設定`TouchActionType.Entered`します。

この種のタッチ処理は、音楽キーボードに便利です。 キーが押されたも別に、指が 1 つのキーからスライドとタイミングを検出できる必要があります。

**サイレント キーボード**ページを小規模定義[ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs)と[ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs)クラスから派生した[ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs)から派生した`BoxView`します。

`Key`クラスの実際の音楽アプリケーションで使用する準備ができます。 という名前のパブリック プロパティを定義します`IsPressed`と`KeyNumber`、MIDI 標準を確立する重要なコードに設定する対象としました。 `Key`クラスは、という名前のイベントも定義されています。`StatusChanged`これが呼び出されたときに、`IsPressed`プロパティの変更。

複数の指は、各キーで許可されます。 このため、`Key`クラスを保持する`List`現在そのキーをタッチするすべての指のタッチ ID 番号の。

```csharp
List<long> ids = new List<long>();
```

`TouchAction`イベント ハンドラーを ID の追加、`ids`両方の一覧を`Pressed`イベントの種類と`Entered`型であり、場合にのみ、`IsInContact`プロパティは`true`の`Entered`イベント。 ID はから削除、`List`の`Released`または`Exited`イベント。

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

`AddToList`と`RemoveFromList`メソッドはどちらも確認の場合、`List`間空および空以外の場合、変更された場合は、呼び出すと、`StatusChanged`イベント。

さまざまな`WhiteKey`と`BlackKey`ページの要素の配置[XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml)、これは、電話が横モードで保持されている場合に最適に表示します。

[![](touch-tracking-images/silentkeyboard-small.png "サイレント [キーボード] ページのスクリーン ショットをトリプル")](touch-tracking-images/silentkeyboard-large.png#lightbox "サイレント [キーボード] ページの 3 倍になるスクリーン ショット")

場合は、キーに指をスイープするわかります色のわずかな変更によって別に 1 つのキーから、タッチ イベントが転送されること。

## <a name="summary"></a>まとめ

この記事では、特殊効果でイベントを呼び出す方法と記述して、低レベルのマルチタッチの処理を実装する効果を使用する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [IOS では、マルチタッチ本の指の追跡](~/ios/app-fundamentals/touch/touch-tracking.md)
- [マルチタッチ指が Android での追跡](~/android/app-fundamentals/touch/touch-tracking.md)
- [タッチ追跡効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
