---
title: SwipeView
description: SwipeView は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/26/2020
ms.openlocfilehash: da6dbe63b7151ef0f9a1defca66fbb3abb25ad1d
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517554"
---
# <a name="xamarinforms-swipeview"></a>SwipeView

![](~/media/shared/preview.png "This API is currently pre-release")

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

は`SwipeView` 、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。

[![IOS と Android の CollectionView の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-collectionview.png "SwipeView スワイプ項目")](swipeview-images/swipeview-collectionview-large.png#lightbox "SwipeView スワイプ項目")

`SwipeView`は、Xamarin. Forms 4.4 で使用できます。 ただし、現在は実験的であり、次のコード`AppDelegate`行を iOS `MainActivity`上のクラス、Android 上のクラス、または`App` UWP のクラスに追加してからを呼び出す`Forms.Init`必要があります。

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` は次の特性を定義します。

- `LeftItems`型`SwipeItems`の。これは、コントロールが左側からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `RightItems`型`SwipeItems`の。コントロールが右側からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `TopItems`コントロールが上`SwipeItems`からスワイプされたときに呼び出すことができるスワイプ項目を表す、型の。
- `BottomItems`コントロールが下`SwipeItems`からスワイプされたときに呼び出すことができるスワイプ項目を表す、型の。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

さらに、は`SwipeView` 、 [`ContentView`](xref:Xamarin.Forms.ContentView)クラス[`Content`](xref:Xamarin.Forms.ContentView.Content)からプロパティを継承します。 `Content`プロパティは`SwipeView`クラスの content プロパティであるため、明示的に設定する必要はありません。

クラス`SwipeView`は、次の4つのイベントも定義します。

- `SwipeStarted`スワイプが開始されたときに発生します。 この`SwipeStartedEventArgs`イベントに付随するオブジェクトには`SwipeDirection` 、型`SwipeDirection`のプロパティがあります。
- `SwipeChanging`スワイプが移動したときに発生します。 この`SwipeChangingEventArgs`イベントに付随するオブジェクトには`SwipeDirection` 、型`SwipeDirection`のプロパティと型`Offset` `double`のプロパティがあります。
- `SwipeEnded`スワイプが終了したときに発生します。 この`SwipeEndedEventArgs`イベントに付随するオブジェクトには`SwipeDirection` 、型`SwipeDirection`のプロパティがあります。
- `CloseRequested`スワイプ項目が閉じられたときに発生します。

さらに、 `SwipeView`に`Open`は`Close`メソッドとメソッドが含まれており、プログラムを使用してスワイプ項目を開いたり閉じたりすることができます。

> [!NOTE]
> `SwipeView`には、iOS および Android のプラットフォーム固有のがあり、を開くときに使用される`SwipeView`遷移を制御します。 詳細については、「 [SwipeView スワイプ Transition mode On iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) 」および「 [SwipeView スワイプ Transition mode on Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md)」を参照してください。

## <a name="create-a-swipeview"></a>SwipeView を作成する

は`SwipeView` 、が`SwipeView`ラップするコンテンツと、スワイプジェスチャによって公開されるスワイプ項目を定義する必要があります。 スワイプ項目`LeftItems`は、、 `RightItems`、 `TopItems`、 `SwipeItem`の 4 `SwipeView`つの方向のコレクションのいずれかに配置される1つ`BottomItems`以上のオブジェクトです。

次の例は、XAML でを`SwipeView`インスタンス化する方法を示しています。

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
    <Grid HeightRequest="60"
          WidthRequest="300"
          BackgroundColor="LightGray">
        <Label Text="Swipe right"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</SwipeView>
```

該当の C# コードを次に示します。

```csharp
// SwipeItems
SwipeItem favoriteSwipeItem = new SwipeItem
{
    Text = "Favorite",
    IconImageSource = "favorite.png",
    BackgroundColor = Color.LightGreen
};
favoriteSwipeItem.Invoked += OnFavoriteSwipeItemInvoked;

SwipeItem deleteSwipeItem = new SwipeItem
{
    Text = "Delete",
    IconImageSource = "delete.png",
    BackgroundColor = Color.LightPink
};
deleteSwipeItem.Invoked += OnDeleteSwipeItemInvoked;

List<SwipeItem> swipeItems = new List<SwipeItem>() { favoriteSwipeItem, deleteSwipeItem };

// SwipeView content
Grid grid = new Grid
{
    HeightRequest = 60,
    WidthRequest = 300,
    BackgroundColor = Color.LightGray
};
grid.Children.Add(new Label
{
    Text = "Swipe right",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});

SwipeView swipeView = new SwipeView
{
    LeftItems = new SwipeItems(swipeItems),
    Content = grid
};
```

この例では、 `SwipeView`コンテンツは[`Grid`](xref:Xamarin.Forms.Grid) 、を含むです[`Label`](xref:Xamarin.Forms.Label)。

[![IOS と Android の SwipeView コンテンツのスクリーンショット](swipeview-images/swipeview-content.png "SwipeView コンテンツ")](swipeview-images/swipeview-content-large.png#lightbox "SwipeView コンテンツ")

スワイプ項目は、 `SwipeView`コンテンツに対する操作を実行するために使用されます。また、コントロールが左側からスワイプされると、次の項目が明らかになります。

[![IOS と Android の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-swipeitems.png "SwipeView スワイプ項目")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView スワイプ項目")

既定では、スワイプ項目はユーザーによってタップされると実行されます。 ただし、この動作は変更可能です。 詳細については、「[スワイプモード](#swipe-mode)」を参照してください。

スワイプ項目が実行されると、スワイプ項目が非表示に`SwipeView`なり、コンテンツが再度表示されます。 ただし、この動作は変更可能です。 詳細については、「[スワイプ動作](#swipe-behavior)」を参照してください。

> [!NOTE]
> コンテンツのスワイプとスワイプは、インラインに配置したり、リソースとして定義したりすることができます。

## <a name="swipe-items"></a>項目のスワイプ

、 `LeftItems` `RightItems`、 `TopItems`、および`BottomItems`の各コレクションは、すべて`SwipeItems`型です。 クラス`SwipeItems`は、次のプロパティを定義します。

- `Mode`スワイプ操作の`SwipeMode`効果を示す型の。 スワイプモードの詳細については、「[スワイプモード](#swipe-mode)」を参照してください。
- `SwipeBehaviorOnInvoked`型`SwipeBehaviorOnInvoked`の。スワイプ項目が呼び出され`SwipeView`た後のの動作を示します。 スワイプ動作の詳細については、「[スワイプ動作](#swipe-behavior)」を参照してください。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

各スワイプ項目は、4つ`SwipeItem` `SwipeItems`の方向のコレクションのいずれかに配置されるオブジェクトとして定義されます。 クラス`SwipeItem`は[`MenuItem`](xref:Xamarin.Forms.MenuItem)クラスから派生し、次のメンバーを追加します。

- スワイプ`BackgroundColor`項目の背景色`Color`を定義する型のプロパティ。 このプロパティは、バインド可能なプロパティによってサポートされます。
- スワイプ`Invoked`項目が実行されるときに発生するイベント。

> [!IMPORTANT]
> クラス[`MenuItem`](xref:Xamarin.Forms.MenuItem)は`Command`、 `CommandParameter`、、 `IconImageSource`、など、いくつかの`Text`プロパティを定義します。 これらのプロパティは、 `SwipeItem`オブジェクトの外観を定義したり、スワイプ項目が呼び出さ`ICommand`れたときに実行されるを定義したりするために設定できます。 詳細については、「 [Xamarin. フォーム MenuItem](~/xamarin-forms/user-interface/menuitem.md)」を参照してください。

次の例では`SwipeItem` 、の`LeftItems`コレクション内の 2 `SwipeView`つのオブジェクトを示します。

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

それぞれの外観は`SwipeItem` `Text`、、 `IconImageSource`、および`BackgroundColor`の各プロパティの組み合わせによって定義されます。

[![IOS と Android の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-swipeitems.png "SwipeView スワイプ項目")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView スワイプ項目")

`SwipeItem`がタップされると、 `Invoked`そのイベントが発生し、登録されているイベントハンドラーによって処理されます。 また`SwipeItem`は、 `Command`が呼び出されたときに`ICommand`実行される実装にプロパティを設定することもできます。

> [!NOTE]
> の外観`SwipeItem`がプロパティ`Text`または`IconImageSource`プロパティを使用してのみ定義されている場合、コンテンツは常に中央揃えになります。

スワイプ項目をオブジェクトとして`SwipeItem`定義するだけでなく、カスタムスワイプ項目ビューを定義することもできます。 詳細については、「[カスタムスワイプ項目](#custom-swipe-items)」を参照してください。

## <a name="swipe-direction"></a>スワイプ方向

`SwipeView`4つの異なるスワイプ方向をサポートします。スワイプ方向は、 `SwipeItems` `SwipeItem`オブジェクトが追加される方向のコレクションによって定義されます。 スワイプの方向は、それぞれ独自のスワイプ項目を保持できます。 たとえば、次の例は、スワイプ`SwipeView`項目がスワイプ方向に依存しているを示しています。

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <SwipeView.RightItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Command="{Binding FavoriteCommand}" />
            <SwipeItem Text="Share"
                       IconImageSource="share.png"
                       BackgroundColor="LightYellow"
                       Command="{Binding ShareCommand}" />
        </SwipeItems>
    </SwipeView.RightItems>
    <!-- Content -->
</SwipeView>
```

この例では、 `SwipeView`コンテンツを右または左にスワイプことができます。 右側にスワイプすると、[**スワイプ] 項目が表示**されます。左側にスワイプすると、**お気に入り**と**共有**スワイプ項目が表示されます。

> [!WARNING]
> で一度に設定できるの`SwipeItems`は、一方向のコレクションの1つ`SwipeView`のインスタンスだけです。 そのため、に2つ`LeftItems`の定義を`SwipeView`含めることはできません。

、 `SwipeStarted` `SwipeChanging`、および`SwipeEnded`の各イベントは、イベント引数の`SwipeDirection`プロパティを使用して、スワイプの方向を報告します。 このプロパティの型`SwipeDirection`はで、次の4つのメンバーで構成される列挙体です。

- `Right`右スワイプが発生したことを示します。
- `Left`左スワイプが発生したことを示します。
- `Up`上向きのスワイプが発生したことを示します。
- `Down`下方向のスワイプが発生したことを示します。

## <a name="swipe-mode"></a>スワイプモード

クラス`SwipeItems`には、 `Mode`スワイプ操作の効果を示すプロパティがあります。 このプロパティは、 `SwipeMode`列挙体のメンバーのいずれかに設定する必要があります。

- `Reveal`スワイプによってスワイプ項目が表示されることを示します。 これは、`SwipeItems.Mode` プロパティの既定値です。
- `Execute`スワイプがスワイプ項目を実行することを示します。

[表示モード] では、スワイプ`SwipeView`は、1つまたは複数のスワイプ項目で構成されるメニューを開き、スワイプ項目を明示的にタップして実行する必要があります。 スワイプ項目が実行されると、スワイプ項目が閉じられ`SwipeView` 、コンテンツが再度表示されます。 実行モードでは`SwipeView` 、ユーザーはスワイプを使用して1つのスワイプ項目で構成されるメニューを開き、自動的に実行されます。 実行後、スワイプ項目が閉じられ、 `SwipeView`コンテンツが再度表示されます。

次の例は、 `SwipeView`実行モードを使用するように構成されたを示しています。

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems Mode="Execute">
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

この例では、 `SwipeView`コンテンツは、すぐに実行されるスワイプ項目を表示するためにスワイプ権限を持つことができます。 実行後に`SwipeView`コンテンツが再表示されます。

## <a name="swipe-behavior"></a>スワイプ動作

`SwipeItems`クラスには`SwipeBehaviorOnInvoked`プロパティがあり、これはスワイプ項目が呼び出された後のの`SwipeView`動作を示します。 このプロパティは、 `SwipeBehaviorOnInvoked`列挙体のメンバーのいずれかに設定する必要があります。

- `Auto`[表示モード] で、 `SwipeView`スワイプ項目が呼び出された後にがを閉じることを`SwipeView`示します。実行モードでは、スワイプ項目が呼び出された後もが開いたままになります。 これは、`SwipeItems.SwipeBehaviorOnInvoked` プロパティの既定値です。
- `Close`スワイプ項目が`SwipeView`呼び出された後にが閉じることを示します。
- `RemainOpen`スワイプ項目が`SwipeView`呼び出された後もが開いたままになることを示します。

次の例は、 `SwipeView`スワイプ項目が呼び出された後も開いたままになるように構成されたを示しています。

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems SwipeBehaviorOnInvoked="RemainOpen">
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

## <a name="custom-swipe-items"></a>カスタムスワイプ項目

カスタムスワイプ項目は、 `SwipeItemView`型を使用して定義できます。 クラス`SwipeItemView`は[`ContentView`](xref:Xamarin.Forms.ContentView)クラスから派生し、次のプロパティを追加します。

- `Command`型`ICommand`の。スワイプ項目がタップされると実行されます。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

クラスは、の`Command`実行`Invoked`後に項目がタップされたときに発生するイベントも定義します。 `SwipeItemView`

次の例は、 `SwipeItemView` `LeftItems` `SwipeView`のコレクション内のオブジェクトを示しています。

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItemView Command="{Binding CheckAnswerCommand}"
                           CommandParameter="{Binding Source={x:Reference resultEntry}, Path=Text}">
                <StackLayout Margin="10"
                             WidthRequest="300">
                    <Entry x:Name="resultEntry"
                           Placeholder="Enter answer"
                           HorizontalOptions="CenterAndExpand" />
                    <Label Text="Check"
                           FontAttributes="Bold"
                           HorizontalOptions="Center" />
                </StackLayout>
            </SwipeItemView>
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

この例では、 `SwipeItemView`は[`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Entry`](xref:Xamarin.Forms.Entry)とを[`Label`](xref:Xamarin.Forms.Label)含むを構成しています。 `Entry`ユーザーがに入力を入力すると、の残りの`SwipeViewItem`部分をタップして、 `ICommand` `SwipeItemView.Command`プロパティで定義されているを実行できます。

## <a name="open-and-close-a-swipeview-programmatically"></a>プログラムによって SwipeView を開いて閉じる

`SwipeView`に`Open`は`Close`メソッドとメソッドが含まれています。これらのメソッドは、プログラムによってスワイプ項目を開いたり閉じたりします。

`Open`メソッドには、 `OpenSwipeItem`を開く方向`SwipeView`を指定するための引数が必要です。 列挙`OpenSwipeItem`体には、次の4つのメンバーがあります。

- `LeftItems`。 `LeftItems`コレクション内のスワイプ`SwipeView`項目を表示するために、が左から開かれることを示します。
- `TopItems`。 `TopItems`コレクション内のスワイプ`SwipeView`項目を表示するために、上部からが開かれることを示します。
- `RightItems`。 `RightItems`コレクション内のスワイプ`SwipeView`項目を表示するために、右側からが開かれることを示します。
- `BottomItems`。 `BottomItems`コレクション内のスワイプ`SwipeView`項目を表示するために、下部からが開かれることを示します。

`SwipeView`という名前`swipeView`のを指定した場合、次の`SwipeView`例では、を開いて`LeftItems`コレクション内のスワイプ項目を表示する方法を示しています。

```csharp
swipeView.Open(OpenSwipeItem.LeftItems);
```

その`swipeView`後、 `Close`メソッドを使用してを閉じることができます。

```csharp
swipeView.Close();
```

> [!NOTE]
> `Close`メソッドが呼び出されると、 `CloseRequested`イベントが発生します。

## <a name="disable-a-swipeview"></a>SwipeView を無効にする

アプリケーションで、コンテンツの項目が有効な操作ではない状態になる場合があります。 このような場合は`SwipeView` 、 `IsEnabled`プロパティをに設定する`false`ことで、を無効にすることができます。 これにより、ユーザーがコンテンツをスワイプしてスワイプした項目を表示できなくなります。

`SwipeItem`また、または`SwipeItemView` `CanExecute`の`ICommand` `Command`プロパティを定義するときに、のデリゲートを指定して、スワイプ項目を有効または無効にすることができます。

## <a name="related-links"></a>関連リンク

- [SwipeView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms の MenuItem](~/xamarin-forms/user-interface/menuitem.md)
