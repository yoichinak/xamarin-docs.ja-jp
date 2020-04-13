---
title: Azure Cosmos DB ドキュメント データベースと Xamarin.Forms を使用してユーザーを認証します。
description: この記事では、アクセス制御を Azure Cosmos DB パーティションコレクションと組み合わせる方法について説明し、ユーザーが Xamarin.Forms アプリケーションで自分のドキュメントにのみアクセスできるようにします。
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e79e647d64103b6d257de7233f488899bcaff40
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388604"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Azure Cosmos DB ドキュメント データベースと Xamarin.Forms を使用してユーザーを認証します。

[![サンプルの](~/media/shared/download.png)ダウンロード サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB ドキュメント データベースは、パーティション分割されたコレクションをサポートし、複数のサーバーとパーティションにまたがって、ストレージとスループットを無制限にサポートできます。この資料では、アクセス制御をパーティション分割されたコレクションと組み合わせる方法について説明し、ユーザーが Xamarin.Forms アプリケーションで自分のドキュメントにのみアクセスできるようにします。_

## <a name="overview"></a>概要

パーティション・コレクションを作成する際には、パーティション・キーを指定する必要があり、同じパーティション・キーを持つ文書は同じパーティションに保管されます。 したがって、ユーザーの ID をパーティション キーとして指定すると、そのユーザーのドキュメントのみを格納するパーティション分割コレクションが作成されます。 これにより、ユーザー数とアイテム数が増加するにつれて、Azure Cosmos DB ドキュメント データベースも拡張されます。

任意のコレクションにアクセスを許可する必要があり、SQL API アクセス制御モデルは 2 種類のアクセス構成を定義します。

- **マスターキー**は、Cosmos DB アカウント内のすべてのリソースに対する完全な管理アクセスを有効にし、Cosmos DB アカウントの作成時に作成されます。
- **リソース トークン**は、データベースのユーザーと、コレクションやドキュメントなどの特定の Cosmos DB リソースに対するユーザーの権限との関係をキャプチャします。

マスターキーを公開すると、Cosmos DB アカウントが悪意のあるまたは過失による使用の可能性に開かれます。 ただし、Azure Cosmos DB リソース トークンは、付与されたアクセス許可に従って、クライアントが Azure Cosmos DB アカウント内の特定のリソースを読み取り、書き込み、および削除できるようにする安全なメカニズムを提供します。

リソース トークンをモバイル アプリケーションに要求、生成、および配信する一般的な方法は、リソース トークン ブローカーを使用することです。 次の図は、サンプル アプリケーションがリソース トークン ブローカーを使用してドキュメント データベース データへのアクセスを管理する方法の概要を示しています。

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

リソース トークン ブローカーは、Azure アプリ サービスでホストされる中間層の Web API サービスであり、Cosmos DB アカウントのマスター キーを所有しています。 サンプル アプリケーションでは、リソース トークン ブローカーを使用して、次のようにドキュメント データベース データへのアクセスを管理します。

1. ログイン時に、Xamarin.Forms アプリケーションは Azure アプリ サービスにアクセスして認証フローを開始します。
1. Azure アプリ サービスは、Facebook で OAuth 認証フローを実行します。 認証フローが完了すると、Xamarin.Forms アプリケーションはアクセス トークンを受け取ります。
1. Xamarin.Forms アプリケーションは、アクセス トークンを使用して、リソース トークン ブローカーからリソース トークンを要求します。
1. リソーストークンブローカーは、アクセストークンを使用して、ユーザーのIDをFacebookに要求します。 その後、ユーザーの ID を使用して Cosmos DB にリソース トークンを要求し、認証されたユーザーのパーティションコレクションへの読み取り/書き込みアクセスを許可します。
1. Xamarin.Forms アプリケーションは、リソース トークンを使用して、リソース トークンによって定義されたアクセス許可を持つ Cosmos DB リソースに直接アクセスします。

> [!NOTE]
> リソース トークンの有効期限が切れると、後続のドキュメント データベース要求は 401 の不正な例外を受け取ります。 この時点で、Xamarin.Forms アプリケーションは ID を再確立し、新しいリソース トークンを要求する必要があります。

Cosmos DB パーティション分割の詳細については[、「Azure Cosmos DB でパーティション分割とスケールを行う方法](/azure/cosmos-db/partition-data/)」を参照してください。 Cosmos DB アクセス制御の詳細については、SQL API の[「Cosmos DB データへのアクセスのセキュリティ保護](/azure/cosmos-db/secure-access-to-data/)」および「[アクセス制御」](/rest/api/documentdb/access-control-on-documentdb-resources/)を参照してください。

## <a name="setup"></a>セットアップ

Xamarin.Forms アプリケーションにリソース トークン ブローカーを統合するプロセスは次のとおりです。

1. アクセス制御を使用する Cosmos DB アカウントを作成します。 詳細については[、「Cosmos DB の設定](#cosmosdb_configuration)」を参照してください。
1. リソース トークン ブローカーをホストする Azure アプリ サービスを作成します。 詳細については、「 [Azure アプリ サービスの構成](#app_service_configuration)」を参照してください。
1. 認証を実行する Facebook アプリを作成します。 詳細については[、「Facebook アプリの構成」を](#facebook_configuration)参照してください。
1. Facebook で簡単に認証を実行するように Azure アプリ サービスを構成します。 詳細については、「 [Azure アプリ サービスの認証の構成](#app_service_authentication_configuration)」を参照してください。
1. Azure アプリ サービスおよび Cosmos DB と通信するように Xamarin.Forms サンプル アプリケーションを構成します。 詳細については、「 [Xamarin.Forms アプリケーションの構成](#forms_application_configuration)」を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)がない場合は、開始する前に[無料のアカウント](https://aka.ms/azfree-docs-mobileapps)を作成します。

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure コスモス DB の構成

アクセス制御を使用する Cosmos DB アカウントを作成するプロセスは次のとおりです。

1. Cosmos DB アカウントを作成します。 詳細については、「 [Azure Cosmos DB アカウントの作成](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)」を参照してください。
1. Cosmos DB アカウントで、 のパーティション`UserItems`キーを指定して、 という`/userid`名前の新しいコレクションを作成します。

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure アプリ サービスの構成

Azure App サービスでリソース トークン ブローカーをホストするプロセスは次のとおりです。

1. Azure ポータルで、新しいアプリ サービス Web アプリを作成します。 詳細については[、「App Service 環境での Web アプリの作成](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)」を参照してください。
1. Azure ポータルで、Web アプリの [アプリの設定] ブレードを開き、次の設定を追加します。
    - `accountUrl`– 値は、Cosmos DB アカウントの [キー] ブレードの [Cosmos DB アカウント URL] である必要があります。
    - `accountKey`– 値は、Cosmos DB アカウントの [キー] ブレードの [Cosmos DB マスター キー (プライマリまたはセカンダリ)] である必要があります。
    - `databaseId`– 値は Cosmos DB データベースの名前にする必要があります。
    - `collectionId`– 値は、Cosmos DB コレクションの名前でなければなりません (この場合`UserItems`は.
    - `hostUrl`– 値は、App Service アカウントの [概要] ブレードの Web アプリの URL である必要があります。

    次のスクリーン ショットは、この構成を示しています。

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. リソース トークン ブローカー ソリューションを Azure アプリ サービス Web アプリに発行します。

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>フェイスブックアプリの構成

認証を実行するFacebookアプリを作成するプロセスは次のとおりです。

1. フェイスブックアプリを作成します。 詳細については、「Facebook デベロッパー センター[でアプリを登録および構成する](https://developers.facebook.com/docs/apps/register)」を参照してください。
1. Facebookログイン製品をアプリに追加します。 詳細については、Facebook デベロッパー センターの[「アプリまたはウェブサイトに](https://developers.facebook.com/docs/facebook-login)Facebook ログインを追加する」を参照してください。
1. 次のようにFacebookログインを設定します。
   - クライアント OAuth ログインを有効にします。
   - Web OAuth ログインを有効にします。
   - `/.auth/login/facebook/callback`有効な OAuth リダイレクト URI をアプリ サービス Web アプリの URI に設定し、追加します。

  次のスクリーン ショットは、この構成を示しています。

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

詳細については、「[アプリケーションを Facebook に登録する](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)」を参照してください。

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure アプリ サービス認証の構成

App Service の簡単な認証を構成するプロセスは次のとおりです。

1. Azure ポータルで、アプリ サービス Web アプリに移動します。
1. Azure ポータルで、[認証/承認] ブレードを開き、次の構成を実行します。
    - アプリ サービス認証を有効にする必要があります。
    - リクエストが認証されていない場合に実行するアクションは **、Facebookでログイン**に設定する必要があります。

    次のスクリーン ショットは、この構成を示しています。

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

また、認証フローを有効にするために、アプリ サービス Web アプリを Facebook アプリと通信するように構成する必要があります。 そのためには、Facebook ID プロバイダーを選択し、Facebook デベロッパー センターの Facebook アプリ設定から**アプリ ID**と**アプリ シークレット**の値を入力します。 詳しくは、[アプリケーションに Facebook 情報を追加するを参照してください](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)。

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms アプリケーションの構成

Xamarin.Forms サンプル アプリケーションを構成するプロセスは次のとおりです。

1. Xamarin.Forms ソリューションを開きます。
1. 次`Constants.cs`の定数の値を開いて更新します。
    - `EndpointUri`– 値は、Cosmos DB アカウントの [キー] ブレードの [Cosmos DB アカウント URL] である必要があります。
    - `DatabaseName`– 値は、ドキュメント データベースの名前である必要があります。
    - `CollectionName`– 値は、ドキュメント データベース コレクションの名前である必要があります (`UserItems`この場合は、 )。
    - `ResourceTokenBrokerUrl`– 値は、App Service アカウントの [概要] ブレードのリソース トークン ブローカー Web アプリの URL である必要があります。

## <a name="initiating-login"></a>ログインの実行

サンプル アプリケーションは、次のコード例に示すように、ブラウザーを ID プロバイダー URL にリダイレクトすることによってログイン プロセスを開始します。

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

これにより、Azure アプリ サービスと Facebook の間で OAuth 認証フローが開始され、Facebook ログイン ページが表示されます。

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

ログインは、iOS の **[キャンセル]** ボタンを押すか、Android の [**戻る**] ボタンを押してキャンセルできます。

## <a name="obtaining-a-resource-token"></a>リソース トークンの取得

認証が成功すると、`WebRedirectAuthenticator.Completed`イベントが発生します。 このイベントの処理を示すコード例を次に示します。

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

認証が成功した結果は、アクセス トークンで、利用可能な`AuthenticatorCompletedEventArgs.Account`プロパティです。 アクセス トークンは抽出され、リソース トークン ブローカーの`resourcetoken`API に対する GET 要求で使用されます。

この`resourcetoken`API はアクセストークンを使用して、ユーザーの ID を Facebook に要求し、さらに Cosmos DB からリソーストークンを要求するために使用されます。 ドキュメント データベース内のユーザーに対して有効なアクセス許可ドキュメントが既に存在する場合、そのドキュメントが取得され、リソース トークンを含む JSON ドキュメントが Xamarin.Forms アプリケーションに返されます。 ユーザーに対して有効なアクセス許可ドキュメントが存在しない場合、ユーザーと権限がドキュメント データベースに作成され、リソース トークンがアクセス許可ドキュメントから抽出され、JSON ドキュメントの Xamarin.Forms アプリケーションに返されます。

> [!NOTE]
> ドキュメント データベース ユーザーはドキュメント データベースに関連付けられたリソースであり、各データベースには 0 個以上のユーザーが含まれる場合があります。 ドキュメント データベース権限は、ドキュメント データベース ユーザーに関連付けられたリソースであり、各ユーザーには 0 個以上の権限が含まれている場合があります。 アクセス許可リソースは、ドキュメントなどのリソースにアクセスするときにユーザーが必要とするセキュリティ トークンへのアクセスを提供します。

API`resourcetoken`が正常に完了すると、HTTP ステータス コード 200 (OK) とリソース トークンを含む JSON ドキュメントが送信されます。 次の JSON データは、典型的な成功した応答メッセージを示しています。

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

イベント`WebRedirectAuthenticator.Completed`ハンドラーは、API から`resourcetoken`応答を読み取り、リソース トークンとユーザー ID を抽出します。リソース トークンは、コンストラクタに`DocumentClient`引数として渡され、Cosmos DB へのアクセスに使用されるエンドポイント、資格情報、および接続ポリシーをカプセル化し、Cosmos DB に対するリクエストの設定と実行に使用されます。 リソース トークンは、リソースに直接アクセスする要求ごとに送信され、認証されたユーザーのパーティション コレクションへの読み取り/書き込みアクセスが許可されることを示します。

## <a name="retrieving-documents"></a>ドキュメントの取得

認証されたユーザーのみに属するドキュメントの取得は、ユーザーの id をパーティション キーとして含むドキュメント クエリを作成することによって実現できます。

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

クエリは、認証されたユーザーに属するすべてのドキュメントを指定されたコレクションから非同期的に取得し、`List<TodoItem>`表示用のコレクションに格納します。

この`CreateDocumentQuery<T>`メソッドは、`Uri`ドキュメント、およびオブジェクトのクエリを実行するコレクションを表す引数を`FeedOptions`指定します。 オブジェクト`FeedOptions`は、クエリによって返される項目の数を無制限に指定し、パーティション キーとしてユーザーの id を指定します。 これにより、ユーザーのパーティションコレクション内のドキュメントのみが結果として返されます。

> [!NOTE]
> リソース トークン ブローカーによって作成されるアクセス許可ドキュメントは、Xamarin.Forms アプリケーションによって作成されたドキュメントと同じドキュメント コレクションに格納されます。 したがって、ドキュメント クエリには、`Where`ドキュメント コレクションに対するクエリにフィルター処理述語を適用する句が含まれています。 この句を使用すると、アクセス許可ドキュメントがドキュメント コレクションから返されないようにします。

ドキュメント コレクションからドキュメントを取得する方法の詳細については、「[ドキュメント コレクション ドキュメントの取得](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)」を参照してください。

## <a name="inserting-documents"></a>ドキュメントの挿入

ドキュメント コレクションにドキュメントを挿入する前に、次`TodoItem.UserId`のコード例に示すように、パーティション キーとして使用されている値でプロパティを更新する必要があります。

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

これにより、ドキュメントはユーザーのパーティション分割されたコレクションに挿入されます。

ドキュメントコレクションへのドキュメントの挿入の詳細については、「ドキュメント[コレクションへのドキュメントの挿入](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)」を参照してください。

## <a name="deleting-documents"></a>ドキュメントの削除

パーティション のキー値は、次のコード例に示すように、パーティションコレクションからドキュメントを削除するときに指定する必要があります。

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

これにより、Cosmos DB は、ドキュメントを削除するパーティション分割コレクションを認識できます。

ドキュメント コレクションからドキュメントを削除する方法の詳細については、「ドキュメント[コレクションからのドキュメントの削除](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、アクセス制御をパーティション分割されたコレクションと組み合わせる方法について説明し、ユーザーが Xamarin.Forms アプリケーションで自分のドキュメント データベース ドキュメントにのみアクセスできるようにします。 ユーザーの ID をパーティション キーとして指定すると、パーティション分割されたコレクションは、そのユーザーのドキュメントのみを格納できます。

## <a name="related-links"></a>関連リンク

- [トド・アズール・コスモス DB 認証 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Azure Cosmos DB ドキュメント データベースの使用](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Azure Cosmos DB データへのアクセスのセキュリティ保護](/azure/cosmos-db/secure-access-to-data/)
- [SQL API のアクセス制御](/rest/api/documentdb/access-control-on-documentdb-resources/)
- [Azure Cosmos DB でのパーティション分割とスケーリングの方法](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB クライアント ライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
