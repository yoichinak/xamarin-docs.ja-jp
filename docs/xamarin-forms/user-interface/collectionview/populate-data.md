---
title: Xamarin.Forms CollectionView データ
description: CollectionView は、ItemsSource プロパティに IEnumerable を実装した任意のコレクションを設定することで、データを設定します。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 1350d5a5a0845029b7ef6a06647ad4c56f0f8135
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048275"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.Forms CollectionView データ

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` には、表示するデータとその外観を定義する以下のプロパティが定義されています。

- `ItemsSource`: `IEnumerable` 型で、表示される項目のコレクションを指定します。既定値は `null` です。
- `ItemTemplate`: [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 型で、表示する項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトによりサポートされます。つまりデータバインディングの対象となる可能性があるという意味です。

## <a name="populate-a-collectionview-with-data"></a>CollectionView にデータを設定する

`CollectionView` には、`ItemsSource` プロパティに `IEnumerable` を実装した任意のコレクションを設定することでデータが設定されます。 項目は任意の文字配列からの `ItemsSource` プロパティを初期化することにより、XAML で追加できます。

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

同等の C# コードは以下のとおりです。

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
> `CollectionView` が、基になるコレクションでの項目の追加、削除、変更時に更新する必要がある場合には、基になるコレクションは、 `ObservableCollection` などのプロパティ変更通知を送信する `IEnumerable` コレクションである必要があります。

既定では、`CollectionView` は、次のスクリーン ショットに示すように垂直方向に一覧項目を表示します。

[![IOS と Android でのテキスト アイテムを格納している CollectionView のスクリーン ショット](populate-data-images/text.png "collectionview テキスト アイテム")](populate-data-images/text-large.png#lightbox "collectionview テキスト アイテム")

`CollectionView` のレイアウトを変更する方法については、[レイアウトの指定](layout.md) を参照してください。 `CollectionView` 内の各項目の外観を定義する方法については、[項目の外観の定義](#define-item-appearance) を参照してください。

### <a name="data-binding"></a>データ バインディング

`CollectionView` は、`ItemsSource` プロパティを `IEnumerable` コレクションにバインドするデータ バインディングを使用してデータを設定することができます。 XAML では、これは `Binding` マークアップ拡張を使って実現します。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

同等の C# コードは以下のとおりです。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例では、`ItemsSource` プロパティのデータは、接続されているビュー モデルの `Monkeys` プロパティにバインドされます。

> [!NOTE]
> Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させるために、コンパイル済みのバインドを有効にすることができます。 詳細については、[コンパイル済みバインディング](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md) を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義する

`CollectionView` 内の各項目の外観は、`CollectionView.ItemTemplate` プロパティを [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) に設定して定義することができます。

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

同等の C# コードは以下のとおりです。

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) に指定された要素により、リスト内の各項目の外観は定義されます。 サンプルでは、`DataTemplate` 内のレイアウトは [ `Grid`](xref:Xamarin.Forms.Grid) により管理されています。 `Grid` は、[ `Image` ](xref:Xamarin.Forms.Image) オブジェクトと 2 つの [ `Label` ](xref:Xamarin.Forms.Label) オブジェクトを含み、そのすべてが `Monkey` クラスのプロパティにバインドされています。

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

次のスクリーン ショットは、リストの各項目をテンプレートに展開した結果を示しています。

[![iOS と Android で、各項目をテンプレート化した CollectionView のスクリーンショット](populate-data-images/datatemplate.png "collectionview テンプレート化された項目")](populate-data-images/datatemplate-large.png#lightbox "collectionview テンプレート化された項目")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択します。

内の各項目の外観、`CollectionView`を設定して、項目の値に基づいて、実行時に選択することができます、`CollectionView.ItemTemplate`プロパティを[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)オブジェクト。

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

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

`ItemTemplate`プロパティに設定されて、`MonkeyDataTemplateSelector`オブジェクト。 次の例は、`MonkeyDataTemplateSelector`クラス。

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

`MonkeyDataTemplateSelector`クラス定義`AmericanMonkey`と`OtherMonkey` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)さまざまなデータ テンプレートに設定されているプロパティ。 `OnSelectTemplate`のオーバーライド、`AmericanMonkey`テンプレート、monkey の名前に"America"が含まれている場合に、青緑に monkey の名前と場所を表示します。 Monkey の名前に"America"が含まれていない場合、`OnSelectTemplate`のオーバーライド、`OtherMonkey`テンプレートで、silver に monkey の名前と場所を表示します。

[![スクリーン ショットの CollectionView ランタイム項目テンプレートの選択、iOS と Android で](populate-data-images/datatemplateselector.png "collectionview ランタイム項目テンプレートの選択")](populate-data-images/datatemplateselector-large.png#lightbox "ランタイムで項目テンプレートの選択、CollectionView")

データ テンプレート セレクターの詳細については、次を参照してください。[作成 Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)します。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms DataTemplateSelector を作成します。](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
