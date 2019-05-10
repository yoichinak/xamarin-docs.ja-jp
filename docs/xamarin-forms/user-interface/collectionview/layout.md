---
title: Xamarin.Forms CollectionView レイアウト
description: 既定では、CollectionView は垂直方向のリストでその項目を表示しますが、 垂直および水平方向のリストとグリッドを指定することができます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 5543bcc93f3c38b56a4a6caa0ea23b8ccf434e1c
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048243"
---
# <a name="xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView レイアウト

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` はレイアウトを制御する以下のプロパティを定義しています。

- `ItemsLayout`。`IItemsLayout` 型で使用するレイアウトを指定します。
- `ItemSizingStrategy`。`ItemSizingStrategy` 型の、使用するアイテム サイズ計測の戦略を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

既定で、 `CollectionView` は、垂直方向のリストでその項目を表示しますが、 以下のいずれかのレイアウトを使用することができます。

- 垂直方向リスト – 新しい項目が追加されるにつれ、垂直方向に拡大する 1 つの列のリスト。
- 水平方向リスト – 新しい項目が追加されるにつれ、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 新しい項目が追加されるにつれ、垂直方向に拡大する複数列のグリッド。
- 水平グリッド – 新しい項目が追加されるにつれ、水平方向に拡大する複数行のグリッド。

これらのレイアウトは、`ItemsLayout` プロパティに `ItemsLayout` クラスから派生したクラスを設定して指定できます このクラスは、 以下のようなプロパティを定義します。

- `Orientation` – `ItemsLayoutOrientation` 型で、項目を追加したときに `CollectionView` が拡大する方向を指定します。
- `SnapPointsAlignment` – `SnapPointsAlignment` 型で、スナップ ポイントを項目と揃える方法を指定します。
- `SnapPointsType` – `SnapPointsType` 型で、スクロールするときのスナップポイントの動作を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。 スナップ ポイントの詳細については、次を参照してください。[のスナップ ポイント](scrolling.md#snap-points)で、 [Xamarin.Forms CollectionView スクロール](scrolling.md)ガイド。

`ItemsLayoutOrientation`列挙体は、次のメンバーを定義します。

- `Vertical` – 項目が追加されたときに、`CollectionView` が垂直に拡大することを示します。
- `Horizontal` – 項目が追加されたときに、`CollectionView` が水平に拡大することを示します。

`ListItemsLayout` クラスは、`ItemsLayout` クラスを継承し、静的な `VerticalList` と `HorizontalList` メンバーを定義します。 これらのメンバーを使って、垂直または水平方向のリストをそれぞれ作成することができます。 また、`ListItemsLayout` オブジェクトは、`ItemsLayoutOrientation` 列挙型メンバーを引数として指定して作成することができます。

`GridItemsLayout` クラスは、 `ItemsLayout` クラスを継承し、 `int` 型の `Span` プロパティを定義しており、グリッドの中に表示する列数または行数を表します。 `Span` プロパティの既定値は 1 で、その値は常に1 以上でなければなりません。

> [!NOTE]
> `CollectionView` は、ネイティブのレイアウト エンジンを使用して、レイアウトを実行します。

## <a name="vertical-list"></a>垂直方向リスト

既定では、`CollectionView` は、垂直リストのレイアウトで項目を表示します ただし、`ItemsLayout` 次のどのレイアウトでも使用できます。

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

ただし、完全さを期して、`CollectionView` は、`ItemsLayout` に静的な `ListItemsLayout.VerticalList` メンバーを設定することで、垂直リストで項目を表示するように設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

また、引数として `ItemsLayoutOrientation` 列挙型のメンバーの `Vertical` を指定して、`ItemsLayout` プロパティに `ListItemsLayout` クラスのオブジェクトを設定することでも実現できます。

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

同等の C# コードは以下のとおりです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.VerticalList
};
```

これにより 1 列のリストが生成され、そのリストは新しい項目が追加されると垂直に拡大します。

[![IOS と Android での CollectionView 縦方向リスト レイアウトのスクリーン ショット](layout-images/vertical-list.png "CollectionView 垂直レイアウト")](layout-images/vertical-list-large.png#lightbox "CollectionView 垂直方向のリストのレイアウト")

## <a name="horizontal-list"></a>水平方向リスト

`CollectionView` は、`ItemsLayout` プロパティに静的な `ListItemsLayout.HorizontalList` メンバーを設定することで、水平方向のリストで項目を表示できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.HorizontalList}">
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

また、引数として `Horizontal` `ItemsLayoutOrientation` 列挙型のメンバーを指定して、`ItemsLayout` プロパティに`ListItemsLayout` オブジェクトを設定することでも実現できます。

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

同等の C# コードは以下のとおりです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.HorizontalList
};
```

これにより 1 行のリストが生成され、新しい項目が追加されると、水平方向に拡大します。

[![IOS と Android での CollectionView 一覧 (横並び) レイアウトのスクリーン ショット](layout-images/horizontal-list.png "CollectionView 水平レイアウト")](layout-images/horizontal-list-large.png#lightbox "CollectionView 水平方向のリストのレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

`CollectionView` は、`ItemsLayout` プロパティに `GridItemsLayout` オブジェクトを設定し、その `Orientation` プロパティに `Vertical` を設定することで垂直のグリッドに項目を表示できます。

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

同等の C# コードは以下のとおりです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定では、垂直の `GridItemsLayout` は、1 つの列に項目を表示します。 ただし、この例では、`GridItemsLayout.Span` プロパティに 2 を設定しています。 これによって、2 列のグリッドになり、新しい項目が追加されると、垂直方向に拡大します。

[![IOS と Android での CollectionView 垂直グリッド レイアウトのスクリーン ショット](layout-images/vertical-grid.png "CollectionView 垂直グリッド レイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 垂直グリッド レイアウト")

## <a name="horizontal-grid"></a>水平グリッド

`CollectionView` は `ItemsLayout` プロパティに `GridItemsLayout` オブジェクトを設定し、その `Orientation` プロパティに `Horizontal` を設定することで、水平のグリッドで項目を表示できます。

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

同等の C# コードは以下のとおりです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平の `GridItemsLayout` は、1 つの行に項目を表示します。 ただし、この例では、`GridItemsLayout.Span` プロパティに 4 を設定しています。 これによって 4 行のグリッドになり、新しい項目が追加されると水平方向に拡大します。

[![IOS と Android での CollectionView 水平グリッド レイアウトのスクリーン ショット](layout-images/horizontal-grid.png "CollectionView 水平グリッド レイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッド レイアウト")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

`CollectionView` は、[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) プロパティに [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) を設定することで、右から左に流れる方向でコンテンツをレイアウトできます。 ただし、`FlowDirection` プロパティは、ページまたはルートレイアウト内のすべての要素をその方向の流れに応じるようにするために、ページまたはルートレイアウトに設定するのが理想的です。

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

親を持つ要素の既定の [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) は、[`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) です。 そのため、`CollectionView` は、[`StackLayout`](xref:Xamarin.Forms.StackLayout) から `FlowDirection` プロパティの値を継承します。さらにその `StackLayout` は、[`ContentPage`](xref:Xamarin.Forms.ContentPage) から `FlowDirection` プロパティの値を継承します。 この結果、次のスクリーン ショットに示すように、右から左のレイアウトになります。

[![IOS と Android での CollectionView 垂直方向のリストを右から左のレイアウトのスクリーン ショット](layout-images/vertical-list-rtl.png "CollectionView 垂直方向のリストを右から左のレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左の垂直方向リストのレイアウト")

フローの方向に関する詳細については、[Right-to-left のローカリゼーション](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)を参照してください。

## <a name="item-sizing"></a>項目のサイズ変更

既定では、`CollectionView` の各項目は、[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) の UI 要素が固定のサイズを指定しない場合は、個別に測定されサイズが決定されます。 この動作は変更でき、`CollectionView.ItemSizingStrategy` プロパティの値で指定します。 このプロパティの値には、`ItemSizingStrategy` 列挙型のメンバーの 1 つを設定することができます。

- `MeasureAllItems` – 各アイテムは個別に測定されます。 これは既定値です。
- `MeasureFirstItem` – 最初の項目のみが測定され、後続のすべての項目にはその最初の項目と同じサイズが与えられます。

> [!IMPORTANT]
> `MeasureFirstItem`戦略をサイズ変更、アイテムのサイズがすべての項目の間で統一する予定は状況で使用するとパフォーマンスの向上が発生します。

次のコード例は、`ItemSizingStrategy` プロパティの設定を示しています。

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

> [!NOTE]
> 戦略をサイズ変更項目は、現在で実装されている iOS のみです。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Right-to-left のローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.Forms の CollectionView スクロール](scrolling.md)
