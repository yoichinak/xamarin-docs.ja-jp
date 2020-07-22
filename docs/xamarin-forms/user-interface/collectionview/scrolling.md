---
title: Xamarin.FormsCollectionView スクロール
description: ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、CollectionView は2つの ScrollTo メソッドを定義して、プログラムによって項目をビューにスクロールします。
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 04d190971fa5ef16e08091600558f7f016bc8605
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84134538"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin.FormsCollectionView スクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)項目を [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ビューにスクロールする2つのメソッドを定義します。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。 どちらのオーバーロードにも、項目が属するグループを示すために指定できる追加の引数、スクロールが完了した後の項目の正確な位置、およびスクロールをアニメーション化するかどうかを指定できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)[`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)メソッドのいずれかが呼び出されたときに発生するイベントを定義 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) します。 [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs)イベントに付随するオブジェクトには、、、、などの `ScrollToRequested` 多くのプロパティがあり `IsAnimated` `Index` `Item` `ScrollToPosition` ます。 これらのプロパティは、メソッドの呼び出しで指定された引数によって設定され `ScrollTo` ます。

また、は、スクロールが発生したことを [`CollectionView`](xref:Xamarin.Forms.CollectionView) `Scrolled` 示すために発生するイベントを定義します。 `ItemsViewScrolledEventArgs`イベントに付随するオブジェクトに `Scrolled` は、多くのプロパティがあります。 詳細については、「[スクロールの検出](#detect-scrolling)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)`ItemsUpdatingScrollMode` `CollectionView` 新しい項目が追加されたときののスクロール動作を表すプロパティも定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](#control-scroll-position-when-new-items-are-added)」を参照してください。

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 スクロールが停止したときに項目が位置にスナップされるため、この機能はスナップと呼ばれています。 詳細については、「[スナップポイント](#snap-points)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ユーザーがスクロールするときに、データを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](populate-data.md#load-data-incrementally)」を参照してください。

## <a name="detect-scrolling"></a>スクロールの検出

[`CollectionView`](xref:Xamarin.Forms.CollectionView)スクロールが `Scrolled` 発生したことを示すために発生するイベントを定義します。 次の XAML の例は、 `CollectionView` イベントのイベントハンドラーを設定するを示してい `Scrolled` ます。

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

このコード例では、イベント `OnCollectionViewScrolled` の発生時にイベントハンドラーが実行され `Scrolled` ます。

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

この例では、イベントハンドラーは、 `OnCollectionViewScrolled` `ItemsViewScrolledEventArgs` イベントに付随するオブジェクトの値を出力します。

> [!IMPORTANT]
> イベントは、 `Scrolled` ユーザーが開始したスクロールと、プログラムによるスクロールのために発生します。

## <a name="scroll-an-item-at-an-index-into-view"></a>インデックスにある項目をスクロールして表示する

最初の [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) メソッドオーバーロードは、指定されたインデックス位置にある項目をビューにスクロールします。 [`CollectionView`](xref:Xamarin.Forms.CollectionView)という名前のオブジェクトがある `collectionView` 場合、次の例は、インデックス12の項目をビューにスクロールする方法を示しています。

```csharp
collectionView.ScrollTo(12);
```

また、アイテムとグループのインデックスを指定することで、グループ化されたデータのアイテムをスクロールして表示することもできます。 次の例は、2番目のグループの3番目の項目をスクロールして表示する方法を示しています。

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> イベントは、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) メソッドが呼び出されたときに発生し [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ます。

## <a name="scroll-an-item-into-view"></a>項目をスクロールして表示する

2番目の [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) メソッドオーバーロードは、指定された項目をビューにスクロールします。 [`CollectionView`](xref:Xamarin.Forms.CollectionView)という名前のオブジェクトがある `collectionView` 場合、次の例は、Proboscis のサル項目をスクロールして表示する方法を示しています。

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
> イベントは、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) メソッドが呼び出されたときに発生し [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ます。

## <a name="disable-scroll-animation"></a>スクロールアニメーションを無効にする

スクロールアニメーションは、項目をビューにスクロールしたときに表示されます。 ただし、このアニメーションは、 `animate` メソッドの引数 `ScrollTo` を次のように設定することによって無効にすることができます `false` 。

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>コントロールのスクロール位置

項目をビューにスクロールすると、スクロールが完了した後の項目の正確な位置は、メソッドの引数を使用して指定でき `position` [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ます。 この引数は、 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) 列挙型のメンバーを受け取ります。

### <a name="makevisible"></a>MakeVisible

メンバーは、 [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) アイテムがビューに表示されるまでスクロールする必要があることを示しています。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

このコード例では、項目をスクロールして表示するために必要な最小限のスクロールが実行されます。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-makevisible.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

> [!NOTE]
> [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)メンバーは、 `position` メソッドの呼び出し時に引数が指定されていない場合に、既定で使用され `ScrollTo` ます。

### <a name="start"></a>開始

メンバーは、 [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) 項目をビューの先頭までスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

次のコード例では、項目がビューの先頭にスクロールされます。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-start.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-start-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

### <a name="center"></a>Center

メンバーは、 [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) アイテムをビューの中央にスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

次のコード例では、アイテムがビューの中央にスクロールされます。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-center.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-center-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

### <a name="end"></a>End

メンバーは、 [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) 項目をビューの末尾までスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

次のコード例では、ビューの最後までスクロールされる項目になります。

[![IOS と Android でスクロールして項目が表示されている CollectionView の一覧のスクリーンショット](scrolling-images/scrolltoposition-end.png "スクロールした項目を含む CollectionView の一覧")](scrolling-images/scrolltoposition-end-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

## <a name="control-scroll-position-when-new-items-are-added"></a>新しい項目が追加されたときのスクロール位置の制御

[`CollectionView`](xref:Xamarin.Forms.CollectionView)バインド可能 `ItemsUpdatingScrollMode` なプロパティによってサポートされるプロパティを定義します。 このプロパティは `ItemsUpdatingScrollMode` 、 `CollectionView` 新しい項目が追加されたときののスクロール動作を表す列挙値を取得または設定します。 `ItemsUpdatingScrollMode` 列挙体を使って、次のメンバーを定義できます。

- `KeepItemsInView`新しい項目が追加されたときに表示される最初の項目を維持するために、スクロールのオフセットを調整します。
- `KeepScrollOffset`新しい項目が追加されたときに、リストの先頭を基準としたスクロールオフセットを維持します。
- `KeepLastItemInView`新しい項目が追加されたときに最後の項目が表示されるように、スクロールのオフセットを調整します。

プロパティの既定値 `ItemsUpdatingScrollMode` は `KeepItemsInView` です。 したがって、新しい項目がに追加されると、リスト内の最初に表示された [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目が表示されたままになります。 新しく追加された項目が常に一覧の一番下に表示されるようにするには、 `ItemsUpdatingScrollMode` プロパティを次のように設定する必要があり `KeepLastItemInView` ます。

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

[`CollectionView`](xref:Xamarin.Forms.CollectionView)`HorizontalScrollBarVisibility`バインド可能 `VerticalScrollBarVisibility` なプロパティによってサポートされるプロパティとプロパティを定義します。 これらのプロパティは、 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 水平方向または垂直方向のスクロールバーを表示するタイミングを表す列挙値を取得または設定します。 `ScrollBarVisibility` 列挙体を使って、次のメンバーを定義できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)プラットフォームの既定のスクロールバーの動作を示し `HorizontalScrollBarVisibility` ます。は、プロパティとプロパティの既定値です `VerticalScrollBarVisibility` 。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)コンテンツがビューに収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="snap-points"></a>スナップ位置

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 この機能はスナップと呼ばれます。これは、スクロールが停止したときに項目が位置にスナップし、クラスの次のプロパティによって制御されるためです [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 。

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)型のは、 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) スクロール時のスナップポイントの動作を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)(型) では、 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) スナップポイントを項目と整列する方法を指定します。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> スナップが発生すると、動きが最も少ない方向に発生します。

### <a name="snap-points-type"></a>スナップポイントの種類

[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)列挙体は、次のメンバーを定義します。

- `None`スクロールが項目にスナップされないことを示します。
- `Mandatory`コンテンツを常に最も近いスナップポイントにスナップし、慣性の方向に沿ってスクロールが自然に停止することを示します。
- `MandatorySingle`と同じ動作を示し `Mandatory` ますが、一度に1つの項目だけをスクロールします。

既定では、 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) プロパティはに設定されます `SnapPointsType.None` 。これにより、次のスクリーンショットに示すように、スクロールによって項目がスナップされません。

[![IOS と Android 上のスナップポイントのない CollectionView 縦の一覧のスクリーンショット](scrolling-images/snappoints-none.png "スナップポイントのない垂直方向の CollectionView リスト")](scrolling-images/snappoints-none-large.png#lightbox "スナップポイントのない垂直方向の CollectionView リスト")

### <a name="snap-points-alignment"></a>スナップポイントの配置

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)列挙体は `Start` 、、、およびの各メンバーを定義し `Center` `End` ます。

> [!IMPORTANT]
> プロパティの値は、 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) プロパティがまたはに設定されている場合にのみ尊重され `Mandatory` `MandatorySingle` ます。

#### <a name="start"></a>開始

メンバーは、 `SnapPointsAlignment.Start` スナップポイントが項目の先頭端に合わせて整列されていることを示します。

既定では、 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) プロパティはに設定されて `SnapPointsAlignment.Start` います。 ただし、完全を期すために、この列挙型のメンバーを設定する方法を次の XAML の例に示します。

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

#### <a name="center"></a>Center

メンバーは、 `SnapPointsAlignment.Center` スナップポイントが項目の中央に配置されていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

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

#### <a name="end"></a>End

メンバーは、 `SnapPointsAlignment.End` スナップポイントが項目の末尾の端に揃えられていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

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
