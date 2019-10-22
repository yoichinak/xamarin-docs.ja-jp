---
title: CollectionView のスクロール
description: ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、CollectionView は2つの ScrollTo メソッドを定義して、プログラムによって項目をビューにスクロールします。
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 7aef14cbb854d89a2088a450353b943402f76a86
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697216"
---
# <a name="xamarinforms-collectionview-scrolling"></a>CollectionView のスクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、項目をビューにスクロールする2つの[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドを定義します。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。 どちらのオーバーロードにも、項目が属するグループを示すために指定できる追加の引数、スクロールが完了した後の項目の正確な位置、およびスクロールをアニメーション化するかどうかを指定できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、いずれかの[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されたときに発生する[`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)イベントを定義します。 @No__t_2 イベントに付随する[`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs)オブジェクトには、`IsAnimated`、`Index`、`Item`、`ScrollToPosition` を含む多くのプロパティがあります。 これらのプロパティは、`ScrollTo` メソッドの呼び出しで指定された引数によって設定されます。

さらに、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、スクロールが発生したことを示すために発生する `Scrolled` イベントを定義します。 @No__t_1 イベントに付随する `ItemsViewScrolledEventArgs` オブジェクトには、多くのプロパティがあります。 詳細については、「[スクロールの検出](#detect-scrolling)」を参照してください。

また[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、新しい項目が追加されたときの `CollectionView` のスクロール動作を表す `ItemsUpdatingScrollMode` プロパティも定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](#control-scroll-position-when-new-items-are-added)」を参照してください。

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 スクロールが停止したときに項目が位置にスナップされるため、この機能はスナップと呼ばれています。 詳細については、「[スナップポイント](#snap-points)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、ユーザーがスクロールするときにデータを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](populate-data.md#load-data-incrementally)」を参照してください。

## <a name="detect-scrolling"></a>スクロールの検出

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、スクロールが発生したことを示すために発生する `Scrolled` イベントを定義します。 次の XAML の例は、`Scrolled` イベントのイベントハンドラーを設定する `CollectionView` を示しています。

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

このコード例では、`Scrolled` イベントが発生すると、`OnCollectionViewScrolled` イベントハンドラーが実行されます。

```csharp
void OnCollectionViewScrolled(object sender, ItemsViewScrolledEventArgs e)
{
    Debug.WriteLine("HorizontalDelta: " + e.HorizontalDelta);
    Debug.WriteLine("VerticalDelta: " + e.VerticalDelta);
    Debug.WriteLine("HorizontalOffset: " + e.HorizontalOffset);
    Debug.WriteLine("VerticalOffset: " + e.VerticalOffset);
    Debug.WriteLine("FirstVisibleItemIndex: " + e.FirstVisibleItemIndex);
    Debug.WriteLine("CenterItemIndex: " + e.CenterItemIndex);
    Debug.WriteLine("LastVisibleItemIndex: " + e.LastVisibleItemIndex);
}
```

この例では、`OnCollectionViewScrolled` イベントハンドラーによって、イベントに付随する `ItemsViewScrolledEventArgs` オブジェクトの値が出力されます。

> [!IMPORTANT]
> @No__t_0 イベントは、ユーザーが開始したスクロール、およびプログラムによるスクロールのために発生します。

## <a name="scroll-an-item-at-an-index-into-view"></a>インデックスにある項目をスクロールして表示する

最初の[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドオーバーロードは、指定されたインデックス位置にある項目をビューにスクロールします。 @No__t_2 という名前の[`CollectionView`](xref:Xamarin.Forms.CollectionView)オブジェクトを指定した場合、次の例では、インデックス12の項目をビューにスクロールする方法を示しています。

```csharp
collectionView.ScrollTo(12);
```

また、アイテムとグループのインデックスを指定することで、グループ化されたデータのアイテムをスクロールして表示することもできます。 次の例は、2番目のグループの3番目の項目をスクロールして表示する方法を示しています。

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> [@No__t_3](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されると、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)イベントが発生します。

## <a name="scroll-an-item-into-view"></a>項目をスクロールして表示する

2番目の[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドオーバーロードは、指定された項目をビューにスクロールします。 @No__t_2 という名前の[`CollectionView`](xref:Xamarin.Forms.CollectionView)オブジェクトがある場合、次の例は、Proboscis のサル項目をスクロールして表示する方法を示しています。

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

また、アイテムとグループを指定すると、グループ化されたデータのアイテムをスクロールして表示することもできます。 次の例では、猿グループ内の Proboscis サル項目をスクロールして表示する方法を示します。

```csharp
GroupedAnimalsViewModel viewModel = BindingContext as GroupedAnimalsViewModel;
AnimalGroup group = viewModel.Animals.FirstOrDefault(a => a.Name == "Monkeys");
Animal monkey = group.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey, group);
```

> [!NOTE]
> [@No__t_3](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されると、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)イベントが発生します。

## <a name="disable-scroll-animation"></a>スクロールアニメーションを無効にする

スクロールアニメーションは、項目をビューにスクロールしたときに表示されます。 ただし、このアニメーションは、`ScrollTo` メソッドの `animate` 引数を `false` に設定することによって無効にすることができます。

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>コントロールのスクロール位置

項目をビューにスクロールすると、スクロールが完了した後の項目の正確な位置を、 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドの `position` 引数を使用して指定できます。 この引数は、 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition)列挙型のメンバーを受け取ります。

### <a name="makevisible"></a>MakeVisible

[@No__t_1](xref:Xamarin.Forms.ScrollToPosition)メンバーは、ビューに表示されるまで項目をスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

このコード例では、項目をスクロールして表示するために必要な最小限のスクロールが実行されます。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-makevisible.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

> [!NOTE]
> @No__t_3 メソッドを呼び出すときに `position` 引数が指定されていない場合、既定では[`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)メンバーが使用されます。

### <a name="start"></a>[開始]

[@No__t_1](xref:Xamarin.Forms.ScrollToPosition)メンバーは、項目をビューの先頭までスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

次のコード例では、項目がビューの先頭にスクロールされます。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-start.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-start-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

### <a name="center"></a>中央揃え

[@No__t_1](xref:Xamarin.Forms.ScrollToPosition)メンバーは、アイテムをビューの中央にスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

次のコード例では、アイテムがビューの中央にスクロールされます。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-center.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-center-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

### <a name="end"></a>終了

[@No__t_1](xref:Xamarin.Forms.ScrollToPosition)メンバーは、項目をビューの最後までスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

次のコード例では、ビューの最後までスクロールされる項目になります。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-end.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-end-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

## <a name="control-scroll-position-when-new-items-are-added"></a>新しい項目が追加されたときのスクロール位置の制御

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、バインド可能なプロパティによってサポートされる `ItemsUpdatingScrollMode` プロパティを定義します。 このプロパティは、新しい項目が追加されたときの `CollectionView` のスクロール動作を表す `ItemsUpdatingScrollMode` 列挙値を取得または設定します。 `ItemsUpdatingScrollMode` 列挙体を使って、次のメンバーを定義できます。

- `KeepItemsInView` は、新しい項目が追加されたときに表示される最初の項目を維持するために、スクロールのオフセットを調整します。
- `KeepScrollOffset` は、新しい項目が追加されたときに、リストの先頭を基準としたスクロールオフセットを維持します。
- `KeepLastItemInView` は、新しい項目が追加されたときに最後の項目が表示されるように、スクロールのオフセットを調整します。

@No__t_0 プロパティの既定値は `KeepItemsInView` です。 したがって、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)に新しい項目が追加されると、一覧に表示されている最初の項目が表示されたままになります。 新しく追加された項目が一覧の下部に常に表示されるようにするには、`ItemsUpdatingScrollMode` プロパティを `KeepLastItemInView` に設定する必要があります。

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>スクロールバーの表示

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、バインド可能なプロパティによってサポートされる `HorizontalScrollBarVisibility` と `VerticalScrollBarVisibility` のプロパティを定義します。 これらのプロパティは、水平方向または垂直方向のスクロールバーを表示するタイミングを表す[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)列挙値を取得または設定します。 `ScrollBarVisibility` 列挙体を使って、次のメンバーを定義できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)は、プラットフォームの既定のスクロールバーの動作を示します。は、`HorizontalScrollBarVisibility` プロパティと `VerticalScrollBarVisibility` プロパティの既定値です。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)は、ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)は、ビューにコンテンツが収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="snap-points"></a>スナップポイント

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 この機能はスナップと呼ばれます。これは、スクロールが停止したときに項目が位置にスナップし、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスの次のプロパティによって制御されるためです。

- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)型の[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)は、スクロール時のスナップポイントの動作を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)型の[`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)は、スナップポイントを項目にどのように整列させるかを指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> スナップが発生すると、動きが最も少ない方向に発生します。

### <a name="snap-points-type"></a>スナップポイントの種類

[@No__t_1](xref:Xamarin.Forms.SnapPointsType)列挙体は、次のメンバーを定義します。

- `None` は、スクロールが項目にスナップされないことを示します。
- `Mandatory` は、コンテンツが常に最も近いスナップポイントにスナップポイントにスナップし、慣性の方向に沿ってスクロールが自然に停止することを示します。
- `MandatorySingle` は `Mandatory` と同じ動作を示しますが、一度に1つの項目だけをスクロールします。

既定では、 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)プロパティは `SnapPointsType.None` に設定されています。これにより、次のスクリーンショットに示すように、スクロールによって項目がスナップされません。

[![IOS と Android 上のスナップポイントのない CollectionView 縦の一覧のスクリーンショット](scrolling-images/snappoints-none.png "スナップポイントのない垂直方向の CollectionView リスト")](scrolling-images/snappoints-none-large.png#lightbox "スナップポイントのない垂直方向の CollectionView リスト")

### <a name="snap-points-alignment"></a>スナップポイントの配置

[@No__t_1](xref:Xamarin.Forms.SnapPointsAlignment)列挙体は、`Start`、`Center`、および `End` のメンバーを定義します。

> [!IMPORTANT]
> [@No__t_1](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティの値は、 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)プロパティが `Mandatory` または `MandatorySingle` に設定されている場合にのみ尊重されます。

#### <a name="start"></a>[開始]

@No__t_0 メンバーは、スナップポイントが項目の先頭端に合わせて整列されていることを示します。

既定では、 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティは `SnapPointsAlignment.Start` に設定されています。 ただし、完全を期すために、この列挙型のメンバーを設定する方法を次の XAML の例に示します。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

ユーザーがスクロールを開始しようとすると、最上位の項目がビューの上部にスワイプます。

[![スタートスナップポイントがある CollectionView 縦のリストのスクリーンショット (iOS と Android)](scrolling-images/snappoints-start.png "開始スナップポイントを含む CollectionView 縦の一覧")](scrolling-images/snappoints-start-large.png#lightbox "開始スナップポイントを含む CollectionView 縦の一覧")

#### <a name="center"></a>中央揃え

@No__t_0 メンバーは、スナップポイントが項目の中央に配置されていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

ユーザーがスクロールを開始しようとすると、最上位の項目は、ビューの上部に中央揃えで配置されます。スワイプ

[![IOS と Android の center スナップポイントがある CollectionView 縦の一覧のスクリーンショット](scrolling-images/snappoints-center.png "中心のスナップポイントを含む CollectionView 縦の一覧")](scrolling-images/snappoints-center-large.png#lightbox "中心のスナップポイントを含む CollectionView 縦の一覧")

#### <a name="end"></a>終了

@No__t_0 メンバーは、スナップポイントが項目の末尾の端に揃えられていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

ユーザーがスクロールを開始しようとすると、下の項目はビューの下部にスワイプます。

[![IOS と Android のエンドスナップポイントがある CollectionView 縦の一覧のスクリーンショット](scrolling-images/snappoints-end.png "終了スナップポイントを含む CollectionView 縦の一覧")](scrolling-images/snappoints-end-large.png#lightbox "終了スナップポイントを含む CollectionView 縦の一覧")

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
