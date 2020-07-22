---
title: エフェクトからのイベントの呼び出し
description: エフェクトではイベントを定義して呼び出すことができ、基礎ネイティブ ビューの変化を信号で送ります。 この記事では、低レベルのマルチタッチ フィンガー トラッキングを実装する方法とタッチ操作を信号で送るイベントを生成する方法を紹介します。
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 892bffa4027a1a61d6c22cc26d1556fb007432d8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136982"
---
# <a name="invoking-events-from-effects"></a>エフェクトからのイベントの呼び出し

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)

_エフェクトではイベントを定義して呼び出すことができ、基礎ネイティブ ビューの変化を信号で送ります。この記事では、低レベルのマルチタッチ フィンガー トラッキングを実装する方法とタッチ操作を信号で送るイベントを生成する方法を紹介します。_

この記事で説明するエフェクトでは、低レベルのタッチ イベントにアクセスできます。 このような低レベルのイベントは既存の `GestureRecognizer` クラス経由では利用できませんが、一部のアプリケーションにとって不可欠です。 たとえば、フィンガーペイント アプリケーションの場合、画面上を動く一本一本の指を追跡する必要があります。 音楽用キーボードでは、個々の鍵盤を押す/離す動きやグリッサンドで鍵盤上をすべる指を検出する必要があります。

エフェクトはあらゆる Xamarin.Forms 要素にアタッチできるため、マルチタッチ フィンガー トラッキングに最適です。

## <a name="platform-touch-events"></a>プラットフォーム タッチ イベント

iOS、Android、Universal Windows Platform にはすべて、タッチ操作の検出をアプリケーションに許可する低レベル API が含まれています。 このようなプラットフォームではすべて、基本的な 3 種類のタッチ イベントが区別されます。

- *押す*。指が画面に触れたとき
- *移動する*。画面に触れている指が移動するとき
- *離す*。指を画面から離したとき

マルチタッチ環境では、複数の指で同時に画面に触れることができます。 複数の指を区別するためにアプリケーションで利用される ID 番号が各種プラットフォームに含まれています。

iOS の場合、`UIView` クラスによって、これら 3 つの基本イベントに相当する 3 つのオーバーライド可能メソッド、`TouchesBegan`、`TouchesMoved`、`TouchesEnded` が定義されます。 「[Multi-Touch Finger Tracking](~/ios/app-fundamentals/touch/touch-tracking.md)」 (マルチタッチ フィンガー トラッキング) という記事に、これらのメソッドの使用方法が記載されています。 しかしながら、iOS プログラムの場合、これらのメソッドを使用するには `UIView` から派生するクラスをオーバーライドする必要があります。 iOS `UIGestureRecognizer` では、これらの同じ 3 つのメソッドも定義されます。`UIGestureRecognizer` から派生するクラスのインスタンスをあらゆる `UIView` オブジェクトにアタッチできます。

Android の場合、`View` クラスによって、すべてのタッチ操作を処理する `OnTouchEvent` という名称のオーバーライド可能メソッドが定義されます。 タッチ操作の種類は、「[Multi-Touch Finger Tracking](~/android/app-fundamentals/touch/touch-tracking.md)」 (マルチタッチ フィンガー トラッキング) という記事で説明されている列挙メンバー `Down`、`PointerDown`、`Move`、`Up`、`PointerUp` によって定義されます。 Android `View` により `Touch` という名前のイベントも定義されます。このイベントによって、イベント ハンドラーをあらゆる `View` オブジェクトにアタッチできます。

ユニバーサル Windows プラットフォーム (UWP) では、`UIElement` クラスによって `PointerPressed`、`PointerMoved`、`PointerReleased` という名前のイベントが定義されます。 これらのイベントに関する説明は、[MSDN のポインター入力の処理](/windows/uwp/input-and-devices/handle-pointer-input/)に関する記事と [`UIElement`](/uwp/api/windows.ui.xaml.uielement/) クラスに関する API 文書にあります。

ユニバーサル Windows プラットフォームの `Pointer` API はマウス、タッチ、ペンによる入力の統一を意図するものです。 そのため、マウス ボタンが押されていないときでも、マウスが要素を横切ると、`PointerMoved` イベントが呼び出されます。 このようなイベントにともなう `PointerRoutedEventArgs` オブジェクトには `Pointer` という名前のプロパティがあります。このプロパティには、マウス ボタンが押されたかどうかや指が画面と接触しているかどうかを示す `IsInContact` プロパティがあります。

さらに、UWP によって `PointerEntered` と `PointerExited` という 2 つのイベントがさらに定義されます。 この 2 つのイベントは、マウスまたは指が要素間を移動したタイミングを示します。 たとえば、A と B という名前が付けられた 2 つの隣接する要素があるとします。いずれの要素にも、ポインター イベントのハンドラーがインストールされています。 指が A を押すと、`PointerPressed` イベントが呼び出されます。 指が動くと、A によって `PointerMoved` イベントが呼び出されます。 指が A から B に移動すると、A によって `PointerExited` イベントが呼び出され、B によって `PointerEntered` イベントが呼び出されます。 指を離すと、B によって `PointerReleased` イベントが呼び出されます。

iOS プラットフォームと Android プラットフォームは UWP とは異なります。指がビューに触れたとき、`TouchesBegan` または `OnTouchEvent` の呼び出しを最初に取得するビューは、指が別のビューに移動した場合でも、すべてのタッチ操作を引き続き取得します。 アプリケーションでポインターがキャプチャされた場合、UWP は同様に動作できます。`PointerEntered` イベント ハンドラーで、要素によって `CapturePointer` が呼び出され、その指からのすべてのタッチ操作が取得されます。

UWP の手法は、音楽用キーボードなど、一部のアプリケーションで非常に便利になります。 各キーはそのキーのタッチ イベントを処理し、`PointerEntered` イベントと `PointerExited` イベントによってキー間の指のスライド移動を検出できます。

そのような理由から、この記事で説明するタッチトラッキング エフェクトでは UWP の手法を実行します。

## <a name="the-touch-tracking-effect-api"></a>タッチトラッキング エフェクト API

[**タッチ トラッキング エフェクト デモ**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) サンプルには、低レベルのタッチトラッキングを実行するクラス (と列挙) が含まれています。 これらの型は名前空間 `TouchTracking` に属し、`Touch` という単語から始まります。 **TouchTrackingEffectDemos** .NET Standard ライブラリ プロジェクトには、タッチ イベント型の `TouchActionType` 列挙が含まれています。

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

プラットフォームにはすべて、タッチ イベントがキャンセルされたことを示すイベントが含まれています。

.NET Standard ライブラリの `TouchEffect` クラスは `RoutingEffect` から派生し、このクラスによって `TouchAction` という名前のイベントと `TouchAction` イベントを呼び出す `OnTouchAction` という名前のメソッドが定義されます。

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

`Capture` プロパティにも注目してください。 タッチ イベントをキャプチャするには、`Pressed` イベントの前にアプリケーションでこのプロパティを `true` に設定する必要があります。 そうしない場合、ユニバーサル Windows プラットフォームの場合のようにタッチ イベントが動作します。

.NET Standard ライブラリの `TouchActionEventArgs` クラスには、各イベントにともなう情報がすべて含まれます。

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

アプリケーションでは、`Id` プロパティを利用して一本一本の指を追跡できます。 `IsInContact` プロパティに注目してください。 このプロパティは常に `Pressed` イベントに対して `true`、`Released` イベントに対して `false` となります。 iOS と Android でも、`Moved` イベントに対して常に `true` となります。 ユニバーサル Windows プラットフォームでは、プログラムがデスクトップで実行されているとき、ボタンを押さずにマウス ポインターが動いたとき、`IsInContact` プロパティは `Moved` イベントに対して `false` にであることがあります。

ソリューションの .NET Standard ライブラリ プロジェクトにファイルを含め、任意の Xamarin.Forms 要素の `Effects` コレクションにインスタンスを追加することで、自分のアプリケーションで `TouchEffect` クラスを使用できます。 タッチ イベントを取得するには、`TouchAction` イベントにハンドラーをアタッチします。

自分のアプリケーションで `TouchEffect` を使用するには、**TouchTrackingEffectDemos** ソリューションに含まれるプラットフォーム実装も必要になります。

## <a name="the-touch-tracking-effect-implementations"></a>タッチトラッキング エフェクト実装

以下は `TouchEffect` の iOS、Android、UWP 実装の説明となります。最も単純な実装 (UWP) で始まり、構造的に他より複雑な iOS 実装で終わっています。

### <a name="the-uwp-implementation"></a>UWP 実装

`TouchEffect` の UWP 実装が最も単純です。 通常どおり、このクラスは `PlatformEffect` から派生し、2 つのアセンブリ属性を含みます。

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

`OnAttached` オーバーライドによって一部の情報がフィールドとして保存され、ハンドラーがすべてのポインター イベントにアタッチされます。

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

`OnPointerPressed` ハンドラーでは、`CommonHandler` メソッドの `onTouchAction` フィールドを呼び出すことでエフェクト イベントが呼び出されます。

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

`OnPointerPressed` ではまた、.NET Standard ライブラリのエフェクト クラスにある `Capture` プロパティの値が確認され、`true` の場合、`CapturePointer` が呼び出されます。

 その他の UWP イベント ハンドラーはさらに単純です。

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

### <a name="the-android-implementation"></a>Android 実装

Android 実装と iOS 実装は指が要素間を移動するときに `Exited` イベントと `Entered` イベントを実装する必要があるため、必然的に複雑性が上がります。 いずれの実装も構造は似通っています。

Android `TouchEffect` クラスでは、`Touch` イベントに対してハンドラーがインストールされます。

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

このクラスにより、2 つの静的ディクショナリも定義されます。

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

`OnAttached` オーバーライドが呼び出されるたびに `viewDictionary` に新しいエントリが取得されます。

```csharp
viewDictionary.Add(view, this);
```

エントリは `OnDetached` のディクショナリから削除されます。 エフェクトがアタッチされている特定のビューに `TouchEffect` のすべてのインスタンスが関連付けられます。 静的ディクショナリにより、あらゆる `TouchEffect` インスタンスで他のすべてのビューとそれに対応する `TouchEffect` インスタンスを列挙できます。 これはビュー間でイベントを転送するために必要です。

一本一本の指を追跡することをアプリケーションで可能にする ID コードが Android によってタッチ イベントに割り当てられます。 `idToEffectDictionary` によってこの ID コードと `TouchEffect` インスタンスが関連付けられます。 指が画面に触れたことで `Touch` ハンドラーが呼び出されると、このディクショナリに項目が追加されます。

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

指が画面から離れると、この項目は `idToEffectDictionary` から削除されます。 `FireEvent` メソッドでは単純に、`OnTouchAction` メソッドを呼び出すために必要なすべての情報が蓄積されます。

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

その他の種類のタッチはすべて 2 つの異なる方法で処理されます。`Capture` プロパティが `true` の場合、タッチ イベントは `TouchEffect` 情報に極めて単純に変換されたものになります。 `Capture` が `false` のときは複雑性が上がります。場合によっては、ビュー間でタッチ イベントが動く必要があるためです。 これは `CheckForBoundaryHop` メソッドの担当となります。このメソッドは移動中に呼び出されます。 このメソッドでは、両方の静的ディクショナリが利用されます。 `viewDictionary` を列挙し、指が現在触れているビューを判断し、`idToEffectDictionary` を使用して特定の ID に関連付けられている現行の `TouchEffect` インスタンス (したがって、現在のビュー) を保存します。

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

`idToEffectDictionary` に変更がある場合、このメソッドは `Exited` と `Entered` に対して `FireEvent` を呼び出し、ビューを変えることがあります。 ただし、`TouchEffect` がアタッチされていないビューに占有されている領域に指が移動することがあります。あるいは、このエフェクトがアタッチされているビューにその領域から移動することがあります。

ビューがアクセスされるときの `try` および `catch` ブロックに注目してください。 このブロックに進み、それからホーム ページに戻るページでは、`OnDetached` メソッドは呼び出されません。項目は `viewDictionary` に残りますが、Android では処分済みと見なされます。

### <a name="the-ios-implementation"></a>iOS 実装

iOS 実装は Android 実装に似ていますが、iOS `TouchEffect` クラスによって `UIGestureRecognizer` の派生物をインスタンス化する必要があります。 これは `TouchRecognizer` という名前の iOS プロジェクトのクラスです。 このクラスは、`TouchRecognizer` インスタンスを格納する 2 つの静的ディクショナリを維持します。

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

この `TouchRecognizer` クラスの構造の大部分は Android `TouchEffect` クラスと似通っています。

> [!IMPORTANT]
> `UIKit` のビューの多くは、既定ではタッチが有効になっていません。 タッチを有効にするには、iOS プロジェクトの `TouchEffect` クラス内で `OnAttached` オーバーライドに `view.UserInteractionEnabled = true;` を追加します。 これは、エフェクトがアタッチされている要素に対応する`UIView` が取得されてから行う必要があります。

## <a name="putting-the-touch-effect-to-work"></a>タッチ エフェクトを動かす

[**TouchTrackingEffectDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) プログラムには、一般的な作業のタッチトラッキング エフェクトをテストするページが 5 つ含まれています。

**[BoxView Dragging]\(BoxView ドラッグ操作\)** ページでは、`BoxView` 要素を `AbsoluteLayout` に追加し、画面上をドラッグできます。 [XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BoxViewDraggingPage.xaml)によって、`BoxView` 要素を `AbsoluteLayout` に追加し、`AbsoluteLayout` を消去するための `Button` ビューが 2 つインスタンス化されます。

また、新しい `BoxView` を `AbsoluteLayout` に追加する[分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BoxViewDraggingPage.xaml.cs)のメソッドによって、`TouchEffect` オブジェクトが `BoxView` に追加され、イベント ハンドラーがエフェクトにアタッチされます。

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

`TouchAction` イベント ハンドラーによってすべての `BoxView` 要素に対してすべてのタッチ イベントが処理されますが、いくつかの注意が必要です。1 つの `BoxView` に対して 2 本の指は許可されません。このプログラムでは、ドラッグ操作のみが実装されるためです。2 本の指では互いに干渉することがあります。 このため、このページでは、現在追跡されている指ごとに埋め込みクラスが定義されます。

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

`dragDictionary` には、現在ドラッグされている `BoxView` ごとのエントリが含まれます。

`Pressed` タッチ操作によって項目がこのディクショナリに追加され、`Released` 操作によってそれが削除されます。 その `BoxView` のディクショナリに既に項目が存在しないかどうかを `Pressed` ロジックで確認する必要があります。 存在する場合、`BoxView` は既にドラッグされていて、新しいイベントはその同じ `BoxView` で 2 本目の指になります。 `Moved` アクションと `Released` アクションについては、その `BoxView` に対するエントリがディクショナリにあるかどうかとそのドラッグされた `BoxView` のタッチ `Id` プロパティがディクショナリ エントリのそれと一致するかどうかをイベント ハンドラーで確認する必要があります。

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

`Pressed` ロジックによって `TouchEffect` オブジェクトの `Capture` プロパティが `true` に設定されます。 これにはその指のすべての後続イベントを同じイベント ハンドラーに届けるという効果があります。

`BoxView` 添付プロパティを変更することで、`Moved` ロジックによって `LayoutBounds` が移動します。 イベント引数の `Location` プロパティは常に、ドラッグされている `BoxView` と相対的になります。`BoxView` が一定の速度でドラッグされている場合、連続するイベントの `Location` プロパティはだいたい同じになります。 たとえば、指が `BoxView` の中心を押すと、`Pressed` アクションによって `PressPoint` プロパティ (50, 50) が格納されます。これは後続イベントに対して同じになります。 `BoxView` が一定の速度で斜めにドラッグされた場合、`Moved` アクション中の後続の `Location` プロパティは値 (55, 55) になることがあります。その場合、`Moved` ロジックによって、`BoxView` の水平および垂直位置に 5 が加算されます。 これで `BoxView` が移動し、その中央が再び指の真下になります。

異なる指を使用し、複数の `BoxView` 要素を同時に動かすことができます。

[![](touch-tracking-images/boxviewdragging-small.png "Triple screenshot of the BoxView Dragging page")](touch-tracking-images/boxviewdragging-large.png#lightbox "Triple screenshot of the BoxView Dragging page")

### <a name="subclassing-the-view"></a>ビューのサブクラス化

多くの場合、Xamarin.Forms 要素ではそれ自体のタッチ イベントを簡単に処理できます。 **[Draggable BoxView Dragging]\(ドラッグ可能 BoxView ドラッグ操作\)** ページの機能は **[BoxView Dragging]\(BoxView ドラッグ操作\)** ページと同じですが、ユーザーがドラッグする要素は `BoxView` から派生した [`DraggableBoxView`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/DraggableBoxView.cs) クラスのインスタンスとなります。

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

このコンストラクターによって `TouchEffect` が作成され、アタッチされ、そのオブジェクトが最初にインスタンス化されたとき、`Capture` プロパティが設定されます。 クラス自体が各指に関連付けられている `isBeingDragged`、`pressPoint`、`touchId` 値を保管するため、ディクショナリは必要ありません。 `DraggableBoxView` の親が `AbsoluteLayout` でない場合でもロジックが動作するように、`Moved` 処理により `TranslationX` プロパティと `TranslationY` プロパティが変更されます。

### <a name="integrating-with-skiasharp"></a>SkiaSharp との統合

次の 2 つのデモンストレーションにはグラフィックスが必要です。そのため、SkiaSharp が使用されます。 以下の例を見る前に [Xamarin.Forms で SkiaSharp を使用する方法](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)に関するページを読むことをお勧めします。 ここで必要なものはすべて、最初の 2 つの記事 (「SkiaSharp Drawing Basics」 (SkiaSharp 描画の基礎) と「SkiaSharp Lines and Paths」 (SkiaSharp の線とパス)) に記載されています。

**[Ellipse Drawing]\(楕円の描画\)** ページでは、画面を指でなぞることで楕円を描くことができます。 指の動かし方に基づき、左上から右下に、あるいは任意の他の隅からその反対側の隅に楕円を描くことができます。 楕円は無作為で選択された色と不透明度で描画されます。

[![](touch-tracking-images/ellipsedrawing-small.png "Triple screenshot of the Ellipse Drawing page")](touch-tracking-images/ellipsedrawing-large.png#lightbox "Triple screenshot of the Ellipse Drawing page")

楕円の 1 つに触れたら、それを別の場所にドラッグできます。 これには "ヒットテスト" と呼ばれている手法が必要です。ヒットテストでは、特定の点にあるグラフィカル オブジェクトが検索されます。 SkiaSharp の楕円は Xamarin.Forms 要素ではありません。そのため、独自の `TouchEffect` 処理を実行できません。 `TouchEffect` は `SKCanvasView` オブジェクト全体に適用する必要があります。

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/EllipseDrawingPage.xaml) ファイルによって、シングルセルの `Grid` で `SKCanvasView` がインスタンス化されます。 `TouchEffect` オブジェクトはその `Grid` にアタッチされます。

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

Android とユニバーサル Windows プラットフォームの場合、`TouchEffect` を `SKCanvasView` に直接アタッチできますが、iOS の場合、それは機能しません。 `Capture` プロパティが `true` に設定されていることに注目してください。

SkiaSharp によってレンダリングされる各楕円が型 `EllipseDrawingFigure` のオブジェクトによって表されます。

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

プログラムによってタッチ入力が処理されるとき、`StartPoint` プロパティと `EndPoint` プロパティが使用されます。楕円の描画には `Rectangle` プロパティが使用されます。 楕円がドラッグされるとき、`LastFingerLocation` プロパティが使用されます。`IsInEllipse` メソッドはヒットテストを支援します。 このメソッドは、点が楕円内にあるとき、`true` を返します。

[分離コード ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/EllipseDrawingPage.xaml.cs)には 3 つのコレクションが保持されます。

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure` ディクショナリには、`completedFigures` コレクションのサブセットが含まれます。 SkiaSharp `PaintSurface` イベント ハンドラーによって単純に、`completedFigures` コレクションと `inProgressFigures` コレクションでオブジェクトがレンダリングされます。

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

タッチ処理の最も巧妙な所は `Pressed` 処理です。 ここはヒットテストが実行される箇所ですが、ユーザーの指の下に楕円があることがコードによって検出された場合、別の指で現在ドラッグされていない場合にのみ、その楕円をドラッグできます。 ユーザーの指の下に楕円がない場合、新しい楕円の描画プロセスがコードによって開始されます。

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

もう 1 つの SkiaSharp 例は **[Finger Paint]** ページです。 2 つの `Picker` ビューからストロークの色と幅を選択し、1 本または複数の指で描画できます。

[![](touch-tracking-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](touch-tracking-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

この例では、画面にペイントされる各線を表す個別のクラスも必要になります。

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

各線をレンダリングするために `SKPath` オブジェクトが使用されます。 [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/FingerPaintPage.xaml.cs) ファイルには、このようなオブジェクトのコレクションが 2 つ保持されます。1 つは現在描画されているポリライン用のコレクションで、もう 1 つは完成したポリライン用のコレクションです。

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` 処理によって新しい `FingerPaintPolyline` が作成され、最初の点を保存するためにパス オブジェクトで `MoveTo` が呼び出され、そのオブジェクトが `inProgressPolylines` ディクショナリに追加されます。 `Moved` 処理によって、指の新しい位置に基づいてパス オブジェクトで `LineTo` が呼び出され、`Released` 処理によって、完成したポリラインが `inProgressPolylines` から `completedPolylines` に移ります。 繰り返しになりますが、実際の SkiaSharp 描画コードは比較的単純です。

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

### <a name="tracking-view-to-view-touch"></a>ビュー間タッチの追跡

これまでの例ではすべて、`TouchEffect` が作成されたか、`Pressed` イベントが発生したとき、`TouchEffect` の `Capture` プロパティが `true` に設定されました。 これにより、最初にビューを押した指に関連付けられているすべてのイベントが同じ要素に受け取られます。 最後のサンプルでは、`Capture` が `true` に設定*されていません*。 それにより、画面に触れている指が要素間を動くとき、さまざまな動作が引き起こされます。 `Type` プロパティが `TouchActionType.Exited` に設定されたイベントを指の移動元の要素が受け取り、`Type` が `TouchActionType.Entered` に設定されたイベントを 2 つ目の要素が受け取ります。

この種類のタッチ処理は音楽用キーボードで非常に便利です。 鍵盤は押されたタイミングだけでなく、指が鍵盤間をスライド移動したタイミングも検出できなければなりません。

**[Silent Keyboard]\(サイレント キーボード\)** ページでは、`BoxView` から派生する [`Key`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/Key.cs) から派生する小さな [`WhiteKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/WhiteKey.cs) クラスと [`BlackKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BlackKey.cs) クラスが定義されます。

`Key` クラスは、実際の音楽プログラムですぐに使用できます。 このクラスによって `IsPressed` および `KeyNumber`という名前のパブリック プロパティが定義されますが、後者は MIDI 標準で決められている鍵盤コードに設定されます。 `Key` クラスによって `StatusChanged` という名前のイベントも定義されます。このイベントは、`IsPressed` プロパティが変更されたときに呼び出されます。

各キーでは複数の指が許可されます。 そのため、`Key` クラスでは、そのキーに現在触れているすべての指のタッチ ID 番号の `List` が保持されます。

```csharp
List<long> ids = new List<long>();
```

`TouchAction` イベント ハンドラーによって、イベントの種類である `Pressed` と `Entered` の両方に対して `ids` リストに ID が追加されますが、`Entered` の場合、`IsInContact` プロパティが `true` のときに限られます。 `Released` イベントと `Exited` イベントの場合、ID が `List` から削除されます。

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

`AddToList` メソッドと `RemoveFromList` メソッドの両方で、`List` が空から空ではない状態に (あるいはその逆に) 変化したかどうかが確認され、変化した場合、`StatusChanged` イベントが呼び出されます。

さまざまな `WhiteKey` および `BlackKey` 要素がページの [XAML ファイル](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/SilentKeyboardPage.xaml) に配置されます。電話を横に構えると見栄えが良くなります。

[![](touch-tracking-images/silentkeyboard-small.png "Triple screenshot of the Silent Keyboard page")](touch-tracking-images/silentkeyboard-large.png#lightbox "Triple screenshot of the Silent Keyboard page")

鍵盤で指をすべらすと、タッチ イベントが鍵盤間を移動することが微妙な色の変化によりわかります。

## <a name="summary"></a>まとめ

この記事では、エフェクトでイベントを呼び出す方法と、低レベルのマルチタッチ処理を実装するエフェクトを記述し、使用する方法を紹介しました。

## <a name="related-links"></a>関連リンク

- [iOS のマルチタッチ フィンガー トラッキング](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Android のマルチタッチ フィンガー トラッキング](~/android/app-fundamentals/touch/touch-tracking.md)
- [タッチ トラッキング エフェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
