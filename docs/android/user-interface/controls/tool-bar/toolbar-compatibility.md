---
title: ツール バーの互換性
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: f8dcda63c97c15367157b6a6cb857c0bfef79a27
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764586"
---
# <a name="toolbar-compatibility"></a>ツール バーの互換性

## <a name="overview"></a>概要

このセクションでは、android `Toolbar` 5.0 ロリポップより前のバージョンの android でを使用する方法について説明します。 アプリが Android 5.0 より前のバージョンの Android をサポートしていない場合は、このセクションをスキップできます。 

は`Toolbar` android v7 サポートライブラリの一部であるため、android 2.1 (API レベル 7) 以降を実行しているデバイスで使用できます。 ただし、 [Android サポートライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet をインストールし、このライブラリに用意されている`Toolbar`実装を使用するようにコードを変更する必要があります。 このセクションでは、この NuGet をインストールする方法について説明します。 **Toolbarfun**アプリを変更し、 [2 つ目のツールバーを追加](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md)して、ロリポップ5.0 より前のバージョンの Android で動作するようにします。

AppCompat バージョンのツールバーを使用するようにアプリを変更するには、次のようにします。 

1. アプリの Android の最小バージョンとターゲットを設定します。

2. AppCompat NuGet パッケージをインストールします。

3. 組み込みの Android テーマではなく、AppCompat テーマを使用します。

4. を`MainActivity`ではなくサブクラス`AppCompatActivity`に変更します。`Activity` 

これらの各手順の詳細については、次のセクションで説明します。

## <a name="set-the-minimum-and-target-android-version"></a>Android の最小バージョンとターゲットを設定する

アプリのターゲットフレームワークは、API レベル21以上に設定する必要があります。それ以外の場合、アプリは適切にデプロイされません。 アプリのデプロイ中に、**パッケージ ' tileModeX ' の属性 ' ' にリソース識別子が見つからない**などのエラーが表示された場合、これは、ターゲットフレームワークが**ANDROID 5.0 (API レベル 21-ロリポップ)** 以上に設定されていないことが原因です。 

ターゲットフレームワークレベルを API レベル21以上に設定し、Android API レベルのプロジェクト設定を、アプリがサポートする最小の Android バージョンに設定します。 Android API レベルの設定の詳細については、「 [ANDROID Api レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。 `ToolbarFun`この例では、Android の最小バージョンは KitKat (API レベル 4.4) に設定されています。 

## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet パッケージをインストールする

次に、 [Android Support Library V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)パッケージをプロジェクトに追加します。 Visual Studio で、 **[参照]** を右クリックし、 **[NuGet パッケージの管理...]** を選択します。 **[参照]** をクリックし、 **Android Support Library v7 AppCompat**を検索します。 **V7**を選択し、 **[インストール]** をクリックします。 

[![NuGet パッケージの管理」で選択した V7 Appcompat パッケージのスクリーンショット](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

この NuGet がインストールされている場合は、他のいくつかの NuGet パッケージがまだ存在していない**場合にも**インストールされます ( **xamarin** **.....................................Xamarin. Android**... Vector. NuGet パッケージのインストールの詳細について[は、「チュートリアル:プロジェクト](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)に NuGet を含めます。 

## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat テーマとツールバーを使用する

Appcompat ライブラリには、appcompat `Theme.AppCompat`ライブラリでサポートされているすべてのバージョンの Android で使用できるテーマがいくつか付属しています。 サンプル`ToolbarFun`アプリのテーマはから`Theme.Material.Light.DarkActionBar`派生しています。これは、ロリポップより前の Android バージョンでは使用できません。 したがって`ToolbarFun` 、この`Theme.AppCompat.Light.DarkActionBar`テーマに対応する AppCompat を使用するには、を適合させる必要があります。 また、は`Toolbar`ロリポップより前のバージョンの Android では使用できないため、の`Toolbar`AppCompat バージョンを使用する必要があります。 したがって、レイアウトで`android.support.v7.widget.Toolbar`は、 `Toolbar`の代わりにを使用する必要があります。 

### <a name="update-layouts"></a>レイアウトの更新

**Resources/layout/Main**を編集し、 `Toolbar`要素を次の xml に置き換えます。 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

**Resources/layout/toolbar .xml**を編集し、その内容を次の xml に置き換えます。 

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

値の`?attr`前にプレフィックスが付け`android:`られていないこと`?`に注意してください (記法は、現在のテーマのリソースを参照していることを思い出してください)。 ここ`?android:attr`でがまだ使用されている場合、Android は、AppCompat ライブラリからではなく、現在実行中のプラットフォームから属性値を参照します。 この例では、 `actionBarSize` AppCompat ライブラリ`android:`によって定義されたを使用しているため、プレフィックスは削除されます。 同様に`@android:style` 、はに`@style`変更され`android:theme` 、属性が AppCompat `ThemeOverlay.Material.Dark.ActionBar`ライブラリ&ndash;のテーマ`ThemeOverlay.AppCompat.Dark.ActionBar`に設定されるようになりました。これは、ではなく、ここでテーマが使用されるようにするためです。 

### <a name="update-the-style"></a>スタイルを更新する

**Resources/values/styles .xml**を編集し、その内容を次の xml に置き換えます。 

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

この例の項目名と親テーマには、AppCompat ライブラリを`android:`使用しているため、プレフィックスが付けられなくなりました。 また、親テーマは、の`Light.DarkActionBar`AppCompat バージョンに変更されます。 

### <a name="update-menus"></a>メニューの更新

以前のバージョンの Android をサポートするために、AppCompat ライブラリは、 `android:`名前空間の属性を反映するカスタム属性を使用します。 ただし、 `showAsAction`一部の属性 ( `<menu>`タグで使用される属性など) は&ndash; `showAsAction` android api 11 で導入されましたが、android api 7 では使用できません。 このため、サポートライブラリによって定義されているすべての属性のプレフィックスとして、カスタム名前空間を使用する必要があります。 メニューリソースファイルでは、という名前`local`空間が属性の`showAsAction`プレフィックスとして定義されています。 

**Resources/menu/top_menus**を編集し、その内容を次の xml に置き換えます。

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

`local`名前空間が次の行に追加されます。

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

属性`showAsAction`は、ではなく`local:` 、この名前空間で始まります。`android:` 

```csharp
local:showAsAction="ifRoom"
```

同様に、 **Resources/menu/edit_menus**を編集し、その内容を次の xml に置き換えます。

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

API レベル11より前のバージョンの Android `showAsAction`では、この名前空間スイッチで属性をどのようにサポートしていますか? AppCompat NuGet を`showAsAction`インストールすると、カスタム属性と使用可能なすべての値がアプリに含まれます。 

## <a name="subclass-appcompatactivity"></a>サブクラス AppCompatActivity

変換の最後の手順は、の`MainActivity` `AppCompactActivity`サブクラスになるようにを変更することです。 **MainActivity.cs**を編集し、次`using`のステートメントを追加します。 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

これは`Toolbar` 、の`Toolbar`AppCompat バージョンであると宣言します。 次に、の`MainActivity`クラス定義を変更します。 

```csharp
public class MainActivity : AppCompatActivity
```

操作バーをの AppCompat バージョンに設定するに`Toolbar`は、の`SetActionBar`呼び出し`SetSupportActionBar`をに置き換えます。 この例では、の`Toolbar` AppCompat バージョンが使用されていることを示すために、タイトルも変更されています。

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

最後に、Android の最小レベルを、サポートされるロリポップの値 (たとえば、API 19) に変更します。 

アプリをビルドし、ロリポップ前のデバイスまたは Android エミュレーターで実行します。 次のスクリーンショットは、KitKat を実行している (API 19) を実行しているツール**バー**の AppCompat バージョンを示しています。 

[![KitKat デバイスで実行されているアプリの完全なスクリーンショット。両方のツールバーが表示されます。](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

Appcompat ライブラリが使用されている場合、android のバージョン&ndash;に基づいてテーマを切り替える必要はありません。 appcompat ライブラリにより、サポートされているすべての Android バージョンで一貫したユーザーエクスペリエンスを実現できます。 

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
