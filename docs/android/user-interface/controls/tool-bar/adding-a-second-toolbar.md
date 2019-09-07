---
title: 2 番目のツール バーの追加
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 9fb5c696e830710e6ad99140477eedcbfe0e8823
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764689"
---
# <a name="adding-a-second-toolbar"></a>2 番目のツール バーの追加

## <a name="overview"></a>概要 

は`Toolbar` 、1つのアクティビティ内で複数&ndash;回使用できる操作バーを置き換える以外にも、画面上の任意の場所に配置するようにカスタマイズすることができ、画面の部分的な幅にのみまたがるように構成できます。 次の例では、2番目`Toolbar`のを作成し、画面の下部に配置する方法を示しています。 これ`Toolbar`は、**コピー**、**切り取り**、および**貼り付け**の各メニュー項目を実装します。 

## <a name="define-the-second-toolbar"></a>2番目のツールバーを定義する 

レイアウトファイルの**Main**を編集し、その内容を次の xml に置き換えます。

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

この XML は、画面`Toolbar`の下部に2番目のを追加`ImageView`し、画面の中央に空のを入力します。 この`Toolbar`の高さは、操作バーの高さに設定されています。 

```xml
android:minHeight="?android:attr/actionBarSize"
```

この`Toolbar`の背景色は、次のように定義されるアクセントの色に設定されます。

```xml
android:background="?android:attr/colorAccent
```

`Toolbar`これは、「」で作成したによって使用されるの`Toolbar`とは別のテーマ (ThemeOverlay) に基づいています。これは、アクティビティのウィンドウ décor またはにバインドされていない[操作バー](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;を置き換えます。最初`Toolbar`ので使用されるテーマ。

**Resources/values/styles .xml**を編集し、スタイル定義に次のアクセント色を追加します。 

```xml
<item name="android:colorAccent">#C7A935</item>
```

これにより、下部のツールバーに濃いオレンジ色が設定されます。 アプリをビルドして実行すると、画面の下部に空の2つ目のツールバーが表示されます。 

[![画面の下部に黄色い2番目のツールバーがあるアプリのスクリーンショット](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)

## <a name="add-edit-menu-items"></a>編集メニュー項目の追加 

ここでは、[編集] メニュー項目を下部`Toolbar`に追加する方法について説明します。 

メニュー項目をセカンダリ`Toolbar`に追加するには: 

1. アプリプロジェクトの`mipmap-`フォルダーにメニューアイコンを追加します (必要な場合)。

2. メニュー項目の内容を定義するには、追加のメニューリソースファイルを [**リソース] メニュー**に追加します。 

3. アクティビティの`OnCreate`メソッドで、 `Toolbar` (を`Toolbar`呼び出し`FindViewById`て) を検索し、のメニューを拡大します。

4. 新しいメニュー項目に対し`OnCreate`て、のクリックハンドラーを実装します。 

以下のセクションでは、このプロセスの詳細について説明します。**切り取り**、**コピー**、**貼り付け**の各メニュー項目が下部`Toolbar`に追加されます。 

### <a name="define-the-edit-menu-resource"></a>[編集] メニューリソースの定義

**Resources/menu**サブディレクトリで、 **edit_menus**という名前の新しい xml ファイルを作成し、内容を次の xml に置き換えます。

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

この XML は、**切り取り**、**コピー**、**貼り付け**の各メニュー項目を作成します (操作バー `mipmap-`を[置き換える](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)ためにフォルダーに追加されたアイコンを使用します)。

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

`edit_toolbar`このコードは、増えで定義されているビューを検索**し、その**タイトルを**編集用**に設定して、そのメニュー項目 ( **edit_menus**で定義) をします。 これは、タップされた編集アイコンを示すトーストを表示するメニュークリックハンドラーを定義します。 

アプリケーションをビルドし、実行します。 アプリを実行すると、上に追加されたテキストとアイコンが次のように表示されます。 

[![[切り取り]、[コピー]、[貼り付け] アイコンがある、下部のツールバーの図](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

**[切り取り]** メニューアイコンをタップすると、次のトーストが表示されます。 

[![[切り取り] メニューアイコンがタップされたことを示すトーストのスクリーンショット](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

いずれかのツールバーの [メニュー項目] をタップすると、結果の toasts が表示されます。 

[![[保存]、[コピー]、[貼り付け] の各メニュー項目のスクリーンショット](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)

## <a name="the-up-button"></a>[上へ] ボタン 

ほとんどの Android アプリは、アプリナビゲーションの **[戻る]** ボタンに依存しています。 **[戻る]** ボタンを押すと、ユーザーが前の画面に移動します。
ただし、 **[up]** ボタンを使用して、ユーザーがアプリのメイン画面に簡単に移動できるようにすることもできます。 ユーザーが [**上**へ] ボタンを選択すると、ユーザーはアプリ階層&ndash;の上位レベルに移動します。つまり、アプリは、前に表示されたアクティビティに戻るのではなく、ユーザーをバックスタック内の複数のアクティビティにポップします。 

`Toolbar`アクション`OnCreate` `SetHomeButtonEnabled`バーとしてを使用する2番目のアクティビティで [上へ] ボタンを有効にするには、2番目のアクティビティのメソッドでメソッドとメソッドを呼び出します。`SetDisplayHomeAsUpEnabled`

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[ [Support V7] ツールバー](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)のコードサンプルでは、 **[Up]** ボタンが動作しています。 このサンプルでは、次に説明する AppCompat ライブラリを使用して、前のアクティビティへの階層ナビゲーションのツールバーの **[上]** ボタンを使用する2番目のアクティビティを実装します。 この例`DetailActivity`では、[ホーム] ボタンを使用して、次`SupportActionBar`のメソッドを呼び出すことにより、 **[上へ]** ボタンを有効にします。 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

ユーザーがから`MainActivity`に`DetailActivity`移動すると、 `DetailActivity`スクリーンショットに示されているように**上向き**のボタン (左向きの矢印) が表示されます。

[![ツールバーの上のボタンの左矢印のスクリーンショットの例](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

**このボタンをタップ**すると、アプリはに`MainActivity`戻ります。 複数レベルの階層を持つより複雑なアプリでは、このボタンをタップすると、ユーザーが前の画面ではなく、アプリの次の最上位レベルに戻ります。 

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
