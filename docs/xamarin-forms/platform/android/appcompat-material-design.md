---
title: "AppCompat とマテリアル設計の追加" の説明: "この記事では Xamarin.Forms 、既存の Android アプリを変換して appcompat とマテリアル設計を使用する方法について説明します。"
ms. 製品: xamarin ms assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 06/27/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="adding-appcompat-and-material-design"></a>AppCompat とマテリアルデザインの追加

_次の手順に従って既存の Xamarin.Forms Android アプリを変換して、AppCompat とマテリアル設計を使用します。_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>概要

次の手順では、既存の android アプリケーションを更新して Xamarin.Forms AppCompat ライブラリを使用し、android バージョンのアプリで素材の設計を有効にする方法について説明し Xamarin.Forms ます。

### <a name="1-update-xamarinforms"></a>1. 更新Xamarin.Forms

ソリューションが2.0 以降を使用していることを確認し Xamarin.Forms ます。 を更新します。Xamarin.Forms
  必要に応じて、NuGet パッケージを2.0 にします。

### <a name="2-check-android-version"></a>2. Android のバージョンを確認する

Android プロジェクトのターゲットフレームワークが Android 6.0 (Marshmallow) であることを確認します。 **Android プロジェクト > オプション > ビルド > 全般設定]** をオンにして、corrent フレームワークが選択されていることを確認します。

 ![](appcompat-images/target-android-6-sml.png "Android General Build Configuration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. 新しいテーマを追加して、マテリアルデザインをサポートする

Android プロジェクトに次の3つのファイルを作成し、以下の内容を貼り付けます。 Google には、[スタイルガイド](https://www.google.com/design/spec/style/color.html#color-color-palette)と[カラーパレットジェネレーター](https://www.materialpalette.com/)が用意されています。このジェネレーターを使用すると、指定した配色に対して代替配色を選択できます。

**リソース/値/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**リソース/値/style.xml**

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

**リソース/値-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. AndroidManifest.xml を更新する

この新しいテーマ情報が使用されていることを確認するには、を追加して**Androidmanifest**ファイルのテーマを設定し `android:theme="@style/MyTheme"` ます (XML の残りの部分はそのままにします)。

**プロパティ/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. ツールバーとタブレイアウトを指定する

**Resources/layout**ディレクトリに**tabbar. Axml**ファイルと**Toolbar. axml**ファイルを作成し、以下の内容を貼り付けます。

**Resources/layout/Tabbar. axml**

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

タブのいくつかのプロパティが設定されています。これには、タブの [重力] `fill` と [モード] を含め `fixed` ます。
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

### <a name="6-update-the-mainactivity"></a>6.`MainActivity`

既存の Xamarin.Forms アプリでは、 **MainActivity.cs**クラスはを継承 `FormsApplicationActivity` します。 `FormsAppCompatActivity`新しい機能を有効にするには、これをに置き換える必要があります。

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

最後に、次に示すように、メソッドの手順 5. の新しいレイアウトを "接続" し `OnCreate` ます。

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
