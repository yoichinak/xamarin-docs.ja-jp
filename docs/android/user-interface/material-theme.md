---
title: マテリアル テーマ
description: テーマをする方法の素材のテーマに Xamarin.Android アプリ
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a3b5f908330833a38aad9e329835a4a437fc29f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771321"
---
# <a name="material-theme"></a>マテリアル テーマ

*材料テーマ*のビューと Android 5.0 (ロリポップ) で始まるアクティビティの外観を決定するユーザー インターフェイスのスタイルがします。 材料テーマは、およびアプリケーション、システム UI で使用するよう Android 5.0 に組み込まれています。 材料テーマは、ユーザーは [設定] メニューから動的に選択できるシステム全体の外観のオプションの意味で「テーマ」ではありません。 代わりに、マテリアル テーマ見なすことができますのアプリの外観をカスタマイズに使用できる関連する組み込みの基本スタイルのセットとして。

Android には、次の 3 つの素材テーマ フレーバーが用意されています。

-  `Theme.Material` &ndash; マテリアルのテーマの濃いバージョンこれは、Android 5.0 では既定のフレーバーです。

-  `Theme.Material.Light` &ndash; マテリアルのテーマの軽量バージョンです。

-  `Theme.Material.Light.DarkActionBar` &ndash; バーが濃いアクションが、マテリアルのテーマの軽量バージョンです。

マテリアルのテーマの種類の例についてはここに表示されます。

[![ダーク テーマ、ライト テーマと暗い操作バー テーマの例のスクリーン ショット](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

一部またはすべての色の属性をオーバーライドする独自のテーマを作成する資料テーマから派生できます。 派生するテーマを作成するなど、 `Theme.Material.Light`、組織のブランドの色を一致するようにアプリ バーの色をオーバーライドしますが、します。 個々 のビューのスタイルを設定することもできます。スタイルを作成するなど、 [CardView](~/android/user-interface/controls/card-view.md)角が丸いよりと暗い背景色を使用します。

1 つのテーマを使用するには、アプリ全体のまたはアプリ内でさまざまな画面 (アクティビティ) のさまざまなテーマを使用することができます。 上記のスクリーン ショットでなど、1 つのアプリを使用して各アクティビティの別のテーマ組み込み配色を示すためにします。 オプション ボタンは、異なる複数のアクティビティに、アプリを切り替えるし、その結果、さまざまなテーマを表示します。

マテリアル テーマがサポートされているは、Android 5.0 でのみと後ために使用できません (または資料テーマから派生したカスタムのテーマ) テーマにアプリ Android の以前のバージョンで実行されています。 ただし、Android 5.0 デバイスの素材のテーマを使用し、適切に代替以前のテーマを古いバージョンの Android での実行時にアプリを構成することができます (を参照してください、[互換性](#compatibility)詳細については、この記事のセクション)。


## <a name="requirements"></a>要件

Xamarin ベースのアプリでの Android 5.0 マテリアルのテーマの新機能を使用する、次が必要。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-  **Android SDK** &ndash; Android 5.0 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-  **Java JDK 1.8** &ndash; JDK 1.7 は、ターゲット API レベル具体的には 23 およびそれ以前の場合、使用できます。 JDK 1.8 はから利用可能な[Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)です。

Android 5.0 アプリ プロジェクトを構成する方法については、次を参照してください。[設定を Android 5.0 プロジェクト](~/android/platform/lollipop.md)です。


## <a name="using-the-built-in-themes"></a>組み込みのテーマの使用

マテリアルのテーマを使用する最も簡単な方法では、カスタマイズを行わなくても組み込みテーマを使用するアプリを構成します。 アプリは既定値に明示的にテーマを構成しない場合は、 `Theme.Material` (濃色のテーマ)。 アプリにのみ 1 つのアクティビティがある場合は、アプリケーション レベルのテーマを構成できます。 アプリに複数のアクティビティがある場合は、ように、すべての活動間で、同じテーマを使用したり、異なるアクティビティにさまざまなテーマを割り当てることができます、アプリケーション レベルでテーマを構成することができます。 次のセクションでは、アプリ レベルで、アクティビティ レベルでは、テーマを構成する方法を説明します。


### <a name="theming-an-application"></a>テーマのアプリケーション

マテリアルのテーマのフレーバーを使用するアプリケーション全体を構成する設定、 `android:theme` 、アプリケーション内のノードの属性**AndroidManifest.xml**次のいずれか。

-  `@android:style/Theme.Material` &ndash; ダーク テーマ。

-  `@android:style/Theme.Material.Light` &ndash; ライト テーマ。

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; 濃い操作バーで明るいテーマです。

次の例は、アプリケーションを構成*MyApp*明るい色のテーマを使用します。

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

アプリケーションを設定する代わりに、`Theme`属性**AssemblyInfo.cs** (または**Properties.cs**)。 例えば:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

アプリケーションのテーマに設定すると`@android:style/Theme.Material.Light`、すべての活動で*MyApp*を使用して表示されます`Theme.Material.Light`です。


### <a name="theming-an-activity"></a>アクティビティのテーマ

アクティビティのテーマを追加する、`Theme`に設定、`[Activity]`上のアクティビティ宣言属性を割り当てる`Theme`マテリアル テーマ フレーバーを使用することにします。 アクティビティで次の例テーマ`Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

このアプリで他のアクティビティが既定値を使用して`Theme.Material`暗い配色 (または、アプリケーションのテーマ設定を構成します。 場合)。

<a name="customtheme" />

## <a name="using-custom-themes"></a>カスタム テーマの使用

カスタム テーマ スタイル会社をブランド化を使用してアプリケーションを作成することで、ブランド化を強化できます&rsquo;の色。 カスタム テーマを作成するには、を定義する組み込みの資料テーマ フレーバーから派生する新しいスタイルを変更する色の属性をオーバーライドします。 派生するカスタム テーマを定義するなど、`Theme.Material.Light.DarkActionBar`と空白ではなくベージュを画面の背景色を変更します。

素材のテーマは、カスタマイズの場合は、次のレイアウト属性を公開します。

-  `colorPrimary` &ndash; アプリ バーの色。

-  `colorPrimaryDark` &ndash; ステータス バーおよびコンテキスト アプリ バーの色これは、濃色のバージョンのでは通常`colorPrimary`です。

-  `colorAccent` &ndash; チェック ボックス、ラジオ ボタン、および編集のテキスト ボックスなどの UI コントロールの色。

-  `windowBackground` &ndash; 画面の背景の色。

-  `textColorPrimary` &ndash; UI テキストに、アプリ バーの色。

-  `statusBarColor` &ndash; ステータス バーの色。

-  `navigationBarColor` &ndash; ナビゲーション バーの色。

これらの画面領域は、次の図にラベルが付けられます。

[![属性とその関連する画面領域の図](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

既定では、`statusBarColor`の値に設定されている`colorPrimaryDark`です。 設定することができます`statusBarColor`を純色設定することもできます`@android:color/transparent`ステータス バーを透明にします。 ナビゲーション バーも使用できる透過的設定`navigationBarColor`に`@android:color/transparent`です。


### <a name="creating-a-custom-app-theme"></a>カスタム アプリのテーマを作成します。

作成し、内のファイルを変更するカスタム アプリ テーマを作成することができます、**リソース**アプリ プロジェクトのフォルダーです。 ユーザー定義のテーマでのアプリのスタイルを設定するには、次の手順を使用します。

-   作成、 **colors.xml**ファイル**リソース/値**&mdash;このファイルを使用して、カスタム テーマの色を定義します。 たとえばには、次のコードを貼り付けることができます**colors.xml**作業を開始するため。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   名前と、カスタム テーマで使用する色リソースのカラー コードを定義するには、このサンプル ファイルを変更します。

-   作成、**リソース/値の v21**フォルダーです。 このフォルダーに作成、 **styles.xml**ファイル。

    [![Styles.xml リソース/値-21.xml フォルダー内の場所](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    注意してください**リソース/値の v21**は Android 5.0 に固有の&ndash;Android の古いバージョンがこのフォルダー内のファイルを読み取れません。

-   追加、`resources`ノード**styles.xml**を定義して、`style`カスタム テーマの名前を持つノード。 たとえば、ここでは、 **styles.xml**を定義するファイル*MyCustomTheme* (組み込みから派生した`Theme.Material.Light`テーマ スタイル)。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   この時点で、アプリは*MyCustomTheme* 、株式が表示されます`Theme.Material.Light`テーマのカスタマイズなし。

    [![カスタム テーマの外観のカスタマイズの前に](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   色のカスタマイズを追加**styles.xml**を変更するレイアウト属性の色を定義することで。 たとえば、アプリ バーの色を変更する`my_blue`を UI コントロールの色を変更したり`my_purple`に色のオーバーライドを追加**styles.xml**で構成されている色リソースを参照する**colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

これらの場所の変更のアプリを使用*MyCustomTheme*でアプリ バーの色が表示されます`my_blue`と UI コントロールに`my_purple`が使用して、`Theme.Material.Light`配色それ以外の場所。

[![カスタム テーマの外観のカスタマイズした後](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

この例では*MyCustomTheme*から色をそのまま利用`Theme.Material.Light`の背景色、ステータス バー、およびテキストの色がの色を変更する、アプリ バー `my_blue` するラジオボタンの色を設定および`my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>カスタム ビューのスタイルを作成します。

Android 5.0 では、個々 のビューのスタイルを設定することです。 作成した後**colors.xml**と**styles.xml**にビューのスタイルを追加する (として、前のセクションで説明)、 **styles.xml**です。
個々 のビューのスタイルを設定するには、次の手順を使用します。

-   編集**Resources/values-v21/styles.xml**を追加し、`style`カスタム ビューのスタイルの名前を持つノード。 このビューのカスタム色の属性を設定`style`ノード。 例については、カスタムを作成する[CardView](~/android/user-interface/controls/card-view.md)スタイルを使用して角が丸いより`my_blue`カードの背景色として追加して、`style`ノード**styles.xml** (、内`resources`ノード) と背景色と上隅にある radius の構成します。

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   レイアウトで、設定、`style`前の手順で選択したカスタム スタイル名と一致するには、そのビューの属性です。 例えば:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

次のスクリーン ショットは、既定値の例を示します`CardView`(表示される、左側) と比較して、`CardView`カスタム スタイルが設定されている`CardView.MyBlue`テーマ (右側に表示)。

[![既定 CardView とカスタム CardView の例](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

この例では、カスタム`CardView`背景色で表示される`my_blue`と 18dp 角の半径。


## <a name="compatibility"></a>互換性

スタイルをアプリが Android 5.0 でマテリアルのテーマを使用しますが、下降傾向と互換性のあるスタイルの Android の以前のバージョンに自動的に元に戻しますするには、次の手順を使用します。

-   カスタム テーマを定義**Resources/values-v21/styles.xml**マテリアル テーマ スタイルから派生しました。 例えば:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   カスタム テーマを定義**Resources/values/styles.xml**を古いテーマから派生したが、上と同じテーマ名を使用します。 例えば:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   **AndroidManifest.xml**、カスタム テーマ名を使ってアプリを構成します。 
    例えば:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   代わりに、カスタム テーマを使用して、特定のアクティビティのスタイルを設定することができます。

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

テーマがで定義された色を使用している場合、 **colors.xml**ファイルを使用してこのファイルを配置することを確認して**リソース/値**(なく**リソース/値の v21**) ようにの両方のバージョンカスタム テーマの色の定義にアクセスできます。

指定されたテーマの定義が使用されます Android 5.0 デバイスでアプリを実行すると**Resources/values-v21/styles.xml**です。 このアプリは、以前の Android デバイスで実行しているときに自動的に戻されますで指定されたテーマの定義に**Resources/values/styles.xml**です。

古いバージョンの Android との互換性をテーマに関する詳細については、次を参照してください。[代替リソース](~/android/app-fundamentals/resources-in-android/alternate-resources.md)です。

## <a name="summary"></a>まとめ

この記事では、新しい素材テーマ スタイルのユーザー インターフェイス Android 5.0 (ロリポップ) に含まれるが導入されました。 アプリのスタイル設定に使用できる 3 つの組み込みマテリアル テーマ フレーバーが説明されているのブランドに関し、アプリでは、カスタム テーマを作成する方法を説明およびテーマをする方法の例を個々 のビューに提供します。 最後に、この記事では、Android の古いバージョンとの下位互換性を維持しながら、アプリでマテリアルのテーマを使用する方法について説明します。



## <a name="related-links"></a>関連リンク

- [ThemeSwitcher (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [代替リソース](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [素材のデザイン](http://developer.android.com/preview/material/index.html)
- [素材のデザインの原則](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [互換性の維持](http://developer.android.com/preview/material/compatibility.html)
