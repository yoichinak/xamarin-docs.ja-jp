---
title: Xamarin. Forms CollectionView EmptyView
description: CollectionView では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 `EmptyView` には、文字列、ビュー、または複数のビューが使用できます。
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: c6a2a53f267a7f6764ec441944193e8c5ecd9189
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739187"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin. Forms CollectionView EmptyView

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)表示するデータがない場合にユーザーフィードバックを提供するために使用できる次のプロパティを定義します。

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)型`object`の、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが`null`のときに表示される文字列、バインディング、またはビュー、または`ItemsSource`プロパティによって指定された`null`コレクションがまたは空の場合に表示される。 既定値は `null` です。
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。指定した`EmptyView`の書式設定に使用するテンプレート。 既定値は `null` です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトでサポートされます。つまり、このプロパティはデータ バインドの対象となることを意味します。

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティを設定するための主な使用シナリオでは、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)に対するフィルター処理操作によってデータが得られず、web サービスからデータを取得しているときにユーザーフィードバックが表示される場合に、ユーザーフィードバックが表示されます。

> [!NOTE]
> プロパティ[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)は、必要に応じて、対話型コンテンツを含むビューに設定できます。

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

`ItemsSource` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null`プロパティは、プロパティがの場合、またはプロパティで指定されたコレクションが`null`または空の場合に表示される文字列に設定できます。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 次の XAML は、このシナリオの例を示しています。

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

同等のコードをC#で示します。

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

その結果、データバインドコレクションが`null`であるため、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定された文字列が表示されます。

テキストが空の[![ビューを含む CollectionView 縦の一覧のスクリーンショット](emptyview-images/null-itemssource.png "テキストを空にした CollectionView の一覧")]表示(emptyview-images/null-itemssource-large.png#lightbox "テキストを空にした CollectionView の一覧表示")

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

`ItemsSource` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null`プロパティは、プロパティがの場合、またはプロパティで指定されたコレクションが`null`または空の場合に表示されるビューに設定できます。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は`EmptyView` 、複数の子ビューを含むビューに設定されたプロパティを示しています。

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

同等のコードをC#で示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`CollectionView`](xref:Xamarin.Forms.CollectionView)がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行すると、によって表示されるコレクションは、 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 `FilterCommand` フィルター処理操作によっ[`StackLayout`](xref:Xamarin.Forms.StackLayout) [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)てデータが生成されない場合、プロパティ値としてセットが表示されます。

カスタムの空の[![ビューが表示された CollectionView の一覧のスクリーンショット (](emptyview-images/filter-multiple-views.png "カスタムの空のビューを使用した")iOS および Android CollectionView の垂直リスト)](emptyview-images/filter-multiple-views-large.png#lightbox "カスタムの空のビューを使用した CollectionView の垂直方向の一覧表示")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>データが使用できないときに、テンプレート化されたカスタム型を表示する

`null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` `ItemsSource`プロパティはカスタム型に設定できます。これは、プロパティがの場合、またはプロパティで指定されたコレクションがまたは空の場合に、そのテンプレートが表示されます。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) プロパティは、の外観を`EmptyView`定義[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)するに設定できます。 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 次の XAML は、このシナリオの例を示しています。

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

同等の C# コードに示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

型`FilterData`は、 `Filter`プロパティ、および対応[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)するを定義します。

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

プロパティは`FilterData`オブジェクトに設定され、プロパティデータ`Filter`はプロパティに[`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)バインドされます。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`CollectionView`](xref:Xamarin.Forms.CollectionView)がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行すると、によって表示されるコレクションは、 `Filter`プロパティに格納されている検索用語に対してフィルター処理されます。 `FilterCommand` フィルター処理操作によってデータ[`Label`](xref:Xamarin.Forms.Label)が生成されない場合、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティ値として設定された[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で定義されているが表示されます。

空のビューテンプレートが表示された[ ![CollectionView の一覧のスクリーンショット (](emptyview-images/emptyviewtemplate.png "空のビューテンプレートを使用した")iOS および Android CollectionView の縦の一覧)](emptyview-images/emptyviewtemplate-large.png#lightbox "空のビューテンプレートを含む CollectionView の一覧")

> [!NOTE]
> データが使用できない[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)ときに、テンプレート化されたカスタム型を表示する場合は、複数の子ビューを含むビューにプロパティを設定できます。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できない[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)ときにとして表示されるビューは[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)、 [`ContentView`](xref:Xamarin.Forms.ContentView)内のオブジェクトとして定義できます。 この`EmptyView`プロパティは、実行時に何らか`ContentView`のビジネスロジックに基づいて、特定のに設定できます。 次の XAML の例は、このシナリオの例を示しています。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

この XAML は、 [`ContentView`](xref:Xamarin.Forms.ContentView)ページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)で2つのオブジェクトを定義[`Switch`](xref:Xamarin.Forms.Switch)します。 `ContentView`オブジェクトは、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定するオブジェクトを制御します。 が切り替えられると、 `OnEmptyViewSwitchToggled`イベントハンドラーによっ`ToggleEmptyView`てメソッドが実行されます。 [`Switch`](xref:Xamarin.Forms.Switch)

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

メソッド`ToggleEmptyView`は、 `collectionView` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)プロパティの値に基づいて、オブジェクトのプロパティ[`ContentView`](xref:Xamarin.Forms.ContentView)を、に格納されている2つのオブジェクトのいずれかに設定します。 [`CollectionView`](xref:Xamarin.Forms.CollectionView)がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行すると、によって表示されるコレクションは、 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 `FilterCommand` フィルター処理操作によっ`ContentView` `EmptyView`てデータが生成されない場合、プロパティとして設定されたオブジェクトが表示されます。

空のビューをスワップ[![した CollectionView 縦の一覧のスクリーンショット (](emptyview-images/swap.png "空のビューをスワップした")iOS および Android CollectionView の縦の一覧)](emptyview-images/swap-large.png#lightbox "スワップされる空のビューを含む CollectionView 縦の一覧")

リソースディクショナリの詳細については、「 [Xamarin. フォームリソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>実行時に EmptyViewTemplate を選択する

の[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)外観は、 [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定することによって、実行時に値に基づいて選択できます。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <CollectionView ItemsSource="{Binding Monkeys}"
                        EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                        EmptyViewTemplate="{StaticResource SearchSelector}" />
    </StackLayout>
</ContentPage>
```

同等のコードをC#で示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

プロパティが[`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) `SearchTermDataTemplateSelector` [プロパティ`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)に設定され、プロパティがオブジェクトに設定されています。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行すると、によって表示されるコレクションは、 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 `FilterCommand` フィルター処理操作に[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `SearchTermDataTemplateSelector`よってデータが生成されない場合、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)オブジェクトによって選択されたがプロパティとして設定され、表示されます。

クラスの`SearchTermDataTemplateSelector`例を次に示します。

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

クラス`SearchTermTemplateSelector`は、さまざま`OtherTemplate`なデータテンプレートに設定されるプロパティと[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティを定義`DefaultTemplate`します。 上書き`OnSelectTemplate`は、 `DefaultTemplate`検索クエリが "xamarin" と等しくない場合に、ユーザーにメッセージを表示するを返します。 検索クエリが "xamarin" と等しい場合、オーバーライドは`OnSelectTemplate`を返し`OtherTemplate`ます。これにより、ユーザーに基本的なメッセージが表示されます。

[ ![CollectionView ランタイムの空のビューテンプレートの選択のスクリーンショット CollectionView での IOS および Android](emptyview-images/datatemplateselector.png "ランタイムの空の表示テンプレートの選択")](emptyview-images/datatemplateselector-large.png#lightbox "CollectionView でのランタイムの空のビューテンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. フォームリソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
