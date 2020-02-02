---
title: 素材のテーマ
description: マテリアルテーマを使用して Xamarin Android アプリをテーマする方法
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 809f6241b3a17f63fe3077f896095c303e1dfd2e
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940835"
---
# <a name="material-theme"></a>素材のテーマ

*マテリアル テーマ*のビューと Android 5.0 (ロリポップ) で始まるアクティビティの外観を決定するユーザー インターフェイスのスタイルがします。 マテリアル テーマは、およびアプリケーション、システム UI で使用するよう Android 5.0 に組み込まれています。 マテリアル テーマは、ユーザーは [設定] メニューから動的に選択できるシステム全体の外観のオプションの意味で「テーマ」ではありません。 代わりに、マテリアル テーマ見なすことができますのアプリの外観をカスタマイズに使用できる関連する組み込みの基本スタイルのセットとして。

Android には、次の3つの素材テーマがあります。

- `Theme.Material` &ndash; のマテリアルテーマのダークバージョンです。これは、Android 5.0 の既定のフレーバーです。

- マテリアルテーマの &ndash; ライトバージョンを `Theme.Material.Light` します。

- `Theme.Material.Light.DarkActionBar` &ndash; のマテリアルテーマの明るいバージョンですが、暗いアクションバーが使用されています。

これらの配色テーマの例の例を次に示します。

[![濃色テーマ、明るいテーマ、およびダーク操作バーテーマのスクリーンショットの例](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

マテリアルテーマから派生させて独自のテーマを作成し、一部またはすべての色属性をオーバーライドすることができます。 たとえば、`Theme.Material.Light`から派生したテーマを作成できますが、ブランドの色と一致するようにアプリバーの色をオーバーライドします。 個々のビューのスタイルを作成することもできます。たとえば、角が丸く、暗い背景色を使用する[CardView](~/android/user-interface/controls/card-view.md)のスタイルを作成できます。

アプリ全体に対して1つのテーマを使用することも、アプリ内のさまざまな画面 (アクティビティ) に対して異なるテーマを使用することもできます。 上のスクリーンショットでは、たとえば、1つのアプリが各アクティビティに対して異なるテーマを使用して、組み込みの配色を示しています。 オプションボタンを利用すると、アプリがさまざまなアクティビティに切り替わり、結果として異なるテーマが表示されます。

マテリアルのテーマは Android 5.0 以降でのみサポートされているため、以前のバージョンの Android で実行するためにアプリのテーマを作成することはできません (または、マテリアルテーマから派生したカスタムテーマを使用することはできません)。 ただし、Android 5.0 デバイスで素材のテーマを使用するようにアプリを構成し、以前のバージョンの Android で動作する場合は以前のテーマに適切にフォールバックできます (詳細については、この記事の「[互換性](#compatibility)」セクションを参照してください)。

## <a name="requirements"></a>要件

Xamarin ベースのアプリで新しい Android 5.0 のマテリアルテーマ機能を使用するには、次のものが必要です。

- Xamarin **android** 4.20 &ndash; Visual Studio または Visual Studio for Mac を使用してインストールおよび構成する必要があります。 

- **Android SDK** &ndash; Android 5.0 (API 21) 以降を Android SDK Manager を使用してインストールする必要があります。

- API レベル23以前を対象としている場合は、 **JAVA jdk 1.8** &ndash; jdk 1.7 を使用できます。 JDK 1.8 は、 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)から入手できます。

Android 5.0 アプリケーションプロジェクトを構成する方法については、「 [android 5.0 プロジェクトの設定](~/android/platform/lollipop.md)」を参照してください。

## <a name="using-the-built-in-themes"></a>組み込みテーマの使用

マテリアルテーマを使用する最も簡単な方法は、カスタマイズせずに組み込みのテーマを使用するようにアプリを構成することです。 テーマを明示的に構成しない場合、アプリは既定で `Theme.Material` (ダークテーマ) になります。 アプリにアクティビティが1つしかない場合は、アクティビティレベルでテーマを構成できます。 アプリに複数のアクティビティがある場合は、すべてのアクティビティで同じテーマを使用するようにアプリケーションレベルでテーマを構成するか、異なるテーマを別のアクティビティに割り当てることができます。 以下のセクションでは、アプリレベルとアクティビティレベルでテーマを構成する方法について説明します。

### <a name="theming-an-application"></a>アプリケーションのテーマを適用する

素材テーマフレーバーを使用するようにアプリケーション全体を構成するには、 **Androidmanifest**のアプリケーションノードの `android:theme` 属性を次のいずれかに設定します。

- `@android:style/Theme.Material` &ndash; ダークテーマです。

- &ndash; 明るいテーマを `@android:style/Theme.Material.Light` します。

- 濃いアクションバーで &ndash; 明るいテーマを `@android:style/Theme.Material.Light.DarkActionBar` します。

次の例では、アプリケーション*MyApp*でライトテーマを使用するように構成します。

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

または、 **AssemblyInfo.cs** (または**Properties.cs**) で application `Theme` 属性を設定することもできます。 例:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

アプリケーションテーマが `@android:style/Theme.Material.Light`に設定されている場合、 *MyApp*のすべてのアクティビティが `Theme.Material.Light`を使用して表示されます。

### <a name="theming-an-activity"></a>アクティビティのテーマを行う

アクティビティをテーマするには、アクティビティ宣言の上にある `[Activity]` 属性に `Theme` 設定を追加し、使用するマテリアルテーマフレーバーに `Theme` を割り当てます。 次の例では、アクティビティと `Theme.Material.Light`をテーマします。

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

このアプリのその他のアクティビティでは、既定の `Theme.Material` ダーク配色 (または、構成されている場合はアプリケーションテーマ設定) が使用されます。

<a name="customtheme" />

## <a name="using-custom-themes"></a>カスタムテーマの使用

ブランド&rsquo;s 色でアプリのスタイルを設定するカスタムテーマを作成することにより、ブランドを強化できます。 カスタムテーマを作成するには、組み込みのマテリアルテーマフレーバーから派生する新しいスタイルを定義し、変更する色属性をオーバーライドします。 たとえば、`Theme.Material.Light.DarkActionBar` から派生したカスタムテーマを定義し、画面の背景色を白ではなくベージュに変更することができます。

マテリアルテーマでは、カスタマイズのために次のレイアウト属性が公開されています。

- アプリバーの色を &ndash; `colorPrimary` ます。

- ステータスバーとコンテキストアプリバーの色 &ndash; `colorPrimaryDark` します。これは通常、`colorPrimary`のダークバージョンです。

- `colorAccent`、チェックボックス、オプションボタン、編集テキストボックスなどの UI コントロールの色を &ndash; します。

- 画面の背景色を &ndash; `windowBackground` ます。

- アプリバーの UI テキストの色 &ndash; `textColorPrimary` します。

- ステータスバーの色 &ndash; `statusBarColor` します。

- ナビゲーションバーの色 &ndash; `navigationBarColor` ます。

次の図に、これらの画面領域のラベルを示します。

[属性とそれに関連付けられた画面領域の ![ダイアグラム](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

既定では、`statusBarColor` は `colorPrimaryDark`の値に設定されます。 `statusBarColor` を純色に設定することも、`@android:color/transparent` に設定してステータスバーを透明にすることもできます。 `navigationBarColor` を `@android:color/transparent`に設定することによって、ナビゲーションバーを透明にすることもできます。

### <a name="creating-a-custom-app-theme"></a>カスタムアプリのテーマの作成

カスタムアプリのテーマを作成するには、アプリプロジェクトの**Resources**フォルダー内のファイルを作成および変更します。 カスタムテーマを使用してアプリのスタイルを設定するには、次の手順に従います。

- このファイルを使用してカスタムテーマの色を定義する &mdash;、**リソース/値**で**色の .xml**ファイルを作成します。 たとえば、次のコードを「 **colors** 」に貼り付けると、作業を開始するのに役立ちます。

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

- **Resources/v21**フォルダーを作成します。 このフォルダーに、次のように**スタイルの .xml**ファイルを作成します。

    [Resources/values-21 フォルダー内の styles .xml の ![場所](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    **リソース/値-v21**は android 5.0 に固有であることに注意してください &ndash; 古いバージョンの android では、このフォルダー内のファイルは読み取られません。

- `resources` ノードをスタイルの **.xml**に追加し、カスタムテーマの名前を使用して `style` ノードを定義します。 たとえば、次に示すのは、 *Mycustomtheme* (組み込みの `Theme.Material.Light` テーマスタイルから派生したもの) を定義する**スタイルの .xml**ファイルです。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

- この時点で、 *Mycustomtheme*を使用するアプリでは、カスタマイズなしで stock `Theme.Material.Light` テーマが表示されます。

    [カスタマイズ前にカスタムテーマの外観を ![する](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

- 変更するレイアウト属性の色を定義して、色のカスタマイズを**スタイルの .xml**に追加します。 たとえば、アプリバーの色を `my_blue` に変更し、UI コントロールの色を `my_purple`に変更するには、color **. xml**で構成されている色リソースを参照する**スタイルの xml**に色のオーバーライドを追加します。

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

これらの変更が適用されると、 *Mycustomtheme*を使用するアプリでは、`my_purple`の `my_blue` と UI コントロールにアプリバーの色が表示されますが、他の場所では `Theme.Material.Light` の配色を使用します。

[カスタマイズ後にカスタムテーマの外観を ![する](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

この例では、 *Mycustomtheme*は背景色、ステータスバー、およびテキストの色の `Theme.Material.Light` から色をではますが、アプリバーの色を `my_blue` に変更し、ラジオボタンの色を `my_purple`に設定します。

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>カスタムビュースタイルの作成

Android 5.0 では、個々のビューのスタイルを設定することもできます。 前のセクションで説明したように、**色 .xml**と**スタイル .xml**を作成した後は、**スタイルの .xml**にビュースタイルを追加できます。
個々のビューのスタイルを適用するには、次の手順に従います。

- **Resources/values-v21/styles .xml**を編集し、カスタムビュースタイルの名前を持つ `style` ノードを追加します。 この `style` ノード内で、ビューのカスタムカラー属性を設定します。 たとえば、角が丸く、カードの背景色として `my_blue` を使用するカスタム [CardView](~/android/user-interface/controls/card-view.md) スタイルを作成するには、`style` ノードを (`resources` ノード内の) **styles.xml** に追加し、背景色を構成します。角の半径:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

- レイアウトで、そのビューの `style` 属性を、前の手順で選択したカスタムスタイル名と一致するように設定します。 例:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

次のスクリーンショットは、(左側に表示される) 既定の `CardView` の例を示しています。これは、(右側に表示されている) カスタム `CardView.MyBlue` テーマでスタイルを設定した `CardView` と比較したものです。

[既定の CardView およびカスタム CardView の ![例](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

この例では、カスタム `CardView` が、背景色 `my_blue` と18dp コーナー半径と共に表示されます。

## <a name="compatibility"></a>互換性

Android 5.0 で素材のテーマを使用するようにアプリのスタイルを設定し、古い Android バージョンで自動的に下互換性のあるスタイルに戻すには、次の手順を使用します。

- マテリアルテーマスタイルから派生する**Resources/values-v21/styles .xml**でカスタムテーマを定義します。 例:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- 以前のテーマから派生した**リソース/値/スタイルの xml**でカスタムテーマを定義しますが、上記と同じテーマ名を使用します。 例:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- **Androidmanifest .xml**で、カスタムテーマ名を使用してアプリを構成します。 
    例:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

- または、カスタムテーマを使用して、特定のアクティビティのスタイルを設定することもできます。

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

テーマで、**色 .xml**ファイルで定義された色を使用する場合は、カスタムテーマの両方のバージョンが色の定義にアクセスできるように、このファイルを resources **/v21**ではなく**リソース/** 値に配置してください。

Android 5.0 デバイスでアプリを実行すると、 **Resources/values-v21/styles**に指定されたテーマ定義が使用されます。 このアプリが古い Android デバイスで実行されている場合は、 **Resources/values/styles .xml**に指定されているテーマ定義に自動的に切り替えられます。

以前のバージョンの Android とのテーマの互換性の詳細については、「[代替リソース](~/android/app-fundamentals/resources-in-android/alternate-resources.md)」を参照してください。

## <a name="summary"></a>要約

この記事では、Android 5.0 (ロリポップ) に含まれる新しいマテリアルテーマのユーザーインターフェイススタイルについて紹介しました。 ここでは、アプリのスタイルを設定するために使用できる3つの組み込みの素材テーマの種類について説明し、アプリをブランド化するためのカスタムテーマを作成する方法を説明しました。また、個々のビューをテーマする方法の例を示しました。 最後に、この記事では、以前のバージョンの Android との下位互換性を維持しながら、アプリで素材のテーマを使用する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [ThemeSwitcher (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-themeswitcher)
- [ロリポップの概要](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [代替リソース](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android ロリポップ](https://developer.android.com/about/versions/lollipop)
- [Android の円の開発者](https://developer.android.com/about/versions/pie/)
- [素材のデザイン](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [素材デザインの原則](https://material.io/design/)
- [互換性の維持](https://developer.android.com/training/backward-compatible-ui/)
