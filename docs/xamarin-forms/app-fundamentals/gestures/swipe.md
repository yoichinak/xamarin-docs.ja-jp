---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 03663803313c870a3361c6e1ffc85cf1f8999068
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137886"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>スワイプ ジェスチャ認識エンジンの追加

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)

_スワイプ ジェスチャが発生するのは、指が画面に沿って水平または垂直方向に動かされたときで、多くの場合コンテンツのナビゲーションを開始するために使われます。この記事のコード例は、[Swipe Gesture](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture) サンプルから取得されています。_

[`View`](xref:Xamarin.Forms.View) にスワイプ ジェスチャを認識させるには、[`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) インスタンスを作成し、[`Direction`](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) プロパティに [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) 列挙値 (`Left`、`Right`、`Up`、または `Down`) を設定し、必要に応じて [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) プロパティを設定し、[`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) イベントを処理して、新しいジェスチャ認識エンジンをビューの [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) コレクションに追加します。 次に示すコード例は、[`BoxView`](xref:Xamarin.Forms.BoxView) に関連付けられている `SwipeGestureRecognizer` です。

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

次に示すのは、同等の C# コードです。

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

[`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) クラスには [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) プロパティも含まれています。このプロパティには、必要に応じて、スワイプが認識されるために必要な最小スワイプ距離を表す、デバイスに依存しない単位の `uint` 値を設定できます。 このプロパティの既定値は 100 で、これはデバイスに依存しない単位で 100 未満のスワイプは無視されることを意味します。

## <a name="recognizing-the-swipe-direction"></a>スワイプの方向を認識する

上の例では、[`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) プロパティに [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) 列挙型の 1 つの値が設定されています。 ただし、このプロパティに `SwipeDirection` 列挙型の複数の値を設定し、複数の方向のスワイプに対して [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) イベントが生成されるようにすることもできます。 ただし、1 つの [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) では同じ軸上で発生するスワイプしか認識できないという制約があります。 したがって、水平軸上で発生するスワイプは、`Direction` プロパティに `Left` と `Right` を設定することによって認識できます。

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

同様に、垂直軸上で発生するスワイプは、[`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) プロパティに `Up` と `Down` を設定することによって認識できます。

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

または、各スワイプ方向に対する [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) を作成すると、すべての方向のスワイプを認識することができます。

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

C# での同等のコードを次に示します。

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
> 上の例では、[`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) イベントの発生に対して同じイベント ハンドラーが応答します。 ただし、各 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) インスタンスでは、必要に応じて、異なるイベント ハンドラーを使用できます。

## <a name="responding-to-the-swipe"></a>スワイプに応答する

次の例では、[`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) イベントに対するイベント ハンドラーを示します。

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

[`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs) を調べることでスワイプの方向を特定し、必要に応じてカスタム ロジックでスワイプに応答できます。 スワイプの方向は、イベント引数の [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) プロパティから取得できます。このプロパティには、[`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) 列挙型の値のいずれかが設定されます。 さらに、イベント引数には [`Parameter`](xref:Xamarin.Forms.SwipedEventArgs.Parameter) プロパティもあり、[`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) プロパティ (定義されている場合) の値が設定されます。

## <a name="using-commands"></a>コマンドを使用する

[`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) クラスには、[`Command`](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) プロパティと [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) プロパティも含まれています。 通常、これらのプロパティは、Model-View-ViewModel (MVVM) パターンを使用するアプリケーションで使用されます。 `Command` プロパティではスワイプ ジェスチャが認識されたときに呼び出される `ICommand` が定義されており、`CommandParameter` プロパティでは `ICommand.` プロパティに渡されるオブジェクトが定義されています。次のコード例では、インスタンスがページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) として設定されているビュー モデルで定義されている `ICommand` に `Command` プロパティをバインドする方法が示されています。

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

同等の XAML コードを次に示します。

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` は、ページの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) として設定されているビュー モデルのインスタンスで定義されている `ICommand` 型のプロパティです。 スワイプ ジェスチャが認識されると、`SwipeCommand` オブジェクトの `Execute` メソッドが実行されます。 `Execute` メソッドへの引数は、[`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) プロパティの値です。 コマンドについて詳しくは、[コマンド インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)に関するページをご覧ください。

## <a name="creating-a-swipe-container"></a>スワイプ コンテナーを作成する

次のコード例で示されている `SwipeContainer` クラスは、スワイプ ジェスチャの認識を実行するために [`View`](xref:Xamarin.Forms.View) にラップされる一般化されたスワイプ認識クラスです。

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

`SwipeContainer` クラスでは、4 つのスワイプ方向すべてに対して [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) オブジェクトが作成されて、`Swipe` イベント ハンドラーが関連付けられます。 これらのイベント ハンドラーでは、`SwipeContainer` によって定義されている `Swipe` イベントが呼び出されます。

次に示すのは、[`BoxView`](xref:Xamarin.Forms.BoxView) をラップしている `SwipeContainer` クラスの XAML コード例です。

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

次に示すコードは、C# ページにおいて `SwipeContainer` で [`BoxView`](xref:Xamarin.Forms.BoxView) をラップする方法の例です。

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

[`BoxView`](xref:Xamarin.Forms.BoxView) がスワイプ ジェスチャを受け取ると、[`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) で [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) イベントが生成されます。 これは `SwipeContainer` クラスによって処理され、クラスで独自の `Swipe` イベントが生成されます。 この `Swipe` イベントは、ページで処理されます。 その後、[`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs) を調べることでスワイプの方向を特定し、必要に応じてカスタム ロジックでスワイプに応答できます。

## <a name="related-links"></a>関連リンク

- [スワイプ ジェスチャ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
