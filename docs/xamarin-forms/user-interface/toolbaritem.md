---
title: ToolbarItem
description: ToolbarItem クラスは、アプリケーションのナビゲーションバーで使用される特殊なボタンです。
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: 0812347e85b0ccb6aa0bbb16649a89bb4d961c9b
ms.sourcegitcommit: a14edebf00f3e0f8944e59042ca7aa5c42173e30
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72780355"
---
# <a name="xamarinforms-toolbaritem"></a>ToolbarItem

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Xamarin [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)クラスは、`Page` オブジェクトの `ToolbarItems` コレクションに追加できる特殊な種類のボタンです。 各 `ToolbarItem` オブジェクトは、アプリケーションのナビゲーションバーにボタンとして表示されます。 `ToolbarItem` インスタンスはアイコンを持つことができ、プライマリまたはセカンダリのメニュー項目として表示されます。 `ToolbarItem` クラスは[`MenuItem`](xref:Xamarin.Forms.MenuItem)から継承されます。

次のスクリーンショットは、iOS および Android のナビゲーションバーに `ToolbarItem` オブジェクトを示しています。

!["Android と iOS の ToolbarItem demo スクリーンショット"](toolbaritem-images/toolbaritem-device-screenshot.png "Android と iOS の ToolbarItem demo スクリーンショット")

`ToolbarItem` クラスは、次のプロパティを定義します。

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)は、プライマリメニューまたはセカンダリメニューに `ToolbarItem` インスタンスを表示するかどうかを決定する `ToolbarItemOrder` 列挙値です。
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)は、`Page` オブジェクトの `ToolbarItems` コレクション内の項目の表示順序を決定する `integer` 値です。

`ToolbarItem` クラスは、次の一般的に使用されるプロパティを `MenuItem` クラスから継承します。

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)は、ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにする `ICommand` です。
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)は、`Command` に渡す必要があるパラメーターを指定する `object` です。
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)は、`ToolbarItem` オブジェクトの表示アイコンを決定する `ImageSource` 値です。
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)は、`ToolbarItem` オブジェクトの表示テキストを決定する `string` です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによってサポートされるため、`ToolbarItem` インスタンスをデータバインディングのターゲットにすることができます。

> [!NOTE]
> [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)オブジェクトからツールバーを作成する代わりに、 [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty)添付プロパティを複数のビューを含むレイアウトクラスに設定することもできます。 詳細については、「[ナビゲーションバーでのビューの表示](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar)」を参照してください。

## <a name="create-a-toolbaritem"></a>ToolbarItem を作成する

`ToolbarItem` オブジェクトは、XAML でインスタンス化できます。 `Text` プロパティと `IconImageSource` プロパティを設定して、ナビゲーションバーでのボタンの表示方法を決定できます。 次の例は、いくつかの共通プロパティが設定された `ToolbarItem` をインスタンス化し、それを `ContentPage`の `ToolbarItems` コレクションに追加する方法を示しています。

```xaml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Example Item"
                 IconImageSource="example_icon.png"
                 Order="Primary"
                 Priority="0" />
</ContentPage.ToolbarItems>
```

この例では、テキストとアイコンを持つ `ToolbarItem` オブジェクトが、プライマリナビゲーションバー領域に最初に表示されます。 `ToolbarItem` は、コードで作成し、`ToolbarItems` コレクションに追加することもできます。

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

`IconImageSource` プロパティとして指定された `string`によって表されるファイルは、各プラットフォームプロジェクトに存在している必要があります。

> [!NOTE]
> イメージ資産は、プラットフォームごとに異なる方法で処理されます。 `ImageSource` は、ローカルファイル、埋め込みリソース、URI、またはストリームを含むソースから取得できます。 Xamarin. フォームで `IconImageSource` プロパティとイメージを設定する方法の詳細については、「 [xamarin. forms の画像](~/xamarin-forms/user-interface/images.md)」を参照してください。

## <a name="define-button-behavior"></a>ボタンの動作を定義する

`ToolbarItem` クラスは、`MenuItem` クラスから `Clicked` イベントを継承します。 イベントハンドラーを `Clicked` イベントにアタッチすると、XAML で `ToolbarItem` インスタンスに対するタップまたはクリックに対応できます。

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

イベントハンドラーは、コード内でアタッチすることもできます。

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

前の例では、`OnItemClicked` イベントハンドラーが参照されていました。 次のコードは、の実装例を示しています。

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem` オブジェクトは、`Command` プロパティと `CommandParameter` プロパティを使用して、イベントハンドラーを使用せずにユーザー入力に応答することもできます。 `ICommand` インターフェイスと MVVM データバインディングの詳細については、「 [MVVM MenuItem の動作](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm)」を参照してください。

## <a name="primary-and-secondary-menus"></a>プライマリメニューとセカンダリメニュー

`ToolbarItemOrder` 列挙型には、`Default`、`Primary`、および `Secondary` の値があります。

`Order` プロパティが `Primary`に設定されている場合、`ToolbarItem` オブジェクトは、すべてのプラットフォームのメインナビゲーションバーに表示されます。 `ToolbarItem` オブジェクトは、ページタイトルに優先順位が付けられます。これは、項目のスペースを確保するために切り捨てられます。 次のスクリーンショットは、iOS および Android のプライマリメニューの `ToolbarItem` オブジェクトを示しています。

!["ToolbarItem のプライマリメニューのスクリーンショット Android と iOS"](toolbaritem-images/toolbaritem-primary-menu.png "ToolbarItem Android と iOS のプライマリメニューのスクリーンショット")

`Order` プロパティが `Secondary`に設定されている場合、動作はプラットフォームによって異なります。 UWP と Android では、[`Secondary` 項目] メニューが3つのドットとして表示され、タップまたはクリックして縦の一覧の項目を表示できます。 IOS では、[`Secondary` 項目] メニューは、ナビゲーションバーの下に水平方向の一覧として表示されます。 次のスクリーンショットは、iOS と Android のセカンダリメニューを示しています。

!["ToolbarItem のセカンダリメニューのスクリーンショット Android と iOS"](toolbaritem-images/toolbaritem-secondary-menu.png "ToolbarItem Android と iOS でのセカンダリメニューのスクリーンショット")

> [!WARNING]
> `Order` プロパティが `Secondary` に設定されている `ToolbarItem` オブジェクトのアイコン動作は、プラットフォーム間で一貫性がありません。 セカンダリメニューに表示されるアイテムには、`IconImageSource` プロパティを設定しないようにしてください。

## <a name="related-links"></a>関連リンク

* [ToolbarItem のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Xamarin 形式の画像](~/xamarin-forms/user-interface/images.md)
* [Xamarin. フォーム MenuItem](~/xamarin-forms/user-interface/menuitem.md)
