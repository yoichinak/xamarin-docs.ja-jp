---
title: 2 番目のツール バーの追加
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: e6104a58c35b4daf8e204b3937022103d774615c
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457798"
---
# <a name="adding-a-second-toolbar"></a>2 番目のツール バーの追加

## <a name="overview"></a>概要 

は、1つの `Toolbar` アクティビティ内で複数回使用できる操作バーを置き換える以外にも、画面上の任意の場所に配置するようにカスタマイズすることができ、 &ndash; 画面の部分的な幅にのみまたがるように構成できます。 次の例では、2番目のを作成 `Toolbar` し、画面の下部に配置する方法を示しています。 これ `Toolbar` は、 **コピー**、 **切り取り**、および **貼り付け** の各メニュー項目を実装します。 

## <a name="define-the-second-toolbar"></a>2番目のツールバーを定義する 

レイアウトファイルの **Main** を編集し、その内容を次の xml に置き換えます。

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

この XML は、画面の下部に2番目のを追加し、画面の中央に空のを `Toolbar` `ImageView` 入力します。 このの高さ `Toolbar` は、操作バーの高さに設定されています。 

```xml
android:minHeight="?android:attr/actionBarSize"
```

このの背景色は、次のように定義される `Toolbar` アクセントの色に設定されます。

```xml
android:background="?android:attr/colorAccent
```

これは、「」で作成したに `Toolbar` よって使用されるのとは別のテーマ (**ThemeOverlay**) に基づいています。これは、 `Toolbar` アクティビティのウィンドウ décor にバインドされていない [操作バー、](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash; または最初に使用されているテーマに置き換えられ `Toolbar` ます。

**リソース/値/styles.xml**を編集し、スタイル定義に次のアクセント色を追加します。 

```xml
<item name="android:colorAccent">#C7A935</item>
```

これにより、下部のツールバーに濃いオレンジ色が設定されます。 アプリをビルドして実行すると、画面の下部に空の2つ目のツールバーが表示されます。 

[![画面の下部に黄色い2番目のツールバーがあるアプリのスクリーンショット](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)

## <a name="add-edit-menu-items"></a>編集メニュー項目の追加 

ここでは、[編集] メニュー項目を下部に追加する方法について説明 `Toolbar` します。 

メニュー項目をセカンダリに追加するには `Toolbar` : 

1. アプリプロジェクトのフォルダーにメニューアイコンを追加 `mipmap-` します (必要な場合)。

2. メニュー項目の内容を定義するには、追加のメニューリソースファイルを [ **リソース] メニュー**に追加します。 

3. アクティビティのメソッドで、 `OnCreate` `Toolbar` (を呼び出して) を検索し、のメニューを拡大し `FindViewById` `Toolbar` ます。

4. 新しいメニュー項目に対して、のクリックハンドラーを実装し `OnCreate` ます。 

以下のセクションでは、このプロセスの詳細について説明します。 **切り取り**、 **コピー**、 **貼り付け** の各メニュー項目が一番下に追加され `Toolbar` ます。 

### <a name="define-the-edit-menu-resource"></a>[編集] メニューリソースの定義

**Resources/menu**サブディレクトリに**edit_menus.xml**という名前の新しい xml ファイルを作成し、内容を次の xml に置き換えます。

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

この XML は、**切り取り**、**コピー**、**貼り付け**の各メニュー項目を作成します (操作バーを置き換えるためにフォルダーに追加されたアイコンを使用し `mipmap-` ます)。 [Replacing the Action Bar](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)

### <a name="inflate-the-menus"></a>メニューを拡大する

`OnCreate` **MainActivity.cs**のメソッドの最後に、次のコード行を追加します。 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

このコードは、 `edit_toolbar` 増えで定義**Main.axml**されているビューを検索し、そのタイトルを**編集**に設定して、そのメニュー項目 ( **edit_menus.xml**で定義) をします。 これは、タップされた編集アイコンを示すトーストを表示するメニュークリックハンドラーを定義します。 

アプリケーションをビルドし、実行します。 アプリを実行すると、上に追加されたテキストとアイコンが次のように表示されます。 

[![[切り取り]、[コピー]、[貼り付け] アイコンがある、下部のツールバーの図](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

[ **切り取り** ] メニューアイコンをタップすると、次のトーストが表示されます。 

[![[切り取り] メニューアイコンがタップされたことを示すトーストのスクリーンショット](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

いずれかのツールバーの [メニュー項目] をタップすると、結果の toasts が表示されます。 

[![[保存]、[コピー]、[貼り付け] の各メニュー項目のスクリーンショット](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)

## <a name="the-up-button"></a>[上へ] ボタン 

ほとんどの Android アプリは、アプリナビゲーションの [ **戻る** ] ボタンに依存しています。[ **戻る** ] ボタンを押すと、ユーザーが前の画面に移動します。
ただし、[ **up** ] ボタンを使用して、ユーザーがアプリのメイン画面に簡単に移動できるようにすることもできます。 ユーザーが [ **上** へ] ボタンを選択すると、ユーザーはアプリ階層の上位レベルに移動し &ndash; ます。つまり、アプリは、前に表示されたアクティビティに戻るのではなく、ユーザーをバックスタック内の複数のアクティビティにポップします。 

アクションバーとしてを使用する2番目のアクティビティで [ **上** へ] ボタンを有効にするには `Toolbar` 、 `SetDisplayHomeAsUpEnabled` `SetHomeButtonEnabled` 2 番目のアクティビティのメソッドでメソッドとメソッドを呼び出し `OnCreate` ます。

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[ [Support V7] ツールバー](/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar) のコードサンプルでは、[ **Up** ] ボタンが動作しています。 このサンプルでは、次に説明する AppCompat ライブラリを使用して、前のアクティビティへの階層ナビゲーションのツールバーの [ **上** ] ボタンを使用する2番目のアクティビティを実装します。 この例では、[ `DetailActivity` ホーム] ボタンを使用して、次のメソッドを呼び出すことにより、[ **上へ** ] ボタンを有効にし `SupportActionBar` ます。 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

ユーザーがからに移動すると `MainActivity` `DetailActivity` 、 `DetailActivity` スクリーンショットに示されているように **上向き** のボタン (左向きの矢印) が表示されます。

[![ツールバーの上のボタンの左矢印のスクリーンショットの例](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

**このボタンをタップ**すると、アプリはに戻り `MainActivity` ます。 複数レベルの階層を持つより複雑なアプリでは、このボタンをタップすると、ユーザーが前の画面ではなく、アプリの次の最上位レベルに戻ります。 

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)