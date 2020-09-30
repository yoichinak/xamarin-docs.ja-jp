---
title: で Azure Cosmos DB ドキュメントデータベースを使用する Xamarin.Forms
description: この記事では、Azure Cosmos DB .NET Standard クライアントライブラリを使用して、Azure Cosmos DB ドキュメントデータベースをアプリケーションに統合する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ef6ccbaac73946ac19f4f5fe194f395234226b5e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562848"
---
# <a name="consume-an-azure-cosmos-db-document-database-in-no-locxamarinforms"></a>で Azure Cosmos DB ドキュメントデータベースを使用する Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)

_Azure Cosmos DB ドキュメントデータベースは、JSON ドキュメントへの低待機時間のアクセスを提供する NoSQL データベースであり、シームレスなスケールとグローバルレプリケーションを必要とするアプリケーション向けに、高速で可用性の高いスケーラブルなデータベースサービスを提供します。この記事では、Azure Cosmos DB .NET Standard クライアントライブラリを使用して、Azure Cosmos DB ドキュメントデータベースをアプリケーションに統合する方法について説明し Xamarin.Forms ます。_

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB ビデオ**

Azure Cosmos DB ドキュメントデータベースアカウントは、Azure サブスクリプションを使用してプロビジョニングできます。 各データベースアカウントには、0個以上のデータベースを含めることができます。 Azure Cosmos DB のドキュメントデータベースは、ドキュメントコレクションとユーザーの論理コンテナーです。

Azure Cosmos DB ドキュメントデータベースには、0個以上のドキュメントコレクションを含めることができます。 各ドキュメントコレクションは異なるパフォーマンスレベルを持つことができるため、頻繁にアクセスされるコレクションに対してより多くのスループットを指定でき、アクセス頻度の低いコレクションのスループットを低くすることができます。

各ドキュメントコレクションは、0個以上の JSON ドキュメントで構成されます。 コレクション内のドキュメントはスキーマフリーであるため、同じ構造またはフィールドを共有する必要はありません。 ドキュメントがドキュメントコレクションに追加されると、Cosmos DB によって自動的にインデックスが付けられ、クエリを使用できるようになります。

開発目的の場合は、エミュレーターを通じてドキュメントデータベースを使用することもできます。 エミュレーターを使用すると、Azure サブスクリプションを作成したりコストをかけたりすることなく、アプリケーションをローカルで開発およびテストできます。 エミュレーターの詳細については、「 [Azure Cosmos DB エミュレーターを使用したローカルでの開発](/azure/cosmos-db/local-emulator/)」を参照してください。

この記事および付随するサンプルアプリケーションは、タスクが Azure Cosmos DB ドキュメントデータベースに格納される Todo リストアプリケーションを示しています。 サンプルアプリケーションの詳細については、「 [サンプルに](~/xamarin-forms/data-cloud/web-services/introduction.md)ついて」を参照してください。

Azure Cosmos DB の詳細については、 [Azure Cosmos DB のドキュメント](/azure/cosmos-db/)を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

## <a name="setup"></a>セットアップ

Azure Cosmos DB ドキュメントデータベースをアプリケーションに統合するプロセスは次のとおり Xamarin.Forms です。

1. Cosmos DB アカウントを作成します。 詳細については、「 [Create a Azure Cosmos DB account](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account)」を参照してください。
1. ソリューションのプラットフォームプロジェクトに [Azure Cosmos DB .NET Standard クライアントライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) NuGet パッケージを追加し Xamarin.Forms ます。
1. `using` `Microsoft.Azure.Documents` 、、およびの各名前空間のディレクティブを、 `Microsoft.Azure.Documents.Client` `Microsoft.Azure.Documents.Linq` Cosmos DB アカウントにアクセスするクラスに追加します。

これらの手順を実行すると、Azure Cosmos DB .NET Standard クライアントライブラリを使用して、ドキュメントデータベースに対する要求を構成および実行できます。

> [!NOTE]
> Azure Cosmos DB .NET Standard クライアントライブラリは、ポータブルクラスライブラリ (PCL) プロジェクトではなく、プラットフォームプロジェクトにのみインストールできます。 このため、サンプルアプリケーションは、コードの重複を回避するための共有アクセスプロジェクト (SAP) です。 ただし、 `DependencyService` クラスを PCL プロジェクトで使用して、プラットフォーム固有のプロジェクトに含まれるクライアントライブラリコード .NET Standard Azure Cosmos DB を呼び出すことができます。

## <a name="consuming-the-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントを使用する

この `DocumentClient` 型は、Azure Cosmos DB アカウントにアクセスするために使用されるエンドポイント、資格情報、および接続ポリシーをカプセル化し、アカウントに対する要求を構成および実行するために使用されます。 次のコード例は、このクラスのインスタンスを作成する方法を示しています。

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

コンストラクターには、Cosmos DB Uri と主キーを指定する必要があり `DocumentClient` ます。 これらは、Azure Portal から取得できます。 詳細については、「 [Connect to a Azure Cosmos DB account](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect)」を参照してください。

### <a name="creating-a-database"></a>データベースの作成

ドキュメントデータベースはドキュメントコレクションとユーザーの論理コンテナーであり、Azure Portal で作成することも、メソッドを使用してプログラムで作成することもでき `DocumentClient.CreateDatabaseIfNotExistsAsync` ます。

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

メソッドは、 `CreateDatabaseIfNotExistsAsync` オブジェクトを引数として指定し `Database` `Database` ます。オブジェクトは、プロパティとしてデータベース名を指定し `Id` ます。 データベースが存在しない場合は、メソッドによって `CreateDatabaseIfNotExistsAsync` 作成されます。データベースが既に存在する場合は、そのデータベースが返されます。 ただし、サンプルアプリケーションでは、メソッドによって返されるデータは無視され `CreateDatabaseIfNotExistsAsync` ます。

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync`メソッドはオブジェクトを返し `Task<ResourceResponse<Database>>` 、応答のステータスコードを確認して、データベースが作成されたか、または既存のデータベースが返されたかを確認できます。

### <a name="creating-a-document-collection"></a>ドキュメントコレクションの作成

ドキュメントコレクションは、JSON ドキュメントのコンテナーであり、Azure Portal で作成することも、メソッドを使用してプログラムで作成することもでき `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` ます。

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

メソッドには、 `CreateDocumentCollectionIfNotExistsAsync` 2 つの強制引数 (として指定されたデータベース名、 `Uri` およびオブジェクト) が必要です `DocumentCollection` 。 オブジェクトは、 `DocumentCollection` プロパティで名前が指定されているドキュメントコレクションを表し `Id` ます。 メソッドは、 `CreateDocumentCollectionIfNotExistsAsync` ドキュメントコレクションが存在しない場合は作成し、既に存在する場合はドキュメントコレクションを返します。 ただし、サンプルアプリケーションでは、メソッドによって返されるデータは無視され `CreateDocumentCollectionIfNotExistsAsync` ます。

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync`メソッドはオブジェクトを返し `Task<ResourceResponse<DocumentCollection>>` 、応答のステータスコードを確認して、ドキュメントコレクションが作成されたかどうか、または既存のドキュメントコレクションが返されたかどうかを確認できます。

必要に応じて、 `CreateDocumentCollectionIfNotExistsAsync` メソッドはオブジェクトを指定することもできます。これには、 `RequestOptions` Cosmos DB アカウントに発行される要求に対して指定できるオプションがカプセル化されます。 プロパティは、 `RequestOptions.OfferThroughput` ドキュメントコレクションのパフォーマンスレベルを定義するために使用されます。サンプルアプリケーションでは、が400要求ユニット/秒に設定されています。 この値は、コレクションが頻繁にアクセスされるかどうかによって増減する必要があります。

> [!IMPORTANT]
> `CreateDocumentCollectionIfNotExistsAsync`メソッドは予約済みのスループットで新しいコレクションを作成することに注意してください。これにより、価格に影響があります。

### <a name="retrieving-document-collection-documents"></a>ドキュメントコレクションのドキュメントの取得

ドキュメントコレクションの内容は、ドキュメントクエリを作成および実行することによって取得できます。 次のメソッドを使用して、ドキュメントクエリが作成され `DocumentClient.CreateDocumentQuery` ます。

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

このクエリは、指定されたコレクションからすべてのドキュメントを非同期に取得し、そのドキュメントを `List<TodoItem>` 表示するためにコレクションに配置します。

メソッドは、 `CreateDocumentQuery<T>` `Uri` ドキュメントに対してクエリを行う必要があるコレクションを表す引数を指定します。 この例では、 `collectionLink` 変数は、 `Uri` ドキュメントを取得するドキュメントコレクションを表すを指定するクラスレベルのフィールドです。

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

メソッドは、 `CreateDocumentQuery<T>` 同期的に実行されるクエリを作成し、オブジェクトを返し `IQueryable<T>` ます。 ただし、メソッドは、 `AsDocumentQuery` `IQueryable<T>` オブジェクトを `IDocumentQuery<T>` 非同期的に実行できるオブジェクトに変換します。 非同期クエリは、メソッドを使用して実行されます。このメソッドは、 `IDocumentQuery<T>.ExecuteNextAsync` ドキュメントデータベースから結果の次のページを取得し、 `IDocumentQuery<T>.HasMoreResults` クエリから返される追加の結果があるかどうかを示すプロパティを使用して実行します。

ドキュメントは、クエリに句を含めることにより、サーバー側でフィルター処理できます。これにより、 `Where` ドキュメントコレクションに対するクエリにフィルター述語が適用されます。

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

このクエリは、プロパティがと等しいコレクションからすべてのドキュメントを取得 `Done` `false` します。

### <a name="inserting-a-document-into-a-document-collection"></a>ドキュメントコレクションへのドキュメントの挿入

ドキュメントは、ユーザーが定義した JSON コンテンツであり、メソッドを使用してドキュメントコレクションに挿入でき `DocumentClient.CreateDocumentAsync` ます。

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

メソッドは、 `CreateDocumentAsync` `Uri` ドキュメントの挿入先のコレクションを表す引数、および `object` 挿入するドキュメントを表す引数を指定します。

### <a name="replacing-a-document-in-a-document-collection"></a>ドキュメントコレクション内のドキュメントの置換

ドキュメントコレクション内のドキュメントは、メソッドを使用して置き換えることができ `DocumentClient.ReplaceDocumentAsync` ます。

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

メソッドは、 `ReplaceDocumentAsync` `Uri` 置換するコレクション内のドキュメントを表す引数と、 `object` 更新されたドキュメントデータを表す引数を指定します。

### <a name="deleting-a-document-from-a-document-collection"></a>ドキュメントコレクションからのドキュメントの削除

ドキュメントは、メソッドを使用してドキュメントコレクションから削除でき `DocumentClient.DeleteDocumentAsync` ます。

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

メソッドは、 `DeleteDocumentAsync` `Uri` 削除する必要があるコレクション内のドキュメントを表す引数を指定します。

### <a name="deleting-a-document-collection"></a>ドキュメントコレクションの削除

ドキュメントコレクションは、次のメソッドを使用してデータベースから削除でき `DocumentClient.DeleteDocumentCollectionAsync` ます。

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

メソッドは、 `DeleteDocumentCollectionAsync` `Uri` 削除するドキュメントコレクションを表す引数を指定します。 このメソッドを呼び出すと、コレクションに格納されているドキュメントも削除されることに注意してください。

### <a name="deleting-a-database"></a>データベースの削除

データベースは、メソッドを使用して Cosmos DB データベースアカウントから削除でき `DocumentClient.DeleteDatabaesAsync` ます。

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

メソッドは、 `DeleteDatabaseAsync` `Uri` 削除するデータベースを表す引数を指定します。 このメソッドを呼び出すと、データベースに格納されているドキュメントコレクションとドキュメントコレクションに格納されているドキュメントも削除されます。

## <a name="summary"></a>まとめ

この記事では、Azure Cosmos DB .NET Standard クライアントライブラリを使用して、Azure Cosmos DB ドキュメントデータベースをアプリケーションに統合する方法について説明しました Xamarin.Forms 。 Azure Cosmos DB ドキュメントデータベースは、JSON ドキュメントへの低待機時間のアクセスを提供する NoSQL データベースであり、シームレスなスケールとグローバルレプリケーションを必要とするアプリケーション向けに、高速で可用性の高いスケーラブルなデータベースサービスを提供します。

## <a name="related-links"></a>関連リンク

- [Todo Azure Cosmos DB (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)
- [Azure Cosmos DB のドキュメント](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard クライアントライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)