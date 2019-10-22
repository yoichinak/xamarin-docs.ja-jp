---
title: Xamarin. Forms CarouselView EmptyView
description: CarouselView では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。
ms.prod: xamarin
ms.assetid: C6DEE1A9-63FC-4889-BC77-F401D5D7DF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2019
ms.openlocfilehash: 55944b422495c9c3a7c93c6a2eab90a2db790780
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697843"
---
# <a name="xamarinforms-carouselview-emptyview"></a>Xamarin. Forms CarouselView EmptyView

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)は、表示するデータがない場合にユーザーフィードバックを提供するために使用できる次のプロパティを定義します。

- 型 `object` の[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null` したときに表示される文字列、バインディング、またはビュー、または `ItemsSource` プロパティによって指定されたコレクションが `null` または空の場合に表示されます。 既定値は `null`です。
- 指定した `EmptyView` の書式設定に使用するテンプレート[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)。 既定値は `null`です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

[@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティを設定するための主な使用シナリオでは、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)でのフィルター処理操作によってデータが得られず、web サービスからデータを取得しているときにユーザーフィードバックが表示される場合に、ユーザーからのフィードバックが表示されます。

> [!NOTE]
> [@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、必要に応じて、対話型コンテンツを含むビューに設定できます。

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

[@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null` とき、または `ItemsSource` プロパティによって指定されたコレクションが `null` または空のときに表示される文字列に設定できます。 次の XAML は、このシナリオの例を示しています。

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

その結果、データバインドコレクションが `null` ため、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定された文字列が表示されます。

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

[@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null` とき、または `ItemsSource` プロパティによって指定されたコレクションが `null` または空のときに表示されるビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は、複数の子ビューを含むビューに設定された `EmptyView` プロパティを示しています。

```xaml
<StackLayout Margin="20">
    <SearchBar SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
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
        </CarouselView.EmptyView>
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
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[@No__t_1](xref:Xamarin.Forms.SearchBar)が `FilterCommand` を実行すると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)によって表示されるコレクションが、 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合は、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定される[`StackLayout`](xref:Xamarin.Forms.StackLayout)ます。

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>データが使用できないときに、テンプレート化されたカスタム型を表示する

[@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null` とき、または `ItemsSource` プロパティで指定されたコレクションが `null` または空の場合に、テンプレートが表示されるカスタム型に設定できます。 [@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは、`EmptyView` の外観を定義する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定できます。 次の XAML は、このシナリオの例を示しています。

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

@No__t_0 型は、`Filter` プロパティ、および対応する[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)を定義します。

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

[@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは `FilterData` オブジェクトに設定され、`Filter` プロパティデータは[`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティにバインドされます。 [@No__t_1](xref:Xamarin.Forms.SearchBar)が `FilterCommand` を実行すると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)によって表示されるコレクションが、`Filter` プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合は、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で定義されている[`Label`](xref:Xamarin.Forms.Label) [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティ値として設定されます。

> [!NOTE]
> データが使用できないときに、テンプレート化されたカスタム型を表示する場合、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは、複数の子ビューを含むビューに設定できます。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できないときに[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)として表示されるビューは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)内の[`ContentView`](xref:Xamarin.Forms.ContentView)オブジェクトとして定義できます。 @No__t_0 プロパティは、実行時にいくつかのビジネスロジックに基づいて、特定の `ContentView` に設定できます。 次の XAML の例は、このシナリオの例を示しています。

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

この XAML は、ページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)内の2つの[`ContentView`](xref:Xamarin.Forms.ContentView)オブジェクトを定義します。 [`Switch`](xref:Xamarin.Forms.Switch)オブジェクトは、どの `ContentView` オブジェクトを[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定するかを制御します。 [@No__t_1](xref:Xamarin.Forms.Switch)が切り替えられると、`OnEmptyViewSwitchToggled` イベントハンドラーは `ToggleEmptyView` メソッドを実行します。

```csharp
void ToggleEmptyView(bool isToggled)
{
    carouselView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

@No__t_0 メソッドは、`carouselView` オブジェクトの[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティを、 [`ResourceDictionary` プロパティの](xref:Xamarin.Forms.Switch.IsToggled)値に基づいて、 [`Switch.IsToggled`](xref:Xamarin.Forms.ResourceDictionary)に格納されている2つの[`ContentView`](xref:Xamarin.Forms.ContentView)オブジェクトのいずれかに設定します。 [@No__t_1](xref:Xamarin.Forms.SearchBar)が `FilterCommand` を実行すると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)によって表示されるコレクションが、 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合は、`EmptyView` プロパティとして設定された `ContentView` オブジェクトが表示されます。

リソースディクショナリの詳細については、「 [Xamarin. フォームリソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>実行時に EmptyViewTemplate を選択する

[@No__t_3](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定することにより、実行時に[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)の外観をその値に基づいて選択できます。

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

[@No__t_1](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは[`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに設定され、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは `SearchTermDataTemplateSelector` オブジェクトに設定されます。

[@No__t_1](xref:Xamarin.Forms.SearchBar)が `FilterCommand` を実行すると、 [`CarouselView`](xref:Xamarin.Forms.CarouselView)によって表示されるコレクションが、 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合は、`SearchTermDataTemplateSelector` オブジェクトによって選択された[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)が[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティとして設定され、表示されます。

次の例は、`SearchTermDataTemplateSelector` クラスを示しています。

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

@No__t_0 クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `DefaultTemplate` と `OtherTemplate` を定義します。 @No__t_0 のオーバーライドは、検索クエリが "xamarin" と等しくない場合に、ユーザーにメッセージを表示する `DefaultTemplate` を返します。 検索クエリが "xamarin" と等しい場合、`OnSelectTemplate` のオーバーライドは `OtherTemplate` を返します。これにより、ユーザーに基本メッセージが表示されます。

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CarouselView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. フォームリソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
