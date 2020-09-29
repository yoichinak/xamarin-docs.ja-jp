---
title: Material Theme
description: マテリアルテーマを使用して Xamarin Android アプリをテーマする方法
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 22549bc7410b62e04cdb34c5c102a2fed18b0cf9
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457095"
---
# <a name="material-theme"></a>Material Theme

*マテリアルテーマ* は、Android 5.0 (ロリポップ) 以降のビューとアクティビティのルックアンドフィールを決定するユーザーインターフェイススタイルです。 マテリアルのテーマは Android 5.0 に組み込まれているため、システム UI やアプリケーションによって使用されます。 マテリアルテーマは、ユーザーが設定メニューから動的に選択できるシステム全体の外観オプションを意味する "テーマ" ではありません。 代わりに、素材のテーマは、アプリのルックアンドフィールをカスタマイズするために使用できる、関連する組み込み基本スタイルのセットと考えることができます。

Android には、次の3つの素材テーマがあります。

- `Theme.Material`&ndash;マテリアルテーマのダークバージョン。これは Android 5.0 の既定のフレーバーです。

- `Theme.Material.Light`&ndash;マテリアルテーマのライトバージョン。

- `Theme.Material.Light.DarkActionBar`&ndash;マテリアルテーマの明るいバージョンですが、ダークアクションバーがあります。

これらの配色テーマの例の例を次に示します。

[![ダークテーマ、明るいテーマ、およびダーク操作バーのテーマのスクリーンショットの例](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

マテリアルテーマから派生させて独自のテーマを作成し、一部またはすべての色属性をオーバーライドすることができます。 たとえば、から派生したテーマを作成でき `Theme.Material.Light` ますが、ブランドの色と一致するようにアプリバーの色をオーバーライドします。 個々のビューのスタイルを作成することもできます。たとえば、角が丸く、暗い背景色を使用する [CardView](~/android/user-interface/controls/card-view.md) のスタイルを作成できます。

アプリ全体に対して1つのテーマを使用することも、アプリ内のさまざまな画面 (アクティビティ) に対して異なるテーマを使用することもできます。 上のスクリーンショットでは、たとえば、1つのアプリが各アクティビティに対して異なるテーマを使用して、組み込みの配色を示しています。 オプションボタンを利用すると、アプリがさまざまなアクティビティに切り替わり、結果として異なるテーマが表示されます。

マテリアルのテーマは Android 5.0 以降でのみサポートされているため、以前のバージョンの Android で実行するためにアプリのテーマを作成することはできません (または、マテリアルテーマから派生したカスタムテーマを使用することはできません)。 ただし、Android 5.0 デバイスで素材のテーマを使用するようにアプリを構成し、以前のバージョンの Android で動作する場合は以前のテーマに適切にフォールバックできます (詳細については、この記事の「 [互換性](#compatibility) 」セクションを参照してください)。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android 5.0 のマテリアルテーマ機能を使用するには、次のものが必要です。

- **Xamarin android** &ndash; 4.20 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。 

- **Android SDK** &ndash; Android SDK Manager を使用して、Android 5.0 (API 21) 以降をインストールする必要があります。

- **JAVA JDK 1.8** &ndash; API レベル23以前を対象としている場合は、JDK 1.7 を使用できます。 JDK 1.8 は、 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)から入手できます。

Android 5.0 アプリケーションプロジェクトを構成する方法については、「 [android 5.0 プロジェクトの設定](~/android/platform/lollipop.md)」を参照してください。

## <a name="using-the-built-in-themes"></a>組み込みテーマの使用

マテリアルテーマを使用する最も簡単な方法は、カスタマイズせずに組み込みのテーマを使用するようにアプリを構成することです。 テーマを明示的に構成しない場合、アプリは既定で `Theme.Material` (ダークテーマ) に設定されます。 アプリにアクティビティが1つしかない場合は、アクティビティレベルでテーマを構成できます。 アプリに複数のアクティビティがある場合は、すべてのアクティビティで同じテーマを使用するようにアプリケーションレベルでテーマを構成するか、異なるテーマを別のアクティビティに割り当てることができます。 以下のセクションでは、アプリレベルとアクティビティレベルでテーマを構成する方法について説明します。

### <a name="theming-an-application"></a>アプリケーションのテーマを適用する

素材テーマフレーバーを使用するようにアプリケーション全体を構成するには、 `android:theme` **AndroidManifest.xml** のアプリケーションノードの属性を次のいずれかに設定します。

- `@android:style/Theme.Material`&ndash;ダークテーマ。

- `@android:style/Theme.Material.Light`&ndash;ライトテーマ。

- `@android:style/Theme.Material.Light.DarkActionBar`&ndash;濃いアクションバーを持つ明るいテーマ。

次の例では、アプリケーション *MyApp* でライトテーマを使用するように構成します。

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

`Theme`または、 **AssemblyInfo.cs** (または**Properties.cs**) でアプリケーション属性を設定することもできます。 次に例を示します。

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

アプリケーションテーマがに設定され `@android:style/Theme.Material.Light` ている場合、 *MyApp* のすべてのアクティビティがを使用して表示され `Theme.Material.Light` ます。

### <a name="theming-an-activity"></a>アクティビティのテーマを行う

アクティビティをテーマするには、 `Theme` アクティビティ宣言の上にある属性に設定を追加 `[Activity]` し、使用する `Theme` マテリアルテーマフレーバーにを割り当てます。 次の例では、アクティビティのテーマをとし `Theme.Material.Light` ます。

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

このアプリの他のアクティビティでは、既定の `Theme.Material` 濃色配色 (または、構成されている場合はアプリケーションテーマ設定) が使用されます。

<a name="customtheme"></a>

## <a name="using-custom-themes"></a>カスタムテーマの使用

自分のブランドの色でアプリのスタイルを設定するカスタムテーマを作成することによって、ブランドを強化することができ &rsquo; ます。 カスタムテーマを作成するには、組み込みのマテリアルテーマフレーバーから派生する新しいスタイルを定義し、変更する色属性をオーバーライドします。 たとえば、から派生するカスタムテーマを定義 `Theme.Material.Light.DarkActionBar` し、画面の背景色を白ではなくベージュに変更することができます。

マテリアルテーマでは、カスタマイズのために次のレイアウト属性が公開されています。

- `colorPrimary`&ndash;アプリバーの色。

- `colorPrimaryDark`&ndash;ステータスバーとコンテキストアプリバーの色。通常は、のダークバージョン `colorPrimary` です。

- `colorAccent`&ndash;チェックボックス、オプションボタン、編集テキストボックスなどの UI コントロールの色。

- `windowBackground`&ndash;画面の背景の色。

- `textColorPrimary`&ndash;アプリバーの UI テキストの色。

- `statusBarColor`&ndash;ステータスバーの色。

- `navigationBarColor`&ndash;ナビゲーションバーの色。

次の図に、これらの画面領域のラベルを示します。

[![属性とそれに関連付けられた画面領域の図](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

既定で `statusBarColor` は、はの値に設定され `colorPrimaryDark` ます。 を純色に設定することも、ステータスバーを透明にするように設定することもでき `statusBarColor` `@android:color/transparent` ます。 をに設定すると、ナビゲーションバーを透明にすることもでき `navigationBarColor` `@android:color/transparent` ます。

### <a name="creating-a-custom-app-theme"></a>カスタムアプリのテーマの作成

カスタムアプリのテーマを作成するには、アプリプロジェクトの **Resources** フォルダー内のファイルを作成および変更します。 カスタムテーマを使用してアプリのスタイルを設定するには、次の手順に従います。

- [**リソース/値**] に**colors.xml**ファイルを作成し &mdash; ます。このファイルを使用して、カスタムテーマの色を定義します。 たとえば、次のコードを **colors.xml** に貼り付けて、作業を開始することができます。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

- このサンプルファイルを変更して、カスタムテーマで使用する色リソースの名前と色コードを定義します。

- **Resources/v21**フォルダーを作成します。 このフォルダーに、 **styles.xml** ファイルを作成します。

    [![Resources/values-21.xml フォルダー内の styles.xml の場所](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    **リソース/値-v21**は android 5.0 &ndash; 以前のバージョンの android では、このフォルダー内のファイルを読み取らないことに注意してください。

- `resources` **styles.xml**にノードを追加し、 `style` カスタムテーマの名前を持つノードを定義します。 たとえば、次に示すのは、 *Mycustomtheme* (組み込みテーマスタイルから派生) を定義する**styles.xml**ファイルです `Theme.Material.Light` 。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

- この時点で、 *Mycustomtheme* を使用するアプリでは、カスタマイズなしで株価テーマが表示され `Theme.Material.Light` ます。

    [![カスタマイズ前のカスタムテーマの外観](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

- 変更するレイアウト属性の色を定義して、 **styles.xml** に色のカスタマイズを追加します。 たとえば、アプリバーの色をに変更 `my_blue` し、UI コントロールの色をに変更するには `my_purple` 、 **colors.xml**で構成された色リソースを参照する**styles.xml**に色のオーバーライドを追加します。

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

これらの変更が適用されると、 *Mycustomtheme* を使用するアプリでは、のアプリバーの色 `my_blue` との UI コントロールが表示され `my_purple` ますが、他のすべての配色を使用し `Theme.Material.Light` ます。

[![カスタマイズ後のカスタムテーマの外観](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

この例では、 *Mycustomtheme* は、 `Theme.Material.Light` 背景色、ステータスバー、およびテキストの色のから色をではしますが、アプリバーの色をに変更 `my_blue` し、ラジオボタンの色をに設定し `my_purple` ます。

<a name="customview"></a>

### <a name="creating-a-custom-view-style"></a>カスタムビュースタイルの作成

Android 5.0 では、個々のビューのスタイルを設定することもできます。 (前のセクションで説明したように) **colors.xml** および **styles.xml** を作成した後、 **styles.xml**にビュースタイルを追加できます。
個々のビューのスタイルを適用するには、次の手順に従います。

- **V21/styles.xml**を編集し、 `style` カスタムビュースタイルの名前を持つノードを追加します。 このノード内のビューのカスタムカラー属性を設定し `style` ます。 たとえば、角が丸く、カードの背景色としてを使用するカスタム [CardView](~/android/user-interface/controls/card-view.md) スタイルを作成するには、 `my_blue` `style` (ノード内の) **styles.xml** にノードを追加 `resources` し、背景色とコーナー半径を構成します。

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

- レイアウトで、 `style` そのビューの属性を、前の手順で選択したカスタムスタイル名と一致するように設定します。 次に例を示します。

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

次のスクリーンショットは、( `CardView` `CardView` 右側に表示されている) カスタムテーマでスタイル設定されたと比較して、既定値 (左側に表示) の例を示してい `CardView.MyBlue` ます。

[![既定の CardView とカスタム CardView の例](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

この例では、カスタムは `CardView` 背景色 `my_blue` と18dp コーナー半径と共に表示されます。

## <a name="compatibility"></a>互換性

Android 5.0 で素材のテーマを使用するようにアプリのスタイルを設定し、古い Android バージョンで自動的に下互換性のあるスタイルに戻すには、次の手順を使用します。

- **リソース/値-v21/styles.xml**で、マテリアルテーマスタイルから派生するカスタムテーマを定義します。 次に例を示します。

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- 以前のテーマから派生した **リソース/値/styles.xml** でカスタムテーマを定義しますが、上記と同じテーマ名を使用します。 次に例を示します。

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- **AndroidManifest.xml**で、カスタムテーマ名を使用してアプリを構成します。 
    次に例を示します。

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

- または、カスタムテーマを使用して、特定のアクティビティのスタイルを設定することもできます。

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

テーマで**colors.xml**ファイルで定義された色を使用する場合は、カスタムテーマの両方のバージョンが色の定義にアクセスできるように、このファイルを resources **/v21**ではなく**リソース/** 値に配置してください。

Android 5.0 デバイスでアプリを実行すると、 **Resources/v21/styles.xml**で指定されたテーマ定義が使用されます。 このアプリが古い Android デバイスで実行されている場合は、[ **リソース/値/styles.xml**] で指定されたテーマ定義に自動的にフォールバックされます。

以前のバージョンの Android とのテーマの互換性の詳細については、「 [代替リソース](~/android/app-fundamentals/resources-in-android/alternate-resources.md)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、Android 5.0 (ロリポップ) に含まれる新しいマテリアルテーマのユーザーインターフェイススタイルについて紹介しました。 ここでは、アプリのスタイルを設定するために使用できる3つの組み込みの素材テーマの種類について説明し、アプリをブランド化するためのカスタムテーマを作成する方法を説明しました。また、個々のビューをテーマする方法の例を示しました。 最後に、この記事では、以前のバージョンの Android との下位互換性を維持しながら、アプリで素材のテーマを使用する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [ThemeSwitcher (サンプル)](/samples/xamarin/monodroid-samples/android50-themeswitcher)
- [ロリポップの概要](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [代替リソース](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android ロリポップ](https://developer.android.com/about/versions/lollipop)
- [Android の円の開発者](https://developer.android.com/about/versions/pie/)
- [素材のデザイン](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [素材デザインの原則](https://material.io/design/)
- [互換性の維持](https://developer.android.com/training/backward-compatible-ui/)