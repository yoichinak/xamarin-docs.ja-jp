---
title: Azure Search と Xamarin. フォームを使用してデータを検索する
description: この記事では、Microsoft の Azure Search ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Search を統合する方法を示します。
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: ea2c733a9c85662b9286f8e8631b601248dc11de
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770837"
---
# <a name="search-data-with-azure-search-and-xamarinforms"></a>Azure Search と Xamarin. フォームを使用してデータを検索する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Azure Search とは、インデックス作成とクエリのデータがアップロードするための機能を提供するクラウド サービスです。これには、インフラストラクチャの要件と、アプリケーションに検索機能を実装するのに関連付けられた検索アルゴリズムの複雑さが削除されます。この記事では、Microsoft の Azure Search ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Search を統合する方法を示します。_

## <a name="overview"></a>概要

データは、Azure Search でインデックスとドキュメントとして格納されます。 *インデックス*によって、Azure Search サービス検索データ ストアは、概念的には、データベース テーブルに似ています。 A*ドキュメント*インデックスでは、検索可能なデータの 1 つの単位は、概念的には、データベースの行に似ています。 ドキュメントをアップロードし、Azure search の検索クエリを送信して、要求はに対して行われた場合、search サービスで特定のインデックス。

Azure Search に対する各要求は、サービスと API キーの名前を含める必要があります。 API キーの 2 種類あります。

- *管理者キー*すべての操作に対する完全な権限を付与します。 これには、サービスの管理、作成、およびインデックス、およびデータ ソースを削除が含まれます。
- *クエリ キー*にインデックスとドキュメントの読み取り専用のアクセスを許可し、検索要求を発行するアプリケーションで使用する必要があります。

最も一般的な要求を Azure Search では、クエリを実行します。 送信できるクエリの 2 種類あります。

- A*検索*インデックス内のすべての検索可能フィールドで 1 つまたは複数の項目を検索しクエリを実行します。 検索クエリは、簡略化された構文、または、Lucene クエリ構文を使用して構築されます。 詳細については、[Azure Search での単純なクエリ構文](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/)、および[Azure Search の Lucene クエリ構文](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/)を参照してください。
- A*フィルター*クエリでは、インデックス内のすべてのフィルター可能フィールドに対してブール式を評価します。 フィルター クエリは、OData フィルター言語のサブセットを使用して構築されます。 詳細については、[Azure Search の OData 式の構文](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/)を参照してください。

検索クエリとフィルターのクエリは、個別にまたは組み合わせて使用できます。 フィルター クエリが、インデックス全体を最初に適用して一緒に使用して、フィルター クエリの結果は、検索クエリが実行されます。

Azure Search には、検索の入力に基づく検索候補もサポートしています。 詳細については、[提案クエリ](#suggestions)を参照してください。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションに Azure Search を統合するためのプロセスは次のとおりです。

1. Azure Search サービスを作成します。 詳細については、[Azure Portal を使用して Azure Search サービスの作成](/azure/search/search-create-service-portal/)を参照してください。
1. ターゲット フレームワークと Silverlight を Xamarin.Forms ソリューションのポータブル クラス ライブラリ (PCL) から削除します。 これは、クロス プラットフォームの開発をサポートしていますが、151 プロファイルやプロファイル 92、Silverlight をサポートされていませんプロファイルに、PCL プロファイルを変更することで実現できます。
1. 追加、 [Microsoft Azure Search ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.Search)Xamarin.Forms ソリューションでは、PCL プロジェクトに NuGet パッケージ。

次の手順を実行した後は、検索のインデックスおよびデータ ソースの管理、アップロードしドキュメントの管理、およびクエリを実行する、Microsoft Search ライブラリ API を使用できます。

## <a name="creating-the-azure-search-index"></a>Azure Search インデックスを作成します。

検索するデータの構造にマップするインデックス スキーマを定義する必要があります。 Azure portal での実現またはプログラムで使用できます、`SearchServiceClient`クラス。 このクラスは、Azure Search への接続を管理し、インデックスを作成するために使用できます。 次のコード例では、このクラスのインスタンスを作成する方法を示します。

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient`コンストラクター オーバー ロードは、検索サービスの名前と`SearchCredentials`引数としてオブジェクトを`SearchCredentials`オブジェクト折り返し、*管理者キー* Azure Search サービス。 *管理者キー*インデックスを作成するが必要です。

> [!NOTE]
> 1 つ`SearchServiceClient`インスタンスは、Azure Search への接続が多すぎますを開くを回避するためにアプリケーションで使用する必要があります。

によってインデックスが定義されている、`Index`の次のコード例に示すオブジェクトします。

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name`プロパティは、インデックスの名前に設定する必要があります、`Index.Fields`配列へのプロパティを設定する必要があります`Field`オブジェクト。 各`Field`インスタンスは、名前、型、および、フィールドの使用方法を指定のプロパティを指定します。 これらのプロパティは次のとおりです。

- `IsKey` フィールドがインデックスのキーであるかどうかを示します。 フィールドの種類のインデックスを 1 つのみ`DataType.String`、キー フィールドとして指定する必要があります。
- `IsFacetable` – このフィールドでファセット ナビゲーションを実行することはあるかどうかを示します。 既定値は `false` です。
- `IsFilterable` フィールドをフィルター クエリで使用できるかどうかを示します。 既定値は `false` です。
- `IsRetrievable` – 検索結果にフィールドを取得できるかどうかを示します。 既定値は `true` です。
- `IsSearchable` フィールドがフルテキスト検索に含めるかどうかを示します。 既定値は `false` です。
- `IsSortable` – でフィールドを使用できるかどうかを示します`OrderBy`式。 既定値は `false` です。

> [!NOTE]
> 展開した後にインデックスを変更するには、再構築して、データの再読み込みする必要があります。

`Index`オブジェクトを指定できます必要に応じて、`Suggesters`プロパティで、オート コンプリートをサポートするか、検索候補クエリに使用されるインデックスのフィールドを定義します。 `Suggesters`配列へのプロパティを設定する必要があります`Suggester`検索候補の結果の構築に使用されるフィールドを定義するオブジェクト。

作成した後、`Index`オブジェクトを呼び出すことによって、インデックスが作成された`Indexes.Create`上、`SearchServiceClient`インスタンス。

> [!NOTE]
> 使用して、アプリケーションからインデックスを作成するも応答性を保持する必要があります、ときに、`Indexes.CreateAsync`メソッド。

詳細については、[.NET SDK を使用して Azure Search インデックスの作成](/azure/search/search-create-index-dotnet/)を参照してください。

## <a name="deleting-the-azure-search-index"></a>Azure Search インデックスを削除します。

呼び出すことにより、インデックスを削除できる`Indexes.Delete`上、`SearchServiceClient`インスタンス。

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure Search インデックスにデータをアップロードします。

インデックスを定義した後は、2 つのモデルのいずれかを使用してデータをアップロードできます。

- **プル モデル**-Azure Cosmos DB、Azure SQL Database、Azure Blob Storage からデータを取り込むは定期的にまたは SQL Server が Azure の仮想マシンでホストされています。
- **プッシュ モデル**–、インデックスにデータをプログラムで送信します。 これは、この記事で採用されたモデルです。

A`SearchIndexClient`インスタンスを作成して、インデックスにデータをインポートする必要があります。 これを呼び出すことにより実現することができます、`SearchServiceClient.Indexes.GetClient`メソッドは、次のコード例で示した。

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

としてインデックスにインポートするデータがパッケージ化、 `IndexBatch` 、オブジェクトのコレクションをカプセル化する`IndexAction`オブジェクト。 各`IndexAction`インスタンスには、ドキュメント、および Azure Search のドキュメントに対して実行する操作を指示するプロパティが含まれています。 上記のコード例で、`IndexAction.Upload`アクションを指定すると、その結果が、新規の場合、インデックスに挿入されるドキュメントまたは既に存在する場合に置き換えられます。 `IndexBatch`オブジェクトが呼び出すことによって、インデックスに送信し、`Documents.Index`メソッドを`SearchIndexClient`オブジェクト。 その他のインデックス作成操作の詳細については、[どのインデックス作成アクションを使用するかを決める](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use)を参照してください。

> [!NOTE]
> 1000 個のドキュメントは、1 つのインデックス作成要求に含めることができます。

上記のコード例で、`monkeyList`として匿名オブジェクトのコレクションからコレクションが作成された`Monkey`オブジェクト。 データを作成しますこの、`id`フィールドをパスカル ケースのマッピングを解決して`Monkey`プロパティの名前をキャメル ケースは、インデックスのフィールド名を検索します。 また、このマッピング行うこともできますを追加して、`[SerializePropertyNamesAsCamelCase]`属性を`Monkey`クラス。

詳細については、[.NET SDK を使用して Azure Search にデータをアップロード](/azure/search/search-import-data-dotnet/)を参照してください。

## <a name="querying-the-azure-search-index"></a>Azure Search インデックスのクエリ

A`SearchIndexClient`インスタンスを作成してインデックスを照会する必要があります。 アプリケーションでは、クエリを実行するときは、最小限の特権の原則に従うし、作成することをお勧め、`SearchIndexClient`を直接渡すこと、*クエリ キー*を引数として。 これによりユーザーは、インデックスとドキュメントに読み取り専用アクセス。 このアプローチは、次のコード例について説明します。

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient`コンストラクター オーバー ロードは、検索サービスの名前、インデックスの名前、および`SearchCredentials`引数としてオブジェクトを`SearchCredentials`オブジェクト折り返し、*クエリ キー* Azure Search サービスの。

### <a name="search-queries"></a>検索クエリ

呼び出すことによって、インデックスのクエリを実行できる、`Documents.SearchAsync`メソッドを`SearchIndexClient`の次のコード例に示すように、インスタンスします。

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync`メソッドは、検索テキストの引数と省略可能な`SearchParameters`クエリを絞り込むために使用できるオブジェクト。 検索クエリが、検索テキストの引数として指定されていますが設定してフィルター クエリを指定することができます、`Filter`のプロパティ、`SearchParameters`引数。 次のコード例では、両方のクエリの種類を示しています。

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

このフィルターのクエリはインデックス全体に適用され、結果からドキュメントを削除します。 場所、`location`フィールドがないと、中国に等しいとベトナム等しくありません。 フィルタ リングの後は、検索クエリはフィルター クエリの結果に対して実行されます。

> [!NOTE]
> フィルター処理、検索することがなく、渡す`*`検索テキストの引数として。

`SearchAsync`メソッドを返します。 を`DocumentSearchResult`クエリの結果を格納しているオブジェクト。 それぞれに、このオブジェクトが列挙`Document`オブジェクトとして作成されている、`Monkey`オブジェクトし、に追加、 `Monkeys` `ObservableCollection`表示します。 次のスクリーン ショット show 検索クエリ結果 Azure Search から返されます。

![](azure-search-images/search.png "検索結果")

検索とフィルター処理の詳細については、[.NET SDK を使用して Azure Search インデックスの照会](/azure/search/search-query-dotnet/)を参照してください。

<a name="suggestions" />

### <a name="suggestion-queries"></a>提案クエリ

Azure Search は、呼び出すことによって、検索クエリに基づく提案が要求される、`Documents.SuggestAsync`メソッドを`SearchIndexClient`インスタンス。 これは、次のコード例について説明します。

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync`メソッド引数を受け取り、検索テキストを使用するサジェスターの名前 (で定義されているインデックス)、および省略可能な`SuggestParameters`クエリを絞り込むために使用できるオブジェクト。 `SuggestParameters`インスタンスは、次のプロパティを設定します。

- `UseFuzzyMatching` – に設定すると`true`、検索テキストに置き換えられたか、不足している文字がある場合でも、Azure Search は提案に見つかります。
- `HighlightPreTag` – 提案ヒットを先頭に付加するタグ。
- `HighlightPostTag` – 提案ヒットに追加するタグ。
- `MinimumCoverage` – を表すクエリのための提案クエリで範囲に含まれるインデックスの割合が、成功を報告します。 既定値は、80 です。
- `Top` 取得する検索候補の数。 1 ~ 100 で、既定値は 5 の整数の可能性があります。

全体的な影響は、ヒットの強調表示、および結果に同様に検索語句のスペルを含むドキュメントを含めるにインデックスの上位 10 個の結果で返されることです。

`SuggestAsync`メソッドを返します。 を`DocumentSuggestResult`クエリの結果を格納しているオブジェクト。 それぞれに、このオブジェクトが列挙`Document`オブジェクトとして作成されている、`Monkey`オブジェクトし、に追加、 `Monkeys` `ObservableCollection`表示します。 次のスクリーン ショットは、Azure Search から返される候補の結果を表示します。

![](azure-search-images/suggest.png "候補の結果")

サンプル アプリケーションで、`SuggestAsync`メソッドは、ユーザーは、検索語句の入力が完了するとにのみ呼び出されます。 ただし、そのこともできますキーを押すたびに実行することによって、オート コンプリートの検索クエリをサポートするためにします。

## <a name="summary"></a>まとめ

この記事では、Microsoft Azure Search ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Search を統合する方法を説明します。 Azure Search とは、インデックス作成とクエリのデータがアップロードするための機能を提供するクラウド サービスです。 これには、インフラストラクチャの要件と、アプリケーションに検索機能を実装するのに関連付けられた検索アルゴリズムの複雑さが削除されます。

## <a name="related-links"></a>関連リンク

- [Azure Search (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Azure Search のドキュメント](/azure/search/)
- [Microsoft Azure Search ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.Search/)
