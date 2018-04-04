---
title: ツールバーの互換性
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: c3418a007ded30175049e83d515f59d5134deee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="toolbar-compatibility"></a>ツールバーの互換性


## <a name="overview"></a>概要

このセクションを使用する方法について説明`Toolbar`Android 5.0 ロリポップより前のバージョンの Android でします。 アプリが Android 5.0 よりも前のバージョンの Android をサポートしていない場合は、このセクションを省略できます。 

`Toolbar`一部であること、Android v7 サポート ライブラリの Android 2.1 (API レベル 7) を実行しているデバイスで使用できます以降。 ただし、 [Android のサポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet がインストールされている必要があり、コード変更を使用するように、`Toolbar`このライブラリで提供される実装します。 このセクションでは、この NuGet をインストールして、変更する方法を説明します、 **ToolbarFun**からアプリ[2 番目のツールバーを追加する](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md)ロリポップ 5.0 よりも前のバージョンの Android 上で実行されるようにします。

ツールバーの AppCompat バージョンを使用するアプリを変更するには。 

1.  アプリの最小値とターゲットの Android バージョンを設定します。

2.  AppCompat NuGet パッケージをインストールします。

3.  組み込みの Android テーマではなく AppCompat テーマを使用します。

4.  変更`MainActivity`ようにそのサブクラス`AppCompatActivity`なく`Activity`です。 

これらの各手順については、次のセクションで詳しく説明します。



## <a name="set-the-minimum-and-target-android-version"></a>最小値とターゲットの Android バージョンを設定します。

API レベル 21 以上、アプリのターゲット フレームワークを設定する必要がありますか、アプリが正常に展開できなくなります。 場合など、エラー**リソース識別子の属性 'tileModeX' に見つかりませんパッケージ 'android'**に見られる、アプリを展開するときにこれは、ターゲット フレームワークに設定されていないために、 **Android 5.0 (API レベル 21 - ロリポップ)**以上です。 

API レベル 21 レベル以上のターゲット フレームワークを設定し、Android API レベルのプロジェクトの設定をサポートするために、アプリは、最低限の Android バージョンに設定します。 Android API レベルの設定についての詳細については、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。 `ToolbarFun`例では、最低限の Android バージョンに設定されている KitKat (API レベル 4.4)。 


## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet パッケージをインストールします。

次に、追加、 [Android のサポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)をプロジェクトにパッケージします。 Visual Studio を右クリックし**参照**選択**NuGet パッケージを管理しています.**.をクリックして**参照**を検索および**Android のサポート ライブラリ v7 AppCompat**です。 選択**Xamarin.Android.Support.v7.AppCompat**  をクリック**インストール**: 

[![スクリーン ショットの V7 Appcompat パッケージ管理の NuGet パッケージの選択](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

この NuGet がインストールされているときに他のいくつかの NuGet パッケージもインストールがまだ存在しない場合 (など**Xamarin.Android.Support.Animated.Vector.Drawable**、 **Xamarin.Android.Support.v4**、および**Xamarin.Android.Support.Vector.Drawable**)。 NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)です。 


## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat テーマとツールバーを使用します。

AppCompat ライブラリが付属のいくつか`Theme.AppCompat`AppCompat ライブラリによってサポートされている Android の任意のバージョンで使用できるテーマ。 `ToolbarFun`は例のアプリのテーマに由来`Theme.Material.Light.DarkActionBar`、ロリポップよりも前の Android バージョンに収録されていません。 したがって、`ToolbarFun`をこのテーマに AppCompat 対応するものを使用する必要があります`Theme.AppCompat.Light.DarkActionBar`です。 また、ため`Toolbar`は Android のバージョンでは使用できない、ロリポップより前お必要があります AppCompat のバージョンを使用`Toolbar`です。 したがって、レイアウトを使用する必要があります`android.support.v7.widget.Toolbar`の代わりに`Toolbar`です。 


### <a name="update-layouts"></a>レイアウトを更新します。

編集**Resources/layout/Main.axml**と置換、`Toolbar`を次の XML 要素。 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

編集**Resources/layout/toolbar.xml**しその内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

なお、`?attr`値が付きます。 されなく`android:`(ことに注意してください、`?`表記は、現在のテーマでのリソースを参照)。 場合`?android:attr`使用されたもここでは、Android は、属性の値を参照 AppCompat ライブラリではなく、現在実行されているプラットフォームからです。 この例を使用するため、 `actionBarSize` AppCompat ライブラリで定義されている、`android:`プレフィックスを削除します。 同様に、`@android:style`に変更が`@style`ように、`android:theme`属性 AppCompat ライブラリのテーマに設定されている&ndash;、`ThemeOverlay.AppCompat.Dark.ActionBar`テーマがここでは使用ではなく`ThemeOverlay.Material.Dark.ActionBar`です。 


### <a name="update-the-style"></a>スタイルを更新します。

編集**Resources/values/styles.xml**しその内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

項目名およびこの例では親テーマが不要になった付いた`android:`AppCompat ライブラリを使用しているためです。 AppCompat バージョンに親テーマの変更も、`Light.DarkActionBar`です。 



### <a name="update-menus"></a>メニューを更新します

Android の以前のバージョンをサポートするために、AppCompat ライブラリで使用するカスタム属性の属性を反映した、`android:`名前空間。 ただし、一部の属性 (など、`showAsAction`で使用される属性、`<menu>`タグ) の古いデバイスで Android のフレームワークに存在しない&ndash;`showAsAction`は Android API 11 で導入されましたが、Android API 7 では使用できません。 このため、すべてのサポート ライブラリで定義された属性のプレフィックス、カスタム名前空間を使用する必要があります。 メニュー リソース ファイルの名前空間と呼ばれる`local`の前に付ける定義、`showAsAction`属性。 

編集**Resources/menu/top_menus.xml**しその内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local`名前空間は、この行に追加されます。

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction`属性は前にこの`local:`名前空間ではなくより `android:` 

```csharp
local:showAsAction="ifRoom"
```

同様に、編集**Resources/menu/edit_menus.xml**しその内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

この名前空間のスイッチを提供する方法のサポート、 `showAsAction` Android バージョン API レベル 11 より前の属性ですか? カスタム属性`showAsAction`AppCompat NuGet がインストールされているときに、アプリに含まれているすべての可能な値です。 


## <a name="subclass-appcompatactivity"></a>サブクラス AppCompatActivity

変換の最後の手順を変更する`MainActivity`のサブクラスであるように`AppCompactActivity`です。 編集**MainActivity.cs**し、以下の追加`using`ステートメント。 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

これにより宣言`Toolbar`の AppCompat バージョン`Toolbar`します。 クラス定義を次に、変更`MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

操作バーを AppCompat のバージョンに設定する`Toolbar`への呼び出しに置き換える`SetActionBar`で`SetSupportActionBar`です。 この例ではタイトルも変更することを示すための AppCompat バージョン`Toolbar`が使用されています。

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

最後に、(たとえば、API 19) サポートされるためには、前のロリポップ値に最低限の Android のレベルを変更します。 

アプリをビルドし、前のロリポップ デバイスまたは Android エミュレーターで実行します。 次のスクリーン ショットを示しています、AppCompat **ToolbarFun** KitKat (API 19) を実行している Nexus 4。 

[![KitKat デバイスで実行されているアプリの全画面スクリーン ショットは、両方のツールバーが表示されます。](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

AppCompat ライブラリを使用するとテーマしなくても切り替えられる Android のバージョンに基づく&ndash;AppCompat ライブラリでは、サポートされているすべての Android バージョン間で一貫性のあるユーザー エクスペリエンスを提供することです。 




## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
