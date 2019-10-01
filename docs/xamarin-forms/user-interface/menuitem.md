---
title: Xamarin. フォーム MenuItem
description: MenuItem クラスは、ListView 項目のコンテキストメニューやシェルアプリケーションのポップアップメニューなどのメニューのメニュー項目を作成するために使用されます。
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
ms.openlocfilehash: cbc39ee38ce623ce446d50494829119058fc88dc
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976473"
---
# <a name="xamarinforms-menuitem"></a>Xamarin. フォーム MenuItem

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitem/)

Xamarin. Forms [`MenuItem`](xref:Xamarin.Forms.MenuItem)クラスは、 `ListView`項目のコンテキストメニューやシェルアプリケーションのポップアップメニューなどのメニューのメニュー項目を定義します。

次のスクリーンショット`MenuItem`は、iOS `ListView`および Android のコンテキストメニューのオブジェクトを示しています。

[!["iOS と Android の MenuItems"](menuitem-images/menuitem-demo-cropped.png "iOS と Android の MenuItems")](menuitem-images/menuitem-demo-full.png#lightbox "iOS と Android の MenuItems の全体イメージ")

クラス`MenuItem`は、次のプロパティを定義します。

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)は、 `ICommand`ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにします。
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)に渡す必要があるパラメーターを指定`object`するです`Command`。
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)表示アイコンを定義する値です。`ImageSource`
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)は、`MenuItem`関連付けられている UI 要素をリストから削除するかどうかを示す値です。`bool`
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)このオブジェクトがユーザー入力に応答するかどうかを示す値です。`bool`
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)表示テキストを指定する値です。`string`

これらのプロパティはオブジェクト[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)によって`MenuItem`サポートされるため、インスタンスをデータバインディングのターゲットにすることができます。

## <a name="create-a-menuitem"></a>MenuItem を作成する

`MenuItem`オブジェクトは、 `ListView`オブジェクトの項目のコンテキストメニュー内で使用できます。 最も一般的なパターン`MenuItem`は、 `ViewCell`インスタンス内にオブジェクトを作成することです`ItemTemplate`。 `DataTemplate`これは、 `ListView`のオブジェクトとして使用されます。 オブジェクトが設定されると、 `DataTemplate`を使用して各項目が作成`MenuItem`され、コンテキストメニューが項目に対してアクティブ化されたときに選択肢が公開されます。 `ListView`

次の例は`MenuItem` 、 `ListView`オブジェクトのコンテキスト内でのインスタンス化を示しています。

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

は`MenuItem` 、コードで作成することもできます。

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

`MenuItem` クラスは、`Clicked` イベントを公開します。 このイベントにイベントハンドラーをアタッチして、 `MenuItem` XAML のインスタンスでのタップまたはクリックに対応することができます。

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

イベントハンドラーは、コード内でアタッチすることもできます。

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

前の例で`OnItemClicked`は、イベントハンドラーが参照されていました。 次のコードは、の実装例を示しています。

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

クラス`MenuItem`は、オブジェクトと`ICommand`インターフェイスを使用して[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 、モデルビュービューモデル (MVVM) パターンをサポートします。 次の XAML は`MenuItem` 、ビューモデルで定義されているコマンドにバインドされたインスタンスを示しています。

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

前の例では、 `MenuItem`ビューモデルのコマンドに`Command`バインド`CommandParameter`されたプロパティとプロパティを使用して、2つのオブジェクトを定義しています。 ビューモデルには、XAML で参照されるコマンドが含まれています。

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

サンプルアプリケーションには、 `DataService` `ListView`オブジェクトを設定するための項目の一覧を取得するために使用されるクラスが含まれています。 ビューモデルは、 `DataService`クラスの項目と共にインスタンス化され、分離コードのと`BindingContext`して設定されます。

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>MenuItem アイコン

> [!WARNING]
> `MenuItem`オブジェクトには、Android のアイコンのみが表示されます。 他のプラットフォームでは、 `Text`プロパティによって指定されたテキストのみが表示されます。

 アイコンは、 `IconImageSource`プロパティを使用して指定します。 アイコンが指定されている場合、 `Text`プロパティによって指定されたテキストは表示されません。 次のスクリーンショットは`MenuItem` 、Android 上のアイコンを含むを示しています。

!["Android の MenuItem アイコンのスクリーンショット"](menuitem-images/menuitem-android-icon.png "Android の MenuItem アイコンのスクリーンショット")

Xamarin. フォームでイメージを使用する方法の詳細については、「 [xamarin. forms の画像](~/xamarin-forms/user-interface/images.md)」を参照してください。

## <a name="cross-platform-context-menu-behavior"></a>クロスプラットフォームコンテキストメニューの動作

コンテキストメニューには、各プラットフォームでアクセスし、異なる方法で表示できます。

Android では、コンテキストメニューは、リスト項目に対して長いプレスによってアクティブ化されます。 コンテキストメニューはタイトルとナビゲーションバー領域`MenuItem`を置き換え、オプションは水平ボタンとして表示されます。

!["Android のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-android-icon.png "Android のコンテキストメニューのスクリーンショット")

IOS では、コンテキストメニューは、リスト項目のスワイプによってアクティブ化されます。 コンテキストメニューがリスト項目`MenuItems`に表示され、水平ボタンとして表示されます。

!["IOS のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-ios-contextmenu.png "IOS のコンテキストメニューのスクリーンショット")

UWP では、コンテキストメニューは、リスト項目を右クリックすることによってアクティブ化されます。 コンテキストメニューは、カーソルの近くに垂直リストとして表示されます。

!["UWP のコンテキストメニューのスクリーンショット"](menuitem-images/menuitem-uwp.png "UWP のコンテキストメニューのスクリーンショット")

## <a name="related-links"></a>関連リンク

* [MenuItem のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitem/)
* [Xamarin 形式の画像](~/xamarin-forms/user-interface/images.md)
