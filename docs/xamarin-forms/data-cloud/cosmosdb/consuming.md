---
title: Azure Cosmos DB ドキュメント データベースの使用
description: この記事では、Azure Cosmos DB .NET Standard クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法について説明します。
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 79547277b00ae1f1d9b035d5fb08685562cefc79
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53052583"
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Azure Cosmos DB ドキュメント データベースの使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)

_Azure Cosmos DB ドキュメント データベースは、JSON ドキュメント、シームレスなスケーリングとグローバル レプリケーションを必要とするアプリケーションの高速、高可用性でスケーラブルなデータベース サービスを提供するための低待機時間のアクセスを提供する NoSQL データベースです。この記事では、Azure Cosmos DB .NET Standard クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法について説明します。_

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB により、 [Xamarin University](https://university.xamarin.com/)**

Azure のサブスクリプションを使用して、Azure Cosmos DB ドキュメント データベース アカウントをプロビジョニングできます。 各データベース アカウントには、0 個以上のデータベースを持つことができます。 Azure Cosmos db ドキュメント データベースは、ドキュメントのコレクションとユーザーの論理コンテナーです。

Azure Cosmos DB ドキュメント データベースでは、0 個以上のドキュメント コレクションを含めることができます。 各ドキュメントのコレクションを指定して、頻繁にアクセスするコレクションのスループットのアプリケーションと頻繁にアクセスするコレクションのスループットが低下する別のパフォーマンス レベルを持つことができます。

0 個以上の JSON ドキュメントの各ドキュメントのコレクションで構成されます。 コレクション内のドキュメントは、スキーマ フリーし、ため、同じ構造またはフィールドを共有する必要はありません。 ドキュメント コレクションにドキュメントが追加される、Cosmos DB は、そのクエリを実行する利用可能になったに自動的にインデックスします。

開発のために、ドキュメント データベースをエミュレーターで使用できます。 エミュレーターを使用して、アプリケーションの開発方法および Azure サブスクリプションを作成したり、コストをかけたりせずにローカルでテストします。 エミュレーターの詳細については、次を参照してください。 [、Azure Cosmos DB Emulator を使用したローカル開発](/azure/cosmos-db/local-emulator/)します。

この記事と付属のサンプル アプリケーション、タスクが、Azure Cosmos DB ドキュメント データベースに格納されます、Todo リスト アプリケーションを示します。 サンプル アプリケーションの詳細については、次を参照してください。[サンプルについて理解する](~/xamarin-forms/data-cloud/walkthrough.md)します。

Azure Cosmos DB の詳細については、次を参照してください。、 [Azure Cosmos DB ドキュメント](/azure/cosmos-db/)します。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合するためのプロセスは次のとおりです。

1. Cosmos DB アカウントを作成します。 詳細については、次を参照してください。 [Azure Cosmos DB アカウントを作成](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)です。
1. 追加、 [Azure Cosmos DB .NET Standard クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)Xamarin.Forms ソリューションでプラットフォームのプロジェクトに NuGet パッケージ。
1. 追加`using`ディレクティブを`Microsoft.Azure.Documents`、 `Microsoft.Azure.Documents.Client`、および`Microsoft.Azure.Documents.Linq`が Cosmos DB アカウントにアクセスするクラスに名前空間。

次の手順を実行すると、構成して、ドキュメント データベースに対する要求を実行する Azure Cosmos DB .NET Standard クライアント ライブラリを使用できます。

> [!NOTE]
> Azure Cosmos DB .NET Standard クライアント ライブラリは、プラットフォームのプロジェクトとポータブル クラス ライブラリ (PCL) プロジェクトにのみインストールできます。 そのため、サンプル アプリケーションは、コードの重複を回避するために共有アクセス プロジェクト (SAP) です。 ただし、`DependencyService`クラスは、プラットフォーム固有のプロジェクトに含まれている、Azure Cosmos DB .NET Standard クライアント ライブラリのコードを呼び出す PCL プロジェクトで使用できます。

## <a name="consuming-the-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントの使用

`DocumentClient`型は、エンドポイント、資格情報、および Azure Cosmos DB アカウントにアクセスするために使用する接続ポリシーをカプセル化し、構成し、アカウントに対する要求を実行するために使用します。 次のコード例では、このクラスのインスタンスを作成する方法を示します。

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Cosmos DB Uri とプライマリ キーを指定する必要があります、`DocumentClient`コンス トラクター。 これらは、Azure Portal から取得できます。 詳細については、次を参照してください。 [Azure Cosmos DB アカウントへの接続](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect)します。

### <a name="creating-a-database"></a>データベースを作成します。

ドキュメント データベースはドキュメントのコレクションと、ユーザーの論理コンテナーですし、Azure Portal で作成されたまたはプログラムで使用できます、`DocumentClient.CreateDatabaseIfNotExistsAsync`メソッド。

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

`CreateDatabaseIfNotExistsAsync`メソッドを指定します、`Database`オブジェクトを引数として、`Database`としてデータベース名を指定するオブジェクト、`Id`プロパティ。 `CreateDatabaseIfNotExistsAsync`メソッドが存在しないかが既に存在する場合、データベースを返す場合は、データベースを作成します。 ただし、によって返されるデータは無視されます、サンプル アプリケーション、`CreateDatabaseIfNotExistsAsync`メソッド。

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync`メソッドを返します。 を`Task<ResourceResponse<Database>>`オブジェクト、および応答のステータス コードをチェックして、データベースが作成された、または、既存のデータベースが返されたかどうかを判断します。

### <a name="creating-a-document-collection"></a>ドキュメント コレクションを作成します。

ドキュメント コレクションは、JSON ドキュメントのコンテナーであり、Azure Portal で作成されたまたはプログラムで使用できます、`DocumentClient.CreateDocumentCollectionIfNotExistsAsync`メソッド。

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

`CreateDocumentCollectionIfNotExistsAsync`メソッドが 2 つの必須引数が必要です、として指定されたデータベース名、 `Uri`、および`DocumentCollection`オブジェクト。 `DocumentCollection`オブジェクトは、名前を指定したドキュメント コレクションを表す、`Id`プロパティ。 `CreateDocumentCollectionIfNotExistsAsync`メソッドが存在しないかが既に存在する場合、ドキュメント コレクションを返す場合に、ドキュメント コレクションを作成します。 ただし、によって返されるデータは無視されます、サンプル アプリケーション、`CreateDocumentCollectionIfNotExistsAsync`メソッド。

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync`メソッドを返します。 を`Task<ResourceResponse<DocumentCollection>>`ドキュメント コレクションが作成された、または、既存のドキュメント コレクションが返されたかどうかを判断するオブジェクト、および応答のステータス コードをチェックすることができます。

必要に応じて、`CreateDocumentCollectionIfNotExistsAsync`メソッドを指定できますも、`RequestOptions`オブジェクトで、Cosmos DB アカウントに発行された要求に指定できるオプションをカプセル化します。 `RequestOptions.OfferThroughput`ドキュメント コレクションのパフォーマンス レベルを定義するプロパティを使用し、サンプル アプリケーションを 400 要求ユニット/秒に設定されます。 この値は、増加または減少に応じてかどうか、コレクションが頻繁または頻繁にアクセスする必要があります。

> [!IMPORTANT]
> なお、`CreateDocumentCollectionIfNotExistsAsync`メソッドは、価格に影響があります、予約済みスループットで新しいコレクションを作成します。

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>ドキュメント コレクションのドキュメントを検索します。

ドキュメント コレクションの内容は、作成、ドキュメントのクエリを実行して取得できます。 ドキュメントのクエリが作成された、`DocumentClient.CreateDocumentQuery`メソッド。

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

このクエリは、非同期的に指定したコレクションからすべてのドキュメントを取得し、配置内のドキュメントを`List<TodoItem>`表示用のコレクション。

`CreateDocumentQuery<T>`メソッドを指定します、`Uri`ドキュメントに対してクエリを実行する必要がありますのコレクションを表す引数。 この例で、`collectionLink`変数がクラス レベルのフィールドを指定する、`Uri`からドキュメントを取得するドキュメントのコレクションを表します。

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>`メソッドを同期的に、実行を返すクエリを作成、`IQueryable<T>`オブジェクト。 ただし、`AsDocumentQuery`メソッドに変換、`IQueryable<T>`オブジェクトを`IDocumentQuery<T>`非同期的に実行できるオブジェクト。 非同期クエリが実行、`IDocumentQuery<T>.ExecuteNextAsync`メソッドを使用して、ドキュメント データベースから結果の次のページを取得、`IDocumentQuery<T>.HasMoreResults`クエリから返されるその他の結果があるかどうかを示すプロパティです。

ドキュメントは、サーバー側をフィルター処理を含めることによって、`Where`ドキュメント コレクションに対するクエリにフィルター述語を適用すると、クエリ句。

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

このクエリでは、すべてを取得しましているコレクションからドキュメント`Done`プロパティは等しく`false`します。

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>ドキュメント コレクションにドキュメントを挿入

ドキュメントがユーザー定義の JSON コンテンツ、およびドキュメント コレクションに挿入できる、`DocumentClient.CreateDocumentAsync`メソッド。

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync`メソッドを指定します、`Uri`引数には、ドキュメントを挿入するコレクションを表す、`object`を挿入するドキュメントを表す引数。

### <a name="replacing-a-document-in-a-document-collection"></a>ドキュメント コレクション内のドキュメントを置換

ドキュメント コレクションにドキュメントを置き換えることができます、`DocumentClient.ReplaceDocumentAsync`メソッド。

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync`メソッドを指定します、`Uri`を置き換える必要があるコレクション内のドキュメントを表す引数と`object`更新されたドキュメントのデータを表す引数です。

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>ドキュメント コレクションからドキュメントを削除します。

ドキュメントを使用して、ドキュメント コレクションから削除できます、`DocumentClient.DeleteDocumentAsync`メソッド。

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync`メソッドを指定します、`Uri`を削除するか、コレクション内のドキュメントを表す引数。

### <a name="deleting-a-document-collection"></a>ドキュメント コレクションの削除

ドキュメント コレクションのデータベースから削除できる、`DocumentClient.DeleteDocumentCollectionAsync`メソッド。

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync`メソッドを指定します、`Uri`引数を削除するドキュメントのコレクションを表します。 このメソッドを呼び出すことがも削除されるコレクションに格納されたドキュメントに注意してください。

### <a name="deleting-a-database"></a>データベースを削除します。

データベースで Cosmos DB データベース アカウントから削除できる、`DocumentClient.DeleteDatabaesAsync`メソッド。

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync`メソッドを指定します、`Uri`引数を削除するデータベースを表します。 このメソッドを呼び出すことがも削除されるデータベースに格納されているドキュメントのコレクションとドキュメント コレクションに格納されているドキュメントに注意してください。

## <a name="summary"></a>まとめ

この記事では、Azure Cosmos DB .NET Standard クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法について説明します。 Azure Cosmos DB ドキュメント データベースは、JSON ドキュメント、シームレスなスケーリングとグローバル レプリケーションを必要とするアプリケーションの高速、高可用性でスケーラブルなデータベース サービスを提供するための低待機時間のアクセスを提供する NoSQL データベースです。


## <a name="related-links"></a>関連リンク

- [Todo の Azure Cosmos DB (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Azure Cosmos DB のドキュメント](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
