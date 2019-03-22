---
title: Xamarin.Forms CollectionView レイアウトを指定します
description: 既定では、CollectionView を垂直方向の一覧でその項目が表示されます。 ただし、垂直および水平方向のリストとグリッドを指定することができます。
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
# <a name="specify-xamarinforms-collectionview-layout"></a>Xamarin.Forms CollectionView レイアウトを指定します

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView`は現在プレビュー段階で、その計画的な機能の一部が不足しています。 さらに、実装が完了すると、API を変更することがあります。

`CollectionView` レイアウトを制御する次のプロパティを定義します。

- `ItemsLayout`、型の`IItemsLayout`、使用するレイアウトを指定します。
- `ItemSizingStrategy`、型の`ItemSizingStrategy`、使用する項目のメジャーの戦略を指定します。

これらのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。

既定で、`CollectionView`を垂直方向に一覧の項目が表示されます。 ただし、使用のレイアウトのいずれかのことができます。

- 垂直方向リスト: 新しい項目が追加されると、垂直方向に拡大する 1 つの列の一覧。
- 水平方向リスト-新しい項目が追加されると、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 複数列のグリッドが新しい項目が追加されると、垂直方向に大きくなります。
- 水平グリッド – 複数の行がグリッドに新しい項目が追加されると、水平方向に拡大します。

これらのレイアウトを設定して指定できます、`ItemsLayout`から派生したクラスにプロパティ、`ItemsLayout`クラス。 このクラスは、次のプロパティを定義します。

- `Orientation`、型の`ItemsLayoutOrientation`、方向を指定、`CollectionView`項目を追加するように展開します。
- `SnapPointsAlignment`、型の`SnapPointsAlignment`、項目を含むのスナップ ポイントを配置する方法を指定します。
- `SnapPointsType`、型の`SnapPointsType`、スクロールするとき、スナップ ポイントの動作を指定します。

これらのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。 スナップ ポイントの詳細については、次を参照してください。[のスナップ ポイント](scrolling.md#snap-points)で、[項目をスクロールして表示](scrolling.md)ガイド。

`ItemsLayoutOrientation`列挙体は、次のメンバーを定義します。

- `Vertical` 示します、`CollectionView`項目が追加されると、垂直方向に展開されます。
- `Horizontal` 示します、`CollectionView`項目が追加されると水平方向に展開されます。

`ListItemsLayout`クラスから継承、`ItemsLayout`クラスし、静的な定義`VerticalList`と`HorizontalList`メンバー。 これらのメンバーは、それぞれに垂直または水平方向のリストを作成する使用できます。 または、`ListItemsLayout`を指定するオブジェクトを作成できる、`ItemsLayoutOrientation`を引数として列挙型メンバー。

`GridItemsLayout`クラスから継承、`ItemsLayout`クラスを定義しますを`Span`型のプロパティ、`int`グリッドに表示する行または列の数を表します。 既定値、`Span`プロパティは 1 であり、その値が 1 以上にある常に必要があります。

> [!NOTE]
> `CollectionView` ネイティブのレイアウト エンジンを使用すると、レイアウトを実行します。

## <a name="vertical-list"></a>垂直方向の一覧

既定では、`CollectionView`垂直方向のリストのレイアウトでその項目が表示されます。 そのため、設定する必要はありません、`ItemsLayout`このレイアウトを使用するプロパティ。

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

ただし、完全を期すのため、`CollectionView`その項目を設定して、垂直方向に一覧表示に設定することができます、 `ItemsLayout` 、静的な`ListItemsLayout.VerticalList`メンバー。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="{x:Static ListItemsLayout.VerticalList}">
    ...
</CollectionView>
```

また、これ行うこともできますを設定して、`ItemsLayout`プロパティのオブジェクトを`ListItemsLayout`クラスを指定する、 `Vertical` `ItemsLayoutOrientation`を引数として列挙型のメンバー。

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

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.VerticalList
};
```

これは、結果、新しい項目が追加されると、垂直方向に拡大、1 つの列の一覧。

[![IOS と Android での CollectionView 縦方向リスト レイアウトのスクリーン ショット](layout-images/vertical-list.png "CollectionView 垂直レイアウト")](layout-images/vertical-list-large.png#lightbox "CollectionView 垂直方向のリストのレイアウト")

## <a name="horizontal-list"></a>一覧 (横並び)

`CollectionView` 水平方向のリストに設定してそのアイテムを表示できます、`ItemsLayout`プロパティを静的な`ListItemsLayout.HorizontalList`メンバー。

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

また、これ行うこともできますを設定して、`ItemsLayout`プロパティを`ListItemsLayout`オブジェクトを指定する、 `Horizontal` `ItemsLayoutOrientation`を引数として列挙型のメンバー。

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

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = ListItemsLayout.HorizontalList
};
```

これは、新しい項目が追加されると、水平方向に拡大、1 つの行の一覧が得られます。

[![IOS と Android での CollectionView 一覧 (横並び) レイアウトのスクリーン ショット](layout-images/horizontal-list.png "CollectionView 水平レイアウト")](layout-images/horizontal-list-large.png#lightbox "CollectionView 水平方向のリストのレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

`CollectionView` 垂直のグリッドで設定してそのアイテムを表示できます、`ItemsLayout`プロパティを`GridItemsLayout`オブジェクト`Orientation`プロパティに設定されて`Vertical`:

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

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定で垂直`GridItemsLayout`1 つの列の項目が表示されます。 ただし、この例の設定、`GridItemsLayout.Span`プロパティを 2。 これは、新しい項目が追加されると、垂直方向に拡大、2 列のグリッドが得られます。

[![IOS と Android での CollectionView 垂直グリッド レイアウトのスクリーン ショット](layout-images/vertical-grid.png "CollectionView 垂直グリッド レイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 垂直グリッド レイアウト")

## <a name="horizontal-grid"></a>水平グリッド

`CollectionView` 水平のグリッドで設定してそのアイテムを表示できます、`ItemsLayout`プロパティを`GridItemsLayout`オブジェクト`Orientation`プロパティに設定されて`Horizontal`:

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

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平`GridItemsLayout`1 つの行項目が表示されます。 ただし、この例の設定、 `GridItemsLayout.Span` 4 プロパティ。 これは、新しい項目が追加されると、水平方向に拡大、4 つの行のグリッドが得られます。

[![IOS と Android での CollectionView 水平グリッド レイアウトのスクリーン ショット](layout-images/horizontal-grid.png "CollectionView 水平グリッド レイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッド レイアウト")

## <a name="right-to-left-layout"></a>右から左のレイアウト

`CollectionView` レイアウトで右から左方向には、そのコンテンツを設定してできます、 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティを[ `RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)します。 ただし、`FlowDirection`フローの方向に応答する ページで、またはルート レイアウト内のすべての要素が発生した場合、ページまたはルートのレイアウトのプロパティが設定理想的には必要があります。

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

既定の[ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)親を持つ要素が[ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)します。 そのため、`CollectionView`継承、`FlowDirection`プロパティの値から、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、さらに継承する、`FlowDirection`プロパティの値から、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). これは、次のスクリーン ショットに示すように、右から左のレイアウトが得られます。

[![IOS と Android での CollectionView 垂直方向のリストを右から左のレイアウトのスクリーン ショット](layout-images/vertical-list-rtl.png "CollectionView 垂直方向のリストを右から左のレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左の垂直方向リストのレイアウト")

フローの方向に関する詳細については、次を参照してください。[右から左のローカリゼーション](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)します。

## <a name="item-sizing"></a>項目のサイズ変更

既定では、各項目を`CollectionView`は個別に測定し、サイズ、内の UI 要素を提供する、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)固定サイズを指定しません。 この動作は、変更できますがで指定された、`CollectionView.ItemSizingStrategy`プロパティの値。 このプロパティの値は、のいずれかに設定することができます、`ItemSizingStrategy`列挙型メンバー。

- `MeasureAllItems` – 各アイテムは個別に測定されます。 これが既定値です。
- `MeasureFirstItem` – 最初の項目と同じサイズに指定されているすべての後続の項目で、最初の項目のみが測定されます。

> [!IMPORTANT]
> `MeasureFirstItem`戦略をサイズ変更は、し、アイテムのサイズがすべての項目に統一するものでは、パフォーマンスの向上の状況で使用する必要があります。

次のコード例は、設定を示しています、`ItemSizingStrategy`プロパティ。

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [右から左のローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [項目をビューにスクロールします。](scrolling.md)
