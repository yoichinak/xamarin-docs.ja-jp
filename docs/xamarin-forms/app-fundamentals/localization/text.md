---
title: 文字列とイメージのローカライズ
description: Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: 09fe3587e4e435383822e50bd12616747b807f82
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108458"
---
# <a name="localization"></a>ローカリゼーション

_Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。_

## <a name="overview"></a>概要

.NET アプリケーションの使用をローカライズするための組み込みメカニズム[RESX ファイル](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx)クラスとクラスの`System.Resources`と`System.Globalization`名前空間。 翻訳された文字列を含む RESX ファイルは、コンパイラによって生成されたクラスの翻訳を厳密に型指定されたアクセスを提供すると共に、Xamarin.Forms アセンブリに埋め込まれます。 翻訳されたテキストは、コードで取得できます。

### <a name="sample-code"></a>サンプル コード

このドキュメントに関連付けられている 2 つのサンプルがあります。

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization)説明する概念の非常に簡潔に説明します。 次に示すコード スニペットでは、このサンプルからすべてです。
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized)はこれらのローカリゼーションの手法を使用する基本的な作業用アプリ。

#### <a name="shared-projects-are-not-recommended"></a>共有プロジェクトは推奨されません。

TodoLocalized サンプルが含まれています、[共有プロジェクト デモ](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/)ビルド システムの制限により、リソース ファイルを取得できません操作を行いますが、 **. designer.cs**にアクセスする能力が損なわれますが、生成されたファイル厳密に型指定されたコードに翻訳された文字列。

このドキュメントの残りの部分は、Xamarin.Forms .NET Standard ライブラリ テンプレートを使用してプロジェクトに関連しています。

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms コードのグローバル化

**グローバライズ**アプリケーションは、プロセスの「世界中の準備完了」になります これは、さまざまな言語を表示することは、コードの記述を意味します。

あるように、ユーザー インターフェイスを構築、アプリケーションのグローバル化の重要な部分の 1 つがない*ハード コーディングされた*テキスト。 代わりに、その選択した言語に翻訳された文字列のセットから何も、ユーザーに表示される取得する必要があります。

このドキュメントで RESX ファイルを使用して、これらの文字列を格納し、ユーザーの設定に応じて表示を取得する方法を説明します。

サンプルでは、英語、フランス語、スペイン語、ドイツ語、中国語、日本語、ロシア語、およびブラジルのポルトガル語の言語を対象します。 アプリケーションは、必要に応じていくつか、または多くの言語に翻訳することができます。

> [!NOTE]
> ユニバーサル Windows プラットフォームでは、RESX ファイルではなく、プッシュ通知のローカライズ化ファイルを使用してください。 詳細については、次を参照してください。 [UWP ローカリゼーション](/windows/uwp/design/globalizing/globalizing-portal/)します。

### <a name="adding-resources"></a>リソースを追加します。

最初の Xamarin.Forms .NET Standard ライブラリ アプリケーションのグローバライズ手順は、アプリで使用されるすべてのテキストを格納するために使用する RESX リソース ファイルを追加しています。 既定のテキストを含む RESX ファイルを追加し、各言語をサポートする追加の RESX ファイルを追加する必要があります。

#### <a name="base-language-resource"></a>基本言語リソース

基本のリソース (RESX) ファイル (サンプルは、英語は既定の言語と仮定)、既定の言語識別文字列が含まれます。 Xamarin.Forms の一般的なコード プロジェクトにプロジェクトを右クリックしを選択して、ファイルを追加**追加 > 新しいファイル.**.

わかりやすい名前を選択します。 **AppResources**キーを押します**OK**します。

[![リソース ファイルを追加](text-images/resx-new-file-sml.png "新しいファイル ダイアログ")](text-images/resx-new-file.png#lightbox "新しいファイル ダイアログ")

2 つのファイルは、プロジェクトに追加されます。

* **AppResources.resx**ファイルに変換可能な文字列が、XML 形式で格納されます。
* **AppResources.designer.cs** RESX XML ファイルで作成されたすべての要素への参照を格納する部分クラスを宣言するファイル。

関連ファイルは、ソリューション ツリーが表示されます。 RESX ファイル*する必要があります*変換可能な新しい文字列を追加するには、次のように編集、 **. designer.cs**ファイルに*いない*編集します。

![](text-images/appresources-tree.png "AppResources.resx ファイル")

##### <a name="string-visibility"></a>文字列の表示

既定では文字列への厳密に型指定された参照が生成されるようになります`internal`アセンブリ。 RESX ファイルの既定のビルド ツールが生成されますので、これは、 **. designer.cs**ファイルと`internal`プロパティ。

選択、 **AppResources.resx**ファイルし、表示、**プロパティ**パッドがこのビルド ツールの場所を構成します。 以下のスクリーン ショット、**カスタム ツール: ResXFileCodeGenerator**します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "AppResources.Resx のプロパティ ウィンドウ")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Properties Pad AppResources.Resx の")](text-images/xs-resx-internal.png#lightbox)

-----

厳密に型指定された文字列プロパティ`public`、構成を手動で変更する必要があります**カスタム ツール: PublicResXFileCodeGenerator**以下のスクリーン ショットに示すように。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "AppResources.Resx のプロパティ ウィンドウ")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Properties Pad AppResources.Resx の")](text-images/xs-resx-internal.png#lightbox)


[![](text-images/xs-resx-public-sml.png "Properties Pad AppResources.Resx の")](text-images/xs-resx-public.png#lightbox)

-----

この変更は省略可能、およびのみが必要 (たとえば、別のアセンブリに RESX ファイルを追加し、コードに対する) 場合は、別のアセンブリ間でローカライズされた文字列を参照するかどうか。 このトピックのサンプルは、文字列のまま`internal`が使用されている同じ Xamarin.Forms .NET Standard ライブラリ アセンブリで定義されているためです。

上記のように、基本 RESX ファイルでカスタム ツールを設定するだけで済みます設定する必要はありません*任意*以下のセクションで説明されている言語に固有の RESX ファイルのビルド ツール。

##### <a name="editing-the-resx-file"></a>RESX ファイルの編集

残念なことはありません組み込み RESX エディターでは、Visual Studio for mac です。 新しい変換可能な文字列を追加するには、新しい XML の追加が必要です`data`各文字列の要素。 各`data`要素は、次を含めることができます。

* `name` (必須) 属性は、この変換可能な文字列のキーです。 有効な場合がありますC#プロパティ名のため、スペースや特殊文字は許可されません。
* `value` 要素 (必須) は、アプリケーションに表示される実際の文字列です。
* `comment` 要素 (省略可能) は、この文字列を使用する方法について説明する変換の手順を含めることができます。
* `xml:space` 属性の文字列に空白文字を保存する方法を制御する (省略可能)。

例をいくつか`data`要素が次に示します。

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

新しい基本 RESX リソース ファイルへすべてのユーザーに表示されるテキストを追加する必要があります、アプリケーションが書き込まれると`data`要素。 含めることをお勧め`comment`として高品質の翻訳を確認することも可能です。

> [!NOTE]
> (無料の Community edition など)、visual Studio には、基本的な RESX エディターが含まれています。 Windows コンピューターへのアクセスがある場合は、便利な方法を追加して RESX ファイル内の文字列を編集することができます。

#### <a name="language-specific-resources"></a>言語固有のリソース

通常、既定のテキスト文字列の実際の変換は行われません (この場合既定 RESX ファイルが多数含まれている文字列の) アプリケーションの大きなチャンクが書き込まれたまで。 引き続き、必要に応じてローカライズ コードをテストするための機械翻訳されたテキストを設定、開発サイクルの早い段階で、言語固有のリソースを追加することをお勧めします。

サポートする言語ごとに 1 つの RESX ファイルが追加されます。
言語固有のリソース ファイルは、特定の名前付け規則に準拠する必要があります。 同じファイル名を使用して、基本のリソース ファイル (例。 **AppResources**) の後にピリオド (.) と、言語コード。 簡単な例は次のとおりです。

* **AppResources.fr.resx** -フランス語の言語の翻訳します。
* **AppResources.es.resx** -スペイン語の翻訳します。
* **AppResources.de.resx** -ドイツ語の言語に翻訳します。
* **AppResources.ja.resx** -日本語の言語の翻訳します。
* **AppResources.zh Hans.resx** - 中国語 (簡体字) の言語の翻訳します。
* **AppResources.zh Hant.resx** - 中国語 (繁体) の言語の翻訳します。
* **AppResources.pt.resx** -ポルトガル語の言語の翻訳します。
* **AppResources.pt BR.resx** -ポルトガル語 (ブラジル) の言語の翻訳します。

一般的なパターンが 2 文字の言語のコードを使用するには、別の形式が使用されるいくつかの例 (中国語) などやがブラジルのポルトガル語) などの他の例 4 文字のロケール識別子が必要な場合。

これらの言語に固有のリソース ファイル*しない*を必要とする **. designer.cs**部分クラスで、通常の XML ファイルとして追加できるように、**ビルド アクション: EmbeddedResource**を設定します。 このスクリーン ショットは、言語固有のリソース ファイルを含むソリューションを示しています。

![](text-images/appresources-langs.png "言語固有のリソース ファイル")

アプリケーションを開発し、基本の RESX ファイルが追加されたテキスト、する必要がありますに送信翻訳者が、各変換`data`要素と戻り値 (に示す名前付け規則を使用して) 言語固有のリソース ファイル、アプリに含まれます。 '機械翻訳' 例をいくつかは、以下に示します。

**AppResources.es.resx (スペイン語)**

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

のみ、 `value` - 変換プログラムで更新する必要がある要素、`comment`を変換するものではありません。 注意してください。 エスケープするための XML ファイルを編集するときに予約文字などの`<`、 `>`、`&`で`&lt;`、 `&gt;`、および`&amp;`に表示される場合、`value`または`comment`します。

<a name="incode" />

### <a name="using-resources-in-code"></a>コードでリソースの使用

RESX リソース ファイル内の文字列がユーザー インターフェイスを使用してコードで使用できる、`AppResources`クラス。 `name` RESX 内の各文字列に割り当てられているファイルは次のように、Xamarin.Forms のコードで参照できますが、そのクラスのプロパティになります。

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

ハードコーディングされているのではなく、テキストが、そのリソースから読み込まれるため、アプリを複数の言語に翻訳することはこれを除くには、iOS、Android、および、ユニバーサル Windows プラットフォーム (UWP) として表示されますが、ユーザー インターフェイスが予想どおり、します。 変換する前に、各プラットフォームで UI を示すスクリーン ショットを次に示します。

![](text-images/simple-example-english.png "変換する前にクロス プラットフォームの Ui")

### <a name="troubleshooting"></a>トラブルシューティング

#### <a name="testing-a-specific-language"></a>特定の言語のテスト

異なるカルチャをすばやくテストするときに、開発中に特に、さまざまな言語に、シミュレーターまたはデバイスを切り替えるには注意が必要になります。

設定によって読み込まれる特定の言語を強制することができます、`Culture`このコード スニペットで示すようにします。

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

このアプローチは直接のカルチャの設定 –、`AppResources`クラス-(デバイスのロケールを使用するのではなく)、アプリ内の言語セレクターを実装するためにも使用できます。

#### <a name="loading-embedded-resources"></a>埋め込みリソースの読み込み

次のコード スニペットは、(RESX ファイル) などの埋め込みリソースで問題をデバッグするときに便利です。 (早い段階でアプリのライフ サイクル) アプリにこのコードを追加し、完全なリソース識別子を示す、アセンブリに埋め込まれているすべてのリソースが一覧表示されます。

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

**AppResources.Designer.cs**ファイル (周囲*33 行*)、完全な*リソース マネージャー名*(例: が指定されて `"UsingResxLocalization.Resx.AppResources"`) 次のコードに似ています。

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

チェック、**アプリケーション出力**デバッグ コードを上記の結果を得ることを確認するに適切なリソースが表示 (つまり`"UsingResxLocalization.Resx.AppResources"`).

ない場合、`AppResources`クラスがそのリソースを読み込むことができません。
リソースが見つかりません、問題を解決するのには、次を確認します。

* プロジェクトの既定の名前空間に一致するルート名前空間、 **AppResources.Designer.cs**ファイル。
* 場合、 **AppResources.resx**ファイルがサブディレクトリにある、サブディレクトリ名が、名前空間の一部にする必要があります*と*リソース識別子の一部です。
* **AppResources.resx**ファイルが**ビルド アクション: EmbeddedResource**します。
* **プロジェクト オプション > ソース コード > .NET の命名ポリシー > リソースの名前を使用して Visual Studio スタイル**がオンになっています。 これを使用する場合でも untick、ただし、名前空間、RESX リソースを参照するときに使用する必要がありますが、アプリ全体で更新します。

#### <a name="doesnt-work-in-debug-mode-android-only"></a>デバッグ モード (Android のみ) では機能しません

翻訳された文字列は、デバッグ中にありませんが、Android のリリース ビルドで作業している場合を右クリックし、 **Android プロジェクト**選択と**オプション > ビルド > Android のビルド**いることを確認し、**アセンブリの配置を高速**がオンになっていません。 このオプションは、リソースの読み込みで問題が発生し、ローカライズされたアプリをテストする場合は使用する必要があります。

### <a name="displaying-the-correct-language"></a>適切な言語を表示します。

これまでに、翻訳を指定することができます、ようにコードを記述する方法について説明しましたが、実際に表示するために方法ではありません。 Xamarin.Forms コードは、利用できます。NET のリソースを適切な言語の翻訳を読み込む、ユーザーが選択した言語を決定するには、各プラットフォームのオペレーティング システムのクエリを実行する必要しますが、あります。

いくつかのプラットフォーム固有のコードは、ユーザーの言語設定を取得するために必要なために、使用、[依存関係サービス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)Xamarin.Forms アプリでその情報を公開し、各プラットフォームの実装します。

最初に、ユーザーの優先カルチャ、次のコードのようなを公開するインターフェイスを定義します。

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

次に、使用、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 、Xamarin.Forms で`App`クラス、インターフェイスを呼び出すし、正しい値に、RESX リソース カルチャを設定します。 手動でこの値、ユニバーサル Windows プラットフォームに対してリソース フレームワークから自動的に設定する必要があることに注意してくださいでは、これらのプラットフォームで、選択した言語を認識します。

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

リソース`Culture`正しい言語の文字列が使われるように、アプリケーションが初めて読み込まれるときに設定する必要があります。 必要に応じて、に従って、アプリの実行中に、ユーザーがその言語の設定を更新する場合に iOS または Android で発生した可能性がプラットフォーム固有のイベントには、この値を更新する場合があります。

実装、`ILocalize`インターフェイスに表示されます、[プラットフォーム固有コード](#Platform-specific_Code)以下のセクション。 これらの実装この利用`PlatformCulture`ヘルパー クラス。

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

コードを表示する言語を検出するためには、iOS、Android、および UWP は、少し異なる方法でこの情報を公開するため、プラットフォーム固有にすることがあります。 コードを`ILocalize`依存関係サービスが以下の各プラットフォームでは、ことを確認する追加のプラットフォームに固有の要件とローカライズされたテキストが正しくレンダリングされる用意されています。

プラットフォーム固有のコードは、オペレーティング システムは、可能ではサポートされていないロケール識別子を構成するユーザーの場合を処理もする必要があります。NET の`CultureInfo`クラス。 このような場合は、サポートされていないロケールを検出し、最適なを置き換えるカスタム コードを記述する必要があります。NET と互換性のあるロケール。

#### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS ユーザーは、日付と時刻の書式のカルチャから個別に好みの言語を選択します。 クエリを実行するだけの Xamarin.Forms アプリをローカライズする適切なリソースを読み込む、`NSLocale.PreferredLanguages`最初の要素の配列。

次の実装、`ILocalize`依存関係サービスは、iOS application プロジェクトに配置する必要があります。 コードをインスタンス化する前に、アンダー スコアに置き換えられた iOS は、ダッシュ (つまり、.NET の標準的な表現) の代わりにアンダー スコアを使用するため、`CultureInfo`クラス。

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
                case "ms-SG":    // "Malaysian (Singapore)" not supported .NET culture
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
> `try/catch`内のブロック、`GetCurrentCultureInfo`メソッドは、完全一致が見つからない場合、検索言語 (ロケール内の文字の最初のブロック) に一致するロケールの指定子: で通常使用するフォールバック動作を模倣します。
>
> Xamarin.Forms の場合、いくつかのロケールは iOS で有効なが有効なに対応していない`CultureInfo`.NET、およびこれに対処する試行が上記のコードにします。
>
> たとえば、iOS**設定 > 一般的な言語&amp;リージョン**画面では、電話を設定することができます**言語**に**英語**が、 **リージョン**に**スペイン**、ロケール文字列になる`"en-ES"`します。 ときに、`CultureInfo`作成が失敗した、コードに戻って、最初の 2 つの文字だけを使用して、表示言語を選択します。
>
> 開発者を変更する必要があります、`iOSToDotnetLanguage`と`ToDotnetFallbackLanguage`はサポートされている言語の必要な特定のケースを処理するメソッド。


など、翻訳、iOS によって自動的にいくつかのシステム定義のユーザー インターフェイス要素がある、**完了**のボタンでは、`Picker`コントロール。 IOS でサポートされる言語を指定する必要があります。 これらの要素の変換を強制する、 **Info.plist**ファイル。 使用してこれらの値を追加する**Info.plist > ソース**次のようにします。

![Info.plist キーをローカライズ](text-images/info-plist.png "Info.plist でローカライズされたキー")

または、開く、 **Info.plist** XML エディターでファイルを開き、値を直接編集します。

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
> もわずかに異なる Apple 扱いますポルトガル語が期待どおりに注意してください。
> [、Docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _「とを使用して pt 言語 ID としてポルトガル語、ブラジル、PT-PT で言語 ID としてポルトガル語ポルトガルで使用されるため、」_ します。
> つまり、ときに、フォールバック言語になりますブラジルのポルトガル語 ios では、この動作を変更するコードを記述しない限り、非標準のロケールでポルトガル語の言語が選択されて (など、`ToDotnetFallbackLanguage`上)。

IOS のローカリゼーションの詳細については、次を参照してください。 [iOS ローカリゼーション](~/ios/app-fundamentals/localization/index.md)します。

#### <a name="android-application-project"></a>Android アプリケーション プロジェクト

Android を使用して、現在選択されているロケールの公開`Java.Util.Locale.Default`、およびダッシュ (これは、次のコードに置き換え) ではなく、アンダー スコア区切り記号を使用します。 Android アプリケーション プロジェクトには、この依存関係サービスの実装を追加します。

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
> `try/catch`内のブロック、`GetCurrentCultureInfo`メソッドは、完全一致が見つからない場合、検索言語 (ロケール内の文字の最初のブロック) に一致するロケールの指定子: で通常使用するフォールバック動作を模倣します。
>
> いくつかのロケール、Xamarin.Forms の場合は Android で有効ですが、有効なに対応していない`CultureInfo`.NET、およびこれに対処する試行が上記のコードにします。
>
> 開発者を変更する必要があります、`iOSToDotnetLanguage`と`ToDotnetFallbackLanguage`はサポートされている言語の必要な特定のケースを処理するメソッド。

このコードは、Android アプリケーション プロジェクトに追加されるは、自動的に翻訳された文字列を表示することがあります。

> [!NOTE]
>️**警告:** 場合、翻訳された文字列は、デバッグ中にありませんが、Android のリリース ビルドで作業しているを右クリックし、 **Android プロジェクト**選択**オプション > ビルド > Androidビルド**いることを確認し、**アセンブリの配置を高速**がオンになっていません。 このオプションは、リソースの読み込みで問題が発生し、ローカライズされたアプリをテストする場合は使用する必要があります。

Android のローカリゼーションの概要の詳細については、次を参照してください。 [Android ローカリゼーション](~/android/app-fundamentals/localization.md)します。

#### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) プロジェクトでは、依存関係サービスは必要ありません。 代わりに、このプラットフォームに自動的にリソースのカルチャを正しく設定されます。

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

.NET Standard ライブラリ プロジェクトのプロパティのノードを展開しをダブルクリックして、 **AssemblyInfo.cs**ファイル。 アセンブリのニュートラル リソース言語を英語に設定するファイルには、次の行を追加します。

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

そのためことを確認する、言語ニュートラル RESX ファイルで定義されている文字列のアプリの既定のカルチャのリソース マネージャーに通知 (**AppResources.resx**) が、アプリを英語版のロケールのいずれかで実行するときに表示されます。

### <a name="example"></a>例

プラットフォーム固有プロジェクトに示すように上記の更新、RESX ファイルを翻訳して、アプリを再コンパイルすると、更新された翻訳は各アプリで利用できるになります。 簡体字中国語に変換するサンプル コードのスクリーン ショットを次に示します。

![](text-images/simple-example-hans.png "簡体中国語に変換されたクロスプラット フォーム Ui")

UWP のローカリゼーションの概要の詳細については、次を参照してください。 [UWP ローカリゼーション](/windows/uwp/design/globalizing/globalizing-portal/)します。

## <a name="localizing-xaml"></a>XAML のローカライズ

ときに XAML で Xamarin.Forms ユーザー インターフェイスのマークアップを作成するようになります、文字列では、XML で直接埋め込まれます。

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

理想的には、ユーザー インターフェイスのコントロールを作成することで実行できる XAML で直接でした翻訳、*マークアップ拡張機能*します。 XAML に RESX リソースを公開するマークアップ拡張機能のコードは、以下に示します。 このクラスは、(XAML ページ) と共に、Xamarin.Forms の一般的なコードに追加する必要があります。

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

以下の項目には、上記のコードで重要な要素について説明します。

* クラスの名前は`TranslateExtension`、としてを参照できる規則により**Translate**マークアップでします。
* クラス実装`IMarkupExtension`、作業を Xamarin.Forms で必要です。
* `"UsingResxLocalization.Resx.AppResources"` RESX リソースのリソース識別子です。 それは、既定の名前空間、リソース ファイルが配置されるフォルダーと既定の RESX ファイル名で構成されます。
* `ResourceManager`を使用してクラスを作成`IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)`、リソースの読み込みには、現在のアセンブリを判断して、静的にキャッシュされている`ResMgr`フィールド。 として作成されます、`Lazy`で最初に使用されるまでの作成が延期されるように、入力、`ProvideValue`メソッド。
* `ci` 依存関係サービスを使用すると、ユーザーの選択した言語をネイティブのオペレーティング システムから取得されます。
* `GetString` リソース ファイルから実際の翻訳された文字列を取得する方法です。 ユニバーサル Windows プラットフォームで`ci`は null になりますので、`ILocalize`インターフェイスは、これらのプラットフォームで実装されていません。 これは、呼び出しに相当、`GetString`最初のパラメーターだけを持つメソッド。 代わりに、リソースのフレームワークを使用して、ロケールが認識自動的に、適切な RESX ファイルから翻訳後の文字列を取得します。
* エラー処理は例外をスローして不足しているリソースのデバッグに役立つに含まれています (で`DEBUG`モードのみ)。

次の XAML スニペットは、マークアップ拡張機能を使用する方法を示します。 機能させるために 2 つの手順があります。

1. カスタム宣言`xmlns:i18n`ルート ノードの名前空間。 `namespace`と`assembly`正確には、この例と同じですが、プロジェクトに異なる可能性があります、プロジェクトの設定に一致する必要があります。
2. 使用`{Binding}`を呼び出すテキストを含む通常の属性の構文、`Translate`マークアップ拡張機能。 リソース キーは、唯一の必須パラメーターです。

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

次より詳細な構文もマークアップ拡張機能の有効です。

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>プラットフォーム固有の要素のローカライズ

Xamarin.Forms コード内のユーザー インターフェイスの翻訳を処理しましたできますが、各プラットフォーム固有プロジェクトで行う必要があるいくつかの要素があります。 このセクションをローカライズする方法について説明します。

* Application Name
* イメージ

サンプル プロジェクトにはと呼ばれる、ローカライズされたイメージが含まれています**flag.png**で参照されているC#次のようにします。

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

フラグのイメージは、このような XAML で参照されても。

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

すべてのプラットフォームについて以下に説明するプロジェクトの構造が実装されている限り、イメージのローカライズ版に上記のようなイメージ参照が自動的に解決されます。

### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS はローカライズ プロジェクトをという名前の名前付け標準を使用または **.lproj**イメージと文字列のリソースを格納するディレクトリ。 これらのディレクトリは、アプリで使用されるイメージのローカライズ版を含めることができ、また、 **InfoPlist.strings**ファイル、アプリ名をローカライズするために使用できます。 IOS のローカリゼーションの詳細については、次を参照してください。 [iOS ローカリゼーション](~/ios/app-fundamentals/localization/index.md)します。

#### <a name="images"></a>イメージ

このスクリーン ショットは、言語固有の iOS サンプル アプリを示しています。 **.lproj**ディレクトリ。 スペイン語のディレクトリと呼ばれる**es.lproj**、既定のイメージのローカライズ版を含むだけでなく**flag.png**:

![](text-images/ios-resources.png "iOS のプロジェクト ディレクトリのローカライズ")

各言語のディレクトリのコピーを格納する**flag.png**、その言語のローカライズされました。 イメージが指定されていない場合は、オペレーティング システムが既定の言語の既定のディレクトリ内のイメージになります。 Retina を完全にサポートするには、指定する必要があります **@2x** と **@3x** 各イメージのコピー。

#### <a name="app-name"></a>アプリ名

内容、 **InfoPlist.strings**単一キー-の値だけをアプリ名を構成するには。

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

アプリケーションを実行すると、アプリの名前と、イメージはどちらもローカライズします。

![](text-images/ios-imageicon.png "iOS アプリのサンプル テキストとイメージのローカライズ")

### <a name="android-application-project"></a>Android アプリケーション プロジェクト

異なるを使用して、ローカライズされたイメージを格納するためのさまざまなスキームに依存して android **drawable**と**文字列**言語コードのサフィックスを持つディレクトリ。 4 文字のロケールのコードが (ZH-TW PT-BR など) に必要な場合は、Android が必要である、さらに注意してください**r** dash/前次のロケールの (例: コード。 zh rTW または pt rBR)。 Android のローカリゼーションの概要の詳細については、次を参照してください。 [Android ローカリゼーション](~/android/app-fundamentals/localization.md)します。

#### <a name="images"></a>イメージ

このスクリーン ショットは、Android、一部のサンプルのローカライズ ドローアブルと文字列。

![](text-images/android-resources.png "Android は、ドローアブルとディレクトリを文字列のローカライズ")

Android が Zh-hans を使用しないことに注意してくださいと簡体字と繁体字中国語; Zh-hant コード代わりに、これは特定の国コード ZH-CN、ZH-TW にのみサポートします。

高密度の画面のさまざまな解像度のイメージをサポートするために使用して追加の言語フォルダーを作成`-*dpi`サフィックスなど、**ドローアブル-es-mdpi**、**ドローアブル-es-xdpi**、 **ドローアブル-es-xxdpi**など。参照してください[代わりの Android リソースを提供する](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)詳細についてはします。

#### <a name="app-name"></a>アプリ名

内容、 **strings.xml**単一キー-の値だけをアプリ名を構成するには。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

更新プログラム、 **MainActivity.cs** Android アプリ プロジェクトのように、 `Label` XML 文字列を参照します。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

アプリは、イメージとアプリの名前をローカライズします。 (スペイン語) で結果のスクリーン ショットを次に示します。

![](text-images/android-imageicon.png "Android のサンプル アプリのテキストとイメージのローカライズ")

### <a name="universal-windows-platform-application-projects"></a>ユニバーサル Windows プラットフォーム アプリケーション プロジェクト

ユニバーサル Windows プラットフォームでは、イメージとアプリ名のローカライズを簡素化するリソースのインフラストラクチャを所有します。 UWP のローカリゼーションの概要の詳細については、次を参照してください。 [UWP ローカリゼーション](/windows/uwp/design/globalizing/globalizing-portal/)します。

#### <a name="images"></a>イメージ

イメージは、次のスクリーン ショットに示すようにリソース固有のフォルダーに配置してローカライズできます。

![](text-images/uwp-image-folder-structure.png "UWP イメージ ローカライズ フォルダー構造")

実行時に、Windows リソース インフラストラクチャは、ユーザーのロケールに基づいて適切なイメージを選択します。

## <a name="summary"></a>まとめ

RESX ファイルと .NET グローバリゼーション クラスを使用して、Xamarin.Forms アプリケーションをローカライズできます。 別に少量のユーザーが設定してどのような言語を検出するためにプラットフォーム固有のコードでは、ローカライズ作業のほとんどは、一般的なコードで集中管理します。

イメージは、一般に、iOS と Android の両方で提供されるマルチ解像度のサポートを活用するためにプラットフォーム固有の方法で処理されます。

## <a name="related-links"></a>関連リンク

- [RESX Localization Sample](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized サンプル アプリ](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [クロスプラット フォームのローカリゼーション](~/cross-platform/app-fundamentals/localization.md)
- [iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
- [UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)
- [CultureInfo クラス (MSDN) を使用します。](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [検索して、特定のカルチャ (MSDN) のリソースの使用](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
