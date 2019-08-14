---
title: ToolbarItem
description: ToolbarItem クラスは、アプリケーションのナビゲーションバーで使用される特殊なボタンです。
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: dfc50defb6eafe705cc9c59b1b9793f1ce48c527
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984304"
---
# <a name="xamarinforms-toolbaritem"></a>ToolbarItem

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Xamarin. Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)クラスは、 `Page`オブジェクトの`ToolbarItems`コレクションに追加できる特殊なボタンです。 各`ToolbarItem`オブジェクトは、アプリケーションのナビゲーションバーにボタンとして表示されます。 インスタンス`ToolbarItem`はアイコンを持つことができ、プライマリまたはセカンダリのメニュー項目として表示されます。 クラス`ToolbarItem`は、から[`MenuItem`](xref:Xamarin.Forms.MenuItem)継承されます。

次のスクリーンショット`ToolbarItem`は、iOS と Android のナビゲーションバーにあるオブジェクトを示しています。

!["Android と iOS の ToolbarItem demo スクリーンショット"](toolbaritem-images/toolbaritem-device-screenshot.png "Android と iOS の ToolbarItem demo スクリーンショット")

コントロール`ToolbarItem`は、次のプロパティを定義します。

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)インスタンスをプライマリメニューとセカンダリメニューの`ToolbarItemOrder`どちらに表示するかを決定する列挙`ToolbarItem`値です。
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)オブジェクトのコレクション内のアイテムの表示順序を決定する値です。`integer` `ToolbarItems` `Page`

クラス`ToolbarItem`は、 `MenuItem`クラスから次の一般的に使用されるプロパティを継承します。

* [`Text`](xref:Xamarin.Forms.MenuItem.Text)は、 `string` `ToolbarItem`オブジェクトの表示テキストを決定するです。
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)オブジェクトの表示アイコンを決定する値です。`ImageSource` `ToolbarItem`
* [`Command`](xref:Xamarin.Forms.MenuItem.Command)は、 `ICommand`ユーザー操作 (指タップやクリックなど) を、ビューモデルで定義されているコマンドにバインドできるようにします。
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)に渡す必要があるパラメーターを指定`object`するです`SearchCommand`。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) `ToolbarItem`インスタンスがデータバインディングのターゲットになることができるように、オブジェクトによってバックアップされます。

## <a name="create-a-toolbaritem"></a>ToolbarItem を作成する

オブジェクト`ToolbarItem`は、XAML でインスタンス化できます。 プロパティ`Text` と`IconImageSource`プロパティを設定して、ナビゲーションバーでのボタンの表示方法を決定できます。 次の例は、いくつかの`ToolbarItem`一般的なプロパティを設定してをインスタンス化する方法を示しています。

```xaml
<ToolbarItem Text="Example Item"
             IconImageSource="example_icon.png"
             Order="Primary"
             Priority="0" />
```

この例で`ToolbarItem`は、テキストとアイコンを持つオブジェクトが、プライマリナビゲーションバー領域に最初に表示されます。 は`ToolbarItem` 、コードで作成し、 `ToolbarItems`コレクションに追加することもできます。

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

プロパティとして指定`string`されたによって表されるファイルは、各プラットフォームプロジェクトに存在している必要があります。 `IconImageSource`

> [!NOTE]
> イメージ資産は、プラットフォームごとに異なる方法で処理されます。 は、ローカルファイル、埋め込みリソース、URI、またはストリームを含むソースから取得できます。`ImageSource` Xamarin. フォームで`IconImageSource`プロパティとイメージを設定する方法の詳細については、「 [xamarin. forms の画像](~/xamarin-forms/user-interface/images.md)」を参照してください。

## <a name="define-button-behavior"></a>ボタンの動作を定義する

クラス`ToolbarItem`は、 `MenuItem`クラス`Clicked`からイベントを継承します。 イベントハンドラーをイベントにアタッチして`Clicked` 、XAML でのインスタンスの`ToolbarItem`タップまたはクリックに対応することができます。

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

イベントハンドラーは、コード内でアタッチすることもできます。

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

前の例で`OnItemClicked`は、イベントハンドラーが参照されていました。 次のコードは、の実装例を示しています。

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem`オブジェクトでは、プロパティ`Command`と`CommandParameter`プロパティを使用して、イベントハンドラーを使用せずにユーザー入力に応答することもできます。 インターフェイスと MVVM データバインディング`ICommand`の詳細については、「 [Xamarin. フォーム MenuItem MVVM Behavior](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm)」を参照してください。

## <a name="primary-and-secondary-menus"></a>プライマリメニューとセカンダリメニュー

列挙型に`Default`は`Primary`、、 `Secondary` 、およびの各値があります。 `ToolbarItemOrder`

プロパティが`Primary`に`ToolbarItem`設定されている場合、オブジェクトは、すべてのプラットフォームのメインナビゲーションバーに表示されます。 `Order` `ToolbarItem`オブジェクトは、ページタイトルに対して優先順位が付けられます。これは、項目の領域を確保するために切り捨てられます。 次のスクリーンショット`ToolbarItem`は、iOS および Android のプライマリメニューのオブジェクトを示しています。

!["ToolbarItem のプライマリメニューのスクリーンショット Android と iOS"](toolbaritem-images/toolbaritem-primary-menu.png "ToolbarItem Android と iOS のプライマリメニューのスクリーンショット")

プロパティが`Order`に`Secondary`設定されている場合、動作はプラットフォームによって異なります。 UWP と Android では、 `Secondary` [項目] メニューが3つのドットとして表示され、タップまたはクリックして縦の一覧の項目を表示できます。 IOS では、 `Secondary` [項目] メニューは、ナビゲーションバーの下に水平方向の一覧として表示されます。 次のスクリーンショットは、iOS と Android のセカンダリメニューを示しています。

!["ToolbarItem のセカンダリメニューのスクリーンショット Android と iOS"](toolbaritem-images/toolbaritem-secondary-menu.png "ToolbarItem Android と iOS でのセカンダリメニューのスクリーンショット")

> [!WARNING]
> プロパティ`Order`がに`ToolbarItem` 設定されているオブジェクトでのアイコンの動作は、プラットフォーム間で一貫性が`Secondary`ありません。 セカンダリメニューに`IconImageSource`表示される項目のプロパティを設定しないようにしてください。

## <a name="related-links"></a>関連リンク

* [ToolbarItem のデモ](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Xamarin 形式の画像](~/xamarin-forms/user-interface/images.md)
