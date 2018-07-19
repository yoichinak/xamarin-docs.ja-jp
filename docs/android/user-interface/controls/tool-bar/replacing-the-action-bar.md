---
title: 操作バーを置き換える
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/27/2018
ms.openlocfilehash: d9a5db70999a79d9b968fcd9a27d45e6d73354c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772592"
---
# <a name="replacing-the-action-bar"></a>操作バーを置き換える

## <a name="overview"></a>概要

最も一般的なのいずれかを使用して、`Toolbar`と置換する既定のアクション バー カスタム`Toolbar`(新しい Android プロジェクトの作成時に、バーを使用して、既定のアクション) です。 `Toolbar`アプリ バーのセクションに、アクティビティのブランド ロゴ、タイトル、メニュー項目、ナビゲーション ボタンとカスタム ビューを追加する機能は、UI で、既定のアクションのバーの上大幅にアップグレードを提供しています。

アプリの既定のアクションのバーを置換する、 `Toolbar`: 

1.  新しいカスタム テーマを作成し、この新しいテーマを使用するようにアプリケーションのプロパティを変更します。 

2.  無効にする、`windowActionBar`ユーザー定義のテーマでの属性を有効にする、`windowNoTitle`属性。

3.  レイアウトを定義する、`Toolbar`です。

4.  含める、`Toolbar`アクティビティのレイアウト**Main.axml**レイアウト ファイルです。 

5.  アクティビティのコードを追加`OnCreate`を探す方法、`Toolbar`を呼び出すと`SetActionBar`をインストールする、`ToolBar`操作バーとして。

次のセクションでは、このプロセスの詳細を説明します。 簡単なアプリの作成し、その操作バーを置き換えるカスタマイズされた`Toolbar`です。 



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

いう新しい Android プロジェクトを作成する**ToolbarFun** (を参照してください[こんにちは, Android](~/android/get-started/hello-android/hello-android-quickstart.md)詳細については、Android プロジェクトを新規作成) します。 このプロジェクトを作成した後にターゲットと最低限の Android API レベルを設定**Android 5.0 (API レベル 21 - ロリポップ)** またはそれ以降。 Android バージョン レベルの設定の詳細については、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。 アプリがビルドされ、実行、ときに、このスクリーン ショットに示すよう、既定値アクション バーを表示します。

[![既定のアクションのバーのスクリーン ショット](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>カスタム テーマを作成します。

開く、**リソース/値**ディレクトリと新しいファイルと呼ばれる、create **styles.xml**です。 その内容を次の XML に置き換えます。 

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

この XML 定義と呼ばれる新しいカスタム テーマ**MyTheme**に基づく、 **Theme.Material.Light.DarkActionBar**ロリポップでテーマ。 `windowNoTitle`属性に設定されている`true`タイトル バーを非表示にします。 

```xml
<item name="android:windowNoTitle">true</item>
```

カスタムのツールバーでは、既定値を表示する`ActionBar`無効にする必要があります。 

```xml
<item name="android:windowActionBar">false</item>
```

Olive-green`colorPrimary`ツールバーの背景色の設定を使用します。 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>カスタム テーマを適用します。

編集**Properties/AndroidManifest.xml**し、以下の追加`android:theme`属性を`<application>`要素、アプリが使用できるように、`MyTheme`カスタム テーマ。 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

詳細については、カスタム テーマをアプリに適用する、次を参照してください。[カスタム テーマを使用して](~/android/user-interface/material-theme.md#customtheme)です。 



## <a name="define-a-toolbar-layout"></a>ツールバーのレイアウトを定義します。

**リソース/レイアウト**ディレクトリと呼ばれる新しいファイルを作成**toolbar.xml**です。 その内容を次の XML に置き換えます。 

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

この XML 定義のカスタム`Toolbar`を置き換える既定のアクション バー。 最小の高さ、`Toolbar`が置き換えられる操作バーのサイズに設定します。 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

背景色、`Toolbar`以前に定義されている olive-green 色に設定されている**styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

ロリポップ、始まる、`android:theme`属性は、個々 のビューのスタイル設定を使用することができます。 `ThemeOverlay.Material`ロリポップで導入されたテーマようなります。 既定値をオーバーレイする`Theme.Material`テーマ、明るいまたは暗いように関連する属性を上書きします。 この例では、`Toolbar`ダーク テーマを使用するので、その内容は次の部分は薄い色。 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

メニュー項目は、暗い背景色と比較できるように、この設定が使用されます。



## <a name="include-the-toolbar-layout"></a>ツールバーのレイアウトを含める

レイアウト ファイルを編集して**Resources/layout/Main.axml**しその内容を次の XML に置き換えます。

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

このレイアウトが含まれています、`Toolbar`で定義されている**toolbar.xml**を使用して、`RelativeLayout`ことを指定する、 `Toolbar` (ボタンの上に)、UI の最上部に配置します。 



## <a name="find-and-activate-the-toolbar"></a>検索し、ツールバーをアクティブ化

編集**MainActivity.cs**し、以下の追加ステートメントを使用します。

```csharp
using Android.Views;
```

また、次のコード行の末尾に追加、`OnCreate`メソッド。

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

このコードを検索、`Toolbar`と呼び出し`SetActionBar`できるように、`Toolbar`アクション バー特性の既定値になります。 ツールバーのタイトルは**マイ ツールバー**です。 このコード例のように、`ToolBar`操作バーとして直接参照することができます。 コンパイルしてこのアプリを実行する&ndash;、カスタマイズされた`Toolbar`が既定のアクションのバーの代わりに表示されます。 

[![緑の画面の配色とユーザー定義のツールバーのスクリーン ショット](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

注意して、`Toolbar`のスタイルとは無関係に、`Theme.Material.Light.DarkActionBar`アプリの残りの部分に適用されているテーマ。 

アプリの実行中に例外が発生した場合は、次を参照してください。、[トラブルシューティング](#troubleshooting)以下のセクションです。

 
## <a name="add-menu-items"></a>メニュー項目を追加します。 

このセクションではメニューに追加されます、`Toolbar`です。 右上の領域、`ToolBar`メニュー項目用に予約されて&ndash;各メニュー項目 (とも呼ばれる、*アクション アイテム*) または実行できます。 現在のアクティビティ内のアクションは、アプリ全体の代理としてアクションを実行できます。 

メニューを追加する、 `Toolbar`: 

1.  メニューのアイコンを (必要な場合) を追加、`mipmap-`アプリ プロジェクトのフォルダーです。 Google は、空き メニューのアイコンのセットを提供、[材料アイコン](https://design.google.com/icons/)ページ。 

2.  新しいメニュー リソース ファイルを追加することで、メニュー項目の内容を定義**リソース/メニュー**です。 

3.  実装、 `OnCreateOptionsMenu` 、アクティビティのメソッド&ndash;このメソッドは、メニュー項目を拡張します。 

4.  実装、 `OnOptionsItemSelected` 、アクティビティのメソッド&ndash;メニュー項目がタップされたときに、このメソッドは、アクションを実行します。 

次のセクションでは、追加することでこのプロセスの詳細を示す**編集**と**保存**にメニュー項目をカスタマイズされた`Toolbar`です。 



### <a name="install-menu-icons"></a>メニューのアイコンをインストールします。

続行、`ToolbarFun`例のアプリ、アプリ プロジェクトにメニューのアイコンを追加します。 ダウンロード[ツールバーのアイコン](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)、展開、および、抽出の内容をコピー *mipmap -* フォルダーをプロジェクトに*mipmap -* の下にフォルダー **ToolbarFun/リソース**プロジェクトに追加したアイコンの各ファイルを含めるとします。


### <a name="define-a-menu-resource"></a>メニュー リソースを定義します。

新しい**メニュー**の下のサブディレクトリ**リソース**です。 **メニュー**サブディレクトリと呼ばれる新しいメニュー リソース ファイルを作成**top_menus.xml**しその内容を次の XML に置き換えます。 

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

この XML では、次の 3 つのメニュー項目を作成します。

-   **編集**が使用するメニュー項目、`ic_action_content_create.png`アイコン (鉛筆)。 

-   A**保存**が使用するメニュー項目、`ic_action_content_save.png`アイコン (フロッピー ディスク)。 

-   A**設定**アイコンがないメニュー項目。

`showAsAction`の属性、**編集**と**保存**メニュー項目を設定する`ifRoom`&ndash;この設定により、これらのメニュー項目に表示される、`Toolbar`がある場合表示する十分な領域。 **設定**メニュー項目のセットを`showAsAction`に`never`&ndash;これが原因で、**設定**メニューに表示される、*オーバーフロー*メニュー (3垂直方向のドット数)。 


### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu を実装します。

次のメソッドを追加**MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android の呼び出し、`OnCreateOptionsMenu`メソッド、アプリは、アクティビティのメニュー リソースを指定できるようにします。 このメソッドで、 **top_menus.xml**リソースが大きく、渡されたに`menu`です。 このコードにより、新しい**編集**、**保存**、および**設定**に表示するメニュー項目、`Toolbar`です。 



### <a name="implement-onoptionsitemselected"></a>OnOptionsItemSelected を実装します。

次のメソッドを追加**MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Android を呼び出すユーザーがメニュー項目をタップしたとき、`OnOptionsItemSelected`メソッドを選択されたメニュー項目に渡します。 この例では、実装には、どのメニュー項目がタップされたを示すためにトーストだけが表示されます。 

ビルドおよび実行する`ToolbarFun`ツールバーの新しいメニュー項目を表示します。 `Toolbar`このスクリーン ショットに示すように 3 つのメニューのアイコンが表示されます。 

[![図の保存、編集の場所を示すと、オーバーフロー メニュー項目](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

ときにユーザー タップ、**編集**メニュー アイテム、トーストを表示することを示す、`OnOptionsItemSelected`メソッドが呼び出されました。 

[![アイテムの編集がタップされたときに表示されるトーストのスクリーン ショット](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

ユーザーが、オーバーフロー メニューをタップしたときに、**設定**メニュー項目が表示されます。 通常より一般的なアクションをオーバーフロー メニューに配置する必要&ndash;この例のオーバーフロー メニューを使用して**設定**が頻繁に使用されないためとして**編集**と**保存**: 

[![オーバーフロー メニューに表示されるスクリーン ショットの設定 メニュー項目](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android のメニューに関する詳細については、Android Developer を参照してください。[メニュー](https://developer.android.com/guide/topics/ui/menus.html)トピックです。 
 

## <a name="troubleshooting"></a>トラブルシューティング

次のヒントは、ツールバーで、アクションのバーを置き換え中に発生する可能性のある問題をデバッグするのに役立ちます。

### <a name="activity-already-has-an-action-bar"></a>アクティビティは既に操作バー

説明したように、カスタム テーマを使用するアプリが正しく構成されていない場合[カスタム テーマを適用する](#apply-the-custom-theme)アプリの実行中、次の例外が発生します。

![カスタム テーマを使用しない場合に発生するエラー](replacing-the-action-bar-images/03-theme-not-defined.png)

エラー メッセージは次が生成するなどのさらに、: _Java.Lang.IllegalStateException: この活動は既にウィンドウ装飾によって提供される操作バー。_ 

このエラーを修正することを確認、`android:theme`属性をユーザー定義のテーマが追加される`<application>`(で**Properties/AndroidManifest.xml**) 前述の[のカスタムテーマを適用する](#apply-the-custom-theme). 場合にこのエラーがさらに、発生する可能性があります、`Toolbar`レイアウト] または [カスタム テーマ正しく構成されていません。


## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
