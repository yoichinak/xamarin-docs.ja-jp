---
title: AppCompat と素材のデザインを追加します。
description: この記事では、AppCompat と素材のデザインを使用する既存の Xamarin.Forms の Android アプリに変換する方法について説明します。
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: cade72aaad60c30993f6b11e98704addd218ffae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293445"
---
# <a name="adding-appcompat-and-material-design"></a>AppCompat と素材のデザインを追加します。

_AppCompat と素材のデザインを使用する既存の Xamarin.Forms の Android アプリに変換する次の手順します。_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>概要

これらの手順では、AppCompat ライブラリを使用し、Xamarin.Forms アプリの Android バージョン マテリアル デザインを有効にする既存の Xamarin.Forms の Android アプリケーションを更新する方法について説明します。

### <a name="1-update-xamarinforms"></a>1.Xamarin.Forms を更新します。

ソリューションが 2.0 以降、Xamarin.Forms を使用することを確認します。 必要な場合は、2.0 に Xamarin.Forms Nuget パッケージを更新します。

### <a name="2-check-android-version"></a>2.Android のバージョンを確認してください。

Android プロジェクトのターゲット フレームワークが Android 6.0 (Marshmallow) であることを確認します。 チェック、 **Android プロジェクト > オプション > ビルド > 全般**正しい framework の設定を選択。

 ![](appcompat-images/target-android-6-sml.png "Android の一般的なビルド構成")

### <a name="3-add-new-themes-to-support-material-design"></a>3.マテリアル デザインをサポートするために新しいテーマを追加します。

Android プロジェクトに次の 3 つのファイルを作成し、以下の内容を貼り付けます。 Google が提供、[スタイル ガイド](http://www.google.com/design/spec/style/color.html#color-color-palette)と[色パレット ジェネレーター](http://www.materialpalette.com/)に指定されている別の配色を選択するのに役立ちます。

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

追加のスタイルを含める必要がある、**値 v21** Android Lollipop 以降を実行しているときに、特定のプロパティを適用するフォルダー。

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

### <a name="4-update-androidmanifestxml"></a>4.AndroidManifest.xml を更新します。

情報がで使用されている設定のテーマをこの新しいテーマを使用して、 **AndroidManifest**ファイルを追加して`android:theme="@style/MyTheme"`(であったため、XML の残りの部分のまま)。

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5.ツールバーとタブのレイアウトを提供します。

作成**Tabbar.axml**と**Toolbar.axml**内のファイル、**リソース/レイアウト**ディレクトリとそのコンテンツからの下に貼り付けます。

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

タブの重力を含むタブについて、いくつかのプロパティが設定されて`fill`とするモード`fixed`します。
多くのタブがある場合は、これを切り替えることがあるスクロール可能な - を通して、Android [TabLayout ドキュメント](https://developer.android.com/reference/android/support/design/widget/TabLayout.html)詳細。

**Resources/layout/Toolbar.axml**

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

これらのファイルでは、アプリケーションの異なる可能性があるツールバーの特定のテーマを作成しています。
参照してください、[こんにちはツールバー](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/)の詳細はブログの投稿。


### <a name="6-update-the-mainactivity"></a>6.更新プログラム、 `MainActivity`

既存の Xamarin.Forms アプリで、 **MainActivity.cs**クラスが継承`FormsApplicationActivity`します。 これで置き換える必要がある`FormsAppCompatActivity`新しい機能が有効にします。

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

最後に、「接続」の手順 5 で、新しいレイアウト、`OnCreate`メソッドを次に示します。

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
