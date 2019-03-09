---
title: マテリアル テーマ
description: テーマ方法素材のテーマで Xamarin.Android アプリ
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: ff94211086956e36da377445d90359789b62fc60
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668323"
---
# <a name="material-theme"></a>マテリアル テーマ

*マテリアル テーマ*のビューと Android 5.0 (ロリポップ) で始まるアクティビティの外観を決定するユーザー インターフェイスのスタイルがします。 マテリアル テーマは、およびアプリケーション、システム UI で使用するよう Android 5.0 に組み込まれています。 マテリアル テーマは、ユーザーは [設定] メニューから動的に選択できるシステム全体の外観のオプションの意味で「テーマ」ではありません。 代わりに、マテリアル テーマ見なすことができますのアプリの外観をカスタマイズに使用できる関連する組み込みの基本スタイルのセットとして。


Android では、次の 3 つの素材のテーマのフレーバーが用意されています。

-  `Theme.Material` &ndash; 素材のテーマの濃いバージョンこれは、Android 5.0 既定フレーバーです。

-  `Theme.Material.Light` &ndash; 素材のテーマの軽量バージョンです。

-  `Theme.Material.Light.DarkActionBar` &ndash; 濃いアクション バーで、素材のテーマの軽量バージョンです。

素材のテーマの種類の例については、ここに表示されます。

[![ダーク テーマ、ライト テーマとダーク操作バーのテーマの例のスクリーン ショット](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

一部またはすべての色属性をオーバーライドする独自のテーマを作成する素材のテーマから派生できます。 派生したテーマを作成するなど、 `Theme.Material.Light`、ブランドの色を一致するように、アプリ バーの色を上書きします。 個々 のビューのスタイルを設定することもできます。スタイルを作成するなど、 [CardView](~/android/user-interface/controls/card-view.md)を角が丸いよりを暗い背景色を使用しています。

1 つのテーマを使用するには、アプリ全体のまたはアプリ内で別の画面 (アクティビティ) のさまざまなテーマを使用することができます。 上記のスクリーン ショットでは、たとえば、1 つのアプリを使用して各アクティビティの別のテーマを組み込みの色スキームを示すために。 オプション ボタンは、別のアクティビティに、アプリを切り替えるし、その結果、さまざまなテーマを表示します。

素材のテーマには、Android 5.0 でのみ以降がサポートされていますが、ために使用できません (またはマテリアルのテーマから派生したカスタム テーマ) テーマにアプリ以前のバージョンの Android を実行するため。 ただし、Android 5.0 デバイスで素材のテーマを使用し、適切に劣る以前のテーマを以前のバージョンの Android での実行時にアプリを構成することができます (を参照してください、[互換性](#compatibility)詳細については、この記事のセクション)。


## <a name="requirements"></a>要件

Xamarin ベースのアプリで新しい Android 5.0 素材のテーマ機能を使用する、次が必要。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-  **Android SDK** &ndash; Android 5.0 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-  **Java JDK 1.8** &ndash;ターゲット API レベル具体的には 23 およびそれ以前の場合、JDK 1.7 を使用できます。 JDK 1.8 は[Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)します。

Android 5.0 アプリのプロジェクトを構成する方法についてを参照してください。[設定を、Android 5.0 プロジェクト](~/android/platform/lollipop.md)します。


## <a name="using-the-built-in-themes"></a>組み込みのテーマの使用

素材のテーマを使用する最も簡単な方法では、組み込みのテーマのカスタマイズを使用するようアプリを構成します。 テーマを明示的に構成しない場合、アプリは既定`Theme.Material`(濃色のテーマ)。 アプリに 1 つだけのアクティビティがある場合は、アプリケーション レベルでテーマを構成できます。 アプリに複数のアクティビティがある場合は、すべてのアクティビティ間で同じテーマを使用したり、さまざまなテーマを別のアクティビティに割り当てることができます、アプリケーション レベルでテーマを構成できます。 次のセクションでは、アプリ レベルで、アクティビティ レベルでは、テーマを構成する方法を説明します。


### <a name="theming-an-application"></a>アプリケーションのテーマ

素材のテーマのフレーバーを使用するアプリケーション全体を構成する設定、`android:theme`アプリケーション ノードの属性**AndroidManifest.xml**次のいずれか。

-  `@android:style/Theme.Material` &ndash; ダーク テーマ。

-  `@android:style/Theme.Material.Light` &ndash; ライト テーマ。

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; 濃いアクション バーで明るいテーマです。

次の例は、アプリケーションを構成します。 *MyApp*明るい色のテーマを使用します。

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

アプリケーションを設定する代わりに、`Theme`属性**AssemblyInfo.cs** (または**Properties.cs**)。 例えば:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

アプリケーションのテーマに設定すると`@android:style/Theme.Material.Light`、すべてのアクティビティで*MyApp*を使用して表示されます`Theme.Material.Light`します。


### <a name="theming-an-activity"></a>アクティビティのテーマ

アクティビティのテーマに追加する、`Theme`に設定、 `[Activity]` 、アクティビティの宣言の上の属性し、割り当てる`Theme`素材のテーマのフレーバーを使用するにします。 アクティビティで次の例のテーマ`Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

このアプリの他のアクティビティが既定値を使用して`Theme.Material`暗めの色彩 (または、アプリケーションのテーマ設定を構成します。 場合)。

<a name="customtheme" />

## <a name="using-custom-themes"></a>カスタム テーマの使用

ブランドを強化するには、アプリをお客様のブランドをスタイル設定をカスタム テーマを作成して&rsquo;s の色。 カスタム テーマを作成するには、スタイルを定義する新しい組み込みの素材のテーマ フレーバーから派生したを変更する必要がある色属性をオーバーライドします。 派生したカスタム テーマを定義するなど、`Theme.Material.Light.DarkActionBar`し、画面の背景色を白ではなくベージュに変更します。

素材のテーマは、カスタマイズは、次のレイアウト属性を公開します。

-  `colorPrimary` &ndash; アプリ バーの色。

-  `colorPrimaryDark` &ndash; ステータス バーとコンテキスト アプリ バーの色これは、通常の濃いバージョン`colorPrimary`します。

-  `colorAccent` &ndash; チェック ボックス、ラジオ ボタン、および編集テキスト ボックスなどの UI コントロールの色。

-  `windowBackground` &ndash; 画面の背景の色。

-  `textColorPrimary` &ndash; アプリ バーでの UI テキストの色。

-  `statusBarColor` &ndash; ステータス バーの色。

-  `navigationBarColor` &ndash; ナビゲーション バーの色。

次の図では、これらの画面領域がラベル付け。

[![属性とその関連する画面領域のダイアグラム](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

既定では、`statusBarColor`の値に設定されている`colorPrimaryDark`します。 設定することができます`statusBarColor`を純色設定することも`@android:color/transparent`ステータス バーを透明にします。 ナビゲーション バーも透明にする設定によって`navigationBarColor`に`@android:color/transparent`します。


### <a name="creating-a-custom-app-theme"></a>カスタム アプリのテーマを作成します。

カスタム アプリのテーマを作成するには作成し、内のファイルを変更すること、**リソース**アプリ プロジェクトのフォルダー。 カスタム テーマを使用してアプリのスタイルを設定するには、次の手順を使用します。

-   作成、 **colors.xml**ファイル**リソース/値**&mdash;このファイルを使用して、カスタム テーマの色を定義します。 たとえばには、次のコードを貼り付けることができます**colors.xml**を参照してください。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   この例で、名前と、カスタム テーマで使用する色リソースのカラー コードを定義するファイルを変更します。

-   作成、**リソース/値-v21**フォルダー。 このフォルダーに作成、 **styles.xml**ファイル。

    [![Styles.xml 21.xml 値-リソース/フォルダーの場所](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    なお**v21 値-リソース/** Android 5.0 に固有&ndash;以前のバージョンの Android はこのフォルダー内のファイルを読み取れません。

-   追加、`resources`ノード**styles.xml**を定義し、`style`カスタム テーマの名前を持つノード。 たとえば、ここでは、 **styles.xml**を定義するファイル*MyCustomTheme* (組み込みから派生した`Theme.Material.Light`テーマ スタイル)。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   この時点では、使用するアプリ*MyCustomTheme*素材が表示されます`Theme.Material.Light`カスタマイズないテーマ。

    [![カスタム テーマの外観のカスタマイズの前に](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   色のカスタマイズを追加**styles.xml**を変更するレイアウト属性の色を定義します。 たとえば、アプリ バーの色を変更する`my_blue`を UI コントロールの色を変更したり`my_purple`、色は上書きに追加**styles.xml**で構成されている色リソースを参照する**colors.xml**:

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

これらの場所の変更、アプリを使用する*MyCustomTheme*で、アプリ バーの色が表示されます`my_blue`で UI コントロールと`my_purple`が使用、`Theme.Material.Light`配色それ以外の場所。

[![カスタム テーマの外観のカスタマイズを完了後](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

この例で*MyCustomTheme*から色を借用`Theme.Material.Light`の背景色、ステータス バー、およびテキストの色がその変更をアプリ バーの色`my_blue`するラジオボタンの色を設定および`my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>カスタム ビューのスタイルを作成します。

Android 5.0 では、個々 のビューのスタイルを設定することが可能です。 作成した後**colors.xml**と**styles.xml**にビューのスタイルを追加する (として、前のセクションで説明)、 **styles.xml**します。
個々 のビューのスタイルを設定するには、次の手順を使用します。

-   編集**Resources/values-v21/styles.xml**を追加し、`style`ノード、カスタム ビューのスタイルの名前に置き換えます。 このビューのカスタムの色属性を設定`style`ノード。 たとえば、カスタムを作成する[CardView](~/android/user-interface/controls/card-view.md)スタイルを使用して、角が丸いより`my_blue`カードの背景色として、追加、`style`ノードを**styles.xml** (、内`resources`ノード) と背景色と上隅にある radius の構成します。

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   レイアウトでは、設定、`style`前の手順で選択したカスタム スタイル名と一致するには、そのビューの属性。 例えば:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

次のスクリーン ショットは、既定値の例を示します`CardView`(に示すように、左上) を比較すると、`CardView`カスタム スタイルが設定されている`CardView.MyBlue`テーマ (右側に表示されます)。

[![CardView を既定とカスタム CardView の例](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

この例では、カスタムの`CardView`の背景色で表示される`my_blue`と 18dp 角の半径。


## <a name="compatibility"></a>互換性

アプリのスタイルを設定できるように、Android 5.0 の素材のテーマを使用して、自動的に古い Android バージョンと互換性のある下向きのスタイルに戻りますが、するには、次の手順を使用します。

-   カスタム テーマを定義**Resources/values-v21/styles.xml**素材のテーマ スタイルから派生しました。 例えば:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   カスタム テーマを定義**Resources/values/styles.xml**古いテーマから派生したが、上記と同じテーマ名を使用します。 例えば:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   **AndroidManifest.xml**、カスタム テーマの名前でアプリを構成します。 
    例えば:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   または、カスタム テーマを使用して、特定のアクティビティのスタイルを設定することができます。

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

テーマで定義された色を使用している場合、 **colors.xml**ファイルで、このファイルに配置することを確認する**リソース/値**(なく**リソース/値-v21**) ようにの両方のバージョンカスタム テーマの色の定義にアクセスできます。

Android 5.0 デバイスでアプリを実行するで指定されたテーマの定義が使用されます**Resources/values-v21/styles.xml**します。 このアプリを以前の Android デバイスで実行すると自動的にフォールバックで指定されたテーマの定義に**Resources/values/styles.xml**します。

古い Android バージョンとの互換性をテーマの詳細については、次を参照してください。[代替リ ソース](~/android/app-fundamentals/resources-in-android/alternate-resources.md)します。

## <a name="summary"></a>まとめ

この記事では、Android 5.0 (Lollipop) に含まれる新しい素材のテーマのユーザー インターフェイス スタイルが導入されました。 アプリのスタイル設定に使用できる 3 つの組み込みマテリアル テーマ フレーバーが説明されているアプリでは、ブランド化のカスタム テーマを作成する方法について説明し、方法の例のテーマに個別のビューを提供します。 最後に、この記事では、以前のバージョンの Android との下位互換性を維持しながら、アプリで素材のテーマを使用する方法について説明します。



## <a name="related-links"></a>関連リンク

- [ThemeSwitcher (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [ロリポップの概要](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [代替リソース](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android Lollipop](https://developer.android.com/about/versions/lollipop)
- [Android の円の開発者](https://developer.android.com/about/versions/pie/)
- [素材のデザイン](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [素材のデザインの原則](https://material.io/design/)
- [互換性の維持](https://developer.android.com/training/backward-compatible-ui/)
