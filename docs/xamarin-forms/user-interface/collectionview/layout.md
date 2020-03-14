---
title: CollectionView レイアウト
description: 既定では、CollectionView は垂直方向のリストでその項目を表示しますが、 垂直および水平方向のリストとグリッドを指定することができます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2019
ms.openlocfilehash: 6c2b3d8bad621db3110fe25041125c5694f21180
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305919"
---
# <a name="xamarinforms-collectionview-layout"></a>CollectionView レイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、レイアウトを制御する次のプロパティを定義します。

- [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout)型の[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)は、使用するレイアウトを指定します。
- [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)型の[`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)は、使用する項目メジャーの方法を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の項目は縦の一覧に表示されます。 以下のいずれかのレイアウトを使用することができます。

- 垂直方向リスト – 新しい項目が追加されるにつれ、垂直方向に拡大する 1 つの列のリスト。
- 水平方向リスト – 新しい項目が追加されるにつれ、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 新しい項目が追加されるにつれ、垂直方向に拡大する複数列のグリッド。
- 水平グリッド – 新しい項目が追加されるにつれ、水平方向に拡大する複数行のグリッド。

これらのレイアウトを指定するには、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから派生したクラスに設定します。 以下のようなプロパティを定義します。

- [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)型の[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)は、項目が追加されたときに[`CollectionView`](xref:Xamarin.Forms.CollectionView)を拡張する方向を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)型の[`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)は、スナップポイントを項目にどのように整列させるかを指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)型の[`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)は、スクロール時のスナップポイントの動作を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。 スナップポイントの詳細については、「 [CollectionView スクロール](scrolling.md)ガイド」の「[スナップポイント](scrolling.md#snap-points)」を参照してください。

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙体は、次のメンバーを定義します。

- `Vertical` は、項目が追加されると、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)が垂直方向に拡張されることを示します。
- `Horizontal` は、項目が追加されると、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)が横方向に拡張されることを示します。

`LinearItemsLayout` クラスは[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承され、各項目の周囲の空白を表す `double`型の `ItemSpacing` プロパティを定義します。 このプロパティの既定値は0で、値は常に0以上である必要があります。 `LinearItemsLayout` クラスは、静的 `Vertical` と `Horizontal` メンバーも定義します。 これらのメンバーを使って、垂直または水平方向のリストをそれぞれ作成することができます。 または、`LinearItemsLayout` オブジェクトを作成し、 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙型のメンバーを引数として指定することもできます。

[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)クラスは[`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承され、次のプロパティを定義します。

- 各項目の周りの垂直方向の空白を表す、型 `double`の `VerticalItemSpacing`。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- 各項目の周りの水平方向の空白を表す、型 `double`の `HorizontalItemSpacing`。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- グリッドに表示する列または行の数を表す `int`型の `Span`。 このプロパティの既定値は1で、その値は常に1以上である必要があります。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、ネイティブレイアウトエンジンを使用してレイアウトを実行します。

## <a name="vertical-list"></a>垂直方向リスト

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)によって項目が縦の一覧レイアウトで表示されます。 このため、このレイアウトを使用するように[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを設定する必要はありません。

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

ただし、完全を期すために、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを `VerticalList`に設定して、項目を縦に並べた一覧に表示するように[`CollectionView`](xref:Xamarin.Forms.CollectionView)を設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

または、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを `LinearItemsLayout` オブジェクトに設定して、`Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙メンバーを `Orientation` プロパティ値として指定することでも実現できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

これにより 1 列のリストが生成され、そのリストは新しい項目が追加されると垂直に拡大します。

[![IOS と Android の CollectionView 縦のリストレイアウトのスクリーンショット](layout-images/vertical-list.png "CollectionView 縦のリストレイアウト")](layout-images/vertical-list-large.png#lightbox "CollectionView 縦のリストレイアウト")

## <a name="horizontal-list"></a>水平方向リスト

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを `HorizontalList`に設定することにより、項目を横方向の一覧に表示できます。

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

また、このレイアウトを実現するには、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを `LinearItemsLayout` オブジェクトに設定し、`Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙メンバーを `Orientation` プロパティ値として指定します。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

これにより 1 行のリストが生成され、新しい項目が追加されると、水平方向に拡大します。

[![IOS と Android での CollectionView 横方向リストレイアウトのスクリーンショット](layout-images/horizontal-list.png "CollectionView 横方向リストレイアウト")](layout-images/horizontal-list-large.png#lightbox "CollectionView 横方向リストレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)プロパティが `Vertical`に設定されている[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)オブジェクトに設定することにより、項目を垂直グリッドに表示できます。

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

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定では、垂直[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)によって1つの列に項目が表示されます。 ただし、この例では、`GridItemsLayout.Span` プロパティを2に設定しています。 これによって、2 列のグリッドになり、新しい項目が追加されると、垂直方向に拡大します。

[![IOS と Android の CollectionView 垂直グリッドレイアウトのスクリーンショット](layout-images/vertical-grid.png "CollectionView 縦グリッドレイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 縦グリッドレイアウト")

## <a name="horizontal-grid"></a>水平グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)プロパティが `Horizontal`に設定されている[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)オブジェクトに設定することにより、項目を水平グリッドに表示できます。

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

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)によって1つの行に項目が表示されます。 ただし、この例では、`GridItemsLayout.Span` プロパティを4に設定しています。 これによって 4 行のグリッドになり、新しい項目が追加されると水平方向に拡大します。

[![IOS と Android の CollectionView 水平グリッドレイアウトのスクリーンショット](layout-images/horizontal-grid.png "CollectionView 水平グリッドレイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッドレイアウト")

## <a name="headers-and-footers"></a>ヘッダーとフッター

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、リスト内の項目と共にスクロールするヘッダーとフッターを表示できます。 ヘッダーとフッターには、文字列、ビュー、または[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトを指定できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、ヘッダーとフッターを指定するための次のプロパティを定義します。

- `object`型の `Header`は、リストの先頭に表示される文字列、バインド、またはビューを指定します。
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の `HeaderTemplate`は、`Header`の書式設定に使用する `DataTemplate` を指定します。
- `object`型の `Footer`は、リストの末尾に表示される文字列、バインド、またはビューを指定します。
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の `FooterTemplate`は、`Footer`の書式設定に使用する `DataTemplate` を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

水平方向に拡張されるレイアウトにヘッダーが追加されると、左から右にヘッダーが表示されます。 同様に、水平方向に拡大するレイアウトにフッターを追加すると、一覧の右側にフッターが表示されます。

### <a name="display-strings-in-the-header-and-footer"></a>ヘッダーとフッターに文字列を表示する

次の例に示すように、`Header` プロパティと `Footer` プロパティを `string` 値に設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    Header = "Monkeys",
    Footer = "2019"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

このコードを実行すると、次のスクリーンショットが表示され、iOS のスクリーンショットにヘッダーが表示され、Android のスクリーンショットに示されたフッターが表示されます。

[![IOS と Android での CollectionView 文字列のヘッダーとフッターのスクリーンショット](layout-images/header-footer-string.png "CollectionView 文字列のヘッダーとフッター")](layout-images/header-footer-string-large.png#lightbox "CollectionView 文字列のヘッダーとフッター")

### <a name="display-views-in-the-header-and-footer"></a>ヘッダーとフッターにビューを表示する

`Header` プロパティと `Footer` プロパティはそれぞれ、ビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の例は、 [`Label`](xref:Xamarin.Forms.Label)オブジェクトを含む[`StackLayout`](xref:Xamarin.Forms.StackLayout)オブジェクトにそれぞれ設定されている `Header` と `Footer` プロパティを示しています。

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

同等の C# コードを次に示します。

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

このコードを実行すると、次のスクリーンショットが表示され、iOS のスクリーンショットにヘッダーが表示され、Android のスクリーンショットに示されたフッターが表示されます。

[![IOS と Android でのビューを使用した CollectionView ヘッダーとフッターのスクリーンショット](layout-images/header-footer-view.png "CollectionView ビューのヘッダーとフッター")](layout-images/header-footer-view-large.png#lightbox "CollectionView ビューのヘッダーとフッター")

### <a name="display-a-templated-header-and-footer"></a>テンプレート化されたヘッダーとフッターを表示する

`HeaderTemplate` プロパティと `FooterTemplate` プロパティは、ヘッダーとフッターの書式設定に使用される[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定できます。 このシナリオでは、次の例に示すように、`Header` プロパティと `Footer` プロパティを、適用するテンプレートの現在のソースにバインドする必要があります。

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

同等の C# コードを次に示します。

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

このコードを実行すると、次のスクリーンショットが表示され、iOS のスクリーンショットにヘッダーが表示され、Android のスクリーンショットに示されたフッターが表示されます。

[![IOS と Android でのテンプレートを使用した CollectionView ヘッダーとフッターのスクリーンショット](layout-images/header-footer-template.png "CollectionView テンプレートのヘッダーとフッター")](layout-images/header-footer-template-large.png#lightbox "CollectionView テンプレートのヘッダーとフッター")

## <a name="item-spacing"></a>項目の間隔

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の各項目には、その周囲に空の領域がありません。 この動作は、`CollectionView`によって使用される項目のレイアウトのプロパティを設定することによって変更できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)が[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを `LinearItemsLayout` オブジェクトに設定する場合、`LinearItemsLayout.ItemSpacing` プロパティは、各項目の周囲の空白を表す `double` 値に設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `LinearItemsLayout.ItemSpacing` プロパティには検証コールバックセットがあります。これにより、プロパティの値が常に0以上になるようにします。

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

このコードを実行すると、垂直方向の単一列リストが生成され、各項目の周りに20の間隔があります。

[![IOS と Android での項目の間隔を含む CollectionView のスクリーンショット](layout-images/vertical-list-spacing.png "CollectionView item の間隔")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView item の間隔")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)によって[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティが[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)オブジェクトに設定されている場合、`GridItemsLayout.VerticalItemSpacing` および `GridItemsLayout.HorizontalItemSpacing` のプロパティは、各項目の垂直方向および水平方向の空白を表す `double` 値に設定できます。

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
> `GridItemsLayout.VerticalItemSpacing` プロパティと `GridItemsLayout.HorizontalItemSpacing` プロパティには検証コールバックが設定されており、プロパティの値が常に0以上であることを確認します。

同等の C# コードを次に示します。

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

[![Android 上の項目間隔を含む CollectionView のスクリーンショット](layout-images/vertical-grid-spacing.png "CollectionView item の間隔")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView item の間隔")

## <a name="item-sizing"></a>項目のサイズ変更

既定では、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)内の UI 要素が固定サイズを指定していない場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の各項目は個別に測定およびサイズ設定されます。 この動作は変更可能ですが、 [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)のプロパティ値によって指定されます。 このプロパティ値は、 [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)列挙型のメンバーのいずれかに設定できます。

- `MeasureAllItems` –各項目は個別に測定されます。 これが既定値です。
- `MeasureFirstItem` –最初の項目のみが測定され、後続のすべての項目には最初の項目と同じサイズが割り当てられます。

> [!IMPORTANT]
> `MeasureFirstItem` のサイズ調整戦略では、項目のサイズがすべての項目にわたって一様であることを意図している場合に、パフォーマンスが向上します。

次のコード例は、 [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)プロパティの設定を示しています。

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>項目の動的なサイズ変更

[`CollectionView`](xref:Xamarin.Forms.CollectionView)内の項目は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)内の要素のレイアウトに関連するプロパティを変更することによって、実行時に動的にサイズを変更できます。 たとえば、次のコード例では、 [`Image`](xref:Xamarin.Forms.Image)オブジェクトの[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)と[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)プロパティを変更します。

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

`OnImageTapped` イベントハンドラーは、タップされる[`Image`](xref:Xamarin.Forms.Image)オブジェクトに応答して実行され、より簡単に表示されるようにイメージのサイズを変更します。

[![IOS と Android での動的な項目サイズ設定を使用した CollectionView のスクリーンショット](layout-images/runtime-resizing.png "CollectionView 動的な項目のサイズ変更")](layout-images/runtime-resizing-large.png#lightbox "CollectionView 動的な項目のサイズ変更")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティを[`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)に設定することによって、右から左へのフロー方向にコンテンツをレイアウトできます。 ただし、`FlowDirection` プロパティは、ページまたはルートレイアウトで設定することをお勧めします。これにより、ページ内のすべての要素またはルートレイアウトがフロー方向に応答します。

```xaml
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

親を持つ要素の既定の[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)は[`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)です。 したがって、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)は[`StackLayout`](xref:Xamarin.Forms.StackLayout)から `FlowDirection` プロパティ値を継承し、さらに、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)から `FlowDirection` プロパティ値を継承します。 この結果、次のスクリーン ショットに示すように、右から左のレイアウトになります。

[![IOS と Android での CollectionView 右から左方向のリストレイアウトのスクリーンショット](layout-images/vertical-list-rtl.png "CollectionView 右から左方向のリストレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左方向のリストレイアウト")

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [CollectionView のスクロール](scrolling.md)
