---
title: "RESTful Web サービスの使用"
description: "アプリケーションへの web サービスの統合は、一般的なシナリオです。 この記事では、Xamarin.Forms アプリケーションから RESTful web サービスを使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f8b748ad1b57218d1e8aab11bdc1037cf3cfa14c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-a-restful-web-service"></a>RESTful Web サービスの使用

_アプリケーションへの web サービスの統合は、一般的なシナリオです。この記事では、Xamarin.Forms アプリケーションから RESTful web サービスを使用する方法を示します。_

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャのスタイルです。 REST 要求は web ページを取得して、サーバーにデータを送信する web ブラウザーを使用して同じ HTTP 動詞を使用して HTTP 経由で行われます。 動詞は次のとおりです。

- **取得**– この操作は web サービスからのデータの取得に使用します。
- **POST** – この操作は、web サービスでデータの新しい項目の作成に使用します。
- **PUT** – この操作は、web サービス上のデータ項目の更新に使用します。
- **修正プログラム**– この操作は、項目を変更する方法に関する一連の命令を記述することで、web サービス上のデータ項目の更新を使用します。 この動作は、サンプル アプリケーションでは使用されません。
- **削除**– この操作を使用して、web サービス上のデータ項目を削除します。

Web サービスの REST に準拠している Api は、RESTful Api と呼ばれますを使用して定義されます。

- ベース URI。
- GET、POST、PUT、PATCH、DELETE などの HTTP メソッド。
- JavaScript Object Notation (JSON) など、データのメディアの種類。

通常、rESTful web サービスは、クライアントにデータを返す JSON メッセージを使用します。 JSON は、テキスト ベースのデータ交換形式で結果が生成されます compact ペイロードでは、データを送信するときに、帯域幅要件が短くなることです。 サンプル アプリケーション、オープン ソースを使用して[NewtonSoft JSON.NET ライブラリ](http://www.newtonsoft.com/json)およびメッセージを逆シリアル化します。

REST のシンプルさがモバイル アプリケーションで web サービスにアクセスするための主な方法を行うことができました。

REST サービスの設定方法は、サンプル アプリケーションに付属している readme ファイルで確認できます。 ただし、実行時に、サンプル アプリケーションは、接続先データに読み取り専用のアクセスを提供する REST の Xamarin でホストされるサービスの次のスクリーン ショットに示すように。

![](rest-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> Ios 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
>
>使用することがない場合のうち ATS を選択することができます、 **HTTPS**プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。

## <a name="consuming-the-web-service"></a>Web サービスの使用

REST サービスは ASP.NET Core を使用して記述し、次の操作を提供します。

<table>
  <thead>
    <tr>
      <th>操作</th>
      <th>HTTP メソッド</th>
      <th>相対 URI</th>
      <th>パラメーター</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>作業アイテムの一覧を取得します。</td>
      <td>GET</td>
      <td>/api/todoitems/</td>
      <td></td>
    </tr>
    <tr>
      <td>新しい作業項目を作成します。</td>
      <td>POST</td>
      <td>/api/todoitems/</td>
      <td>JSON 形式 <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>作業項目を更新します。</td>
      <td>PUT</td>
      <td>/api/todoitems/</td>
      <td>JSON 形式 <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>作業項目を削除します。</td>
      <td>Del</td>
      <td>/api/todoitems/{id}</td>
      <td></td>
    </tr>
  </tbody>
</table>

Uri の大部分が含まれて、`TodoItem`パス内の ID。 たとえば、削除するため、 `TodoItem` ID がある`6bb8a868-dba1-4f1a-93b7-24ebce87e243`、クライアントに DELETE 要求を送信`http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`です。 サンプル アプリケーションで使用されるデータ モデルの詳細については、次を参照してください。[データ モデリング](~/xamarin-forms/data-cloud/walkthrough.md)です。

Web API フレームワークは、要求を受け取ると、アクションに要求をルーティングします。 これらのアクションは単なるパブリック メソッド、`TodoItemsController`クラスです。 フレームワークでは、ルーティング テーブルを使って、次のコード例に示されている要求に応答を起動する操作を決定します。

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

ルーティング テーブルを含むルート テンプレートでは、し、ルーティング テーブルにルート テンプレートに対して URI と照合しようと Web API フレームワークが HTTP 要求を受信するとします。 一致するルートで見つからない場合、クライアントが 404 (見つかりません) エラーを受信します。 一致するルートが見つかった場合、Web API コント ローラーとアクションとして次に選択されます。

- コント ローラーを検索するには、Web API では、値に"controller"を追加、 *{controller}*変数。
- アクションを検索するには、Web API は HTTP メソッドではし、コント ローラーのアクションは、属性として同じ HTTP メソッドを使用して装飾を確認します。
- *{Id}*をアクション パラメーターのプレース ホルダー変数が割り当てられます。

REST サービスでは、基本認証を使用します。 詳細については、次を参照してください。 [RESTful web サービス認証](~/xamarin-forms/data-cloud/authentication/rest.md)です。 ASP.NET Web API ルーティングの詳細については、次を参照してください。 [ASP.NET Web API でルーティング](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api)ASP.NET web サイトです。 ASP.NET Core を使用して、REST サービスの構築に関する詳細については、次を参照してください。[ネイティブなモバイル アプリケーションのバックエンド サービスの作成](/aspnet/core/mobile/native-mobile-backend/)です。

> [!NOTE]
> サンプル アプリケーションは、web サービスへの読み取り専用のアクセスを提供する Xamarin でホストされている REST サービスを使用します。 そのため、作成、更新、およびデータを削除する操作では、アプリケーションで使用するデータは変更されません。 ただし、REST サービスのホスト可能なバージョンは利用で、 **TodoRESTService**フォルダーに付随する[のサンプル コード](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)です。
> これにより、完全をホストする場合、REST サービス自分で、作成、更新、読み取り、およびデータへのアクセスを削除します。

`HttpClient` HTTP 経由で要求を送受信するクラスを使用します。 HTTP 要求を送信するための機能を提供し、リソースを識別する URI から HTTP 応答を受信します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)です。

`HttpResponseMessage`クラスは、HTTP 要求が行われた後に、web サービスから受信した HTTP 応答メッセージを表します。 応答、状態コード、ヘッダー、およびの本文を含むに関する情報が含まれています。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`です。 いずれかを使用してコンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式に基づいて、します。

### <a name="creating-the-httpclient-object"></a>HTTPClient オブジェクトを作成します。

`HttpClient`次のコード例に示すように HTTP 要求を行うアプリケーションに必要な限りのオブジェクトが存在するように、クラス レベルでインスタンスが宣言されています。

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

`HttpClient.MaxResponseContentBufferSize`プロパティの使用を HTTP 応答メッセージの内容を読み取るときにバッファーに書き込むバイトの最大数を指定します。 このプロパティの既定のサイズは、整数の最大サイズです。 したがって、プロパティが設定値を小さくする、予防策として、web サービスからの応答として、アプリケーションが受け入れるデータの量を制限します。

### <a name="retrieving-data"></a>データの取得

`HttpClient.GetAsync`次のコード例に示すように、URI で指定された web サービスに GET 要求を送信し、web サービスから応答を受信するメソッドを使用します。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST サービスでの HTTP ステータス コードの送信、 `HttpResponseMessage.IsSuccessStatusCode` HTTP 要求の成功または失敗するかどうかを示すために、プロパティです。 この操作の残りの部分は、サービスは、応答を要求が成功したことと、必要な情報が、応答を示す HTTP ステータス コード 200 (OK) を送信します。

HTTP 操作が成功した場合は、表示するための応答のコンテンツを読み取る。 `HttpResponseMessage.Content`プロパティは、HTTP 応答のコンテンツを表す、`HttpContent.ReadAsStringAsync`メソッドでは、文字列を HTTP コンテンツを非同期に書き込みます。 このコンテンツはから JSON に変換されます、`List`の`TodoItem`インスタンス。

### <a name="creating-data"></a>データの作成

`HttpClient.PostAsync`メソッドは、URI で指定された web サービスに POST 要求を送信するために使用し、次のコード例を次に示すように、web サービスから応答を受信します。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

`TodoItem`インスタンスは、web サービスに送信するための JSON ペイロードに変換されます。 このペイロードは、その後で、要求が行われる前に、web サービスに送信される HTTP コンテンツの本文に埋め込ま、`PostAsync`メソッドです。

REST サービスでの HTTP ステータス コードの送信、 `HttpResponseMessage.IsSuccessStatusCode` HTTP 要求の成功または失敗するかどうかを示すために、プロパティです。 この操作に共通の応答は次のとおりです。

- **201 (CREATED)** – 応答の送信前に作成される新しいリソースに、要求が発生しました。
- **400 (BAD REQUEST)** – 要求がサーバーで認識されません。
- **409 (CONFLICT)** – サーバー上の競合があるため要求が実行されませんでした。

### <a name="updating-data"></a>データの更新

`HttpClient.PutAsync`次のコード例に示すように、URI で指定された web サービスに PUT 要求を送信し、web サービスから応答を受信するメソッドを使用します。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
操作、`PutAsync`メソッドは、 `PostAsync` web サービスでのデータの作成に使用されるメソッド。 ただし、web サービスから送信された応答は異なります。

REST サービスでの HTTP ステータス コードの送信、 `HttpResponseMessage.IsSuccessStatusCode` HTTP 要求の成功または失敗するかどうかを示すために、プロパティです。 この操作に共通の応答は次のとおりです。

- **204 (NO CONTENT)** – 要求が正常に処理され、応答は意図的に空白です。
- **400 (BAD REQUEST)** – 要求がサーバーで認識されません。
- **404 (NOT FOUND)** : 要求されたリソースがサーバー上に存在しません。

### <a name="deleting-data"></a>データの削除

`HttpClient.DeleteAsync`次のコード例に示すように、URI で指定された web サービスに DELETE 要求を送信し、web サービスから応答を受信するメソッドを使用します。

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

REST サービスでの HTTP ステータス コードの送信、 `HttpResponseMessage.IsSuccessStatusCode` HTTP 要求の成功または失敗するかどうかを示すために、プロパティです。 この操作に共通の応答は次のとおりです。

- **204 (NO CONTENT)** – 要求が正常に処理され、応答は意図的に空白です。
- **400 (BAD REQUEST)** – 要求がサーバーで認識されません。
- **404 (NOT FOUND)** : 要求されたリソースがサーバー上に存在しません。

## <a name="summary"></a>まとめ

この記事は、Xamarin.Forms アプリケーションから RESTful web サービスを使用する方法を調べるを使用して、`HttpClient`クラスです。 REST のシンプルさがモバイル アプリケーションで web サービスにアクセスするための主な方法を行うことができました。


## <a name="related-links"></a>関連リンク

- [ネイティブ モバイル アプリケーションのバックエンド サービスの作成](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
