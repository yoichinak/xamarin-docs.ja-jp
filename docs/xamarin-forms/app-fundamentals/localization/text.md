---
title: 文字列とイメージのローカライズ
description: Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: b17a1177abafe4e605263664038842863302ac3b
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249688"
---
# <a name="localization"></a>ローカリゼーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)

_Xamarin.Forms アプリは、.NET リソース ファイルを使用してローカライズできます。_

## <a name="overview"></a>概要

.NET アプリケーションをローカライズするための組み込みメカニズムでは、`System.Resources` および `System.Globalization` 名前空間にある [RESX ファイル](https://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx)とクラスが使われます。 翻訳された文字列を含む RESX ファイルは、厳密に型指定された翻訳へのアクセスを提供するコンパイラ生成のクラスと共に、Xamarin.Forms アセンブリに埋め込まれます。 その後、翻訳されたテキストをコードで取得できます。

### <a name="sample-code"></a>サンプル コード

このドキュメントに関連付けられているサンプルには、以下の 2 つがあります。

- [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization)。概念のとても簡単な説明です。 以下に示すコード スニペットはすべて、このサンプルのものです。
- [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized)。これらのローカライズ手法を使用する基本的な作業アプリです。

#### <a name="shared-projects-are-not-recommended"></a>共有プロジェクトは推奨されない

TodoLocalized サンプルには[共有プロジェクト デモ](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/)が含まれていますが、ビルド システムの制限により、リソース ファイルでは、コード内の厳密に型指定されて翻訳された文字列にアクセスする機能を中断させる、 **.designer.cs** ファイルは生成されません。

このドキュメントの残りの部分は、Xamarin.Forms .NET 標準ライブラリ テンプレートを使用するプロジェクトに関連します。

## <a name="globalizing-xamarinforms-code"></a>Xamarin.Forms コードのグローバル化

アプリケーションの**グローバル化**は、"国際化対応" のためのプロセスです。 これは、さまざまな言語を表示できるコードを書き込むことを意味します。

アプリケーションのグローバル化の重要な部分の 1 つは、テキストが*ハードコーディング* されないように、ユーザー インターフェイスを構築することです。 代わりに、ユーザーに対して表示されるすべてのものを、選択された言語に翻訳されている文字列セットから取得する必要があります。

このドキュメントでは、RESX ファイルを使用して、それらの文字列を格納し、ユーザーの設定に応じて表示するために取得する方法を説明します。

サンプルの対象となるのは、英語、フランス語、スペイン語、ドイツ語、中国語、日本語、ロシア語、ポルトガル語 (ブラジル) です。 必要に応じて、アプリケーションをいくつかの、または多くの言語に翻訳することができます。

> [!NOTE]
> ユニバーサル Windows プラットフォームでは、プッシュ通知のローカライズのために、RESX ファイルではなく、RESW ファイルを使用する必要があります。 詳細については、[UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)に関するページを参照してください。

### <a name="adding-resources"></a>リソースの追加

Xamarin.Forms .NET 標準ライブラリ アプリケーションをグローバル化する最初の手順は、アプリで使用するすべてのテキストを格納するために利用される RESX リソース ファイルを追加することです。 既定のテキストを含む RESX ファイルを追加してから、サポートする各言語の RESX ファイルをさらに追加する必要があります。

#### <a name="base-language-resource"></a>基本言語リソース

基本リソース (RESX) ファイルには、既定の言語文字列が含まれます (サンプルでは、英語が既定の言語であると仮定します)。 Xamarin.Forms の一般的なコード プロジェクトにファイルを追加する場合は、プロジェクトを右クリックし、 **[追加] > [新しいファイル...]** の順に選択します。

**AppResources** などのわかりやすい名前を選んで、 **[OK]** を押します。

[![リソース ファイルを追加する](text-images/resx-new-file-sml.png "[新しいファイル] ダイアログ")](text-images/resx-new-file.png#lightbox "[新しいファイル] ダイアログ")

次の 2 つのファイルがプロジェクトに追加されます。

- **AppResources.resx**ファイル。ここには、翻訳された文字列が XML 形式で格納されます。
- **AppResources.designer.cs** ファイル。ここでは、RESX XML ファイルに作成されたすべての要素への参照を含む部分クラスが宣言されます。

ソリューション ツリーは、ファイルが関連していることを示します。 新しい翻訳可能な文字列に追加するには、RESX ファイルを編集する*必要があります*。 **.designer.cs** ファイルは編集*しないでください*。

![](text-images/appresources-tree.png "AppResources.resx ファイル")

##### <a name="string-visibility"></a>文字列の可視性

既定では、文字列への厳密に型指定された参照が生成されると、アセンブリに対して `internal` になります。 これは、RESX ファイルの既定のビルド ツールによって、`internal` プロパティを使用して **.designer.cs** ファイルが生成されるためです。

**AppResources.resx** ファイルを選択し、 **[プロパティ]** パッドを表示して、このビルド ツールが構成されている場所を確認します。 以下のスクリーンショットには、**カスタム ツールがResXFileCodeGenerator** と示されています。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "AppResources.Resx の [プロパティ] ウィンドウ")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "AppResources.Resx の [プロパティ] パッド")](text-images/xs-resx-internal.png#lightbox)

-----

厳密に型指定された文字列プロパティを `public` にするには、以下のスクリーンショットに示すように、**カスタム ツールがPublicResXFileCodeGenerator** になるように構成を手動で変更する必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "AppResources.Resx の [プロパティ] ウィンドウ")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "AppResources.Resx の [プロパティ] パッド")](text-images/xs-resx-internal.png#lightbox)

[![](text-images/xs-resx-public-sml.png "AppResources.Resx の [プロパティ] パッド")](text-images/xs-resx-public.png#lightbox)

-----

この変更は省略可能であり、さまざまなアセンブリ全体でローカライズされた文字列を参照する場合 (コードに異なるアセンブリの RESX ファイルを配置する場合など) にのみ必須となります。 このトピックのサンプルでは文字列は `internal` のままにしておきます。これらが使用されるのと同じ Xamarin.Forms .NET 標準ライブラリ アセンブリで、そのように定義されているためです。

上記のように、基本的な RESX ファイルでカスタム ツールを設定するだけで済みます。以下のセクションで説明する言語固有の RESX ファイルでは、*いずれ* のビルド ツールも設定する必要はありません。

##### <a name="editing-the-resx-file"></a>RESX ファイルの編集

残念ながら、Visual Studio for Mac には組み込みの RESX エディターはありません。 新しい翻訳可能な文字列を追加するには、文字列ごとに新しい XML の `data` 要素を追加する必要があります。 各 `data` 要素には次のものを含めることができます。

- `name` 属性 (必須)。これは翻訳可能な文字列のキーです。 有効な C# プロパティ名にする必要があります。したがって、スペースや特殊文字は許可されません。
- `value` 要素 (必須)。これは、アプリケーションに表示される実際の文字列です。
- `comment` 要素 (省略可能)。これには、文字列の使用方法について説明する翻訳者に対する指示を含めることができます。
- `xml:space` 属性 (省略可能)。これにより、文字列の間隔を保持する方法が制御されます。

`data` 要素の例をいくつか以下に示します。

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

アプリケーションの書き込み時に、ユーザーに表示されるテキストのすべての部分を、新しい `data` 要素の基本的な RESX リソース ファイルに追加する必要があります。 高品質な翻訳を確保するために、できるだけ多くの `comment` を含めることをお勧めします。

> [!NOTE]
> Visual Studio (無償の Community エディションを含む) には、基本的な RESX エディターが含まれています。 Windows コンピューターにアクセスできる場合、それが RESX ファイルで文字列を追加および編集する便利な方法となることがあります。

#### <a name="language-specific-resources"></a>言語固有のリソース

通常、アプリケーションの大きなチャンクが書き込まれる (この場合、既定の RESX ファイルに多くの文字列が含まれる) まで、既定のテキスト文字列の実際の翻訳は行われません。 この場合も、開発サイクルの早い段階で言語固有のリソースを追加することをお勧めします。必要に応じて、機械翻訳されたテキストを取り込めば、ローカライズ コードのテストに役立ちます。

サポートする言語ごとに 1 つの RESX ファイルがさらに追加されます。
言語固有のリソース ファイルでは、特定の名前付け規則に従う必要があります。基本リソース ファイル ( **AppResources** など) と同じファイル名を使用します。その後にはピリオド (.) と言語コードが続きます。 簡単な例を以下に示します。

- **AppResources.fr.resx** - フランス語の言語翻訳。
- **AppResources.es.resx** - スペイン語の言語翻訳。
- **AppResources.de.resx** - ドイツ語の言語翻訳。
- **AppResources.ja.resx** - 日本語の言語翻訳。
- **AppResources.zh-Hans.resx** - 簡体字中国語の言語翻訳。
- **AppResources.zh-Hant.resx** - 繁体字中国語の言語翻訳。
- **AppResources.pt.resx** - ポルトガル語の言語翻訳。
- **AppResources.pt-BR.resx** - ポルトガル語 (ブラジル) の言語翻訳。

一般的なパターンでは、2 文字の言語コードを使用しますが、異なる形式が使用される例 (中国語など) がいくつかあり、また、4 文字のロケール識別子が必要な例 (ポルトガル語 (ブラジル) など) もあります。

これらの言語固有のリソース ファイルでは **.designer.cs** 部分クラスを必要と*しない* ため、**ビルド アクションをEmbeddedResource** に設定し、通常の XML ファイルとして追加することができます。 次のスクリーンショットには、言語固有のリソース ファイルを含むソリューションが示されています。

![](text-images/appresources-langs.png "言語固有のリソース ファイル")

アプリケーションが開発され、基本的な RESX ファイルにテキストが追加されたら、翻訳者に送信する必要があります。翻訳者は、各 `data` 要素を翻訳し、アプリに含める (示されている名前付け規則を使用する) 言語固有のリソース ファイルを返します。 "機械翻訳された" 例をいくつか以下に示します。

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

**AppResources.pt-BR.resx (ポルトガル語 (ブラジル))**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

翻訳者が更新する必要があるのは、`value` 要素のみです。`comment` は翻訳するためのものではありません。 注: `<`、`>`、`&` などの予約文字が `value` または `comment` に表示される場合、XML ファイルを編集し、`&lt;`、`&gt;`、および `&amp;` でエスケープします。

<a name="incode" />

### <a name="using-resources-in-code"></a>コードでのリソースの使用

RESX リソース ファイル内の文字列は、`AppResources` クラスを使用するユーザー インターフェイス コードで利用できます。 RESX ファイルの各文字列に割り当てられている `name` は、以下に示すように、Xamarin.Forms コードで参照できる、そのクラスのプロパティになります。

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) 上のユーザー インターフェイスは予期したとおりにレンダリングされます。ただし、テキストは、ハードコーディングされるのではなく、リソースから読み込まれるため、ここではアプリを複数の言語に翻訳することができます。 翻訳前の各プラットフォーム上の UI が表示されたスクリーンショットを以下に示します。

![](text-images/simple-example-english.png "翻訳前のクロスプラットフォーム UI")

### <a name="troubleshooting"></a>トラブルシューティング

#### <a name="testing-a-specific-language"></a>特定の言語のテスト

特に開発中に異なるカルチャをすばやくテストする必要がある場合、シミュレーターまたはデバイスを異なる言語に切り替えるのが難しいことがあります。

次のコード スニペットに示すように、`Culture` を設定することで、特定の言語を強制的に読み込むことができます。

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

この方法 (`AppResources` クラスに直接カルチャを設定する) は、(デバイスのロケールを使用するのではなく) アプリ内に言語セレクターを実装する場合にも使用できます。

#### <a name="loading-embedded-resources"></a>埋め込みリソースの読み込み

次のコード スニペットは、埋め込みリソース (RESX ファイルなど) に関する問題のデバッグを試行する場合に便利です。 (アプリのライフサイクルの早い段階で) アプリにこのコードを追加すると、アセンブリに埋め込まれているリソースがすべてリストされ、完全なリソース識別子が示されます。

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

**AppResources.Designer.cs** ファイル (*33 行目* 付近) では、以下のコードのような完全な*リソース マネージャー名* ( `"UsingResxLocalization.Resx.AppResources"` など) が指定されています。

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

**[アプリケーション出力]** で、上記のデバッグ コードの結果を調べ、正しいリソース (つまり、`"UsingResxLocalization.Resx.AppResources"`) がリストされていることを確認します。

ない場合は、`AppResources` クラスでそのリソースを読み込むことができません。
リソースが見つからない問題を解決するには、以下を確認します。

- プロジェクトの既定の名前空間と、**AppResources.Designer.cs** ファイル内のルート名前空間が一致している。
- **AppResources.resx** ファイルがサブディレクトリにある場合、そのサブディレクトリ名は、名前空間の一部であり、*かつ*、リソース識別子の一部である必要があります。
- **AppResources.resx** ファイルの**ビルド アクションがEmbeddedResource** となっている。
- **[プロジェクト オプション] > [ソース コード] > [.NET の命名ポリシー] > [Use Visual Studio-style resources names]\(Visual Studio スタイルのリソース名を使用\)** がオンになっている。 必要に応じて、これをオフにしてもかまいませんが、RESX リソースの参照時に使用される名前空間をアプリ全体で更新する必要があります。

#### <a name="doesnt-work-in-debug-mode-android-only"></a>デバッグ モードで動作しない (Android のみ)

翻訳された文字列が Android のリリース ビルドで動作しているが、デバッグ中は動作しない場合、 **[Android プロジェクト]** を右クリックし、 **[オプション] > [ビルド] > [Android のビルド]** の順に選択し、 **[迅速なアセンブリの配置]** がオフになっていることを確認します。 このオプションにより、リソースの読み込みに関する問題が発生するため、ローカライズされたアプリをテストする場合は使用しないでください。

### <a name="displaying-the-correct-language"></a>適切な言語の表示

ここまでは、翻訳を提供できるようにコードを書き込む方法について説明しましたが、実際の表示方法は説明していません。 Xamarin.Forms コードでは .NET のリソースを利用して、適切な言語翻訳を読み込むことはできますが、ユーザーが選択した言語を判別するには、各プラットフォーム上のオペレーティング システムに対してクエリを実行する必要があります。

ユーザーの言語設定を取得するためにいくつかのプラットフォーム固有のコードが必要であるため、[依存関係サービス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)を使用して、Xamarin.Forms アプリでその情報を公開し、プラットフォームごとに実装します。

最初に、以下のコードのような、ユーザーの優先するカルチャを公開するインターフェイスを定義します。

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

次に、Xamarin.Forms `App` クラスの [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) を使用して、インターフェイスを呼び出し、RESX リソース カルチャを適切な値に設定します。 リソース フレームワークではプラットフォーム上の選択された言語が自動的に認識されるため、ユニバーサル Windows プラットフォームに対して、この値を手動で設定する必要がないことに注意してください。

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

適切な言語文字列が使用されるようにするため、最初のアプリケーションでの読み込み時に、リソース `Culture` を設定する必要があります。 アプリの実行中にユーザーが言語設定を更新した場合に iOS や Android で発生する可能性のあるプラットフォーム固有のイベントに従い、この値を必要に応じて更新することがあります。

`ILocalize` インターフェイスの実装は、以下の「[プラットフォーム固有のコード](#Platform-specific_Code)」セクションで示されています。 これらの実装では、次の `PlatformCulture` ヘルパー クラスが利用されます。

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

表示する言語を検出するコードはプラットフォーム固有である必要があります。これは、iOS、Android、および UWP ではすべて、若干異なる方法でこの情報が公開されるためです。 `ILocalize` 依存関係サービスのコードは、以下のように、プラットフォームごとに提供されます。また、ローカライズされたテキストが正しくレンダリングされていることを確認するための追加のプラットフォーム固有の要件も示されます。

プラットフォーム固有のコードでは、オペレーティング システムでユーザーが、.NET の `CultureInfo` クラスでサポートされていないロケール識別子を構成できるというケースを処理する必要もあります。 このようなケースでは、カスタム コードを書き込み、サポートされていないロケールを検出し、最適な .NET 互換ロケールに置き換える必要があります。

#### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS ユーザーは、日付と時刻の形式のカルチャから個別に優先する言語を選択します。 Xamarin.Forms アプリをローカライズするために適切なリソースを読み込む場合は、最初の要素について、`NSLocale.PreferredLanguages` 配列のクエリを実行するだけで済みます。

`ILocalize` 依存関係サービスの次の実装は、iOS アプリケーション プロジェクトに配置する必要があります。 iOS では (.NET 標準の表現である) ダッシュではなく、アンダースコアを使用するため、コードで、`CultureInfo` クラスをインスタンス化する前にそのアンダースコアが置き換えられます。

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
            // .NET cultures don't support underscores
            string netLanguage = iOSLanguage.Replace("_", "-");

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
> `GetCurrentCultureInfo` メソッド内の `try/catch` ブロックでは、ロケール指定子で通常使用されるフォールバック動作が模倣されます。完全一致が見つからない場合は、言語 (ロケール内の文字の最初のブロック) のみに基づいて、近似一致を見つけます。
>
> Xamarin.Forms の場合、iOS でいくつかのロケールが有効ですが、.NET の有効な `CultureInfo` には対応しておらず、上記のコードでこの処理が試行されます。
>
> たとえば、iOS の **[設定] > [General Language &amp; Region]\(一般的な言語とリージョン\)** 画面では、電話の **[言語]** を **[英語]** に設定し、 **[リージョン]** を **[スペイン]** に設定できます。その結果、ロケール文字列は `"en-ES"` となります。 `CultureInfo` の作成に失敗すると、コードでは、表示言語を選択するために最初の 2 文字のみを使用するようにフォールバックします。
>
> 開発者は `iOSToDotnetLanguage` および `ToDotnetFallbackLanguage` メソッドを変更し、サポートされている言語に必要な特定のケースを処理する必要があります。

`Picker` コントロールの **[完了]** ボタンなど、iOS によって自動的に翻訳されるシステム定義のユーザー インターフェイス要素がいくつかあります。 iOS でこれらの要素を強制的に翻訳するには、**Info.plist** ファイルでサポートする言語を示す必要があります。 以下に示すように、 **[Info.plist] > [ソース]** を使用して、これらの値を追加できます。

![Info.plist のローカライズ キー](text-images/info-plist.png "Info.plist のローカライズ キー")

または、XML エディターで **Info.plist** ファイルを開き、次のように値を直接編集します。

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

依存関係サービスを実装し、**Info.plist** を更新したら、iOS アプリでローカライズされたテキストを表示できます。

> [!NOTE]
> Apple では、予期したものとは若干異なる方法でポルトガル語が扱われることに注意してください。
> [そのドキュメント](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2)からの引用: _"ブラジルで使用される場合は、ポルトガル語の言語 ID として pt を使用し、ポルトガルで使用される場合は、ポルトガル語の言語 ID として pt-PT を使用します"_ 。
> これは、非標準ロケールでポルトガルの言語が選択されると、(上記の `ToDotnetFallbackLanguage` など) この動作を変更するためにコードが書き込まれない限り、iOS ではフォールバック言語がポルトガル語 (ブラジル) になることを意味します。

iOS のローカライズについて詳しくは、[iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)に関するページを参照してください。

#### <a name="android-application-project"></a>Android アプリケーション プロジェクト

Android では、`Java.Util.Locale.Default` を使用して、現在選択されているロケールを公開します。また、(以下のコードで置き換えられる) ダッシュではなく、アンダースコア区切り文字が使用されます。 この依存関係サービスの実装を、Android アプリケーション プロジェクトに追加します。

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
> `GetCurrentCultureInfo` メソッド内の `try/catch` ブロックでは、ロケール指定子で通常使用されるフォールバック動作が模倣されます。完全一致が見つからない場合は、言語 (ロケール内の文字の最初のブロック) のみに基づいて、近似一致を見つけます。
>
> Xamarin.Forms の場合、Android でいくつかのロケールが有効ですが、.NET の有効な `CultureInfo` には対応しておらず、上記のコードでこの処理が試行されます。
>
> 開発者は `iOSToDotnetLanguage` および `ToDotnetFallbackLanguage` メソッドを変更し、サポートされている言語に必要な特定のケースを処理する必要があります。

このコードが Android アプリケーション プロジェクトに追加されたら、翻訳された文字列を自動的に表示できます。

> [!WARNING]
> 翻訳された文字列が Android のリリース ビルドで動作しているが、デバッグ中は動作しない場合、 **[Android プロジェクト]** を右クリックし、 **[オプション] > [ビルド] > [Android のビルド]** の順に選択し、 **[迅速なアセンブリの配置]** がオフになっていることを確認します。 このオプションにより、リソースの読み込みに関する問題が発生するため、ローカライズされたアプリをテストする場合は使用しないでください。

Android のローカライズについて詳しくは、「[Android のローカライズ](~/android/app-fundamentals/localization.md)」を参照してください。

#### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) プロジェクトでは、依存関係サービスは必要ありません。 代わりに、このプラットフォームで自動的にリソースのカルチャが正しく設定されます。

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

.NET 標準ライブラリ プロジェクトの [プロパティ] ノードを展開し、**AssemblyInfo.cs** ファイルをダブルクリックします。 次の行をファイルに追加し、ニュートラル リソース言語を英語に設定します。

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

これにより、アプリの既定のカルチャがリソース マネージャーに通知されるため、英語ロケールのいずれかでアプリが実行されているときに、言語に依存しない RESX ファイル (**AppResources.resx**) で定義された文字列が確実に表示されるようになります。

### <a name="example"></a>例

前述のようにプラットフォーム固有のプロジェクトを更新し、翻訳された RESX ファイルを使用してアプリを再コンパイルした後、更新された翻訳は各アプリで使用できるようになります。 簡体中国語に翻訳されたサンプル コードのスクリーンショットを以下に示します。

![](text-images/simple-example-hans.png "簡体字中国語に翻訳されたクロスプラットフォーム UI")

UWP のローカライズについて詳しくは、[UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)に関するページを参照してください。

## <a name="localizing-xaml"></a>XAML のローカライズ

XAML で Xamarin.Forms ユーザー インターフェイスを構築する場合、マークアップは次のようになり、XML に直接文字列が埋め込まれています。

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

XAML で直接ユーザー インターフェイス コントロールを翻訳できるのが理想的です。これは、*マークアップ拡張* を作成することで実行できます。 XAML に RESX リソースを公開するマークアップ拡張のコードを、以下に示します。 このクラスは、(XAML ページと共に) Xamarin.Forms の一般的なコードに追加する必要があります。

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

上記のコードの重要な要素については、以下のように箇条書きにして説明します。

- クラスには `TranslateExtension` という名前が付けられていますが、規則により、マークアップでは **Translate** と呼ぶことができます。
- クラスでは、Xamarin.Forms で動作させるために必要な、`IMarkupExtension` が実装されます。
- `"UsingResxLocalization.Resx.AppResources"` は、RESX リソースのリソース識別子です。 これは、既定の名前空間、リソース ファイルが配置されるフォルダーおよび既定の RESX ファイル名で構成されます。
- `ResourceManager` クラスは、リソースの読み込み元の現在のアセンブリを特定するために `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` を使用して作成され、静的な `ResMgr` フィールドにキャッシュされます。 これは `Lazy` 型として作成されるため、`ProvideValue` メソッドで最初に使用されるまで、その作成は延期されます。
- `ci` では依存関係サービスを使用して、ネイティブ オペレーティング システムからユーザーが選択した言語を取得します。
- `GetString` は、リソース ファイルから実際の翻訳された文字列を取得するメソッドです。 ユニバーサル Windows プラットフォームでは、`ILocalize` インターフェイスがそのプラットフォームに実装されないため、`ci` は null となります。 これは、最初のパラメーターのみを使用する `GetString` メソッドの呼び出しに相当します。 代わりに、リソース フレームワークでは自動的にロケールが認識され、適切な RESX ファイルから翻訳された文字列が取得されます。
- エラー処理は、(`DEBUG` モードのみで) 例外をスローして、欠落しているリソースをデバッグできるように含まれています。

次の XAML スニペットは、マークアップ拡張を使用する方法を示しています。 これを動作させるための手順は、次の 2 つです。

1. ルート ノードでカスタムの `xmlns:i18n` 名前空間を宣言します。 `namespace` と `assembly` は、プロジェクト設定と完全に一致する必要があります。この例では、これらは同一ですが、ご利用のプロジェクトでは異なる可能性があります。
2. 通常は `Translate` マークアップ拡張を呼び出すためのテキストが含まれる属性で、`{Binding}` 構文を使用します。 リソース キーは唯一の必須パラメーターです。

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

マークアップ拡張では、以下のより冗長な構文も有効です。

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>プラットフォーム固有の要素のローカライズ

Xamarin.Forms コードでユーザー インターフェイスの翻訳を処理できますが、プラットフォーム固有の各プロジェクトで行う必要がある要素がいくつかあります。 このセクションでは、以下のローカライズ方法について説明します。

- Application Name
- イメージ

サンプル プロジェクトには、次のような C# で参照される、**flag.png** というローカライズされたイメージが含まれています。

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

フラグ イメージは、次のような XAML でも参照されます。

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

以下に説明するプロジェクト構造体が実装されている限り、すべてのプラットフォームで、このようなイメージ参照がローカライズされたバージョンのイメージに自動的に解決されます。

### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS では、イメージおよび文字列リソースを含めるために、ローカライズ プロジェクト (つまり、 **.lproj**) ディレクトリという命名標準が使用されます。 これらのディレクトリには、アプリで使用されるローカライズされたバージョンのイメージを含めることができます。また、アプリ名をローカライズするために使用できる、**InfoPlist.strings** ファイルも含めることができます。 iOS のローカライズについて詳しくは、[iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)に関するページを参照してください。

#### <a name="images"></a>イメージ

以下のスクリーンショットは、言語固有の **.lproj** ディレクトリを含む、iOS サンプル アプリを示しています。 **es.lproj** というスペイン語のディレクトリには、ローカライズされたバージョンの既定のイメージと、**flag.png** が含まれています。

![](text-images/ios-resources.png "iOS のローカライズ プロジェクト ディレクトリ")

各言語のディレクトリには、その言語用にローカライズされた、**flag.png** のコピーが含まれています。 イメージが指定されていない場合、オペレーティング システムでは既定で、その既定の言語ディレクトリのイメージに設定されます。 完全に Retina をサポートするには、各イメージの **@2x** および **@3x** のコピーを指定する必要があります。

#### <a name="app-name"></a>アプリ名

**InfoPlist.strings** の内容は、アプリ名を構成するための単一のキーと値のみです。

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

アプリケーションの実行時に、アプリ名とイメージの両方がローカライズされます。

![](text-images/ios-imageicon.png "iOS のサンプル アプリ テキストとイメージのローカライズ")

### <a name="android-application-project"></a>Android アプリケーション プロジェクト

Android では、言語コード サフィックスが異なる**ドローアブル**および**文字列**ディレクトリを使用して、ローカライズされたイメージを格納するための異なるスキーマに従います。 4 文字のロケール コード (zh-TW や pt-BR など) が必要な場合、Android ではダッシュの後の、ロケール コードの前に追加の **r** が必要になることに注意してください (たとえば、 zh-rTW や pt-rBR)。 Android のローカライズについて詳しくは、「[Android のローカライズ](~/android/app-fundamentals/localization.md)」を参照してください。

#### <a name="images"></a>イメージ

以下のスクリーンショットは、いくつかのローカライズされたドローアブルと文字列を含む、Android のサンプルを示しています。

![](text-images/android-resources.png "Android のローカライズされたドローアブルと文字列のディレクトリ")

Android では、簡体中国語と繁体字中国語で zh-Hans と zh-Hant コードを使用しないことに注意してください。代わりに、国固有のコードである zh-CN と zh-TW のみがサポートされます。

高密度画面でさまざまな解像度イメージをサポートするには、**drawables-es-mdpi**、**drawables-es-xdpi**、**drawables-es-xxdpi** などの `-*dpi` サフィックスを使用して、追加の言語フォルダーを作成します。詳細については、[代替の Android リソースの提供](https://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)に関するページを参照してください。

#### <a name="app-name"></a>アプリ名

**strings.xml** の内容は、アプリ名を構成するための単一のキーと値のみです。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

`Label` で文字列 XML が参照されるように、Android アプリ プロジェクトで **MainActivity.cs** を更新します。

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

これで、アプリによって、アプリの名前とイメージがローカライズされます。 (スペイン語の) 結果のスクリーンショットを以下に示します。

![](text-images/android-imageicon.png "Android のサンプル アプリ テキストとイメージのローカライズ")

### <a name="universal-windows-platform-application-projects"></a>ユニバーサル Windows プラットフォームのアプリケーション プロジェクト

ユニバーサル Windows プラットフォームでは、イメージとアプリ名のローカライズを簡素化するリソース インフラストラクチャを所有しています。 UWP のローカライズについて詳しくは、[UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)に関するページを参照してください。

#### <a name="images"></a>イメージ

イメージは、次のスクリーンショットに示すように、リソース固有のフォルダーに配置してローカライズできます。

![](text-images/uwp-image-folder-structure.png "UWP イメージ ローカライズ フォルダーの構造")

実行時に、Windows リソース インフラストラクチャで、ユーザーのロケールに基づいて適切なイメージが選択されます。

## <a name="summary"></a>まとめ

Xamarin.Forms アプリケーションは、RESX ファイルと .NET グローバリゼーション クラスを使用してローカライズできます。 ユーザーが優先する言語を検出するための少量のプラットフォーム固有のコードとは異なり、ローカライズ作業のほとんどが一般的なコードで一元化されます。

イメージは一般的に、iOS と Android の両方で提供されるマルチ解像度サポートを利用するためのプラットフォーム固有の方法で処理されます。

## <a name="related-links"></a>関連リンク

- [RESX ローカライズ サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingresxlocalization)
- [TodoLocalized サンプル アプリ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todolocalized)
- [クロスプラットフォームのローカライズ](~/cross-platform/app-fundamentals/localization.md)
- [iOS のローカライズ](~/ios/app-fundamentals/localization/index.md)
- [Android のローカライズ](~/android/app-fundamentals/localization.md)
- [UWP のローカライズ](/windows/uwp/design/globalizing/globalizing-portal/)
- [CultureInfo クラスの使用 (MSDN)](https://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [固有カルチャのリソースの検索と使用 (MSDN)](https://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
