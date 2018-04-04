---
title: Android のローカライズ
description: このドキュメントでは、Android SDK、および Xamarin とそれらにアクセスする方法のローカリゼーション機能を紹介します。
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6924cc9989c8ab1ca66472b628cdab677e546a3e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="android-localization"></a>Android のローカライズ

_このドキュメントでは、Android SDK、および Xamarin とそれらにアクセスする方法のローカリゼーション機能を紹介します。_

## <a name="android-platform-features"></a>Android プラットフォーム機能

このセクションでは、Android のローカライズの主な機能について説明します。 進んで、[次のセクション](#basics)に特定のコードと例を参照してください。

### <a name="locale"></a>ロケール

ユーザーがその言語でを選択**設定 > 言語入力 &**です。 この選択を制御、表示言語と地域の設定を使用します。 日付と数値の書式設定)。

現在のロケールは現在のコンテキストを使用してクエリを実行できます`Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

この値を言語コードとアンダー スコアで区切られた、ロケールのコードの両方を含むロケール識別子となります。 リファレンスについては、ここでは、 [Java ロケールの一覧](http://www.oracle.com/technetwork/java/javase/locales-137662.html)と[StackOverflow 経由での Android でサポートされているロケール](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android)です。

一般的な例は、次のとおりです。

* `en_US` 英語 (米国 Statees) の場合
* `es_ES` スペイン語 (スペイン)
* `ja_JP` 日本語 (日本) の
* `zh_CN` 中国語 (中国)
* `zh_TW` 中国語 (台湾)
* `pt_PT` ポルトガル語 (ポルトガル)
* `pt_BR` ポルトガル語 (ブラジル)

### <a name="localechanged"></a>LOCALE_CHANGED

Android 生成`android.intent.action.LOCALE_CHANGED`ユーザーがその言語の選択を変更するときにします。

アクティビティを設定してこれを処理することを選択できます、`android:configChanges`次のように、アクティビティの属性。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Android での国際化の基本事項

Android のローカリゼーション戦略では、次の主要部分があります。

- ローカライズされた文字列、イメージ、およびその他のリソースを格納するリソースのフォルダーです。

- `GetText` コードのローカライズされた文字列を取得するために使用するメソッド

- `@string/id` AXML ファイルに自動的に配置するレイアウト内の文字列をローカライズします。

### <a name="resource-folders"></a>リソースのフォルダー

Android アプリケーションは、次のようなリソースのフォルダー内のほとんどのコンテンツを管理します。

* **レイアウト**-AXML レイアウト ファイルが含まれています。
* **ドロウアブル**-イメージ、およびその他のドロウアブル リソースが含まれています。
* **値**-文字列が含まれています。
* **raw** -データ ファイルが含まれています。

ほとんどの開発者の使用に慣れている**dpi**のサフィックスの付いた、**ドロウアブル**Android デバイスごとに正しいバージョンを選択できるようにすること、イメージの複数のバージョンを提供するディレクトリ。 同じメカニズムを使用すると、言語とカルチャ識別子を持つリソース ディレクトリを付けませんによって複数の言語の翻訳を提供します。

![複数のカルチャ識別子のリソース/描画とリソース/値のフォルダーのスクリーン ショット](localization-images/resources.png)

> [!NOTE]
> ような最上位レベルの言語を指定するときに`es`だけで 2 つの文字が必要ですただし、ディレクトリ名の形式には、ダッシュと小文字が必要な完全ロケールを指定する場合**r** 例については、2つの部分を分離するには。**pt rBR**または**zh rCN**です。 (アンダー スコアを持つ、コードに返される値と比較します。 `pt_BR`) これらの両方が .NET の値に異なる`CultureInfo`クラスの使用方法、ダッシュのみ (います `pt-BR`) Xamarin プラットフォーム間で操作するときに、これらの相違点に注意してください。

#### <a name="stringsxml-file-format"></a>Strings.xml ファイル形式

ローカライズされた**値**ディレクトリ (例です。 **値 es**または**値 pt-rBR**) という名前のファイルを含める必要があります**Strings.xml**そのロケールの翻訳済みテキストが含まれる。

変換可能な各文字列は、リソースとして指定された ID を持つ XML 要素、`name`属性と翻訳された文字列値として。

```xml
<string name="app_name">TaskyL10n</string>
```

通常の XML 規則に従ってエスケープする必要があります、 `name` (スペースまたはダッシュ) の有効な Android リソース ID を指定する必要があります。 たとえば、既定値 (英語) の文字列ファイルの例を次に示します。

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

スペイン語のディレクトリ**値 es**が同じ名前のファイルが含まれています (**Strings.xml**) に翻訳を格納しています。

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

![複数のスクリーン ショット値フォルダー、Strings.xml のファイルを含む各](localization-images/values.png)

文字列ファイルのセットアップでは、レイアウトとコードの両方で、翻訳した値を参照できます。

### <a name="axml-layout-files"></a>AXML レイアウト ファイル

レイアウト ファイルでローカライズされた文字列を参照するには、使用、`@string/id`構文です。 このサンプルではからの XML スニペット`text`に設定されているプロパティのローカライズ リソース Id (他のいくつかの属性が省略されている)。

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

コードでの翻訳された文字列を取得するを使用して、`GetText`メソッドと、リソース ID のパス。

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>数量の文字列

Android の文字列リソースも作成できます*数量文字列*翻訳を提供するさまざまな異なる数量の場合などの翻訳を許可します。

* 「は 1 つのタスクを左。」
* "2 を行うにはタスクがまだ"。

(ジェネリック型ではなく"There are n タスク左") です。

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

完全な文字列の使用を表示するために、`GetQuantityString`を表示するには、リソース ID と値を渡して、メソッド (渡される 2 回)。 2 番目のパラメーター Android によって判別される*を*`quantity`に使用される文字列、3 番目のパラメーターは、実際には (両方ともは必須です) の文字列に置換値。

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

詳しく説明しているが、 [Android docs](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals)です。特定の言語で、'special' を処理するものが必要ない場合`quantity`文字列は無視されます (たとえば、英語だけが使用`one`と`other`以外の場合は指定する、`zero`文字列は効果がなく、使用されません)。

### <a name="images"></a>イメージ

イメージのローカライズされた文字列ファイルと同じ規則に従います: に、アプリケーションで参照されているすべてのイメージを配置する必要があります**ドロウアブル**ディレクトリがフォールバックします。

ロケール固有のイメージはし、修飾ドロウアブル フォルダーなど**ドロウアブル es**または**ドロウアブル日本**(dpi 指定子も追加できます)。

このスクリーン ショットでは 4 つのイメージに保存されます、**ドロウアブル**ディレクトリが 1 人だけ、 **flag.png**に合わせてローカライズその他のディレクトリにコピーします。

![それぞれ 1 つまたは複数のローカライズされた .png ファイルを含む複数のドロウアブル フォルダーのスクリーン ショット](localization-images/drawable.png)


#### <a name="other-resource-types"></a>その他のリソースの種類

また、言語固有のリソースのレイアウト、アニメーション、および raw ファイルを含む、他の種類の代わりを指定することもできます。 この場合は、対象言語の 1 つ以上の特定の画面のレイアウトを指定することが、たとえばでしたレイアウトを作成する、専用では、非常に長いテキスト ラベルをドイツ語にします。

Android 4.2 には、サポートが導入されました。[右から左 (RTL) 言語](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html)アプリケーション設定を設定した場合`android:supportsRtl="true"`です。 リソース修飾子`"ldrtl"`右から左へ表示用にデザインされたカスタム レイアウトを格納するディレクトリ名に含めることができます。

リソース ディレクトリの名前付けとフォールバックの詳細については、Android ドキュメントを参照してください[代替のリソースを提供する](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)です。


### <a name="app-name"></a>アプリ名

アプリケーション名が簡単にローカライズを使用して、`@string/id`での`MainLauncher`アクティビティ。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>右から左 (RTL) の言語

Android 4.2 以降では、右から左のレイアウトで詳しく説明の完全サポート、[右から左へのネイティブ サポート ブログ](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html)です。

Android 4.2 (API レベル 17) を使用する場合とで指定できる値は、新しい揃え`start`と`end`の代わりに`left`と`right`(たとえば`android:paddingStart`)。 あるような新しい Api `LayoutDirection`、 `TextDirection`、および`TextAlignment`RTL リーダーの対応する画面の作成に役立ちます。

次のスクリーン ショットでは、[ローカライズ**Tasky**サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)アラビア語で。

[![アラビア語の Tasky アプリのスクリーン ショット](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

次のスクリーン ショットでは、[ローカライズ**Tasky**サンプル](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)ヘブライ語で。

[![ヘブライ語で Tasky アプリのスクリーン ショット](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

使用して右から左へテキストをローカライズ**Strings.xml** LTR テキストと同じ方法でファイル。

## <a name="testing"></a>テスト中

既定のロケールを十分にテストすることを確認してください。 アプリケーションがクラッシュする場合は、何らかの理由 (つまりが欠落している) 既定のリソースを読み込むことができません。

### <a name="emulator-testing"></a>エミュレーターのテスト

Google を参照してください[Android エミュレーターでテスト](https://developer.android.com/guide/topics/resources/localization.html#testing)エミュレーターが ADB シェルを使用して特定のロケールに設定する方法についてのセクションでします。

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>デバイスのテスト

デバイスでテストする、言語を変更する、**設定**アプリ。
**ヒント:**言語設定を元に戻すことができるように、メニュー項目の場所と、アイコンのメモを作成します。


## <a name="summary"></a>まとめ

この記事では、組み込みのリソースの処理を使用した Android アプリケーションのローカライズの基本について説明します。 IOS、Android および (Xamarin.Forms) を含むクロスプラット フォーム アプリでの i18n および L10n の詳細を学習できます[このクロスプラット フォーム ガイド](~/cross-platform/app-fundamentals/localization.md)です。



## <a name="related-links"></a>関連リンク

- [(コードのローカライズ版) Tasky (サンプル)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android のリソースをローカライズします。](http://developer.android.com/guide/topics/resources/localization.html)
- [クロスプラット フォームのローカリゼーションの概要](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms ローカリゼーション](~/xamarin-forms/app-fundamentals/localization.md)
- [iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)
