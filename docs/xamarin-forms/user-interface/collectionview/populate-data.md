---
title: Xamarin.Formsデータの CollectionView
description: CollectionView には、ItemsSource プロパティを、IEnumerable を実装する任意のコレクションに設定することによってデータが設定されます。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/29/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e381184271d4a7bfa9872d2502d2281b1f3864bf
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84134564"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.Formsデータの CollectionView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)には、表示するデータとその外観を定義する次のプロパティが含まれています。

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)型のは、 `IEnumerable` 表示される項目のコレクションを指定します。の既定値は `null` です。
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)型のは、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 表示される項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)`ItemsUpdatingScrollMode` `CollectionView` 新しい項目が追加されたときののスクロール動作を表すプロパティを定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](scrolling.md#control-scroll-position-when-new-items-are-added)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ユーザーがスクロールするときにデータの増分仮想化をサポートします。 詳細については、「[データの増分読み込み](#load-data-incrementally)」を参照してください。

## <a name="populate-a-collectionview-with-data"></a>CollectionView にデータを設定する

には、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) プロパティをを実装する任意のコレクションに設定することにより、データが設定され `IEnumerable` ます。 項目は、 `ItemsSource` 文字列の配列からプロパティを初期化することによって、XAML で追加できます。

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
> `x:Array` 要素には、配列内の項目の型を示す `Type` 属性が必要です。

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!WARNING]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)UI スレッドからが更新された場合、は例外をスロー [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) します。

既定では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 次のスクリーンショットに示すように、によって項目が縦の一覧に表示されます。

[![IOS と Android のテキスト項目を含む CollectionView のスクリーンショット](populate-data-images/text.png "CollectionView のテキスト項目")](populate-data-images/text-large.png#lightbox "CollectionView のテキスト項目")

> [!IMPORTANT]
> 基に [`CollectionView`](xref:Xamarin.Forms.CollectionView) なるコレクションで項目が追加、削除、または変更されたときにを更新する必要がある場合、基になるコレクションは、 `IEnumerable` などのプロパティ変更通知を送信するコレクションである必要があり `ObservableCollection` ます。

レイアウトを変更する方法の詳細につい [`CollectionView`](xref:Xamarin.Forms.CollectionView) ては、「 [ Xamarin.Forms CollectionView layout](layout.md)」を参照してください。 の各項目の外観を定義する方法の詳細につい `CollectionView` ては、「[項目の外観を定義](#define-item-appearance)する」を参照してください。

### <a name="data-binding"></a>データ バインディング

[`CollectionView`](xref:Xamarin.Forms.CollectionView)データバインディングを使用してデータを設定し、そのプロパティをコレクションにバインドすることができ [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `IEnumerable` ます。 XAML では、これはマークアップ拡張機能を使用して実現され `Binding` ます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例では、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) プロパティデータは `Monkeys` 接続されたビューモデルのプロパティにバインドされます。

> [!NOTE]
> コンパイル済みバインディングは、アプリケーションでのデータバインディングのパフォーマンスを向上させるために有効にすることができ Xamarin.Forms ます。 詳しくは、「[コンパイル済みのバインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義する

の各項目の外観は、プロパティをに設定すること [`CollectionView`](xref:Xamarin.Forms.CollectionView) によって定義でき [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

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

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

「」で指定された要素は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) リスト内の各項目の外観を定義します。 この例では、内のレイアウト `DataTemplate` はによって管理されてい [`Grid`](xref:Xamarin.Forms.Grid) ます。 には、 `Grid` [`Image`](xref:Xamarin.Forms.Image) オブジェクトと、次の2つのオブジェクトが含まれてい [`Label`](xref:Xamarin.Forms.Label) ます。これらはすべて、クラスのプロパティにバインドされ `Monkey` ます。

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

次のスクリーンショットは、リスト内の各項目をテンプレートした結果を示しています。

[![IOS と Android で、各項目がテンプレート化されている CollectionView のスクリーンショット](populate-data-images/datatemplate.png "CollectionView のテンプレート項目")](populate-data-images/datatemplate-large.png#lightbox "CollectionView のテンプレート項目")

データ テンプレートの詳細については、「[Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」を参照してください。

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

の各項目の外観は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目の値に基づいて実行時に選択できます。これは、プロパティをオブジェクトに設定することによって [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 行います。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティがオブジェクトに設定されてい `MonkeyDataTemplateSelector` ます。 クラスの例を次に示し `MonkeyDataTemplateSelector` ます。

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

`MonkeyDataTemplateSelector`クラスは、 `AmericanMonkey` `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) さまざまなデータテンプレートに設定されるプロパティとプロパティを定義します。 この `OnSelectTemplate` オーバーライドは、 `AmericanMonkey` サル名に "America" が含まれている場合に、ジャングルの名前と場所を青緑で表示するテンプレートを返します。 サル名に "America" が含まれていない場合、この上書きによって `OnSelectTemplate` テンプレートが返されます `OtherMonkey` 。このテンプレートには、シルバーのサル名と場所が表示されます。

[![IOS と Android での CollectionView runtime item テンプレートの選択のスクリーンショット](populate-data-images/datatemplateselector.png "CollectionView でのランタイム項目テンプレートの選択")](populate-data-images/datatemplateselector-large.png#lightbox "CollectionView でのランタイム項目テンプレートの選択")

データテンプレートセレクターの詳細については、「 [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

> [!IMPORTANT]
> を使用する場合は [`CollectionView`](xref:Xamarin.Forms.CollectionView) 、オブジェクトのルート要素をに設定しないで [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ViewCell` ください。 これにより、にセルの概念がないため、例外がスローされ `CollectionView` ます。

## <a name="context-menus"></a>コンテキスト メニュー

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、を使用してデータ項目のコンテキストメニューをサポートしてい `SwipeView` ます。これにより、スワイプジェスチャを使用してコンテキストメニューが示されます。 `SwipeView`は、コンテンツの項目をラップし、そのコンテンツ項目のコンテキストメニュー項目を提供するコンテナーコントロールです。 そのため、 `CollectionView` `SwipeView` ラップするコンテンツを定義するを作成し、 `SwipeView` スワイプジェスチャによって公開されるコンテキストメニュー項目を作成することによって、に対してコンテキストメニューを実装します。 これを実現するには、の `SwipeView` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 各データ項目の外観を定義するのルートビューとしてを設定し `CollectionView` ます。

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <SwipeView>
                <SwipeView.LeftItems>
                    <SwipeItems>
                        <SwipeItem Text="Favorite"
                                   IconImageSource="favorite.png"
                                   BackgroundColor="LightGreen"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.FavoriteCommand}"
                                   CommandParameter="{Binding}" />
                        <SwipeItem Text="Delete"
                                   IconImageSource="delete.png"
                                   BackgroundColor="LightPink"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.DeleteCommand}"
                                   CommandParameter="{Binding}" />
                    </SwipeItems>
                </SwipeView.LeftItems>
                <Grid BackgroundColor="White"
                      Padding="10">
                    <!-- Define item appearance -->
                </Grid>
            </SwipeView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    // Define item appearance
    Grid grid = new Grid { Padding = 10, BackgroundColor = Color.White };
    // ...

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: collectionView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: collectionView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.LeftItems = new SwipeItems { favoriteSwipeItem, deleteSwipeItem };
    swipeView.Content = grid;    
    return swipeView;
});
```

この例では、 `SwipeView` コンテンツは、 [`Grid`](xref:Xamarin.Forms.Grid) 内の各項目の外観を定義するです [`CollectionView`](xref:Xamarin.Forms.CollectionView) 。 スワイプ項目は、コンテンツに対する操作を実行するために使用され `SwipeView` ます。また、コントロールが左側からスワイプされると、次の項目が明らかになります。

[![IOS と Android の CollectionView コンテキストメニュー項目のスクリーンショット](populate-data-images/swipeview.png "CollectionView と SwipeView コンテキストメニュー項目")](populate-data-images/swipeview-large.png#lightbox "CollectionView と SwipeView コンテキストメニュー項目")

`SwipeView`4つの異なるスワイプ方向をサポートします。スワイプ方向は、オブジェクトが追加される方向のコレクションによって定義され `SwipeItems` `SwipeItems` ます。 既定では、スワイプ項目はユーザーがタップしたときに実行されます。 また、スワイプ項目が実行されると、スワイプ項目が非表示に `SwipeView` なり、コンテンツが再度表示されます。 ただし、これらの動作は変更できます。

コントロールの詳細については `SwipeView` 、「 [ Xamarin.Forms SwipeView](~/xamarin-forms/user-interface/swipeview.md)」を参照してください。

## <a name="pull-to-refresh"></a>引っ張って更新

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、を使用したプルへのプルの機能がサポートされています。これにより、表示され `RefreshView` ているデータを、項目の一覧を取得して更新することができます。 は、子がスクロール可能なコンテンツをサポートしている場合に、その `RefreshView` 子に対してプルを行う機能を提供するコンテナーコントロールです。 そのため、の子として設定することにより、に対する pull to refresh が実装され `CollectionView` `RefreshView` ます。

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CollectionView ItemsSource="{Binding Animals}">
        ...
    </CollectionView>
</RefreshView>
```

これに相当する C# コードを次に示します。

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = collectionView;
// ...
```

ユーザーが更新を開始すると、 `ICommand` プロパティによって定義されたが実行され `Command` ます。これにより、表示されている項目が更新されます。 更新が行われている間に、更新の視覚化が表示されます。これは、アニメーションの進行状況の円で構成されます。

[![IOS と Android での CollectionView のプルから更新のスクリーンショット](populate-data-images/pull-to-refresh.png "CollectionView のプルから更新")](populate-data-images/pull-to-refresh-large.png#lightbox "CollectionView のプルから更新")

プロパティの値は `RefreshView.IsRefreshing` 、の現在の状態を示し `RefreshView` ます。 ユーザーによって更新がトリガーされると、このプロパティは自動的にに移行 `true` します。 更新が完了したら、プロパティをにリセットする必要があり `false` ます。

の詳細について `RefreshView` は、「 [ Xamarin.Forms refreshview](~/xamarin-forms/user-interface/refreshview.md)」を参照してください。

## <a name="load-data-incrementally"></a>データを増分読み込み

[`CollectionView`](xref:Xamarin.Forms.CollectionView)ユーザーがスクロールするときにデータの増分仮想化をサポートします。 これにより、ユーザーがスクロールするときに web サービスからデータページを非同期的に読み込むなどのシナリオが可能になります。 さらに、より多くのデータが読み込まれるポイントは、ユーザーが空白の領域を表示しないように、またはスクロールから停止するように構成できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、次のプロパティを定義して、データの増分読み込みを制御します。

- `RemainingItemsThreshold`型の、 `int` イベントが発生するリストにまだ表示されていない項目のしきい値 `RemainingItemsThresholdReached` 。
- `RemainingItemsThresholdReachedCommand`に `ICommand` 到達したときに実行される、型の `RemainingItemsThreshold` 。
- `RemainingItemsThresholdReachedCommandParameter`: `object` 型、`RemainingItemsThresholdReachedCommand` に渡されるパラメーター。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)また、は `RemainingItemsThresholdReached` 、 `CollectionView` 項目が表示されていない大きさまでスクロールしたときに発生するイベントも定義し `RemainingItemsThreshold` ます。 このイベントを処理して、さらに多くの項目を読み込むことができます。 さらに、 `RemainingItemsThresholdReached` イベントが発生すると、 `RemainingItemsThresholdReachedCommand` が実行され、増分データの読み込みがビューモデルで行われるようになります。

プロパティの既定値 `RemainingItemsThreshold` は-1 です。これは、 `RemainingItemsThresholdReached` イベントが発生しないことを示します。 プロパティ値が0の場合、 `RemainingItemsThresholdReached` の最後の項目が表示されるときにイベントが発生し [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) ます。 0より大きい値の場合、 `RemainingItemsThresholdReached` `ItemsSource` にまだスクロールされていない項目の数が含まれていると、イベントが発生します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)プロパティを検証し `RemainingItemsThreshold` て、その値が常に-1 以上であることを確認します。

次の XAML の例は、データを増分読み込みするを示してい [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

このコード例では、 `RemainingItemsThresholdReached` まだスクロールされていない項目が5つある場合にイベントが発生し、応答ではイベントハンドラーが実行され `OnCollectionViewRemainingItemsThresholdReached` ます。

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> をビューモデルの実装にバインドすることによって、データを徐々に読み込むこともでき `RemainingItemsThresholdReachedCommand` `ICommand` ます。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.FormsRefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.FormsSwipeView](~/xamarin-forms/user-interface/swipeview.md)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Formsデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
