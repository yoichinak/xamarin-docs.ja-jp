---
title: 操作バーの置き換え
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: f20568b5d76fcc1788d19497e372bcd0cecc61ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029083"
---
# <a name="replacing-the-action-bar"></a>操作バーの置き換え

## <a name="overview"></a>概要

`Toolbar` を使用する最も一般的な用途の1つは、既定のアクションバーをカスタム `Toolbar` に置き換えることです (新しい Android プロジェクトを作成すると、既定の操作バーが使用されます)。 `Toolbar` には、ブランド化されたロゴ、タイトル、メニュー項目、ナビゲーションボタン、およびカスタムビューをアクティビティの UI のアプリバーセクションに追加する機能が用意されているため、既定の操作バーを大幅にアップグレードできます。

アプリの既定のアクションバーを `Toolbar`に置き換えるには、次のようにします。 

1. 新しいカスタムテーマを作成し、この新しいテーマを使用するようにアプリのプロパティを変更します。 

2. カスタムテーマの `windowActionBar` 属性を無効にし、`windowNoTitle` 属性を有効にします。

3. `Toolbar`のレイアウトを定義します。

4. アクティビティの**メインの axml**レイアウトファイルに `Toolbar` レイアウトを含めます。 

5. アクティビティの `OnCreate` メソッドにコードを追加して `Toolbar` を特定し `SetActionBar` を呼び出して、`ToolBar` を操作バーとしてインストールします。

以下のセクションでは、このプロセスについて詳しく説明します。 単純なアプリが作成され、その操作バーがカスタマイズされた `Toolbar`に置き換えられます。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**Toolbarfun**という名前の新しい android プロジェクトを作成します (新しい android プロジェクトの作成の詳細については[、「Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 」を参照してください)。 このプロジェクトが作成されたら、ターゲットと最小の Android API レベルを**android 5.0 (API レベル 21-ロリポップ)** 以降に設定します。 Android バージョンレベルの設定の詳細については、「 [ANDROID API レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。 アプリをビルドして実行すると、次のスクリーンショットに示すように、既定のアクションバーが表示されます。

[既定のアクションバーの![スクリーンショット](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)

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

この XML では、ロリポップの**DarkActionBar**テーマに基づく**mytheme**という新しいカスタムテーマが定義されています。 タイトルバーを非表示にするには、`windowNoTitle` 属性を `true` に設定します。 

```xml
<item name="android:windowNoTitle">true</item>
```

カスタムツールバーを表示するには、既定の `ActionBar` を無効にする必要があります。 

```xml
<item name="android:windowActionBar">false</item>
```

ツールバーの背景色には、オリーブ緑色の `colorPrimary` 設定が使用されます。 

```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>カスタムテーマを適用する

**Properties/AndroidManifest .xml**を編集し、次の `android:theme` 属性を `<application>` 要素に追加して、アプリが `MyTheme` カスタムテーマを使用するようにします。 

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

この XML は、既定のアクションバーを置き換えるカスタム `Toolbar` を定義します。 `Toolbar` の高さの最小値は、置き換えられる操作バーのサイズに設定されます。 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

`Toolbar` の背景色は、前に**スタイル .xml**で定義したオリーブ色に設定されています。

```csharp
android:background="?android:attr/colorPrimary"
```

ロリポップ以降では、`android:theme` 属性を使用して個々のビューのスタイルを設定できます。 ロリポップで導入された `ThemeOverlay.Material` テーマを使用すると、既定の `Theme.Material` テーマをオーバーレイし、関連する属性を上書きして明るいまたは暗いようにすることができます。 この例では、`Toolbar` はダークテーマを使用して、その内容が明るい色になるようにしています。 

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

このレイアウトには、**ツールバー .xml**で定義された `Toolbar` が含まれています。また、`RelativeLayout` を使用して、`Toolbar` が UI の一番上 (ボタンの上) に配置されることを指定します。 

## <a name="find-and-activate-the-toolbar"></a>ツールバーの検索とアクティブ化

**MainActivity.cs**を編集し、次の using ステートメントを追加します。

```csharp
using Android.Views;
```

また、`OnCreate` メソッドの末尾に次のコード行を追加します。

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

このコードは、`Toolbar` を検索し、`SetActionBar` を呼び出して、`Toolbar` が既定のアクションバーの特性に対して実行されるようにします。 ツールバーのタイトルが **[マイツールバー]** に変わります。 このコード例に示すように、`ToolBar` を操作バーとして直接参照できます。 このアプリをコンパイルして実行すると、カスタマイズされた `Toolbar` が既定のアクションバーの代わりに表示さ &ndash; ます。 

[緑色の配色を使用してカスタマイズされたツールバーのスクリーンショットを![](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

`Toolbar` は、アプリの残りの部分に適用される `Theme.Material.Light.DarkActionBar` テーマとは別にスタイルが設定されていることに注意してください。 

アプリの実行中に例外が発生した場合は、以下の[トラブルシューティングに関する](#troubleshooting)セクションを参照してください。

## <a name="add-menu-items"></a>メニュー項目の追加 

このセクションでは、`Toolbar`にメニューを追加します。 `ToolBar` の右上の領域はメニュー &ndash; 項目用に予約されています。各メニュー項目 (*アクション項目*とも呼ばれます) は、現在のアクティビティ内でアクションを実行できます。また、アプリ全体に代わってアクションを実行することもできます。 

`Toolbar`にメニューを追加するには: 

1. アプリプロジェクトの `mipmap-` フォルダーにメニューアイコン (必要に応じて) を追加します。 Google では、[素材アイコン](https://design.google.com/icons/)ページに一連の無料メニューアイコンが用意されています。 

2. [**リソース] メニュー**の下に新しいメニューリソースファイルを追加して、メニュー項目の内容を定義します。 

3. このメソッドがメニュー項目を増えする &ndash;、アクティビティの `OnCreateOptionsMenu` メソッドを実装します。 

4. アクティビティの `OnOptionsItemSelected` メソッドを実装し &ndash; このメソッドは、メニュー項目がタップされたときにアクションを実行します。 

以下のセクションでは、カスタマイズされた `Toolbar`に **[編集]** メニュー項目と **[保存]** メニュー項目を追加して、このプロセスを詳しく説明します。 

### <a name="install-menu-icons"></a>インストールメニューアイコン

`ToolbarFun` サンプルアプリを引き続き使用して、アプリプロジェクトにメニューアイコンを追加します。 [ツールバーのアイコン](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)をダウンロードして解凍し、抽出した*mipmap*フォルダーの内容を、 **[toolbarfun/Resources]** の下のプロジェクトの*mipmap*フォルダーにコピーして、プロジェクトに追加した各アイコンファイルを含めます。

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

- `ic_action_content_create.png` アイコン (鉛筆) を使用する **[編集]** メニュー項目。 

- `ic_action_content_save.png` アイコン (フロッピーディスク) を使用する **[保存]** メニュー項目。 

- アイコンのない**基本設定**メニュー項目。

**[編集]** および **[保存]** メニュー項目の `showAsAction` 属性は `ifRoom` に設定され &ndash; この設定により、これらのメニュー項目が表示されるのに十分な領域がある場合は、これらのメニュー項目が `Toolbar` に表示されます。 **[基本設定]** メニュー `showAsAction`項目`never`をこれに設定&ndash;すると、[**設定] メニューが** *オーバーフロー*メニューに表示されます (3 つの垂直ドット)。 

### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu を実装する

次のメソッドを**MainActivity.cs**に追加します。

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android では、アプリがアクティビティのメニューリソースを指定できるように `OnCreateOptionsMenu` メソッドを呼び出します。 このメソッドでは、 **top_menus**リソースが、渡された `menu`に拡大されます。 このコードにより、新しい **[編集]** 、 **[保存]** 、 **[基本設定]** の各メニュー項目が `Toolbar`に表示されます。 

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

ユーザーがメニュー項目をタップすると、Android によって `OnOptionsItemSelected` メソッドが呼び出され、選択されたメニュー項目が渡されます。 この例では、実装によって、どのメニュー項目がタップされたかを示すトーストのみが表示されます。 

`ToolbarFun` をビルドして実行すると、ツールバーに新しいメニュー項目が表示されます。 このスクリーンショットに示すように、`Toolbar` に3つのメニューアイコンが表示されるようになりました。 

[編集、保存、およびオーバーフローメニュー項目の場所を示す![ダイアグラム](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

ユーザーが **[編集]** メニュー項目をタップすると、`OnOptionsItemSelected` メソッドが呼び出されたことを示すトーストが表示されます。 

[[項目の編集] がタップされたときに表示されるトーストの![スクリーンショット](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

ユーザーがオーバーフローメニューをタップすると、 **[基本設定]** メニュー項目が表示されます。 通常、あまり一般的ではない操作はオーバーフローメニューに配置する必要があります。この例では、**編集** と **保存** として頻繁に使用されないため、オーバーフロー メニューを使用して**設定**を行う &ndash; ます。 

[オーバーフローメニューに表示される [基本設定] メニュー項目のスクリーンショットを![](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android メニューの詳細については、「Android 開発者[メニュー](https://developer.android.com/guide/topics/ui/menus.html) 」を参照してください。 

## <a name="troubleshooting"></a>トラブルシューティング

次のヒントは、操作バーをツールバーに置き換えるときに発生する可能性のある問題をデバッグするのに役立ちます。

### <a name="activity-already-has-an-action-bar"></a>アクティビティには既に操作バーがあります

「[カスタムテーマの適用](#apply-the-custom-theme)」で説明されているカスタムテーマを使用するようにアプリが適切に構成されていない場合、アプリの実行中に次の例外が発生する可能性があります。

![カスタムテーマが使用されていない場合に発生する可能性があるエラー](replacing-the-action-bar-images/03-theme-not-defined.png)

さらに、次のようなエラーメッセージが生成される可能性があります。 _IllegalStateException: このアクティビティには、ウィンドウ décor によって指定されたアクションバーが既にあり_ます。 

このエラーを修正するには、「[カスタムテーマを適用](#apply-the-custom-theme)する」の説明に従って、カスタムテーマの `android:theme` 属性が `<application>` に追加されていることを確認します ( **Properties/AndroidManifest .xml**)。 また、`Toolbar` レイアウトまたはカスタムテーマが適切に構成されていない場合にも、このエラーが発生することがあります。

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
