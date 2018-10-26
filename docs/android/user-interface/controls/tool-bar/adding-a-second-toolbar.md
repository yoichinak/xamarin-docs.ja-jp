---
title: 2 番目のツールバーを追加します。
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: b8da13afb7fd8d7198e8bfe7476b40a5cd09769a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112553"
---
# <a name="adding-a-second-toolbar"></a>2 番目のツールバーを追加します。


## <a name="overview"></a>概要 

`Toolbar`アクション バーには置換を超えることができます&ndash;アクティビティ内で複数回を使用することできます、ことができますが、画面の任意の場所に配置用にカスタマイズし、画面の部分の幅のみをまたがるするように構成できます。 次の例は、1 秒あたりに作成する方法を示しています。`Toolbar`し、画面の下部に配置します。 これは、`Toolbar`実装**コピー**、**切り取り**、および**貼り付け**メニュー項目。 


## <a name="define-the-second-toolbar"></a>2 番目のツールバーを定義します。 

レイアウト ファイルを編集**Main.axml**でその内容を次の XML に置き換えます。

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

この XML は、2 つ目を追加します。`Toolbar`空の画面の一番下`ImageView`、画面の中央の情報を入力します。 この高さ`Toolbar`アクション バーの高さに設定されます。 

```xml
android:minHeight="?android:attr/actionBarSize"
```

これの背景色`Toolbar`次に定義されているアクセント カラーに設定されます。

```xml
android:background="?android:attr/colorAccent
```

通知この`Toolbar`、さまざまなテーマに基づきます (**ThemeOverlay.Material.Dark.ActionBar**) で使用した場合、`Toolbar`で作成した[操作バーの置き換え](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;アクティビティのウィンドウも親しみやすくしたり、最初に使用されるテーマにバインドされていない`Toolbar`します。

編集**Resources/values/styles.xml**次アクセント カラーをスタイル定義に追加します。 

```xml
<item name="android:colorAccent">#C7A935</item>
```

これにより、下部のツールバーでは濃いオレンジ色にします。 ビルドと、アプリを実行している画面の下部にある空白 2 番目ツールバーが表示されます。 

[![画面の下部にある黄色の 2 番目のツールバーを使用したアプリのスクリーン ショット](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>編集メニュー項目を追加します。 

このセクションは、下部にあるを編集 メニュー項目を追加する方法を説明します`Toolbar`します。 

セカンダリにメニュー項目を追加する`Toolbar`: 

1.  メニュー アイコンを追加、 `mipmap-` (必要な) 場合は、アプリ プロジェクトのフォルダー。

2.  メニュー項目の内容を定義する追加のメニュー リソース ファイルを追加することで**リソース/メニュー**します。 

3.  アクティビティの`OnCreate`メソッド、検索、 `Toolbar` (呼び出して`FindViewById`) 拡張と、`Toolbar`のメニュー。

4.  クリック ハンドラーを実装する`OnCreate`新しいメニュー項目。 

次のセクションでは、このプロセスの詳細を示します:**切り取り**、**コピー**、および**貼り付け**メニュー項目を下に追加されます`Toolbar`します。 



### <a name="define-the-edit-menu-resource"></a>編集メニュー リソースを定義します。

**リソース/メニュー**サブディレクトリという新しい XML ファイルを作成**edit_menus.xml**内容を次の XML に置き換えます。

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

この XML を作成、**切り取り**、**コピー**、および**貼り付け**メニュー項目 (に追加されたアイコンを使用して、`mipmap-`フォルダー[操作バーの置き換え](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



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

このコードでは検索、`edit_toolbar`ビューで定義されている**Main.axml**、そのタイトルを設定**編集**、そのメニュー項目を拡張し、(で定義されている**edit_menus.xml**)。 メニューを定義を編集するアイコンがタップされたかを示すトーストを表示するハンドラーをクリックします。 

アプリケーションをビルドし、実行します。 アプリの実行時に、次のように、テキストとアイコンの上に追加は表示されます。 

[![下部ツールバーには、切り取り、コピー、および貼り付けのアイコンの図](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

タップすると、**切り取り**メニュー アイコンが表示される次のトースト。 

[![切り取りメニュー アイコンがタップされたことを示すトーストのスクリーン ショット](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

いずれかのツールバーのメニュー項目をタップして、結果として得られるのトースト通知が表示されます。 

[![スクリーン ショットのトーストの保存、コピー、および貼り付けメニュー項目のタップ](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>最新のボタン 

ほとんどの Android アプリの依存、**戻る**アプリのナビゲーション ボタン; キーを押して、**戻る**ボタンは、前の画面が表示されます。
ただし、提供することがありますも、**を**を簡単にユーザーがアプリのメイン画面に「上」に移動するボタンをクリックします。 ユーザーが選択すると、**を**ボタンをユーザーがアプリの階層内の上位レベルに移動&ndash;は、アプリが表示されます、ユーザーはバックアップを以前にアクセスしたポップをそれぞれのバックアップではなく、戻るスタック内の複数アクティビティアクティビティ。 

有効にする、**を** ボタンを使用する 2 番目のアクティビティを`Toolbar`のアクション バーとして呼び出す、`SetDisplayHomeAsUpEnabled`と`SetHomeButtonEnabled`メソッドは、2 つ目のアクティビティの`OnCreate`メソッド。

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[V7 ツールバーをサポートして](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)コード例は、**を** ボタンの動作。 (を次に説明した AppCompat ライブラリを使用して) このサンプルは、ツールバーを使用する 2 番目のアクティビティを実装して**を**の階層型ナビゲーション、前の活動に戻るボタンをクリックします。 この例で、`DetailActivity`ホーム ボタンにより、**を**ボタンは、次のことで`SupportActionBar`メソッドの呼び出し。 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

ユーザーの操作から`MainActivity`に`DetailActivity`、`DetailActivity`が表示されます、**を**ボタン (左矢印) のスクリーン ショットに示すように。

[![ツールバーの上ボタン左矢印のスクリーン ショットの例](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

これをタップして**を**ボタンにより、アプリに戻る`MainActivity`します。 階層の複数のレベルでさらに複雑なアプリケーションでは、このボタンをタップする戻ったら、ユーザー、前の画面ではなく、アプリで次に高いレベルに。 



## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
