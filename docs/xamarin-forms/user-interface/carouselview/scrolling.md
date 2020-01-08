---
title: CarouselView のスクロール
description: ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 さらに、CarouselView は2つの ScrollTo メソッドを定義して、プログラムによって項目をビューにスクロールします。
ms.prod: xamarin
ms.assetid: 92D7B618-07FA-4343-9D0F-212525E92C39
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: ce0e0b63206ab918b5d761be3e619370aec1eec7
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489753"
---
# <a name="xamarinforms-carouselview-scrolling"></a>CarouselView のスクロール

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、次のスクロール関連のプロパティを定義します。

- 水平スクロールバーを表示するタイミングを指定する `ScrollBarVisibility`型の `HorizontalScrollBarVisibility`。
- `bool`型の `IsDragging`。 `CarouselView` がスクロール中かどうかを示します。 これは読み取り専用プロパティで、既定値は `false`です。
- `bool`型の `IsScrollAnimated`。 `CarouselView`をスクロールするときにアニメーションを実行するかどうかを指定します。 既定値は `true`です。
- `ItemsUpdatingScrollMode`型の `ItemsUpdatingScrollMode`。新しい項目が追加されたときの `CarouselView` のスクロール動作を表します。
- 垂直スクロールバーを表示するタイミングを指定する `ScrollBarVisibility`型の `VerticalScrollBarVisibility`。

これらのプロパティはすべて[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットにすることができることを意味します。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)では、項目をスクロールして表示する2つの[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドも定義されています。 オーバーロードの 1 つは、ビュー内の指定したインデックスにある項目にスクロールし、もう一つは、ビュー内の指定した項目にスクロールします。 どちらのオーバーロードにも、スクロールの完了後に項目の正確な位置を示すために指定できる追加の引数と、スクロールをアニメーション化するかどうかを指定できます。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、いずれかの[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されたときに発生する[`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)イベントを定義します。 `ScrollToRequested` イベントに付随する[`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs)オブジェクトには、`IsAnimated`、`Index`、`Item`、`ScrollToPosition`を含む多くのプロパティがあります。 これらのプロパティは、`ScrollTo` メソッドの呼び出しで指定された引数によって設定されます。

さらに、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)は、スクロールが発生したことを示すために発生する `Scrolled` イベントを定義します。 `Scrolled` イベントに付随する `ItemsViewScrolledEventArgs` オブジェクトには、多くのプロパティがあります。 詳細については、「[スクロールの検出](#detect-scrolling)」を参照してください。

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 スクロールが停止したときに項目が位置にスナップされるため、この機能はスナップと呼ばれています。 詳細については、「[スナップポイント](#snap-points)」を参照してください。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、ユーザーがスクロールするときにデータを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](populate-data.md#load-data-incrementally)」を参照してください。

## <a name="detect-scrolling"></a>スクロールの検出

`IsDragging` プロパティを調べて、現在[`CarouselView`](xref:Xamarin.Forms.CarouselView)が項目をスクロールしているかどうかを判断できます。

さらに、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)は、スクロールが発生したことを示すために発生する `Scrolled` イベントを定義します。 このイベントは、スクロールに関するデータが必要な場合に使用します。

次の XAML の例は、`Scrolled` イベントのイベントハンドラーを設定する `CarouselView` を示しています。

```xaml
<CarouselView Scrolled="OnCollectionViewScrolled">
    ...
</CarouselView>
```

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView();
carouselView.Scrolled += OnCarouselViewScrolled;
```

このコード例では、`Scrolled` イベントが発生すると、`OnCarouselViewScrolled` イベントハンドラーが実行されます。

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

この例では、`OnCarouselViewScrolled` イベントハンドラーによって、イベントに付随する `ItemsViewScrolledEventArgs` オブジェクトの値が出力されます。

> [!IMPORTANT]
> `Scrolled` イベントは、ユーザーが開始したスクロール、およびプログラムによるスクロールのために発生します。

## <a name="scroll-an-item-at-an-index-into-view"></a>インデックスにある項目をスクロールして表示する

最初の[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドオーバーロードは、指定されたインデックス位置にある項目をビューにスクロールします。 `carouselView`という名前の[`CarouselView`](xref:Xamarin.Forms.CarouselView)オブジェクトを指定した場合、次の例では、インデックス6の項目をビューにスクロールする方法を示しています。

```csharp
carouselView.ScrollTo(6);
```

> [!NOTE]
> [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されると、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)イベントが発生します。

## <a name="scroll-an-item-into-view"></a>項目をスクロールして表示する

2番目の[`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドオーバーロードは、指定された項目をビューにスクロールします。 `carouselView`という名前の[`CarouselView`](xref:Xamarin.Forms.CarouselView)オブジェクトがある場合、次の例は、Proboscis のサル項目をスクロールして表示する方法を示しています。

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
carouselView.ScrollTo(monkey);
```

> [!NOTE]
> [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドが呼び出されると、 [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)イベントが発生します。

## <a name="disable-scroll-animation"></a>スクロールアニメーションを無効にする

[`CarouselView`](xref:Xamarin.Forms.CarouselView)内の項目間を移動すると、スクロールアニメーションが表示されます。 このアニメーションは、ユーザーが開始したスクロールとプログラムによるスクロールの両方で発生します。 `IsScrollAnimated` プロパティを `false` に設定すると、両方のスクロールカテゴリのアニメーションが無効になります。

または、`ScrollTo` メソッドの `animate` 引数を `false` に設定して、プログラムによるスクロールでのスクロールアニメーションを無効にすることもできます。

```csharp
carouselView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>コントロールのスクロール位置

項目をビューにスクロールすると、スクロールが完了した後の項目の正確な位置を、 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*)メソッドの `position` 引数を使用して指定できます。 この引数は、 [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition)列挙型のメンバーを受け取ります。

### <a name="makevisible"></a>MakeVisible

[`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)メンバーは、ビューに表示されるまで項目をスクロールする必要があることを示します。

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

このコード例では、項目をスクロールして表示するために必要な最小限のスクロールが実行されます。

> [!NOTE]
> `ScrollTo` メソッドを呼び出すときに `position` 引数が指定されていない場合、既定では[`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)メンバーが使用されます。

### <a name="start"></a>Start

[`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition)メンバーは、項目をビューの先頭までスクロールする必要があることを示します。

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

このコード例では、項目がビューの先頭にスクロールされます。

### <a name="center"></a>中央揃え

[`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition)メンバーは、アイテムをビューの中央にスクロールする必要があることを示します。

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

このコード例では、アイテムがビューの中央にスクロールされます。

### <a name="end"></a>終了

[`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition)メンバーは、項目をビューの最後までスクロールする必要があることを示します。

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.End);
```

このコード例では、ビューの最後までスクロールされる項目になります。

## <a name="control-scroll-position-when-new-items-are-added"></a>新しい項目が追加されたときのスクロール位置の制御

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、バインド可能なプロパティによってサポートされる `ItemsUpdatingScrollMode` プロパティを定義します。 このプロパティは、新しい項目が追加されたときの `CarouselView` のスクロール動作を表す `ItemsUpdatingScrollMode` 列挙値を取得または設定します。 `ItemsUpdatingScrollMode` 列挙体を使って、次のメンバーを定義できます。

- `KeepItemsInView` は、新しい項目が追加されたときに表示される最初の項目を維持するために、スクロールのオフセットを調整します。
- `KeepScrollOffset` は、新しい項目が追加されたときに、リストの先頭を基準としたスクロールオフセットを維持します。
- `KeepLastItemInView` は、新しい項目が追加されたときに最後の項目が表示されるように、スクロールのオフセットを調整します。

`ItemsUpdatingScrollMode` プロパティの既定値は `KeepItemsInView`です。 したがって、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)に新しい項目が追加されると、一覧に表示されている最初の項目が表示されたままになります。 新しく追加された項目が一覧の下部に常に表示されるようにするには、`ItemsUpdatingScrollMode` プロパティを `KeepLastItemInView`に設定する必要があります。

```xaml
<CarouselView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CarouselView>
```

該当の C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>スクロールバーの表示

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、バインド可能なプロパティによってサポートされる `HorizontalScrollBarVisibility` と `VerticalScrollBarVisibility` のプロパティを定義します。 これらのプロパティは、水平方向または垂直方向のスクロールバーを表示するタイミングを表す[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)列挙値を取得または設定します。 `ScrollBarVisibility` 列挙体を使って、次のメンバーを定義できます。

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)は、プラットフォームの既定のスクロールバーの動作を示します。は、`HorizontalScrollBarVisibility` プロパティと `VerticalScrollBarVisibility` プロパティの既定値です。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)は、ビューにコンテンツが収まる場合でも、スクロールバーが表示されることを示します。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)は、ビューにコンテンツが収まらない場合でも、スクロールバーが表示されないことを示します。

## <a name="snap-points"></a>スナップ位置

ユーザーが、スクロールを開始するためにスワイプした場合に、項目が完全に表示されるように、スクロールの終了位置を制御することができます。 この機能はスナップと呼ばれます。これは、スクロールが停止したときに項目が位置にスナップし、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスの次のプロパティによって制御されるためです。

- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)型の[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)は、スクロール時のスナップポイントの動作を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)型の[`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)は、スナップポイントを項目にどのように整列させるかを指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトによりサポートされます。つまりデータバインディングの対象となる可能性があるという意味です。

> [!NOTE]
> スナップが発生すると、動きが最も少ない方向に発生します。

### <a name="snap-points-type"></a>スナップポイントの種類

[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)列挙体は、次のメンバーを定義します。

- `None` は、スクロールが項目にスナップされないことを示します。
- `Mandatory` は、コンテンツが常に最も近いスナップポイントにスナップポイントにスナップし、慣性の方向に沿ってスクロールが自然に停止することを示します。
- `MandatorySingle` は `Mandatory`と同じ動作を示しますが、一度に1つの項目だけをスクロールします。

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)の[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)プロパティは `SnapPointsType.MandatorySingle`に設定されます。これにより、スクロールによって一度に1つの項目だけがスクロールされるようになります。

### <a name="snap-points-alignment"></a>スナップポイントの配置

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)列挙体は、`Start`、`Center`、および `End` のメンバーを定義します。

> [!IMPORTANT]
> [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティの値は、 [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)プロパティが `Mandatory` または `MandatorySingle` に設定されている場合にのみ尊重されます。

#### <a name="start"></a>Start

`SnapPointsAlignment.Start` メンバーは、スナップポイントが項目の先頭端に合わせて整列されていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

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

該当の C# コードを次に示します。

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

ユーザーが水平スクロール[`CarouselView`](xref:Xamarin.Forms.CarouselView)でスクロールを開始しようとすると、左側の項目がビューの左側に配置されます。

#### <a name="center"></a>中央揃え

`SnapPointsAlignment.Center` メンバーは、スナップポイントが項目の中央に配置されていることを示します。

既定では、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)の[`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)プロパティは `Center`に設定されます。 ただし、完全を期すために、この列挙型のメンバーを設定する方法を次の XAML の例に示します。

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

該当の C# コードを次に示します。

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

ユーザーが水平スクロール[`CarouselView`](xref:Xamarin.Forms.CarouselView)でスクロールを開始しようとすると、中央の項目はビューの中央に揃えられます。

#### <a name="end"></a>終了

`SnapPointsAlignment.End` メンバーは、スナップポイントが項目の末尾の端に揃えられていることを示します。 次の XAML の例は、この列挙型のメンバーを設定する方法を示しています。

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

該当の C# コードを次に示します。

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

ユーザーが水平スクロール[`CarouselView`](xref:Xamarin.Forms.CarouselView)でスクロールを開始しようとすると、右側の項目がビューの右側に配置されます。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
