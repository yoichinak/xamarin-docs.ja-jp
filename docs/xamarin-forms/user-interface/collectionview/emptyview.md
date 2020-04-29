---
title: Xamarin. Forms CollectionView EmptyView
description: CollectionView では、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: d5a22d110e52a397827fb451bc16872b72293755
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517489"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin. Forms CollectionView EmptyView

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)表示するデータがない場合にユーザーフィードバックを提供するために使用できる次のプロパティを定義します。

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)`object`型の[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 、プロパティが`null`のときに表示される文字列、バインディング、またはビュー、または`ItemsSource`プロパティによって指定された`null`コレクションがまたは空の場合に表示される。 既定値は `null` です。
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の。指定した`EmptyView`の書式設定に使用するテンプレート。 既定値は `null` です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、プロパティをデータバインディングのターゲットにできることを意味します。

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティを設定するための主な使用シナリオでは、に対するフィルター処理操作[`CollectionView`](xref:Xamarin.Forms.CollectionView)によってデータが得られず、web サービスからデータを取得しているときにユーザーフィードバックが表示される場合に、ユーザーフィードバックが表示されます。

> [!NOTE]
> プロパティ[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)は、必要に応じて、対話型コンテンツを含むビューに設定できます。

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが使用できないときに文字列を表示する

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティは、プロパティが`null`の場合、またはプロパティで`ItemsSource`指定されたコレクションが`null`または空の場合に表示される文字列に設定できます。 次の XAML は、このシナリオの例を示しています。

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

該当の C# コードを次に示します。

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

その結果、データバインドコレクションが`null`であるため、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定された文字列が表示されます。

[![IOS および Android での空の表示テキストを含む CollectionView 縦の一覧のスクリーンショット](emptyview-images/null-itemssource.png "テキストを空にした CollectionView の一覧表示")](emptyview-images/null-itemssource-large.png#lightbox "テキストを空にした CollectionView の一覧表示")

## <a name="display-views-when-data-is-unavailable"></a>データが使用できないときにビューを表示する

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)プロパティは、プロパティが`null`の場合、またはプロパティで`ItemsSource`指定されたコレクションが`null`または空の場合に表示されるビューに設定できます。 1つのビュー、または複数の子ビューを含むビューを指定できます。 次の XAML の例は`EmptyView` 、複数の子ビューを含むビューに設定されたプロパティを示しています。

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

該当の C# コードを次に示します。

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

がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行`FilterCommand`すると、によって表示[`CollectionView`](xref:Xamarin.Forms.CollectionView)されるコレクションは、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成[`StackLayout`](xref:Xamarin.Forms.StackLayout)されない[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)場合、プロパティ値としてセットが表示されます。

[![IOS と Android 上のカスタムの空のビューを使用した CollectionView の一覧のスクリーンショット](emptyview-images/filter-multiple-views.png "カスタムの空のビューを使用した CollectionView の垂直方向の一覧表示")](emptyview-images/filter-multiple-views-large.png#lightbox "カスタムの空のビューを使用した CollectionView の垂直方向の一覧表示")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>データが使用できないときに、テンプレート化されたカスタム型を表示する

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティは[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)カスタム型に設定できます。これは、プロパティが`null`の場合、または`ItemsSource`プロパティで指定されたコレクションが`null`または空の場合に、そのテンプレートが表示されます。 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティは、の外観を定義[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)するに設定でき`EmptyView`ます。 次の XAML は、このシナリオの例を示しています。

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

該当の C# コードを次に示します。

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

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ`FilterData`はオブジェクトに設定され、プロパティデータ`Filter`はプロパティに[`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)バインドされます。 がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行`FilterCommand`すると、によって表示[`CollectionView`](xref:Xamarin.Forms.CollectionView)されるコレクションは、 `Filter`プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成[`Label`](xref:Xamarin.Forms.Label)されない[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)場合、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティ値として設定されたで定義されているが表示されます。

[![IOS と Android の空のビューテンプレートを含む CollectionView の一覧のスクリーンショット](emptyview-images/emptyviewtemplate.png "空のビューテンプレートを含む CollectionView の一覧")](emptyview-images/emptyviewtemplate-large.png#lightbox "空のビューテンプレートを含む CollectionView の一覧")

> [!NOTE]
> データが使用できないときに、テンプレート化され[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)たカスタム型を表示する場合は、複数の子ビューを含むビューにプロパティを設定できます。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に EmptyView を選択する

データが使用できない[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)ときにとして表示されるビューは[`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)、内のオブジェクトとして定義できます。 この`EmptyView`プロパティは、実行時に何らか`ContentView`のビジネスロジックに基づいて、特定のに設定できます。 次の XAML は、このシナリオの例を示しています。

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

この XAML は、 [`ContentView`](xref:Xamarin.Forms.ContentView)ページ[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)レベルで2つのオブジェクトを定義[`Switch`](xref:Xamarin.Forms.Switch)します。 `ContentView`オブジェクトは、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ値として設定するオブジェクトを制御します。 [`Switch`](xref:Xamarin.Forms.Switch)が切り替えられると、 `OnEmptyViewSwitchToggled`イベントハンドラーによっ`ToggleEmptyView`てメソッドが実行されます。

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

メソッド`ToggleEmptyView` [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)は、 [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)プロパティの値に`collectionView`基づいて、オブジェクトのプロパティ[`ContentView`](xref:Xamarin.Forms.ContentView)を、に格納[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)されている2つのオブジェクトのいずれかに設定します。 がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行`FilterCommand`すると、によって表示[`CollectionView`](xref:Xamarin.Forms.CollectionView)されるコレクションは、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成`ContentView`されない場合`EmptyView` 、プロパティとして設定されたオブジェクトが表示されます。

[![IOS と Android で空のビューをスワップした CollectionView の一覧のスクリーンショット](emptyview-images/swap.png "スワップされる空のビューを含む CollectionView 縦の一覧")](emptyview-images/swap-large.png#lightbox "スワップされる空のビューを含む CollectionView 縦の一覧")

リソースディクショナリの詳細については、「 [Xamarin. フォームリソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>実行時に EmptyViewTemplate を選択する

の外観は、 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティを[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)オブジェクトに設定することによって、実行時に値に基づいて選択できます。

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

該当の C# コードを次に示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)プロパティ[`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)がプロパティに設定され、 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)プロパティが`SearchTermDataTemplateSelector`オブジェクトに設定されています。

がを[`SearchBar`](xref:Xamarin.Forms.SearchBar)実行`FilterCommand`すると、によって表示[`CollectionView`](xref:Xamarin.Forms.CollectionView)されるコレクションは、 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている検索用語に対してフィルター処理されます。 フィルター処理操作によってデータが生成[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)されない`SearchTermDataTemplateSelector`場合、オブジェクトによっ[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)て選択されたがプロパティとして設定され、表示されます。

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

クラス`SearchTermTemplateSelector`は、 `DefaultTemplate`さまざま`OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)なデータテンプレートに設定されるプロパティとプロパティを定義します。 上書き`OnSelectTemplate`は、 `DefaultTemplate`検索クエリが "xamarin" と等しくない場合に、ユーザーにメッセージを表示するを返します。 検索クエリが "xamarin" と等しい場合、オーバーライドは`OnSelectTemplate`を返し`OtherTemplate`ます。これにより、ユーザーに基本的なメッセージが表示されます。

[![IOS と Android での CollectionView runtime の空のビューテンプレートの選択のスクリーンショット](emptyview-images/datatemplateselector.png "CollectionView でのランタイムの空のビューテンプレートの選択")](emptyview-images/datatemplateselector-large.png#lightbox "CollectionView でのランタイムの空のビューテンプレートの選択")

データテンプレートセレクターの詳細については、「 [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Forms のデータ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms のリソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
