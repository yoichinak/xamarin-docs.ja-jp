---
title: ツール バーの互換性
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 8d5f0b06a804e9b9af7bb15111c70e4505c79fd0
ms.sourcegitcommit: 40a56bbc1e038a9181101580ad18a4584edb5ab0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2021
ms.locfileid: "98025514"
---
# <a name="toolbar-compatibility"></a>ツール バーの互換性

## <a name="overview"></a>概要

このセクションでは、android `Toolbar` 5.0 ロリポップより前のバージョンの android でを使用する方法について説明します。 アプリが Android 5.0 より前のバージョンの Android をサポートしていない場合は、このセクションをスキップできます。 

`Toolbar`は android v7 サポートライブラリの一部であるため、android 2.1 (API レベル 7) 以降を実行しているデバイスで使用できます。 ただし、 [Android サポートライブラリ V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet をインストールし、 `Toolbar` このライブラリに用意されている実装を使用するようにコードを変更する必要があります。 このセクションでは、この NuGet をインストールする方法について説明します。 **Toolbarfun** アプリを変更し、 [2 つ目のツールバーを追加](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) して、ロリポップ5.0 より前のバージョンの Android で動作するようにします。

AppCompat バージョンのツールバーを使用するようにアプリを変更するには、次のようにします。 

1. アプリの Android の最小バージョンとターゲットを設定します。

2. AppCompat NuGet パッケージをインストールします。

3. 組み込みの Android テーマではなく、AppCompat テーマを使用します。

4. `MainActivity`をではなくサブクラスに変更 `AppCompatActivity` `Activity` します。 

これらの各手順の詳細については、次のセクションで説明します。

## <a name="set-the-minimum-and-target-android-version"></a>Android の最小バージョンとターゲットを設定する

アプリのターゲットフレームワークは、API レベル21以上に設定する必要があります。それ以外の場合、アプリは適切にデプロイされません。 アプリのデプロイ中に、 **パッケージ ' tileModeX ' の属性 ' ' にリソース識別子が見つからない** などのエラーが表示された場合、これは、ターゲットフレームワークが **ANDROID 5.0 (API レベル 21-ロリポップ)** 以上に設定されていないことが原因です。 

ターゲットフレームワークレベルを API レベル21以上に設定し、Android API レベルのプロジェクト設定を、アプリがサポートする最小の Android バージョンに設定します。 Android API レベルの設定の詳細については、「 [ANDROID Api レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。 この `ToolbarFun` 例では、Android の最小バージョンは KitKat (API レベル 4.4) に設定されています。 

## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet パッケージをインストールする

次に、 [Android Support Library V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) パッケージをプロジェクトに追加します。 Visual Studio で、[ **参照** ] を右クリックし、[ **NuGet パッケージの管理...**] を選択します。[ **参照** ] をクリックし、 **Android Support Library v7 AppCompat** を検索します。 **V7** を選択し、[**インストール**] をクリックします。 

[![[NuGet パッケージの管理」で選択した V7 Appcompat パッケージのスクリーンショット](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

この NuGet がインストールされている場合、他のいくつかの NuGet パッケージもまだ存在していなければインストールされます ( **xamarin. android**............................... **...** **. vector.** NuGet パッケージのインストールの詳細については、「 [チュートリアル: プロジェクトに nuget を含める](/visualstudio/mac/nuget-walkthrough)」を参照してください。 

## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat テーマとツールバーを使用する

AppCompat ライブラリには、 `Theme.AppCompat` appcompat ライブラリでサポートされているすべてのバージョンの Android で使用できるテーマがいくつか付属しています。 `ToolbarFun`サンプルアプリのテーマはから派生してい `Theme.Material.Light.DarkActionBar` ます。これは、ロリポップより前の Android バージョンでは使用できません。 したがって、 `ToolbarFun` このテーマに対応する AppCompat を使用するには、を適合させる必要があり `Theme.AppCompat.Light.DarkActionBar` ます。 また、 `Toolbar` はロリポップより前のバージョンの Android では使用できないため、の AppCompat バージョンを使用する必要があり `Toolbar` ます。 したがって、レイアウトでは、の代わりにを使用する必要があり `android.support.v7.widget.Toolbar` `Toolbar` ます。 

### <a name="update-layouts"></a>レイアウトの更新

**Resources/layout/Main** を編集し、 `Toolbar` 要素を次の xml に置き換えます。 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

**Resources/layout/toolbar.xml** を編集し、その内容を次の XML に置き換えます。 

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

値の前にプレフィックスが付けられていないことに注意して `?attr` `android:` ください (記法は、現在のテーマのリソースを参照していることを思い出して `?` ください)。 `?android:attr`ここでがまだ使用されている場合、Android は、AppCompat ライブラリからではなく、現在実行中のプラットフォームから属性値を参照します。 この例では、AppCompat ライブラリによって定義されたを使用しているため、 `actionBarSize` `android:` プレフィックスは削除されます。 同様に、 `@android:style` はに変更され、 `@style` 属性が AppCompat ライブラリのテーマに設定されるようになりました。これは、で `android:theme` &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` はなく、ここでテーマが使用されるよう `ThemeOverlay.Material.Dark.ActionBar` にするためです。 

### <a name="update-the-style"></a>スタイルを更新する

**リソース/値/styles.xml** を編集し、その内容を次の XML に置き換えます。 

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

この例の項目名と親テーマには、 `android:` AppCompat ライブラリを使用しているため、プレフィックスが付けられなくなりました。 また、親テーマは、の AppCompat バージョンに変更され `Light.DarkActionBar` ます。 

### <a name="update-menus"></a>メニューの更新

以前のバージョンの Android をサポートするために、AppCompat ライブラリは、名前空間の属性を反映するカスタム属性を使用し `android:` ます。 ただし、一部の属性 ( `showAsAction` タグで使用される属性など `<menu>` ) は android &ndash; `showAsAction` api 11 で導入されましたが、android api 7 では使用できません。 このため、サポートライブラリによって定義されているすべての属性のプレフィックスとして、カスタム名前空間を使用する必要があります。 メニューリソースファイルでは、という名前空間 `local` が属性のプレフィックスとして定義されてい `showAsAction` ます。 

**[リソース/メニュー/top_menus.xml** を編集し、その内容を次の XML に置き換えます。

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

`showAsAction`属性は、で `local:` はなく、この名前空間で始まります。`android:` 

```csharp
local:showAsAction="ifRoom"
```

同様に、[ **リソース/メニュー/edit_menus.xml** を編集し、その内容を次の XML に置き換えます。

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

`showAsAction`API レベル11より前のバージョンの Android では、この名前空間スイッチで属性をどのようにサポートしていますか? `showAsAction`AppCompat NuGet をインストールすると、カスタム属性と使用可能なすべての値がアプリに含まれます。 

## <a name="subclass-appcompatactivity"></a>サブクラス AppCompatActivity

変換の最後の手順は、のサブクラスになるようにを変更する `MainActivity` ことです `AppCompatActivity` 。 **MainActivity.cs** を編集し、次のステートメントを追加し `using` ます。 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

これ `Toolbar` は、の AppCompat バージョンであると宣言し `Toolbar` ます。 次に、のクラス定義を変更し `MainActivity` ます。 

```csharp
public class MainActivity : AppCompatActivity
```

操作バーをの AppCompat バージョンに設定するには、の呼び出しをに `Toolbar` 置き換え `SetActionBar` `SetSupportActionBar` ます。 この例では、の AppCompat バージョンが使用されていることを示すために、タイトルも変更されてい `Toolbar` ます。

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

最後に、Android の最小レベルを、サポートされるロリポップの値 (たとえば、API 19) に変更します。 

アプリをビルドし、ロリポップ前のデバイスまたは Android エミュレーターで実行します。 次のスクリーンショットは、KitKat を実行している (API 19) を実行しているツール **バー** の AppCompat バージョンを示しています。 

[![KitKat デバイスで実行されているアプリの完全なスクリーンショット。両方のツールバーが表示されます。](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

AppCompat ライブラリが使用されている場合、Android のバージョンに基づいてテーマを切り替える必要はありません &ndash; 。 appcompat ライブラリにより、サポートされているすべての Android バージョンで一貫したユーザーエクスペリエンスを実現できます。 

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
