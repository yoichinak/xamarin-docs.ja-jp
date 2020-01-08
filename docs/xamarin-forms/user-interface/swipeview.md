---
title: SwipeView
description: SwipeView は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 4119a650c431013bb0c8e680de600ed4e73d0c93
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490506"
---
# <a name="xamarinforms-swipeview"></a>SwipeView

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

`SwipeView` は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。

[![IOS と Android の CollectionView の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-collectionview.png "SwipeView スワイプ項目")](swipeview-images/swipeview-collectionview-large.png#lightbox "SwipeView スワイプ項目")

`SwipeView` は、Xamarin. Forms 4.4 で使用できます。 ただし、現在は実験的で、次のコード行を iOS の `AppDelegate` クラス、Android の `MainActivity` クラス、または UWP の `App` クラスに追加してから、`Forms.Init`を呼び出す前に使用することができます。

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` 次のプロパティを定義します。

- `SwipeItems`型の `LeftItems`。これは、コントロールが左側からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `SwipeItems`型の `RightItems`。コントロールが右側からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `SwipeItems`型の `TopItems`。コントロールが一番上からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `SwipeItems`型の `BottomItems`。コントロールが下からスワイプされたときに呼び出すことができるスワイプ項目を表します。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

さらに、`SwipeView` は、 [`ContentView`](xref:Xamarin.Forms.ContentView)クラスから[`Content`](xref:Xamarin.Forms.ContentView.Content)プロパティを継承します。 `Content` プロパティは `SwipeView` クラスの content プロパティであるため、明示的に設定する必要はありません。

`SwipeView` クラスは、次の4つのイベントも定義します。

- `SwipeStarted` は、スワイプが開始されるときに発生します。 このイベントに付随する `SwipeStartedEventArgs` オブジェクトには、`SwipeDirection`型の `SwipeDirection` プロパティがあります。
- スワイプが移動すると `SwipeChanging` が発生します。 このイベントに付随する `SwipeChangingEventArgs` オブジェクトには、`SwipeDirection`型の `SwipeDirection` プロパティ、および型 `double`の `Offset` プロパティがあります。
- `SwipeEnded` は、スワイプが終了したときに発生します。 このイベントに付随する `SwipeEndedEventArgs` オブジェクトには、`SwipeDirection`型の `SwipeDirection` プロパティがあります。
- `CloseRequested` は、スワイプ項目が閉じられたときに発生します。

さらに、`SwipeView` は、スワイプ項目を閉じる `Close` メソッドを定義します。

> [!NOTE]
> `SwipeView` には、iOS と Android のプラットフォーム固有のものがあり、`SwipeView`を開くときに使用される移行を制御します。 詳細については、「 [SwipeView スワイプ Transition mode On iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) 」および「 [SwipeView スワイプ Transition mode on Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md)」を参照してください。

## <a name="create-a-swipeview"></a>SwipeView を作成する

`SwipeView` では、`SwipeView` がラップするコンテンツと、スワイプジェスチャによって公開されるスワイプ項目を定義する必要があります。 スワイプ項目は、`LeftItems`、`RightItems`、`TopItems`、または `BottomItems`の4つの `SwipeView` 方向コレクションのいずれかに配置される1つ以上の `SwipeItem` オブジェクトです。

次の例では、インスタンス化する方法を示しています、 `SwipeView` XAML で。

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

この例では、`SwipeView` コンテンツは[`Label`](xref:Xamarin.Forms.Label)を含む[`Grid`](xref:Xamarin.Forms.Grid)です。

[![IOS と Android の SwipeView コンテンツのスクリーンショット](swipeview-images/swipeview-content.png "SwipeView コンテンツ")](swipeview-images/swipeview-content-large.png#lightbox "SwipeView コンテンツ")

スワイプ項目は `SwipeView` の内容に対してアクションを実行するために使用され、コントロールが左側からスワイプされると明らかになります。

[![IOS と Android の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-swipeitems.png "SwipeView スワイプ項目")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView スワイプ項目")

既定では、スワイプ項目はユーザーによってタップされると実行されます。 ただし、この動作は変更可能です。 詳細については、「[スワイプモード](#swipe-mode)」を参照してください。

スワイプ項目が実行されると、スワイプ項目は非表示になり、`SwipeView` の内容が再表示されます。 ただし、この動作は変更可能です。 詳細については、「[スワイプ動作](#swipe-behavior)」を参照してください。

> [!NOTE]
> コンテンツのスワイプとスワイプは、インラインに配置したり、リソースとして定義したりすることができます。

## <a name="swipe-items"></a>項目のスワイプ

`LeftItems`、`RightItems`、`TopItems`、および `BottomItems` の各コレクションは、すべて型の `SwipeItems`です。 `SwipeItems` クラスは、次のプロパティを定義します。

- `SwipeMode`型の `Mode`。スワイプ操作の効果を示します。 スワイプモードの詳細については、「[スワイプモード](#swipe-mode)」を参照してください。
- `SwipeBehaviorOnInvoked`型の `SwipeBehaviorOnInvoked`。スワイプ項目が呼び出された後の `SwipeView` の動作を示します。 スワイプ動作の詳細については、「[スワイプ動作](#swipe-behavior)」を参照してください。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

各スワイプ項目は、4つの `SwipeItems` 方向のコレクションのいずれかに配置される `SwipeItem` オブジェクトとして定義されます。 `SwipeItem` クラスは[`MenuItem`](xref:Xamarin.Forms.MenuItem)クラスから派生し、次のメンバーを追加します。

- スワイプ項目の背景色を定義する、`Color`型の `BackgroundColor` プロパティ。 このプロパティは、バインド可能なプロパティによってサポートされます。
- スワイプ項目が実行されるときに発生する `Invoked` イベント。

> [!IMPORTANT]
> [`MenuItem`](xref:Xamarin.Forms.MenuItem)クラスは、`Command`、`CommandParameter`、`IconImageSource`、`Text`など、いくつかのプロパティを定義します。 これらのプロパティは、`SwipeItem` オブジェクトに設定して外観を定義したり、スワイプ項目が呼び出されたときに実行される `ICommand` を定義したりできます。 詳細については、「 [Xamarin. フォーム MenuItem](~/xamarin-forms/user-interface/menuitem.md)」を参照してください。

次の例は、`SwipeView`の `LeftItems` コレクション内の2つの `SwipeItem` オブジェクトを示しています。

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

各 `SwipeItem` の外観は、`Text`、`IconImageSource`、および `BackgroundColor` の各プロパティによって定義されます。

[![IOS と Android の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-swipeitems.png "SwipeView スワイプ項目")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView スワイプ項目")

`SwipeItem` がタップされると、その `Invoked` イベントが発生し、登録されているイベントハンドラーによって処理されます。 または、`Command` プロパティを、`SwipeItem` が呼び出されたときに実行される `ICommand` の実装に設定することもできます。

> [!NOTE]
> スワイプ項目を `SwipeItem` オブジェクトとして定義するだけでなく、カスタムスワイプ項目ビューを定義することもできます。 詳細については、「[カスタムスワイプ項目](#custom-swipe-items)」を参照してください。

## <a name="swipe-direction"></a>スワイプ方向

`SwipeView` では、4つの異なるスワイプ方向がサポートされます。スワイプ方向は、`SwipeItem` オブジェクトが追加される方向 `SwipeItems` コレクションによって定義されます。 スワイプの方向は、それぞれ独自のスワイプ項目を保持できます。 たとえば、次の例は、スワイプ項目がスワイプ方向に依存している `SwipeView` を示しています。

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

この例では、`SwipeView` の内容を右または左にスワイプできます。 右側にスワイプすると、[**スワイプ] 項目が表示**されます。左側にスワイプすると、**お気に入り**と**共有**スワイプ項目が表示されます。

> [!WARNING]
> `SwipeView`で一度に設定できる方向 `SwipeItems` コレクションのインスタンスは1つだけです。 したがって、`SwipeView`に2つの `LeftItems` 定義を設定することはできません。

`SwipeStarted`、`SwipeChanging`、および `SwipeEnded` イベントは、イベント引数の `SwipeDirection` プロパティを使用して、スワイプの方向を報告します。 このプロパティの型は `SwipeDirection`です。これは、次の4つのメンバーで構成される列挙体です。

- `Right` は、右スワイプが発生したことを示します。
- `Left` は、左スワイプが発生したことを示します。
- `Up` は、上向きのスワイプが発生したことを示します。
- `Down` は、下方向のスワイプが発生したことを示します。

## <a name="swipe-mode"></a>スワイプモード

`SwipeItems` クラスには、スワイプ操作の効果を示す `Mode` プロパティがあります。 このプロパティは、`SwipeMode` 列挙型のメンバーのいずれかに設定する必要があります。

- `Reveal` は、スワイプによってスワイプ項目が表示されることを示します。 これは、`SwipeItems.Mode` プロパティの既定値です。
- `Execute` は、スワイプがスワイプ項目を実行することを示します。

[表示モード] では、ユーザーは `SwipeView` をスワイプして、1つまたは複数のスワイプ項目で構成されるメニューを開くことができます。また、スワイプ項目を明示的にタップして実行する必要があります。 スワイプ項目が実行されると、スワイプ項目が閉じられ、`SwipeView` の内容が再表示されます。 実行モードでは、ユーザーは `SwipeView` をスワイプして、1つのスワイプ項目で構成されるメニューを開き、自動的に実行されます。 実行後、スワイプ項目が閉じられ、`SwipeView` の内容が再表示されます。

次の例は、実行モードを使用するように構成された `SwipeView` を示しています。

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

この例では、`SwipeView` コンテンツをスワイプして、スワイプ項目を表示することができます。これはすぐに実行されます。 次に実行すると、`SwipeView` コンテンツが再表示されます。

## <a name="swipe-behavior"></a>スワイプ動作

`SwipeItems` クラスには `SwipeBehaviorOnInvoked` プロパティがあります。これは、スワイプ項目が呼び出された後の `SwipeView` の動作を示します。 このプロパティは、`SwipeBehaviorOnInvoked` 列挙型のメンバーのいずれかに設定する必要があります。

- `Auto` は、[表示] モードでは、スワイプ項目が呼び出された後 `SwipeView` が閉じることを示します。実行モードでは、スワイプ項目が呼び出された後も `SwipeView` は開いたままになります。 これは、`SwipeItems.SwipeBehaviorOnInvoked` プロパティの既定値です。
- `Close` は、スワイプ項目が呼び出された後に `SwipeView` を閉じることを示します。
- `RemainOpen` は、スワイプ項目が呼び出された後も `SwipeView` が開いたままになることを示します。

次の例は、スワイプ項目が呼び出された後も開いたままになるように構成された `SwipeView` を示しています。

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

カスタムスワイプ項目は、`SwipeItemView` の種類を使用して定義できます。 `SwipeItemView` クラスは[`ContentView`](xref:Xamarin.Forms.ContentView)クラスから派生し、次のプロパティを追加します。

- `ICommand`型の `Command`。スワイプ項目がタップされると実行されます。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

`SwipeItemView` クラスは、`Command` の実行後に項目がタップされたときに発生する `Invoked` イベントも定義します。

次の例は、`SwipeView`の `LeftItems` コレクション内の `SwipeItemView` オブジェクトを示しています。

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

この例では、`SwipeItemView` は[`Entry`](xref:Xamarin.Forms.Entry)と[`Label`](xref:Xamarin.Forms.Label)を含む[`StackLayout`](xref:Xamarin.Forms.StackLayout)で構成されています。 ユーザーが `Entry`に入力を入力すると、`SwipeViewItem` の残りの部分をタップして、`SwipeItemView.Command` プロパティで定義されている `ICommand` を実行できます。

## <a name="disable-a-swipeview"></a>SwipeView を無効にする

アプリケーションで、コンテンツの項目が有効な操作ではない状態になる場合があります。 このような場合は、`IsEnabled` プロパティを `false`に設定することによって、`SwipeView` を無効にできます。 これにより、ユーザーがコンテンツをスワイプしてスワイプした項目を表示できなくなります。

また、`SwipeItem` または `SwipeItemView`の `Command` プロパティを定義する場合は、スワイプ項目を有効または無効にするために、`ICommand` の `CanExecute` デリゲートを指定できます。

## <a name="related-links"></a>関連リンク

- [SwipeView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)
