---
title: ポップアップ メニュー
description: 固定されるポップアップ メニューを特定のビューに追加する方法。
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: 1e74c8b7745936f6e9a8890fd26acafe2f2fb6d5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61288652"
---
# <a name="popup-menu"></a>ポップアップ メニュー

[ポップアップ メニュー](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (とも呼ばれる、_ショートカット メニューの _) が特定の表示が固定されるメニューです。 次の例では、1 つのアクティビティには、ボタンが含まれています。 ユーザーがボタンをタップする 3 つのアイテムをポップアップ メニューが表示されます。

[![ボタンとポップアップ メニューの 3 つの項目を使用してアプリの例](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>ポップアップ メニューを作成します。

最初の手順では、メニューのメニュー リソース ファイルを作成しで配置を**リソース/メニュー**します。 たとえば、前のスクリーン ショットに表示される 3 つの項目 メニューのコードは、次の XML **Resources/menu/popup_menu.xml**:

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

インスタンスを次に、作成`PopupMenu`し、そのビューを固定します。 インスタンスを作成するときに`PopupMenu`への参照をそのコンス トラクターに渡す、`Context`ビュー、メニューをアタッチするとします。 その結果、その構築時に、このビューには、ポップアップ メニューが固定されます。

次の例では、`PopupMenu`ボタンの click イベント ハンドラーが作成されます (名前は`showPopupMenu`)。 このボタンは、ビューでも、`PopupMenu`アンカーは、次のコード例に示すようにします。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

最後に、ポップアップ メニューがある必要があります*膨張*で以前に作成されたメニュー リソース。 次の例では、メニューの呼び出しで[展開](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/)メソッドを追加し、その[表示](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/)メソッドを呼び出してそれを表示します。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>メニュー イベントの処理

ユーザーがメニュー項目を選択すると、 [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/)クリックしてイベントが発生し、メニューは非表示にします。 メニューの外側の任意の場所をタップすると、単に通知を閉じた。 どちらの場合、メニューが閉じられたときにその[DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/)が発生します。 次のコードは、両方のイベント ハンドラーを追加、`MenuItemClick`と`DismissEvent`イベント。

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
