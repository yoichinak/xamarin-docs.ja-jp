---
title: Android のローカライズ
description: このドキュメントでは、Android SDK と Xamarin を使用してアクセスする方法のローカライズ機能について説明します。
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f6a7f8859340dcc8e48b6a4e6f56847168f4b71e
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829761"
---
# <a name="android-localization"></a>Android のローカライズ

_このドキュメントでは、Android SDK と Xamarin を使用してアクセスする方法のローカライズ機能について説明します。_

## <a name="android-platform-features"></a>Android プラットフォーム機能

このセクションでは、Android のローカリゼーションの主な機能について説明します。 スキップする、[次のセクション](#basics)に固有のコードと例を参照してください。

### <a name="locale"></a>ロケール

ユーザーの言語を選択する**設定 > 言語 & 入力**します。 これを選択したコントロールの表示言語と地域設定の両方を使用 (例。 日付と数値の書式設定)。

現在のロケールは現在のコンテキストを使用してクエリを実行できる`Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

この値は、言語コードとアンダー スコアで区切られた、ロケールのコードの両方を含むロケール識別子になります。 リファレンスについては、ここでは、 [Java ロケールの一覧](https://www.oracle.com/technetwork/java/javase/locales-137662.html)と[StackOverflow を使用して、Android でサポートされているロケール](https://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android)します。

一般的な例は、次のとおりです。

* `en_US` 英語 (米国)
* `es_ES` スペイン語 (スペイン)
* `ja_JP` 日本語 (日本) の
* `zh_CN` 中国語 (中国) の
* `zh_TW` 中国語 (台湾) の
* `pt_PT` ポルトガル語 (ポルトガル)
* `pt_BR` ポルトガル語 (ブラジル)

### <a name="localechanged"></a>LOCALE_CHANGED

Android 生成`android.intent.action.LOCALE_CHANGED`にユーザーがその言語の選択を変更します。

アクティビティは、これを設定して処理することもできます、`android:configChanges`次のように、アクティビティの属性。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Android での国際化の基礎

Android のローカライズ戦略では、次の主要部分があります。

- ローカライズされた文字列、イメージ、およびその他のリソースを格納するリソース フォルダー。

- `GetText` メソッドは、コードのローカライズされた文字列を取得するために使用

- `@string/id` AXML ファイルで自動的に配置するレイアウト内の文字列をローカライズします。

### <a name="resource-folders"></a>リソース フォルダー

Android アプリケーションは、次のようなリソースのフォルダー内のほとんどのコンテンツを管理します。

* **レイアウト**-AXML レイアウト ファイルが含まれています。
* **drawable** -イメージ、描画可能なその他のリソースが含まれています。
* **値**-文字列が含まれています。
* **raw** -データ ファイルが含まれています。

ほとんどの開発者の使用に慣れている**dpi**のサフィックス、 **drawable** Android デバイスごとに適切なバージョンを選択できるようにすること、イメージの複数のバージョンを提供するディレクトリ。 言語とカルチャ識別子を持つリソース ディレクトリのアスタリスクで複数言語の翻訳を提供するのには、同じメカニズムを使用します。

![複数のカルチャ識別子のリソース/drawable とリソース/値のフォルダーのスクリーン ショット](localization-images/resources.png)

> [!NOTE]
> 最上位レベルのような言語を指定するときに`es`だけ 2 つの文字が必要ですディレクトリ名の形式には、ダッシュ、小文字必要があります、完全なロケールを指定するときに**r** 例については、2つの部分を分離するには。**pt rBR**または**zh rCN**します。 (例: アンダー スコアを持つ、コードで返される値と比較します。 `pt_BR`) これらの両方が .NET の値に異なる`CultureInfo`クラスで使用では、ダッシュのみ (例。 `pt-BR`) Xamarin のプラットフォームで作業する場合は、これらの違いに注意してくださいを保持します。

#### <a name="stringsxml-file-format"></a>Strings.xml ファイルの形式

ローカライズされた**値**ディレクトリ (例。 **値 es**または**値-pt-rBR**) という名前のファイルを含める必要があります**Strings.xml**そのロケールの翻訳されたテキストが含まれる。

変換可能な各文字列は、リソースとして指定された ID を持つ XML 要素、`name`属性と値として変換された文字列。

```xml
<string name="app_name">TaskyL10n</string>
```

通常の XML 規則に従ってエスケープする必要があると、 `name` (スペースまたはダッシュ) の有効な Android のリソース ID でなければなりません。 次の例では、既定値 (英語) の文字列のファイルの例に示します。

**values/Strings.xml**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

スペイン語の directory**値 es**と同じ名前のファイルが含まれています (**Strings.xml**) 翻訳を格納しています。

**values-es/Strings.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![複数のスクリーン ショット値フォルダー、Strings.xml ファイルを含む各](localization-images/values.png)

文字列ファイル設定では、レイアウトとコードの両方の翻訳済みの値を参照できます。

### <a name="axml-layout-files"></a>AXML レイアウト ファイル

レイアウト ファイルでローカライズされた文字列を参照するには、使用、`@string/id`構文。 このサンプルではからの XML スニペット`text`を設定するプロパティにローカライズされたリソース Id (その他のいくつかの属性が省略されている)。

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>GetText メソッド

コードでの翻訳された文字列を取得する、`GetText`メソッドをリソース ID を渡します。

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>数量の文字列

Android の文字列リソースを作成することも*数量文字列*などさまざまな数量の場合の別の翻訳を提供する翻訳者を許可します。

* 「がありますが左に 1 つのタスク。」
* "2 があるタスクはまだしないでください"。

(ジェネリックではなく「は」n タスク左) です。

**Strings.xml**

```xml
<plurals name="numberOfTasks">
   <!--
      As a developer, you should always supply "one" and "other"
      strings. Your translators will know which strings are actually
      needed for their language.
    -->
   <item quantity="one">There is %d task left.</item>
   <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

完全な文字列の使用を表示するために、`GetQuantityString`メソッドを表示するには、リソース ID と値を渡します (これが 2 回渡される)。 2 番目のパラメーターを使用して Android を特定*を*`quantity`に使用される文字列、3 番目のパラメーターは実際には (両方は必要な) 文字列に代入される値です。

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

有効な`quantity`スイッチします。

* ゼロ
* one
* 2 つ
* いくつか
* many
* その他

詳細に記述している、 [Android docs](https://developer.android.com/guide/topics/resources/string-resource.html#Plurals)します。'特別な' を処理するものを特定の言語が必要ない場合`quantity`文字列は無視されます (たとえば、英語ののみを使用`one`と`other`指定する、`zero`文字列には効果はありません、これは使用されません)。

### <a name="images"></a>画像

イメージのローカライズされた文字列ファイルと同じ規則に従います: に、アプリケーションで参照されているすべてのイメージを配置する必要があります**drawable**ディレクトリがフォールバックします。

ロケール固有のイメージがなど修飾 drawable フォルダーに配置されて、 **drawable es**または**drawable ja** (dpi 指定子を追加こともできます)。

このスクリーン ショットでは 4 つのイメージに保存されます、 **drawable**ディレクトリが 1 つだけ、 **flag.png**は、ローカライズその他のディレクトリにコピーします。

![それぞれ 1 つまたは複数のローカライズされた .png ファイルを含む複数の drawable フォルダーのスクリーン ショット](localization-images/drawable.png)


#### <a name="other-resource-types"></a>その他のリソースの種類

また、レイアウト、アニメーション、raw ファイルなどの言語に固有のリソース、他の種類の代わりを指定することもできます。 この場合は、ターゲット言語の 1 つ以上の特定の画面のレイアウトを指定できます、たとえばする可能性がありますレイアウトを作成できる非常に長いテキストのラベルをドイツ語の具体的には。

Android 4.2 には、サポートが導入されました。[右から左 (RTL) 言語](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html)アプリケーション設定を設定する場合`android:supportsRtl="true"`します。 リソース修飾子`"ldrtl"`右から左へ表示用にデザインされたカスタム レイアウトを格納するディレクトリ名に含めることができます。

リソース ディレクトリの名前付けとフォールバックの詳細については、Android ドキュメントを参照してください[代替のリソースを提供する](https://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)します。


### <a name="app-name"></a>アプリ名

アプリケーション名が簡単にローカライズを使用して、`@string/id`での`MainLauncher`アクティビティ。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>右から左 (RTL) 言語

Android 4.2 以降では、RTL レイアウトで詳しく説明の完全なサポート、[右から左へのネイティブ サポート ブログ](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html)します。

Android 4.2 (API レベル 17) を使用する場合とで値を指定できる、新しい配置`start`と`end`の代わりに`left`と`right`(たとえば`android:paddingStart`)。 ような新しい Api もあります`LayoutDirection`、`TextDirection`と`TextAlignment`に適応する画面を右から左へのリーダーの構築を支援します。

次のスクリーン ショット、[ローカライズ**Tasky**サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)アラビア語。

[![アラビア語 Tasky アプリのスクリーン ショット](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

次のスクリーン ショットに示します、[ローカライズ**Tasky**サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)ヘブライ語で。

[![ヘブライ語の Tasky アプリのスクリーン ショット](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

使用して右から左へテキストをローカライズ**Strings.xml** LTR テキストと同じ方法でファイル。

## <a name="testing"></a>テスト

既定のロケールを徹底的にテストしてください。 アプリケーションがクラッシュする場合 (つまりが欠落している) 何らかの理由により、既定のリソースを読み込むことができません。

### <a name="emulator-testing"></a>エミュレーターのテスト

Google を参照してください[、Android エミュレーターでテスト](https://developer.android.com/guide/topics/resources/localization.html#testing)エミュレーターが ADB シェルを使用して特定のロケールに設定する方法についてのセクション。

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>デバイスのテスト

デバイスをテストするには、言語を変更、**設定**アプリ。

> [!TIP]
> ようにをメモして、アイコンとメニュー項目の場所、言語設定を元に戻すことができます。


## <a name="summary"></a>Summary

この記事では、組み込みのリソースの処理を使用して Android アプリケーションのローカライズの基本について説明します。 IOS、Android、および (Xamarin.Forms) を含むクロス プラットフォーム アプリでの i18n および L10n の詳細を学習できます[このクロスプラット フォーム対応ガイド](~/cross-platform/app-fundamentals/localization.md)します。



## <a name="related-links"></a>関連リンク

- [Tasky (コードのローカライズ版) (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android のリソースとローカライズ](https://developer.android.com/guide/topics/resources/localization.html)
- [クロスプラット フォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms のローカライズ](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)
