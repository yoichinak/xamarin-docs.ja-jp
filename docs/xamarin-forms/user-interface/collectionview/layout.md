---
title: Xamarin.Forms CollectionView のレイアウトを指定する
description: 既定では、CollectionView は垂直方向のリストでその項目を表示しますが、垂直および水平方向のリストとグリッドを指定することができます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 8ed365ed41ac31c66d41f1a32a7a16929cdc6770
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329940"
---
# <a name="specify-xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView のレイアウトを指定する

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` は現在プレビュー段階で、計画されている機能の一部が不足しています。 さらに、実装の完了時には、API は変更される可能性があります。

`CollectionView` にはレイアウトを制御する以下のプロパティが定義されています。

- `ItemsLayout`。`IItemsLayout` 型で使用するレイアウトを指定します。
- `ItemSizingStrategy`。`ItemSizingStrategy` 型で使用するアイテムのサイズ計測の戦略を指定します。

これらのプロパティは、[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。

既定で、`CollectionView` は、垂直方向のリストでその項目を表示しますが、以下のいずれかのレイアウトを使用することができます。

- 垂直方向リスト – 新しい項目が追加されるにつれ、垂直方向に拡大する 1 つの列のリスト。
- 水平方向リスト – 新しい項目が追加されるにつれ、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 新しい項目が追加されるにつれ、垂直方向に拡大する複数列のグリッド。
- 水平グリッド – 新しい項目が追加されるにつれ、水平方向に拡大する複数行のグリッド。

これらのレイアウトは、`ItemsLayout` プロパティに `ItemsLayout` クラスから派生したクラスを設定して指定できます このクラスは、以下のようなプロパティが定義されています。

- `Orientation` – `ItemsLayoutOrientation` 型で、アイテムを追加した時に `CollectionView` が拡大する方向を指定します。
- `SnapPointsAlignment` – `SnapPointsAlignment` 型で、アイテムを含むスナップ ポイントを配置する方法を指定します。
- `SnapPointsType` – `SnapPointsType` 型で、スクロールするときのスナップポイントの動作を指定します。

これらのプロパティは、[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。 スナップ ポイントの詳細については、[ビューの項目をスクロールして表示](scrolling.md) の [スナップ ポイント](scrolling.md#snap-points) を参照してください。

`ItemsLayoutOrientation` 列挙体には、以下のメンバーが定義されています。

- `Vertical` – アイテムが追加された時に、`CollectionView` が垂直に拡大することを示します。
- `Horizontal` – アイテムが追加された時に、`CollectionView` が水平に拡大することを示します。

`ListItemsLayout` クラスは、`ItemsLayout` クラスを継承し、静的な `VerticalList` と `HorizontalList` メンバーが定義されています。 これらのメンバーを使って、垂直または水平方向のリストをそれぞれ作成することができます。 また、`ListItemsLayout` オブジェクトは、`ItemsLayoutOrientation` 列挙型メンバーを引数として指定して生成することができます。

`GridItemsLayout` クラスは、 `ItemsLayout` クラスを継承し、 `int` 型の `Span` プロパティが定義されており、グリッドの中に表示する列数・行数を表します。 `Span` プロパティの規定値は 1 で、その値は常に1 以上である必要があります。

> [!NOTE]
> `CollectionView` は、ネイティブのレイアウト エンジンを使用して、レイアウトを実行します。

## <a name="vertical-list"></a>垂直方向リスト

既定では、`CollectionView` は、垂直リストのレイアウトでアイテムを表示します。そのため、このレイアウトを使うために `ItemsLayout` プロパティを設定する必要はありません。

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

ただし、完全を求めるなら、`CollectionView` は、`ItemsLayout` に静的な `ListItemsLayout.VerticalList` メンバーを設定することで、垂直リストでアイテムを表示するための設定ができます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

また、`ItemsLayout` プロパティに 引数として `ItemsLayoutOrientation` 列挙型のメンバーの `Vertical` を指定した `ListItemsLayout` クラスのオブジェクトをセットすることでも設定できます。

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

同等の C# コードは以下の通りです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.VerticalList
};
```

この結果、1 列のリストになり、新しいアイテムが追加されると垂直に拡大します。

[![IOS と Android での CollectionView 縦方向リスト レイアウトのスクリーン ショット](layout-images/vertical-list.png "CollectionView 垂直レイアウト")](layout-images/vertical-list-large.png#lightbox "CollectionView 垂直方向のリストのレイアウト")

## <a name="horizontal-list"></a>水平方向リスト

`CollectionView` は、`ItemsLayout` プロパティに静的な `ListItemsLayout.HorizontalList` メンバーを設定することで、水平方向のリストにアイテムを表示できます。

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

また、`ItemsLayout` プロパティに 引数として `ItemsLayoutOrientation` 列挙型のメンバーの `Horizontal` を指定した `ListItemsLayout` オブジェクトをセットすることでも設定できます。

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

同等の C# コードは以下の通りです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.HorizontalList
};
```

この結果、1 行リストになり、新しいアイテムが追加されると、水平方向に拡大します。

[![IOS と Android での CollectionView 一覧 (横並び) レイアウトのスクリーン ショット](layout-images/horizontal-list.png "CollectionView 水平レイアウト")](layout-images/horizontal-list-large.png#lightbox "CollectionView 水平方向のリストのレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

`CollectionView` は、`ItemsLayout` プロパティに `GridItemsLayout` オブジェクトを設定し、その `Orientation` プロパティに `Vertical` を設定することで垂直のグリッドにアイテムを表示できます。

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

同等の C# コードは以下の通りです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定では、垂直の `GridItemsLayout` は、1 つの列にアイテムを表示します。 ただし、この例では、 `GridItemsLayout.Span` プロパティに 2 を設定しています。 これによって、 2 列のグリッドになり、新しい項目が追加されると、垂直方向に拡大します。

[![IOS と Android での CollectionView 垂直グリッド レイアウトのスクリーン ショット](layout-images/vertical-grid.png "CollectionView 垂直グリッド レイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 垂直グリッド レイアウト")

## <a name="horizontal-grid"></a>水平グリッド

`CollectionView` は、`ItemsLayout` プロパティに `GridItemsLayout` オブジェクトを設定し、その `Orientation` プロパティに `Horizontal` を設定することで水平のグリッドでアイテムを表示できます。

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

同等の C# コードは以下の通りです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平の `GridItemsLayout` は、1 つの行にアイテムを表示します。ただし、この例では、 `GridItemsLayout.Span` プロパティに 4 を設定しています。 これによって、4 行のグリッドになり、新しい項目が追加されると、水平方向に拡大します。

[![IOS と Android での CollectionView 水平グリッド レイアウトのスクリーン ショット](layout-images/horizontal-grid.png "CollectionView 水平グリッド レイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッド レイアウト")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

`CollectionView` は、[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティに[ `RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)を設定することで、右から左へ流れる方向でコンテンツをレイアウトできます。 ただし、`FlowDirection` プロパティは、ページまたはルートレイアウト内の全ての要素がその流れの方向に反応させるために、ページまたはルートレイアウトに設定するのが理想的です。

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

親を持つ要素の既定の [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) は、[ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) です。 そのため、`CollectionView` は、[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) から `FlowDirection` プロパティの値を継承します。さらにその `StackLayout` は、[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) から `FlowDirection` プロパティの値を継承しています。 この結果、次のスクリーン ショットに示すように、右から左のレイアウトになります。

[![IOS と Android での CollectionView 垂直方向のリストを右から左のレイアウトのスクリーン ショット](layout-images/vertical-list-rtl.png "CollectionView 垂直方向のリストを右から左のレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左の垂直方向リストのレイアウト")

フローの方向に関する詳細については、[Right-to-left のローカリゼーション](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)を参照してください。

## <a name="item-sizing"></a>項目のサイズ変更

既定では、`CollectionView` の各項目は、[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) の UI 要素 が、固定のサイズを指定しない場合は、個別に測定しサイズが決定されます。 この動作は、変更できますが、それは `CollectionView.ItemSizingStrategy` プロパティの値で指定します。 このプロパティの値には、`ItemSizingStrategy` 列挙型のメンバーの 1 つを設定することができます。

- `MeasureAllItems` – 各アイテムは個別に測定されます。 これは既定値です。
- `MeasureFirstItem` – 最初の項目のみが測定され、全ての後続の項目は最初の項目と同じサイズが与えられます。

> [!IMPORTANT]
> `MeasureFirstItem` サイズ戦略は、項目のサイズが全ての項目を通して均一であることを意図した状況で使用する必要があり、その結果としてパフォーマンスが向上します。

次のコード例は、`ItemSizingStrategy` プロパティの設定を示しています。

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

同等の C# コードは以下のとおりです。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Right-to-left のローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [ビューのアイテムへのスクロール](scrolling.md)
