---
title: Xamarin. Forms SearchBar
description: Xamarin. Forms SearchBar は、検索を開始するために使用されるユーザー入力コントロールです。 SearchBar コントロールは、プレースホルダーテキスト、クエリ入力、実行、およびキャンセルをサポートしています。 この記事では、XAML とコードで SearchBar を使用する方法について説明します。
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/12/2019
ms.openlocfilehash: 391820cf2e94c1131f4082798ee9efa05d8489b8
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739396"
---
# <a name="xamarinforms-searchbar"></a>Xamarin. Forms SearchBar

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin. Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar)は、検索を開始するために使用されるユーザー入力コントロールです。 コントロール`SearchBar`は、プレースホルダーテキスト、クエリ入力、検索実行、およびキャンセルをサポートしています。 次のスクリーンショットは`SearchBar` 、結果がに表示さ`ListView`れるクエリを示しています。

Ios と android の[ ![searchbar on ios と Android のスクリーンショット](searchbar-images/device-searchbars-cropped.png "") ](searchbar-images/device-searchbars.png#lightbox "IOS と Android の Searchbar")

は`SearchBar` 、次のプロパティを定義します。

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)[キャンセル] ボタンの色を定義するです。`Color`
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)フォントが太字、斜体、または`SearchBar`そのどちらでもないかを決定する列挙値です。`FontAttributes`
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily)は、`SearchBar`によって使用されるフォントファミリを決定するです。`string`
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize)には、 `NamedSize`列挙値、 `double`またはプラットフォーム間の特定のフォントサイズを表す値を指定できます。
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)クエリテキストの水平方向の配置を定義する列挙値です。`TextAlignment`
* [`Placeholder`](xref:Xamarin.Forms.SearchBar.Placeholder)は、"検索..." などのプレースホルダーテキストを定義するです。`string`
* [`PlaceholderColor`](xref:Xamarin.Forms.SearchBar.PlaceholderColor)プレースホルダーテキストの色を定義するです。`Color`
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)は、 `ICommand`ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにします。
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)に渡す必要があるパラメーターを指定`object`するです`SearchCommand`。
* [`Text`](xref:Xamarin.Forms.SearchBar.Text)`string` の`SearchBar`クエリテキストを含むです。
* [`TextColor`](xref:Xamarin.Forms.SearchBar.TextColor)クエリテキストの色を定義するです。`Color`

これらのプロパティはオブジェクト[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)によってバックアップさ`SearchBar`れます。つまり、をカスタマイズして、データバインディングのターゲットにすることができます。 でフォントプロパティを指定`SearchBar`することは、他の[Xamarin フォームテキストコントロール](~/xamarin-forms/user-interface/text/index.md)でのテキストのカスタマイズと一致します。 詳細については、「 [Xamarin. Forms のフォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="create-a-searchbar"></a>SearchBar を作成する

は`SearchBar` 、XAML でインスタンス化できます。 省略可能`Placeholder`なプロパティを設定して、[クエリ入力] ボックスにヒントテキストを定義できます。 の既定値`Placeholder`は空の文字列であるため、設定されていない場合、プレースホルダーは表示されません。 次の例は、省略可能`SearchBar` `Placeholder`なプロパティセットを使用して、XAML でをインスタンス化する方法を示しています。

```xaml
<SearchBar Placeholder="Search items..." />
```

は`SearchBar` 、コードで作成することもできます。

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>SearchBar の外観のプロパティ

`SearchBar`コントロールには、コントロールの外観をカスタマイズする多くのプロパティが定義されています。 次の例は、複数のプロパティ`SearchBar`が指定された XAML でをインスタンス化する方法を示しています。

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

これらのプロパティは、コードでを`SearchBar`作成するときに指定することもできます。

```csharp
SearchBar searchBar = new SearchBar
{
    Placeholder = "Search items...",
    PlaceholderColor = Color.Orange,
    TextColor = Color.Orange,
    HorizontalTextAlignment = TextAlignment.Center,
    FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(SearchBar)),
    FontAttributes = FontAttributes.Italic
};
```

次のスクリーンショットは、 `SearchBar`その結果を示しています。

Ios および android でカスタマイズされた searchbar for ios と android のカスタマイズされ[![た Searchbar のスクリーンショット](searchbar-images/device-searchbars-styled-cropped.png "") ](searchbar-images/device-searchbars-styled.png#lightbox "IOS および Android でのカスタマイズ")された searchbar

## <a name="perform-a-search-with-event-handlers"></a>イベントハンドラーを使用した検索の実行

次のいずれかのイベントに`SearchBar`イベントハンドラーをアタッチすることで、コントロールを使用して検索を実行できます。

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)は、ユーザーが [検索] ボタンをクリックするか enter キーを押すと呼び出されます。
* [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged)クエリボックス内のテキストが変更されるたびに呼び出されます。

次の例は、XAML で`TextChanged`イベントにアタッチされ、を`ListView`使用して検索結果を表示するイベントハンドラーを示しています。

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

イベントハンドラーは、 `SearchBar`コードで作成されたにアタッチすることもできます。

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

が XAML とコードのどちらを使用し`SearchBar`て作成されているかにかかわらず、分離コードファイル内のイベントハンドラーは同じです。`TextChanged`

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

前の例では、クエリに`DataService`一致する項目`GetSearchResults`を返すことができるメソッドを持つクラスが存在することを意味しています。 `ItemsSource` `ListView`コントロールの`Text` プロパティ`GetSearchResults`値がメソッドに渡され、結果がコントロールのプロパティを更新するために使用されます。 `SearchBar` 全体的な影響として、検索結果が`ListView`コントロールに表示されます。

サンプルアプリケーションには、 `DataService`検索機能をテストするために使用できるクラス実装が用意されています。

## <a name="perform-a-search-using-a-viewmodel"></a>ビューモデルを使用して検索を実行する

プロパティ`SearchCommand` `ICommand`と`SearchCommandParameter`プロパティを実装にバインドすることにより、イベントハンドラーを使用せずに検索を実行できます。 サンプルプロジェクトでは、モデルビュービューモデル (MVVM) パターンを使用してこれらの実装を示しています。 MVVM を使用したデータバインディングの詳細については、「[データバインディングと MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)」を参照してください。

サンプルアプリケーションのビューモデルには、次のコードが含まれています。

```csharp
public class SearchViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ICommand PerformSearch => new Command<string>((string query) =>
    {
        SearchResults = DataService.GetSearchResults(query);
    });

    private List<string> searchResults = DataService.Fruits;
    public List<string> SearchResults
    {
        get
        {
            return searchResults;
        }
        set
        {
            searchResults = value;
            NotifyPropertyChanged();
        }
    }
}
```

> [!NOTE]
> ビューモデルは、検索を実行`DataService`できるクラスが存在することを前提としています。 サンプルデータなどのクラスは、サンプルアプリケーションで使用できます。`DataService`

次の XAML は、を`SearchBar`例のビューモデルにバインドする方法を示しています。コントロールは`ListView` 、検索結果を表示します。

```xaml
<ContentPage ...>
    <ContentPage.BindingContext>
        <viewmodels:SearchViewModel />
    </ContentPage.BindingContext>
    <StackLayout ...>
        <SearchBar x:Name="searchBar"
                   ...
                   SearchCommand="{Binding PerformSearch}"
                   SearchCommandParameter="{Binding Text, Source={x:Reference searchBar}}"/>
        <ListView x:Name="searchResults"
                  ...
                  ItemsSource="{Binding SearchResults} />
    </StackLayout>
</ContentPage>
```

この例では`BindingContext` 、を`SearchViewModel`クラスのインスタンスとして設定します。 このメソッドは`SearchCommand` 、 `ICommand` `Text` `PerformSearch` ビューモデル`SearchBar`のにプロパティをバインドし、プロパティをプロパティにバインドします。`SearchCommandParameter` プロパティは、ビューモデルの`SearchResults`プロパティにバインドされます。 `ListView.ItemsSource`

インターフェイスとバインドの`ICommand`詳細については、「 [Xamarin. Forms data binding](~/xamarin-forms/app-fundamentals/data-binding/index.md) 」と「 [ICommand インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [SearchBar デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin. フォームテキストコントロール](~/xamarin-forms/user-interface/text/index.md)
* [Xamarin 形式のフォント](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
