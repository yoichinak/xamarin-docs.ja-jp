---
title: RESTful Web サービスの使用
description: アプリケーションへの web サービスの統合は、一般的なシナリオです。 この記事では、Xamarin.Forms アプリケーションから rest ベースの web サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 1b25a4a1b65a1473bd122ae9cf7c1a6a72ff9ccc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61328207"
---
# <a name="consuming-a-restful-web-service"></a>RESTful Web サービスの使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)

_アプリケーションへの web サービスの統合は、一般的なシナリオです。この記事では、Xamarin.Forms アプリケーションから rest ベースの web サービスを使用する方法を示します。_

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャ スタイルです。 REST 要求は、web ページを取得し、サーバーにデータを送信する web ブラウザーを使用して、同じ HTTP 動詞を使用して HTTP 経由で行われます。 動詞は次のとおりです。

- **GET**– この操作は web サービスからのデータの取得に使用します。
- **POST** – この操作は、web サービスでデータの新しい項目の作成に使用されます。
- **PUT** – この操作は web サービス上のデータ項目の更新に使用します。
- **PATCH**– この操作は、項目を変更する方法に関する一連の命令を記述することで、web サービス上のデータ項目の更新を使用します。 この動作は、サンプル アプリケーションでは使用されません。
- **DELETE**– この操作を使用して、web サービス上のデータ項目を削除します。

Web サービスの REST に準拠している Api では、RESTful Api と呼ばれ、使用して定義されます。

- ベース URI。
- GET、POST、PUT、PATCH、DELETE などの HTTP メソッド。
- データは、JavaScript Object Notation (JSON) などのメディアの種類。

通常、RESTful web サービスは、クライアントにデータを返す JSON メッセージを使用します。 JSON は、テキスト ベースのデータ交換形式で結果が生成されます compact ペイロードでは、データを送信するときに、帯域幅要件が短くなることです。 サンプル アプリケーション、オープン ソースを使用して[NewtonSoft JSON.NET ライブラリ](http://www.newtonsoft.com/json)およびメッセージを逆シリアル化します。

REST の簡潔さが、モバイル アプリケーションの web サービスにアクセスする主な方法を行うことができました。

サンプル アプリケーションを実行すると、次のスクリーン ショットに示すようにローカルにホストされた REST サービスに接続されます。

![](rest-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
>
>使用することができない場合の ATS を選択することができます、 **HTTPS**プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="consuming-the-web-service"></a>Web サービスの使用

REST サービスでは、ASP.NET Core を使用して記述し、次の操作を提供します。

|操作|HTTP メソッド|相対 URI|パラメーター|
|--- |--- |--- |--- |
|To Do アイテムのリストの取得|GET|/api/todoitems/|
|新しい to do 項目を作成します。|POST|/api/todoitems/|JSON 形式の TodoItem|
|To Do アイテムの更新|PUT|/api/todoitems/|JSON 形式の TodoItem|
|To Do アイテムの削除|Del|/api/todoitems/{id}|

Uri の大部分が含まれて、`TodoItem`パス内の ID。 たとえば、削除するため、 `TodoItem` ID が持つ`6bb8a868-dba1-4f1a-93b7-24ebce87e243`、クライアントに DELETE 要求を送信`http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`。 サンプル アプリケーションで使用されるデータ モデルの詳細については、[、データのモデリング](~/xamarin-forms/data-cloud/walkthrough.md)を参照してください。

Web API フレームワークは、要求を受信すると、アクションに要求をルーティングします。 これらのアクションは単なるパブリック メソッド、`TodoItemsController`クラス。 フレームワークでは、ルーティング テーブルを使ってを呼び出す次のコード例に示されている要求に応答するアクションを決定します。

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

ルーティング テーブルには、ルート テンプレートが含まれています、ルーティング テーブルにルート テンプレートに対して URI と照合しようと、Web API フレームワークは、HTTP 要求を受け取るとします。 一致するクライアントが 404 (見つかりません) エラーを受信ルートを見つけることはできません。 一致するルートが見つかった場合 Web API を選択、コント ローラーとアクションには、次のようにします。

- コント ローラーを検索するには、Web API では、値に"controller"を追加、 *{controller}* 変数。
- アクションを検索するは、Web API は HTTP メソッドが、属性として同じ HTTP メソッドで装飾するコント ローラー アクションではします。
- *{Id}* プレース ホルダー変数は、アクション パラメーターにマップされます。

REST サービスでは、基本認証を使用します。 詳細については、[RESTful web サービスを認証](~/xamarin-forms/data-cloud/authentication/rest.md)を参照してください。 ASP.NET Web API ルーティングの詳細については、次を参照してください。 [ASP.NET Web API におけるルーティング](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api)、ASP.NET web サイト。 ASP.NET Core を使用して REST サービスの構築についての詳細については、[ネイティブ モバイル アプリケーションのバックエンド サービスを作成する](/aspnet/core/mobile/native-mobile-backend/)を参照してください。

`HttpClient`クラスは HTTP 経由で要求を送受信するために使用します。 HTTP 要求を送信する機能を提供し、リソースを識別する URI から HTTP 応答を受信します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、[非同期サポートの概要](~/cross-platform/platform/async.md)を参照してください。

`HttpResponseMessage`クラスは、HTTP 要求が行われた後に、web サービスから受信した HTTP 応答メッセージを表します。 状態コード、ヘッダー、および任意の本文を含む、応答に関する情報が含まれています。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`します。 いずれかを使用して、コンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式に応じて、します。

### <a name="creating-the-httpclient-object"></a>HTTPClient オブジェクトを作成します。

`HttpClient`の次のコード例に示すように HTTP 要求を実行するアプリケーションに必要な限りのオブジェクトが存在するために、クラス レベルでインスタンスが宣言されています。

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    _client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>データの取得

`HttpClient.GetAsync`の次のコード例に示すように、URI で指定された web サービスに GET 要求を送信し、web サービスから応答を受信するメソッドを使用します。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  var response = await _client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST サービスでの HTTP ステータス コードの送信、`HttpResponseMessage.IsSuccessStatusCode`を HTTP 要求が成功または失敗するかどうかを示すために、プロパティ。 この操作の残りの部分は、サービスは、応答の要求された情報は、応答で要求が成功したことを示すには、HTTP 状態コード 200 (OK) を送信します。

HTTP 操作が成功した場合、応答のコンテンツを読み取り表示するため、します。 `HttpResponseMessage.Content`プロパティは、HTTP 応答のコンテンツを表す、`HttpContent.ReadAsStringAsync`メソッドでは、文字列に、HTTP コンテンツを非同期に書き込みます。 このコンテンツはから JSON に変換し、`List`の`TodoItem`インスタンス。

### <a name="creating-data"></a>データの作成

`HttpClient.PostAsync`メソッドは、URI で指定された web サービスに POST 要求を送信するために使用し、次のコード例に示すように、web サービスから応答を受信します。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await _client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");

  }
  ...
}
```

`TodoItem`インスタンスは、web サービスに送信するための JSON ペイロードに変換されます。 このペイロードで要求が行われる前に、web サービスに送信される HTTP コンテンツの本文に埋め込まれている、`PostAsync`メソッド。

REST サービスでの HTTP ステータス コードの送信、`HttpResponseMessage.IsSuccessStatusCode`を HTTP 要求が成功または失敗するかどうかを示すために、プロパティ。 この操作の一般的な応答は次のとおりです。

- **201 (CREATED)** – 応答の送信前に作成される新しいリソースに要求が発生しました。
- **400 (BAD REQUEST)** – 要求がサーバーで認識されません。
- **409 (CONFLICT)** – サーバー上の競合のため、要求が実行できないことです。

### <a name="updating-data"></a>データの更新

`HttpClient.PutAsync`の次のコード例に示すように、URI で指定された web サービスに PUT 要求を送信し、web サービスから応答を受信するメソッドを使用します。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await _client.PutAsync (uri, content);
  ...
}
```
操作、`PutAsync`メソッドは、 `PostAsync` web サービスでデータを作成するために使用されるメソッド。 ただし、web サービスから送信されたような応答は異なります。

REST サービスでの HTTP ステータス コードの送信、`HttpResponseMessage.IsSuccessStatusCode`を HTTP 要求が成功または失敗するかどうかを示すために、プロパティ。 この操作の一般的な応答は次のとおりです。

- **204 (NO CONTENT)** – 要求が正常に処理され、応答は意図的に空白です。
- **400 (BAD REQUEST)** – 要求がサーバーで認識されません。
- **404 (NOT FOUND)** : 要求されたリソースがサーバー上に存在しません。

### <a name="deleting-data"></a>データの削除

`HttpClient.DeleteAsync`の次のコード例に示すように、URI で指定された web サービスに DELETE 要求を送信し、web サービスから応答を受信するメソッドを使用します。

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  var response = await _client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

REST サービスでの HTTP ステータス コードの送信、`HttpResponseMessage.IsSuccessStatusCode`を HTTP 要求が成功または失敗するかどうかを示すために、プロパティ。 この操作の一般的な応答は次のとおりです。

- **204 (NO CONTENT)** – 要求が正常に処理され、応答は意図的に空白です。
- **400 (BAD REQUEST)** – 要求がサーバーで認識されません。
- **404 (NOT FOUND)** : 要求されたリソースがサーバー上に存在しません。

## <a name="related-links"></a>関連リンク

- [ネイティブ モバイル アプリケーションのバックエンド サービスの作成](/aspnet/core/mobile/native-mobile-backend/)
- [この TodoREST (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
