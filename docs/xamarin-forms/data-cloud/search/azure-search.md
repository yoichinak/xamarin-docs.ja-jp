---
title: Azure Search を使用したデータを検索
description: この記事では、Microsoft Azure Search のライブラリを使用して、Azure Search を Xamarin.Forms アプリケーションに統合する方法を示します。
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: bb1ebec25d747f1188f39e9c9032145bcdc3cb97
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242129"
---
# <a name="searching-data-with-azure-search"></a>Azure Search を使用したデータを検索

_Azure Search は、インデックスの作成とクエリ機能によりアップロードされたデータを提供するクラウド サービスです。これは、インフラストラクチャの要件と従来アプリケーションでの検索機能の実装に関連付けられている検索アルゴリズムの複雑さを削除します。この記事では、Microsoft Azure Search のライブラリを使用して、Azure Search を Xamarin.Forms アプリケーションに統合する方法を示します。_

## <a name="overview"></a>概要

データは、Azure Search のインデックスとドキュメントとして格納されます。 *インデックス*Azure Search サービスによって、検索可能なデータ ストアは、概念的には、データベース テーブルに似ています。 A*ドキュメント*インデックスの中で検索可能なデータの 1 つの単位は、概念的には、データベースの行に似ています。 ときにドキュメントをアップロードし、Azure search の検索クエリを送信して、要求は、search サービス内の特定のインデックスに行われます。

Azure Search に行われた各要求では、サービス、および API キーの名前を含める必要があります。 API キーの 2 つの種類があります。

- *管理キー*すべての操作に対するフル アクセス権を付与します。 これには、サービスの管理、作成、およびインデックス、およびデータ ソースの削除が含まれます。
- *クエリ キー*インデックスとドキュメント、読み取り専用アクセスを許可し、検索要求を発行するアプリケーションで使用する必要があります。

Azure Search に最も一般的な要求では、クエリを実行します。 送信できるクエリの 2 つの種類があります。

- A*検索*インデックス内のすべての検索可能フィールドの 1 つまたは複数の項目の検索クエリを実行します。 検索クエリは、簡単な構文または Lucene のクエリ構文を使用して構築されます。 詳細については、次を参照してください。 [Azure Search で簡単なクエリ構文](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/)、および[Azure Search でのクエリ構文の Lucene](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/)です。
- A*フィルター*クエリでは、インデックス内のすべてのフィルターを適用できるフィールドに対してブール式を評価します。 フィルター クエリは、OData フィルター言語のサブセットを使用して構築されます。 詳細については、次を参照してください。 [Azure Search の OData 式の構文](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/)です。

検索のクエリとフィルターのクエリは、個別にまたは組み合わせて使用できます。 フィルター クエリが全体のインデックスを最初に適用一緒に使用するされ、フィルター クエリの結果は、検索クエリが実行されます。

Azure Search では、検索入力に基づく提案を取得してもサポートします。 詳細については、次を参照してください。[提案クエリ](#suggestions)です。

## <a name="setup"></a>セットアップ

Azure Search を Xamarin.Forms アプリケーションに統合するためのプロセスは次のとおりです。

1. Azure Search サービスを作成します。 詳細については、次を参照してください。 [Azure ポータルを使用して Azure Search サービスの作成](/azure/search/search-create-service-portal/)です。
1. Xamarin.Forms ソリューション ポータブル クラス ライブラリ (PCL) からのターゲット フレームワークとして Silverlight を削除します。 これは、クロス プラットフォーム開発をサポートしていますが、プロファイル 151 またはプロファイル 92 など、Silverlight をサポートしていないすべてのプロファイルに PCL プロファイルを変更することで実現できます。
1. 追加、 [Microsoft Azure Search ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.Search)NuGet パッケージを PCL プロジェクト Xamarin.Forms ソリューションにします。

次の手順を実行すると、検索インデックスおよびデータ ソースの管理、アップロードし、ドキュメントの管理、およびクエリを実行する、Microsoft Search ライブラリ API を使用できます。

## <a name="creating-the-azure-search-index"></a>Azure Search のインデックスを作成します。

検索するデータの構造にマップされるインデックス スキーマを定義する必要があります。 プログラムを使用するか、Azure ポータルで実現できます、`SearchServiceClient`クラスです。 このクラスは、Azure Search への接続を管理し、インデックスを作成するために使用できます。 次のコード例では、このクラスのインスタンスを作成する方法を示します。

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient`検索サービス名を受け取るコンス トラクター オーバー ロードと`SearchCredentials`オブジェクトを引数として、`SearchCredentials`オブジェクトの折り返し、*管理者キー* Azure Search サービスのです。 *管理者キー*インデックスを作成するが必要です。

> [!NOTE]
>  1 つ`SearchServiceClient`インスタンスは、Azure Search に多数の接続を開くことを避けるために、アプリケーションで使用する必要があります。

によってインデックスが定義されている、`Index`オブジェクト、次のコード例で示したようにします。

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

`Index.Name`プロパティは、インデックスの名前に設定する必要があります、`Index.Fields`プロパティは、型の配列を設定する必要があります`Field`オブジェクト。 各`Field`インスタンスは、名前、型、および、フィールドの使用方法を指定のプロパティを指定します。 これらのプロパティは次のとおりです。

- `IsKey` フィールドにインデックスのキーがあるかどうかを示します。 型のインデックス内のフィールドを 1 つだけ`DataType.String`、キー フィールドとして指定する必要があります。
- `IsFacetable` このフィールドのファセット ナビゲーションを実行することがあるかどうかを示します。 既定値は `false` です。
- `IsFilterable` – フィルター クエリ内でフィールドを使用できるかどうかを示します。 既定値は `false` です。
- `IsRetrievable` – 検索結果で、フィールドを取得できるかどうかを示します。 既定値は `true` です。
- `IsSearchable` –、フルテキスト検索でフィールドが含まれているかどうかを示します。 既定値は `false` です。
- `IsSortable` – でフィールドを使用できるかどうかを示す`OrderBy`式。 既定値は `false` です。

> [!NOTE]
> 展開の後にインデックスを変更するには、再構築して、データを再読み込みする必要があります。

`Index`オブジェクトを指定できます必要に応じて、`Suggesters`プロパティで、オート コンプリートをサポートまたは提案クエリの検索に使用されるインデックスのフィールドを定義します。 `Suggesters`プロパティは、型の配列を設定する必要があります`Suggester`検索候補の結果の構築に使用されるフィールドを定義するオブジェクト。

作成した後、`Index`オブジェクトを呼び出すことによって、インデックスが作成された`Indexes.Create`上、`SearchServiceClient`インスタンス。

> [!NOTE]
> アプリケーションからインデックスを作成するを応答で保持する必要があります、ときに使用して、`Indexes.CreateAsync`メソッドです。

詳細については、次を参照してください。 [.NET SDK を使用して、Azure Search インデックスの作成](/azure/search/search-create-index-dotnet/)です。

## <a name="deleting-the-azure-search-index"></a>Azure Search のインデックスを削除します。

呼び出すことによって、インデックスを削除できる`Indexes.Delete`上、`SearchServiceClient`インスタンス。

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure Search インデックスにデータをアップロードします。

インデックスを定義した後は、2 つのモデルのいずれかを使用してデータをアップロードすることができます。

- **プル モデル**– Azure Cosmos DB、Azure SQL Database、Azure Blob ストレージからデータが取り込まれ定期的にまたは SQL Server が Azure の仮想マシンでホストされています。
- **プッシュ モデル**– データをプログラムによって、インデックスに送信します。 これは、この記事で採用されたモデルです。

A`SearchIndexClient`インデックスにデータをインポートするインスタンスを作成する必要があります。 これには、呼び出すことによって、`SearchServiceClient.Indexes.GetClient`メソッドを次のコード例で示したようにします。

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

としてインデックスにインポートするデータがパッケージ化、 `IndexBatch` 、オブジェクトのコレクションをカプセル化する`IndexAction`オブジェクト。 各`IndexAction`インスタンスには、ドキュメント、および Azure Search がドキュメントに対して実行する操作を示すプロパティが含まれています。 上記のコード例で、`IndexAction.Upload`アクションを指定すると、新規である場合、インデックスに挿入されるドキュメントでまたはが既に存在する場合は、交換します。 `IndexBatch`を呼び出して、オブジェクトは、インデックスに送信されますが、`Documents.Index`メソッドを`SearchIndexClient`オブジェクト。 その他のインデックス作成アクションについては、次を参照してください。[を使用するインデックス操作を決定](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use)です。

> [!NOTE]
> 1 つのインデックス作成要求では、1000 を超えるドキュメントを含めることができます。

上記のコード例で、`monkeyList`のコレクションから匿名オブジェクトとしてコレクションを作成`Monkey`オブジェクト。 これにより、作成データを`id`フィールド、および pascal のマッピングを解決`Monkey`プロパティの名前をキャメル ケースは、フィールドのインデックス名を検索します。 代わりに、このマッピングを実行することも追加することで、`[SerializePropertyNamesAsCamelCase]`属性を`Monkey`クラスです。

詳細については、次を参照してください。 [Azure search .NET SDK を使用してデータをアップロード](/azure/search/search-import-data-dotnet/)です。

## <a name="querying-the-azure-search-index"></a>Azure Search のインデックスのクエリを実行します。

A`SearchIndexClient`索引へのクエリ インスタンスを作成する必要があります。 アプリケーションでは、クエリを実行するときは、最低限の特権の原則に従いを作成することをお勧め、`SearchIndexClient`を直接渡すこと、*クエリ キー*を引数として。 これによりユーザーは、インデックスとドキュメントに読み取り専用アクセス。 この方法は、次のコード例について説明します。

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient`コンス トラクターのオーバー ロードは、検索サービス名、インデックス名、および`SearchCredentials`オブジェクトを引数として、`SearchCredentials`オブジェクトの折り返し、*クエリ キー* Azure Search サービスのです。

### <a name="search-queries"></a>検索クエリ

呼び出すことによって、インデックスのクエリを実行できます、`Documents.SearchAsync`メソッドを`SearchIndexClient`インスタンス、次のコード例で示したようにします。

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

`SearchAsync`メソッドは、検索テキストの引数と省略可能な使用`SearchParameters`クエリを絞り込むために使用するオブジェクト。 検索クエリが、検索テキストの引数として指定されていますが、フィルター クエリを設定して指定できます、`Filter`のプロパティ、`SearchParameters`引数。 次のコード例では、両方のクエリの種類を示しています。

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

このフィルターのクエリは、インデックス全体に適用され、結果からドキュメントを削除位置、`location`フィールドには、中国に等しいとベトナム等しくありません。 フィルタ リングの後、フィルター クエリの結果、検索クエリが実行されます。

> [!NOTE]
> 検索することがなくフィルターは、渡す`*`検索テキストの引数として。

`SearchAsync`メソッドを返します、`DocumentSearchResult`クエリの結果を格納しているオブジェクト。 それぞれに、このオブジェクトが列挙`Document`オブジェクトとして作成されている、`Monkey`オブジェクトを追加、 `Monkeys` `ObservableCollection`表示用です。 次のスクリーン ショット番組検索クエリの結果 Azure Search からが返されます。

![](azure-search-images/search.png "検索結果")

検索とフィルター処理の詳細については、次を参照してください。 [.NET SDK を使用して、Azure Search インデックスにクエリ](/azure/search/search-query-dotnet/)です。

<a name="suggestions" />

### <a name="suggestion-queries"></a>提案クエリ

Azure Search を使用する要求されるまでの推奨事項に基づいて、検索クエリを呼び出すことによって、`Documents.SuggestAsync`メソッドを`SearchIndexClient`インスタンス。 これを次のコード例に示します。

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

`SuggestAsync`メソッド引数を取る検索テキストを使用する、suggester の名前 (で定義されているインデックス)、および省略可能な`SuggestParameters`クエリを絞り込むために使用するオブジェクト。 `SuggestParameters`インスタンスは、次のプロパティを設定します。

- `UseFuzzyMatching` – に設定すると`true`、検索文字列の代用または不足している文字がある場合でも、Azure Search は提案に見つかります。
- `HighlightPreTag` – 先頭提案ヒットに追加するタグです。
- `HighlightPostTag` – 提案ヒットに付加されるタグ。
- `MinimumCoverage` – 表すクエリに対して提案クエリによりカバーする必要があります、インデックスの割合が、成功を報告します。 既定値は 80 です。
- `Top` 取得する検索候補の数。 1 ~ 5 の既定値の 100 の整数にする必要があります。

全体的な効果は、ヒットの強調表示、および結果に含まれるドキュメントが同様に検索語句のスペルを含めるにインデックスから上位 10 個の結果で返されることです。

`SuggestAsync`メソッドを返します、`DocumentSuggestResult`クエリの結果を格納しているオブジェクト。 それぞれに、このオブジェクトが列挙`Document`オブジェクトとして作成されている、`Monkey`オブジェクトを追加、 `Monkeys` `ObservableCollection`表示用です。 次のスクリーン ショットは、Azure Search から返される候補の結果を表示します。

![](azure-search-images/suggest.png "候補の結果")

サンプル アプリケーションで、`SuggestAsync`メソッドは、ユーザーは、検索語句の入力が終了したときにのみ呼び出されます。 ただし、そのこともできますキーを押すたびに実行することによって検索のオートコンプリート クエリをサポートするためにします。

## <a name="summary"></a>まとめ

この記事では、Microsoft Azure Search のライブラリを使用して、Azure Search を Xamarin.Forms アプリケーションに統合する方法が示されています。 Azure Search は、インデックスの作成とクエリ機能によりアップロードされたデータを提供するクラウド サービスです。 これは、インフラストラクチャの要件と従来アプリケーションでの検索機能の実装に関連付けられている検索アルゴリズムの複雑さを削除します。


## <a name="related-links"></a>関連リンク

- [Azure 検索 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Azure Search のドキュメント](/azure/search/)
- [Microsoft Azure ライブラリの検索](https://www.nuget.org/packages/Microsoft.Azure.Search/)
