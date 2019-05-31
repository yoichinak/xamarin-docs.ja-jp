---
title: Xamarin.Forms シェルでの検索
description: Xamarin.Forms シェル アプリケーションでは、各ページの上部に追加できる検索ボックスとして提供される、統合された検索機能を利用できます。
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: acaa847b61443eff480e2b39e388f5df9de06e42
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054422"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms シェル

![](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

Xamarin.Forms シェルには、`SearchHandler` クラスによって提供される統合された検索機能が組み込まれています。 検索機能は、サブクラス化された `SearchHandler` オブジェクトを `Shell.SearchHandler` 添付プロパティに設定することで、ページに追加できます。 これにより、検索ボックスがページの上部に追加されます。

[![iOS および Android 上での、シェルの SearchHandler のスクリーンショット](search-images/searchhandler.png "シェルの SearchHandler")](search-images/searchhandler-large.png#lightbox "シェルの SearchHandler")

検索ボックスにクエリが入力されると、検索候補領域にデータが設定される場合があります。

[![iOS および Android 上での、シェルの SearchHandler の検索結果を示したスクリーンショット](search-images/search-suggestions.png "シェルの SearchHandler の検索結果")](search-images/search-suggestions-large.png#lightbox "シェルの SearchHandler の検索結果")

その後、1 つの検索結果が選択されると、アプリケーションは、別のページに移動するなど、適切に応答することができます。

## <a name="searchhandler-class"></a>SearchHandler クラス

`SearchHandler` クラスでは、外観と動作を制御する次のプロパティを定義します。

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
- `IsSearchEnabled`: `bool` 型、検索ボックスの有効化された状態を表します。 既定値は `true` です。
- `ItemsSource`: `IEnumerable` 型、候補領域に項目のコレクションが表示されるように指定します。また、既定値は `null` です。
- `ItemTemplate`: [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 型、候補領域に表示される項目のコレクション内の、各項目に適用するテンプレートを指定します。
- `Placeholder`: `string` 型、検索ボックスが空の場合に表示するテキスト。
- `Query`: `string` 型、ユーザーが検索ボックスに入力したテキスト。
- `QueryIcon`: [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型、検索が利用できることをユーザーに示すために使用されるアイコン。
- `QueryIconHelpText`: `string` 型、クエリ アイコン用のアクセス可能なヘルプ テキスト。
- `QueryIconName`: `string` 型、スクリーン リーダーで使用するためのクエリ アイコンの名前。
- `SearchBoxVisibility`: `SearchBoxVisibility` 型、検索ボックスの表示/非表示。 既定では、検索ボックスは表示され、完全に展開されています。
- `SelectedItem`: `object` 型、検索結果内の選択された項目。 このプロパティは読み取り専用であり、既定値は `null` です。
- `ShowsResults`: `bool` 型、テキスト入力時に、候補領域に検索結果が表示されるかどうかを指示します。 既定値は `false` です。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティがデータ バインディングの対象になる場合があります。

また、`SearchHandler`ク ラスでは、オーバーライドできる次のようなメソッドを提供しています。

- `OnClearPlaceholderClicked`: `ClearPlaceholderIcon` がタップされるたびに呼び出されます。
- `OnItemSelected`: ユーザーによって検索結果が選択されるたびに呼び出されます。
- `OnQueryChanged`: `Query` プロパティが変更されたときに呼び出されます。
- `OnQueryConfirmed`: ユーザーが Enter キーを押すか、または検索ボックスでクエリを確定するたびに、呼び出されます。

ユーザーが検索ボックスにクエリを入力すると、`Query` プロパティが更新され、更新ごとに `OnQueryChanged` メソッドが実行されます。 このメソッドは、検索ボックス下に表示される候補領域を更新するために使用できます。 ユーザーが候補領域から結果を選択すると、`OnItemSelected` メソッドが実行されます。

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

[![iOS および Android 上での、シェルの SearchHandler の検索結果を示したスクリーンショット](search-images/search-results.png "シェルの SearchHandler の検索結果")](search-images/search-results-large.png#lightbox "シェルの SearchHandler の検索結果")

検索クエリが変更されると、検索候補領域が更新されます。

[![iOS および Android 上での、シェルの SearchHandler の検索結果を示したスクリーンショット](search-images/search-results-change.png "シェルの SearchHandler の検索結果")](search-images/search-results-change-large.png#lightbox "シェルの SearchHandler の検索結果")

検索結果が選択されると、`MonkeyDetailPage` へナビゲートされ、選択されたサルに関するデータが表示されます。

[![iOS および Android 上での、サルの詳細のスクリーンショット](search-images/detailpage.png "サルの詳細")](search-images/detailpage-large.png#lightbox "サルの詳細")

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

該当の C# コードを次に示します。

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

[![iOS および Android 上での、シェルの SearchHandler におけるテンプレート化された検索結果のスクリーンショット](search-images/search-results-template.png "シェルの SearchHandler のテンプレート化された検索結果")](search-images/search-results-template-large.png#lightbox "シェルの SearchHandler のテンプレート化された検索結果")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="searchbox-visibility"></a>検索ボックスの表示/非表示

検索ボックスがページの上部に追加されると、既定で検索ボックスが表示され、完全に展開されます。 しかし、`SearchHandler.SearchBoxVisibility` プロパティを `SearchBoxVisibility` 列挙メンバーのいずれかに設定することで、この動作を変更することができます。

- `Hidden` – 検索ボックスは表示されず、アクセスできません。
- `Collapsible` – 表示するためのアクションをユーザーが実行するまで、検索ボックスは非表示になります。
- `Expanded` – 検索ボックスは表示され、完全に展開されます。

次の例は、検索ボックスを非表示にする方法を示しています。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
- [Xamarin.Forms シェルのナビゲーション](navigation.md)
