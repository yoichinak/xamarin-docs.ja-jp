---
title: " Xamarin.Forms ToolbarItem" description: "ToolbarItem クラスは、アプリケーションのナビゲーションバーで使用される特殊なボタンです。"
ms. 製品: xamarin ms. assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1: xamarin-forms author: profexorgeek ms. author: jusjohns ms. date: 07/29/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-toolbaritem"></a>Xamarin.FormsToolbarItem

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

クラスは、 Xamarin.Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) オブジェクトのコレクションに追加できる特殊なボタンです `Page` `ToolbarItems` 。 各 `ToolbarItem` オブジェクトは、アプリケーションのナビゲーションバーにボタンとして表示されます。 `ToolbarItem`インスタンスはアイコンを持つことができ、プライマリまたはセカンダリのメニュー項目として表示されます。 クラスは、 `ToolbarItem` から継承さ [`MenuItem`](xref:Xamarin.Forms.MenuItem) れます。

次のスクリーンショットでは、 `ToolbarItem` iOS と Android のナビゲーションバーにオブジェクトが表示されています。

!["Android と iOS の ToolbarItem demo スクリーンショット"](toolbaritem-images/toolbaritem-device-screenshot.png "Android と iOS の ToolbarItem demo スクリーンショット")

`ToolbarItem`クラスは、次のプロパティを定義します。

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)インスタンスを `ToolbarItemOrder` `ToolbarItem` プライマリメニューとセカンダリメニューのどちらに表示するかを決定する列挙値です。
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)オブジェクトの `integer` コレクション内のアイテムの表示順序を決定する値です `Page` `ToolbarItems` 。

クラスは、 `ToolbarItem` クラスから次の一般的に使用されるプロパティを継承し `MenuItem` ます。

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)は、 `ICommand` ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにします。
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)`object`に渡す必要があるパラメーターを指定するです `Command` 。
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)`ImageSource`オブジェクトの表示アイコンを決定する値です `ToolbarItem` 。
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)は、 `string` オブジェクトの表示テキストを決定するです `ToolbarItem` 。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) `ToolbarItem` インスタンスがデータバインディングのターゲットになることができるように、オブジェクトによってバックアップされます。

> [!NOTE]
> オブジェクトからツールバーを作成する代わりに、 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 添付プロパティを複数のビューを含むレイアウトクラスに設定することもできます。 詳細については、「[ナビゲーションバーでのビューの表示](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar)」を参照してください。

## <a name="create-a-toolbaritem"></a>ToolbarItem を作成する

オブジェクトは、 `ToolbarItem` XAML でインスタンス化できます。 `Text`プロパティとプロパティを設定して、 `IconImageSource` ナビゲーションバーでのボタンの表示方法を決定できます。 次の例では、 `ToolbarItem` いくつかの共通プロパティセットを使用してをインスタンス化し、それをのコレクションに追加する方法を示し `ContentPage` `ToolbarItems` ます。

```xaml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Example Item"
                 IconImageSource="example_icon.png"
                 Order="Primary"
                 Priority="0" />
</ContentPage.ToolbarItems>
```

この例では、 `ToolbarItem` テキストとアイコンを持つオブジェクトが、プライマリナビゲーションバー領域に最初に表示されます。 は、 `ToolbarItem` コードで作成し、コレクションに追加することもでき `ToolbarItems` ます。

```csharp
ToolbarItem item = new ToolbarItem
{
    Text = "Example Item",
    IconImageSource = ImageSource.FromFile("example_icon.png"),
    Order = ToolbarItemOrder.Primary,
    Priority = 0
};

// "this" refers to a Page object
this.ToolbarItems.Add(item);
```

プロパティとして指定されたによって表されるファイルは、 `string` `IconImageSource` 各プラットフォームプロジェクトに存在している必要があります。

> [!NOTE]
> イメージ資産は、プラットフォームごとに異なる方法で処理されます。 は、 `ImageSource` ローカルファイル、埋め込みリソース、URI、またはストリームを含むソースから取得できます。 でのプロパティとイメージの設定の詳細について `IconImageSource` Xamarin.Forms は、「 [」の Xamarin.Forms 「イメージ](~/xamarin-forms/user-interface/images.md)」を参照してください。

## <a name="define-button-behavior"></a>ボタンの動作を定義する

クラスは、 `ToolbarItem` `Clicked` クラスからイベントを継承し `MenuItem` ます。 イベントハンドラーをイベントにアタッチし `Clicked` て、XAML でのインスタンスのタップまたはクリックに対応することができ `ToolbarItem` ます。

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

イベントハンドラーは、コード内でアタッチすることもできます。

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

前の例では、イベントハンドラーが参照されていま `OnItemClicked` した。 次のコードは、の実装例を示しています。

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem`オブジェクトでは、 `Command` プロパティとプロパティを使用して、イベントハンドラーを使用 `CommandParameter` せずにユーザー入力に応答することもできます。 `ICommand`インターフェイスと MVVM データバインディングの詳細については、「 [ Xamarin.Forms MenuItem MVVM Behavior](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm)」を参照してください。

## <a name="enable-or-disable-a-toolbaritem-at-runtime"></a>実行時に ToolbarItem を有効または無効にする

実行時にを無効にするには、 `ToolbarItem` そのプロパティを実装にバインドし、デリゲートがを有効にし、必要に応じて無効にするようにし `Command` `ICommand` `canExecute` `ICommand` ます。

詳細については、「[実行時に MenuItem を有効または無効](menuitem.md#enable-or-disable-a-menuitem-at-runtime)にする」を参照してください。

## <a name="primary-and-secondary-menus"></a>プライマリメニューとセカンダリメニュー

`ToolbarItemOrder`列挙型には `Default` 、、、およびの各値があり `Primary` `Secondary` ます。

プロパティがに設定されている場合、オブジェクトは、 `Order` `Primary` `ToolbarItem` すべてのプラットフォームのメインナビゲーションバーに表示されます。 `ToolbarItem`オブジェクトは、ページタイトルに対して優先順位が付けられます。これは、項目の領域を確保するために切り捨てられます。 次のスクリーンショットは、 `ToolbarItem` iOS および Android のプライマリメニューのオブジェクトを示しています。

!["ToolbarItem のプライマリメニューのスクリーンショット Android と iOS"](toolbaritem-images/toolbaritem-primary-menu.png "ToolbarItem Android と iOS のプライマリメニューのスクリーンショット")

`Order`プロパティがに設定されている場合 `Secondary` 、動作はプラットフォームによって異なります。 UWP と Android では、[ `Secondary` 項目] メニューが3つのドットとして表示され、タップまたはクリックして縦の一覧の項目を表示できます。 IOS では、[ `Secondary` 項目] メニューは、ナビゲーションバーの下に水平方向の一覧として表示されます。 次のスクリーンショットは、iOS と Android のセカンダリメニューを示しています。

!["ToolbarItem のセカンダリメニューのスクリーンショット Android と iOS"](toolbaritem-images/toolbaritem-secondary-menu.png "ToolbarItem Android と iOS でのセカンダリメニューのスクリーンショット")

> [!WARNING]
> `ToolbarItem`プロパティがに設定されているオブジェクトでのアイコンの動作 `Order` は、 `Secondary` プラットフォーム間で一貫性がありません。 `IconImageSource`セカンダリメニューに表示される項目のプロパティを設定しないようにしてください。

## <a name="related-links"></a>関連リンク

* [ToolbarItem のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [画像Xamarin.Forms](~/xamarin-forms/user-interface/images.md)
* [Xamarin.FormsMenuItem](~/xamarin-forms/user-interface/menuitem.md)
