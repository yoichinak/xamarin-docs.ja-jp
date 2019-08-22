---
title: CollectionView レイアウト
description: 既定では、CollectionView は垂直方向のリストでその項目を表示しますが、 垂直および水平方向のリストとグリッドを指定することができます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/12/2019
ms.openlocfilehash: e22b79fada5582adfec05ce7c5ebeddd6fe7e5d2
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888656"
---
# <a name="xamarinforms-collectionview-layout"></a>CollectionView レイアウト

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)レイアウトを制御する次のプロパティを定義します。

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)型[`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout)ので、使用するレイアウトを指定します。
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)型[`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)のは、使用される項目メジャー戦略を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の項目が縦の一覧に表示されます。 以下のいずれかのレイアウトを使用することができます。

- 垂直方向リスト – 新しい項目が追加されるにつれ、垂直方向に拡大する 1 つの列のリスト。
- 水平方向リスト – 新しい項目が追加されるにつれ、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 新しい項目が追加されるにつれ、垂直方向に拡大する複数列のグリッド。
- 水平グリッド – 新しい項目が追加されるにつれ、水平方向に拡大する複数行のグリッド。

これらのレイアウトは、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから派生するクラスにプロパティを設定することによって指定できます。 以下のようなプロパティを定義します。

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)型[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)の。は、を項目として[`CollectionView`](xref:Xamarin.Forms.CollectionView)追加する方向を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)(型[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)) では、スナップポイントを項目と整列する方法を指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)型[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)のは、スクロール時のスナップポイントの動作を指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。 スナップポイントの詳細については、「 [CollectionView スクロール](scrolling.md)ガイド」の「[スナップポイント](scrolling.md#snap-points)」を参照してください。

列挙[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)体は、次のメンバーを定義します。

- `Vertical`項目が追加[`CollectionView`](xref:Xamarin.Forms.CollectionView)されると、が垂直方向に展開されることを示します。
- `Horizontal`項目が追加[`CollectionView`](xref:Xamarin.Forms.CollectionView)されると、が水平方向に展開されることを示します。

クラス[`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout)は、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) クラスから`double`継承され、各項目の周囲の空白を表す型のプロパティを定義します。`ItemSpacing` このプロパティの既定値は0で、値は常に0以上である必要があります。 クラス`ListItemsLayout`は、静的`Vertical`メンバーと`Horizontal`メンバーも定義します。 これらのメンバーを使って、垂直または水平方向のリストをそれぞれ作成することができます。 または、 `ListItemsLayout`オブジェクトを作成し、 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙型のメンバーを引数として指定することもできます。

クラス[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)は、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承され、次のプロパティを定義します。

- `VerticalItemSpacing`型`double`の。各項目の周りの垂直方向の空白を表します。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- `HorizontalItemSpacing`型`double`の。各項目の周りの水平方向の空白を表します。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- `Span`グリッドに表示`int`する列または行の数を表す、型の。 このプロパティの既定値は1で、その値は常に1以上である必要があります。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)ネイティブレイアウトエンジンを使用してレイアウトを実行します。

## <a name="vertical-list"></a>垂直方向リスト

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、は、項目を縦に並べたリストレイアウトで表示します。 このため、このレイアウトを使用するよう[`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)にプロパティを設定する必要はありません。

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

ただし、完全を[`CollectionView`](xref:Xamarin.Forms.CollectionView)期すために、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティをに設定する`VerticalList`ことによって、項目を縦に並べて表示するようにを設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

または、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout)クラスのオブジェクトに設定して、列挙体`Orientation`の`Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)メンバーをプロパティ値として指定する方法もあります。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout Orientation="Vertical" />
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

[IOS および Android(layout-images/vertical-list.png "CollectionView の縦の一覧レイアウト")![の CollectionView 縦のリストレイアウトのスクリーンショット]](layout-images/vertical-list-large.png#lightbox "CollectionView 縦のリストレイアウト")

## <a name="horizontal-list"></a>水平方向リスト

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティをに設定することにより、項目`HorizontalList`を横方向の一覧に表示できます。

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

または、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout)クラスのオブジェクトに設定して、列挙体`Orientation`の`Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)メンバーをプロパティ値として指定する方法もあります。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout Orientation="Horizontal" />
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

[ ![IOS および Android](layout-images/horizontal-list.png "CollectionView の横の一覧レイアウト")の CollectionView 横リストレイアウトのスクリーンショット](layout-images/horizontal-list-large.png#lightbox "CollectionView 横方向リストレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では[`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) 、プロパティがに[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) `Vertical`設定されているオブジェクト[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)にプロパティを設定することにより、項目を垂直グリッドに表示できます。

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

既定では、縦[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)のは1つの列に項目が表示されます。 ただし、この例では、`GridItemsLayout.Span` プロパティに 2 を設定しています。 これによって、2 列のグリッドになり、新しい項目が追加されると、垂直方向に拡大します。

[IOS および Android(layout-images/vertical-grid.png "CollectionView の垂直グリッドレイアウト")![の CollectionView 垂直グリッドレイアウトのスクリーンショット]](layout-images/vertical-grid-large.png#lightbox "CollectionView 縦グリッドレイアウト")

## <a name="horizontal-grid"></a>水平グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では[`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) 、プロパティがに[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) `Horizontal`設定されているオブジェクト[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)にプロパティを設定することにより、項目を水平グリッドに表示できます。

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

既定では、水平[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)方向には1行に項目が表示されます。 ただし、この例では、`GridItemsLayout.Span` プロパティに 4 を設定しています。 これによって 4 行のグリッドになり、新しい項目が追加されると水平方向に拡大します。

[IOS および Android(layout-images/horizontal-grid.png "CollectionView の水平グリッドレイアウト")![の CollectionView 水平グリッドレイアウトのスクリーンショット]](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッドレイアウト")

## <a name="headers-and-footers"></a>ヘッダーとフッター

[`CollectionView`](xref:Xamarin.Forms.CollectionView)リスト内の項目と共にスクロールするヘッダーとフッターを表示できます。 ヘッダーとフッターには、文字列、ビュー、また[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)はオブジェクトを指定できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ヘッダーとフッターを指定するための次のプロパティを定義します。

- `Header`型`object`のは、リストの先頭に表示される文字列、バインド、またはビューを指定します。
- `HeaderTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のは、の書式`DataTemplate`設定に使用`Header`するを指定します。
- `Footer`型`object`のは、リストの末尾に表示される文字列、バインド、またはビューを指定します。
- `FooterTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のは、の書式`DataTemplate`設定に使用`Footer`するを指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

> [!IMPORTANT]
> 現在、ヘッダーとフッターは Android でのみサポートされています。

水平方向に拡張されるレイアウトにヘッダーが追加されると、左から右にヘッダーが表示されます。 同様に、水平方向に拡大するレイアウトにフッターを追加すると、一覧の右側にフッターが表示されます。

### <a name="display-strings-in-the-header-and-footer"></a>ヘッダーとフッターに文字列を表示する

プロパティ`Header` `string`と`Footer`プロパティは、次の例に示すように、値に設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    Header = "Monkeys",
    Footer = "2019"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

### <a name="display-views-in-the-header-and-footer"></a>ヘッダーとフッターにビューを表示する

プロパティ`Header` と`Footer`プロパティはそれぞれ、ビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の例では`Header` 、 `Footer` [`Label`](xref:Xamarin.Forms.Label)オブジェクトを含む[`StackLayout`](xref:Xamarin.Forms.StackLayout)オブジェクトにそれぞれ設定されているプロパティとプロパティを示しています。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.Header>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Monkeys"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Header>
    <CollectionView.Footer>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Friends of Xamarin Monkey"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Footer>
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    Header = new StackLayout
    {
        Children =
        {
            new Label { Text = "Monkeys", ... }
        }
    },
    Footer = new StackLayout
    {
        Children =
        {
            new Label { Text = "Friends of Xamarin Monkey", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

### <a name="display-a-templated-header-and-footer"></a>テンプレート化されたヘッダーとフッターを表示する

プロパティ`HeaderTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)と`FooterTemplate`プロパティは、ヘッダーとフッターの書式設定に使用されるオブジェクトに設定できます。 このシナリオ`Header`では、次`Footer`の例に示すように、プロパティとプロパティを、適用するテンプレートの現在のソースにバインドする必要があります。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="{Binding .}"
                Footer="{Binding .}">
    <CollectionView.HeaderTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Monkeys"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.HeaderTemplate>
    <CollectionView.FooterTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Friends of Xamarin Monkey"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.FooterTemplate>
    ...
</CollectionView>
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    HeaderTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    }),
    FooterTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    })
};
collectionView.SetBinding(ItemsView.HeaderProperty, ".");
collectionView.SetBinding(ItemsView.FooterProperty, ".");
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

## <a name="item-spacing"></a>項目の間隔

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、の各項目には、周囲の空白はありません。 この動作は、 `CollectionView`によって使用される項目のレイアウトのプロパティを設定することによって変更できます。

が[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティを[`ListItemsLayout`](xref:Xamarin.Forms.ListItemsLayout)オブジェクトに設定すると、プロパティ`ListItemsLayout.ItemSpacing`は、各項目の周囲`double`の空白を表す値に設定できます。

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
> `ListItemsLayout.ItemSpacing`プロパティには、プロパティの値が常に0以上であることを保証する検証コールバックセットがあります。

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

このコードを実行すると、垂直方向の単一列リストが生成され、各項目の周りに20の間隔があります。

[![項目の間隔がある CollectionView のスクリーンショット (IOS と Android の](layout-images/vertical-list-spacing.png "CollectionView 項目の間隔"))](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView item の間隔")

が[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout)プロパティ[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) をオブジェクトに設定`GridItemsLayout.HorizontalItemSpacing`すると、プロパティ`double`とプロパティは、各項目の垂直方向および水平方向の空白を表す値に設定できます。 `GridItemsLayout.VerticalItemSpacing`

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
> プロパティ`GridItemsLayout.VerticalItemSpacing` と`GridItemsLayout.HorizontalItemSpacing`プロパティには検証コールバックが設定されています。これにより、プロパティの値が常に0以上になるようにします。

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

このコードを実行すると、垂直方向の2列グリッドが生成されます。このグリッドには、各項目の周りに20の垂直方向の間隔があり、各項目の前後に30の水平方向の間隔があります。

[![項目の間隔がある CollectionView のスクリーンショット (IOS と Android の](layout-images/vertical-grid-spacing.png "CollectionView 項目の間隔"))](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView item の間隔")

## <a name="item-sizing"></a>項目のサイズ変更

既定では、の各項目[`CollectionView`](xref:Xamarin.Forms.CollectionView)は個別に測定およびサイズ設定されます。これは[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 、内の UI 要素が固定サイズを指定しない場合に限ります。 この動作は変更可能ですが、 [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)プロパティ値によって指定されます。 このプロパティ値は、 [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)列挙体のいずれかのメンバーに設定できます。

- `MeasureAllItems`–各項目は個別に測定されます。 これは既定値です。
- `MeasureFirstItem` – 最初の項目のみが測定され、後続のすべての項目にはその最初の項目と同じサイズが与えられます。

> [!IMPORTANT]
> サイズ`MeasureFirstItem`調整戦略を使用すると、項目のサイズがすべての項目にわたって一様であることが意図されている場合に、パフォーマンスが向上します。

次のコード例は、プロパティ[`ItemSizingStrategy`](xref:Xamarin.Forms.ItemsView.ItemSizingStrategy)の設定を示しています。

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

内の項目[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)内の要素のレイアウトに関連するプロパティを変更することで、実行時に動的に変更できます。 たとえば、次のコード例では、 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`Image`](xref:Xamarin.Forms.Image)オブジェクト[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)のプロパティとプロパティを変更します。

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

イベントハンドラーは、タップされる[`Image`](xref:Xamarin.Forms.Image)オブジェクトに応答して実行され、より簡単に表示されるようにイメージのサイズを変更します。 `OnImageTapped`

[![動的な項目のサイズ設定を使用した CollectionView のスクリーンショット IOS と Android の](layout-images/runtime-resizing.png "CollectionView 動的な項目のサイズ")]設定(layout-images/runtime-resizing-large.png#lightbox "CollectionView 動的な項目のサイズ変更")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティをに設定する[`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)ことによって、右から左へのフロー方向にコンテンツをレイアウトできます。 ただし、`FlowDirection` プロパティは、ページまたはルートレイアウト内のすべての要素をその方向の流れに応じるようにするために、ページまたはルートレイアウトに設定するのが理想的です。

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

親を持つ要素の既定の [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) は、[`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) です。 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentPage`](xref:Xamarin.Forms.ContentPage) `FlowDirection`したがって、は、 `FlowDirection`からプロパティ値を[継承し、さらにからプロパティ値を継承します。`CollectionView`](xref:Xamarin.Forms.CollectionView) この結果、次のスクリーン ショットに示すように、右から左のレイアウトになります。

[ ![IOS と Android での CollectionView の右から左方向のリストレイアウトのスクリーンショット (](layout-images/vertical-list-rtl.png "右から左に記述")された、右から左に記述された一覧レイアウト)](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左方向のリストレイアウト")

フローの方向に関する詳細については、[Right-to-left のローカリゼーション](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Right-to-left のローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [CollectionView のスクロール](scrolling.md)
