---
title: "ポップアップ メニュー"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: f976d798ae1b1279fc8f82d3cf1d738bb2c93911
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="popup-menu"></a>ポップアップ メニュー

`PopupMenu`クラスは、特定のビューに関連付けられているポップアップ メニューを表示するためのサポートを追加します。 次の図は、2 番目の項目が選択されているのと同じように強調表示を伴うこのボタンをクリックするには、ポップアップ メニューを示しています。

 [![3 つの項目を 3 つ PopopMenu の例](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png#lightbox)

Android 4 は、いくつかの新機能を追加`PopupMenu`ビットを容易に操作、つまりを作成します。

-   **展開**&ndash;拡張メソッドは、ポップアップ メニュー クラスで直接利用できるようになりました。
-   **DismissEvent** &ndash;ポップアップ メニュー クラスが、DismissEvent になりました。

これらの機能強化を見てみましょう。 この例では、ボタンを含む 1 つのアクティビティがあります。 ユーザーには、ボタンがクリックすると、次に示すようにポップアップ メニューが表示されます。

 [![ボタンとポップアップ メニューの項目 3 に、エミュレーターで実行されているアプリの例](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png#lightbox)


## <a name="creating-a-popup-menu"></a>ポップアップ メニューを作成します。

インスタンスを作成するとき、 `PopupMenu`、そのコンス トラクターへの参照を渡す必要があります、 `Context`、だけでなく、ビュー、メニューが接続されています。 この場合、作成、`PopupMenu`で、ボタンの click イベント ハンドラー、これは名前`showPopupMenu`です。
このボタンは、ビューを添付しますでも、`PopupMenu`次のコードに示すように、します。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

Android 3 は、XML リソースのメニューを展開するためのコードに必要な最初への参照を取得する、 `MenuInflator`、まずその`Inflate`に展開するには、メニューとメニュー インスタンスに含まれる XML のリソース ID を持つメソッドです。 このようなアプローチでは、Android 4 では次のコードとして後で引き続き動作します。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

Android 4 以降、今すぐ呼び出せます`Inflate`のインスタンスに直接、`PopupMenu`です。 これにより、コードが次のようより簡潔に。

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

上記のコードで、メニューを膨張させる後を呼び出して`menu.Show`を画面に表示します。


## <a name="handling-menu-events"></a>メニュー イベントを処理します。

ユーザーがメニュー項目を選択したときに、`MenuItemClick`イベントを発生させるし、メニューは非表示にします。 メニューの外側の任意の場所をタップするだけでも消去されます。 Android 4 では、メニューが破棄されたときに、どちらの場合、`DismissEvent`が生成されます。 次のコードは、両方のイベント ハンドラーを追加、`MenuItemClick`と`DismissEvent`イベント。

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
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
