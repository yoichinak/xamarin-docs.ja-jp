---
title: 2 番目のツールバーを追加します。
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f2255ab5ac7e153266093877a1c52bfc08927a09
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766638"
---
# <a name="adding-a-second-toolbar"></a>2 番目のツールバーを追加します。


## <a name="overview"></a>概要 

`Toolbar`行える置換を超える操作バー&ndash;アクティビティ内で複数回を使用することできます、だということで、画面上のどこにでも配置用にカスタマイズし、画面の部分の幅のみをまたがるするように構成できます。 次の例は、1 秒あたりに作成する方法を示しています。`Toolbar`し、画面の下部に配置します。 これは、`Toolbar`実装**コピー**、**切り取り**、および**貼り付け**メニュー項目。 


## <a name="define-the-second-toolbar"></a>2 番目のツールバーを定義します。 

レイアウト ファイルを編集して**Main.axml**とその内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

この XML は、2 番目を追加`Toolbar`空の画面の下部に`ImageView`画面の中央を入力します。 この高さ`Toolbar`操作バーの高さに設定されます。 

```xml
android:minHeight="?android:attr/actionBarSize"
```

これの背景色`Toolbar`が次に定義するアクセント カラーに設定します。

```xml
android:background="?android:attr/colorAccent
```

注意してくださいこの`Toolbar`、さまざまなテーマに基づきます (**ThemeOverlay.Material.Dark.ActionBar**) で使用するより、`Toolbar`で作成した[操作バーを置き換える](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;アクティビティのウィンドウの装飾または、最初に使用されるテーマにバインドされていない`Toolbar`です。

編集**Resources/values/styles.xml**し、スタイル定義を次アクセント カラーを追加します。 

```xml
<item name="android:colorAccent">#C7A935</item>
```

これにより、下部のツールバーは濃いオレンジ色にします。 ビルドと、アプリを実行して、画面の下部にある空白 2 番目ツールバーが表示されます。 

[![画面の下部にある黄色の 2 番目のツールバーでアプリのスクリーン ショット](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>編集メニュー項目を追加します。 

メニュー項目の編集を下部に追加する方法を説明`Toolbar`です。 

セカンダリにメニュー項目を追加する`Toolbar`: 

1.  メニューのアイコンを追加、 `mipmap-` (必要な場合) は、アプリ プロジェクトのフォルダーです。

2.  メニュー項目の内容を定義する追加のメニュー リソース ファイルを追加**リソース/メニュー**です。 

3.  アクティビティの`OnCreate`メソッド、検索、 `Toolbar` (を呼び出して`FindViewById`) 拡張と、`Toolbar`のメニュー。

4.  クリック ハンドラーを実装する`OnCreate`新しいメニュー項目。 

次のセクションでは、このプロセスの詳細を示す:**切り取り**、**コピー**、および**貼り付け**メニュー項目を下に追加されます`Toolbar`です。 



### <a name="define-the-edit-menu-resource"></a>編集メニュー リソースを定義します。

**リソース/メニュー**サブディレクトリと呼ばれる新しい XML ファイルを作成**edit_menus.xml**し、内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

この XML を作成、**切り取り**、**コピー**、および**貼り付け**メニュー項目 (に追加されたアイコンを使用して、`mipmap-`フォルダーに[操作バーを置き換える](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



### <a name="inflate-the-menus"></a>メニューを展開します。

最後に、`OnCreate`メソッド**MainActivity.cs**、次のコード行を追加します。 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

このコードを検索、`edit_toolbar`ビューで定義されている**Main.axml**のタイトルを設定**編集**、そのメニュー項目を拡張し、(で定義されている**edit_menus.xml**)。 メニューを定義するハンドラー編集アイコンがタップされたを示すためにトーストを表示する をクリックします。 

アプリケーションをビルドし、実行します。 アプリの実行時に、次のように、テキストとアイコンの上に追加は表示されます。 

[![下部のツールバーには、切り取り、コピー、および貼り付けのアイコンの図](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

タップすると、**切り取り**メニュー アイコンにより、次のトーストを表示します。 

[![メニューの切り取り アイコンがタップされたことを示すトーストのスクリーン ショット](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

いずれかのツールバーのメニュー項目をタップすると、結果として得られるトーストが表示されます。 

[![スクリーン ショットのトーストのコピーを保存し、メニュー項目のタップを貼り付けます](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>印ボタン 

ほとんどの Android アプリの依存、**戻る**アプリ ナビゲーション ボタン以外のキーを押して、**戻る**ボタンには、前の画面に戻ります。
ただし、することも提供する、**を**ユーザーがアプリのメイン画面に「最大」に移動しやすくボタンをクリックします。 ユーザーが選択すると、**を** ボタン、ユーザーがアプリの階層内の上位レベルに移動&ndash;は、アプリをポップ戻るユーザーに、以前に開いたポップ バックではなく、バック スタックに複数のアクティビティアクティビティ。 

有効にする、**を** ボタンを使用する 2 番目のアクティビティで、`Toolbar`その操作バーとして呼び出す、`SetDisplayHomeAsUpEnabled`と`SetHomeButtonEnabled`2 番目のアクティビティ内のメソッド`OnCreate`メソッド。

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[V7 ツールバーをサポートして](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)コード サンプルでは、**を** ボタンの動作です。 このサンプル (を次に示す AppCompat ライブラリを使用) が、ツールバーを使用して 2 番目のアクティビティを実装**を**階層間を移動、前の活動に戻るボタンをクリックします。 この例では、`DetailActivity`により、[ホーム] ボタン、**を**させて、次のボタン`SupportActionBar`メソッドの呼び出し。 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

ユーザーの操作から`MainActivity`に`DetailActivity`、`DetailActivity`が表示されます、**を**ボタン (左矢印) のスクリーン ショットに示すように。

[![ツールバーに上向きボタン左矢印のスクリーン ショットの例](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

これをタップ**を**ボタンで、アプリに戻るには`MainActivity`します。 アプリではより複雑な階層の複数のレベルで、このボタンをタップする戻ったら、ユーザー、前の画面ではなく、アプリで次に高いレベルにします。 



## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
