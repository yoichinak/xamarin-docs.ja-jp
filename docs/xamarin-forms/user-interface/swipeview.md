---
title: " Xamarin.Forms SwipeView" description: " Xamarin.Forms SwipeView は、コンテンツの項目を囲むコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。
ms. 製品: xamarin ms assetId: 602456B5-701B-4948-B454: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 03/26/2020 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="xamarinforms-swipeview"></a>Xamarin.FormsSwipeView

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

`SwipeView`は、コンテンツの項目をラップするコンテナーコントロールであり、スワイプジェスチャによって表示されるコンテキストメニュー項目を提供します。

[![IOS と Android の CollectionView の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-collectionview.png "SwipeView スワイプ項目")](swipeview-images/swipeview-collectionview-large.png#lightbox "SwipeView スワイプ項目")

`SwipeView`は4.4 で使用でき Xamarin.Forms ます。 ただし、現在は実験的であり、次のコード行を `AppDelegate` iOS 上のクラス、Android 上のクラス、 `MainActivity` または UWP のクラスに追加してからを呼び出す必要があり `App` `Forms.Init` ます。

```csharp
Forms.SetFlags("SwipeView_Experimental");
```

`SwipeView` は次の特性を定義します。

- `LeftItems`型の `SwipeItems` 。これは、コントロールが左側からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `RightItems`型の `SwipeItems` 。コントロールが右側からスワイプたときに呼び出すことができるスワイプ項目を表します。
- `TopItems``SwipeItems`コントロールが上からスワイプされたときに呼び出すことができるスワイプ項目を表す、型の。
- `BottomItems``SwipeItems`コントロールが下からスワイプされたときに呼び出すことができるスワイプ項目を表す、型の。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

さらに、は、 `SwipeView` [`Content`](xref:Xamarin.Forms.ContentView.Content) クラスからプロパティを継承し [`ContentView`](xref:Xamarin.Forms.ContentView) ます。 `Content`プロパティはクラスの content プロパティである `SwipeView` ため、明示的に設定する必要はありません。

クラスは、 `SwipeView` 次の4つのイベントも定義します。

- `SwipeStarted`スワイプが開始されたときに発生します。 `SwipeStartedEventArgs`このイベントに付随するオブジェクトには `SwipeDirection` 、型のプロパティがあり `SwipeDirection` ます。
- `SwipeChanging`スワイプが移動したときに発生します。 `SwipeChangingEventArgs`このイベントに付随するオブジェクトには、 `SwipeDirection` 型のプロパティ `SwipeDirection` と `Offset` 型のプロパティがあり `double` ます。
- `SwipeEnded`スワイプが終了したときに発生します。 `SwipeEndedEventArgs`このイベントに付随するオブジェクトには `SwipeDirection` 、型のプロパティがあり `SwipeDirection` ます。
- `CloseRequested`スワイプ項目が閉じられたときに発生します。

さらに、に `SwipeView` はメソッドとメソッドが含まれており、プログラムを使用して `Open` `Close` スワイプ項目を開いたり閉じたりすることができます。

> [!NOTE]
> `SwipeView`には、iOS および Android のプラットフォーム固有のがあり、を開くときに使用される遷移を制御し `SwipeView` ます。 詳細については、「 [SwipeView スワイプ Transition mode On iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) 」および「 [SwipeView スワイプ Transition mode on Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md)」を参照してください。

## <a name="create-a-swipeview"></a>SwipeView を作成する

は `SwipeView` 、がラップするコンテンツ `SwipeView` と、スワイプジェスチャによって公開されるスワイプ項目を定義する必要があります。 スワイプ項目は `SwipeItem` 、、、、の4つの方向のコレクションのいずれかに配置される1つ以上のオブジェクトです `SwipeView` `LeftItems` `RightItems` `TopItems` `BottomItems` 。

次の例は、XAML でをインスタンス化する方法を示してい `SwipeView` ます。

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

これに相当する C# コードを次に示します。

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

この例では、 `SwipeView` コンテンツは、を [`Grid`](xref:Xamarin.Forms.Grid) 含むです [`Label`](xref:Xamarin.Forms.Label) 。

[![IOS と Android の SwipeView コンテンツのスクリーンショット](swipeview-images/swipeview-content.png "SwipeView コンテンツ")](swipeview-images/swipeview-content-large.png#lightbox "SwipeView コンテンツ")

スワイプ項目は、コンテンツに対する操作を実行するために使用され `SwipeView` ます。また、コントロールが左側からスワイプされると、次の項目が明らかになります。

[![IOS と Android の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-swipeitems.png "SwipeView スワイプ項目")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView スワイプ項目")

既定では、スワイプ項目はユーザーによってタップされると実行されます。 ただし、この動作は変更可能です。 詳細については、「[スワイプモード](#swipe-mode)」を参照してください。

スワイプ項目が実行されると、スワイプ項目が非表示に `SwipeView` なり、コンテンツが再度表示されます。 ただし、この動作は変更可能です。 詳細については、「[スワイプ動作](#swipe-behavior)」を参照してください。

> [!NOTE]
> コンテンツのスワイプとスワイプは、インラインに配置したり、リソースとして定義したりすることができます。

## <a name="swipe-items"></a>項目のスワイプ

`LeftItems`、、 `RightItems` 、およびの各 `TopItems` `BottomItems` コレクションは、すべて型 `SwipeItems` です。 `SwipeItems`クラスは、次のプロパティを定義します。

- `Mode``SwipeMode`スワイプ操作の効果を示す型の。 スワイプモードの詳細については、「[スワイプモード](#swipe-mode)」を参照してください。
- `SwipeBehaviorOnInvoked`型の `SwipeBehaviorOnInvoked` `SwipeView` 。スワイプ項目が呼び出された後のの動作を示します。 スワイプ動作の詳細については、「[スワイプ動作](#swipe-behavior)」を参照してください。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

各スワイプ項目は `SwipeItem` 、4つの方向のコレクションのいずれかに配置されるオブジェクトとして定義され `SwipeItems` ます。 `SwipeItem`クラスはクラスから派生 [`MenuItem`](xref:Xamarin.Forms.MenuItem) し、次のメンバーを追加します。

- `BackgroundColor` `Color` スワイプ項目の背景色を定義する型のプロパティ。 このプロパティは、バインド可能なプロパティによってサポートされます。
- `Invoked`スワイプ項目が実行されるときに発生するイベント。

> [!IMPORTANT]
> クラスは、、、、など、 [`MenuItem`](xref:Xamarin.Forms.MenuItem) いくつかのプロパティを定義し `Command` `CommandParameter` `IconImageSource` `Text` ます。 これらのプロパティは、オブジェクトの `SwipeItem` 外観を定義したり、 `ICommand` スワイプ項目が呼び出されたときに実行されるを定義したりするために設定できます。 詳細については、「 [ Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)」を参照してください。

次の例で `SwipeItem` `LeftItems` は、のコレクション内の2つのオブジェクトを示し `SwipeView` ます。

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

それぞれの外観 `SwipeItem` は `Text` 、、、およびの各プロパティの組み合わせによって定義され `IconImageSource` `BackgroundColor` ます。

[![IOS と Android の SwipeView スワイプ項目のスクリーンショット](swipeview-images/swipeview-swipeitems.png "SwipeView スワイプ項目")](swipeview-images/swipeview-swipeitems-large.png#lightbox "SwipeView スワイプ項目")

`SwipeItem`がタップされると、その `Invoked` イベントが発生し、登録されているイベントハンドラーによって処理されます。 または、 `Command` `ICommand` が呼び出されたときに実行される実装にプロパティを設定することもでき `SwipeItem` ます。

> [!NOTE]
> の外観 `SwipeItem` がプロパティまたはプロパティを使用してのみ定義されている場合 `Text` `IconImageSource` 、コンテンツは常に中央揃えになります。

スワイプ項目をオブジェクトとして定義するだけで `SwipeItem` なく、カスタムスワイプ項目ビューを定義することもできます。 詳細については、「[カスタムスワイプ項目](#custom-swipe-items)」を参照してください。

## <a name="swipe-direction"></a>スワイプ方向

`SwipeView`4つの異なるスワイプ方向をサポートします。スワイプ方向は、オブジェクトが追加される方向のコレクションによって定義され `SwipeItems` `SwipeItem` ます。 スワイプの方向は、それぞれ独自のスワイプ項目を保持できます。 たとえば、次の例は、スワイプ `SwipeView` 項目がスワイプ方向に依存しているを示しています。

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

この例では、 `SwipeView` コンテンツを右または左にスワイプことができます。 右側にスワイプすると、[**スワイプ] 項目が表示**されます。左側にスワイプすると、**お気に入り**と**共有**スワイプ項目が表示されます。

> [!WARNING]
> で一度に設定できるのは、一方向のコレクションの1つのインスタンスだけ `SwipeItems` `SwipeView` です。 そのため、に2つの定義を含めることはできません `LeftItems` `SwipeView` 。

`SwipeStarted`、、およびの各イベントは、 `SwipeChanging` `SwipeEnded` イベント引数のプロパティを使用して、スワイプの方向を報告し `SwipeDirection` ます。 このプロパティの型は `SwipeDirection` で、次の4つのメンバーで構成される列挙体です。

- `Right`右スワイプが発生したことを示します。
- `Left`左スワイプが発生したことを示します。
- `Up`上向きのスワイプが発生したことを示します。
- `Down`下方向のスワイプが発生したことを示します。

## <a name="swipe-mode"></a>スワイプモード

`SwipeItems`クラスには、 `Mode` スワイプ操作の効果を示すプロパティがあります。 このプロパティは、列挙体のメンバーのいずれかに設定する必要があり `SwipeMode` ます。

- `Reveal`スワイプによってスワイプ項目が表示されることを示します。 これは、`SwipeItems.Mode` プロパティの既定値です。
- `Execute`スワイプがスワイプ項目を実行することを示します。

[表示モード] では、スワイプは、 `SwipeView` 1 つまたは複数のスワイプ項目で構成されるメニューを開き、スワイプ項目を明示的にタップして実行する必要があります。 スワイプ項目が実行されると、スワイプ項目が閉じられ、 `SwipeView` コンテンツが再度表示されます。 実行モードでは、ユーザーはスワイプを使用して `SwipeView` 1 つのスワイプ項目で構成されるメニューを開き、自動的に実行されます。 実行後、スワイプ項目が閉じられ、 `SwipeView` コンテンツが再度表示されます。

次の例は、 `SwipeView` 実行モードを使用するように構成されたを示しています。

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

この例では、 `SwipeView` コンテンツは、すぐに実行されるスワイプ項目を表示するためにスワイプ権限を持つことができます。 実行後に `SwipeView` コンテンツが再表示されます。

## <a name="swipe-behavior"></a>スワイプ動作

`SwipeItems`クラスにはプロパティがあり `SwipeBehaviorOnInvoked` 、これは `SwipeView` スワイプ項目が呼び出された後のの動作を示します。 このプロパティは、列挙体のメンバーのいずれかに設定する必要があり `SwipeBehaviorOnInvoked` ます。

- `Auto`[表示モード] で、 `SwipeView` スワイプ項目が呼び出された後にがを閉じることを示します。実行モードでは、 `SwipeView` スワイプ項目が呼び出された後もが開いたままになります。 これは、`SwipeItems.SwipeBehaviorOnInvoked` プロパティの既定値です。
- `Close``SwipeView`スワイプ項目が呼び出された後にが閉じることを示します。
- `RemainOpen``SwipeView`スワイプ項目が呼び出された後もが開いたままになることを示します。

次の例は、 `SwipeView` スワイプ項目が呼び出された後も開いたままになるように構成されたを示しています。

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

カスタムスワイプ項目は、型を使用して定義でき `SwipeItemView` ます。 `SwipeItemView`クラスはクラスから派生 [`ContentView`](xref:Xamarin.Forms.ContentView) し、次のプロパティを追加します。

- `Command`型の `ICommand` 。スワイプ項目がタップされると実行されます。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

`SwipeItemView`クラスは `Invoked` 、の実行後に項目がタップされたときに発生するイベントも定義し `Command` ます。

次の例は `SwipeItemView` 、のコレクション内のオブジェクトを示してい `LeftItems` `SwipeView` ます。

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

この例では、は `SwipeItemView` とを含むを構成して [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Entry`](xref:Xamarin.Forms.Entry) [`Label`](xref:Xamarin.Forms.Label) います。 ユーザーがに入力を入力すると、の残りの部分をタップして、 `Entry` `SwipeViewItem` `ICommand` プロパティで定義されているを実行でき `SwipeItemView.Command` ます。

## <a name="open-and-close-a-swipeview-programmatically"></a>プログラムによって SwipeView を開く/閉じる

`SwipeView`にはメソッドとメソッドが含まれてい `Open` `Close` ます。これらのメソッドは、プログラムによってスワイプ項目を開いたり閉じたりします。

メソッドには、を `Open` `OpenSwipeItem` 開く方向を指定するための引数が必要です `SwipeView` 。 `OpenSwipeItem`列挙体には、次の4つのメンバーがあります。

- `LeftItems``SwipeView`。コレクション内のスワイプ項目を表示するために、が左から開かれることを示し `LeftItems` ます。
- `TopItems``SwipeView`。コレクション内のスワイプ項目を表示するために、上部からが開かれることを示し `TopItems` ます。
- `RightItems``SwipeView`。コレクション内のスワイプ項目を表示するために、右側からが開かれることを示し `RightItems` ます。
- `BottomItems``SwipeView`。コレクション内のスワイプ項目を表示するために、下部からが開かれることを示し `BottomItems` ます。

という名前のを指定した `SwipeView` `swipeView` 場合、次の例では、を開いてコレクション内のスワイプ項目を表示する方法を示してい `SwipeView` `LeftItems` ます。

```csharp
swipeView.Open(OpenSwipeItem.LeftItems);
```

その後、メソッドを使用してを `swipeView` 閉じることができ `Close` ます。

```csharp
swipeView.Close();
```

> [!NOTE]
> `Close`メソッドが呼び出されると、 `CloseRequested` イベントが発生します。

## <a name="disable-a-swipeview"></a>SwipeView を無効にする

アプリケーションで、コンテンツの項目が有効な操作ではない状態になる場合があります。 このような場合は、 `SwipeView` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。 これにより、ユーザーがコンテンツをスワイプしてスワイプした項目を表示できなくなります。

また、またはのプロパティを定義するときに、のデリゲートを指定して、 `Command` `SwipeItem` `SwipeItemView` `CanExecute` `ICommand` スワイプ項目を有効または無効にすることができます。

## <a name="related-links"></a>関連リンク

- [SwipeView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.FormsMenuItem](~/xamarin-forms/user-interface/menuitem.md)
