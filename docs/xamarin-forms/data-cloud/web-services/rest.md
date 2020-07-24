---
title: RESTful Web サービスを使用する
description: Web サービスをアプリケーションに統合することは、一般的なシナリオです。 この記事では、アプリケーションから RESTful web サービスを使用する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 972a5ac99ff775ea7301f803c333d8239ae7f193
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938191"
---
# <a name="consume-a-restful-web-service"></a>RESTful Web サービスを使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_Web サービスをアプリケーションに統合することは、一般的なシナリオです。この記事では、アプリケーションから RESTful web サービスを使用する方法について説明 Xamarin.Forms します。_

表現は、web サービスを構築するためのアーキテクチャスタイルです。 REST 要求は、web ブラウザーが web ページの取得やサーバーへのデータの送信に使用するのと同じ HTTP 動詞を使用して HTTP 経由で実行されます。 動詞は次のとおりです。

- **GET** –この操作は、web サービスからデータを取得するために使用されます。
- **POST** -この操作は、web サービスでデータの新しい項目を作成するために使用されます。
- **PUT** –この操作は、web サービス上のデータ項目を更新するために使用されます。
- **PATCH** –この操作は、項目の変更方法に関する一連の命令を記述することによって、web サービス上のデータ項目を更新するために使用されます。 この動詞は、サンプルアプリケーションでは使用されません。
- [**削除**] –この操作は、web サービス上のデータの項目を削除するために使用されます。

REST に準拠する Web サービス Api は RESTful Api と呼ばれ、次のものを使用して定義されます。

- ベース URI。
- GET、POST、PUT、PATCH、DELETE などの HTTP メソッド。
- データのメディアの種類 (JavaScript Object Notation (JSON) など)。

RESTful web サービスは通常、JSON メッセージを使用して、クライアントにデータを返します。 JSON は、コンパクトなペイロードを生成するテキストベースのデータ交換形式であり、データの送信時に帯域幅の要件が少なくなります。 このサンプルアプリケーションでは、オープンソースの[Newtonsoft JSON.NET ライブラリ](https://www.newtonsoft.com/json)を使用してメッセージをシリアル化および逆シリアル化します。

REST の簡潔さは、モバイルアプリケーションで web サービスにアクセスするための主要な方法として役立ちました。

サンプルアプリケーションを実行すると、次のスクリーンショットに示すように、ローカルにホストされている REST サービスに接続します。

![サンプル アプリケーション](rest-images/portal.png)

> [!NOTE]
> IOS 9 以降では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。 IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。
>
>インターネットリソースに対して**HTTPS**プロトコルとセキュリティで保護された通信を使用できない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="consuming-the-web-service"></a>Web サービスを使用する

REST サービスは ASP.NET Core を使用して記述され、次の操作を提供します。

|操作|HTTP メソッド|相対 URI|パラメーター|
|--- |--- |--- |--- |
|To Do アイテムのリストの取得|GET|/api/todoitems/|
|新しい to do 項目を作成する|POST|/api/todoitems/|JSON 形式の TodoItem|
|To Do アイテムの更新|PUT|/api/todoitems/|JSON 形式の TodoItem|
|To Do アイテムの削除|DELETE|/api/todoitems/{id}|

Uri の大部分には、 `TodoItem` パス内の ID が含まれます。 たとえば、ID がであるを削除するために、 `TodoItem` `6bb8a868-dba1-4f1a-93b7-24ebce87e243` クライアントはに delete 要求を送信し `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243` ます。 サンプルアプリケーションで使用されるデータモデルの詳細については、「[データのモデリング](~/xamarin-forms/data-cloud/web-services/introduction.md)」を参照してください。

Web API フレームワークは、要求を受信すると、要求をアクションにルーティングします。 これらのアクションは、単にクラスのパブリックメソッドです `TodoItemsController` 。 フレームワークは、ルーティングテーブルを使用して、次のコード例に示すように、要求に応じて呼び出すアクションを決定します。

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

ルーティングテーブルにはルートテンプレートが含まれており、Web API フレームワークは HTTP 要求を受信すると、URI とルーティングテーブル内のルートテンプレートを一致させようとします。 一致するルートが見つからない場合、クライアントは 404 (見つかりません) エラーを受け取ります。 一致するルートが見つかった場合、Web API は次のようにコントローラーとアクションを選択します。

- コントローラーを検索するために、Web API は *{controller}* 変数の値に "controller" を追加します。
- アクションを検索するために、Web API は HTTP メソッドを参照し、属性と同じ HTTP メソッドで修飾されたコントローラーアクションを調べます。
- *{Id}* プレースホルダー変数がアクションパラメーターにマップされています。

REST サービスは基本認証を使用します。 詳細について[は、「RESTful web サービスの認証](~/xamarin-forms/data-cloud/authentication/rest.md)」を参照してください。 ASP.NET Web API ルーティングの詳細については、ASP.NET web サイトの「[ルーティング」 ASP.NET Web API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api)を参照してください。 ASP.NET Core を使用した REST サービスの構築の詳細については、「[ネイティブモバイルアプリケーションのバックエンドサービスの作成](/aspnet/core/mobile/native-mobile-backend/)」を参照してください。

クラスは、 `HttpClient` HTTP を介して要求を送受信するために使用されます。 HTTP 要求を送信し、URI で識別されるリソースから HTTP 応答を受信するための機能を提供します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、「 [Async Support の概要](~/cross-platform/platform/async.md)」を参照してください。

クラスは、 `HttpResponseMessage` http 要求が行われた後に web サービスから受信した http 応答メッセージを表します。 これには、ステータスコード、ヘッダー、および任意の本文を含む、応答に関する情報が含まれます。 クラスは、 `HttpContent` HTTP 本文とコンテンツヘッダー (やなど) を表し `Content-Type` `Content-Encoding` ます。 コンテンツは、 `ReadAs` `ReadAsStringAsync` `ReadAsByteArrayAsync` データの形式に応じて、やなどの任意のメソッドを使用して読み取ることができます。

### <a name="creating-the-httpclient-object"></a>HTTPClient オブジェクトの作成

`HttpClient`インスタンスはクラスレベルで宣言されるので、次のコード例に示すように、アプリケーションが HTTP 要求を行う必要がある限り、オブジェクトが存続するようにします。

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>データの取得

`HttpClient.GetAsync`メソッドは、URI で指定された web サービスに GET 要求を送信し、web サービスから応答を受信するために使用されます。次のコード例を参照してください。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  HttpResponseMessage response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      string content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST サービスは、http `HttpResponseMessage.IsSuccessStatusCode` 要求が成功したか失敗したかを示す http 状態コードをプロパティに送信します。 この操作では、REST サービスは HTTP 状態コード 200 (OK) を応答に送信します。これは、要求が成功したことと、要求された情報が応答内にあることを示します。

HTTP 操作が成功した場合は、応答の内容が読み取られて表示されます。 `HttpResponseMessage.Content`プロパティは http 応答の内容を表し、 `HttpContent.ReadAsStringAsync` メソッドは http コンテンツを非同期的に文字列に書き込みます。 このコンテンツは、JSON からインスタンスのに変換され `List` `TodoItem` ます。

> [!WARNING]
> メソッドを使用して `ReadAsStringAsync` 大きな応答を取得すると、パフォーマンスに悪影響を及ぼす可能性があります。 このような場合は、応答を直接逆シリアル化して、完全にバッファーに格納する必要がないようにする必要があります。

### <a name="creating-data"></a>データの作成

`HttpClient.PostAsync`メソッドは、URI で指定された web サービスに POST 要求を送信し、次のコード例に示すように web サービスから応答を受信するために使用されます。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  string json = JsonConvert.SerializeObject (item);
  StringContent content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");
  }
  ...
}
```

インスタンスは、 `TodoItem` web サービスに送信するために JSON ペイロードに変換されます。 このペイロードは、メソッドを使用して要求が行われる前に、web サービスに送信される HTTP コンテンツの本文に埋め込まれ `PostAsync` ます。

REST サービスは、http `HttpResponseMessage.IsSuccessStatusCode` 要求が成功したか失敗したかを示す http 状態コードをプロパティに送信します。 この操作の一般的な応答は次のとおりです。

- **201 (作成済み)** –要求により、応答が送信される前に新しいリソースが作成されました。
- **400 (無効な要求)** -要求がサーバーで認識されません。
- **409 (競合)** –サーバーの競合により、要求を実行できませんでした。

### <a name="updating-data"></a>データの更新

`HttpClient.PutAsync`メソッドは、URI で指定された web サービスに PUT 要求を送信し、web サービスから応答を受信するために使用されます。次のコード例を参照してください。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```

メソッドの操作 `PutAsync` は、 `PostAsync` web サービスでデータを作成するために使用されるメソッドと同じです。 ただし、web サービスから送信される可能性のある応答は異なります。

REST サービスは、http `HttpResponseMessage.IsSuccessStatusCode` 要求が成功したか失敗したかを示す http 状態コードをプロパティに送信します。 この操作の一般的な応答は次のとおりです。

- **204 (コンテンツなし)** –要求が正常に処理され、応答が意図的に空白になります。
- **400 (無効な要求)** -要求がサーバーで認識されません。
- **404 (見つかりません)** -要求されたリソースがサーバーに存在しません。

### <a name="deleting-data"></a>データの削除

`HttpClient.DeleteAsync`メソッドは、URI で指定された web サービスに DELETE 要求を送信し、web サービスから応答を受信するために使用されます。次のコード例を参照してください。

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  HttpResponseMessage response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

REST サービスは、http `HttpResponseMessage.IsSuccessStatusCode` 要求が成功したか失敗したかを示す http 状態コードをプロパティに送信します。 この操作の一般的な応答は次のとおりです。

- **204 (コンテンツなし)** –要求が正常に処理され、応答が意図的に空白になります。
- **400 (無効な要求)** -要求がサーバーで認識されません。
- **404 (見つかりません)** -要求されたリソースがサーバーに存在しません。

## <a name="related-links"></a>関連リンク

- [ネイティブモバイルアプリケーションのバックエンドサービスの作成](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
