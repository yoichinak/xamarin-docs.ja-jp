---
title: スワイプ ジェスチャ認識エンジンを追加します。
description: この記事では、ビューで発生してスワイプ ジェスチャを認識する方法について説明します。
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: 95e95d8849824cd2dc31c2019627cc5adbbefeec
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131526"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>スワイプ ジェスチャ認識エンジンを追加します。

_スワイプ ジェスチャでは、指を水平または垂直方向で画面に移動しは多くの場合、コンテンツ ナビゲーションを開始するために使用するときに発生します。この記事のコード例がから取得した、[スワイプ ジェスチャ](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/)サンプル。_

させる、 [ `View` ](xref:Xamarin.Forms.View)をスワイプ ジェスチャを認識して、作成、 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)インスタンスは、設定、 [ `Direction` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction)プロパティを[ `SwipeDirection`](xref:Xamarin.Forms.SwipeDirection)列挙値 (`Left`、 `Right`、 `Up`、または`Down`)、必要に応じて設定されて、 [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold)プロパティ、ハンドル、 [ `Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)イベントに新しいジェスチャ レコグナイザーを追加し、 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers)ビューのコレクション。 次のコード例は、`SwipeGestureRecognizer`にアタッチされている、 [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

ここでは、相当C#コード。

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

[ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)クラスも含まれます、 [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold)を必要に応じて設定できるプロパティ、`uint`達成する必要があります最小スワイプの距離を表す値をデバイスに依存しない単位で認識されるようにスワイプします。 このプロパティの既定値は、100、100 未満のデバイスに依存しない単位は無視されますが、任意のスワイプのといったことを意味します。

## <a name="recognizing-the-swipe-direction"></a>スワイプ方向を認識します。

上記の例では、 [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction)プロパティから値を 1 つに設定されて、 [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection)列挙体。 ただし、このプロパティから複数の値を設定することがも、`SwipeDirection`列挙型、ように、 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)イベントが複数の一方向のスワイプへの応答で発生します。 ただしの制約は、1 つ[ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)同じ軸上のみ発生するカードを認識できます。 設定してそのため、水平軸上で発生するカードを認識できる、`Direction`プロパティを`Left`と`Right`:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

同様を設定して、垂直軸上で発生するカードを認識できる、 [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction)プロパティを`Up`と`Down`:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

または、 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)を各方向にスワイプといったを認識する各スワイプの方向を作成することができます。

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Right" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Up" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Down" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

ここでは、相当C#コード。

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;
var rightSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Right };
rightSwipeGesture.Swiped += OnSwiped;
var upSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up };
upSwipeGesture.Swiped += OnSwiped;
var downSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Down };
downSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
boxView.GestureRecognizers.Add(rightSwipeGesture);
boxView.GestureRecognizers.Add(upSwipeGesture);
boxView.GestureRecognizers.Add(downSwipeGesture);
```

> [!NOTE]
> 上記の例では、同じイベント ハンドラーに応答する、 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)イベントの発生します。 ただし、各[ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)必要に応じて、インスタンスが別のイベント ハンドラーを使用できます。

## <a name="responding-to-the-swipe"></a>スワイプへの応答

イベント ハンドラー、 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)イベントが次の例で示すようにします。

```csharp
void OnSwiped(object sender, SwipedEventArgs e)
{
    switch (e.Direction)
    {
        case SwipeDirection.Left:
            // Handle the swipe
            break;
        case SwipeDirection.Right:
            // Handle the swipe
            break;
        case SwipeDirection.Up:
            // Handle the swipe
            break;
        case SwipeDirection.Down:
            // Handle the swipe
            break;
    }
}
```

[ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs)調べる必要に応じてスワイプに応答してカスタム ロジックを持つ、スワイプの方向を判断することができます。 スワイプの方向を取得できます、 [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction)プロパティの値のいずれかに設定されるイベント引数の[ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection)列挙体。 さらに、イベントの引数もある、 [ `Parameter` ](xref:Xamarin.Forms.SwipedEventArgs.Parameter)プロパティの値に設定される、 [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter)プロパティで定義されている場合。

## <a name="using-commands"></a>コマンドを使用します。

[ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)クラスも含まれています。 [ `Command` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Command)と[ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter)プロパティ。 通常、これらのプロパティは、モデル-ビュー-ビューモデル (MVVM) パターンを使用するアプリケーションで使用されます。 `Command`プロパティを定義、`ICommand`スワイプ ジェスチャを認識すると、ときに呼び出されると、`CommandParameter`に渡されるオブジェクトを定義するプロパティ、`ICommand.`次のコード例は、バインドする方法を示しています、 `Command`プロパティを`ICommand`ページとして設定すると、インスタンスをビュー モデルで定義されている[ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext):

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

同等の XAML コードに示します。

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` 型のプロパティは、`ICommand`ページとして設定されているビュー モデルのインスタンスで定義されている[ `BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)します。 スワイプ ジェスチャが認識される、`Execute`のメソッド、`SwipeCommand`オブジェクトが実行されます。 引数、`Execute`メソッドの値である、 [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter)プロパティ。 コマンドの詳細については、次を参照してください。 [、のコマンド インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)します。

## <a name="creating-a-swipe-container"></a>スワイプ コンテナーを作成します。

`SwipeContainer` 、次のコード例に示すクラスは、ある一般化されたスワイプ認識クラスにラップされる、 [ `View` ](xref:Xamarin.Forms.View)スワイプ ジェスチャ認識を実行します。

```csharp
public class SwipeContainer : ContentView
{
    public event EventHandler<SwipedEventArgs> Swipe;

    public SwipeContainer()
    {
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Left));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Right));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Up));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Down));
    }

    SwipeGestureRecognizer GetSwipeGestureRecognizer(SwipeDirection direction)
    {
        var swipe = new SwipeGestureRecognizer { Direction = direction };
        swipe.Swiped += (sender, e) => Swipe?.Invoke(this, e);
        return swipe;
    }
}
```

`SwipeContainer`クラスを作成します[ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)オブジェクトに対するすべての 4 つのスワイプ方向とアタッチ`Swipe`イベント ハンドラー。 これらのイベント ハンドラーを呼び出す、`Swipe`によって定義されたイベント、`SwipeContainer`します。

次の XAML コード例は、`SwipeContainer`クラスの折り返し、 [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

次のコード例に示す方法、`SwipeContainer`ラップ、 [ `BoxView` ](xref:Xamarin.Forms.BoxView)で、C#ページ。

```csharp
public class SwipeContainerPageCS : ContentPage
{
    public SwipeContainerPageCS()
    {
        var boxView = new BoxView { Color = Color.Teal, ... };
        var swipeContainer = new SwipeContainer { Content = boxView, ... };
        swipeContainer.Swipe += (sender, e) =>
        {
          // Handle the swipe
        };

        Content = new StackLayout
        {
            Children = { swipeContainer }
        };
    }
}
```

ときに、 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 、スワイプ ジェスチャを受け取る、 [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped)内のイベント、 [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer)が発生します。 これによって処理されますが、`SwipeContainer`クラスは、独自に発生する`Swipe`イベント。 これは、`Swipe`ページでイベントを処理します。 [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs)必要に応じてスワイプに応答してカスタム ロジックを持つ、スワイプの方向を決定する、この方法を調査することができます。

## <a name="related-links"></a>関連リンク

- [スワイプ ジェスチャ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
