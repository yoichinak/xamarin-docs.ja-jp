---
title: ポップアップメニュー
description: 特定のビューに固定されているポップアップメニューを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: e7665ee1d3506fb4b6a237a7c6906d9bfb3e9cb1
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510208"
---
# <a name="xamarinandroid-popup-menu"></a>Xamarin. Android ポップアップメニュー

[PopupMenu](xref:Android.Widget.PopupMenu) (_ショートカットメニュー_とも呼ばれます) は、特定のビューに固定されたメニューです。 次の例では、1つのアクティビティにボタンが含まれています。 ユーザーがボタンをタップすると、3項目のポップアップメニューが表示されます。

[![ボタンと3項目のポップアップメニューを使用したアプリの例](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>ポップアップメニューの作成

最初の手順では、メニューのメニューリソースファイルを作成し、 **[リソース/メニュー]** に配置します。 たとえば、次の XML は、前のスクリーンショットの**Resources/menu/popup_menu**に表示される3項目メニューのコードです。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

次に、の`PopupMenu`インスタンスを作成し、それをビューに固定します。 の`PopupMenu`インスタンスを作成するときは、そのコンストラクターに、メニューがアタッチ`Context`されるビューに加えて、への参照を渡します。 その結果、ポップアップメニューは、その構築時にこのビューに固定されます。

次の例`PopupMenu`では、ボタン (という名前`showPopupMenu`) の click イベントハンドラーにが作成されています。 このボタンは、次のコード例に`PopupMenu`示すように、が固定されているビューでもあります。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

最後に、ポップアップメニューは、前に作成したメニュー*リソースで拡大*する必要があります。 次の例では、メニューの[膨張](xref:Android.Views.LayoutInflater.Inflate*)メソッドの呼び出しが追加され、 [Show](xref:Android.Widget.PopupMenu.Show)メソッドが呼び出されて表示されます。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>メニューイベントの処理

ユーザーがメニュー項目を選択すると、 [MenuItemClick](xref:Android.Widget.PopupMenu.MenuItemClick) click イベントが発生し、メニューは破棄されます。 メニューの外側の任意の場所をタップすると、単にそれを無視します。 どちらの場合も、メニューが破棄されると、その[DismissEvent](xref:Android.Widget.PopupMenu.Dismiss)が発生します。 次のコードは、イベント`MenuItemClick`と`DismissEvent`イベントの両方のイベントハンドラーを追加します。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
    menu.Show ();
};
```



## <a name="related-links"></a>関連リンク

- [PopupMenuDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
