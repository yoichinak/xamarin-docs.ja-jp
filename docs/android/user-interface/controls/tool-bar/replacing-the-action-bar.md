---
title: 操作バーの置き換え
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: df6a479123dfc0fa2e5a47c9210a4bdf24d066e1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762461"
---
# <a name="replacing-the-action-bar"></a>操作バーの置き換え

## <a name="overview"></a>概要

の最も一般的な用途`Toolbar`の1つは、既定のアクションバーをカスタム`Toolbar`に置き換えることです (新しい Android プロジェクトを作成すると、既定の操作バーが使用されます)。 では、ブランド化されたロゴ、タイトル、メニュー項目、ナビゲーションボタン、およびカスタムビューをアクティビティのUIのアプリバーセクションに追加できるため、既定の操作バーを大幅にアップグレードできます。`Toolbar`

アプリの既定のアクションバーを次のよう`Toolbar`に置き換えるには: 

1. 新しいカスタムテーマを作成し、この新しいテーマを使用するようにアプリのプロパティを変更します。 

2. カスタムテーマの`windowNoTitle` 属性を無効にして、属性を有効`windowActionBar`にします。

3. のレイアウト`Toolbar`を定義します。

4. アクティビティの`Toolbar` **メインの axml**レイアウトファイルにレイアウトを含めます。 

5. アクティビティの`OnCreate`メソッドにコードを追加してを`Toolbar`検索し`SetActionBar` 、を呼び出し`ToolBar`てをアクションバーとしてインストールします。

以下のセクションでは、このプロセスについて詳しく説明します。 単純なアプリが作成され、その操作バーがカスタマイズ`Toolbar`されたに置き換えられます。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**Toolbarfun**という名前の新しい android プロジェクトを作成します (新しい android プロジェクトの作成の詳細については[、「Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照してください)。 このプロジェクトが作成されたら、ターゲットと最小の Android API レベルを**android 5.0 (API レベル 21-ロリポップ)** 以降に設定します。 Android バージョンレベルの設定の詳細については、「 [ANDROID API レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。 アプリをビルドして実行すると、次のスクリーンショットに示すように、既定のアクションバーが表示されます。

[![既定のアクションバーのスクリーンショット](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)

## <a name="create-a-custom-theme"></a>カスタムテーマを作成する

**Resources/values**ディレクトリを開き、「 **styles**」という名前の新しいファイルを作成します。 その内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

この XML では、ロリポップの**DarkActionBar**テーマに基づく**mytheme**という新しいカスタムテーマが定義されています。 タイトル`windowNoTitle`バーを非表示`true`にするには、属性をに設定します。 

```xml
<item name="android:windowNoTitle">true</item>
```

カスタムツールバーを表示するには`ActionBar` 、既定値を無効にする必要があります。 

```xml
<item name="android:windowActionBar">false</item>
```

ツールバーの背景`colorPrimary`色には、オリーブ緑色の設定が使用されます。 

```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>カスタムテーマを適用する

**Properties/androidmanifest**を編集し、次`android:theme`の属性を`<application>`要素に追加して、アプリがカスタム`MyTheme`テーマを使用するようにします。 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

カスタムテーマをアプリに適用する方法の詳細については、「[カスタムテーマの使用](~/android/user-interface/material-theme.md#customtheme)」を参照してください。 

## <a name="define-a-toolbar-layout"></a>ツールバーのレイアウトを定義する

**Resources/layout**ディレクトリで、 **toolbar**という名前の新しいファイルを作成します。 その内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

この XML は、既定`Toolbar`のアクションバーを置き換えるカスタムを定義します。 の最小の高さ`Toolbar`は、置き換えられる操作バーのサイズに設定されます。 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

の`Toolbar`背景色は、以前に**スタイル .xml**で定義したオリーブ色に設定されています。

```csharp
android:background="?android:attr/colorPrimary"
```

ロリポップ以降では、 `android:theme`属性を使用して個々のビューのスタイルを設定できます。 ロリポップ`ThemeOverlay.Material`で導入されたテーマを使用すると、 `Theme.Material`既定のテーマをオーバーレイし、関連する属性を上書きして明るいまたは暗いようにすることができます。 この例では、 `Toolbar`はダークテーマを使用して、その内容が明るい色になるようにしています。 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

この設定は、メニュー項目が濃い背景色とコントラストをするように使用されます。

## <a name="include-the-toolbar-layout"></a>ツールバーのレイアウトを含める

Layout file **Resources/layout/Main. axml**を編集し、その内容を次の xml に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
</RelativeLayout>
```

このレイアウトには`Toolbar` 、**ツールバー .xml**で定義され`RelativeLayout`たが含まれ`Toolbar`ており、を使用して、が UI の最上部 (ボタンの上) に配置されることを指定します。 

## <a name="find-and-activate-the-toolbar"></a>ツールバーの検索とアクティブ化

**MainActivity.cs**を編集し、次の using ステートメントを追加します。

```csharp
using Android.Views;
```

また、 `OnCreate`メソッドの末尾に次のコード行を追加します。

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

このコードは、 `Toolbar`との`SetActionBar`呼び出しを検索`Toolbar`して、が既定のアクションバーの特性を取得するようにします。 ツールバーのタイトルが **[マイツールバー]** に変わります。 次のコード例に示すように`ToolBar` 、を操作バーとして直接参照できます。 このアプリ&ndash;をコンパイルして実行`Toolbar`します。カスタマイズした内容が既定のアクションバーの代わりに表示されます。 

[![緑色の配色を持つカスタマイズされたツールバーのスクリーンショット](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

は、 `Toolbar`アプリの残りの部分に`Theme.Material.Light.DarkActionBar`適用されるテーマとは別にスタイルが設定されていることに注意してください。 

アプリの実行中に例外が発生した場合は、以下の[トラブルシューティングに関する](#troubleshooting)セクションを参照してください。

## <a name="add-menu-items"></a>メニュー項目の追加 

このセクションでは、 `Toolbar`メニューをに追加します。 の右上の領域`ToolBar`はメニュー項目&ndash;用に予約されています。各メニュー項目 (*アクション項目*とも呼ばれます) は、現在のアクティビティ内でアクションを実行できます。また、アプリ全体に代わってアクションを実行することもできます。 

にメニューを追加する`Toolbar`には、次のようにします。 

1. アプリプロジェクトのフォルダーにメニューアイコン (必要`mipmap-`に応じて) を追加します。 Google では、[素材アイコン](https://design.google.com/icons/)ページに一連の無料メニューアイコンが用意されています。 

2. [**リソース] メニュー**の下に新しいメニューリソースファイルを追加して、メニュー項目の内容を定義します。 

3. `OnCreateOptionsMenu` アクティビティ&ndash;のメソッドを実装します。このメソッドは、メニュー項目を増えします。 

4. `OnOptionsItemSelected` アクティビティ&ndash;のメソッドを実装します。このメソッドは、メニュー項目がタップされたときにアクションを実行します。 

以下のセクションでは、カスタマイズ`Toolbar`されたに **[編集]** メニュー項目と **[保存]** メニュー項目を追加して、このプロセスを詳しく説明します。 

### <a name="install-menu-icons"></a>インストールメニューアイコン

`ToolbarFun`サンプルアプリを続けて、アプリプロジェクトにメニューアイコンを追加します。 [ツールバーのアイコン](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)をダウンロードして解凍し、抽出した*mipmap*フォルダーの内容を、 **[toolbarfun/Resources]** の下のプロジェクトの*mipmap*フォルダーにコピーして、プロジェクトに追加した各アイコンファイルを含めます。

### <a name="define-a-menu-resource"></a>メニューリソースの定義

**[リソース]** の下に新しい**メニュー**サブディレクトリを作成します。 **メニュー**サブディレクトリで、 **top_menus**という名前の新しいメニューリソースファイルを作成し、その内容を次の xml に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

この XML では、次の3つのメニュー項目が作成されます。

- `ic_action_content_create.png`アイコン (鉛筆) を使用する **[編集]** メニュー項目。 

- `ic_action_content_save.png`アイコン (フロッピーディスク) を使用する **[保存]** メニュー項目。 

- アイコンのない**基本設定**メニュー項目。

`Toolbar` &ndash; `ifRoom` `showAsAction` **[編集]** および **[保存]** メニュー項目の属性をこの設定に設定すると、これらのメニュー項目が表示されるのに十分な領域がある場合は、これらのメニュー項目がに表示されます。 **[基本設定]** メニュー `showAsAction`項目`never`をこれに設定&ndash;すると、[**設定] メニューが** *オーバーフロー*メニューに表示されます (3 つの垂直ドット)。 

### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu を実装する

次のメソッドを**MainActivity.cs**に追加します。

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android はメソッド`OnCreateOptionsMenu`を呼び出して、アプリがアクティビティのメニューリソースを指定できるようにします。 このメソッドでは、 **top_menus**リソースが渡さ`menu`れたに大きくなります。 このコード`Toolbar`により、新しい **[編集]** 、 **[保存]** 、 **[基本設定]** の各メニュー項目がに表示されます。 

### <a name="implement-onoptionsitemselected"></a>OnOptionsItemSelected を実装する

次のメソッドを**MainActivity.cs**に追加します。

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

ユーザーがメニュー項目をタップすると、Android に`OnOptionsItemSelected`よってメソッドが呼び出され、選択されたメニュー項目が渡されます。 この例では、実装によって、どのメニュー項目がタップされたかを示すトーストのみが表示されます。 

をビルドし`ToolbarFun`て実行すると、ツールバーに新しいメニュー項目が表示されます。 で`Toolbar`は、次のスクリーンショットのように3つのメニューアイコンが表示されるようになりました。 

[![編集、保存、およびオーバーフローの各メニュー項目の場所を示す図](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

ユーザーが **[編集]** メニュー項目をタップすると、 `OnOptionsItemSelected`メソッドが呼び出されたことを示すトーストが表示されます。 

[![[項目の編集] がタップされたときに表示されるトーストのスクリーンショット](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

ユーザーがオーバーフローメニューをタップすると、 **[基本設定]** メニュー項目が表示されます。 通常、あまり一般的ではないアクションはオーバーフローメニュー &ndash;に配置する必要があります。この例では、**編集**および**保存**として頻繁に使用されないため、オーバーフローメニューを使用して**設定**を行います。 

[![オーバーフローメニューに表示される [基本設定] メニュー項目のスクリーンショット](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android メニューの詳細については、「Android 開発者[メニュー](https://developer.android.com/guide/topics/ui/menus.html) 」を参照してください。 

## <a name="troubleshooting"></a>トラブルシューティング

次のヒントは、操作バーをツールバーに置き換えるときに発生する可能性のある問題をデバッグするのに役立ちます。

### <a name="activity-already-has-an-action-bar"></a>アクティビティには既に操作バーがあります

「[カスタムテーマの適用](#apply-the-custom-theme)」で説明されているカスタムテーマを使用するようにアプリが適切に構成されていない場合、アプリの実行中に次の例外が発生する可能性があります。

![カスタムテーマが使用されていない場合に発生する可能性があるエラー](replacing-the-action-bar-images/03-theme-not-defined.png)

さらに、次のようなエラーメッセージが生成される場合があります。_IllegalStateException:このアクティビティには、ウィンドウ décor によって指定されたアクションバーが既にあります。_ 

このエラーを修正するには、 `android:theme` 「[カスタムテーマを適用](#apply-the-custom-theme)する」の`<application>`説明に従って、カスタムテーマの属性が (**プロパティ/androidmanifest .xml**) に追加されていることを確認します。 また、 `Toolbar`レイアウトまたはカスタムテーマが適切に構成されていない場合にも、このエラーが発生することがあります。

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
