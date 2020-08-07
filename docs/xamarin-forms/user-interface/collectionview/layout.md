---
title: Xamarin.FormsCollectionView レイアウト
description: 既定では、CollectionView は項目を縦に並べて表示します。 ただし、縦と横のリストおよびグリッドを指定することもできます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 73e7ace96c17aea2b397f2706e128ea498338b09
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918269"
---
# <a name="no-locxamarinforms-collectionview-layout"></a>Xamarin.FormsCollectionView レイアウト

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)レイアウトを制御する次のプロパティを定義します。

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)型ので、 [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout) 使用するレイアウトを指定します。
- [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)型のは、 [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) 使用される項目メジャー戦略を指定します。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

既定では、の [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目が縦の一覧に表示されます。 ただし、次のいずれかのレイアウトを使用できます。

- 縦方向の一覧–新しい項目が追加されたときに垂直方向に拡張される単一の列リスト。
- 横方向の一覧–新しい項目が追加されたときに水平方向に拡張される単一行のリストです。
- 垂直グリッド-新しいアイテムが追加されると、垂直方向に拡張される複数列のグリッドです。
- 水平グリッド-新しいアイテムが追加されると、水平方向に拡張される複数行のグリッドです。

これらのレイアウトは、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) クラスから派生するクラスにプロパティを設定することによって指定でき [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) ます。 このクラスは、次のプロパティを定義します。

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)型の。は、を [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目として追加する方向を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)(型) では、 [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) スナップポイントを項目と整列する方法を指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)型のは、 [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) スクロール時のスナップポイントの動作を指定します。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。 スナップポイントの詳細については、「 [ Xamarin.Forms CollectionView スクロール](scrolling.md)ガイド」の「[スナップポイント](scrolling.md#snap-points)」を参照してください。

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙体は、次のメンバーを定義します。

- `Vertical`項目が追加されると、が垂直方向に展開されることを示し [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。
- `Horizontal`項目が追加されると、が水平方向に展開されることを示し [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。

クラスは、 `LinearItemsLayout` クラスから継承され、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) `ItemSpacing` 各項目の周囲の空白を表す型のプロパティを定義し `double` ます。 このプロパティの既定値は0で、値は常に0以上である必要があります。 クラスは、 `LinearItemsLayout` 静的 `Vertical` メンバーとメンバーも定義し `Horizontal` ます。 これらのメンバーを使用すると、縦または横のリストをそれぞれ作成できます。 または、 `LinearItemsLayout` オブジェクトを作成し、 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 列挙型のメンバーを引数として指定することもできます。

クラスは、 [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) クラスから継承され、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) 次のプロパティを定義します。

- `VerticalItemSpacing`型の `double` 。各項目の周りの垂直方向の空白を表します。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- `HorizontalItemSpacing`型の `double` 。各項目の周りの水平方向の空白を表します。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- `Span``int`グリッドに表示する列または行の数を表す、型の。 このプロパティの既定値は1で、その値は常に1以上である必要があります。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)ネイティブレイアウトエンジンを使用してレイアウトを実行します。

## <a name="vertical-list"></a>縦方向の一覧

既定では、は、項目を縦に並べ [`CollectionView`](xref:Xamarin.Forms.CollectionView) たリストレイアウトで表示します。 このため、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) このレイアウトを使用するようにプロパティを設定する必要はありません。

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

ただし、完全を期すために、XAML では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) プロパティをに設定することにより、項目を垂直方向の一覧に表示するように設定できます [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `VerticalList` 。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

または、プロパティを [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) オブジェクトに設定し `LinearItemsLayout` 、 `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 列挙型のメンバーをプロパティ値として指定することで、これを実現することもでき `Orientation` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

この結果、1つの列リストが作成されます。新しい項目が追加されると、垂直方向に拡張されます。

[![IOS と Android の CollectionView 縦のリストレイアウトのスクリーンショット](layout-images/vertical-list.png "CollectionView 縦のリストレイアウト")](layout-images/vertical-list-large.png#lightbox "CollectionView 縦のリストレイアウト")

## <a name="horizontal-list"></a>横方向の一覧

XAML では、の [`CollectionView`](xref:Xamarin.Forms.CollectionView) プロパティをに設定することによって、項目を水平方向の一覧に表示でき [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `HorizontalList` ます。

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

または、プロパティを [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) オブジェクトに設定し `LinearItemsLayout` 、 `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) 列挙型のメンバーをプロパティ値として指定することで、このレイアウトを実現することもでき `Orientation` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

これにより、新しい項目が追加されたときに水平方向に拡張される1行のリストが生成されます。

[![IOS と Android での CollectionView 横方向リストレイアウトのスクリーンショット](layout-images/horizontal-list.png "CollectionView 横方向リストレイアウト")](layout-images/horizontal-list-large.png#lightbox "CollectionView 横方向リストレイアウト")

## <a name="vertical-grid"></a>垂直グリッド

XAML では、の [`CollectionView`](xref:Xamarin.Forms.CollectionView) プロパティをに設定することにより、項目を垂直グリッドに表示でき [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `VerticalGrid` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalGrid, 2">
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

または、プロパティを [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) プロパティがに設定されているオブジェクトに設定することによって、このレイアウトを実現することもでき `Vertical` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定では、縦のは [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 1 つの列に項目が表示されます。 ただし、この例では、プロパティを2に設定して `GridItemsLayout.Span` います。 これにより、新しい項目が追加されたときに垂直方向に拡大する2列のグリッドが生成されます。

[![IOS と Android の CollectionView 垂直グリッドレイアウトのスクリーンショット](layout-images/vertical-grid.png "CollectionView 縦グリッドレイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 縦グリッドレイアウト")

## <a name="horizontal-grid"></a>水平グリッド

XAML では、の [`CollectionView`](xref:Xamarin.Forms.CollectionView) プロパティをに設定することにより、項目を水平グリッドに表示でき [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `HorizontalGrid` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalGrid, 4">
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

または、プロパティを [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) プロパティがに設定されているオブジェクトに設定することによって、このレイアウトを実現することもでき `Horizontal` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平方向には [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) 1 行に項目が表示されます。 ただし、この例では、プロパティを4に設定して `GridItemsLayout.Span` います。 これにより、新しい項目が追加されたときに水平方向に拡大する4行のグリッドが生成されます。

[![IOS と Android の CollectionView 水平グリッドレイアウトのスクリーンショット](layout-images/horizontal-grid.png "CollectionView 水平グリッドレイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッドレイアウト")

## <a name="headers-and-footers"></a>ヘッダーとフッター

[`CollectionView`](xref:Xamarin.Forms.CollectionView)リスト内の項目と共にスクロールするヘッダーとフッターを表示できます。 ヘッダーとフッターには、文字列、ビュー、またはオブジェクトを指定でき [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ヘッダーとフッターを指定するための次のプロパティを定義します。

- `Header`型のは、 `object` リストの先頭に表示される文字列、バインド、またはビューを指定します。
- `HeaderTemplate`型のは、の [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `DataTemplate` 書式設定に使用するを指定し `Header` ます。
- `Footer`型のは、 `object` リストの末尾に表示される文字列、バインド、またはビューを指定します。
- `FooterTemplate`型のは、の [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `DataTemplate` 書式設定に使用するを指定し `Footer` ます。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

水平方向に拡張されるレイアウトにヘッダーが追加されると、左から右にヘッダーが表示されます。 同様に、水平方向に拡大するレイアウトにフッターを追加すると、一覧の右側にフッターが表示されます。

### <a name="display-strings-in-the-header-and-footer"></a>ヘッダーとフッターに文字列を表示する

`Header`プロパティと `Footer` プロパティは、次の `string` 例に示すように、値に設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

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

`Header`プロパティと `Footer` プロパティはそれぞれ、ビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の例では `Header` 、 `Footer` オブジェクトを含むオブジェクトにそれぞれ設定されているプロパティとプロパティを示して [`StackLayout`](xref:Xamarin.Forms.StackLayout) い [`Label`](xref:Xamarin.Forms.Label) ます。

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

これに相当する C# コードを次に示します。

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

`HeaderTemplate`プロパティと `FooterTemplate` プロパティは、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ヘッダーとフッターの書式設定に使用されるオブジェクトに設定できます。 このシナリオでは、 `Header` `Footer` 次の例に示すように、プロパティとプロパティを、適用するテンプレートの現在のソースにバインドする必要があります。

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

これに相当する C# コードを次に示します。

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

既定では、内の各項目の間にスペースはありません [`CollectionView`](xref:Xamarin.Forms.CollectionView) 。 この動作は、によって使用される項目のレイアウトのプロパティを設定することによって変更でき `CollectionView` ます。

が [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) プロパティをオブジェクトに設定すると、 `LinearItemsLayout` プロパティは、 `LinearItemsLayout.ItemSpacing` `double` 項目間のスペースを表す値に設定できます。

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
> プロパティには、 `LinearItemsLayout.ItemSpacing` プロパティの値が常に0以上であることを保証する検証コールバックセットがあります。

これに相当する C# コードを次に示します。

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

このコードを実行すると、項目間に20の間隔がある垂直方向の単一列リストが生成されます。

[![IOS と Android での項目の間隔を含む CollectionView のスクリーンショット](layout-images/vertical-list-spacing.png "CollectionView item の間隔")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView item の間隔")

が [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) プロパティをオブジェクトに設定すると、 [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) `GridItemsLayout.VerticalItemSpacing` プロパティとプロパティは、 `GridItemsLayout.HorizontalItemSpacing` `double` 項目間の垂直方向および水平方向の空白を表す値に設定できます。

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
> プロパティ `GridItemsLayout.VerticalItemSpacing` と `GridItemsLayout.HorizontalItemSpacing` プロパティには検証コールバックが設定されています。これにより、プロパティの値が常に0以上になるようにします。

これに相当する C# コードを次に示します。

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

このコードを実行すると、2列の垂直グリッドが表示されます。このグリッドには、項目間に20の垂直方向の間隔があり、項目間には30の水平方向の間隔があります。

[![Android 上の項目間隔を含む CollectionView のスクリーンショット](layout-images/vertical-grid-spacing.png "CollectionView item の間隔")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView item の間隔")

## <a name="item-sizing"></a>項目のサイズ変更

既定では、の各項目 [`CollectionView`](xref:Xamarin.Forms.CollectionView) は個別に測定およびサイズ設定されます。これは、内の UI 要素が固定サイズを指定しない場合に限り [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 この動作は変更可能ですが、プロパティ値によって指定され [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) ます。 このプロパティ値は、列挙体のいずれかのメンバーに設定でき [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) ます。

- `MeasureAllItems`–各項目は個別に測定されます。 これが既定値です。
- `MeasureFirstItem`–最初の項目のみが測定され、後続のすべての項目には最初の項目と同じサイズが割り当てられます。

> [!IMPORTANT]
> サイズ調整 `MeasureFirstItem` 戦略を使用すると、項目のサイズがすべての項目にわたって一様であることが意図されている場合に、パフォーマンスが向上します。

次のコード例は、プロパティの設定を示してい [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) ます。

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>項目の動的なサイズ変更

内の項目は [`CollectionView`](xref:Xamarin.Forms.CollectionView) 、内の要素のレイアウトに関連するプロパティを変更することで、実行時に動的に変更でき [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 たとえば、次のコード例では、 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) オブジェクトのプロパティとプロパティを変更し [`Image`](xref:Xamarin.Forms.Image) ます。

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

`OnImageTapped`イベントハンドラーは、タップされるオブジェクトに応答して実行され、 [`Image`](xref:Xamarin.Forms.Image) より簡単に表示されるようにイメージのサイズを変更します。

[![IOS と Android での動的な項目サイズ設定を使用した CollectionView のスクリーンショット](layout-images/runtime-resizing.png "CollectionView 動的な項目のサイズ変更")](layout-images/runtime-resizing-large.png#lightbox "CollectionView 動的な項目のサイズ変更")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、プロパティをに設定することによって、右から左へのフロー方向にコンテンツをレイアウトでき [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) ます。 ただし、 `FlowDirection` プロパティはページまたはルートレイアウトで設定することをお勧めします。これにより、ページ内のすべての要素、またはルートレイアウトがフロー方向に応答するようになります。

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

親を [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 持つ要素の既定値は [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) です。 したがって、は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) からプロパティ値を継承し、 `FlowDirection` さらにからプロパティ値を [`StackLayout`](xref:Xamarin.Forms.StackLayout) 継承し `FlowDirection` [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。 この結果、次のスクリーンショットに示されている右から左のレイアウトになります。

[![IOS と Android での CollectionView 右から左方向のリストレイアウトのスクリーンショット](layout-images/vertical-list-rtl.png "CollectionView 右から左方向のリストレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左方向のリストレイアウト")

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.FormsCollectionView スクロール](scrolling.md)
