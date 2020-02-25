---
title: Xamarin. フォーム MenuItem
description: MenuItem クラスは、ListView 項目のコンテキストメニューやシェルアプリケーションのポップアップメニューなどのメニューのメニュー項目を作成するために使用されます。
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
ms.openlocfilehash: b4690feb6444405d090a0b2bafd6c8615b2ffa8b
ms.sourcegitcommit: 6d86aac422d6ce2131930d18ada161d117c8c61b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2020
ms.locfileid: "77567069"
---
# <a name="xamarinforms-menuitem"></a>Xamarin. フォーム MenuItem

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)

Xamarin [`MenuItem`](xref:Xamarin.Forms.MenuItem)クラスは、`ListView` 項目のコンテキストメニューやシェルアプリケーションのポップアップメニューなどのメニューのメニュー項目を定義します。

次のスクリーンショットは、iOS および Android の `ListView` のコンテキストメニューに `MenuItem` オブジェクトを示しています。

[!["IOS と Android での MenuItems"](menuitem-images/menuitem-demo-cropped.png "IOS と Android の MenuItems")](menuitem-images/menuitem-demo-full.png#lightbox "IOS と Android のフルイメージでの MenuItems")

`MenuItem` クラスは、次のプロパティを定義します。

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)は、ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにする `ICommand` です。
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)は、`Command`に渡す必要があるパラメーターを指定する `object` です。
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)は、表示アイコンを定義する `ImageSource` 値です。
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)は、`MenuItem` が関連する UI 要素を一覧から削除するかどうかを示す `bool` 値です。
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)は、このオブジェクトがユーザー入力に応答するかどうかを示す `bool` 値です。
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)は、表示テキストを指定する `string` 値です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされるため、`MenuItem` インスタンスをデータバインディングのターゲットにすることができます。

## <a name="create-a-menuitem"></a>MenuItem を作成する

`MenuItem` オブジェクトは、`ListView` オブジェクトの項目のコンテキストメニュー内で使用できます。 最も一般的なパターンは、`ViewCell` インスタンス内に `MenuItem` オブジェクトを作成することです。これは、`ListView`s `ItemTemplate`の `DataTemplate` オブジェクトとして使用されます。 `ListView` オブジェクトに値が設定されると、`DataTemplate`を使用して各項目が作成され、項目に対してコンテキストメニューがアクティブになったときに `MenuItem` の選択肢が公開されます。

次の例では、`ListView` オブジェクトのコンテキスト内でインスタンス化 `MenuItem` を示します。

```xaml
<ListView>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.ContextActions>
                    <MenuItem Text="Context Menu Option" />
                </ViewCell.ContextActions>
                <Label Text="{Binding .}" />
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

コードでは、`MenuItem` を作成することもできます。

```csharp
// A function returns a ViewCell instance that
// is used as the template for each list item
DataTemplate dataTemplate = new DataTemplate(() =>
{
    // A Label displays the list item text
    Label label = new Label();
    label.SetBinding(Label.TextProperty, ".");

    // A ViewCell serves as the DataTemplate
    ViewCell viewCell = new ViewCell
    {
        View = label
    };

    // Add a MenuItem instance to the ContextActions
    MenuItem menuItem = new MenuItem
    {
        Text = "Context Menu Option"
    };
    viewCell.ContextActions.Add(menuItem);

    // The function returns the custom ViewCell
    // to the DataTemplate constructor
    return viewCell;
});

// Finally, the dataTemplate is provided to
// the ListView object
ListView listView = new ListView
{
    ...
    ItemTemplate = dataTemplate
};
```

## <a name="define-menuitem-behavior-with-events"></a>イベントで MenuItem の動作を定義する

`MenuItem` クラスは、`Clicked` イベントを公開します。 このイベントにイベントハンドラーをアタッチして、XAML の `MenuItem` インスタンスでのタップまたはクリックに対応することができます。

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

イベントハンドラーは、コード内でアタッチすることもできます。

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

前の例では、`OnItemClicked` イベントハンドラーが参照されていました。 次のコードは、の実装例を示しています。

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    // The sender is the menuItem
    MenuItem menuItem = sender as MenuItem;

    // Access the list item through the BindingContext
    var contextItem = menuItem.BindingContext;

    // Do something with the contextItem here
}
```

## <a name="define-menuitem-behavior-with-mvvm"></a>MVVM で MenuItem の動作を定義する

`MenuItem` クラスは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトと `ICommand` インターフェイスを使用して、モデルビュービューモデル (MVVM) パターンをサポートします。 次の XAML は、ビューモデルで定義されているコマンドにバインドされている `MenuItem` インスタンスを示しています。

```xaml
<ContentPage.BindingContext>
    <viewmodels:ListPageViewModel />
</ContentPage.BindingContext>

<StackLayout>
    <Label Text="{Binding Message}" ... />
    <ListView ItemsSource="{Binding Items}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ViewCell.ContextActions>
                        <MenuItem Text="Edit"
                                    IconImageSource="icon.png"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.EditCommand}"
                                    CommandParameter="{Binding .}"/>
                        <MenuItem Text="Delete"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.DeleteCommand}"
                                    CommandParameter="{Binding .}"/>
                    </ViewCell.ContextActions>
                    <Label Text="{Binding .}" />
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

前の例では、2つの `MenuItem` オブジェクトが、`Command`、およびビューモデルのコマンドにバインドされた `CommandParameter` プロパティで定義されています。 ビューモデルには、XAML で参照されるコマンドが含まれています。

```csharp
public class ListPageViewModel : INotifyPropertyChanged
{
    ...

    public ICommand EditCommand => new Command<string>((string item) =>
    {
        Message = $"Edit command was called on: {item}";
    });

    public ICommand DeleteCommand => new Command<string>((string item) =>
    {
        Message = $"Delete command was called on: {item}";
    });
}
```

サンプルアプリケーションには、`ListView` オブジェクトを設定するための項目の一覧を取得するために使用される `DataService` クラスが含まれています。 ビューモデルは、`DataService` クラスの項目を使用してインスタンス化され、分離コードの `BindingContext` として設定されます。

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>MenuItem アイコン

> [!WARNING]
> `MenuItem` オブジェクトには、Android 上のアイコンのみが表示されます。 他のプラットフォームでは、`Text` プロパティによって指定されたテキストだけが表示されます。

 アイコンは、`IconImageSource` プロパティを使用して指定します。 アイコンが指定されている場合、`Text` プロパティによって指定されたテキストは表示されません。 次のスクリーンショットは、Android 上のアイコンを含む `MenuItem` を示しています。

!["Android の MenuItem アイコンのスクリーンショット"](menuitem-images/menuitem-android-icon.png "Android の MenuItem アイコンのスクリーンショット")

Xamarin. フォームでイメージを使用する方法の詳細については、「 [xamarin. forms の画像](~/xamarin-forms/user-interface/images.md)」を参照してください。

## <a name="enable-or-disable-a-menuitem-at-runtime"></a>実行時に MenuItem を有効または無効にする

実行時に `MenuItem` を無効にするには、`Command` プロパティを `ICommand` 実装にバインドし、`canExecute` デリゲートが適切に `ICommand` を有効または無効にするようにします。

> [!IMPORTANT]
> `Command` プロパティを使用して `MenuItem`を有効または無効にする場合は、`IsEnabled` プロパティを別のプロパティにバインドしないでください。

次の例では、`Command` プロパティが `MyCommand`という名前の `ICommand` にバインドされている `MenuItem` を示します。

```xaml
<MenuItem Text="My menu item"
          Command="{Binding MyCommand}" />
```

`ICommand` の実装では、`MenuItem`を有効または無効にするために `bool` プロパティの値を返す `canExecute` デリゲートが必要です。

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    bool isMenuItemEnabled = false;
    public bool IsMenuItemEnabled
    {
        get { return isMenuItemEnabled; }
        set
        {
            isMenuItemEnabled = value;
            MyCommand.ChangeCanExecute();
        }
    }

    public Command MyCommand { get; private set; }

    public ToolbarItemViewModel()
    {
        MyCommand = new Command(() =>
        {
            // Execute logic here
        },
        () => IsToolbarItemEnabled);
    }
}
```

この例では、`IsMenuItemEnabled` プロパティが設定されるまで、`MenuItem` は無効になっています。 このエラーが発生すると、`Command.ChangeCanExecute` メソッドが呼び出され、`MyCommand` の `canExecute` デリゲートが再評価されます。

## <a name="cross-platform-context-menu-behavior"></a>クロスプラットフォームコンテキストメニューの動作

コンテキストメニューには、各プラットフォームでアクセスし、異なる方法で表示できます。

Android では、コンテキストメニューは、リスト項目に対して長いプレスによってアクティブ化されます。 コンテキストメニューは、タイトルとナビゲーションバー領域に置き換わるものであり、`MenuItem` オプションは水平ボタンとして表示されます。

!["Android のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-android-icon.png "Android のコンテキストメニューのスクリーンショット")

IOS では、コンテキストメニューは、リスト項目のスワイプによってアクティブ化されます。 コンテキストメニューがリスト項目に表示され、`MenuItems` が横ボタンとして表示されます。

!["IOS のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-ios-contextmenu.png "IOS のコンテキストメニューのスクリーンショット")

UWP では、コンテキストメニューは、リスト項目を右クリックすることによってアクティブ化されます。 コンテキストメニューは、カーソルの近くに垂直リストとして表示されます。

!["UWP のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-uwp.png "UWP のコンテキストメニューのスクリーンショット")

## <a name="related-links"></a>関連リンク

* [MenuItem のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)
* [Xamarin 形式の画像](~/xamarin-forms/user-interface/images.md)
