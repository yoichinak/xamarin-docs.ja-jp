---
title: ポップアップメニュー
description: 特定のビューに固定されているポップアップメニューを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/31/2018
ms.openlocfilehash: 5d445f84b7634895c59120e905daaf6fee403ac9
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453606"
---
# <a name="xamarinandroid-popup-menu"></a>Xamarin. Android ポップアップメニュー

[PopupMenu](xref:Android.Widget.PopupMenu) (_ショートカットメニュー_とも呼ばれます) は、特定のビューに固定されたメニューです。 次の例では、1つのアクティビティにボタンが含まれています。 ユーザーがボタンをタップすると、3項目のポップアップメニューが表示されます。

[![ボタンと3項目のポップアップメニューを使用したアプリの例](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)

## <a name="creating-a-popup-menu"></a>ポップアップメニューの作成

最初の手順では、メニューのメニューリソースファイルを作成し、[ **リソース/メニュー**] に配置します。 たとえば、次の XML は、前のスクリーンショット、 **Resources/menu/popup_menu.xml**に表示される3項目メニューのコードです。

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

次に、のインスタンスを作成 `PopupMenu` し、それをビューに固定します。 のインスタンスを作成するときは `PopupMenu` 、そのコンストラクターに、メニューが `Context` アタッチされるビューに加えて、への参照を渡します。 その結果、ポップアップメニューは、その構築時にこのビューに固定されます。

次の例では、 `PopupMenu` ボタン (という名前) の click イベントハンドラーにが作成されてい `showPopupMenu` ます。 このボタンは、 `PopupMenu` 次のコード例に示すように、が固定されているビューでもあります。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

最後に、ポップアップメニューは、前に作成したメニュー *リソースで拡大する必要* があります。 次の例では、メニューの [膨張](xref:Android.Views.LayoutInflater.Inflate*) メソッドの呼び出しが追加され、 [Show](xref:Android.Widget.PopupMenu.Show) メソッドが呼び出されて表示されます。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

## <a name="handling-menu-events"></a>メニューイベントの処理

ユーザーがメニュー項目を選択すると、 [MenuItemClick](xref:Android.Widget.PopupMenu.MenuItemClick) click イベントが発生し、メニューは破棄されます。 メニューの外側の任意の場所をタップすると、単にそれを無視します。 どちらの場合も、メニューが破棄されると、その [DismissEvent](xref:Android.Widget.PopupMenu.Dismiss) が発生します。 次のコードは、イベントとイベントの両方のイベントハンドラーを追加し `MenuItemClick` `DismissEvent` ます。

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

- [PopupMenuDemo (サンプル)](/samples/xamarin/monodroid-samples/popupmenudemo)