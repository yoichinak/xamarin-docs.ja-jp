---
title: Xamarin.Forms CollectionView
description: CollectionView は、別のレイアウトの仕様を使用してデータのリストを表示するための柔軟で高パフォーマンスのビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 46ab8169b31624e3862cf14f477bcd6cf8c8f3a3
ms.sourcegitcommit: 408b78dd6eded4696469e316af7922a5991f2211
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2018
ms.locfileid: "53246309"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` 別のレイアウトの仕様を使用してデータのリストを表示するためのビュー。 目的より柔軟に提供してパフォーマンスの高い代替に`ListView`します。 中に、`CollectionView`と`ListView`Api は似ています、注目すべきいくつか違いがあります。

- `CollectionView` セルの概念はありません。 代わりに、データ テンプレートを使用して、一覧のデータの各アイテムの外観を定義します。
- `CollectionView` API サーフェスを減少`ListView`します。 多くのプロパティおよびイベントから`ListView`が存在しない`CollectionView`します。
- `CollectionView` 柔軟なレイアウト モデル、データをリストまたはグリッドの垂直方向または水平方向に表示することが可能があります。

A`CollectionView`設定によって使用されるその`ItemsSource`プロパティを実装するコレクションを`IEnumerable`、およびその`ItemTemplate`プロパティを`DataTemplate`各リスト項目の外観を定義します。 さらに、その`ItemsLayout`にプロパティを設定する必要があります、`ItemsLayout`一覧については、レイアウトの仕様を定義するクラス。

> [!IMPORTANT]
> `CollectionView` 、早期プレビュー版では現在、その計画的な機能の多くが不足しています。 さらに、実装が完了すると、API は変更されます。

`CollectionView` Xamarin.Forms の 4.0 pre1 で使用できます。 ただし、現在、試験段階は、し、次のコードの行を追加することでのみ使用できます、`AppDelegate`クラスでは、iOS、または、 `MainActivity` android では、クラスを呼び出す前に`Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

## <a name="populating-a-collectionview-with-data"></a>データの CollectionView の設定

A`CollectionView`設定によってデータが読み込まれて、`ItemSource`プロパティを実装するコレクションを`IEnumerable`します。 初期化することにより XAML で項目を追加できる、`ItemsSource`項目の配列からのプロパティ。

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> なお、`x:Array`要素が必要です、`Type`配列内の項目の種類を示す属性です。

さらに、`CollectionView`にバインドするデータ バインディングを使用してデータを設定することができます、`ItemsSource`プロパティを`IEnumerable`コレクション。 XAML 内でこれは、`Binding`マークアップ拡張機能。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

この例で、`ItemsSource`プロパティ データにバインド、`Monkeys`を返す接続されているビュー モデルのプロパティを`IList<Monkey>`コレクション。 次のコード例は、`Monkey`クラスは、4 つのプロパティが含まれています。

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

## <a name="providing-a-view-for-the-empty-state"></a>空の状態のビューを提供します。

ときに、`CollectionView`表示するには任意のデータを持たないユーザーに適切なメッセージを表示できます。 これは、設定によって実現されます、`CollectionView.EmptyView`プロパティを`object`でコレクションが指定されたときに表示される、`ItemSource`プロパティが空。

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}">
    <CollectionView.EmptyView>
        <Label Text="No items to display" />
    </CollectionView.EmptyView>
</CollectionView>
```

`EmptyView`にプロパティを設定することができます、`string`バインド、または、ビューを必要な場合は、対話型コンテンツを含めることができます。

さらに、`EmptyViewTemplate`にプロパティを設定することができます、`DataTemplate`書式設定に使用される、`EmptyView`します。

## <a name="defining-list-item-appearance"></a>リスト項目の外観を定義します。

内の各項目のデータの外観、`CollectionView`を設定して定義することができます、`CollectionView.ItemTemplate`プロパティを`DataTemplate`:

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
    ...
</CollectionView>
```

この例で、`DataTemplate`が含まれています、`Grid`で、 `Image`、2 つと`Label`インスタンス、すべてのプロパティにバインドする、`Monkey`オブジェクト。

データ テンプレートの詳細については、次を参照してください。[データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)します。

## <a name="specifying-a-layout"></a>レイアウトの指定

既定で、`CollectionView`を垂直方向に一覧の項目が表示されます。 ただし、使用レイアウト仕様は次のいずれかのことができます。

- 垂直方向リスト: 新しい項目が追加されると、垂直方向に拡大する 1 つの列の一覧。
- 水平方向リスト-新しい項目が追加されると、水平方向に拡大する 1 つの行のリスト。
- 垂直グリッド – 複数列のグリッドが新しい項目が追加されると、垂直方向に大きくなります。
- 水平グリッド – 複数の行がグリッドに新しい項目が追加されると、水平方向に拡大します。

これらのレイアウトを設定して指定できます、`CollectionView.ItemsLayout`プロパティを`ItemsLayout`クラスで、`ListItemsLayout`リストのレイアウトを提供するクラスと`GridItemsLayout`グリッド レイアウトを提供するクラス。

`ItemsLayout`クラスを定義、`Orientation`型のプロパティ、`ItemsLayoutOrientation`します。 `ItemsLayoutOrientation`列挙体を定義`Vertical`と`Horizontal`メンバーの値。

`ListItemsLayout`クラスから継承、`ItemsLayout`クラスし、静的な定義`VerticalList`と`HorizontalList`メンバー。 これらのメンバーは、それぞれに垂直または水平方向のリストを作成する使用できます。 または、`ListItemsLayout`を指定するインスタンスを作成できる、`ItemsLayoutOrientation`を引数として列挙型メンバー。

`GridItemsLayout`クラスから継承、`ItemsLayout`クラスを定義しますを`Span`型のプロパティ、`int`グリッドに表示する行または列の数を表します。 既定値、`Span`プロパティは 1 であり、その値が 1 以上にある常に必要があります。

### <a name="vertical-list"></a>垂直方向の一覧

既定で、`CollectionView`垂直方向のリストのレイアウトでその項目が表示されます。 そのため、設定する必要はありません、`ItemsLayout`このレイアウトを使用するプロパティ。

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

また、これ行うこともできますを設定して、`ItemsLayout`プロパティのインスタンスを`ListItemsLayout`クラスを指定する、 `Vertical` `ItemsLayoutOrientation`を引数として列挙型のメンバー。

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

これは、結果、新しい項目が追加されると、垂直方向に拡大、1 つの列の一覧。

[![CollectionView 垂直レイアウト](collectionview-images/vertical-list.png "CollectionView 垂直方向のリストのレイアウト")](collectionview-images/vertical-list-large.png#lightbox)

### <a name="horizontal-list"></a>一覧 (横並び)

A`CollectionView`を設定して水平方向の一覧でその項目を表示できます、`ItemsLayout`プロパティを静的な`ListItemsLayout.HorizontalList`メンバー。

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

また、これ行うこともできますを設定して、`ItemsLayout`プロパティのインスタンスを`ListItemsLayout`クラスを指定する、 `Horizontal` `ItemsLayoutOrientation`を引数として列挙型のメンバー。

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

これは、新しい項目が追加されると、水平方向に拡大、1 つの行の一覧が得られます。

[![CollectionView 水平レイアウト](collectionview-images/horizontal-list.png "CollectionView 水平方向のリストのレイアウト")](collectionview-images/horizontal-list-large.png#lightbox)

### <a name="vertical-grid"></a>垂直グリッド

A`CollectionView`垂直のグリッドで設定してそのアイテムを表示できます、`ItemsLayout`プロパティを`GridItemsLayout`インスタンスを`Orientation`プロパティに設定されて`Vertical`:

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

既定で垂直`GridItemsLayout`1 つの列の項目が表示されます。 ただし、この例の設定、`GridItemsLayout.Span`プロパティを 2。 これは、新しい項目が追加されると、垂直方向に拡大、2 列のグリッドが得られます。

[![垂直グリッド レイアウトの CollectionView](collectionview-images/vertical-grid.png "CollectionView 垂直グリッド レイアウト")](collectionview-images/vertical-grid-large.png#lightbox)

### <a name="horizontal-grid"></a>水平グリッド

A`CollectionView`水平のグリッドで設定してそのアイテムを表示できます、`ItemsLayout`プロパティを`GridItemsLayout`インスタンスを`Orientation`プロパティに設定されて`Horizontal`:

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

既定では、水平`GridItemsLayout`1 つの行項目が表示されます。 ただし、この例の設定、 `GridItemsLayout.Span` 4 プロパティ。 これは、新しい項目が追加されると、水平方向に拡大、4 つの行のグリッドが得られます。

[![水平グリッド レイアウトの CollectionView](collectionview-images/horizontal-grid.png "CollectionView 水平グリッド レイアウト")](collectionview-images/horizontal-grid-large.png#lightbox)

## <a name="scrolling"></a>スクロール

`CollectionView` 要求された項目をビューにスクロールできる、`ScrollTo`メソッド。 このメソッドは、ビューに指定したインデックス位置にある項目をスクロールします。

```csharp
_collectionView.ScrollTo(12);
```

また、このメソッドのオーバー ロードは、指定した項目をスクロールして表示を使用できます。

```csharp
var viewModel = BindingContext as MonkeysViewModel;
var monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
_collectionView.ScrollTo(monkey);
```

両方のオーバー ロードを許可することを示す項目が含まれるグループ、スクロールがある完了した (開始、中央、終了) 後の項目とスクロールをアニメーション化するかどうかの正確な位置を指定する追加の引数。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
