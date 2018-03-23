---
title: Azure Cosmos DB ドキュメント データベースとユーザーの認証
description: Azure の Cosmos DB ドキュメント データベースは、パーティションのコレクションは、無制限のストレージとスループットをサポートしながら複数のサーバーとは、パーティションにまたがることができますをサポートします。 この記事では、ユーザーは、Xamarin.Forms アプリケーションで、各自のドキュメントのみアクセスできるようにパーティション分割のコレクションとアクセス制御を結合する方法について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 8de64d6489b4022e43bcf694f3b13d6f7eaaecbd
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2018
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Azure Cosmos DB ドキュメント データベースとユーザーの認証

_Azure の Cosmos DB ドキュメント データベースは、パーティションのコレクションは、無制限のストレージとスループットをサポートしながら複数のサーバーとは、パーティションにまたがることができますをサポートします。この記事では、ユーザーは、Xamarin.Forms アプリケーションで、各自のドキュメントのみアクセスできるようにパーティション分割のコレクションとアクセス制御を結合する方法について説明します。_

## <a name="overview"></a>概要

パーティションのコレクションを作成するときに、パーティション キーを指定する必要があり、同じパーティション キーを持つドキュメントは、同じパーティションに格納されます。 そのため、パーティション キーとして、ユーザーの id を指定することになりますが、そのユーザーのドキュメントを保存してだけをパーティション分割されたコレクション。 これもにより、ユーザーの数として Azure Cosmos DB ドキュメント データベースをスケール項目が増加します。

任意のコレクションにアクセスを許可する必要があり、SQL API のアクセス制御モデル アクセスの構成要素の 2 種類の定義します。

- **マスター _ キー** Cosmos DB アカウント内のすべてのリソースへの完全な管理者のアクセスを有効にして、Cosmos DB アカウントの作成時に作成されます。
- **リソース トークン**データベースのユーザーと、ユーザーをコレクションやドキュメントなど、特定の Cosmos DB リソースに対して持っているアクセス許可の間のリレーションシップをキャプチャします。

マスター _ キーを公開するには、悪意のあるまたは過失による使用の可能性を Cosmos DB アカウントが表示されます。 ただし、Azure Cosmos DB リソース トークンは、クライアントを読み取り、書き込み、および付与されたアクセス許可に従って Azure Cosmos DB アカウント内の特定のリソースを削除できるように安全なメカニズムを提供します。

要求する一般的な方法は、生成、およびモバイル アプリケーションにリソース トークンを提供するは、リソース トークン ブローカーを使用してです。 次の図は、サンプル アプリケーションでリソース トークン ブローカーを使用して、ドキュメント データベースのデータへのアクセスを管理する方法の大まかな概要を示しています。

![](authentication-images/documentdb-authentication.png "ドキュメント データベース認証プロセス")

リソース トークンのブローカーは、中間層 Web API サービス、Cosmos DB アカウントのマスター _ キーを所有する Azure App Service でホストされています。 サンプル アプリケーションでは、リソース トークン ブローカーを使用して、次のように、ドキュメント データベースのデータへのアクセスを管理します。

1. 、ログインには、Xamarin.Forms アプリケーションは、認証フローを開始する Azure App Service を接続します。
1. Azure App Service では、facebook OAuth 認証フローを実行します。 認証フローが完了したら、Xamarin.Forms アプリケーションはアクセス トークンを受け取ります。
1. Xamarin.Forms アプリケーションでは、アクセス トークンを使用して、リソース トークンのブローカーからリソース トークンを要求します。
1. リソース トークンのブローカーでは、アクセス トークンを使用して、Facebook からのユーザーの id を要求します。 ユーザーの id から Cosmos DB では、認証されたユーザーのパーティション分割コレクションへの読み取り/書き込みアクセス許可に使用されるリソース トークンを要求に使用されます。
1. Xamarin.Forms アプリケーションでは、リソース トークンを使用して、リソース トークンが定義されているアクセス許可を持つ Cosmos DB リソースに直接アクセスします。

> [!NOTE]
> リソース トークンの期限が切れると、データベースに要求する後続のドキュメントには、401 unauthorized 例外が表示されます。 この時点では、Xamarin.Forms アプリケーションは、id を再度確立し、新しいリソース トークンを要求する必要があります。

Cosmos DB がパーティション分割の詳細については、次を参照してください。[ハウツー パーティションおよび Azure Cosmos DB のスケール](/azure/cosmos-db/partition-data/)です。 Cosmos DB アクセス制御の詳細については、次を参照してください。 [Cosmos DB のデータへのアクセスをセキュリティで保護する](/azure/cosmos-db/secure-access-to-data/)と[SQL API のアクセス制御](/rest/api/documentdb/access-control-on-documentdb-resources/)です。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーション リソース トークンのブローカーに統合するためのプロセスは次のとおりです。

1. アクセス制御が使用する Cosmos DB アカウントを作成します。 詳細については、次を参照してください。 [Cosmos DB 構成](#cosmosdb_configuration)です。
1. リソース トークン ブローカーをホストする Azure App Service を作成します。 詳細については、次を参照してください。 [Azure アプリのサービス構成](#app_service_configuration)です。
1. 認証を実行する Facebook アプリを作成します。 詳細については、次を参照してください。 [Facebook アプリの構成](#facebook_configuration)です。
1. Facebook と簡単に認証を実行する Azure App Service を構成します。 詳細については、次を参照してください。 [Azure アプリ サービスの認証構成](#app_service_authentication_configuration)です。
1. Azure App Service と Cosmos DB 通信するために、Xamarin.Forms サンプル アプリケーションを構成します。 詳細については、次を参照してください。 [Xamarin.Forms アプリケーション構成](#forms_application_configuration)です。

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB 構成

アクセス制御が使用する Cosmos DB アカウントを作成するプロセスは次のとおりです。

1. Cosmos DB アカウントを作成します。 詳細については、次を参照してください。 [Cosmos DB の Azure アカウントを作成する](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)です。
1. Cosmos DB アカウントでは、という名前の新しいコレクションを作成`UserItems`のパーティション キーを指定する`/userid`です。

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service の構成

Azure App Service でリソース トークン ブローカーをホストするためのプロセスは次のとおりです。

1. Azure ポータルでは、新しい App Service web アプリを作成します。 詳細については、次を参照してください。 [App Service Environment で web アプリを作成](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)です。
1. Azure ポータルで web アプリのアプリの設定ブレードを開き、次の設定を追加します。
    - `accountUrl` – Cosmos DB アカウントのキーのブレードから Cosmos DB アカウントの URL を指定する必要があります。
    - `accountKey` – Cosmos DB アカウントのキーのブレードから Cosmos DB のマスター _ キー (プライマリまたはセカンダリ) を指定する必要があります。
    - `databaseId` – Cosmos DB データベースの名前を指定する必要があります。
    - `collectionId` – Cosmos DB コレクションの名前を指定する必要があります (この場合、 `UserItems`)。
    - `hostUrl` – アプリ サービス アカウントの概要のブレードから web アプリの URL を指定する必要があります。

    次のスクリーン ショットは、この構成を示しています。

    [![](authentication-images/azure-web-app-settings.png "App Service Web アプリ設定")](authentication-images/azure-web-app-settings-large.png#lightbox "App Service Web アプリ設定")

1. リソース トークンのブローカー ソリューションを Azure App Service web アプリに発行します。

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook アプリの構成

認証を実行する Facebook アプリを作成するプロセスは次のとおりです。

1. Facebook アプリを作成します。 詳細については、次を参照してください。[登録してアプリを構成する](https://developers.facebook.com/docs/apps/register)Facebook デベロッパー センターにします。
1. Facebook ログイン製品をアプリに追加します。 詳細については、次を参照してください。[ビルドされたアプリまたは web サイトを Facebook ログインを追加](https://developers.facebook.com/docs/facebook-login)Facebook デベロッパー センターにします。
1. Facebook ログインを次のように構成します。
  - クライアント OAuth ログインを有効にします。
  - Web OAuth ログインを有効にします。
  - 設定された有効な OAuth リダイレクト URI App Service web アプリの URI を`/.auth/login/facebook/callback`追加されます。

  次のスクリーン ショットは、この構成を示しています。

  ![](authentication-images/facebook-oauth-settings.png "Facebook ログイン OAuth 設定")

詳細については、次を参照してください。 [Facebook にアプリケーションを登録](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)です。

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure App Service の認証の構成

App Service 容易に認証を構成する手順は次のとおりです。

1. Azure ポータルでは、App Service web アプリに移動します。
1. Azure ポータルで開く、認証/承認ブレードし、次の構成を実行します。
  - アプリ サービスの認証を有効にする必要があります。
  - 要求が認証されていないときに実行するアクションを設定する必要があります**Facebook でログイン**です。

  次のスクリーン ショットは、この構成を示しています。

  [![](authentication-images/app-service-authentication-settings.png "App Service Web アプリの認証設定")](authentication-images/app-service-authentication-settings-large.png#lightbox "App Service Web アプリの認証設定")

App Service web アプリは、認証フローを有効にする Facebook アプリとの通信にも構成する必要があります。 これには、Facebook id プロバイダーを選択し、入力して、**アプリ ID**と**アプリ シークレット**Facebook デベロッパー センターで Facebook アプリ設定の値。 詳細については、次を参照してください。 [Facebook の追加情報を、アプリケーションに](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)です。

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms アプリケーションの構成

Xamarin.Forms サンプル アプリケーションを構成する手順は次のとおりです。

1. Xamarin.Forms ソリューションを開きます。
1. 開いている`Constants.cs`し、次の定数の値を更新します。
  - `EndpointUri` – Cosmos DB アカウントのキーのブレードから Cosmos DB アカウントの URL を指定する必要があります。
  - `DatabaseName` – ドキュメント データベースの名前を指定する必要があります。
  - `CollectionName` – 値は、ドキュメントのデータベース コレクションの名前を指定する必要があります (この場合、 `UserItems`)。
  - `ResourceTokenBrokerUrl` – 値は、アプリのサービス アカウントの概要のブレードからリソース トークンのブローカー web アプリの URL をする必要があります。

## <a name="initiating-login"></a>ログインを開始しています

サンプル アプリケーションは、次のコード例に示すように、id プロバイダーの URL にブラウザーをリダイレクトする Xamarin.Auth を使用して、ログイン プロセスを開始します。

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

これにより、Azure App Service と Facebook、Facebook のログイン ページを表示するが開始される OAuth 認証フローします。

![](authentication-images/login.png "Facebook ログイン")

キーを押してログインを取り消すことができます、**キャンセル**ボタンを押すと、iOS で、**戻る**Android、ユーザーが認証されていない場合、id プロバイダーのユーザー インターフェイスは、ボタン画面から削除されます。

Xamarin.Auth の詳細については、次を参照してください。[を Id プロバイダーにユーザーの認証](~/xamarin-forms/data-cloud/authentication/oauth.md)です。

## <a name="obtaining-a-resource-token"></a>リソース トークンを取得します。

次の認証が成功した、`WebRedirectAuthenticator.Completed`イベントが発生します。 次のコード例では、このイベントの処理を示しています。

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

成功した認証の結果は、使用されるアクセス トークン`AuthenticatorCompletedEventArgs.Account`プロパティです。 アクセス トークンが抽出され、リソース トークン ブローカーへの GET 要求で使用される`resourcetoken`API です。

`resourcetoken` API では、Facebook、さらに、Cosmos DB からリソース トークンを要求するために使用されてから、ユーザーの id を要求するアクセス トークンを使用します。 ドキュメント データベース内のユーザーに対して有効なアクセス許可のドキュメントが既に存在する場合を取得し、Xamarin.Forms アプリケーションにリソース トークンを含む JSON ドキュメントが返されました。 ユーザーのアクセス許可が有効なドキュメントが存在しない場合、ユーザー名とアクセス許可がデータベースに作成、ドキュメント、およびリソース トークンが、アクセス許可のドキュメントから抽出され、JSON ドキュメント内の Xamarin.Forms アプリケーションに返されます。

> [!NOTE]
> ドキュメント データベース ユーザーはドキュメント データベースに関連付けられているリソースであり、各データベースには、0 個以上のユーザーが含まれています。 ドキュメント データベース権限は、ドキュメントのデータベース ユーザーに関連付けられているリソースであり、各ユーザーは、0 個以上のアクセス許可を含めることがあります。 アクセス許可リソースでは、ドキュメントなどのリソースにアクセスしようとするときに、ユーザーが必要とするセキュリティ トークンへのアクセスを提供します。

場合、 `resourcetoken` API が正常に完了する、HTTP ステータス コード 200 (OK)、応答でリソース トークンを含む JSON ドキュメントと共に送信されます。 次の JSON データは、標準的な成功の応答メッセージを示しています。

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed`イベント ハンドラーからの応答を読み取り、 `resourcetoken` API し、リソース トークンとユーザー id を抽出します。リソース トークンがへの引数として渡され、`DocumentClient`エンドポイント、資格情報、および Cosmos DB にアクセスするために使用する接続ポリシーをカプセル化し、構成および Cosmos DB に対する要求を実行するために使用するコンス トラクターです。 リソース トークンは、リソースに直接アクセスするには、各要求と一緒に送信され、認証されたユーザーのパーティション分割されたコレクションへの読み取り/書き込みアクセスが許可されていることを示します。

## <a name="retrieving-documents"></a>ドキュメントの取得

認証されたユーザーにのみ属しているドキュメントの取得は、次のコード例では、パーティション キーとしてユーザーの id が含まれており、説明されているドキュメントのクエリを作成することで実現できます。

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

クエリに非同期的に指定したコレクションから、認証されたユーザーに属するドキュメントをすべて取得し、配置で、`List<TodoItem>`表示用のコレクション。

`CreateDocumentQuery<T>`メソッドを指定します、`Uri`ドキュメントについては、クエリを実行する必要がありますのコレクションを表す引数および`FeedOptions`オブジェクト。 `FeedOptions`オブジェクトは、無制限の数の項目が、クエリと、パーティション キーとしてユーザーの id によって返されることを指定します。 これにより、ユーザーのパーティション分割されたコレクション内のドキュメントのみが結果で返されること。

> [!NOTE]
> リソース トークンのブローカーで作成される、アクセス許可のドキュメントが Xamarin.Forms アプリケーションで作成したドキュメントと同じドキュメント コレクションに格納されていることに注意してください。 したがって、ドキュメントのクエリが含まれています、`Where`ドキュメント コレクションに対するクエリにフィルター述語を適用する句。 この句は、ドキュメントのアクセス許可がないドキュメント コレクションから返されることにより、します。

ドキュメントのコレクションからドキュメントの取得の詳細については、次を参照してください。[ドキュメント コレクションのドキュメントを取得する](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query)です。

## <a name="inserting-documents"></a>ドキュメントの挿入

ドキュメントのドキュメントのコレクションに挿入する前に、`TodoItem.UserId`次のコード例で示したように、パーティション キーとして使用されている値を持つプロパティを更新する必要があります。

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

これにより、ドキュメントをユーザーのパーティション分割されたコレクションに挿入されます。

ドキュメントのコレクションをドキュメントに挿入する方法の詳細については、次を参照してください。[ドキュメント コレクション内のドキュメントの挿入](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document)です。

## <a name="deleting-documents"></a>ドキュメントを削除します。

次のコード例に示されていると、パーティションのコレクションからドキュメントを削除するときに、パーティション キー値を指定する必要があります。

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

これにより、Cosmos DB は、パーティション分割されていることを知っているコレクションからドキュメントを削除します。

詳細については、ドキュメント コレクションからドキュメントを削除すると、次を参照してください。[ドキュメント コレクションからドキュメントを削除する](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document)です。

## <a name="summary"></a>まとめ

この記事は、ユーザーは、各自のドキュメント データベース ドキュメント Xamarin.Forms アプリケーションでのみアクセスできるようにパーティション分割のコレクションとアクセス制御を結合する方法を説明します。 パーティション キーとして、ユーザーの id を指定することにより、パーティション分割コレクションは、そのユーザーのドキュメントにのみ格納できます。


## <a name="related-links"></a>関連リンク

- [Todo Azure Cosmos DB Auth (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Azure Cosmos DB ドキュメント データベースの使用](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Azure Cosmos DB のデータへのアクセスをセキュリティで保護します。](/azure/cosmos-db/secure-access-to-data/)
- [SQL API のアクセス制御](/rest/api/documentdb/access-control-on-documentdb-resources/)です。
- [パーティションと Azure Cosmos DB にスケールする方法](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
