---
title: Azure Search とを使用してデータを検索する Xamarin.Forms
description: この記事では、Microsoft Azure 検索ライブラリを使用して Azure Search をアプリケーションに統合する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 14d26c1360c1c1b7997598ef1263e3dd62e3c013
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561782"
---
# <a name="search-data-with-azure-search-and-no-locxamarinforms"></a>Azure Search とを使用してデータを検索する Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Azure Search は、アップロードされたデータのインデックス作成機能とクエリ機能を提供するクラウドサービスです。これにより、従来、アプリケーションでの検索機能の実装に関連するインフラストラクチャ要件と検索アルゴリズムの複雑さが解消されます。この記事では、Microsoft Azure 検索ライブラリを使用して Azure Search をアプリケーションに統合する方法について説明し Xamarin.Forms ます。_

## <a name="overview"></a>概要

データは、インデックスおよびドキュメントとして Azure Search に格納されます。 *インデックス*は、Azure Search サービスによって検索できるデータの格納場所であり、概念的にはデータベーステーブルに似ています。 *ドキュメント*は、インデックス内の検索可能なデータの1つの単位であり、概念的にはデータベースの行に似ています。 ドキュメントをアップロードし、検索クエリを Azure Search に送信すると、search サービスの特定のインデックスに対して要求が行われます。

Azure Search に対して行われる各要求には、サービスの名前と API キーを含める必要があります。 API キーには、次の2種類があります。

- *管理者キー* は、すべての操作に対する完全な権限を付与します。 これには、サービスの管理、インデックスの作成と削除、およびデータソースが含まれます。
- *クエリキー* は、インデックスとドキュメントへの読み取り専用アクセスを付与し、検索要求を発行するアプリケーションで使用する必要があります。

Azure Search するための最も一般的な要求は、クエリを実行することです。 送信できるクエリには、次の2種類があります。

- *検索*クエリは、インデックス内のすべての検索可能なフィールド内の1つ以上の項目を検索します。 検索クエリは、簡略化された構文または Lucene クエリ構文を使用して作成されます。 詳細については、「 [Azure Search の単純なクエリ構文](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/)」と「 [Azure Search での Lucene クエリ構文](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/)」を参照してください。
- *フィルター*クエリは、インデックス内のすべてのフィルター可能なフィールドに対してブール式を評価します。 フィルタークエリは、OData フィルター言語のサブセットを使用して作成されます。 詳細については、「 [Azure Search の OData 式の構文](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/)」を参照してください。

検索クエリとフィルタークエリは、個別または組み合わせて使用できます。 フィルタークエリを一緒に使用すると、最初にインデックス全体に適用され、その後、フィルタークエリの結果に対して検索クエリが実行されます。

Azure Search は、検索入力に基づいて候補を取得することもできます。 詳細については、「 [提案クエリ](#suggestion-queries)」を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

## <a name="setup"></a>セットアップ

アプリケーションに Azure Search を統合するプロセス Xamarin.Forms は次のとおりです。

1. Azure Search サービスを作成します。 詳細については、「 [Azure Portal を使用した Azure Search サービスの作成](/azure/search/search-create-service-portal/)」を参照してください。
1. Silverlight を Xamarin.Forms ソリューションポータブルクラスライブラリ (PCL) からターゲットフレームワークとして削除します。 これを実現するには、PCL プロファイルをクロスプラットフォーム開発をサポートするプロファイルに変更しますが、プロファイル151やプロファイル92などの Silverlight はサポートしていません。
1. ソリューションの PCL プロジェクトに [Microsoft Azure Search ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.Search) NuGet パッケージを追加し Xamarin.Forms ます。

これらの手順を実行すると、Microsoft Search ライブラリ API を使用して、検索インデックスとデータソースの管理、ドキュメントのアップロードと管理、およびクエリの実行を行うことができます。

## <a name="creating-the-azure-search-index"></a>Azure Search インデックスの作成

検索するデータの構造にマップするインデックススキーマを定義する必要があります。 これは、Azure Portal で、またはクラスを使用してプログラムで実行でき `SearchServiceClient` ます。 このクラスは、Azure Search への接続を管理し、インデックスの作成に使用できます。 次のコード例は、このクラスのインスタンスを作成する方法を示しています。

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

コンストラクターのオーバーロードは、 `SearchServiceClient` 検索サービスの名前と `SearchCredentials` オブジェクトを引数として受け取り、 `SearchCredentials` Azure Search サービスの *管理者キー* をラップするオブジェクトを使用します。 インデックスを作成するには、 *管理者キー* が必要です。

> [!NOTE]
> アプリケーションで1つのインスタンスを使用して、 `SearchServiceClient` Azure Search への接続が多すぎないようにする必要があります。

インデックスは、 `Index` 次のコード例に示すように、オブジェクトによって定義されます。

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

プロパティは、 `Index.Name` インデックスの名前に設定する必要があります。また、プロパティは、 `Index.Fields` オブジェクトの配列に設定する必要があり `Field` ます。 各インスタンスでは、 `Field` 名前、型、およびフィールドの使用方法を指定するプロパティを指定します。 これには次のようなプロパティがあります。

- `IsKey` –フィールドがインデックスのキーであるかどうかを示します。 `DataType.String`キーフィールドとして指定する必要があるのは、型のインデックス内の1つのフィールドだけです。
- `IsFacetable` –このフィールドでファセットナビゲーションを実行できるかどうかを示します。 既定値は `false` です。
- `IsFilterable` –フィルタークエリでフィールドを使用できるかどうかを示します。 既定値は `false` です。
- `IsRetrievable` –検索結果でフィールドを取得できるかどうかを示します。 既定値は `true` です。
- `IsSearchable` –フィールドがフルテキスト検索に含まれるかどうかを示します。 既定値は `false` です。
- `IsSortable` –式でフィールドを使用できるかどうかを示し `OrderBy` ます。 既定値は `false` です。

> [!NOTE]
> 配置後にインデックスを変更するには、データの再構築と再読み込みが必要です。

オブジェクトでは、必要に応じて、 `Index` `Suggesters` オートコンプリートまたは検索候補クエリをサポートするために使用されるインデックス内のフィールドを定義するプロパティを指定できます。 プロパティは、 `Suggesters` `Suggester` 検索候補の結果を作成するために使用されるフィールドを定義するオブジェクトの配列に設定する必要があります。

オブジェクトを作成した後 `Index` 、インスタンスでを呼び出すことによってインデックスが作成され `Indexes.Create` `SearchServiceClient` ます。

> [!NOTE]
> 応答性を維持する必要があるアプリケーションからインデックスを作成する場合は、メソッドを使用し `Indexes.CreateAsync` ます。

詳細については、「 [.NET SDK を使用した Azure Search インデックスの作成](/azure/search/search-create-index-dotnet/)」を参照してください。

## <a name="deleting-the-azure-search-index"></a>Azure Search インデックスを削除しています

インデックスは、インスタンスでを呼び出すことによって削除でき `Indexes.Delete` `SearchServiceClient` ます。

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure Search インデックスにデータをアップロードしています

インデックスを定義した後、次の2つのモデルのいずれかを使用してデータをアップロードできます。

- **プルモデル** –データは、Azure 仮想マシンでホストされている Azure Cosmos DB、Azure SQL Database、Azure Blob Storage、または SQL Server から定期的に取り込まれたされます。
- **プッシュモデル** –データはプログラムによってインデックスに送信されます。 これは、この記事で採用されているモデルです。

`SearchIndexClient`インデックスにデータをインポートするには、インスタンスを作成する必要があります。 これは、 `SearchServiceClient.Indexes.GetClient` 次のコード例に示すように、メソッドを呼び出すことによって実現できます。

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

インデックスにインポートされるデータはオブジェクトとしてパッケージ化され `IndexBatch` 、オブジェクトのコレクションをカプセル化 `IndexAction` します。 各 `IndexAction` インスタンスには、ドキュメントと、ドキュメントに対して実行するアクション Azure Search を示すプロパティが含まれています。 上記のコード例では、 `IndexAction.Upload` アクションが指定されています。これにより、ドキュメントが新しい場合はインデックスに挿入され、既に存在する場合は置き換えられます。 次に、オブジェクトの `IndexBatch` メソッドを呼び出して、オブジェクトをインデックスに送信し `Documents.Index` `SearchIndexClient` ます。 その他のインデックス作成アクションの詳細については、「 [使用するインデックス作成アクションの決定](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use)」を参照してください。

> [!NOTE]
> 1つのインデックス作成要求に含めることができるのは、1000のドキュメントのみです。

上記のコード例では、 `monkeyList` コレクションはオブジェクトのコレクションから匿名オブジェクトとして作成されることに注意して `Monkey` ください。 これにより、フィールドのデータが作成さ `id` れ、Pascal ケースの `Monkey` プロパティ名と camel ケースの検索インデックスフィールド名とのマッピングが解決されます。 また、属性をクラスに追加することによって、このマッピングを実現することもでき `[SerializePropertyNamesAsCamelCase]` `Monkey` ます。

詳細については、「 [.NET SDK を使用して Azure Search にデータをアップロードする](/azure/search/search-import-data-dotnet/)」を参照してください。

## <a name="querying-the-azure-search-index"></a>Azure Search インデックスのクエリ

`SearchIndexClient`インデックスを照会するには、インスタンスを作成する必要があります。 アプリケーションでクエリを実行する場合は、最小限の特権の原則に従い、を直接作成し `SearchIndexClient` て、 *クエリキー* を引数として渡すことをお勧めします。 これにより、ユーザーがインデックスとドキュメントに対する読み取り専用アクセス権を持つことが保証されます。 この方法を次のコード例に示します。

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

コンストラクターのオーバーロードは、 `SearchIndexClient` 検索サービス名、インデックス名、および `SearchCredentials` オブジェクトを引数として受け取り、 `SearchCredentials` Azure Search サービスの *クエリキー* をラップするオブジェクトを使用します。

### <a name="search-queries"></a>検索クエリ

`Documents.SearchAsync` `SearchIndexClient` 次のコード例に示すように、インスタンスでメソッドを呼び出すことにより、インデックスを照会できます。

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

メソッドは、 `SearchAsync` 検索テキスト引数と、 `SearchParameters` クエリをさらに絞り込むために使用できるオプションのオブジェクトを受け取ります。 検索クエリは検索テキスト引数として指定されますが、フィルタークエリは引数のプロパティを設定することによって指定でき `Filter` `SearchParameters` ます。 次のコード例は、両方のクエリの種類を示しています。

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

このフィルタークエリは、インデックス全体に適用され、 `location` フィールドが中国と等しくなく、ベトナムと等しくない結果からドキュメントを削除します。 フィルター処理後に、フィルタークエリの結果に対して検索クエリが実行されます。

> [!NOTE]
> 検索せずにフィルター処理を行うには、を `*` 検索テキスト引数として渡します。

メソッドは、 `SearchAsync` `DocumentSearchResult` クエリ結果を含むオブジェクトを返します。 このオブジェクトが列挙され、各 `Document` オブジェクトがオブジェクトとして作成され `Monkey` 、 `Monkeys` 表示のためにに追加され `ObservableCollection` ます。 次のスクリーンショットは Azure Search から返された検索クエリの結果を示しています。

![検索結果](azure-search-images/search.png)

検索とフィルター処理の詳細については、「 [.NET SDK を使用した Azure Search インデックスのクエリ](/azure/search/search-query-dotnet/)」を参照してください。

### <a name="suggestion-queries"></a>提案クエリ

Azure Search を使用すると、インスタンスでメソッドを呼び出すことによって、検索クエリに基づいて提案を要求でき `Documents.SuggestAsync` `SearchIndexClient` ます。 これを次のコード例に示します。

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

メソッドは、 `SuggestAsync` 検索テキスト引数、使用する suggester の名前 (インデックスで定義されています)、および `SuggestParameters` クエリをさらに絞り込むために使用できるオプションのオブジェクトを受け取ります。 インスタンスは、 `SuggestParameters` 次のプロパティを設定します。

- `UseFuzzyMatching` –に設定 `true` されている場合、検索テキストに置換文字または見つからない文字がある場合でも、Azure Search は候補を検索します。
- `HighlightPreTag` –提案のヒットの前に付加されるタグ。
- `HighlightPostTag` –提案のヒットに追加されるタグ。
- `MinimumCoverage` -クエリが成功したことを報告するために、候補クエリによってカバーされる必要があるインデックスの割合を表します。 既定値は80です。
- `Top` –取得する提案の数。 1 ~ 100 の整数を指定してください。既定値は5です。

全体的な影響として、インデックスの上位10件の結果がヒットの強調表示と共に返され、結果には同様のスペルの検索語句を含むドキュメントが含まれます。

メソッドは、 `SuggestAsync` `DocumentSuggestResult` クエリ結果を含むオブジェクトを返します。 このオブジェクトが列挙され、各 `Document` オブジェクトがオブジェクトとして作成され `Monkey` 、 `Monkeys` 表示のためにに追加され `ObservableCollection` ます。 次のスクリーンショットは Azure Search から返された提案の結果を示しています。

![提案の結果](azure-search-images/suggest.png)

サンプルアプリケーションで `SuggestAsync` は、ユーザーが検索用語の入力を終了したときにのみメソッドが呼び出されることに注意してください。 ただし、各 keypress でを実行することで、オートコンプリート検索クエリをサポートするために使用することもできます。

## <a name="summary"></a>まとめ

この記事では、Microsoft Azure 検索ライブラリを使用して Azure Search をアプリケーションに統合する方法について説明し Xamarin.Forms ます。 Azure Search は、アップロードされたデータのインデックス作成機能とクエリ機能を提供するクラウドサービスです。 これにより、従来、アプリケーションでの検索機能の実装に関連するインフラストラクチャ要件と検索アルゴリズムの複雑さが解消されます。

## <a name="related-links"></a>関連リンク

- [Azure Search (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Azure Search のドキュメント](/azure/search/)
- [Microsoft Azure 検索ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.Search/)