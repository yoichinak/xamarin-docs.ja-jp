---
title: AppCompat とマテリアルデザインの追加
description: この記事では、既存の Xamarin Android アプリを変換して、AppCompat とマテリアル設計を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: 36c5733c347e3493b5ed423c52766c7e33fbdb3d
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728331"
---
# <a name="adding-appcompat-and-material-design"></a>AppCompat とマテリアルデザインの追加

_次の手順に従って、既存の Xamarin. Forms Android アプリを変換して、AppCompat とマテリアル設計を使用する_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>の概要

次の手順では、既存の Xamarin. Forms Android アプリケーションを更新して AppCompat ライブラリを使用し、Android バージョンの Xamarin. Forms アプリで素材の設計を有効にする方法について説明します。

### <a name="1-update-xamarinforms"></a>1. Xamarin. フォームを更新する

ソリューションが Xamarin. Forms 2.0 以降を使用していることを確認します。 必要に応じて、Xamarin. Forms NuGet パッケージを2.0 に更新します。

### <a name="2-check-android-version"></a>2. Android のバージョンを確認する

Android プロジェクトのターゲットフレームワークが Android 6.0 (Marshmallow) であることを確認します。 **Android プロジェクト > オプション > ビルド > 全般設定**をオンにして、corrent フレームワークが選択されていることを確認します。

 ![](appcompat-images/target-android-6-sml.png "Android General Build Configuration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. 新しいテーマを追加して、マテリアルデザインをサポートする

Android プロジェクトに次の3つのファイルを作成し、以下の内容を貼り付けます。 Google には、[スタイルガイド](https://www.google.com/design/spec/style/color.html#color-color-palette)と[カラーパレットジェネレーター](https://www.materialpalette.com/)が用意されています。このジェネレーターを使用すると、指定した配色に対して代替配色を選択できます。

**Resources/values/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Android ロリポップ以降で実行するときに特定のプロパティを適用するには、 **v21**フォルダーに追加のスタイルを含める必要があります。

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Update AndroidManifest .xml

この新しいテーマ情報が使用されるようにするには、`android:theme="@style/MyTheme"` を追加して**Androidmanifest**ファイルのテーマを設定します (XML の残りの部分はそのままにします)。

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. ツールバーとタブレイアウトを指定する

**Resources/layout**ディレクトリに**tabbar. Axml**ファイルと**Toolbar. axml**ファイルを作成し、以下の内容を貼り付けます。

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

タブのいくつかのプロパティが設定されています。これには、`fill` に対するタブの重心、`fixed`のモードが含まれます。
多数のタブがある場合、詳細については、Android [TabLayout のドキュメント](https://developer.android.com/reference/android/support/design/widget/TabLayout.html)を参照してください。

**Resources/layout/Toolbar. axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

これらのファイルでは、アプリケーションによって異なる可能性があるツールバーの特定のテーマを作成しています。
詳細については、 [Hello ツールバー](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/)のブログ記事を参照してください。

### <a name="6-update-the-mainactivity"></a>6. `MainActivity` を更新する

既存の Xamarin. Forms アプリでは、 **MainActivity.cs**クラスは `FormsApplicationActivity`から継承します。 新しい機能を有効にするには、これを `FormsAppCompatActivity` に置き換える必要があります。

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

最後に、次に示すように、`OnCreate` メソッドの手順 5. の新しいレイアウトを "接続" します。

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
