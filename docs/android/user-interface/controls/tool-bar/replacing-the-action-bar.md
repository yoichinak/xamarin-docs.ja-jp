---
title: 操作バーの置き換え
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: 9e9fa1e2651661670f89baac7fcd438b3d14bfb3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108224"
---
# <a name="replacing-the-action-bar"></a>操作バーの置き換え

## <a name="overview"></a>概要

最も一般的なのいずれかを使用して、`Toolbar`に置き換えて、カスタムの既定の操作バーが`Toolbar`(新しい Android プロジェクトを作成すると、使用して、既定のアクション バー)。 `Toolbar`アプリ バーのセクションにアクティビティのブランド ロゴ、タイトル、メニュー項目、ナビゲーション ボタン、およびカスタム ビューを追加する機能を提供します。 UI で、既定のアクション バーの上を大幅にアップグレードを提供しています。

アプリの既定の操作バーを置換する、 `Toolbar`: 

1.  新しいカスタム テーマを作成し、この新しいテーマを使用するように、アプリのプロパティを変更します。 

2.  無効にする、`windowActionBar`カスタム テーマの属性し、有効にする、`windowNoTitle`属性。

3.  レイアウトを定義する、`Toolbar`します。

4.  含める、 `Toolbar` 、アクティビティのレイアウト**Main.axml**レイアウト ファイルです。 

5.  アクティビティのコードを追加`OnCreate`を検索するメソッド、`Toolbar`を呼び出すと`SetActionBar`をインストールする、`ToolBar`操作バーとして。

次のセクションでは、このプロセスの詳細を説明します。 簡単なアプリが作成され、その操作バーが置き換えられますカスタマイズされた`Toolbar`します。 



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

という新しい Android プロジェクトを作成**ToolbarFun** (を参照してください[こんにちは, Android](~/android/get-started/hello-android/hello-android-quickstart.md)詳細については、新しい Android プロジェクトの作成) します。 このプロジェクトを作成すると後、は、ターゲットと最小の Android API レベルを設定**Android 5.0 (API レベル 21 - ロリポップ)** またはそれ以降。 Android のバージョン レベルの設定の詳細については、[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)を参照してください。 アプリをビルドして実行して、ときにこのスクリーン ショットで、既定の操作バーが表示されます。

[![既定の操作バーのスクリーン ショット](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>カスタム テーマを作成します。

開く、**リソース/値**ディレクトリおよびという新しいファイルを作成する**styles.xml**します。 その内容を次の XML に置き換えます。 

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

この XML の定義と呼ばれる新しいカスタム テーマ**MyTheme**に基づく、 **Theme.Material.Light.DarkActionBar** Lollipop のテーマ。 `windowNoTitle`属性に設定されて`true`タイトル バーを非表示にします。 

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

アプリにカスタム テーマを適用する方法についての詳細については、[カスタム テーマを使用して](~/android/user-interface/material-theme.md#customtheme)を参照してください。 



## <a name="define-a-toolbar-layout"></a>ツールバーのレイアウトを定義します。

**リソース/レイアウト**ディレクトリと呼ばれる新しいファイルを作成**toolbar.xml**します。 その内容を次の XML に置き換えます。 

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

この XML 定義のカスタム`Toolbar`を置き換える既定の操作バー。 最小の高さ、`Toolbar`に置換するアクション バーのサイズに設定されます。 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

背景色、`Toolbar`以前に定義されている olive-green 色に設定されている**styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

ロリポップ、以降、`android:theme`属性を使用して、個々 のビューのスタイルを設定することができます。 `ThemeOverlay.Material` Lollipop で導入されたテーマにより、既定のオーバーレイできる`Theme.Material`テーマを淡色や濃色のいずれかにする適切な属性を上書きします。 この例で、`Toolbar`その内容が薄い色になるように、ダーク テーマを使用します。 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

この設定は、暗い背景色をメニュー項目を比較するために使用されます。



## <a name="include-the-toolbar-layout"></a>ツールバーのレイアウトを含める

レイアウト ファイルを編集**Resources/layout/Main.axml**内容を次の XML に置き換えます。

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

このレイアウトが含まれています、`Toolbar`で定義されている**toolbar.xml**を使用して、`RelativeLayout`ことを指定する、 `Toolbar` (上部のボタン)、UI の最上部に配置します。 



## <a name="find-and-activate-the-toolbar"></a>検索して、ツールバーをアクティブ化

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

このコードを検索、`Toolbar`と呼び出し`SetActionBar`ように、`Toolbar`既定アクション バーの特性になります。 ツールバーのタイトルは**My ツールバー**します。 このコード例のように、`ToolBar`アクション バーとして直接参照できます。 このアプリをコンパイルして&ndash;、カスタマイズされた`Toolbar`が既定の操作バーの代わりに表示されます。 

[![緑のカラー スキームを使用してカスタマイズされたツールバーのスクリーン ショット](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

注意、`Toolbar`とは無関係のスタイルが、`Theme.Material.Light.DarkActionBar`アプリの残りの部分に適用されているテーマ。 

アプリの実行中に例外が発生した場合は、次を参照してください。、[トラブルシューティング](#troubleshooting)以下のセクション。

 
## <a name="add-menu-items"></a>メニュー項目を追加します。 

このセクションではメニューに追加されます、`Toolbar`します。 右上の領域、`ToolBar`メニュー項目用に予約されて&ndash;各メニュー項目 (とも呼ばれる、*アクション アイテム*) 現在のアクティビティ内でアクションを実行できるか、アプリ全体の代わりにアクションを実行できます。 

メニューを追加する、 `Toolbar`: 

1.  メニューのアイコンを (必要な場合) を追加、`mipmap-`アプリ プロジェクトのフォルダー。 Google の無料のメニュー アイコンのセットを提供する、[素材アイコン](https://design.google.com/icons/)ページ。 

2.  新しいメニュー リソース ファイルを追加することでメニュー項目の内容を定義する**リソース/メニュー**します。 

3.  実装、`OnCreateOptionsMenu`メソッド、アクティビティの&ndash;このメソッドは、メニュー項目を拡張します。 

4.  実装、`OnOptionsItemSelected`メソッド、アクティビティの&ndash;メニュー項目がタップされたときに、このメソッドが操作を実行します。 

次のセクションでは、追加することでこのプロセスの詳細を示す**編集**と**保存**にメニュー項目をカスタマイズされた`Toolbar`します。 



### <a name="install-menu-icons"></a>メニューのアイコンをインストールします。

進める、`ToolbarFun`例のアプリ、メニュー アイコンをアプリ プロジェクトに追加します。 ダウンロード[ツール バー アイコン](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)、解凍の抽出した内容をコピー *mipmap-* フォルダーをプロジェクトに*mipmap-* フォルダー **ToolbarFun/リソース**プロジェクトに追加したアイコンの各ファイルを含めるとします。


### <a name="define-a-menu-resource"></a>メニュー リソースを定義します。

新規作成**メニュー**サブディレクトリ**リソース**します。 **メニュー**サブディレクトリという新しいメニュー リソース ファイルを作成**top_menus.xml**内容を次の XML に置き換えます。 

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

この XML は、次の 3 つのメニュー項目を作成します。

-   **編集**を使用するメニュー項目、`ic_action_content_create.png`アイコン (鉛筆の形)。 

-   A**保存**を使用するメニュー項目、`ic_action_content_save.png`アイコン (フロッピー ディスク)。 

-   A**設定**がアイコン メニュー項目。

`showAsAction`の属性、**編集**と**保存**メニュー項目を設定する`ifRoom`&ndash;この設定により、これらのメニュー項目に表示される、`Toolbar`がある場合それらを表示して用の空き領域。 **設定**メニュー項目のセット`showAsAction`に`never`&ndash;これにより、**の基本設定**メニューに表示される、*オーバーフロー* (3 つのメニュー縦向きドット) です。 


### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu を実装します。

次のメソッドを追加**MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android の呼び出し、`OnCreateOptionsMenu`メソッド、アプリは、アクティビティのメニュー リソースを指定できるようにします。 このメソッドで、 **top_menus.xml**リソースが大きく、渡されたに`menu`。 このコードにより、新しい**編集**、**保存**、および**設定**に表示されるメニュー項目、`Toolbar`します。 



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

Android を呼び出し、ユーザーがメニュー項目をタップしたとき、`OnOptionsItemSelected`メソッドを選択したメニュー項目に渡します。 この例で、実装には、どのメニュー項目がタップされたかを示すトーストだけが表示されます。 

ビルドおよび実行`ToolbarFun`にツールバーで、新しいメニュー項目を参照してください。 `Toolbar`ようになりましたこのスクリーン ショットに示すように 3 つのメニュー アイコンを表示します。 

[![図に、保存、編集の場所を示すと、オーバーフロー メニュー項目](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

ユーザーのタップ時に、**編集**ことを示すトースト、メニュー項目が表示されます、`OnOptionsItemSelected`メソッドが呼び出されました。 

[![アイテムの編集がタップされたときに表示されるトーストのスクリーン ショット](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

ユーザーが、オーバーフロー メニューをタップすると、**設定**メニュー項目が表示されます。 頻度の低い操作をオーバーフロー メニューに配置する通常、&ndash;この例のオーバーフロー メニューを使用して**設定**頻繁に使用されていないためとして**編集**と**保存**: 

[![オーバーフロー メニューに表示されるメニュー項目を基本設定のスクリーン ショット](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android のメニューの詳細については、Android の開発者を参照してください。[メニュー](https://developer.android.com/guide/topics/ui/menus.html)トピック。 
 

## <a name="troubleshooting"></a>トラブルシューティング

次のヒントは、ツールバーで、操作バーを交換している間に発生する問題のデバッグに役立ちます。

### <a name="activity-already-has-an-action-bar"></a>アクティビティは既にアクション バー

説明したように、カスタム テーマを使用するアプリが正しく構成されていない場合[カスタム テーマを適用する](#apply-the-custom-theme)アプリの実行中に、次の例外が発生します。

![カスタム テーマを使用しない場合に発生するエラー](replacing-the-action-bar-images/03-theme-not-defined.png)

エラー メッセージは次が生成するなどのさらに、: _Java.Lang.IllegalStateException: このアクティビティは既にウィンドウも親しみやすくによって提供されるアクション バー。_ 

このエラーを修正することを確認、`android:theme`属性にユーザー定義のテーマが追加された場合`<application>`(で**Properties/AndroidManifest.xml**) 前述の[カスタムテーマを適用](#apply-the-custom-theme). 場合にこのエラーがさらに、発生する可能性があります、`Toolbar`レイアウトまたはカスタム テーマ正しく構成されていません。


## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
