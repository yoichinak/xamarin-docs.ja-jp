---
title: "ローカリゼーション"
description: "Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: e04ea24883bdf1e29a538aaff92c555df8e1755f
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2018
---
# <a name="localization"></a>ローカリゼーション

_Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。_

## <a name="overview"></a>概要

.NET アプリケーションの使用をローカライズするための組み込みメカニズム[RESX ファイル](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx)と、クラス、`System.Resources`と`System.Globalization`名前空間。 翻訳された文字列を含む RESX ファイルは、コンパイラによって生成されたクラス、翻訳を厳密に型指定されたアクセスを提供すると共に、Xamarin.Forms アセンブリに埋め込まれます。 翻訳文字列は、コードで取得できます。

### <a name="sample-code"></a>サンプル コード

このドキュメントに関連付けられている 2 つのサンプルがあります。

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization)説明する概念の非常に簡潔に説明します。 次に示すコード スニペットは、このサンプルからすべてです。
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized)これらローカリゼーションの手法を使用する基本的な作業用アプリがします。

#### <a name="shared-projects-are-not-recommended"></a>共有プロジェクトは推奨されません。

TodoLocalized サンプルが含まれています、[共有プロジェクト デモ](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/)ビルド システムの制限のためのリソース ファイルが得られなかったが、 **. designer.cs**にアクセスする機能を中断するように生成されたファイル文字列を翻訳[厳密に型指定されたコードで](~/xamarin-forms/app-fundamentals/localization.md)です。

このドキュメントの残りの部分は、Xamarin.Forms PCL テンプレートを使用してプロジェクトに関連しています。

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms コードのグローバル化

**グローバライズ**アプリケーションが「world の準備完了」にするというプロセス これは、さまざまな言語を表示することは、コードの記述を意味します。

あるように、ユーザー インターフェイスを構築、アプリケーションのグローバル化の主要部分のいずれかがない*ハード コーディングされた*テキスト。 代わりに、その選択した言語に翻訳された文字列のセットから何も、ユーザーに表示される取得する必要があります。

このドキュメントで RESX ファイルを使用して、それらの文字列を格納され、ユーザーの設定に応じて表示を取得する方法について確認します。

サンプルは、英語、フランス語、スペイン語、ドイツ語、中国語、日本語、ロシア語、およびブラジル ポルトガル語の言語を対象します。 アプリケーションは、必要に応じてできるだけ少ないまたは多くの言語に翻訳することができます。

### <a name="adding-resources"></a>リソースを追加します。

Xamarin.Forms PCL アプリケーションのグローバル化の最初の手順は、アプリで使用されるすべてのテキストを格納するために使用する、RESX リソース ファイルを追加しています。 既定のテキストを含む RESX ファイルを追加し、サポート対象の各言語用の追加の RESX ファイルを追加する必要があります。

#### <a name="base-language-resource"></a>ベース言語リソース

基本リソース (RESX) ファイル (サンプルは、英語は既定の言語と仮定) 既定の言語識別文字列が含まれます。 Xamarin.Forms の一般的なコード プロジェクトにプロジェクトを右クリックしを選択するファイルを追加**追加 > 新しいファイル.**.

などのわかりやすい名前を選択**AppResources**とキーを押します**OK**です。

[![リソース ファイルを追加](localization-images/resx-new-file-sml.png "新しいファイル ダイアログ ボックス")](localization-images/resx-new-file.png#lightbox "新しいファイル ダイアログ ボックス")

2 つのファイルは、プロジェクトに追加されます。

* **AppResources.resx**ファイルを XML 形式に変換可能な文字列が格納されます。
* **AppResources.designer.cs** RESX XML ファイルで作成されたすべての要素への参照を格納する部分クラスを宣言するファイル。

ソリューション ツリー関連ファイルが表示されます。 RESX ファイル*必要があります*新しい変換可能な文字列; を追加するように編集する、 **. designer.cs**ファイルにする必要があります*いない*を編集します。

![](localization-images/appresources-tree.png "AppResources.resx File")

##### <a name="string-visibility"></a>文字列の可視性

既定では文字列への厳密に型指定された参照が生成されるようになります`internal`アセンブリにします。 これは、RESX ファイルの既定のビルド ツールによって生成されるため、 **. designer.cs**ファイルと`internal`プロパティです。

選択、 **AppResources.resx**ファイルし、表示、**プロパティ**このビルド ツールは、場所を表示するパッドを構成します。 次のスクリーン ショット、**カスタム ツール: ResXFileCodeGenerator**です。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-internal-sml.png "AppResources.Resx のプロパティ ウィンドウ")](localization-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "AppResources.Resx のプロパティの埋め込み")](localization-images/xs-resx-internal.png#lightbox)

-----

厳密に型指定された文字列プロパティ`public`、構成を手動で変更する必要があります**カスタム ツール: PublicResXFileCodeGenerator**次のスクリーン ショットに示すように。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-public-sml.png "AppResources.Resx のプロパティ ウィンドウ")](localization-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "AppResources.Resx のプロパティの埋め込み")](localization-images/xs-resx-internal.png#lightbox)


[![](localization-images/xs-resx-public-sml.png "AppResources.Resx のプロパティの埋め込み")](localization-images/xs-resx-public.png#lightbox)

-----

この変更は省略可能であり、のみ (たとえば、配置した場合、RESX ファイルを別のアセンブリをコードに) 異なるアセンブリ全体でローカライズされた文字列を参照する必要があるかどうかに必要です。 このトピックのサンプルは、文字列のまま`internal`が使用されている同じ Xamarin.Forms PCL アセンブリで定義されているためです。

のみ; 上記のように、基本の RESX ファイルにカスタム ツールを設定する必要があります。設定する必要はありません*任意*次のセクションで説明されている言語固有の RESX ファイルでのビルド ツール。

##### <a name="editing-the-resx-file"></a>RESX ファイルの編集

残念ながらありません組み込み RESX エディターが Visual studio for mac です。 新しい変換可能な文字列を追加する新しい XML の追加が必要`data`各文字列の要素。 各`data`要素は、次を含めることができます。

* `name` (必須) 属性は、この変換可能な文字列のキーです。 有効な c# プロパティ名のスペースまたは特殊文字が許可されていませんのでなければなりません。
* `value` 要素 (必須)、アプリケーションに表示される実際の文字列です。
* `comment` 要素 (省略可能) は、この文字列の使用方法を説明する変換プログラムの命令を含めることができます。
* `xml:space` 属性の文字列で表される間隔を保存する方法を制御する (省略可能) です。

いくつかの例`data`要素を挙げています。

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

新しい、ユーザーに表示されるテキストのすべてを基本の RESX リソース ファイルに追加する必要があります、アプリケーションが書き込まれると、`data`要素。 含めることをお勧め`comment`の高品質変換されるように、可能な限りです。

> [!NOTE]
> (無料の Community edition を含む)、visual Studio には、基本的な RESX エディターが含まれています。 Windows コンピューターにアクセスする場合は、追加し、RESX ファイル内の文字列を編集するための便利な方法をすることができます。

#### <a name="language-specific-resources"></a>言語固有のリソース

通常、(後者既定 RESX ファイルが多数含まれている文字列の) アプリケーションの大きなチャンクが書き込まれたまで、既定のテキスト文字列の実際の変換は行われません。 引き続き、必要に応じて、ローカリゼーション コードのテストを支援する機械翻訳されたテキストの読み込み、開発サイクルの早い段階で言語固有のリソースを追加することをお勧めします。

サポートする言語ごとに 1 つの追加 RESX ファイルが追加されます。
言語固有のリソース ファイルは、特定の名前付け規則に従う必要があります同じファイル名を使用して基本のリソース ファイル (。 **AppResources**) の後にピリオド (.) とし、言語コードです。 簡単な例を次のとおりです。

* **AppResources.fr.resx** -フランス語の言語の翻訳します。
* **AppResources.es.resx** -スペイン語の言語の翻訳します。
* **AppResources.de.resx** -ドイツ語の言語の翻訳します。
* **AppResources.ja.resx** -日本語の言語の翻訳します。
* **AppResources.zh Hans.resx** - 中国語 (簡体字) の言語の翻訳します。
* **AppResources.zh Hant.resx** - 中国語 (繁体字) の言語の翻訳します。
* **AppResources.pt.resx** -ポルトガル語の言語の翻訳します。
* **AppResources.pt BR.resx** -ポルトガル語 (ブラジル) の言語の翻訳します。

一般的なパターンは、2 文字の言語コードを使用するがある場合 (中国語) など、別の形式が使用されている例であり、ブラジルのポルトガル語) などその他の例 4 文字のロケール識別子が必要です。

これらの言語に固有のリソース ファイル*しない*を必要とする**. designer.cs**部分クラスで、通常の XML ファイルとして追加できるように、**ビルド アクション: EmbeddedResource**を設定します。 このスクリーン ショットは、言語固有のリソース ファイルを含むソリューションを示しています。

![](localization-images/appresources-langs.png "言語固有のリソース ファイル")

アプリケーションを開発し、基本の RESX ファイルが追加されたテキスト、する必要がありますに送ること翻訳それぞれは、変換者`data`要素と戻り値 (表示される名前付け規則を使用して) 言語固有のリソース ファイルをアプリに含めます。 '機械翻訳' 例をいくつか次に示します。

**AppResources.es.resx (Spanish)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (日本語)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt BR.resx (ブラジル ポルトガル語)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

のみ、 `value` - 変換プログラムで更新する必要がある要素、`comment`翻訳するものではありません。 注意してください。 エスケープするために XML ファイルを編集するときの予約文字などの`<`、 `>`、`&`で`&lt;`、 `&gt;`、および`&amp;`に表示される場合、`value`または`comment`です。

<a name="incode" />

### <a name="using-resources-in-code"></a>コードでリソースの使用

RESX リソース ファイル内の文字列は、ユーザー インターフェイスを使用してコードで使用できる、`AppResources`クラスです。 `name` RESX 内の各文字列に割り当てられているファイルは次のように、Xamarin.Forms コードで参照できますが、そのクラスのプロパティになります。

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

ハードコーディングされているのではなく、テキストが、そのリソースから読み込まれているため、アプリを複数の言語に翻訳することはこれを除く iOS、Android、および、Windows プラットフォームをレンダリングするとしてのユーザー インターフェイスが期待されます。 変換する前に各プラットフォームで UI を表示されたスクリーン ショットを次に示します。

![](localization-images/simple-example-english.png "変換する前にクロスプラット フォームの Ui")

### <a name="troubleshooting"></a>トラブルシューティング

#### <a name="testing-a-specific-language"></a>特定の言語のテスト

シミュレーターまたはデバイスをさまざまな言語、複数の異なるカルチャをすばやくテストするときに、開発時に particulary に切り替えるには手間がかかる場合があります。

特定の言語設定によって読み込まれるを強制することができます、`Culture`このコード スニペットに示すようにします。

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

直接、カルチャの設定 – このアプローチ、`AppResources`クラス – (デバイスのロケールを使用するのではなく)、アプリ内の言語セレクターの実装にも使用できます。

#### <a name="loading-embedded-resources"></a>埋め込みリソースの読み込み

次のコード スニペットは、埋め込まれたリソース (RESX ファイルなど) に関する問題をデバッグする場合に便利です。 (早い段階でアプリのライフ サイクル) アプリに次のコードを追加し、完全なリソース識別子を示す、アセンブリに埋め込まれているすべてのリソースが一覧表示します。

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

**AppResources.Designer.cs**ファイル (周囲*33 を行*)、完全*リソース マネージャー名*が (指定されています。 `"UsingResxLocalization.Resx.AppResources"`) 次のコードに似ています。

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

チェック、**アプリケーション出力**デバッグ コードが上記の結果、ことを確認する正しいリソースが一覧表示されます (ie です。`"UsingResxLocalization.Resx.AppResources"`).

ない場合、`AppResources`クラスがそのリソースを読み込むことができません。
リソースを見つけることはできませんの問題を解決するのには、次を確認します。

* プロジェクトの既定の名前空間のルート名前空間の一致、 **AppResources.Designer.cs**ファイル。
* 場合、 **AppResources.resx**ファイルがサブディレクトリにある、サブディレクトリ名が、名前空間の一部にする必要があります*と*リソース識別子の一部です。
* **AppResources.resx**ファイルが**ビルド アクション: EmbeddedResource**です。
* **プロジェクトのオプション > ソース コード > .NET Naming Policies > を使用して Visual Studio スタイル リソース名**にチェック マークします。 これをしたい場合でも untick、アプリ全体更新ただし、名前空間、RESX リソースを参照しているときに使用する必要があります。

#### <a name="doesnt-work-in-debug-mode-android-only"></a>(Android のみ)、デバッグ モードでは機能しません

翻訳された文字列を使用している Android リリース ビルドでは、デバッグ中にない場合を右クリックし、 **Android プロジェクト**選択**オプション > ビルド > Android ビルド**ことを確認して、**アセンブリの展開を高速**チェック マークがないです。 このオプションは、リソースの読み込みで問題が発生しは、ローカライズされたアプリをテストしている場合、使用できません。

### <a name="displaying-the-correct-language"></a>適切な言語を表示します。

これまで、翻訳を指定することができます、ようにコードを記述する方法について説明しましたが、実際に表示されるように作成する方法ではなくです。 Xamarin.Forms コード活用できます。NET のリソースを適切な言語の翻訳を読み込む、ユーザーが選択した言語を決定するには、各プラットフォームのオペレーティング システムのクエリを実行する必要があります。

一部のプラットフォーム固有のコードが必要なため、ユーザーの言語の優先順位を取得するを使用して、[依存関係サービス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)Xamarin.Forms アプリでは、その情報を公開し、プラットフォームごとに実装します。

最初に、ユーザーの優先カルチャ、次のコードのようなを公開するインターフェイスを定義します。

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

次に、使用、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 、Xamarin.Forms で`App`クラス、インターフェイスを呼び出すし、適切な値に、RESX リソースのカルチャを設定します。 手動でこの値の Windows Phone と、ユニバーサル Windows プラットフォーム リソース フレームワークから自動的に設定する必要があることに注意してくださいでは、これらのプラットフォームで、選択した言語を認識します。

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

リソース`Culture`正しい言語文字列を使用できるように、アプリケーションが初めて読み込まれるときに設定する必要があります。 必要に応じて、に従って、アプリの実行中に、ユーザーが、言語設定を更新する場合に iOS または Android で発生した可能性がプラットフォームに固有のイベントには、この値を更新する可能性があります。

実装、`ILocalize`にインターフェイスが表示されます、[プラットフォーム固有のコード](#Platform-specific_Code)以下のセクションです。 このような実装を利用`PlatformCulture`ヘルパー クラス。

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>プラットフォーム固有のコード

コードを表示する言語を検出するためには、iOS、Android、および Windows プラットフォームは、少し異なる方法でこの情報を公開するため、プラットフォーム固有にすることがあります。 コードを`ILocalize`依存関係サービスは、の下の各プラットフォームのことを確認する追加のプラットフォームに固有の要件とローカライズされたテキストは正しくレンダリング提供します。

プラットフォーム固有のコードでは、オペレーティング システムが構成でサポートされていないはロケール識別子をできますケースは処理もする必要があります。NET の`CultureInfo`クラスです。 このような場合は、サポートされていないロケールを検出し、最適なを代用カスタム コードを記述する必要があります。NET と互換性のあるロケールです。

#### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS ユーザーは、日付と時刻のカルチャの書式設定から個別に使用する言語を選択します。 クエリを実行する必要がある Xamarin.Forms アプリをローカライズする正しいリソースを読み込む、`NSLocale.PreferredLanguages`最初の要素の配列。

次の実装、 `ILocalize` iOS アプリケーション プロジェクトの依存関係サービスを配置する必要があります。 コード化する前に、アンダー スコアに置き換えられた iOS は、ダッシュを使用する (.NET の標準的な表現) ではなくアンダー スコアを使用するため、`CultureInfo`クラス。

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch`内のブロック、`GetCurrentCultureInfo`メソッドが、完全一致が見つからない場合、検索 (ロケール内の文字の最初のブロック) の言語に一致するロケールの指定子: で通常使用するフォールバック動作を模倣します。
>
> Xamarin.Forms 場合いくつかのロケールは iOS では有効が、有効なに対応していない`CultureInfo`.NET、およびこれに対処する試行前のコードにします。
>
> たとえば、iOS**設定 > 一般的な言語&amp;地域**画面では、お客様の電話を設定することができます**言語**に**英語**ですが、 **地域**に**スペイン**、その結果は、ロケール文字列の`"en-ES"`します。 ときに、`CultureInfo`作成の失敗、コードはフォールバックして、最初の 2 つの文字だけを使用して、表示言語を選択します。
>
> 開発者が変更する必要があります、`iOSToDotnetLanguage`と`ToDotnetFallbackLanguage`サポートされる言語の必要な特定のケースを処理するメソッド。


自動的に変換して iOS などいくつかのシステム定義のユーザー インターフェイス要素がある、**完了**のボタンでは、`Picker`コントロール。 これらの要素ではサポートしている言語を指定する必要がありますを変換する iOS を強制的に、 **Info.plist**ファイル。 使用してこれらの値を追加する**Info.plist > ソース**次のようにします。

![Info.plist のローカリゼーション キー](localization-images/info-plist.png "Info.plist のローカライズされたキー")

または、開く、 **Info.plist** XML エディターでファイルし、値を直接編集します。

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

依存関係サービスを実装し、更新した後**Info.plist**、iOS アプリをローカライズされたテキストを表示することになります。

> [!NOTE]
> もわずかに異なる Apple 扱いますポルトガル語が想定されるように注意してください。
> [、Docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _「として使用して pt 言語 ID は、ポルトガル語として使用されるブラジルおよび PT-PT で言語 ID は、ポルトガル語のポルトガルで使用されていると」_です。
> つまり、ときに、フォールバック言語になりますブラジル ポルトガル語、iOS でこの動作を変更するコードを記述しない限り、非標準のロケールでポルトガル語の言語が選択されて (など、`ToDotnetFallbackLanguage`上)。

#### <a name="android-application-project"></a>Android アプリケーション プロジェクト

Android を使用して、現在選択されているロケールの公開`Java.Util.Locale.Default`とをダッシュ (これは、次のコードに置き換え) ではなくアンダー スコアの区切り記号を使用しています。 Android アプリケーション プロジェクトにこの依存関係サービスの実装を追加します。

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> `try/catch`内のブロック、`GetCurrentCultureInfo`メソッドが、完全一致が見つからない場合、検索 (ロケール内の文字の最初のブロック) の言語に一致するロケールの指定子: で通常使用するフォールバック動作を模倣します。
>
> Xamarin.Forms 場合いくつかのロケールは Android では有効が、有効なに対応していない`CultureInfo`.NET、およびこれに対処する試行前のコードにします。
>
> 開発者が変更する必要があります、`iOSToDotnetLanguage`と`ToDotnetFallbackLanguage`サポートされる言語の必要な特定のケースを処理するメソッド。


Android アプリケーション プロジェクトにこのコードを追加するは、翻訳された文字列を自動的に表示することがあります。

> [!NOTE]
>️**警告:**翻訳済みの文字列を使用している Android リリース ビルドでは、デバッグ中にない場合を右クリックし、 **Android プロジェクト**選択**オプション > ビルド > Androidビルド**ことを確認して、**アセンブリの展開を高速**チェック マークがないです。 このオプションは、リソースの読み込みで問題が発生しは、ローカライズされたアプリをテストしている場合、使用できません。

#### <a name="windows-application-projects"></a>Windows アプリケーション プロジェクト

Windows 8.1 とユニバーサル Windows プラットフォーム (UWP) のプロジェクトには、依存関係サービスは必要ありません – これらのプラットフォームでリソースのカルチャを正しく設定に自動的にします。

このドキュメントの後半で説明されている XAML マークアップ拡張機能を実装することがあります、 `ILocalize` Windows Phone の下に示すように実装します。

##### <a name="windows-phone-80"></a>Windows Phone 8.0

は使用されませんが、`App`クラスは、内部での Windows Phone の実装、`ILocalize`依存関係サービス。 このクラスを Windows Phone アプリ プロジェクトに追加します。後で説明されている XAML マークアップ拡張機能を実装する場合は必要になります。

```csharp
[assembly: Dependency(typeof(UsingResxLocalization.WinPhone.Localize))]

namespace UsingResxLocalization.WinPhone
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci) { }
        public System.Globalization.CultureInfo GetCurrentCultureInfo ()
        {
            return System.Threading.Thread.CurrentThread.CurrentUICulture;
        }
    }
}

```

Windows Phone 8.0 のプロジェクトは、ローカライズされたテキストを表示するのには適切に構成する必要があります。
プロジェクトのオプションでサポートされる言語を選択する必要があります*と*、 **WMAppManifest.xml**ファイル。
これらの設定が更新されない場合は、ローカライズされた RESX リソースは読み込まれません。

##### <a name="project-options"></a>プロジェクトのオプション

Windows Phone プロジェクトを右クリックし、選択**プロパティ**です。 **アプリケーション**ティックのタブ、**サポートされているカルチャ**アプリケーションがサポートします。

[![](localization-images/winphone-projectproperties-sml.png "プロジェクトのプロパティ - サポートされているカルチャ")](localization-images/winphone-projectproperties.png#lightbox "プロジェクト プロパティでサポートされているカルチャ")

##### <a name="wmappmanifestxml"></a>WMAppManifest.xml

Windows Phone プロジェクトの [プロパティ] ノードを展開し、ダブルクリック、 **WMAppManifest.xml**ファイル。 をクリックして、**パッケージング**タブし、目盛りをアプリケーションでサポートされているすべての言語です。

[![](localization-images/winphone-wmappmanifest-sml.png "WMAppManifest.xml - サポートされる言語")](localization-images/winphone-wmappmanifest.png#lightbox "WMAppManifest.xml - サポートされる言語")

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

ポータブル クラス ライブラリ (PCL) プロジェクトで、[プロパティ] ノードを展開し、ダブルクリック、 **AssemblyInfo.cs**ファイル。 ニュートラル リソース アセンブリ言語を英語に設定するファイルに次の行を追加します。

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

そのためことを確認する、文字列、言語ニュートラル RESX ファイルで定義されているアプリの既定のカルチャのリソース マネージャー通知 (**AppResources.resx**) がアプリを日本語のロケールのいずれかで実行するときに表示されます。

### <a name="example"></a>例

プラットフォーム固有のプロジェクトとして表示されている上記を更新して、翻訳済みの RESX ファイルで、アプリを再コンパイル後、は、更新された翻訳を各アプリケーションで使用可能なになります。 簡体字中国語に変換するサンプル コードのスクリーン ショットを次に示します。

![](localization-images/simple-example-hans.png "簡体字中国語に変換されるクロスプラット フォームの Ui")

## <a name="localizing-xaml"></a>XAML のローカライズ

ときに、マークアップの XAML で Xamarin.Forms ユーザー インターフェイスを作成ようになります、文字列とは、XML で直接埋め込まれます。

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

理想的には、ユーザー インターフェイスでコントロールを直接作成することで行うことができる XAML を変換おでした、*マークアップ拡張機能*します。 XAML に RESX リソースを公開するマークアップ拡張機能のコードは、以下に示します。 このクラスは、Xamarin.Forms の一般的なコード (XAML ページ) とに追加する必要があります。

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in XAML
    [ContentProperty("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci = null;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(
            () => new ResourceManager(ResourceId, IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly));

        public string Text { get; set; }

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public object ProvideValue(IServiceProvider serviceProvider)
        {
            if (Text == null)
                return string.Empty;

            var translation = ResMgr.Value.GetString(Text, ci);
            if (translation == null)
            {
#if DEBUG
                throw new ArgumentException(
                    string.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
#else
                translation = Text; // HACK: returns the key, which GETS DISPLAYED TO THE USER
#endif
            }
            return translation;
        }
    }
}
```

以下の項目では、上記のコードで重要な要素について説明します。

* クラスの名前は`TranslateExtension`、としてを参照すれば慣例**翻訳**マークアップでします。
* クラスを実装`IMarkupExtension`の機能を Xamarin.Forms で必要となります。
* `"UsingResxLocalization.Resx.AppResources"` RESX リソースのリソース識別子です。 これは、既定の名前空間、リソース ファイルが配置されているフォルダー、および既定の RESX ファイル名ので構成されます。
* `ResourceManager`を使用してクラスを作成`IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)`、リソースの読み込みを現在のアセンブリを特定し、静的でキャッシュされた`ResMgr`フィールドです。 として作成される、`Lazy`で最初に使用されるまでの作成が遅延されるように、入力、`ProvideValue`メソッドです。
* `ci` 依存関係サービスを使用して、ネイティブのオペレーティング システムからユーザーの選択した言語を取得します。
* `GetString` リソース ファイルから実際の翻訳された文字列を取得する方法です。 Windows Phone 8.1 とユニバーサル Windows プラットフォームに`ci`が null でないため、`ILocalize`インターフェイスは、これらのプラットフォームで実装されていません。 これは、呼び出すことと同じ、`GetString`最初のパラメーターだけを持つメソッドです。 代わりに、リソース フレームワークでは、ロケールを自動的に認識され、適切な RESX ファイルから翻訳した文字列を取得します。
* エラー処理が例外をスローして不足しているリソースをデバッグする際に含まれています (で`DEBUG`モードのみ)。

次の XAML スニペットは、マークアップ拡張機能を使用する方法を示します。 機能させる手順は次の 2 つです。

1. ユーザー設定の宣言`xmlns:i18n`ルート ノードの名前空間。 `namespace`と`assembly`正確には、この例では同一ですが、プロジェクトに異なる可能性がありますのプロジェクト設定に一致する必要があります。
2. 使用して`{Binding}`を呼び出すテキストが通常含まれる属性の構文、`Translate`マークアップ拡張機能です。 リソース キーは、唯一の必須パラメーターです。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

次より冗語構文もマークアップ拡張機能に対して有効です。

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>プラットフォーム固有の要素のローカライズ

Xamarin.Forms コード内のユーザー インターフェイスの翻訳お処理できますが、各プラットフォームに固有のプロジェクトで行う必要があるいくつかの要素があります。 このセクションをローカライズする方法について説明します。

* Application Name
* イメージ

サンプル プロジェクトと呼ばれるローカライズされたイメージが含まれています。 **flag.png**、次のように c# で参照されています。

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

フラグのイメージは、次のように、XAML で参照されても。

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

すべてのプラットフォームでは、以下に示すプロジェクトの構造が実装されている限り、イメージのローカライズ版に上記のようなイメージの参照は自動的に解決します。

### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS プロジェクトのローカリゼーションをという名前の名前付け標準を使用してまたは**.lproj**イメージと文字列のリソースを格納するディレクトリ。 これらのディレクトリは、アプリで使用されるイメージのローカライズ版を含めることができます、さらに、 **InfoPlist.strings**アプリ名をローカライズするために使用するファイル。

#### <a name="images"></a>イメージ

このスクリーン ショットは、言語固有の iOS のサンプル アプリを示しています。 **.lproj**ディレクトリ。 スペイン語のディレクトリと呼ばれる**es.lproj**、既定のイメージのローカライズ版を含むだけでなく**flag.png**:

![](localization-images/ios-resources.png "iOS のプロジェクト ディレクトリのローカライズ")

各言語のディレクトリのコピーを格納する**flag.png**、その言語のローカライズされました。 イメージが指定されていない場合、オペレーティング システムは既定の言語の既定のディレクトリにあるイメージをします。 指定する必要があります Retina を完全にサポートするには、  **@2x** と **@3x** 各イメージのコピー。

#### <a name="app-name"></a>アプリ名

内容、 **InfoPlist.strings**単一キー-の値だけをアプリ名を構成するのには。

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

このアプリケーションを実行すると、アプリの名前と、イメージが両方ローカライズされています。

![](localization-images/ios-imageicon.png "iOS アプリのサンプル テキストとイメージのローカライズ")

### <a name="android-application-project"></a>Android アプリケーション プロジェクト

Android のようなさまざまなを使用して、ローカライズされた画像を格納するためのさまざまなされます**ドロウアブル**と**文字列**言語コードのサフィックスを持つディレクトリ。 4 文字のロケールのコードが (ZH-TW PT-BR など) 必要な場合は、Android が必要である、さらに注意してください**r** dash/前述の次のロケールの (コード zh rTW または pt rBR)。

#### <a name="images"></a>イメージ

このスクリーン ショットに示しています、Android、一部のサンプル ドロウアブルと文字列のローカライズします。

![](localization-images/android-resources.png "Android は、ドロウアブルとディレクトリを文字列のローカライズ")

ただし、Android に zh Hans 使用されないと簡体字、繁体字中国語; のそれコード代わりに、これは特定の国コード ZH-CN、ZH-TW にのみサポートします。

高密度の画面のさまざまな解像度のイメージをサポートするために使用して追加の言語フォルダーを作成する`-*dpi`サフィックスなど**ドロウアブル es-mdpi**、**ドロウアブル es-xdpi**、 **ドロウアブル es-xxdpi**, などです。参照してください[代わりの Android リソースを提供する](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)詳細についてはします。

#### <a name="app-name"></a>アプリ名

内容、 **strings.xml**単一キー-の値だけをアプリ名を構成するのには。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

更新プログラム、 **MainActivity.cs** Android アプリのプロジェクトにできるように、 `Label` XML 文字列を参照します。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

アプリは、アプリの名前と画像をローカライズします。 (スペイン語) で結果のスクリーン ショットを次に示します。

![](localization-images/android-imageicon.png "Android のサンプル アプリのテキストとイメージのローカライズ")

### <a name="windows-phone-80-application-project"></a>Windows Phone 8.0 のアプリケーション プロジェクト

Windows Phone は、単純な組み込みの方法を特定のローカライズされたイメージを選択してもアプリの名前をローカライズするためがありません。

#### <a name="images"></a>イメージ

この制限を回避するサンプルは、修正案イメージの読み込みを使用してローカライズを導入する方法、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)の`Image`コントロール。

カスタム レンダラーのコードを次に示しますソースがある場合、`FileImageSource`これには、ファイル名を抽出し、使用して、ローカライズされたイメージへのパスを構築、し、`CurrentUICulture`です。 一部の言語には特別な処理が必要となるフォールバックも期待される役目です。例では、既定値は、いくつかの特殊なケースでを除く 2 文字の言語コードのみを使用するのには。

```csharp
using System.IO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinPhone;

[assembly: ExportRenderer(typeof(Image), typeof(UsingResxLocalization.WinPhone.LocalizedImageRenderer))]
namespace UsingResxLocalization.WinPhone
{
    public class LocalizedImageRenderer : ImageRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Image> e)
        {
            base.OnElementChanged(e);

            if (e.NewElement != null)
            {
                var s = e.NewElement.Source as FileImageSource;
                if (s != null)
                {
                    var fileName = s.File;
                    string ci = System.Threading.Thread.CurrentThread.CurrentUICulture.ToString();
                    // you might need some custom logic here to support particular cultures and fallbacks
                    if (ci == "pt-BR") {
                        // use the complete string 'as is'
                    } else if (ci == "zh-CN") {
                         // we could have named the image directories differently,
                         // but this keeps them consisent with RESX file naming
                        ci = "zh-Hans";
                    } else if (ci == "zh-TW" || ci == "zh-HK") {
                        ci = "zh-Hant";
                    } else {
                        // for all others, just use the two-character language code
                        ci = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName;
                    }
                    e.NewElement.Source = Path.Combine("Assets/" + ci + "/" + fileName);
                }
            }
        }
    }
}
```

このコードは、次に示すディレクトリ構造でのローカライズされたイメージとは動作します。 (特定のロケールを処理して、イメージが利用できないときにフォール) など、特定のローカライズの要件を満たすためのコードを変更することをお勧めします。

![](localization-images/winphone-resources.png "WinPhone は、イメージ ディレクトリ構造に合わせてローカライズ")

これで、Windows Phone は、イメージをローカライズします。 スペイン語と簡体字中国語) の「結果のスクリーン ショット次に示します。

![](localization-images/winphone-image-sml.png "WinPhone サンプル アプリのテキストとイメージのローカライズ")

#### <a name="app-name"></a>アプリ名

Microsoft のドキュメントを参照してください[Windows Phone 8.0 アプリのタイトルのローカライズ](http://msdn.microsoft.com/library/windows/apps/ff967550(v=vs.105).aspx)です。

### <a name="windows-phone-81-and-universal-windows-platform-application-projects"></a>Windows Phone 8.1 およびユニバーサル Windows プラットフォーム アプリケーション プロジェクト

Windows Phone 8.1 とユニバーサル Windows プラットフォームの双方に、リソースのインフラストラクチャをイメージとアプリ名のローカライズを簡略化します。

#### <a name="images"></a>イメージ

イメージは、次のスクリーン ショットに示すようにリソース固有のフォルダーに配置してローカライズできます。

![](localization-images/uwp-image-folder-structure.png "WinPhone 8.1 および UWP イメージ ローカリゼーション フォルダー構造")

実行時に、Windows リソース インフラストラクチャは、ユーザーのロケールに基づく適切なイメージを選択します。

#### <a name="app-name"></a>アプリ名

Microsoft のドキュメントを参照してください[Windows 8.1 ストア アプリ: ユーザーにアプリを記述する情報のローカライズ](https://msdn.microsoft.com/library/windows/apps/hh454044.aspx)と[アプリ マニフェストから文字列を読み込む](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323.aspx#loading_strings_from_the_app_manifest.)します。

## <a name="summary"></a>まとめ

RESX ファイルおよび .NET グローバリゼーションのクラスを使用して、Xamarin.Forms アプリケーションをローカライズできます。 別に、少量のプラットフォーム固有のコードでは、ユーザーが優先言語を検出するために、ほとんどのローカリゼーション リソースは、一般的なコードでの集中管理します。

イメージは一般に、iOS と Android の両方で提供されるマルチ解像度のサポートを活用するためにプラットフォーム固有の方法で処理されます。 Windows Phone には、クロス プラットフォーム フレンドリな方法では; でイメージをローカライズするカスタム コードが必要です。この機能を追加するサンプル コードが指定されました。


## <a name="related-links"></a>関連リンク

- [RESX Localization Sample](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [クロスプラット フォームのローカリゼーション](~/cross-platform/app-fundamentals/localization.md)
- [iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
- [CultureInfo クラス (MSDN) を使用します。](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [検索して、特定のカルチャ (MSDN) のリソースを使用します。](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
