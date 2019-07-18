---
title: Xamarin.Forms CollectionView データ
description: CollectionView は、ItemsSource プロパティに IEnumerable を実装した任意のコレクションを設定することで、データを設定します。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: d2729250c0f991564ae70ddf6a15b40425ed6c46
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005278"
---
# <a name="xamarinforms-collectionview-data"></a>Xamarin.Forms CollectionView データ

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 表示されるデータを定義する次のプロパティとその外観を定義します。

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)、型の`IEnumerable`、表示される項目のコレクションを指定の既定値を持つと`null`します。
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)、型の[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)、表示する項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

## <a name="populate-a-collectionview-with-data"></a>CollectionView にデータを設定する

A [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)設定によってデータが読み込まれて、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを実装するコレクションを`IEnumerable`します。 項目は任意の文字配列からの `ItemsSource` プロパティを初期化することにより、XAML で追加できます。

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

同等のコードをC#で示します。

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
> 場合、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)は項目の追加、削除、または基になるコレクションで変更されても更新するために必要な基になるコレクションである必要があります、`IEnumerable`プロパティを送信するコレクションの変更通知、`ObservableCollection`.

既定では、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)次のスクリーン ショットに示すように垂直方向に一覧項目が表示されます。

[![IOS と Android でのテキスト アイテムを格納している CollectionView のスクリーン ショット](populate-data-images/text.png "collectionview テキスト アイテム")](populate-data-images/text-large.png#lightbox "collectionview テキスト アイテム")

変更する方法については、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)レイアウトを参照してください[レイアウトを指定](layout.md)します。 `CollectionView` 内の各項目の外観を定義する方法については、[項目の外観の定義](#define-item-appearance) を参照してください。

### <a name="data-binding"></a>データ バインディング

[`CollectionView`](xref:Xamarin.Forms.CollectionView) バインドするデータ バインディングを使用してデータを設定することができます、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを`IEnumerable`コレクション。 XAML では、これは `Binding` マークアップ拡張を使って実現します。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例で、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティ データにバインド、`Monkeys`接続されているビュー モデルのプロパティ。

> [!NOTE]
> Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させるために、コンパイル済みのバインドを有効にすることができます。 詳細については、[コンパイル済みバインディング](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md) を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義する

内の各項目の外観、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)を設定して定義することができます、 [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

同等のコードをC#で示します。

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

内の各項目の外観、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)を設定して、項目の値に基づいて、実行時に選択することができます、 [ `CollectionView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティを[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)オブジェクト。

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

[ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティに設定されて、`MonkeyDataTemplateSelector`オブジェクト。 次の例は、`MonkeyDataTemplateSelector`クラス。

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

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms DataTemplateSelector を作成します。](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
