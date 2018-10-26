---
title: ツールバーの互換性
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 12c19cf1024b78e8be30b7c9f2652019e9854375
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110330"
---
# <a name="toolbar-compatibility"></a>ツールバーの互換性


## <a name="overview"></a>概要

このセクションを使用する方法を説明します`Toolbar`Android 5.0 Lollipop より前のバージョンの Android です。 アプリが Android 5.0 より前のバージョンの Android をサポートしていない場合は、このセクションをスキップできます。 

`Toolbar`一部ですが、Android v7 サポート ライブラリでは、Android 2.1 (API レベル 7) を実行しているデバイスで使用できます以降。 ただし、 [Android サポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet がインストールされている必要があり、コード変更を使用するよう、`Toolbar`このライブラリで提供される実装です。 このセクションは、この NuGet をインストールして、変更する方法を説明します、 **ToolbarFun**からアプリ[2 番目のツールバーを追加する](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md)ロリポップ 5.0 より前のバージョンの Android で実行されるようにします。

ツールバーの AppCompat バージョンを使用するアプリを変更するには。 

1.  アプリの最小値とターゲット Android バージョンを設定します。

2.  AppCompat NuGet パッケージをインストールします。

3.  組み込み Android テーマの代わりに AppCompat のテーマを使用します。

4.  変更`MainActivity`ようにそのサブクラス`AppCompatActivity`なく`Activity`します。 

これらの各手順については、次のセクションで詳しく説明します。



## <a name="set-the-minimum-and-target-android-version"></a>最小値とターゲット Android バージョンを設定します。

API レベル 21 以上、アプリのターゲット フレームワークを設定する必要があります。 またはアプリが正常に展開できなくなります。 場合など、エラー**リソース識別子の検出されない属性 'tileModeX' パッケージ 'android' で**表示されるアプリをデプロイするときにこれは、ターゲット フレームワークに設定されていないためは**Android 5.0 (API レベル 21 - ロリポップ)** 以上。 

API レベル 21 レベル以上のターゲット フレームワークを設定し、Android API レベルのプロジェクトの設定をサポートするために、アプリがある最小の Android バージョンに設定します。 Android API レベルを設定する方法についての詳細については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。 `ToolbarFun`例では、最小 Android バージョンは、KitKat (API レベル 4.4) に設定されています。 


## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet パッケージをインストールします。

次に、追加、 [Android サポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)をプロジェクトにパッケージします。 Visual Studio で、右クリックして**参照**選択**NuGet パッケージの管理.**.クリックして**参照**検索**Android サポート ライブラリ v7 AppCompat**します。 選択**Xamarin.Android.Support.v7.AppCompat**クリック**インストール**: 

[![NuGet パッケージの管理で選択されているスクリーン ショットの V7 Appcompat パッケージ](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

この NuGet がインストールされているときにその他のいくつかの NuGet パッケージがインストールされていることもまだ存在しない場合 (など**Xamarin.Android.Support.Animated.Vector.Drawable**、 **Xamarin.Android.Support.v4**、**Xamarin.Android.Support.Vector.Drawable**)。 NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: NuGet でプロジェクトを含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)します。 


## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat テーマとツールバーを使用します。

AppCompat ライブラリが付属のいくつか`Theme.AppCompat`AppCompat ライブラリによってサポートされている Android の任意のバージョンで使用できるテーマ。 `ToolbarFun`は例のアプリのテーマに由来`Theme.Material.Light.DarkActionBar`、ロリポップよりも前の Android バージョンで使用できません。 そのため、`ToolbarFun`このテーマの AppCompat 対応を使用する必要があります`Theme.AppCompat.Light.DarkActionBar`します。 また、ため`Toolbar`は Android のバージョンでは使用できませんロリポップより前を使用しなければならない AppCompat バージョンの`Toolbar`します。 したがって、レイアウトを使用する必要があります`android.support.v7.widget.Toolbar`の代わりに`Toolbar`します。 


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

編集**Resources/layout/toolbar.xml**内容を次の XML に置き換えます。 

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

なお、`?attr`値が不要になった付いて`android:`(することを思い出してください、`?`表記は、現在のテーマでのリソースを参照)。 場合`?android:attr`まだ使用されていたここでは、Android は、属性の値を参照、AppCompat ライブラリではなく、現在実行中のプラットフォームから。 この例を使用するため、 `actionBarSize` 、AppCompat ライブラリによって定義されている、`android:`プレフィックスが削除されます。 同様に、`@android:style`に変更されます`@style`ように、 `android:theme` AppCompat ライブラリの属性が設定をテーマに&ndash;、`ThemeOverlay.AppCompat.Dark.ActionBar`テーマがここでは使用なく`ThemeOverlay.Material.Dark.ActionBar`します。 


### <a name="update-the-style"></a>スタイルを更新します。

編集**Resources/values/styles.xml**内容を次の XML に置き換えます。 

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

項目の名前とこの例では親のテーマが不要になった付いて`android:`AppCompat ライブラリを使用しているためです。 AppCompat バージョンに親のテーマを変更することも、`Light.DarkActionBar`します。 



### <a name="update-menus"></a>メニューを更新します

AppCompat ライブラリを以前のバージョンの Android をサポートするには、属性をミラー化されたカスタム属性を使用して、`android:`名前空間。 ただし、一部の属性 (など、`showAsAction`で使用される属性、`<menu>`タグ) の古いデバイスで Android のフレームワークに存在しない&ndash;`showAsAction`は Android API 11 で導入されましたが、Android API 7 では使用できません。 このため、サポート ライブラリによって定義された属性のすべてのプレフィックス、カスタム名前空間を使用する必要があります。 メニュー リソース ファイルに名前空間と呼ばれる`local`プレフィックスが定義されている、`showAsAction`属性。 

編集**Resources/menu/top_menus.xml**内容を次の XML に置き換えます。

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

`showAsAction`は、属性の前にこの`local:`名前空間ではなくより `android:` 

```csharp
local:showAsAction="ifRoom"
```

同様に、編集**Resources/menu/edit_menus.xml**内容を次の XML に置き換えます。

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

この名前空間のスイッチはサポートを提供する方法、 `showAsAction` API レベル 11 より前の Android のバージョンの属性ですか? カスタム属性`showAsAction`AppCompat NuGet がインストールされている場合、アプリに含まれているすべての可能な値。 


## <a name="subclass-appcompatactivity"></a>サブクラス AppCompatActivity

最後の手順、変換では、変更するのには`MainActivity`のサブクラスをあるように`AppCompactActivity`します。 編集**MainActivity.cs**し、以下の追加`using`ステートメント。 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

これを宣言して`Toolbar`の AppCompat バージョンである`Toolbar`します。 クラス定義を次に、変更`MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

操作バーの AppCompat バージョンに設定する`Toolbar`への呼び出しを置き換える`SetActionBar`で`SetSupportActionBar`。 この例で、タイトルもあることを示す変更の AppCompat バージョン`Toolbar`が使用されています。

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

最後に、最低限の Android のレベルを (API 19 など) のサポートされる事前ロリポップ値に変更します。 

アプリをビルドし、事前ロリポップ デバイスまたは Android エミュレーターで実行します。 次のスクリーン ショットの AppCompat バージョン**ToolbarFun** KitKat (API 19) を実行している Nexus 4。 

[![KitKat デバイスで実行されているアプリの完全なスクリーン ショットは、両方のツールバーが表示されます。](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

AppCompat ライブラリを使用すると場合、テーマ必要はありません切り替わるように Android のバージョンに基づく&ndash;AppCompat ライブラリでは、サポートされているすべての Android バージョン間で一貫したユーザー エクスペリエンスを提供します。 




## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
