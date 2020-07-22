---
title: Xamarin.FormsSearchBar
description: Xamarin.FormsSearchbar は、検索を開始するために使用されるユーザー入力コントロールです。 SearchBar コントロールは、プレースホルダーテキスト、クエリ入力、実行、およびキャンセルをサポートしています。 この記事では、XAML とコードで SearchBar を使用する方法について説明します。
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d8ceb139b1b9cd77aa922f98c80884d5c3e1a474
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127544"
---
# <a name="xamarinforms-searchbar"></a>Xamarin.FormsSearchBar

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin.Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar) は、検索を開始するために使用されるユーザー入力コントロールです。 コントロールは、 `SearchBar` プレースホルダーテキスト、クエリ入力、検索実行、およびキャンセルをサポートしています。 次のスクリーンショットは、 `SearchBar` 結果がに表示されるクエリを示してい `ListView` ます。

[![IOS と Android の SearchBar のスクリーンショット](searchbar-images/device-searchbars-cropped.png "IOS と Android の SearchBar")](searchbar-images/device-searchbars.png#lightbox "IOS と Android の SearchBar")

`SearchBar`クラスは、次のプロパティを定義します。

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)`Color`[キャンセル] ボタンの色を定義するです。
* `CharacterSpacing`: `double` 型、`SearchBar` テキストの文字間の間隔。
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)`FontAttributes` `SearchBar` フォントが太字、斜体、またはそのどちらでもないかを決定する列挙値です。
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily)は `string` 、によって使用されるフォントファミリを決定するです `SearchBar` 。
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize)には、 `NamedSize` 列挙値、または `double` プラットフォーム間の特定のフォントサイズを表す値を指定できます。
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)`TextAlignment`クエリテキストの水平方向の配置を定義する列挙値です。
* `VerticalTextAlignment``TextAlignment`クエリテキストの垂直方向の配置を定義する列挙値です。
* [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)は、 `string` "検索..." などのプレースホルダーテキストを定義するです。
* [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)`Color`プレースホルダーテキストの色を定義するです。
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)は、 `ICommand` ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにします。
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)`object`に渡す必要があるパラメーターを指定するです `SearchCommand` 。
* [`Text`](xref:Xamarin.Forms.InputView.Text)`string`のクエリテキストを含むです `SearchBar` 。
* [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)`Color`クエリテキストの色を定義するです。

これらのプロパティはオブジェクトによってバックアップされ [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。つまり、を `SearchBar` カスタマイズして、データバインディングのターゲットにすることができます。 でフォントプロパティを指定すること `SearchBar` は、他の[ Xamarin.Forms テキストコントロール](~/xamarin-forms/user-interface/text/index.md)のテキストをカスタマイズするのと同じです。 詳細については、「 [」の Xamarin.Forms 「フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

## <a name="create-a-searchbar"></a>SearchBar を作成する

は、 `SearchBar` XAML でインスタンス化できます。 省略可能な `Placeholder` プロパティを設定して、[クエリ入力] ボックスにヒントテキストを定義できます。 の既定値 `Placeholder` は空の文字列であるため、設定されていない場合、プレースホルダーは表示されません。 次の例は、 `SearchBar` 省略可能なプロパティセットを使用して、XAML でをインスタンス化する方法を示してい `Placeholder` ます。

```xaml
<SearchBar Placeholder="Search items..." />
```

は、 `SearchBar` コードで作成することもできます。

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>SearchBar の外観のプロパティ

コントロールには、 `SearchBar` コントロールの外観をカスタマイズする多くのプロパティが定義されています。 次の例は、 `SearchBar` 複数のプロパティが指定された XAML でをインスタンス化する方法を示しています。

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

これらのプロパティは、コードでオブジェクトを作成するときに指定することもでき `SearchBar` ます。

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

次のスクリーンショットは、結果として得られるコントロールを示してい `SearchBar` ます。

[![IOS および Android でのカスタマイズされた SearchBar のスクリーンショット](searchbar-images/device-searchbars-styled-cropped.png "IOS および Android でのカスタマイズされた SearchBar")](searchbar-images/device-searchbars-styled.png#lightbox "IOS および Android でのカスタマイズされた SearchBar")

> [!NOTE]
> IOS では、 `SearchBarRenderer` クラスにはオーバーライド可能なメソッドが含まれてい `UpdateCancelButton` ます。 このメソッドは、[キャンセル] ボタンが表示されるタイミングを制御し、カスタムレンダラーでオーバーライドできます。 カスタムレンダラーの詳細については、「 [ Xamarin.Forms カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。

## <a name="perform-a-search-with-event-handlers"></a>イベントハンドラーを使用した検索の実行

`SearchBar`次のいずれかのイベントにイベントハンドラーをアタッチすることで、コントロールを使用して検索を実行できます。

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)は、ユーザーが [検索] ボタンをクリックするか enter キーを押すと呼び出されます。
* [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)クエリボックス内のテキストが変更されるたびに呼び出されます。

次の例は、XAML でイベントにアタッチされ、 `TextChanged` を使用して `ListView` 検索結果を表示するイベントハンドラーを示しています。

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

イベントハンドラーは、 `SearchBar` コードで作成されたにアタッチすることもできます。

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

`TextChanged`が XAML とコードのどちらを使用して作成されているかにかかわらず、分離コードファイル内のイベントハンドラーは同じです `SearchBar` 。

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

前の例では、 `DataService` `GetSearchResults` クエリに一致する項目を返すことができるメソッドを持つクラスが存在することを意味しています。 `SearchBar`コントロールの `Text` プロパティ値がメソッドに渡され、 `GetSearchResults` 結果がコントロールのプロパティを更新するために使用され `ListView` `ItemsSource` ます。 全体的な影響として、検索結果がコントロールに表示され `ListView` ます。

サンプルアプリケーションには、 `DataService` 検索機能をテストするために使用できるクラス実装が用意されています。

## <a name="perform-a-search-using-a-viewmodel"></a>ビューモデルを使用して検索を実行する

`SearchCommand`プロパティとプロパティを実装にバインドすることにより、イベントハンドラーを使用せずに検索を実行でき `SearchCommandParameter` `ICommand` ます。 サンプルプロジェクトでは、モデルビュービューモデル (MVVM) パターンを使用してこれらの実装を示しています。 MVVM を使用したデータバインディングの詳細については、「[データバインディングと MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)」を参照してください。

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
> ビューモデルは、 `DataService` 検索を実行できるクラスが存在することを前提としています。 サンプルデータなどのクラスは、 `DataService` サンプルアプリケーションで使用できます。

次の XAML は、を例のビューモデルにバインドする方法を示してい `SearchBar` `ListView` ます。コントロールは、検索結果を表示します。

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
                  ItemsSource="{Binding SearchResults}" />
    </StackLayout>
</ContentPage>
```

この例では、 `BindingContext` をクラスのインスタンスとして設定し `SearchViewModel` ます。 このメソッドは、ビューモデルのにプロパティをバインドし、プロパティ `SearchCommand` `PerformSearch` `ICommand` `SearchBar` `Text` をプロパティにバインドし `SearchCommandParameter` ます。 プロパティは、 `ListView.ItemsSource` `SearchResults` ビューモデルのプロパティにバインドされます。

インターフェイスとバインドの詳細については `ICommand` 、「 [ Xamarin.Forms データバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」と「 [ICommand インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [SearchBar デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin.Formsテキストコントロール](~/xamarin-forms/user-interface/text/index.md)
* [フォントXamarin.Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin.Formsデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
