---
title: "Azure Cosmos DB ドキュメント データベースの使用"
description: "Azure Cosmos DB ドキュメント データベースは、JSON ドキュメントでは、シームレスなスケールとグローバルのレプリケーションを必要とするアプリケーションのデータベースの高速、高可用性、スケーラブルなサービスを提供する低待機時間のアクセスを提供する NoSQL データベースです。 この記事では、Microsoft Azure DocumentDB クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 41e28366a856f5f0c12db6087117aebb4de72844
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Azure Cosmos DB ドキュメント データベースの使用

_Azure Cosmos DB ドキュメント データベースは、JSON ドキュメントでは、シームレスなスケールとグローバルのレプリケーションを必要とするアプリケーションのデータベースの高速、高可用性、スケーラブルなサービスを提供する低待機時間のアクセスを提供する NoSQL データベースです。この記事では、Microsoft Azure DocumentDB クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法について説明します。_

## <a name="overview"></a>概要

Azure サブスクリプションを使用して Azure Cosmos DB ドキュメント データベース アカウントをプロビジョニングすることができます。 各データベースのアカウントには、0 個以上のデータベースを持つことができます。 Azure Cosmos db ドキュメント データベースは、ドキュメントのコレクションと、ユーザーの論理コンテナーです。

Azure Cosmos DB ドキュメント データベースは、0 個以上のドキュメントのコレクションを含めることがあります。 各ドキュメントのコレクションには、スループットを頻繁にアクセスされるコレクションを指定して頻繁にアクセスされるコレクションで以下のスループットを許可する、さまざまなパフォーマンス レベルを持つことができます。

各ドキュメント コレクションは、0 個以上の JSON ドキュメントで構成されます。 コレクション内のドキュメント スキーマのないされため、同じ構造またはフィールドを共有する必要はありません。 ドキュメントは、ドキュメントのコレクションに追加されるため、Cosmos DB 自動的にインデックスが照会する利用可能になったとします。

開発用に、ドキュメント データベースも、エミュレーターで使用します。 エミュレーターを使用して、アプリケーションの開発方法および Azure サブスクリプションを作成またはコストをかけずことがなく、ローカルでテストします。 エミュレーターの詳細については、次を参照してください。 [Cosmos DB の Azure エミュレーターでローカル開発](/azure/cosmos-db/local-emulator/)です。

この記事と付属のサンプル アプリケーションは、Todo リスト アプリケーションが Azure Cosmos DB ドキュメント データベース内のタスクを格納する場所を示します。 サンプル アプリケーションについての詳細については、次を参照してください。[サンプルを理解する](~/xamarin-forms/data-cloud/walkthrough.md)です。

> [!NOTE]
> DocumentDB クライアント ライブラリは、現在とユニバーサル Windows プラットフォーム (UWP) アプリケーションです。 ただし、DocumentDB クライアント ライブラリを使用する中間層 web サービスを作成して、UWP アプリケーションからサービスを起動するには、UWP アプリケーションから Azure Cosmos DB ドキュメント データベースを使用します。

Azure Cosmos DB の詳細については、次を参照してください。、 [Azure Cosmos DB に関するドキュメント](/azure/cosmos-db/)です。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合するためのプロセスは次のとおりです。

1. Cosmos DB アカウントを作成します。 詳細については、次を参照してください。 [Cosmos DB アカウントを作成する](/azure/cosmos-db/documentdb-dotnetcore-get-started#step-1-create-a-documentdb-account)です。
1. 追加、 [DocumentDB クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)Xamarin.Forms ソリューション内のプラットフォーム プロジェクトに NuGet パッケージです。
1. 追加`using`のディレクティブを`Microsoft.Azure.Documents`、 `Microsoft.Azure.Documents.Client`、および`Microsoft.Azure.Documents.Linq`Cosmos DB アカウントにアクセスするクラスに名前空間。

次の手順を実行すると、ドキュメントのデータベースに対して要求を構成および実行 DocumentDB クライアント ライブラリを使用できます。

> [!NOTE]
> Azure DocumentDB のクライアント ライブラリは、プラットフォームのプロジェクトとポータブル クラス ライブラリ (PCL) プロジェクトにないにのみインストールできます。 したがって、サンプル アプリケーションは、コードの重複を避けるために共有アクセス プロジェクト (SAP) です。 ただし、`DependencyService`プラットフォーム固有のプロジェクトに含まれる Azure DocumentDB クライアント ライブラリ コードを呼び出すために PCL プロジェクトにクラスを使用できます。

## <a name="consuming-the-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントの使用

`DocumentClient`型は、エンドポイント、資格情報、および Azure Cosmos DB アカウントへのアクセスに使用する接続ポリシーをカプセル化し、構成して、アカウントに対する要求を実行するために使用します。 次のコード例では、このクラスのインスタンスを作成する方法を示します。

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Cosmos DB Uri とプライマリ キーを指定する必要があります、`DocumentClient`コンス トラクターです。 これらは、Azure ポータルから取得することができます。 詳細については、次を参照してください。 [Cosmos DB の Azure アカウントに関連付ける](/azure/cosmos-db/documentdb-dotnetcore-get-started#a-idconnectastep-3-connect-to-an-azure-cosmos-db-account)です。

### <a name="creating-a-database"></a>データベースを作成します。

ドキュメント データベースは、ドキュメントのコレクションと、ユーザーの論理コンテナーでき、プログラムを使用するか、Azure ポータルで作成された、`DocumentClient.CreateDatabaseIfNotExistsAsync`メソッド。

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

`CreateDatabaseIfNotExistsAsync`メソッドを指定します、`Database`オブジェクトを引数として、`Database`としてデータベース名を指定するオブジェクト、`Id`プロパティです。 `CreateDatabaseIfNotExistsAsync`メソッドが、存在しないか、既に存在する場合、データベースが返されます場合、データベースを作成します。 ただし、によって返されるデータは無視されます、サンプル アプリケーション、`CreateDatabaseIfNotExistsAsync`メソッドです。

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync`メソッドを返します、`Task<ResourceResponse<Database>>`オブジェクト、および応答のステータス コードをチェックして、データベースが作成された、または既存のデータベースが返されたかどうかを判断します。

### <a name="creating-a-document-collection"></a>ドキュメント コレクションを作成します。

ドキュメント コレクションは、JSON ドキュメントのコンテナーであり、Azure ポータルで作成されたプログラムを使用するかを指定できます、`DocumentClient.CreateDocumentCollectionIfNotExistsAsync`メソッド。

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

`CreateDocumentCollectionIfNotExistsAsync`メソッドが必要です: 2 つの必須引数として指定されたデータベース名、 `Uri`、および`DocumentCollection`オブジェクト。 `DocumentCollection`オブジェクトで指定する名前を持つドキュメント コレクションを表します、`Id`プロパティです。 `CreateDocumentCollectionIfNotExistsAsync`またはが既に存在する場合、ドキュメントのコレクションを返しますが存在しない場合、メソッドがドキュメントのコレクションを作成します。 ただし、によって返されるデータは無視されます、サンプル アプリケーション、`CreateDocumentCollectionIfNotExistsAsync`メソッドです。

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync`メソッドを返します、`Task<ResourceResponse<DocumentCollection>>`ドキュメント コレクションが作成された、または既存のドキュメント コレクションが返されたかどうかを決定するオブジェクト、および応答のステータス コードをチェックすることができます。

必要に応じて、`CreateDocumentCollectionIfNotExistsAsync`方法を指定することも、 `RequestOptions` Cosmos DB アカウントに対して発行された要求に対して指定できるオプションをカプセル化するオブジェクト。 `RequestOptions.OfferThroughput`プロパティを使用してドキュメント コレクションのパフォーマンス レベルを定義し、1 秒あたりの 400 の要求単位に設定されているアプリケーション、サンプルにします。 この値を増加または減少に応じてかどうか、コレクションやあまり頻繁にアクセスする必要があります。

> [!IMPORTANT]
> なお、`CreateDocumentCollectionIfNotExistsAsync`メソッドは予約済みスループット、価格に影響のある新しいコレクションを作成します。

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>ドキュメントのコレクションのドキュメントを取得します。

ドキュメント コレクションの内容は、作成し、ドキュメントのクエリを実行して取得できます。 ドキュメントのクエリが作成、`DocumentClient.CreateDocumentQuery`メソッド。

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

このクエリは、非同期的に、指定されたコレクションからすべてのドキュメントを取得し、配置内のドキュメント、`List<TodoItem>`表示用のコレクション。

`CreateDocumentQuery<T>`メソッドを指定します、`Uri`ドキュメントに対してクエリを実行する必要がありますのコレクションを表す引数。 この例では、`collectionLink`変数はクラス レベルのフィールドを指定する、`Uri`からドキュメントを取得するドキュメントのコレクションを表します。

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>`メソッドを同期的に、実行を返すクエリを作成、`IQueryable<T>`オブジェクト。 ただし、`AsDocumentQuery`メソッドに変換、`IQueryable<T>`オブジェクトを`IDocumentQuery<T>`オブジェクトに非同期的に実行することができます。 非同期クエリの実行が、`IDocumentQuery<T>.ExecuteNextAsync`を持つドキュメント データベースから結果の次のページを取得するメソッド、`IDocumentQuery<T>.HasMoreResults`クエリから返されるその他の結果があるかどうかを示すプロパティです。

表示されるドキュメントを含めることによってサーバー側をフィルター処理、`Where`ドキュメント コレクションに対するクエリにフィルター述語を適用すると、クエリ内の句。

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

このクエリでは、すべてを取得、コレクションからドキュメント`Done`プロパティと等しい`false`です。

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>ドキュメントをドキュメントのコレクションに挿入します。

ドキュメントがユーザー定義 JSON コンテンツ、および使用して、ドキュメント コレクションに挿入することができます、`DocumentClient.CreateDocumentAsync`メソッド。

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync`メソッドを指定します、`Uri`ドキュメントを挿入する、コレクションを表す引数および`object`を挿入するドキュメントを表す引数。

### <a name="replacing-a-document-in-a-document-collection"></a>ドキュメント コレクション内のドキュメントを置き換える

ドキュメントを使用して、ドキュメント コレクションで置換できる、`DocumentClient.ReplaceDocumentAsync`メソッド。

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync`メソッドを指定します、`Uri`を置き換える必要がある、コレクション内のドキュメントを表す引数および`object`更新されたドキュメントのデータを表す引数です。

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>ドキュメントのコレクションからドキュメントを削除します。

ドキュメントを使用して、ドキュメント コレクションから削除できます、`DocumentClient.DeleteDocumentAsync`メソッド。

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync`メソッドを指定します、`Uri`を削除するか、コレクション内のドキュメントを表す引数です。

### <a name="deleting-a-document-collection"></a>ドキュメントのコレクションを削除します。

使用して、データベースからドキュメント コレクションを削除することができます、`DocumentClient.DeleteDocumentCollectionAsync`メソッド。

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync`メソッドを指定します、`Uri`引数を削除するドキュメントのコレクションを表します。 このメソッドを呼び出すことがも削除されるコレクションに格納されたドキュメントに注意してください。

### <a name="deleting-a-database"></a>データベースを削除します。

データベースを使用して、Cosmos DB データベース アカウントから削除できます、`DocumentClient.DeleteDatabaesAsync`メソッド。

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync`メソッドを指定します、`Uri`引数を削除するデータベースを表します。 このメソッドを呼び出すことがも削除される、データベースに格納されているドキュメントのコレクションとドキュメントのコレクションに格納されているドキュメントに注意してください。

## <a name="summary"></a>まとめ

この記事では、Microsoft Azure DocumentDB クライアント ライブラリを使用して、Xamarin.Forms アプリケーションに Azure Cosmos DB ドキュメント データベースを統合する方法について説明します。 Azure Cosmos DB ドキュメント データベースは、JSON ドキュメントでは、シームレスなスケールとグローバルのレプリケーションを必要とするアプリケーションのデータベースの高速、高可用性、スケーラブルなサービスを提供する低待機時間のアクセスを提供する NoSQL データベースです。


## <a name="related-links"></a>関連リンク

- [TodoDocumentDB (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Cosmos DB のドキュメント](/azure/cosmos-db/)
- [DocumentDB クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
