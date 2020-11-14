---
title: Xamarin.Forms CarouselView EmptyView
description: CarouselView では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。
ms.prod: xamarin
ms.assetid: C6DEE1A9-63FC-4889-BC77-F401D5D7DF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5aa20e29e22df9de1d6cdd6208e56fa2e0fe0325
ms.sourcegitcommit: f2942b518f51317acbb263be5bc0c91e66239f50
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94590351"
---
# <a name="no-locxamarinforms-carouselview-emptyview"></a>Xamarin.Forms CarouselView EmptyView

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) 表示するデータがない場合にユーザーフィードバックを提供するために使用できる次のプロパティを定義します。

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)型の、 `object` プロパティがのときに表示される文字列、バインディング、またはビュー、または [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` プロパティによって指定されたコレクション `ItemsSource` が `null` または空の場合に表示される。 既定値は `null` です。
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)型の [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。指定したの書式設定に使用するテンプレート `EmptyView` 。 既定値は `null` です。

これらのプロパティは、オブジェクトによって支えられてい [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。これは、プロパティをデータバインディングのターゲットにできることを意味します。

プロパティを設定するための主な使用シナリオで [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) は、に対するフィルター処理操作によってデータが得られ [`CarouselView`](xref:Xamarin.Forms.CarouselView) ず、web サービスからデータを取得しているときにユーザーフィードバックが表示される場合に、ユーザーフィードバックが表示されます。

> [!NOTE]
> プロパティは、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 必要に応じて、対話型コンテンツを含むビューに設定できます。

データ テンプレートの詳細については、「[Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」を参照してください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

プロパティは、プロパティ [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) がの場合 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 、またはプロパティ `null` で指定されたコレクション `ItemsSource` がまたは空の場合に表示される文字列に設定でき `null` ます。 次の XAML は、このシナリオの例を示しています。

```xaml
<CarouselView ItemsSource="{Binding EmptyMonkeys}"
              EmptyView="No items to display." />
```

これに相当する C# コードを次に示します。

```csharp
CarouselView carouselView = new CarouselView
{
    EmptyView = "No items to display."
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

その結果、データバインドコレクションがであるため、 `null` プロパティ値として設定された文字列が [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 表示されます。

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

プロパティは、プロパティ [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) がの場合 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 、またはプロパティ `null` で指定されたコレクション `ItemsSource` がまたは空の場合に表示されるビューに設定でき `null` ます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は、 `EmptyView` 複数の子ビューを含むビューに設定されたプロパティを示しています。

```xaml
<StackLayout Margin="20">
    <SearchBar SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <ContentView>
                <StackLayout HorizontalOptions="CenterAndExpand"
                             VerticalOptions="CenterAndExpand">
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
        </CarouselView.EmptyView>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

この例では、冗長のようなもの [`ContentView`](xref:Xamarin.Forms) がのルート要素として追加されてい [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) ます。 これは、内部的には、 `EmptyView` レイアウトのコンテキストを提供しないネイティブコンテナーにが追加されるためです Xamarin.Forms 。 したがって、を構成するビューを配置するには、ルートレイアウトを `EmptyView` 追加する必要があります。その子は、ルートレイアウト内に配置できるレイアウトです。

これに相当する C# コードを次に示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new ContentView
    {
        Content = new StackLayout
        {
            Children =
            {
                new Label { Text = "No results matched your filter.", ... },
                new Label { Text = "Try a broader filter?", ... }
            }
        }
    }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

がを実行すると、に [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` よって表示されるコレクション [`CarouselView`](xref:Xamarin.Forms.CarouselView) は、プロパティに格納されている検索用語に対してフィルター処理され [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) ます。 フィルター処理操作によってデータが生成されない場合は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) プロパティ値として設定 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) されます。

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>データが使用できないときに、テンプレート化されたカスタム型を表示する

プロパティは [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) カスタム型に設定できます。これは、プロパティがの場合 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` 、またはプロパティで指定されたコレクション `ItemsSource` がまたは空の場合に、そのテンプレートが表示され `null` ます。 プロパティは、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) の外観を定義するに設定でき `EmptyView` ます。 次の XAML は、このシナリオの例を示しています。

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <controls:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CarouselView.EmptyView>
        <CarouselView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CarouselView.EmptyViewTemplate>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

これに相当する C# コードを次に示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

型は、 `FilterData` `Filter` プロパティ、および対応するを定義し [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。

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

プロパティ [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) はオブジェクトに設定され、プロパティ `FilterData` データはプロパティ `Filter` にバインドされ [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) ます。 がを実行すると、に [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` よって表示されるコレクション [`CarouselView`](xref:Xamarin.Forms.CarouselView) は、プロパティに格納されている検索用語に対してフィルター処理され `Filter` ます。 フィルター処理操作によってデータが生成されない場合、 [`Label`](xref:Xamarin.Forms.Label) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) プロパティ値として設定されたで定義されているが [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 表示されます。

> [!NOTE]
> データが使用できないときに、テンプレート化されたカスタム型を表示する場合は、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 複数の子ビューを含むビューにプロパティを設定できます。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できないときにとして表示されるビュー [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) は、 [`ContentView`](xref:Xamarin.Forms.ContentView) 内のオブジェクトとして定義でき [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 この `EmptyView` プロパティは、 `ContentView` 実行時に何らかのビジネスロジックに基づいて、特定のに設定できます。 次の XAML の例は、このシナリオの例を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:viewmodels="clr-namespace:CarouselViewDemos.ViewModels"
             x:Class="CarouselViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.BindingContext>
        <viewmodels:MonkeysViewModel />
    </ContentPage.BindingContext>
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
        <SearchBar SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CarouselView x:Name="carouselView"
                      ItemsSource="{Binding Monkeys}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

この XAML は、ページレベルで2つのオブジェクトを定義し [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Switch`](xref:Xamarin.Forms.Switch) ます。オブジェクトは、 `ContentView` プロパティ値として設定するオブジェクトを制御し [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) ます。 [`Switch`](xref:Xamarin.Forms.Switch)が切り替えられると、 `OnEmptyViewSwitchToggled` イベントハンドラーによってメソッドが実行され `ToggleEmptyView` ます。

```csharp
void ToggleEmptyView(bool isToggled)
{
    carouselView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

メソッドは、 `ToggleEmptyView` [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) `carouselView` プロパティの値に基づいて、オブジェクトのプロパティを [`ContentView`](xref:Xamarin.Forms.ContentView) 、に格納されている2つのオブジェクトのいずれかに設定し [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) ます。 がを実行すると、に [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` よって表示されるコレクション [`CarouselView`](xref:Xamarin.Forms.CarouselView) は、プロパティに格納されている検索用語に対してフィルター処理され [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) ます。 フィルター処理操作によってデータが生成されない場合は、 `ContentView` プロパティとして設定されたオブジェクト `EmptyView` が表示されます。

リソースディクショナリの詳細については、「 [ Xamarin.Forms リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>実行時に EmptyViewTemplate を選択する

の外観は、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) プロパティをオブジェクトに設定することによって、実行時に値に基づいて選択でき [`CarouselView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) ます。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls">
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
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <CarouselView ItemsSource="{Binding Monkeys}"
                      EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                      EmptyViewTemplate="{StaticResource SearchSelector}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView()
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

プロパティが [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) プロパティに設定され、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) プロパティがオブジェクトに設定されてい `SearchTermDataTemplateSelector` ます。

がを実行すると、に [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` よって表示されるコレクション [`CarouselView`](xref:Xamarin.Forms.CarouselView) は、プロパティに格納されている検索用語に対してフィルター処理され [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) ます。 フィルター処理操作によってデータが生成されない場合、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) オブジェクトによって選択されたがプロパティとし `SearchTermDataTemplateSelector` て設定さ [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) れ、表示されます。

クラスの例を次に示し `SearchTermDataTemplateSelector` ます。

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

`SearchTermTemplateSelector`クラスは、 `DefaultTemplate` `OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) さまざまなデータテンプレートに設定されるプロパティとプロパティを定義します。 `OnSelectTemplate`上書きは `DefaultTemplate` 、検索クエリが "xamarin" と等しくない場合に、ユーザーにメッセージを表示するを返します。 検索クエリが "xamarin" と等しい場合、オーバーライドはを `OnSelectTemplate` 返し `OtherTemplate` ます。これにより、ユーザーに基本メッセージが表示されます。

データテンプレートセレクターの詳細については、「 [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.Forms データテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms のリソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [DataTemplateSelector を作成する Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
