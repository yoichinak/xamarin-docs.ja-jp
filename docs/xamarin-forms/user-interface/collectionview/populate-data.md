---
title: CollectionView データ
description: CollectionView は、ItemsSource プロパティに IEnumerable を実装した任意のコレクションを設定することで、データを設定します。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: ce745109ea2852b597de3a8a5922a171ad83e289
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738915"
---
# <a name="xamarinforms-collectionview-data"></a>CollectionView データ

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)表示するデータとその外観を定義する次のプロパティを定義します。

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)型`IEnumerable`のは、表示される項目のコレクションを指定します。の`null`既定値はです。
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)のは、表示される項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

## <a name="populate-a-collectionview-with-data"></a>CollectionView にデータを設定する

に[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティをを実装`IEnumerable`する任意のコレクションに設定することにより、データが設定されます。 項目は任意の文字配列からの `ItemsSource` プロパティを初期化することにより、XAML で追加できます。

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
> 基になるコレクションで項目が追加、削除、または変更されたときにを更新する必要`IEnumerable` `ObservableCollection`[がある場合、基になるコレクションは、などのプロパティ変更通知を送信するコレクションである必要`CollectionView`](xref:Xamarin.Forms.CollectionView)があります。

既定では[`CollectionView`](xref:Xamarin.Forms.CollectionView) 、次のスクリーンショットに示すように、によって項目が縦の一覧に表示されます。

[![IOS と Android でのテキスト アイテムを格納している CollectionView のスクリーン ショット](populate-data-images/text.png "collectionview テキスト アイテム")](populate-data-images/text-large.png#lightbox "collectionview テキスト アイテム")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)レイアウトを変更する方法の詳細については、「[レイアウトを指定](layout.md)する」を参照してください。 `CollectionView` 内の各項目の外観を定義する方法については、[項目の外観の定義](#define-item-appearance) を参照してください。

### <a name="data-binding"></a>データ バインディング

[`CollectionView`](xref:Xamarin.Forms.CollectionView)データバインディングを使用してデータを設定し、その[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティを`IEnumerable`コレクションにバインドすることができます。 XAML では、これは `Binding` マークアップ拡張を使って実現します。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例では、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティデータは接続さ`Monkeys`れたビューモデルのプロパティにバインドされます。

> [!NOTE]
> Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させるために、コンパイル済みのバインドを有効にすることができます。 詳細については、[コンパイル済みバインディング](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md) を参照してください。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義する

の[`CollectionView`](xref:Xamarin.Forms.CollectionView)各項目の外観は、 [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)プロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)をに設定することによって定義できます。

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

## <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

の[`CollectionView`](xref:Xamarin.Forms.CollectionView)各項目の外観は、項目の値に基づいて実行時に選択できます。これは[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 、プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定することによって行います。

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

プロパティ[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)が`MonkeyDataTemplateSelector`オブジェクトに設定されています。 クラスの`MonkeyDataTemplateSelector`例を次に示します。

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

クラス`MonkeyDataTemplateSelector`は、さまざま`OtherMonkey`なデータテンプレートに設定されるプロパティと[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティを定義`AmericanMonkey`します。 この`OnSelectTemplate`オーバーライドは、 `AmericanMonkey`サル名に "America" が含まれている場合に、ジャングルの名前と場所を青緑で表示するテンプレートを返します。 サル名に "America" が含まれてい`OnSelectTemplate`ない場合、 `OtherMonkey`この上書きによってテンプレートが返されます。このテンプレートには、シルバーのサル名と場所が表示されます。

[CollectionView での iOS および Android(populate-data-images/datatemplateselector.png "ランタイム項目テンプレート選択")![の CollectionView ランタイム項目テンプレートの選択のスクリーンショット]](populate-data-images/datatemplateselector-large.png#lightbox "CollectionView でのランタイム項目テンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

> [!IMPORTANT]
> を使用[`CollectionView`](xref:Xamarin.Forms.CollectionView)する場合は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクト`ViewCell`のルート要素をに設定しないでください。 これにより、にセルの概念が`CollectionView`ないため、例外がスローされます。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
