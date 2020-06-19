---
title: Xamarin.FormsMenuItem
description: MenuItem クラスは、ListView 項目のコンテキストメニューやシェルアプリケーションのポップアップメニューなどのメニューのメニュー項目を作成するために使用されます。
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6b27f778a417a2bc0b458af4214ee8cb914fd93d
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990854"
---
# <a name="xamarinforms-menuitem"></a>Xamarin.FormsMenuItem

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)

クラスは、 Xamarin.Forms [`MenuItem`](xref:Xamarin.Forms.MenuItem) `ListView` 項目のコンテキストメニューやシェルアプリケーションのポップアップメニューなどのメニューのメニュー項目を定義します。

次のスクリーンショットは、 `MenuItem` `ListView` IOS および Android のコンテキストメニューのオブジェクトを示しています。

[!["IOS と Android での MenuItems"](menuitem-images/menuitem-demo-cropped.png "IOS と Android の MenuItems")](menuitem-images/menuitem-demo-full.png#lightbox "IOS と Android のフルイメージでの MenuItems")

`MenuItem`クラスは、次のプロパティを定義します。

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)は、 `ICommand` ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにします。
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)`object`に渡す必要があるパラメーターを指定するです `Command` 。
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)`ImageSource`表示アイコンを定義する値です。
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)は `bool` 、 `MenuItem` 関連付けられている UI 要素をリストから削除するかどうかを示す値です。
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)`bool`このオブジェクトがユーザー入力に応答するかどうかを示す値です。
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)`string`表示テキストを指定する値です。

これらのプロパティはオブジェクトによってサポートさ [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) れるため、 `MenuItem` インスタンスをデータバインディングのターゲットにすることができます。

## <a name="create-a-menuitem"></a>MenuItem を作成する

`MenuItem`オブジェクトは、オブジェクトの項目のコンテキストメニュー内で使用でき `ListView` ます。 最も一般的なパターンは、インスタンス内にオブジェクトを作成することです `MenuItem` `ViewCell` 。これは、のオブジェクトとして使用され `DataTemplate` `ListView` `ItemTemplate` ます。 `ListView`オブジェクトが設定されると、を使用して各項目が作成され `DataTemplate` 、 `MenuItem` コンテキストメニューが項目に対してアクティブ化されたときに選択肢が公開されます。

次の例は、 `MenuItem` オブジェクトのコンテキスト内でのインスタンス化を示してい `ListView` ます。

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

は、 `MenuItem` コードで作成することもできます。

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

`MenuItem` クラスは、`Clicked` イベントを公開します。 このイベントにイベントハンドラーをアタッチして、XAML のインスタンスでのタップまたはクリックに対応することができ `MenuItem` ます。

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

イベントハンドラーは、コード内でアタッチすることもできます。

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

前の例では、イベントハンドラーが参照されていま `OnItemClicked` した。 次のコードは、の実装例を示しています。

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

クラスは、 `MenuItem` オブジェクトとインターフェイスを使用して、モデルビュービューモデル (MVVM) パターンをサポートし [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) `ICommand` ます。 次の XAML は、 `MenuItem` ビューモデルで定義されているコマンドにバインドされたインスタンスを示しています。

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

前の例では、 `MenuItem` `Command` `CommandParameter` ビューモデルのコマンドにバインドされたプロパティとプロパティを使用して、2つのオブジェクトを定義しています。 ビューモデルには、XAML で参照されるコマンドが含まれています。

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

サンプルアプリケーションには、 `DataService` オブジェクトを設定するための項目の一覧を取得するために使用されるクラスが含まれてい `ListView` ます。 ビューモデルは、クラスの項目と共にインスタンス化され、 `DataService` 分離コードのとして設定され `BindingContext` ます。

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>MenuItem アイコン

> [!WARNING]
> `MenuItem`オブジェクトには、Android のアイコンのみが表示されます。 他のプラットフォームでは、プロパティによって指定されたテキストのみ `Text` が表示されます。

 アイコンは、プロパティを使用して指定し `IconImageSource` ます。 アイコンが指定されている場合、プロパティによって指定されたテキストは表示され `Text` ません。 次のスクリーンショットは、 `MenuItem` Android 上のアイコンを含むを示しています。

!["Android の MenuItem アイコンのスクリーンショット"](menuitem-images/menuitem-android-icon.png "Android の MenuItem アイコンのスクリーンショット")

でのイメージの使用方法の詳細について Xamarin.Forms は、「 [」の Xamarin.Forms 「イメージ](~/xamarin-forms/user-interface/images.md)」を参照してください。

## <a name="enable-or-disable-a-menuitem-at-runtime"></a>実行時に MenuItem を有効または無効にする

実行時にを無効にするには、 `MenuItem` そのプロパティを実装にバインドし、デリゲートがを有効にし、必要に応じて無効にするようにし `Command` `ICommand` `canExecute` `ICommand` ます。

> [!IMPORTANT]
> プロパティを使用し `IsEnabled` てを `Command` 有効または無効にする場合は、プロパティを別のプロパティにバインドしないで `MenuItem` ください。

次の例は、 `MenuItem` `Command` プロパティがという名前のにバインドされているを示してい `ICommand` `MyCommand` ます。

```xaml
<MenuItem Text="My menu item"
          Command="{Binding MyCommand}" />
```

`ICommand`実装で `canExecute` は、プロパティの値を返すデリゲートを `bool` 使用して、を有効または無効にする必要があり `MenuItem` ます。

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

    public MyViewModel()
    {
        MyCommand = new Command(() =>
        {
            // Execute logic here
        },
        () => IsMenuItemEnabled);
    }
}
```

この例では、 `MenuItem` プロパティが設定されるまで、は無効になってい `IsMenuItemEnabled` ます。 このエラーが発生すると、 `Command.ChangeCanExecute` メソッドが呼び出され、 `canExecute` のデリゲートが `MyCommand` 再評価されます。

## <a name="cross-platform-context-menu-behavior"></a>クロスプラットフォームコンテキストメニューの動作

コンテキストメニューには、各プラットフォームでアクセスし、異なる方法で表示できます。

Android では、コンテキストメニューは、リスト項目に対して長いプレスによってアクティブ化されます。 コンテキストメニューはタイトルとナビゲーションバー領域を置き換え、 `MenuItem` オプションは水平ボタンとして表示されます。

!["Android のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-android-icon.png "Android のコンテキストメニューのスクリーンショット")

IOS では、コンテキストメニューは、リスト項目のスワイプによってアクティブ化されます。 コンテキストメニューがリスト項目に表示され、 `MenuItems` 水平ボタンとして表示されます。

!["IOS のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-ios-contextmenu.png "IOS のコンテキストメニューのスクリーンショット")

UWP では、コンテキストメニューは、リスト項目を右クリックすることによってアクティブ化されます。 コンテキストメニューは、カーソルの近くに垂直リストとして表示されます。

!["UWP のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-uwp.png "UWP のコンテキストメニューのスクリーンショット")

## <a name="related-links"></a>関連リンク

* [MenuItem のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)
* [画像Xamarin.Forms](~/xamarin-forms/user-interface/images.md)
