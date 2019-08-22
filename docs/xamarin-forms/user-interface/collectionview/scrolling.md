---
title: CollectionView のスクロール
description: ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、CollectionView は2つの ScrollTo メソッドを定義して、プログラムによって項目をビューにスクロールします。
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2019
ms.openlocfilehash: 303266f44664f7f57aeaf36869a3a06c8eb91870
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888649"
---
# <a name="xamarinforms-collectionview-scrolling"></a>CollectionView のスクロール

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)項目を[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)ビューにスクロールする2つのメソッドを定義します。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。 どちらのオーバーロードにも、スクロールの完了後に項目の正確な位置を示すために指定できる追加の引数と、スクロールをアニメーション化するかどうかを指定できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)メソッドのいずれかが呼び出されたときに発生するイベントを[`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)定義します。 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) `Item` `ScrollToPosition` `IsAnimated` `Index`イベントに`ScrollToRequested`付随する[オブジェクトには、、、、などの多くのプロパティがあります。`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) これらのプロパティは、 `ScrollTo`メソッドの呼び出しで指定された引数によって設定されます。

また、は[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、スクロール`Scrolled`が発生したことを示すために発生するイベントを定義します。 イベント`ItemsViewScrolledEventArgs` に`Scrolled`付随するオブジェクトには、多くのプロパティがあります。 詳細については、「[スクロールの検出](#detect-scrolling)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)新しい項目が`ItemsUpdatingScrollMode`追加され`CollectionView`たときののスクロール動作を表すプロパティも定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](#control-scroll-position-when-new-items-are-added)」を参照してください。

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 スクロールが停止したときに項目が位置にスナップされるため、この機能はスナップと呼ばれています。 詳細については、「[スナップポイント](#snap-points)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ユーザーがスクロールするときに、データを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](populate-data.md#load-data-incrementally)」を参照してください。

## <a name="detect-scrolling"></a>スクロールの検出

[`CollectionView`](xref:Xamarin.Forms.CollectionView)スクロールが`Scrolled`発生したことを示すために発生するイベントを定義します。 次の XAML の例は`CollectionView` 、 `Scrolled`イベントのイベントハンドラーを設定するを示しています。

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

このコード例`OnCollectionViewScrolled`では、イベントの`Scrolled`発生時にイベントハンドラーが実行されます。

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

イベント`OnCollectionViewScrolled`ハンドラーは、イベントに付随する`ItemsViewScrolledEventArgs`オブジェクトの値を出力します。

> [!IMPORTANT]
> イベント`Scrolled`は、ユーザーが開始したスクロールと、プログラムによるスクロールのために発生します。

## <a name="scroll-an-item-at-an-index-into-view"></a>インデックスにある項目をスクロールして表示する

最初[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)のメソッドオーバーロードは、指定されたインデックス位置にある項目をビューにスクロールします。 という[`CollectionView`](xref:Xamarin.Forms.CollectionView)名前`collectionView`のオブジェクトがある場合、次の例は、インデックス12の項目をビューにスクロールする方法を示しています。

```csharp
collectionView.ScrollTo(12);
```

> [!NOTE]
> イベントは、 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されたときに発生します。 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)

## <a name="scroll-an-item-into-view"></a>項目をスクロールして表示する

2番[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)目のメソッドオーバーロードは、指定された項目をビューにスクロールします。 という[`CollectionView`](xref:Xamarin.Forms.CollectionView)名前`collectionView`のオブジェクトがある場合、次の例では、指定された項目をスクロールして表示する方法を示しています。

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

> [!NOTE]
> イベントは、 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されたときに発生します。 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)

## <a name="scroll-bar-visibility"></a>スクロールバーの表示

[`CollectionView`](xref:Xamarin.Forms.CollectionView)バインド`HorizontalScrollBarVisibility`可能`VerticalScrollBarVisibility`なプロパティによってサポートされるプロパティとプロパティを定義します。 これらのプロパティは、水平[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)方向または垂直方向のスクロールバーを表示するタイミングを表す列挙値を取得または設定します。 `ScrollBarVisibility` 列挙体を使って、次のメンバーを定義できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)プラットフォームの既定のスクロールバーの動作を示します。は、プロパティ`HorizontalScrollBarVisibility`と`VerticalScrollBarVisibility`プロパティの既定値です。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)コンテンツがビューに収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="control-scroll-position"></a>コントロールのスクロール位置

項目をビューにスクロールすると、スクロールが完了した後の項目の正確な位置は、 `position` [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドの引数を使用して指定できます。 この引数は、 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition)列挙型のメンバーを受け取ります。

### <a name="makevisible"></a>MakeVisible

メンバー [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)は、アイテムがビューに表示されるまでスクロールする必要があることを示しています。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

このコード例では、項目をスクロールして表示するために必要な最小限のスクロールが実行されます。

スクロールされた[(scrolling-images/scrolltoposition-makevisible.png "項目が")表示されて![いる、iOS および Android の CollectionView 縦の一覧の項目がスクロールされた CollectionView 縦の一覧のスクリーンショット]](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

> [!NOTE]
> メンバーは、メソッドの`ScrollTo`呼び出し時に`position`引数が指定されていない場合に、既定で使用されます。 [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)

### <a name="start"></a>[開始]

メンバー [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition)は、項目をビューの先頭までスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

次のコード例では、項目がビューの先頭にスクロールされます。

スクロールされた[(scrolling-images/scrolltoposition-start.png "項目が")表示されて![いる、iOS および Android の CollectionView 縦の一覧の項目がスクロールされた CollectionView 縦の一覧のスクリーンショット]](scrolling-images/scrolltoposition-start-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

### <a name="center"></a>中央揃え

メンバー [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition)は、アイテムをビューの中央にスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

次のコード例では、アイテムがビューの中央にスクロールされます。

スクロールされた[(scrolling-images/scrolltoposition-center.png "項目が")表示されて![いる、iOS および Android の CollectionView 縦の一覧の項目がスクロールされた CollectionView 縦の一覧のスクリーンショット]](scrolling-images/scrolltoposition-center-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

### <a name="end"></a>終了

メンバー [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition)は、項目をビューの末尾までスクロールする必要があることを示します。

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

次のコード例では、ビューの最後までスクロールされる項目になります。

スクロールされた[(scrolling-images/scrolltoposition-end.png "項目が")表示されて![いる、iOS および Android の CollectionView 縦の一覧の項目がスクロールされた CollectionView 縦の一覧のスクリーンショット]](scrolling-images/scrolltoposition-end-large.png#lightbox "スクロールした項目を含む CollectionView の一覧")

## <a name="disable-scroll-animation"></a>スクロールアニメーションを無効にする

スクロールアニメーションは、項目をビューにスクロールしたときに表示されます。 ただし、このアニメーションは、 `animate` `ScrollTo`メソッドの引数を次のように設定`false`することによって無効にすることができます。

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position-when-new-items-are-added"></a>新しい項目が追加されたときのスクロール位置の制御

[`CollectionView`](xref:Xamarin.Forms.CollectionView)バインド可能なプロパティによってサポートされるプロパティを定義します。`ItemsUpdatingScrollMode` このプロパティは、 `ItemsUpdatingScrollMode` `CollectionView`新しい項目が追加されたときののスクロール動作を表す列挙値を取得または設定します。 `ItemsUpdatingScrollMode` 列挙体を使って、次のメンバーを定義できます。

- `KeepItemsInView`新しい項目が追加されたときに表示される最初の項目を維持するために、スクロールのオフセットを調整します。
- `KeepScrollOffset`新しい項目が追加されたときに、リストの先頭を基準としたスクロールオフセットを維持します。
- `KeepLastItemInView`新しい項目が追加されたときに最後の項目が表示されるように、スクロールのオフセットを調整します。

`ItemsUpdatingScrollMode`プロパティの既定値は`KeepItemsInView`です。 したがって、新しい項目がに[`CollectionView`](xref:Xamarin.Forms.CollectionView)追加されると、リスト内の最初に表示された項目が表示されたままになります。 新しく追加された項目が常に一覧`ItemsUpdatingScrollMode`の一番下に表示されるようにするには、プロパティを次のように`KeepLastItemInView`設定する必要があります。

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="snap-points"></a>スナップポイント

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 この機能はスナップと呼ばれます。これは、スクロールが停止したときに項目が位置にスナップし[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 、クラスの次のプロパティによって制御されるためです。

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)型[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)のは、スクロール時のスナップポイントの動作を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)(型[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)) では、スナップポイントを項目と整列する方法を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

> [!NOTE]
> スナップが発生すると、動きが最も少ない方向に発生します。

### <a name="snap-points-type"></a>スナップポイントの種類

列挙[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)体は、次のメンバーを定義します。

- `None`スクロールが項目にスナップされないことを示します。
- `Mandatory`コンテンツを常に最も近いスナップポイントにスナップし、慣性の方向に沿ってスクロールが自然に停止することを示します。
- `MandatorySingle`と`Mandatory`同じ動作を示しますが、一度に1つの項目だけをスクロールします。

既定[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)では、プロパティはに`SnapPointsType.None`設定されます。これにより、次のスクリーンショットに示すように、スクロールによって項目がスナップされません。

スナップポイント[![のない CollectionView 縦の一覧のスクリーンショット、IOS と Android の](scrolling-images/snappoints-none.png "CollectionView 縦の一覧 (スナップポイントなし"))](scrolling-images/snappoints-none-large.png#lightbox "スナップポイントのない垂直方向の CollectionView リスト")

### <a name="snap-points-alignment"></a>スナップポイントの配置

列挙[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)体は、 `Center`、、 `End`およびの各メンバーを定義`Start`します。

> [!IMPORTANT]
> プロパティの[`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)値は、プロパティがまたは`MandatorySingle`に[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) `Mandatory`設定されている場合にのみ尊重されます。

#### <a name="start"></a>開始

メンバー `SnapPointsAlignment.Start`は、スナップポイントが項目の先頭端に合わせて整列されていることを示します。

既定では、 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティはに`SnapPointsAlignment.Start`設定されています。 ただし、完全を期すために、この列挙型のメンバーを設定する方法を次の XAML の例に示します。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Start">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

ユーザーがスクロールを開始しようとすると、最上位の項目がビューの上部にスワイプます。

スタート[![スナップポイントがある CollectionView 縦の一覧と、](scrolling-images/snappoints-start.png "スタートスナップポイントを使用")した iOS および Android の CollectionView 縦の一覧のスクリーンショット](scrolling-images/snappoints-start-large.png#lightbox "開始スナップポイントを含む CollectionView 縦の一覧")

#### <a name="center"></a>中央揃え

メンバー `SnapPointsAlignment.Center`は、スナップポイントが項目の中央に配置されていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Center">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

ユーザーがスクロールを開始しようとすると、最上位の項目は、ビューの上部に中央揃えで配置されます。スワイプ

中央のスナップポイントがある[ ![CollectionView 縦向きのリストのスクリーンショット (](scrolling-images/snappoints-center.png "中央のスナップポイントがある")iOS と Android の CollectionView 縦の一覧)](scrolling-images/snappoints-center-large.png#lightbox "中心のスナップポイントを含む CollectionView 縦の一覧")

#### <a name="end"></a>終了

メンバー `SnapPointsAlignment.End`は、スナップポイントが項目の末尾の端に揃えられていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="End">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

ユーザーがスクロールを開始しようとすると、下の項目はビューの下部にスワイプます。

終了スナップポイントがある[ ![CollectionView 縦のリストのスクリーンショット、IOS と Android の](scrolling-images/snappoints-end.png "CollectionView 縦の一覧と終了スナップポイント")](scrolling-images/snappoints-end-large.png#lightbox "終了スナップポイントを含む CollectionView 縦の一覧")

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
