---
title: CollectionView レイアウト
description: 既定では、CollectionView は項目を縦に並べて表示します。 ただし、縦と横のリストおよびグリッドを指定することもできます。
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2019
ms.openlocfilehash: 0b64583f2bd17ddecac66778066406f81c7e7800
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517610"
---
# <a name="xamarinforms-collectionview-layout"></a>CollectionView レイアウト

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)レイアウトを制御する次のプロパティを定義します。

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)型[`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout)ので、使用するレイアウトを指定します。
- [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)型[`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)のは、使用される項目メジャー戦略を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の項目が縦の一覧に表示されます。 ただし、次のいずれかのレイアウトを使用できます。

- 縦方向の一覧–新しい項目が追加されたときに垂直方向に拡張される単一の列リスト。
- 横方向の一覧–新しい項目が追加されたときに水平方向に拡張される単一行のリストです。
- 垂直グリッド-新しいアイテムが追加されると、垂直方向に拡張される複数列のグリッドです。
- 水平グリッド-新しいアイテムが追加されると、水平方向に拡張される複数行のグリッドです。

これらのレイアウトは、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから派生するクラスにプロパティを設定することによって指定できます。 このクラスは、次のプロパティを定義します。

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)型[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)の。は、を項目として[`CollectionView`](xref:Xamarin.Forms.CollectionView)追加する方向を指定します。
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)(型[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)) では、スナップポイントを項目と整列する方法を指定します。
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)型[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)のは、スクロール時のスナップポイントの動作を指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。 スナップポイントの詳細については、「 [CollectionView スクロール](scrolling.md)ガイド」の「[スナップポイント](scrolling.md#snap-points)」を参照してください。

列挙[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)体は、次のメンバーを定義します。

- `Vertical`項目が追加[`CollectionView`](xref:Xamarin.Forms.CollectionView)されると、が垂直方向に展開されることを示します。
- `Horizontal`項目が追加[`CollectionView`](xref:Xamarin.Forms.CollectionView)されると、が水平方向に展開されることを示します。

クラス`LinearItemsLayout`は、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承され、各`ItemSpacing`項目の周囲の`double`空白を表す型のプロパティを定義します。 このプロパティの既定値は0で、値は常に0以上である必要があります。 クラス`LinearItemsLayout`は、静的`Vertical`メンバーと`Horizontal`メンバーも定義します。 これらのメンバーを使用すると、縦または横のリストをそれぞれ作成できます。 または、 `LinearItemsLayout`オブジェクトを作成し、 [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙型のメンバーを引数として指定することもできます。

クラス[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)は、 [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)クラスから継承され、次のプロパティを定義します。

- `VerticalItemSpacing`型`double`の。各項目の周りの垂直方向の空白を表します。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- `HorizontalItemSpacing`型`double`の。各項目の周りの水平方向の空白を表します。 このプロパティの既定値は0で、値は常に0以上である必要があります。
- `Span`グリッドに表示`int`する列または行の数を表す、型の。 このプロパティの既定値は1で、その値は常に1以上である必要があります。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)ネイティブレイアウトエンジンを使用してレイアウトを実行します。

## <a name="vertical-list"></a>縦方向の一覧

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、は、項目を縦に並べたリストレイアウトで表示します。 このため、このレイアウトを使用するよう[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)にプロパティを設定する必要はありません。

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

ただし、完全を期すため[`CollectionView`](xref:Xamarin.Forms.CollectionView)に、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティをに設定する`VerticalList`ことによって、項目を縦に並べて表示するようにを設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

または[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) 、プロパティを`LinearItemsLayout`オブジェクトに設定し、 `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙型のメンバーを`Orientation`プロパティ値として指定することで、これを実現することもできます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

該当の C# コードを次に示します。

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

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティをに設定することにより、項目`HorizontalList`を横方向の一覧に表示できます。

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

または[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) 、プロパティを`LinearItemsLayout`オブジェクトに設定し、 `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)列挙型のメンバーを`Orientation`プロパティ値として指定することで、このレイアウトを実現することもできます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

該当の C# コードを次に示します。

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

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティがに[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) `Vertical`設定されているオブジェクトにプロパティを設定することにより、項目を垂直グリッドに表示できます。

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

該当の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

既定では、縦[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)のは1つの列に項目が表示されます。 ただし、この例では`GridItemsLayout.Span` 、プロパティを2に設定しています。 これにより、新しい項目が追加されたときに垂直方向に拡大する2列のグリッドが生成されます。

[![IOS と Android の CollectionView 垂直グリッドレイアウトのスクリーンショット](layout-images/vertical-grid.png "CollectionView 縦グリッドレイアウト")](layout-images/vertical-grid-large.png#lightbox "CollectionView 縦グリッドレイアウト")

## <a name="horizontal-grid"></a>水平グリッド

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティがに[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) `Horizontal`設定されているオブジェクトにプロパティを設定することにより、項目を水平グリッドに表示できます。

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

該当の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

既定では、水平[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)方向には1行に項目が表示されます。 ただし、この例では`GridItemsLayout.Span` 、プロパティを4に設定しています。 これにより、新しい項目が追加されたときに水平方向に拡大する4行のグリッドが生成されます。

[![IOS と Android の CollectionView 水平グリッドレイアウトのスクリーンショット](layout-images/horizontal-grid.png "CollectionView 水平グリッドレイアウト")](layout-images/horizontal-grid-large.png#lightbox "CollectionView 水平グリッドレイアウト")

## <a name="headers-and-footers"></a>ヘッダーとフッター

[`CollectionView`](xref:Xamarin.Forms.CollectionView)リスト内の項目と共にスクロールするヘッダーとフッターを表示できます。 ヘッダーとフッターには、文字列、ビュー、また[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)はオブジェクトを指定できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ヘッダーとフッターを指定するための次のプロパティを定義します。

- `Header`型`object`のは、リストの先頭に表示される文字列、バインド、またはビューを指定します。
- `HeaderTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のは、の書式`DataTemplate`設定に使用するを`Header`指定します。
- `Footer`型`object`のは、リストの末尾に表示される文字列、バインド、またはビューを指定します。
- `FooterTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のは、の書式`DataTemplate`設定に使用するを`Footer`指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

水平方向に拡張されるレイアウトにヘッダーが追加されると、左から右にヘッダーが表示されます。 同様に、水平方向に拡大するレイアウトにフッターを追加すると、一覧の右側にフッターが表示されます。

### <a name="display-strings-in-the-header-and-footer"></a>ヘッダーとフッターに文字列を表示する

プロパティ`Header`と`Footer`プロパティは、次の`string`例に示すように、値に設定できます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

該当の C# コードを次に示します。

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

プロパティ`Header`と`Footer`プロパティはそれぞれ、ビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の例では`Header` 、 `Footer` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label)オブジェクトを含むオブジェクトにそれぞれ設定されているプロパティとプロパティを示しています。

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

該当の C# コードを次に示します。

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

プロパティ`HeaderTemplate`と`FooterTemplate`プロパティは、ヘッダーと[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)フッターの書式設定に使用されるオブジェクトに設定できます。 このシナリオでは、 `Header`次`Footer`の例に示すように、プロパティとプロパティを、適用するテンプレートの現在のソースにバインドする必要があります。

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

該当の C# コードを次に示します。

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

既定では、内の各項目の間にスペース[`CollectionView`](xref:Xamarin.Forms.CollectionView)はありません。 この動作は、 `CollectionView`によって使用される項目のレイアウトのプロパティを設定することによって変更できます。

が[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを`LinearItemsLayout`オブジェクトに設定すると、プロパティ`LinearItemsLayout.ItemSpacing`は、項目間のスペース`double`を表す値に設定できます。

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
> `LinearItemsLayout.ItemSpacing`プロパティには、プロパティの値が常に0以上であることを保証する検証コールバックセットがあります。

該当の C# コードを次に示します。

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

[`CollectionView`](xref:Xamarin.Forms.CollectionView)が[`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout)プロパティを[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)オブジェクトに設定すると、プロパティ`GridItemsLayout.VerticalItemSpacing`と`GridItemsLayout.HorizontalItemSpacing`プロパティは、項目間`double`の垂直方向および水平方向の空白を表す値に設定できます。

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
> プロパティ`GridItemsLayout.VerticalItemSpacing`と`GridItemsLayout.HorizontalItemSpacing`プロパティには検証コールバックが設定されています。これにより、プロパティの値が常に0以上になるようにします。

該当の C# コードを次に示します。

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

既定では、の各項目[`CollectionView`](xref:Xamarin.Forms.CollectionView)は個別に測定およびサイズ設定されます。これは[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 、内の UI 要素が固定サイズを指定しない場合に限ります。 この動作は変更可能ですが、 [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)プロパティ値によって指定されます。 このプロパティ値は、 [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)列挙体のいずれかのメンバーに設定できます。

- `MeasureAllItems`–各項目は個別に測定されます。 これが既定値です。
- `MeasureFirstItem`–最初の項目のみが測定され、後続のすべての項目には最初の項目と同じサイズが割り当てられます。

> [!IMPORTANT]
> サイズ`MeasureFirstItem`調整戦略を使用すると、項目のサイズがすべての項目にわたって一様であることが意図されている場合に、パフォーマンスが向上します。

次のコード例は、プロパティ[`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)の設定を示しています。

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

該当の C# コードを次に示します。

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

`OnImageTapped`イベントハンドラーは、タップされる[`Image`](xref:Xamarin.Forms.Image)オブジェクトに応答して実行され、より簡単に表示されるようにイメージのサイズを変更します。

[![IOS と Android での動的な項目サイズ設定を使用した CollectionView のスクリーンショット](layout-images/runtime-resizing.png "CollectionView 動的な項目のサイズ変更")](layout-images/runtime-resizing-large.png#lightbox "CollectionView 動的な項目のサイズ変更")

## <a name="right-to-left-layout"></a>右から左へのレイアウト

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)プロパティをに設定する[`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)ことによって、右から左へのフロー方向にコンテンツをレイアウトできます。 ただし、プロパティ`FlowDirection`はページまたはルートレイアウトで設定することをお勧めします。これにより、ページ内のすべての要素、またはルートレイアウトがフロー方向に応答するようになります。

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

親を[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)持つ要素の既定値は[`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)です。 したがって、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 `FlowDirection`から[`StackLayout`](xref:Xamarin.Forms.StackLayout)プロパティ値を継承し、さらにから`FlowDirection`プロパティ値を継承[`ContentPage`](xref:Xamarin.Forms.ContentPage)します。 この結果、次のスクリーンショットに示されている右から左のレイアウトになります。

[![IOS と Android での CollectionView 右から左方向のリストレイアウトのスクリーンショット](layout-images/vertical-list-rtl.png "CollectionView 右から左方向のリストレイアウト")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView 右から左方向のリストレイアウト")

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [CollectionView のスクロール](scrolling.md)
