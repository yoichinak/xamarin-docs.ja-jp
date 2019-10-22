---
title: CollectionView データ
description: CollectionView には、ItemsSource プロパティを、IEnumerable を実装する任意のコレクションに設定することによってデータが設定されます。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: 5afdaa9afa4c5ced39498a1cb45de07fe4bf4195
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696707"
---
# <a name="xamarinforms-collectionview-data"></a>CollectionView データ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)には、表示するデータとその外観を定義する次のプロパティが含まれています。

- `IEnumerable` 型の[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)は、表示される項目のコレクションを指定します。既定値は `null` です。
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)は、表示される項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、新しい項目が追加されたときの `CollectionView` のスクロール動作を表す `ItemsUpdatingScrollMode` プロパティを定義します。 このプロパティの詳細については、「[新しい項目が追加されたときのコントロールのスクロール位置](scrolling.md#control-scroll-position-when-new-items-are-added)」を参照してください。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、ユーザーがスクロールするときにデータを徐々に読み込むこともできます。 詳細については、「[データの増分読み込み](#load-data-incrementally)」を参照してください。

## <a name="populate-a-collectionview-with-data"></a>CollectionView にデータを設定する

[@No__t_1](xref:Xamarin.Forms.CollectionView)には、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを `IEnumerable` を実装する任意のコレクションに設定することによってデータが設定されます。 項目は、文字列の配列から `ItemsSource` プロパティを初期化することによって、XAML で追加できます。

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

> [!IMPORTANT]
> 基になるコレクションで項目が追加、削除、または変更されたときに、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)を更新する必要がある場合、基になるコレクションは、`ObservableCollection` などのプロパティ変更通知を送信する `IEnumerable` コレクションである必要があります。

既定では、次のスクリーンショットに示すように、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の項目が縦の一覧に表示されます。

[![IOS と Android のテキスト項目を含む CollectionView のスクリーンショット](populate-data-images/text.png "CollectionView のテキスト項目")](populate-data-images/text-large.png#lightbox "CollectionView のテキスト項目")

[@No__t_1](xref:Xamarin.Forms.CollectionView)レイアウトを変更する方法の詳細については、「 [Xamarin CollectionView layout](layout.md)」を参照してください。 @No__t_0 内の各項目の外観を定義する方法の詳細については、「[項目の外観を定義](#define-item-appearance)する」を参照してください。

### <a name="data-binding"></a>データ バインディング

データバインディングを使用して、その[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを `IEnumerable` コレクションにバインドすることによって、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)にデータを設定できます。 XAML では、これは `Binding` マークアップ拡張機能を使用して実現されます。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

これに相当する C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例では、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティデータを接続されたビューモデルの `Monkeys` プロパティにバインドします。

> [!NOTE]
> コンパイル済みのバインドを有効にすると、Xamarin. Forms アプリケーションでのデータバインディングのパフォーマンスを向上させることができます。 詳しくは、「[コンパイル済みのバインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義する

[@No__t_1](xref:Xamarin.Forms.CollectionView)内の各項目の外観は、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義できます。

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

[@No__t_1](xref:Xamarin.Forms.DataTemplate)で指定された要素は、リスト内の各項目の外観を定義します。 この例では、`DataTemplate` 内のレイアウトは[`Grid`](xref:Xamarin.Forms.Grid)によって管理されています。 @No__t_0 には、 [`Image`](xref:Xamarin.Forms.Image)オブジェクトと、すべて `Monkey` クラスのプロパティにバインドされる2つの[`Label`](xref:Xamarin.Forms.Label)オブジェクトが含まれています。

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

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

[@No__t_1](xref:Xamarin.Forms.CollectionView)内の各項目の外観は、項目の値に基づいて実行時に選択できます。そのためには、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定します。

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

[@No__t_1](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティは `MonkeyDataTemplateSelector` オブジェクトに設定されます。 次の例は、`MonkeyDataTemplateSelector` クラスを示しています。

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

@No__t_0 クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `AmericanMonkey` と `OtherMonkey` を定義します。 @No__t_0 のオーバーライドは、`AmericanMonkey` テンプレートを返します。これにより、サル名に "America" が含まれている場合に、サルの名前と場所が青緑で表示されます。 サル名に "America" が含まれていない場合、`OnSelectTemplate` のオーバーライドは `OtherMonkey` テンプレートを返します。このテンプレートには、銀のサルの名前と場所が表示されます。

[![IOS と Android での CollectionView runtime item テンプレートの選択のスクリーンショット](populate-data-images/datatemplateselector.png "CollectionView でのランタイム項目テンプレートの選択")](populate-data-images/datatemplateselector-large.png#lightbox "CollectionView でのランタイム項目テンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

> [!IMPORTANT]
> [@No__t_1](xref:Xamarin.Forms.CollectionView)を使用する場合は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトのルート要素を `ViewCell` に設定しないでください。 これにより、`CollectionView` にセルの概念がないため、例外がスローされます。

## <a name="pull-to-refresh"></a>プルして更新

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、`RefreshView` を通じてプルを更新する機能がサポートされています。これにより、表示されているデータを、項目の一覧を取得して更新できます。 @No__t_0 は、子がスクロール可能なコンテンツをサポートしていれば、その子に対してプルを行う機能を提供するコンテナーコントロールです。 そのため、`RefreshView` の子として設定することにより、`CollectionView` の pull to refresh が実装されます。

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

ユーザーが更新を開始すると、`Command` プロパティによって定義された `ICommand` が実行されます。これにより、表示される項目が更新されます。 更新が行われている間に、更新の視覚化が表示されます。これは、アニメーションの進行状況の円で構成されます。

[![IOS と Android での CollectionView のプルから更新のスクリーンショット](populate-data-images/pull-to-refresh.png "CollectionView のプルから更新")](populate-data-images/pull-to-refresh-large.png#lightbox "CollectionView のプルから更新")

@No__t_0 プロパティの値は、`RefreshView` の現在の状態を示します。 ユーザーによって更新がトリガーされると、このプロパティは自動的に `true` に移行します。 更新が完了したら、プロパティを `false` にリセットする必要があります。

@No__t_0 の詳細については、「 [Xamarin. フォーム RefreshView](~/xamarin-forms/user-interface/refreshview.md)」を参照してください。

## <a name="load-data-incrementally"></a>データを増分読み込み

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、ユーザーが項目をスクロールすると、データの読み込みを段階的にサポートします。 これにより、ユーザーがスクロールするときに web サービスからデータページを非同期的に読み込むなどのシナリオが可能になります。 さらに、より多くのデータが読み込まれるポイントは、ユーザーが空白の領域を表示しないように、またはスクロールから停止するように構成できます。

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、次のプロパティを定義して、データの増分読み込みを制御します。

- `int` 型の `RemainingItemsThreshold`、`RemainingItemsThresholdReached` イベントが発生するリストにまだ表示されていない項目のしきい値。
- `ICommand` 型の `RemainingItemsThresholdReachedCommand`。 `RemainingItemsThreshold` に達したときに実行されます。
- `RemainingItemsThresholdReachedCommandParameter`: `object` 型、`RemainingItemsThresholdReachedCommand`に渡されるパラメーターです。

また、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) `RemainingItemsThreshold` 項目が表示されていない場合に `CollectionView` がスクロールされたときに発生する `RemainingItemsThresholdReached` イベントも定義します。 このイベントを処理して、さらに多くの項目を読み込むことができます。 さらに、`RemainingItemsThresholdReached` イベントが発生すると、`RemainingItemsThresholdReachedCommand` が実行され、増分データの読み込みがビューモデルで行われるようになります。

@No__t_0 プロパティの既定値は-1 です。これは、`RemainingItemsThresholdReached` イベントが発生しないことを示します。 プロパティ値が0の場合、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)の最後の項目が表示されるときに `RemainingItemsThresholdReached` イベントが発生します。 0より大きい値の場合、`ItemsSource` にまだスクロールされていない項目の数が含まれていると、`RemainingItemsThresholdReached` イベントが発生します。

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は `RemainingItemsThreshold` プロパティを検証して、その値が常に-1 以上になるようにします。

次の XAML の例は、データを増分読み込みする[`CollectionView`](xref:Xamarin.Forms.CollectionView)を示しています。

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

このコード例では、`RemainingItemsThresholdReached` イベントは、まだスクロールされていない項目が5つある場合に発生し、応答では `OnCollectionViewRemainingItemsThresholdReached` イベントハンドラーを実行します。

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> データは、`RemainingItemsThresholdReachedCommand` をビューモデルの `ICommand` の実装にバインドすることによって、段階的に読み込むこともできます。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. フォーム RefreshView](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
