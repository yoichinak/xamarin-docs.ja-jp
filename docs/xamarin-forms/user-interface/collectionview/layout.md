---
title: Xamarin.Forms CollectionView レイアウト
description: 既定では、CollectionView は垂直方向のリストでその項目を表示しますが、 垂直および水平方向のリストとグリッドを指定することができます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2019
ms.openlocfilehash: 786cea04718022847bba2ecffed8f377dd49bd8b
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512818"
---
# <a name="xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView レイアウト

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) レイアウトを制御する次のプロパティを定義します。

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)、型の[ `IItemsLayout` ](xref:Xamarin.Forms.IItemsLayout)、使用するレイアウトを指定します。
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)、型の[ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy)、使用する項目のメジャーの戦略を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

既定で、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)を垂直方向に一覧の項目が表示されます。 以下のいずれかのレイアウトを使用することができます。

- 垂直方向リスト – 新しい項目が追加されるにつれ、垂直方向に拡大する 1 つの列のリスト。
- 水平方向リスト – 新しい項目が追加されるにつれ、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 新しい項目が追加されるにつれ、垂直方向に拡大する複数列のグリッド。
- 水平グリッド – 新しい項目が追加されるにつれ、水平方向に拡大する複数行のグリッド。

これらのレイアウトを設定して指定できます、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)から派生したクラスにプロパティ、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout)クラス。 以下のようなプロパティを定義します。

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)、型の[ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation)、方向を指定、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)項目を追加するように展開します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)、型の[ `SnapPointsAlignment` ](xref:Xamarin.Forms.SnapPointsAlignment)、項目を含むのスナップ ポイントを配置する方法を指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)、型の[ `SnapPointsType` ](xref:Xamarin.Forms.SnapPointsType)、スクロールするとき、スナップ ポイントの動作を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。 スナップ ポイントの詳細については、次を参照してください。[のスナップ ポイント](scrolling.md#snap-points)で、 [Xamarin.Forms CollectionView スクロール](scrolling.md)ガイド。

[ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙体は、次のメンバーを定義します。

- `Vertical` 示します、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)項目が追加されると、垂直方向に展開されます。
- `Horizontal` 示します、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)項目が追加されると水平方向に展開されます。

[ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout)クラスから継承、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout)クラスを定義しますを`ItemSpacing`型のプロパティ、 `double`、各アイテムの周囲の空白部分を表します。 このプロパティの既定値は 0、およびその値が 0 以上にある常に必要があります。 `ListItemsLayout`クラスには静的な定義も`Vertical`と`Horizontal`メンバー。 これらのメンバーを使って、垂直または水平方向のリストをそれぞれ作成することができます。 または、`ListItemsLayout`を指定するオブジェクトを作成できる、 [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation)を引数として列挙型メンバー。

[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout)クラスから継承、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsLayout)クラスし、次のプロパティを定義します。

- `VerticalItemSpacing`、型の`double`、各アイテムの周囲の垂直方向の空白を表します。 このプロパティの既定値は 0、およびその値が 0 以上にある常に必要があります。
- `HorizontalItemSpacing`、型の`double`、各項目の水平方向の空白を表します。 このプロパティの既定値は 0、およびその値が 0 以上にある常に必要があります。
- `Span`、型の`int`、グリッドに表示する行または列の数を表します。 このプロパティの既定値は 1 であり、その値が 1 以上にある常に必要があります。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) ネイティブのレイアウト エンジンを使用すると、レイアウトを実行します。

## <a name="vertical-list"></a>垂直方向リスト

既定では、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)垂直方向のリストのレイアウトでその項目が表示されます。 そのため、設定する必要はありません、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)このレイアウトを使用するプロパティ。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

ただし、完全を期すのため、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)その項目を設定して、垂直方向に一覧表示に設定することができます、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを`VerticalList`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

また、これ行うこともできますを設定して、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティのオブジェクトを[ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout)クラスを指定する、 `Vertical` [ `ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)を引数として列挙型メンバー。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.Vertical
};
```

これにより 1 列のリストが生成され、そのリストは新しい項目が追加されると垂直に拡大します。

[![IOS と Android での CollectionView 縦方向リスト レイアウトのスクリーン ショット](layout-images/vertical-list.png "CollectionView 垂直レイアウト")](layout-images/vertical-list-large.png#lightbox "CollectionView 垂直方向のリストのレイアウト")

## <a name="horizontal-list"></a>水平方向リスト

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 水平方向のリストに設定してそのアイテムを表示できます、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを`HorizontalList`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalList">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

また、これ行うこともできますを設定して、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout)オブジェクトを指定する、 `Horizontal` [ `ItemsLayoutOrientation` ](xref:Xamarin.Forms.ItemsLayoutOrientation)を引数として列挙型メンバー。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout>
            <x:Arguments>
                <ItemsLayoutOrientation>Horizontal</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.Horizontal
};
```

これにより 1 行のリストが生成され、新しい項目が追加されると、水平方向に拡大します。

[![IOS と Android での CollectionView 一覧 (横並び) レイアウトのスクリーン ショット](layout-images/horizontal-list.png "CollectionView 水平レイアウト")](layout-images/horizontal-list-large.png#lightbox "CollectionView 水平方向のリストのレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 垂直のグリッドで設定してそのアイテムを表示できます、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout)オブジェクト[ `Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation) にプロパティを設定`Vertical`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定で垂直[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 1 つの列の項目が表示されます。 ただし、この例では、`GridItemsLayout.Span` プロパティに 2 を設定しています。 これによって、2 列のグリッドになり、新しい項目が追加されると、垂直方向に拡大します。

[![IOS と Android での CollectionView 垂直グリッド レイアウトのスクリーン ショット](layout-images/vertical-grid.png "CollectionView 垂直グリッド レイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 垂直グリッド レイアウト")

## <a name="horizontal-grid"></a>水平グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 水平のグリッドで設定してそのアイテムを表示できます、 [ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout)オブジェクト[`Orientation` ](xref:Xamarin.Forms.ItemsLayout.Orientation)プロパティに設定`Horizontal`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout) 1 つの行項目が表示されます。 ただし、この例では、`GridItemsLayout.Span` プロパティに 4 を設定しています。 これによって 4 行のグリッドになり、新しい項目が追加されると水平方向に拡大します。

[![IOS と Android での CollectionView 水平グリッド レイアウトのスクリーン ショット](layout-images/horizontal-grid.png "CollectionView 水平グリッド レイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッド レイアウト")

## <a name="item-spacing"></a>アイテム間の間隔

既定では、各項目を[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)周囲の空白部分はありません。 プロパティで使用される項目のレイアウトを設定してこの動作を変更することができます、`CollectionView`します。

ときに、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)設定その[ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[ `ListItemsLayout` ](xref:Xamarin.Forms.ListItemsLayout)オブジェクト、`ListItemsLayout.ItemSpacing`にプロパティを設定することができます、 `double`各アイテムの周囲の空白部分を表す値。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout ItemSpacing="20">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>    
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `ListItemsLayout.ItemSpacing`プロパティがプロパティの値が 0 以上では常にことにより、検証コールバック セット。

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

このコードは、各アイテムの周囲の 20 の空白文字のある垂直方向の 1 つの列リストが得られます。

[![IOS と Android でのアイテム間の間隔での CollectionView のスクリーン ショット](layout-images/vertical-list-spacing.png "CollectionView アイテム間の間隔")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView アイテム間の間隔")

ときに、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)設定その[ `ItemsLayout` ](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[ `GridItemsLayout` ](xref:Xamarin.Forms.GridItemsLayout)オブジェクト、`GridItemsLayout.VerticalItemSpacing`と`GridItemsLayout.HorizontalItemSpacing`プロパティを指定できます設定`double`を各項目垂直方向および水平方向に空の領域を表す値。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2"
                        VerticalItemSpacing="20"
                        HorizontalItemSpacing="30" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `GridItemsLayout.VerticalItemSpacing`と`GridItemsLayout.HorizontalItemSpacing`プロパティがある検証コールバックを設定すると、プロパティの値が 0 以上では常にあることを確認します。

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
    {
        VerticalItemSpacing = 20,
        HorizontalItemSpacing = 30
    }
};
```

このコードは、各項目、20 の上下の間隔と各アイテムの周囲の 30 の左右の間隔を持つ垂直の 2 つの列グリッドが得られます。

[![IOS と Android でのアイテム間の間隔での CollectionView のスクリーン ショット](layout-images/vertical-grid-spacing.png "CollectionView アイテム間の間隔")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView アイテム間の間隔")

## <a name="item-sizing"></a>項目のサイズ変更

既定では、各項目を[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)は個別に測定し、サイズ、内の UI 要素を提供する、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)固定サイズを指定しません。 この動作は、変更できますがで指定された、 [ `CollectionView.ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)プロパティの値。 このプロパティの値は、のいずれかに設定することができます、 [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemSizingStrategy)列挙型メンバー。

- `MeasureAllItems` – 各アイテムは個別に測定されます。 これは既定値です。
- `MeasureFirstItem` – 最初の項目のみが測定され、後続のすべての項目にはその最初の項目と同じサイズが与えられます。

> [!IMPORTANT]
> `MeasureFirstItem`戦略をサイズ変更、アイテムのサイズがすべての項目の間で統一する予定は状況で使用するとパフォーマンスの向上が発生します。

次のコード例は、設定を示しています、 [ `ItemSizingStrategy` ](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)プロパティ。

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>項目の動的なサイズ変更

内の項目を[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)動的にサイズを変更できる実行時に変更することでレイアウト内の要素のプロパティを関連する、 [ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。 たとえば、次のコード例の変更、 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)と[ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)のプロパティ、 [ `Image` ](xref:Xamarin.Forms.Image)オブジェクト。

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

`OnImageTapped`への応答でイベント ハンドラーが実行、 [ `Image` ](xref:Xamarin.Forms.Image) 、タップされるオブジェクトしより簡単に表示されるように、イメージのサイズを変更します。

[![IOS と Android での動的なサイズに設定したの CollectionView のスクリーン ショット](layout-images/runtime-resizing.png "CollectionView 動的アイテムのサイズ変更")](layout-images/runtime-resizing-large.png#lightbox "CollectionView 動的アイテムのサイズ変更")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CollectionView`](xref:Xamarin.Forms.CollectionView) レイアウトで右から左方向には、そのコンテンツを設定してできます、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティを[ `RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)します。 ただし、`FlowDirection` プロパティは、ページまたはルートレイアウト内のすべての要素をその方向の流れに応じるようにするために、ページまたはルートレイアウトに設定するのが理想的です。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

親を持つ要素の既定の [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) は、[`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) です。 そのため、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)継承、`FlowDirection`プロパティの値から、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、さらに継承する、`FlowDirection`プロパティの値から、 [`ContentPage`](xref:Xamarin.Forms.ContentPage). この結果、次のスクリーン ショットに示すように、右から左のレイアウトになります。

[![IOS と Android での CollectionView 垂直方向のリストを右から左のレイアウトのスクリーン ショット](layout-images/vertical-list-rtl.png "CollectionView 垂直方向のリストを右から左のレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左の垂直方向リストのレイアウト")

フローの方向に関する詳細については、[Right-to-left のローカリゼーション](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Right-to-left のローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.Forms の CollectionView スクロール](scrolling.md)
