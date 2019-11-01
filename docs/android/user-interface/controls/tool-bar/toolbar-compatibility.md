---
title: ツール バーの互換性
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 809dc8ec8fd1106b8ad8631c0c506067abdf0d97
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029072"
---
# <a name="toolbar-compatibility"></a>ツール バーの互換性

## <a name="overview"></a>概要

このセクションでは、android 5.0 ロリポップより前のバージョンの Android で `Toolbar` を使用する方法について説明します。 アプリが Android 5.0 より前のバージョンの Android をサポートしていない場合は、このセクションをスキップできます。 

`Toolbar` は Android v7 サポートライブラリの一部であるため、Android 2.1 (API レベル 7) 以降を実行しているデバイスで使用できます。 ただし、 [Android サポートライブラリ V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet をインストールし、このライブラリに用意されている `Toolbar` の実装を使用するようにコードを変更する必要があります。 このセクションでは、この NuGet をインストールする方法について説明します。 **Toolbarfun**アプリを変更し、 [2 つ目のツールバーを追加](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md)して、ロリポップ5.0 より前のバージョンの Android で動作するようにします。

AppCompat バージョンのツールバーを使用するようにアプリを変更するには、次のようにします。 

1. アプリの Android の最小バージョンとターゲットを設定します。

2. AppCompat NuGet パッケージをインストールします。

3. 組み込みの Android テーマではなく、AppCompat テーマを使用します。

4. `Activity`ではなく `AppCompatActivity` サブクラスになるように `MainActivity` を変更します。 

これらの各手順の詳細については、次のセクションで説明します。

## <a name="set-the-minimum-and-target-android-version"></a>Android の最小バージョンとターゲットを設定する

アプリのターゲットフレームワークは、API レベル21以上に設定する必要があります。それ以外の場合、アプリは適切にデプロイされません。 アプリのデプロイ中に、**パッケージ ' tileModeX ' の属性 ' ' にリソース識別子が見つからない**などのエラーが表示された場合、これは、ターゲットフレームワークが**ANDROID 5.0 (API レベル 21-ロリポップ)** 以上に設定されていないことが原因です。 

ターゲットフレームワークレベルを API レベル21以上に設定し、Android API レベルのプロジェクト設定を、アプリがサポートする最小の Android バージョンに設定します。 Android API レベルの設定の詳細については、「 [ANDROID Api レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください。 `ToolbarFun` の例では、Android の最小バージョンは KitKat (API レベル 4.4) に設定されています。 

## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet パッケージをインストールする

次に、 [Android Support Library V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)パッケージをプロジェクトに追加します。 Visual Studio で、 **[参照]** を右クリックし、 **[NuGet パッケージの管理...]** を選択します。 **[参照]** をクリックし、 **Android Support Library v7 AppCompat**を検索します。 **V7**を選択し、 **[インストール]** をクリックします。 

[[NuGet パッケージの管理」で選択した V7 Appcompat パッケージのスクリーンショット![](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

この NuGet がインストールされている場合は、他のいくつかの NuGet パッケージがまだ存在していない**場合にも**インストールされます ( **xamarin** **.....................................Xamarin. Android**... Vector. NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください。 

## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat テーマとツールバーを使用する

AppCompat ライブラリには、AppCompat ライブラリでサポートされているすべてのバージョンの Android で使用できる `Theme.AppCompat` テーマがいくつか付属しています。 `ToolbarFun` サンプルアプリのテーマは `Theme.Material.Light.DarkActionBar`から派生しています。これは、ロリポップより前の Android バージョンでは使用できません。 したがって、このテーマに対応する AppCompat を使用するには、`Theme.AppCompat.Light.DarkActionBar`を `ToolbarFun` する必要があります。 また、`Toolbar` はロリポップより前のバージョンの Android では使用できないため、`Toolbar`の AppCompat バージョンを使用する必要があります。 したがって、レイアウトでは `Toolbar`ではなく `android.support.v7.widget.Toolbar` を使用する必要があります。 

### <a name="update-layouts"></a>レイアウトの更新

**Resources/layout/Main**を編集し、`Toolbar` 要素を次の xml に置き換えます。 

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

`?attr` 値には、`android:` の前にプレフィックスが付けられていないことに注意してください (`?` 表記は、現在のテーマのリソースを参照していることを思い出してください)。 ここで `?android:attr` がまだ使用されている場合、Android は、AppCompat ライブラリからではなく、現在実行中のプラットフォームから属性値を参照します。 この例では、AppCompat ライブラリによって定義された `actionBarSize` を使用するため、`android:` プレフィックスが削除されます。 同様に、`@android:style` が `@style` に変更されます。これにより、`android:theme` 属性が AppCompat ライブラリのテーマに設定され &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` ではなく `ThemeOverlay.Material.Dark.ActionBar`テーマが使用されるようになります。 

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

この例の項目名と親テーマには、AppCompat ライブラリを使用しているため `android:` というプレフィックスが付けられなくなりました。 また、親テーマは、`Light.DarkActionBar`の AppCompat バージョンに変更されます。 

### <a name="update-menus"></a>メニューの更新

以前のバージョンの Android をサポートするために、AppCompat ライブラリは `android:` 名前空間の属性を反映するカスタム属性を使用します。 ただし、一部の属性 (`<menu>` タグで使用される `showAsAction` 属性など) は、android API 11 で導入されたものの、android api 7 では使用できない &ndash; `showAsAction` の Android framework には存在しません。 このため、サポートライブラリによって定義されているすべての属性のプレフィックスとして、カスタム名前空間を使用する必要があります。 メニューリソースファイルでは、`showAsAction` 属性の前にプレフィックスとして `local` という名前空間が定義されています。 

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

次の行を使用して、`local` 名前空間を追加します。

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` 属性は、ではなく、この `local:` 名前空間で始まり `android:` 

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

API レベル11より前のバージョンの Android では、この名前空間スイッチで `showAsAction` 属性をどのようにサポートしていますか? AppCompat NuGet をインストールすると、カスタム属性 `showAsAction` と使用可能なすべての値がアプリに含まれます。 

## <a name="subclass-appcompatactivity"></a>サブクラス AppCompatActivity

変換の最後の手順は、`AppCompactActivity`のサブクラスになるように `MainActivity` を変更することです。 **MainActivity.cs**を編集し、次の `using` ステートメントを追加します。 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

これにより、`Toolbar`の AppCompat バージョンとして `Toolbar` が宣言されます。 次に、`MainActivity`のクラス定義を変更します。 

```csharp
public class MainActivity : AppCompatActivity
```

操作バーを `Toolbar`の AppCompat バージョンに設定するには、`SetActionBar` の呼び出しを `SetSupportActionBar`に置き換えます。 この例では、`Toolbar` の AppCompat バージョンが使用されていることを示すために、タイトルも変更されています。

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

最後に、Android の最小レベルを、サポートされるロリポップの値 (たとえば、API 19) に変更します。 

アプリをビルドし、ロリポップ前のデバイスまたは Android エミュレーターで実行します。 次のスクリーンショットは、KitKat を実行している (API 19) を実行しているツール**バー**の AppCompat バージョンを示しています。 

[KitKat デバイスで実行されているアプリの完全なスクリーンショット![、両方のツールバーが表示されます。](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

AppCompat ライブラリを使用する場合は、Android のバージョンに基づいてテーマを切り替える必要はありません &ndash;、AppCompat ライブラリにより、サポートされているすべての Android バージョンで一貫したユーザーエクスペリエンスを提供できるようになります。 

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
