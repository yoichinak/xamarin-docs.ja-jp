---
title: 操作バーの置き換え
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 1339833c9971c70ccb855dc340b12eb60bf22350
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457667"
---
# <a name="replacing-the-action-bar"></a>操作バーの置き換え

## <a name="overview"></a>概要

の最も一般的な用途の1つ `Toolbar` は、既定のアクションバーをカスタムに置き換える `Toolbar` ことです (新しい Android プロジェクトを作成すると、既定の操作バーが使用されます)。 では、ブランド化された `Toolbar` ロゴ、タイトル、メニュー項目、ナビゲーションボタン、およびカスタムビューをアクティビティの UI のアプリバーセクションに追加できるため、既定の操作バーを大幅にアップグレードできます。

アプリの既定のアクションバーを次のように置き換えるには `Toolbar` : 

1. 新しいカスタムテーマを作成し、この新しいテーマを使用するようにアプリのプロパティを変更します。 

2. カスタムテーマの属性を無効にして `windowActionBar` 、属性を有効にし `windowNoTitle` ます。

3. のレイアウトを定義 `Toolbar` します。

4. `Toolbar`アクティビティの**メインの axml**レイアウトファイルにレイアウトを含めます。 

5. アクティビティのメソッドにコードを追加してを検索し、を `OnCreate` `Toolbar` 呼び出し `SetActionBar` てを `ToolBar` アクションバーとしてインストールします。

以下のセクションでは、このプロセスについて詳しく説明します。 単純なアプリが作成され、その操作バーがカスタマイズされたに置き換えられ `Toolbar` ます。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**Toolbarfun**という名前の新しい android プロジェクトを作成します (新しい android プロジェクトの作成の詳細については[、「Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照してください)。 このプロジェクトが作成されたら、ターゲットと最小の Android API レベルを **android 5.0 (API レベル 21-ロリポップ)** 以降に設定します。 Android バージョンレベルの設定の詳細については、「 [ANDROID API レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。 アプリをビルドして実行すると、次のスクリーンショットに示すように、既定のアクションバーが表示されます。

[![既定のアクションバーのスクリーンショット](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)

## <a name="create-a-custom-theme"></a>カスタムテーマを作成する

**Resources/values**ディレクトリを開き、 **styles.xml**という名前の新しいファイルを作成します。 その内容を次の XML に置き換えます。 

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

この XML では、ロリポップの**DarkActionBar**テーマに基づく**mytheme**という新しいカスタムテーマが定義されています。 `windowNoTitle` `true` タイトルバーを非表示にするには、属性をに設定します。 

```xml
<item name="android:windowNoTitle">true</item>
```

カスタムツールバーを表示するには、既定値を無効にする `ActionBar` 必要があります。 

```xml
<item name="android:windowActionBar">false</item>
```

`colorPrimary`ツールバーの背景色には、オリーブ緑色の設定が使用されます。 

```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>カスタムテーマを適用する

**プロパティ/AndroidManifest.xml**を編集し、次の属性を要素に追加して、 `android:theme` `<application>` アプリがカスタムテーマを使用するようにし `MyTheme` ます。 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

カスタムテーマをアプリに適用する方法の詳細については、「 [カスタムテーマの使用](~/android/user-interface/material-theme.md#customtheme)」を参照してください。 

## <a name="define-a-toolbar-layout"></a>ツールバーのレイアウトを定義する

**Resources/layout**ディレクトリで、 **toolbar.xml**という名前の新しいファイルを作成します。 その内容を次の XML に置き換えます。 

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

この XML は、既定のアクションバーを置き換えるカスタムを定義し `Toolbar` ます。 の最小の高さ `Toolbar` は、置き換えられる操作バーのサイズに設定されます。 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

の背景色は、 `Toolbar` 前に **styles.xml**で定義したオリーブ色に設定されています。

```csharp
android:background="?android:attr/colorPrimary"
```

ロリポップ以降では、 `android:theme` 属性を使用して個々のビューのスタイルを設定できます。 ロリポップで導入されたテーマを使用すると、 `ThemeOverlay.Material` 既定のテーマをオーバーレイし、関連する属性を上書きして `Theme.Material` 明るいまたは暗いようにすることができます。 この例では、は `Toolbar` ダークテーマを使用して、その内容が明るい色になるようにしています。 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

この設定は、メニュー項目が濃い背景色とコントラストをするように使用されます。

## <a name="include-the-toolbar-layout"></a>ツールバーのレイアウトを含める

Layout file **Resources/layout/Main. axml** を編集し、その内容を次の xml に置き換えます。

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

このレイアウトにはtoolbar.xmlで定義されているが含まれて `Toolbar` おり、を使用して**toolbar.xml** `RelativeLayout` 、 `Toolbar` が UI の一番上 (ボタンの上) に配置されることを指定します。 

## <a name="find-and-activate-the-toolbar"></a>ツールバーの検索とアクティブ化

**MainActivity.cs**を編集し、次の using ステートメントを追加します。

```csharp
using Android.Views;
```

また、メソッドの末尾に次のコード行を追加し `OnCreate` ます。

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

このコードは、との呼び出しを検索して `Toolbar` `SetActionBar` 、が既定の `Toolbar` アクションバーの特性を取得するようにします。 ツールバーのタイトルが **[マイツールバー]** に変わります。 次のコード例に示すように、を `ToolBar` 操作バーとして直接参照できます。 このアプリをコンパイルして実行し &ndash; ます。カスタマイズした `Toolbar` 内容が既定のアクションバーの代わりに表示されます。 

[![緑色の配色を持つカスタマイズされたツールバーのスクリーンショット](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

は、 `Toolbar` `Theme.Material.Light.DarkActionBar` アプリの残りの部分に適用されるテーマとは別にスタイルが設定されていることに注意してください。 

アプリの実行中に例外が発生した場合は、以下の [トラブルシューティングに関する](#troubleshooting) セクションを参照してください。

## <a name="add-menu-items"></a>メニュー項目の追加 

このセクションでは、メニューをに追加し `Toolbar` ます。 の右上の領域 `ToolBar` はメニュー項目用に予約されています &ndash; 。各メニュー項目 ( *アクション項目*とも呼ばれます) は、現在のアクティビティ内でアクションを実行できます。また、アプリ全体に代わってアクションを実行することもできます。 

にメニューを追加するには、次のようにし `Toolbar` ます。 

1. アプリプロジェクトのフォルダーにメニューアイコン (必要に応じて) を追加 `mipmap-` します。 Google では、 [素材アイコン](https://design.google.com/icons/) ページに一連の無料メニューアイコンが用意されています。 

2. [ **リソース] メニュー**の下に新しいメニューリソースファイルを追加して、メニュー項目の内容を定義します。 

3. `OnCreateOptionsMenu`アクティビティのメソッドを実装し &ndash; ます。このメソッドは、メニュー項目を増えします。 

4. `OnOptionsItemSelected`アクティビティのメソッドを実装し &ndash; ます。このメソッドは、メニュー項目がタップされたときにアクションを実行します。 

以下のセクションでは、カスタマイズされたに [ **編集** ] メニュー項目と [ **保存** ] メニュー項目を追加して、このプロセスを詳しく説明し `Toolbar` ます。 

### <a name="install-menu-icons"></a>インストールメニューアイコン

サンプルアプリを続けて `ToolbarFun` 、アプリプロジェクトにメニューアイコンを追加します。 [ツールバーのアイコン](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)をダウンロードして解凍し、抽出した*mipmap*フォルダーの内容を、[ **toolbarfun/Resources** ] の下のプロジェクトの*mipmap*フォルダーにコピーして、プロジェクトに追加した各アイコンファイルを含めます。

### <a name="define-a-menu-resource"></a>メニューリソースの定義

[**リソース**] の下に新しい**メニュー**サブディレクトリを作成します。 **メニュー**サブディレクトリで、 **top_menus.xml**という名前の新しいメニューリソースファイルを作成し、その内容を次の XML に置き換えます。 

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

- アイコン (鉛筆) を使用する [ **編集** ] メニュー項目 `ic_action_content_create.png` 。 

- **Save** `ic_action_content_save.png` アイコン (フロッピーディスク) を使用する [保存] メニュー項目。 

- アイコンのない **基本設定** メニュー項目。

[ `showAsAction` **編集** ] および [ **保存** ] メニュー項目の属性をこの設定に設定すると `ifRoom` &ndash; 、これらのメニュー項目が表示される `Toolbar` のに十分な領域がある場合は、これらのメニュー項目がに表示されます。 [**基本設定**] メニュー `showAsAction`項目をこれに設定`never` &ndash;すると、[**設定] メニューが***オーバーフロー*メニューに表示されます (3 つの垂直ドット)。 

### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu を実装する

次のメソッドを **MainActivity.cs**に追加します。

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android はメソッドを呼び出して、 `OnCreateOptionsMenu` アプリがアクティビティのメニューリソースを指定できるようにします。 このメソッドでは、 **top_menus.xml** リソースが、渡されたに大きくなり `menu` ます。 このコードにより、新しい [ **編集**]、[ **保存**]、[ **基本設定** ] の各メニュー項目がに表示され `Toolbar` ます。 

### <a name="implement-onoptionsitemselected"></a>OnOptionsItemSelected を実装する

次のメソッドを **MainActivity.cs**に追加します。

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

ユーザーがメニュー項目をタップすると、Android によってメソッドが呼び出され、選択された `OnOptionsItemSelected` メニュー項目が渡されます。 この例では、実装によって、どのメニュー項目がタップされたかを示すトーストのみが表示されます。 

をビルドして実行すると、 `ToolbarFun` ツールバーに新しいメニュー項目が表示されます。 では、 `Toolbar` 次のスクリーンショットのように3つのメニューアイコンが表示されるようになりました。 

[![編集、保存、およびオーバーフローの各メニュー項目の場所を示す図](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

ユーザーが [ **編集** ] メニュー項目をタップすると、メソッドが呼び出されたことを示すトーストが表示され `OnOptionsItemSelected` ます。 

[![[項目の編集] がタップされたときに表示されるトーストのスクリーンショット](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

ユーザーがオーバーフローメニューをタップすると、[ **基本設定** ] メニュー項目が表示されます。 通常、あまり一般的ではないアクションはオーバーフローメニューに配置する必要があり &ndash; ます。この例では、**編集**および**保存**として頻繁に使用されないため、オーバーフローメニューを使用して**設定**を行います。 

[![オーバーフローメニューに表示される [基本設定] メニュー項目のスクリーンショット](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android メニューの詳細については、「Android 開発者 [メニュー](https://developer.android.com/guide/topics/ui/menus.html) 」を参照してください。 

## <a name="troubleshooting"></a>トラブルシューティング

次のヒントは、操作バーをツールバーに置き換えるときに発生する可能性のある問題をデバッグするのに役立ちます。

### <a name="activity-already-has-an-action-bar"></a>アクティビティには既に操作バーがあります

「 [カスタムテーマの適用](#apply-the-custom-theme)」で説明されているカスタムテーマを使用するようにアプリが適切に構成されていない場合、アプリの実行中に次の例外が発生する可能性があります。

![カスタムテーマが使用されていない場合に発生する可能性があるエラー](replacing-the-action-bar-images/03-theme-not-defined.png)

さらに、次のようなエラーメッセージが生成される可能性があります。 _IllegalStateException: このアクティビティには、ウィンドウ décor によって指定されたアクションバーが既にあり_ます。 

このエラーを修正するには、「 `android:theme` カスタムテーマを適用する」の説明に従って、カスタムテーマの属性が `<application>` (**プロパティ/AndroidManifest.xml**) [Apply the Custom Theme](#apply-the-custom-theme)に追加されていることを確認します。 また、 `Toolbar` レイアウトまたはカスタムテーマが適切に構成されていない場合にも、このエラーが発生することがあります。

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)