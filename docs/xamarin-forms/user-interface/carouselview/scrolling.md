---
title: Xamarin.FormsCarouselView スクロール
description: ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、CarouselView は2つの ScrollTo メソッドを定義して、プログラムによって項目をビューにスクロールします。
ms.prod: xamarin
ms.assetid: 92D7B618-07FA-4343-9D0F-212525E92C39
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cf5d3924a726247ea1884acc75720566d76c76e4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929217"
---
# <a name="xamarinforms-carouselview-scrolling"></a>Xamarin.FormsCarouselView スクロール

![プレリリース API](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)スクロールに関連する次のプロパティを定義します。

- `HorizontalScrollBarVisibility``ScrollBarVisibility`水平スクロールバーを表示するタイミングを指定する型の。
- `IsDragging``bool`がスクロールするかどうかを示す型の `CarouselView` 。 これは読み取り専用プロパティで、既定値は `false` です。
- `IsScrollAnimated`を `bool` スクロールするときにアニメーションを実行するかどうかを指定する型の `CarouselView` 。 既定値は `true` です。
- `ItemsUpdatingScrollMode`型の `ItemsUpdatingScrollMode` `CarouselView` 。これは、新しい項目が追加されたときののスクロール動作を表します。
- `VerticalScrollBarVisibility`型の `ScrollBarVisibility` 。垂直スクロールバーを表示するタイミングを指定します。

これらのプロパティはすべて、オブジェクトによってバックアップされ [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、データバインディングのターゲットにすることができることを意味します。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)では、項目をスクロールして表示する2つのメソッドも定義されています。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。 どちらのオーバーロードにも、スクロールの完了後に項目の正確な位置を示すために指定できる追加の引数と、スクロールをアニメーション化するかどうかを指定できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)[`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)メソッドのいずれかが呼び出されたときに発生するイベントを定義 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) します。 [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs)イベントに付随するオブジェクトには、、、、などの `ScrollToRequested` 多くのプロパティがあり `IsAnimated` `Index` `Item` `ScrollToPosition` ます。 これらのプロパティは、メソッドの呼び出しで指定された引数によって設定され `ScrollTo` ます。

また、は、スクロールが発生したことを [`CarouselView`](xref:Xamarin.Forms.CarouselView) `Scrolled` 示すために発生するイベントを定義します。 `ItemsViewScrolledEventArgs`イベントに付随するオブジェクトに `Scrolled` は、多くのプロパティがあります。 詳細については、「[スクロールの検出](#detect-scrolling)」を参照してください。

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 スクロールが停止したときに項目が位置にスナップされるため、この機能はスナップと呼ばれています。 詳細については、「[スナップポイント](#snap-points)」を参照してください。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)ユーザーがスクロールするときに、データを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](populate-data.md#load-data-incrementally)」を参照してください。

## <a name="detect-scrolling"></a>スクロールの検出

`IsDragging`プロパティを調べて、が現在項目をスクロールしているかどうかを確認でき [`CarouselView`](xref:Xamarin.Forms.CarouselView) ます。

また、は、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) `Scrolled` スクロールが発生したことを示すために発生するイベントを定義します。 このイベントは、スクロールに関するデータが必要な場合に使用します。

次の XAML の例は、 `CarouselView` イベントのイベントハンドラーを設定するを示してい `Scrolled` ます。

```xaml
<CarouselView Scrolled="OnCollectionViewScrolled">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.Scrolled += OnCarouselViewScrolled;
```

このコード例では、イベント `OnCarouselViewScrolled` の発生時にイベントハンドラーが実行され `Scrolled` ます。

```csharp
void OnCarouselViewScrolled(object sender, ItemsViewScrolledEventArgs e)
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

この例では、イベントハンドラーは、 `OnCarouselViewScrolled` `ItemsViewScrolledEventArgs` イベントに付随するオブジェクトの値を出力します。

> [!IMPORTANT]
> イベントは、 `Scrolled` ユーザーが開始したスクロールと、プログラムによるスクロールのために発生します。

## <a name="scroll-an-item-at-an-index-into-view"></a>インデックスにある項目をスクロールして表示する

最初の [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) メソッドオーバーロードは、指定されたインデックス位置にある項目をビューにスクロールします。 [`CarouselView`](xref:Xamarin.Forms.CarouselView)という名前のオブジェクトがある `carouselView` 場合、次の例は、インデックス6の項目をビューにスクロールする方法を示しています。

```csharp
carouselView.ScrollTo(6);
```

> [!NOTE]
> イベントは、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) メソッドが呼び出されたときに発生し [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ます。

## <a name="scroll-an-item-into-view"></a>項目をスクロールして表示する

2番目の [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) メソッドオーバーロードは、指定された項目をビューにスクロールします。 [`CarouselView`](xref:Xamarin.Forms.CarouselView)という名前のオブジェクトがある `carouselView` 場合、次の例は、Proboscis のサル項目をスクロールして表示する方法を示しています。

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
carouselView.ScrollTo(monkey);
```

> [!NOTE]
> イベントは、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) メソッドが呼び出されたときに発生し [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ます。

## <a name="disable-scroll-animation"></a>スクロールアニメーションを無効にする

内の項目間を移動すると、スクロールアニメーションが表示され [`CarouselView`](xref:Xamarin.Forms.CarouselView) ます。 このアニメーションは、ユーザーが開始したスクロールとプログラムによるスクロールの両方で発生します。 `IsScrollAnimated`プロパティをに設定する `false` と、両方のスクロールカテゴリのアニメーションが無効になります。

または、 `animate` メソッドの引数 `ScrollTo` をに設定して、 `false` プログラムによるスクロールでのスクロールアニメーションを無効にすることもできます。

```csharp
carouselView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>コントロールのスクロール位置

項目をビューにスクロールすると、スクロールが完了した後の項目の正確な位置は、メソッドの引数を使用して指定でき `position` [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) ます。 この引数は、 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) 列挙型のメンバーを受け取ります。

### <a name="makevisible"></a>MakeVisible

メンバーは、 [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) アイテムがビューに表示されるまでスクロールする必要があることを示しています。

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

このコード例では、項目をスクロールして表示するために必要な最小限のスクロールが実行されます。

> [!NOTE]
> [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)メンバーは、 `position` メソッドの呼び出し時に引数が指定されていない場合に、既定で使用され `ScrollTo` ます。

### <a name="start"></a>[開始]

メンバーは、 [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) 項目をビューの先頭までスクロールする必要があることを示します。

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

このコード例では、項目がビューの先頭にスクロールされます。

### <a name="center"></a>Center

メンバーは、 [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) アイテムをビューの中央にスクロールする必要があることを示します。

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

このコード例では、アイテムがビューの中央にスクロールされます。

### <a name="end"></a>End

メンバーは、 [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) 項目をビューの末尾までスクロールする必要があることを示します。

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.End);
```

このコード例では、ビューの最後までスクロールされる項目になります。

## <a name="control-scroll-position-when-new-items-are-added"></a>新しい項目が追加されたときのスクロール位置の制御

[`CarouselView`](xref:Xamarin.Forms.CarouselView)バインド可能 `ItemsUpdatingScrollMode` なプロパティによってサポートされるプロパティを定義します。 このプロパティは `ItemsUpdatingScrollMode` 、 `CarouselView` 新しい項目が追加されたときののスクロール動作を表す列挙値を取得または設定します。 `ItemsUpdatingScrollMode` 列挙体を使って、次のメンバーを定義できます。

- `KeepItemsInView`新しい項目が追加されたときに表示される最初の項目を維持するために、スクロールのオフセットを調整します。
- `KeepScrollOffset`新しい項目が追加されたときに、リストの先頭を基準としたスクロールオフセットを維持します。
- `KeepLastItemInView`新しい項目が追加されたときに最後の項目が表示されるように、スクロールのオフセットを調整します。

プロパティの既定値 `ItemsUpdatingScrollMode` は `KeepItemsInView` です。 したがって、新しい項目がに追加されると、リスト内の最初に表示された [`CarouselView`](xref:Xamarin.Forms.CarouselView) 項目が表示されたままになります。 新しく追加された項目が常に一覧の一番下に表示されるようにするには、 `ItemsUpdatingScrollMode` プロパティを次のように設定する必要があり `KeepLastItemInView` ます。

```xaml
<CarouselView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>スクロールバーの表示

[`CarouselView`](xref:Xamarin.Forms.CarouselView)`HorizontalScrollBarVisibility`バインド可能 `VerticalScrollBarVisibility` なプロパティによってサポートされるプロパティとプロパティを定義します。 これらのプロパティは、 [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) 水平方向または垂直方向のスクロールバーを表示するタイミングを表す列挙値を取得または設定します。 `ScrollBarVisibility` 列挙体を使って、次のメンバーを定義できます。

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

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) プロパティはに設定されます `SnapPointsType.MandatorySingle` 。これにより、スクロールによって一度に1つの項目だけがスクロールされるようになります。

次のスクリーンショットは、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) スナップがオフになっているを示しています。

[![IOS と Android のスナップポイントのない CarouselView のスクリーンショット](scrolling-images/snappoints-none.png "スナップポイントのない CarouselView")](scrolling-images/snappoints-none-large.png#lightbox "スナップポイントのない CarouselView")

### <a name="snap-points-alignment"></a>スナップポイントの配置

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)列挙体は `Start` 、、、およびの各メンバーを定義し `Center` `End` ます。

> [!IMPORTANT]
> プロパティの値は、 [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) プロパティがまたはに設定されている場合にのみ尊重され `Mandatory` `MandatorySingle` ます。

#### <a name="start"></a>[開始]

メンバーは、 `SnapPointsAlignment.Start` スナップポイントが項目の先頭端に合わせて整列されていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

ユーザーが水平スクロールでスクロールを開始しようとすると [`CarouselView`](xref:Xamarin.Forms.CarouselView) 、左側の項目はビューの左側にスワイプます。

[![スタートスナップポイントがある CarouselView のスクリーンショット (iOS と Android)](scrolling-images/snappoints-start.png "スナップポイントを開始する CarouselView")](scrolling-images/snappoints-start-large.png#lightbox "スナップポイントを開始する CarouselView")

#### <a name="center"></a>Center

メンバーは、 `SnapPointsAlignment.Center` スナップポイントが項目の中央に配置されていることを示します。

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) プロパティはに設定されて `Center` います。 ただし、完全を期すために、この列挙型のメンバーを設定する方法を次の XAML の例に示します。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

ユーザーが水平スクロールでスクロールを開始しようとすると [`CarouselView`](xref:Xamarin.Forms.CarouselView) 、中央の項目はビューの中央に揃えられます。スワイプは次のようになります。

[![IOS と Android の center スナップポイントを含む CarouselView のスクリーンショット](scrolling-images/snappoints-center.png "Center スナップポイントを使用した CarouselView")](scrolling-images/snappoints-center-large.png#lightbox "Center スナップポイントを使用した CarouselView")

#### <a name="end"></a>End

メンバーは、 `SnapPointsAlignment.End` スナップポイントが項目の末尾の端に揃えられていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

ユーザーが水平スクロールでスクロールを開始しようとすると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 右側の項目がビューの右側にスワイプます。

[![IOS と Android のエンドスナップポイントがある CarouselView のスクリーンショット](scrolling-images/snappoints-end.png "CarouselView と終了スナップポイント")](scrolling-images/snappoints-end-large.png#lightbox "CarouselView と終了スナップポイント")

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
