---
title: Xamarin. Forms CollectionView EmptyView
description: CollectionView では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 `EmptyView` には、文字列、ビュー、または複数のビューが使用できます。
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f58f160594d21b1cf8826017f19ad31beb8eeda1
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130980"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin. Forms CollectionView EmptyView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、表示するデータがない場合にユーザーフィードバックを提供するために使用できる次のプロパティを定義します。

- 型 `object`の[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null`したときに表示される文字列、バインディング、またはビュー、または `ItemsSource` プロパティによって指定されたコレクションが `null` または空の場合に表示されます。 既定値は `null` です。
- 指定した `EmptyView`の書式設定に使用するテンプレート[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)型の[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)。 既定値は `null` です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティを設定するための主な使用シナリオでは、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)でのフィルター処理操作によってデータが得られず、web サービスからデータを取得しているときにユーザーフィードバックが表示される場合に、ユーザーからのフィードバックが表示されます。

> [!NOTE]
> [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、必要に応じて、対話型コンテンツを含むビューに設定できます。

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null`とき、または `ItemsSource` プロパティによって指定されたコレクションが `null` または空のときに表示される文字列に設定できます。 次の XAML は、このシナリオの例を示しています。

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

同等の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

その結果、データバインドコレクションが `null`ため、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定された文字列が表示されます。

[![IOS および Android での空の表示テキストを含む CollectionView 縦の一覧のスクリーンショット](emptyview-images/null-itemssource.png "テキストを空にした CollectionView の一覧表示")](emptyview-images/null-itemssource-large.png#lightbox "テキストを空にした CollectionView の一覧表示")

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null`とき、または `ItemsSource` プロパティによって指定されたコレクションが `null` または空のときに表示されるビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は、複数の子ビューを含むビューに設定された `EmptyView` プロパティを示しています。

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

同等の C# コードを次に示します。

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

[`SearchBar`](xref:Xamarin.Forms.SearchBar)が `FilterCommand`を実行すると、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)によって表示されるコレクションが、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定された[`StackLayout`](xref:Xamarin.Forms.StackLayout)が表示されます。

[![IOS と Android 上のカスタムの空のビューを使用した CollectionView の一覧のスクリーンショット](emptyview-images/filter-multiple-views.png "カスタムの空のビューを使用した CollectionView の垂直方向の一覧表示")](emptyview-images/filter-multiple-views-large.png#lightbox "カスタムの空のビューを使用した CollectionView の垂直方向の一覧表示")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>データが使用できないときに、テンプレート化されたカスタム型を表示する

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティが `null`とき、または `ItemsSource` プロパティで指定されたコレクションが `null` または空の場合に、テンプレートが表示されるカスタム型に設定できます。 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは、`EmptyView`の外観を定義する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定できます。 次の XAML は、このシナリオの例を示しています。

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

同等の C# コードを次に示します。

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

`FilterData` 型は、`Filter` プロパティ、および対応する[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)を定義します。

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

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは `FilterData` オブジェクトに設定され、`Filter` プロパティデータは[`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティにバインドされます。 [`SearchBar`](xref:Xamarin.Forms.SearchBar)が `FilterCommand`を実行すると、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)によって表示されるコレクションが、`Filter` プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で定義されている[`Label`](xref:Xamarin.Forms.Label) [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティ値として設定されます。

[![IOS と Android の空のビューテンプレートを含む CollectionView の一覧のスクリーンショット](emptyview-images/emptyviewtemplate.png "空のビューテンプレートを含む CollectionView の一覧")](emptyview-images/emptyviewtemplate-large.png#lightbox "空のビューテンプレートを含む CollectionView の一覧")

> [!NOTE]
> データが使用できないときに、テンプレート化されたカスタム型を表示する場合、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは、複数の子ビューを含むビューに設定できます。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できないときに[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)として表示されるビューは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)内の[`ContentView`](xref:Xamarin.Forms.ContentView)オブジェクトとして定義できます。 `EmptyView` プロパティは、実行時にいくつかのビジネスロジックに基づいて、特定の `ContentView`に設定できます。 次の XAML の例は、このシナリオの例を示しています。

```xaml
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

この XAML は、ページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)内の2つの[`ContentView`](xref:Xamarin.Forms.ContentView)オブジェクトを定義します。 [`Switch`](xref:Xamarin.Forms.Switch)オブジェクトは、どの `ContentView` オブジェクトを[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定するかを制御します。 [`Switch`](xref:Xamarin.Forms.Switch)が切り替えられると、`OnEmptyViewSwitchToggled` イベントハンドラーは `ToggleEmptyView` メソッドを実行します。

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView` メソッドは、`collectionView` オブジェクトの[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティを、 [`ResourceDictionary`プロパティの](xref:Xamarin.Forms.Switch.IsToggled)値に基づいて、 [`Switch.IsToggled`](xref:Xamarin.Forms.ResourceDictionary)に格納されている2つの[`ContentView`](xref:Xamarin.Forms.ContentView)オブジェクトのいずれかに設定します。 [`SearchBar`](xref:Xamarin.Forms.SearchBar)が `FilterCommand`を実行すると、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)によって表示されるコレクションが、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合は、`EmptyView` プロパティとして設定された `ContentView` オブジェクトが表示されます。

[![IOS と Android で空のビューをスワップした CollectionView の一覧のスクリーンショット](emptyview-images/swap.png "スワップされる空のビューを含む CollectionView 縦の一覧")](emptyview-images/swap-large.png#lightbox "スワップされる空のビューを含む CollectionView 縦の一覧")

リソースディクショナリの詳細については、「 [Xamarin. フォームリソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>実行時に EmptyViewTemplate を選択する

[`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定することにより、実行時に[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)の外観をその値に基づいて選択できます。

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

同等の C# コードを次に示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは[`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに設定され、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは `SearchTermDataTemplateSelector` オブジェクトに設定されます。

[`SearchBar`](xref:Xamarin.Forms.SearchBar)が `FilterCommand`を実行すると、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)によって表示されるコレクションが、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成されない場合は、`SearchTermDataTemplateSelector` オブジェクトによって選択された[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)が[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティとして設定され、表示されます。

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

`SearchTermTemplateSelector` クラスは、さまざまなデータテンプレートに設定されている[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティの `DefaultTemplate` と `OtherTemplate` を定義します。 `OnSelectTemplate` のオーバーライドは、検索クエリが "xamarin" と等しくない場合に、ユーザーにメッセージを表示する `DefaultTemplate`を返します。 検索クエリが "xamarin" と等しい場合、`OnSelectTemplate` のオーバーライドは `OtherTemplate`を返します。これにより、ユーザーに基本メッセージが表示されます。

[![IOS と Android での CollectionView runtime の空のビューテンプレートの選択のスクリーンショット](emptyview-images/datatemplateselector.png "CollectionView でのランタイムの空のビューテンプレートの選択")](emptyview-images/datatemplateselector-large.png#lightbox "CollectionView でのランタイムの空のビューテンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. フォームデータテンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms のリソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
