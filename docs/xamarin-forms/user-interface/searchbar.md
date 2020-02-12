---
title: Xamarin. Forms SearchBar
description: Xamarin. Forms SearchBar は、検索を開始するために使用されるユーザー入力コントロールです。 SearchBar コントロールは、プレースホルダーテキスト、クエリ入力、実行、およびキャンセルをサポートしています。 この記事では、XAML とコードで SearchBar を使用する方法について説明します。
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/04/2019
ms.openlocfilehash: a48a91b886cadcbe9dfa73a524b7bfa9fb2cf5fb
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130973"
---
# <a name="xamarinforms-searchbar"></a>Xamarin. Forms SearchBar

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin. Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar)は、検索を開始するために使用されるユーザー入力コントロールです。 `SearchBar` コントロールは、プレースホルダーテキスト、クエリ入力、検索実行、およびキャンセルをサポートしています。 次のスクリーンショットは、`ListView`に結果が表示される `SearchBar` クエリを示しています。

[![IOS と Android の SearchBar のスクリーンショット](searchbar-images/device-searchbars-cropped.png "IOS と Android の SearchBar")](searchbar-images/device-searchbars.png#lightbox "IOS と Android の SearchBar")

`SearchBar` クラスは、次のプロパティを定義します。

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)は、[キャンセル] ボタンの色を定義する `Color` です。
* `CharacterSpacing`: `double` 型、`SearchBar` テキストの文字間の間隔。
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)は、`SearchBar` フォントが太字、斜体、またはどちらでもないかを決定する `FontAttributes` 列挙値です。
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily)は、`SearchBar`で使用されるフォントファミリを決定する `string` です。
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize)には、`NamedSize` 列挙値、またはプラットフォーム間の特定のフォントサイズを表す `double` 値を指定できます。
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)は、クエリテキストの水平方向の配置を定義する `TextAlignment` 列挙値です。
* `VerticalTextAlignment` は、クエリテキストの垂直方向の配置を定義する `TextAlignment` 列挙値です。
* [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)は、"検索..." などのプレースホルダーテキストを定義する `string` です。
* [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)は、プレースホルダーテキストの色を定義する `Color` です。
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)は、ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにする `ICommand` です。
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)は、`SearchCommand`に渡す必要があるパラメーターを指定する `object` です。
* [`Text`](xref:Xamarin.Forms.InputView.Text)は、`SearchBar`内のクエリテキストを含む `string` です。
* [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)は、クエリテキストの色を定義する `Color` です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされています。つまり、`SearchBar` をカスタマイズして、データバインディングのターゲットにすることができます。 `SearchBar` でのフォントプロパティの指定は、他の[Xamarin. フォームテキストコントロール](~/xamarin-forms/user-interface/text/index.md)でのテキストのカスタマイズと一致します。 詳細については、「 [Xamarin. Forms のフォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="create-a-searchbar"></a>SearchBar を作成する

`SearchBar` は、XAML でインスタンス化できます。 省略可能な `Placeholder` プロパティは、[クエリ入力] ボックスにヒントテキストを定義するように設定できます。 `Placeholder` の既定値は空の文字列であるため、設定されていない場合はプレースホルダーが表示されません。 次の例は、省略可能な `Placeholder` プロパティセットを使用して、XAML で `SearchBar` をインスタンス化する方法を示しています。

```xaml
<SearchBar Placeholder="Search items..." />
```

コードでは、`SearchBar` を作成することもできます。

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>SearchBar の外観のプロパティ

`SearchBar` コントロールでは、コントロールの外観をカスタマイズする多くのプロパティが定義されています。 次の例は、複数のプロパティを指定して XAML で `SearchBar` をインスタンス化する方法を示しています。

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

これらのプロパティは、コードで `SearchBar` オブジェクトを作成するときに指定することもできます。

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

次のスクリーンショットは、結果として得られる `SearchBar` コントロールを示しています。

[![IOS および Android でのカスタマイズされた SearchBar のスクリーンショット](searchbar-images/device-searchbars-styled-cropped.png "IOS および Android でのカスタマイズされた SearchBar")](searchbar-images/device-searchbars-styled.png#lightbox "IOS および Android でのカスタマイズされた SearchBar")

> [!NOTE]
> IOS では、`SearchBarRenderer` クラスに、オーバーライド可能な `UpdateCancelButton` メソッドが含まれています。 このメソッドは、[キャンセル] ボタンが表示されるタイミングを制御し、カスタムレンダラーでオーバーライドできます。 カスタムレンダラーの詳細については、「 [Xamarin. フォームカスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。

## <a name="perform-a-search-with-event-handlers"></a>イベントハンドラーを使用した検索の実行

`SearchBar` コントロールを使用して検索を実行するには、次のいずれかのイベントにイベントハンドラーをアタッチします。

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)は、ユーザーが [検索] ボタンをクリックするか enter キーを押すと呼び出されます。
* [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)は、[クエリ] ボックス内のテキストが変更されるたびに呼び出されます。

次の例は、XAML の `TextChanged` イベントにアタッチされたイベントハンドラーを示し、`ListView` を使用して検索結果を表示します。

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

イベントハンドラーは、コードで作成された `SearchBar` にアタッチすることもできます。

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

分離コードファイル内の `TextChanged` イベントハンドラーは、`SearchBar` が XAML とコードのどちらを使用して作成されているかにかかわらず、同じです。

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

前の例では、クエリに一致する項目を返すことができる `GetSearchResults` メソッドを持つ `DataService` クラスが存在することを意味しています。 `SearchBar` コントロールの `Text` プロパティ値が `GetSearchResults` メソッドに渡され、結果を使用して `ListView` コントロールの `ItemsSource` プロパティが更新されます。 全体的な影響として、`ListView` コントロールに検索結果が表示されます。

サンプルアプリケーションには、検索機能をテストするために使用できる `DataService` クラスの実装が用意されています。

## <a name="perform-a-search-using-a-viewmodel"></a>ビューモデルを使用して検索を実行する

`SearchCommand` と `SearchCommandParameter` のプロパティを `ICommand` の実装にバインドすることによって、イベントハンドラーなしで検索を実行できます。 サンプルプロジェクトでは、モデルビュービューモデル (MVVM) パターンを使用してこれらの実装を示しています。 MVVM を使用したデータバインディングの詳細については、「[データバインディングと MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)」を参照してください。

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
> ビューモデルは、検索を実行できる `DataService` クラスが存在することを前提としています。 サンプルアプリケーションでは、データの例を含む `DataService` クラスを使用できます。

次の XAML は、`SearchBar` をサンプルビューモデルにバインドする方法を示しています。この例では、`ListView` コントロールを使用して検索結果を表示しています。

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

この例では、`BindingContext` を `SearchViewModel` クラスのインスタンスに設定します。 このメソッドは、`SearchCommand` プロパティをビューモデルの `PerformSearch` `ICommand` にバインドし、`SearchBar` の `Text` プロパティを `SearchCommandParameter` プロパティにバインドします。 `ListView.ItemsSource` プロパティは、ビューモデルの `SearchResults` プロパティにバインドされます。

`ICommand` インターフェイスとバインドの詳細については、「 [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」と「 [ICommand インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [SearchBar デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin. フォームテキストコントロール](~/xamarin-forms/user-interface/text/index.md)
* [Xamarin 形式のフォント](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
