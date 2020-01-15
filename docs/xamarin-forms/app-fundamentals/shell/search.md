---
title: Xamarin.Forms シェルでの検索
description: Xamarin.Forms シェル アプリケーションでは、各ページの上部に追加できる検索ボックスとして提供される、統合された検索機能を利用できます。
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: 9bd4fe5f1a35e2a6f36540cbee13838841b36d92
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490065"
---
# <a name="xamarinforms-shell-search"></a>Xamarin.Forms シェルでの検索

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms シェルには、`SearchHandler` クラスによって提供される統合された検索機能が組み込まれています。 検索機能は、サブクラス化された `SearchHandler` オブジェクトを `Shell.SearchHandler` 添付プロパティに設定することで、ページに追加できます。 これにより、検索ボックスがページの上部に追加されます。

[![iOS および Android 上の Shell SearchHandler のスクリーンショット](search-images/searchhandler.png "Shell SearchHandler")](search-images/searchhandler-large.png#lightbox "Shell SearchHandler")

検索ボックスにクエリが入力されると、`Query` プロパティが更新され、更新ごとに `OnQueryChanged` メソッドが実行されます。 このメソッドをオーバーライドして、検索候補領域にデータを設定できます。

[![iOS および Android 上の Shell SearchHandler での検索結果のスクリーンショット](search-images/search-suggestions.png "Shell SearchHandler の検索結果")](search-images/search-suggestions-large.png#lightbox "Shell SearchHandler の検索結果")

検索候補領域から結果が選択されると、`OnItemSelected` メソッドが実行されます。 このメソッドは、詳細ページに移動するなどによって、適切に応答するようにオーバーライドできます。

## <a name="create-a-searchhandler"></a>SearchHandler を作成する

検索機能は、`SearchHandler` クラスをサブクラス化して `OnQueryChanged` および `OnItemSelected` メソッドをオーバーライドすることで、シェル アプリケーションに追加できます。

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

`OnQueryChanged` オーバーライドには、2 つの引数があります。前回の検索クエリを格納する `oldValue` と、現在の検索クエリを格納する `newValue` です。 検索候補領域は、現在の検索クエリと一致する項目を含む `IEnumerable` コレクションを `SearchHandler.ItemsSource` プロパティに設定することで、更新できます。

検索結果がユーザーによって選択されると、`OnItemSelected` オーバーライドが実行されて、`SelectedItem` プロパティが設定されます。 この例では、メソッドによって、選択した `Animal` に関するデータを表示した別のページへナビゲートされます。 ナビゲーションについて詳しくは、「[Xamarin.Forms シェルのナビゲーション](navigation.md)」をご覧ください。

> [!NOTE]
> 追加の `SearchHandler` プロパティは、検索ボックスの外観を制御するために設定できます。

## <a name="consume-a-searchhandler"></a>SearchHandler を使用する

サブクラス化された `SearchHandler` は、`Shell.SearchHandler` 添付プロパティにサブクラス化された型のオブジェクトを設定することで、使用できます。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

該当の C# コードを次に示します。

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

`MonkeySearchHandler.OnQueryChanged` メソッドは、`Animal` オブジェクトの `List` を返します。 `DisplayMemberName` プロパティには各 `Animal` オブジェクトの `Name` プロパティが設定され、候補領域に表示されるデータは各動物の名前になります。

ユーザーが検索クエリを入力すると検索候補が表示されるように、`ShowsResults` プロパティは `true` に設定されます。

[![iOS および Android 上の Shell SearchHandler での検索結果のスクリーンショット](search-images/search-results.png "Shell SearchHandler の検索結果")](search-images/search-results-large.png#lightbox "Shell SearchHandler の検索結果")

検索クエリが変更されると、検索候補領域が更新されます。

[![iOS および Android 上の Shell SearchHandler での検索結果のスクリーンショット](search-images/search-results-change.png "Shell SearchHandler の検索結果")](search-images/search-results-change-large.png#lightbox "Shell SearchHandler の検索結果")

検索結果が選択されると、`MonkeyDetailPage` へナビゲートされ、選択されたサルに関するデータが表示されます。

[![iOS および Android 上のサルの詳細のスクリーンショット](search-images/detailpage.png "サルの詳細")](search-images/detailpage-large.png#lightbox "サルの詳細")

## <a name="define-search-results-item-appearance"></a>検索結果の項目の外観を定義する

検索結果での `string` データの表示に加えて、`SearchHandler.ItemTemplate` プロパティに [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を設定することで、各検索結果項目の外観を定義できます。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に指定された要素では、候補領域にある各項目の外観を定義します。 この例では、`DataTemplate` 内のレイアウトは [`Grid`](xref:Xamarin.Forms.Grid) によって管理されています。 `Grid` には [`Image`](xref:Xamarin.Forms.Image) オブジェクトと [`Label`](xref:Xamarin.Forms.Label) オブジェクトが格納され、両方とも各 `Monkey` オブジェクトのプロパティにバインドされています。

次のスクリーンショットは、候補領域内の各項目をテンプレート化した結果を示しています。

[![iOS および Android 上の Shell SearchHandler でのテンプレート化された検索結果のスクリーンショット](search-images/search-results-template.png "Shell SearchHandler のテンプレート化された検索結果")](search-images/search-results-template-large.png#lightbox "Shell SearchHandler のテンプレート化された検索結果")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="search-box-visibility"></a>検索ボックスの表示

`SearchHandler` がページの上部に追加されると、既定で検索ボックスが表示され、完全に展開されます。 しかし、`SearchHandler.SearchBoxVisibility` プロパティを `SearchBoxVisibility` 列挙メンバーのいずれかに設定することで、この動作を変更することができます。

- `Hidden` – 検索ボックスは表示されず、アクセスできません。
- `Collapsible` – 表示するためのアクションをユーザーが実行するまで、検索ボックスは非表示になります。 iOS では、ページのコンテンツを垂直方向にバウンスさせることで検索ボックスが表示されます。Android では、疑問符アイコンをタップすると検索ボックスが表示されます。
- `Expanded` – 検索ボックスは表示され、完全に展開されます。 これは、`SearchHandler.SearchBoxVisibility` プロパティの既定値です。

> [!IMPORTANT]
> iOS では、折りたたみ可能な検索ボックスには iOS 11 以上が必要です。

次の例は、検索ボックスを非表示にする方法を示しています。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:AnimalSearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="search-box-focus"></a>検索ボックスのフォーカス

検索ボックスをタップすると、検索ボックスが入力フォーカスを取得して、オンスクリーン キーボードが呼び出されます。 これは、検索ボックスに入力フォーカスを設定しようと試み、成功したら `true` を返す `Focus` メソッド呼び出すことによって、プログラムで実現することもできます。 検索ボックスがフォーカスを獲得すると、`Focus` イベントが発生し、オーバーライド可能な `OnFocused` メソッドが呼び出されます。

検索ボックスに入力フォーカスがあるときに、画面上の他の場所をタップすると、オンスクリーン キーボードが消去され、検索ボックスから入力フォーカスが失われます。 これも `Unfocus` メソッドを呼び出すことによって、プログラムで実現できます。 検索ボックスからフォーカスが失われると、`Unfocused` イベントが発生し、オーバーライド可能な `OnUnfocus` メソッドが呼び出されます。

検索ボックスのフォーカス状態は、`IsFocused` プロパティから取得できます。これは `SearchHandler` に現在入力フォーカスがある場合に `true` を返します。

## <a name="searchhandler-appearance"></a>SearchHandler の外観

`SearchHandler` クラスでは、その外観に影響する次のプロパティを定義します。

- `BackgroundColor`:`Color` 型、検索ボックスのテキストの背景の色。
- `CancelButtonColor`:`Color` 型、キャンセル ボタンの色。
- `CharacterSpacing`: `double` 型、`SearchHandler` テキストの文字間の間隔。
- `FontAttributes`:`FontAttributes` 型、ボ:ックスのテキストが斜体か太字かを示します。
- `FontFamily`:`string` 型、検索ボックスのテキストに使用されるフォント ファミリ。
- `FontSize`:`double` 型、検索ボックスのテキストのサイズ。
- `HorizontalTextAlignment`:`TextAlignment` 型、検索ボックスのテキストの水平方向の配置。
- `PlaceholderColor`:`Color` 型、プレースホルダー検索ボックスのテキストの色。
- `TextColor`:`Color` 型、検索ボックスのテキストの色。
- `VerticalTextAlignment`: `TextAlignment` 型、検索ボックスのテキストの垂直方向の配置。

## <a name="searchhandler-keyboard"></a>SearchHandler キーボード

ユーザーが `SearchHandler` とやりとりする際に表示されるキーボードは、プログラムで `Keyboard` プロパティによって、[`Keyboard`](xref:Xamarin.Forms.Keyboard) クラスの次のいずれかのプロパティに設定できます。

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) - 絵文字が使えるテキスト メッセージや場所に使います。
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) - 既定のキーボード。
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) - 電子メール アドレスを入力するときに使用します。
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) - 数値を入力するときに使用します。
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) - [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) を指定しないで、テキストを入力するときに使用します。
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) - 電話番号を入力するときに使用します。
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) - テキストを入力するときに使用します。
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) - ファイル パスおよび Web アドレスを入力するために使用します。

XAML では次のようにしてこれを実現できます。

```xaml
<SearchHandler Keyboard="Email" />
```

これに相当する C# コードを次に示します。

```csharp
SearchHandler searchHandler = new SearchHandler { Keyboard = Keyboard.Email };
```

[`Keyboard`](xref:Xamarin.Forms.Keyboard) クラスには、大文字の設定、スペルチェック、および単語補完候補の動作を指定することで、キーボードをカスタマイズするために使用できる [`Create`](xref:Xamarin.Forms.Keyboard.Create*) ファクトリ メソッドもあります。 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 列挙値がメソッドへの引数として指定され、カスタマイズされた `Keyboard` が返されます。 `KeyboardFlags` 列挙体には次の値が含まれます。

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) - キーボードに機能は追加されません。
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) - 入力された各文の最初の単語の最初の文字が自動的に大文字になることを示します。
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) - 入力したテキストに対してスペル チェックが実行されることを示します。
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) - 入力したテキストに対して単語補完が提供されることを示します。
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) - 各単語の最初の文字が自動的に大文字になることを示します。
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) - すべての文字が自動的に大文字になることを示します。
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) - 大文字の自動設定を行わないことを示します。
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) - 入力したテキストに対して、スペルチェック、単語補完、および文への大文字の設定が行われることを示します。

次の XAML コード例は、既定の [`Keyboard`](xref:Xamarin.Forms.Keyboard) をカスタマイズして、単語補完を提供し、入力したすべての文字を大文字に設定する方法を示しています。

```xaml
<SearchHandler Placeholder="Enter search terms">
    <SearchHandler.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </SearchHandler.Keyboard>
</SearchHandler>
```

これに相当する C# コードを次に示します。

```csharp
SearchHandler searchHandler = new SearchHandler { Placeholder = "Enter search terms" };
searchHandler.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

## <a name="searchhandler-reference"></a>SearchHandler リファレンス

`SearchHandler` クラスでは、外観と動作を制御する次のプロパティを定義します。

- `BackgroundColor`:`Color` 型、検索ボックスのテキストの背景の色。
- `CancelButtonColor`:`Color` 型、キャンセル ボタンの色。
- `ClearIcon`: [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型、検索ボックスの内容を消去するために表示されるアイコン。
- `ClearIconHelpText`: `string` 型、クリア アイコン用のアクセス可能なヘルプ テキスト。
- `ClearIconName`: `string` 型、スクリーン リーダーで使用するためのクリア アイコンの名前。
- `ClearPlaceholderCommand`: `ICommand` 型、`ClearPlaceholderIcon` がタップされたときに実行されます。
- `ClearPlaceholderCommandParameter`: `object` 型、`ClearPlaceholderCommand` に渡されるパラメーター。
- `ClearPlaceholderEnabled`: `bool` 型、`ClearPlaceholderCommand` を実行できるかどうかを判定します。 既定値は `true` です。
- `ClearPlaceholderHelpText`: `string` 型、クリア プレースホルダ― アイコンのためのアクセス可能なヘルプ テキスト。
- `ClearPlaceholderIcon`: [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型、検索ボックスが空の場合に表示されるクリア プレースホルダー アイコン。
- `ClearPlaceholderName`: `string` 型、スクリーン リーダーで使用するためのクリア プレースホルダー アイコンの名前。
- `Command`: `ICommand` 型、検索クエリが確定されたときに実行されます。
- `CommandParameter`: `object` 型、`Command` に渡されるパラメーター。
- `DisplayMemberName`: `string` 型、`ItemsSource` コレクションにあるデータの項目ごとに表示されるプロパティの名前またはパスを表します。
- `FontAttributes`:`FontAttributes` 型、ボ:ックスのテキストが斜体か太字かを示します。
- `FontFamily`:`string` 型、検索ボックスのテキストに使用されるフォント ファミリ。
- `FontSize`:`double` 型、検索ボックスのテキストのサイズ。
- `HorizontalTextAlignment`:`TextAlignment` 型、検索ボックスのテキストの水平方向の配置。
- `IsFocused`:`bool` 型、`SearchHandler` に現在入力フォーカスがあるかどうかを表します。
- `IsSearchEnabled`: `bool` 型、検索ボックスの有効化された状態を表します。 既定値は `true` です。
- `ItemsSource`: `IEnumerable` 型、候補領域に項目のコレクションが表示されるように指定します。また、既定値は `null` です。
- `ItemTemplate`: [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 型、候補領域に表示される項目のコレクション内の、各項目に適用するテンプレートを指定します。
- `Keyboard`: `Keyboard` 型、`SearchHandler`用のキーボードです。
- `Placeholder`: `string` 型、検索ボックスが空の場合に表示するテキスト。
- `PlaceholderColor`:`Color` 型、プレースホルダー検索ボックスのテキストの色。
- `Query`: `string` 型、ユーザーが検索ボックスに入力したテキスト。
- `QueryIcon`: [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型、検索が利用できることをユーザーに示すために使用されるアイコン。
- `QueryIconHelpText`: `string` 型、クエリ アイコン用のアクセス可能なヘルプ テキスト。
- `QueryIconName`: `string` 型、スクリーン リーダーで使用するためのクエリ アイコンの名前。
- `SearchBoxVisibility`: `SearchBoxVisibility` 型、検索ボックスの表示/非表示。 既定では、検索ボックスは表示され、完全に展開されています。
- `SelectedItem`: `object` 型、検索結果内の選択された項目。 このプロパティは読み取り専用であり、既定値は `null` です。
- `ShowsResults`: `bool` 型、テキスト入力時に、候補領域に検索結果が表示されるかどうかを指示します。 既定値は `false` です。
- `TextColor`:`Color` 型、検索ボックスのテキストの色。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

また、`SearchHandler`ク ラスでは、オーバーライドできる次のようなメソッドを提供しています。

- `OnClearPlaceholderClicked`: `ClearPlaceholderIcon` がタップされるたびに呼び出されます。
- `OnItemSelected`: ユーザーによって検索結果が選択されるたびに呼び出されます。
- `OnFocused`: `SearchHandler` が入力フォーカスを獲得したときに呼び出されます。
- `OnQueryChanged`: `Query` プロパティが変更されたときに呼び出されます。
- `OnQueryConfirmed`: ユーザーが Enter キーを押すか、または検索ボックスでクエリを確定するたびに、呼び出されます。
- `OnUnfocus`: `SearchHandler` から入力フォーカスが失われるときに呼び出されます。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms シェルのナビゲーション](navigation.md)
