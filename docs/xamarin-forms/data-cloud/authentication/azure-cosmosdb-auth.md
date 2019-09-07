---
title: Azure Cosmos DB ドキュメントデータベースと Xamarin. フォームを使用してユーザーを認証する
description: この記事では、各自のドキュメントで、Xamarin.Forms アプリケーションでのみアクセスできるように、Azure Cosmos DB がパーティション分割コレクションとアクセス制御を結合する方法について説明します。
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 7b073e0233fb9c5511593ed80313f402c888c811
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771008"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Azure Cosmos DB ドキュメントデータベースと Xamarin. フォームを使用してユーザーを認証する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB ドキュメント データベースでは、パーティション分割されたコレクションは、無制限のストレージとスループットをサポートしながら複数のサーバーと、パーティションにまたがることができますをサポートします。この記事では、各自のドキュメントで、Xamarin.Forms アプリケーションでのみアクセスできるように、パーティションのコレクションとアクセス制御を結合する方法について説明します。_

## <a name="overview"></a>概要

パーティション分割されたコレクションを作成するときに、パーティション キーを指定する必要があり、同じパーティション キーを持つドキュメントを同じパーティションに格納されます。 そのため、パーティション キーとして、ユーザーの id を指定することになりますが、そのユーザーのドキュメントを保存してのみをパーティション分割コレクション。 これは、ユーザーの数と、Azure Cosmos DB ドキュメント データベースが拡張され、項目を増やすことにもによりします。

任意のコレクションにアクセスを付与する必要があり、SQL API のアクセス制御モデルが 2 つの型へのアクセスの構成体を定義します。

- **マスター _ キー** Cosmos DB アカウント内のすべてのリソースへの完全な管理者のアクセスを有効にして、Cosmos DB アカウントの作成時に作成されます。
- **リソース トークン**データベースのユーザーと、ユーザーが持つコレクションやドキュメントなど、特定の Cosmos DB リソースのアクセス許可の間のリレーションシップをキャプチャします。

マスター _ キーを公開するには、悪意のあるまたは過失による使用の可能性が Cosmos DB アカウントが表示されます。 ただし、Azure Cosmos DB リソース トークンは、クライアントを読み取り、書き込み、および付与されたアクセス許可に従って、Azure Cosmos DB アカウントの特定のリソースを削除できるように安全なメカニズムを提供します。

要求に一般的なアプローチは、生成、およびモバイル アプリケーションにリソース トークンを提供するリソース トークン ブローカーを使用するがします。 次の図は、サンプル アプリケーションでリソース トークン ブローカーを使用して、ドキュメント データベースのデータへのアクセスを管理する方法の概要を示しています。

![](azure-cosmosdb-auth-images/documentdb-authentication.png "ドキュメント データベースの認証プロセス")

リソース トークン ブローカーは、Cosmos DB アカウントのマスター _ キーを所有する、Azure App Service でホストされている、中間層 Web API サービスです。 サンプル アプリケーションでは、リソース トークン ブローカーを使用して、次のように、ドキュメント データベースのデータへのアクセスを管理します。

1. ログインには、Xamarin.Forms アプリケーションは、認証フローを開始する Azure App Service を接続します。
1. Azure App Service では、facebook OAuth 認証フローを実行します。 認証フローが完了すると、Xamarin.Forms アプリケーションはアクセス トークンを受け取ります。
1. Xamarin.Forms アプリケーションでは、アクセス トークンを使用して、リソース トークン ブローカーからリソース トークンを要求します。
1. リソース トークン ブローカーでは、アクセス トークンを使用して Facebook からユーザーの id を要求します。 ユーザーの id はから認証されたユーザーのパーティション分割コレクションへの読み取り/書き込みアクセスを許可するために使用する Cosmos DB リソース トークンを要求に使用されます。
1. Xamarin.Forms アプリケーションでは、リソース トークンを使用して、リソース トークンによって定義されているアクセス許可を持つ Cosmos DB リソースに直接アクセスします。

> [!NOTE]
> リソース トークンの期限が切れると、データベースに要求する後続のドキュメントには 401 unauthorized 例外が表示されます。 この時点では、Xamarin.Forms アプリケーションでは、id を再度確立し、新しいリソース トークンを要求する必要があります。

Cosmos DB がパーティション分割の詳細については、次を参照してください。[パーティション分割と Azure Cosmos DB でのスケーリング方法](/azure/cosmos-db/partition-data/)します。 Cosmos DB へのアクセス制御の詳細については、次を参照してください。 [Cosmos DB のデータへのアクセスをセキュリティで保護する](/azure/cosmos-db/secure-access-to-data/)と[SQL API でのアクセス制御](/rest/api/documentdb/access-control-on-documentdb-resources/)します。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションにリソース トークン ブローカーを統合するためのプロセスは次のとおりです。

1. アクセス制御を使用する Cosmos DB アカウントを作成します。 詳細については、次を参照してください。 [Cosmos DB 構成](#cosmosdb_configuration)します。
1. リソース トークン ブローカーをホストする Azure App Service を作成します。 詳細については、次を参照してください。 [Azure App Service の構成](#app_service_configuration)します。
1. 認証を実行する Facebook アプリを作成します。 詳細については、次を参照してください。 [Facebook アプリの構成](#facebook_configuration)します。
1. Facebook と簡単に認証を実行する Azure App Service を構成します。 詳細については、次を参照してください。 [Azure App Service の認証構成](#app_service_authentication_configuration)します。
1. Azure App Service と Cosmos DB と通信する Xamarin.Forms のサンプル アプリケーションを構成します。 詳細については、次を参照してください。 [Xamarin.Forms アプリケーションの構成](#forms_application_configuration)します。

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB の構成

アクセス制御を使用する Cosmos DB アカウントを作成するプロセスは次のとおりです。

1. Cosmos DB アカウントを作成します。 詳細については、次を参照してください。 [Azure Cosmos DB アカウントを作成](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)です。
1. という名前の新しいコレクションを作成、Cosmos DB アカウントで`UserItems`のパーティション キーを指定する`/userid`します。

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service の構成

Azure App Service でリソース トークン ブローカーをホストするためのプロセスは次のとおりです。

1. Azure portal では、新しい App Service web アプリを作成します。 詳細については、次を参照してください。 [App Service Environment で web アプリの作成](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)です。
1. Azure portal の web アプリでは、アプリの設定 ブレードを開き、次の設定を追加します。
    - `accountUrl` – 値は、Cosmos DB アカウントの [キー] ブレードから Cosmos DB アカウントの URL を指定する必要があります。
    - `accountKey` – 値は、Cosmos DB アカウントの [キー] ブレードから Cosmos DB のマスター キー (プライマリまたはセカンダリ) を指定する必要があります。
    - `databaseId` – 値は、Cosmos DB データベースの名前を指定する必要があります。
    - `collectionId` – 値は、Cosmos DB コレクションの名前を指定する必要があります (この場合、 `UserItems`)。
    - `hostUrl` – 値は、App Service のアカウントの概要ブレードから web アプリの URL を指定する必要があります。

    次のスクリーン ショットは、この構成を示しています。

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web アプリ設定")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web アプリの設定")

1. リソース トークン ブローカーのソリューションを Azure App Service の web アプリに発行します。

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook アプリの構成

認証を実行する Facebook アプリを作成するプロセスは次のとおりです。

1. Facebook アプリを作成します。 詳細については、次を参照してください。[登録とアプリを構成する](https://developers.facebook.com/docs/apps/register)Facebook デベロッパー センターでします。
1. アプリに Facebook ログイン製品を追加します。 詳細については、次を参照してください。[アプリまたは web サイトを Facebook ログインを追加](https://developers.facebook.com/docs/facebook-login)Facebook デベロッパー センターでします。
1. Facebook ログインを次のように構成します。
   - クライアントの OAuth ログインを有効にします。
   - Web の OAuth ログインを有効にします。
   - 有効な OAuth リダイレクト URI、App Service web アプリの URI を使用して設定`/.auth/login/facebook/callback`追加されます。

  次のスクリーン ショットは、この構成を示しています。

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook ログイン OAuth の設定")

詳細については、次を参照してください。 [Facebook にアプリケーションを登録](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)します。

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure App Service 認証の構成

App Service の簡単な認証を構成するためのプロセスは次のとおりです。

1. Azure ポータルでは、App Service の web アプリに移動します。
1. Azure portal でのオープン、認証/承認 ブレードと、次の構成を実行します。
    - App Service の認証を有効にする必要があります。
    - 要求が認証されていない場合に実行するアクションを設定する必要があります**Facebook を使用したログイン**します。

    次のスクリーン ショットは、この構成を示しています。

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web アプリの認証設定")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web アプリの認証設定")

App Service の web アプリは、認証フローを有効にする、Facebook アプリとの通信にも構成する必要があります。 これは、Facebook id プロバイダーを選択し、入力によって実現できます、**アプリ ID**と**アプリ シークレット**Facebook のデベロッパー センターで Facebook アプリ設定の値。 詳細については、次を参照してください。 [Facebook の追加については、アプリケーションに](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)します。

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms アプリケーションの構成

Xamarin.Forms のサンプル アプリケーションを構成するためのプロセスは次のとおりです。

1. Xamarin.Forms ソリューションを開きます。
1. 開いている`Constants.cs`し、次の定数の値を更新します。
    - `EndpointUri` – 値は、Cosmos DB アカウントの [キー] ブレードから Cosmos DB アカウントの URL を指定する必要があります。
    - `DatabaseName` – 値は、ドキュメント データベースの名前を指定する必要があります。
    - `CollectionName` – 値は、ドキュメント データベースのコレクションの名前を指定する必要があります (この場合、 `UserItems`)。
    - `ResourceTokenBrokerUrl` – 値は、App Service のアカウントの概要ブレードからリソース トークン ブローカーの web アプリの URL を指定する必要があります。

## <a name="initiating-login"></a>ログインを開始します。

サンプル アプリケーションでは、次のコード例に示すように、id プロバイダーの URL にブラウザーをリダイレクトする Xamarin.Auth を使用して、ログイン プロセスを開始します。

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

これにより、Azure App Service と Facebook のログイン ページが表示される Facebook の間に開始される OAuth 認証フローをします。

![](azure-cosmosdb-auth-images/login.png "Facebook ログイン")

キーを押して、ログインをキャンセルできます、**キャンセル**iOS かキーを押して、**戻る**android では、ユーザーが認証されていない場合、id プロバイダーのユーザー インターフェイスは、ボタンをクリックします。画面から削除されます。

詳細については、Xamarin.Auth は、次を参照してください。[を Id プロバイダーにユーザーを認証](~/xamarin-forms/data-cloud/authentication/oauth.md)します。

## <a name="obtaining-a-resource-token"></a>リソース トークンを取得します。

次の認証が成功、`WebRedirectAuthenticator.Completed`イベントが発生します。 次のコード例では、このイベントの処理を示しています。

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

成功した認証の結果は、使用可能なアクセス トークンを`AuthenticatorCompletedEventArgs.Account`プロパティ。 アクセス トークンが抽出され、リソース トークン ブローカーの GET 要求で使用される`resourcetoken`API。

`resourcetoken` API は、アクセス トークンを使用して、さらに、Cosmos db リソース トークンを要求するために使用がの Facebook からユーザーの id を要求します。 ドキュメント データベース内のユーザーに対して有効なアクセス許可のドキュメントが既に存在する場合は、それが取得され、Xamarin.Forms アプリケーションにリソース トークンを含む JSON ドキュメントが返されます。 ユーザーの有効なアクセス許可のドキュメントが存在しない場合、ユーザーとアクセス許可が、ドキュメント データベースで作成されたとリソース トークンは、アクセス許可に関するドキュメントから抽出し、JSON ドキュメントでは、Xamarin.Forms アプリケーションに返されます。

> [!NOTE]
> ドキュメント データベースのユーザーは、ドキュメント データベースに関連付けられているリソースであり、各データベースは、0 個以上のユーザーを含めることができます。 ドキュメント データベースのアクセス許可は、ドキュメント データベース ユーザーに関連付けられているリソースであり、各ユーザーは、0 個以上のアクセス許可を含めることができます。 アクセス許可リソースは、ドキュメントなどのリソースにアクセスするときに、ユーザーが必要とするセキュリティ トークンへのアクセスを提供します。

場合、 `resourcetoken` API が正常に完了すると、HTTP 状態コード 200 (OK) 応答には、リソース トークンを含む JSON ドキュメントと共に送信されます。 次の JSON データは、一般的な正常な応答メッセージを示しています。

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed`イベント ハンドラーからの応答を読み取り、 `resourcetoken` API リソースのトークンとユーザー id を抽出します。リソース トークンがへの引数として渡されたし、`DocumentClient`エンドポイント、資格情報、および、Cosmos DB へのアクセスに使用される接続ポリシーをカプセル化し、構成および Cosmos DB に対する要求を実行するために使用するコンス トラクター。 リソース トークンは、直接、リソースにアクセスするには、各要求と一緒に送信され、認証されたユーザーのパーティション分割コレクションへの読み取り/書き込みアクセスが許可されていることを示します。

## <a name="retrieving-documents"></a>ドキュメントを検索します。

次のコード例では、パーティション キーとして、ユーザーの id が含まれます、説明についてはドキュメント クエリを作成して、認証されたユーザーにのみ属しているドキュメントの取得を実現できます。

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

クエリは非同期的に指定したコレクションから、認証されたユーザーに属しているドキュメントをすべて取得しでに配置する`List<TodoItem>`表示用のコレクション。

`CreateDocumentQuery<T>`メソッドを指定します、`Uri`ドキュメントについては、クエリを実行する必要がありますのコレクションを表す引数と`FeedOptions`オブジェクト。 `FeedOptions`オブジェクトは、項目の数に制限はありませんが、クエリと、パーティション キーとして、ユーザーの id によって返されることを指定します。 これにより、ユーザーのパーティション分割されたコレクション内のドキュメントのみが結果に返されること。

> [!NOTE]
> Xamarin.Forms アプリケーションで作成したドキュメントと同じドキュメント コレクションにリソース トークン ブローカーを作成、アクセス許可のドキュメントが保存されるように注意してください。 そのため、ドキュメントのクエリに含まれる、`Where`ドキュメント コレクションに対するクエリにフィルター述語を適用する句。 この句は、ドキュメントのコレクションからのアクセス許可のドキュメントが返されないことにより、します。

ドキュメント コレクションからドキュメントを取得する方法についての詳細については、次を参照してください。[ドキュメント コレクションのドキュメントを取得する](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)します。

## <a name="inserting-documents"></a>ドキュメントを挿入します。

ドキュメント コレクションにドキュメントを挿入する前に、`TodoItem.UserId`プロパティは、次のコード例で示した、パーティション キーとして使用されている値を更新する必要があります。

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

これにより、ユーザーのパーティション分割コレクションにドキュメントが挿入されること。

ドキュメント コレクションにドキュメントを挿入する詳細については、次を参照してください。[ドキュメント コレクションにドキュメントを挿入](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)します。

## <a name="deleting-documents"></a>ドキュメントの削除

次のコード例に示されていると、パーティション分割されたコレクションからドキュメントを削除するときに、パーティション キー値を指定する必要があります。

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

これにより、Cosmos DB は、パーティション分割されていることを知っているからドキュメントを削除するコレクション。

ドキュメント コレクションからドキュメントを削除する方法の詳細については、次を参照してください。[ドキュメント コレクションからドキュメントを削除する](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)します。

## <a name="summary"></a>まとめ

この記事では、独自のドキュメント データベース ドキュメント、Xamarin.Forms アプリケーションでのみアクセスできるように、パーティションのコレクションとアクセス制御を結合する方法について説明します。 パーティション キーとして、ユーザーの id を指定することにより、パーティション分割コレクションの場合、そのユーザーのドキュメントしか格納します。

## <a name="related-links"></a>関連リンク

- [Todo Azure Cosmos DB Auth (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Azure Cosmos DB ドキュメント データベースの使用](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Azure Cosmos DB のデータへのアクセスをセキュリティで保護します。](/azure/cosmos-db/secure-access-to-data/)
- [SQL API でのアクセス制御](/rest/api/documentdb/access-control-on-documentdb-resources/)します。
- [パーティション分割と Azure Cosmos DB でスケーリングする方法](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
