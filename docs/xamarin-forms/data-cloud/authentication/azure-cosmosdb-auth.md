---
title: "Azure Cosmos DB ドキュメントデータベースを使用したユーザーの認証 Xamarin.Forms " と "説明:" この記事では、ユーザーがアプリケーション内の自分のドキュメントにのみアクセスできるように、アクセス制御を Azure Cosmos DB パーティション分割されたコレクションと組み合わせる方法について説明し Xamarin.Forms ます。 "
ms. 製品: xamarin ms. assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 06/16/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Azure Cosmos DB ドキュメントデータベースを使用してユーザーを認証するXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB ドキュメントデータベースは、複数のサーバーとパーティションにまたがることができるパーティション分割コレクションをサポートし、無制限のストレージとスループットをサポートします。この記事では、アクセス制御をパーティション分割されたコレクションと組み合わせることで、ユーザーがアプリケーション内の自分のドキュメントにしかアクセスできないようにする方法について説明し Xamarin.Forms ます。_

## <a name="overview"></a>概要

パーティション分割されたコレクションを作成するときにパーティションキーを指定する必要があり、同じパーティションキーを持つドキュメントは同じパーティションに格納されます。 そのため、ユーザーの id をパーティションキーとして指定すると、そのユーザーのドキュメントのみを格納するパーティション分割されたコレクションになります。 これにより、ユーザーとアイテムの数が増えるにつれて、Azure Cosmos DB ドキュメントデータベースが拡張されるようになります。

すべてのコレクションに対するアクセス権を付与する必要があります。また、SQL API アクセス制御モデルでは、次の2種類のアクセス構成要素が定義されています。

- **マスターキー**を使用すると、Cosmos DB アカウント内のすべてのリソースへの完全な管理アクセスが可能になり、Cosmos DB アカウントの作成時に作成されます。
- **リソーストークン**は、データベースのユーザーと、コレクションやドキュメントなどの特定の Cosmos DB リソースに対するユーザーのアクセス許可との間のリレーションシップをキャプチャします。

マスターキーを公開すると、Cosmos DB アカウントが開き、悪意のあるまたは誤っが使用される可能性があります。 ただし、Azure Cosmos DB リソーストークンは、許可されたアクセス許可に従って、Azure Cosmos DB アカウント内の特定のリソースの読み取り、書き込み、および削除をクライアントに許可する安全なメカニズムを提供します。

リソーストークンを要求、生成、およびモバイルアプリケーションに配信するための一般的な方法は、リソーストークンブローカーを使用することです。 次の図は、サンプルアプリケーションがリソーストークンブローカーを使用してドキュメントデータベースデータへのアクセスを管理する方法の概要を示しています。

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

リソーストークンブローカーは、Azure App Service でホストされる中間層 Web API サービスで、Cosmos DB アカウントのマスターキーを所有します。 このサンプルアプリケーションでは、リソーストークンブローカーを使用して、次のようにドキュメントデータベースデータへのアクセスを管理します。

1. ログイン時に、 Xamarin.Forms アプリケーションは Azure App Service に接続して認証フローを開始します。
1. Azure App Service は、Facebook で OAuth 認証フローを実行します。 認証フローが完了すると、 Xamarin.Forms アプリケーションはアクセストークンを受け取ります。
1. アプリケーションは、 Xamarin.Forms アクセストークンを使用してリソーストークンブローカーからリソーストークンを要求します。
1. リソーストークンブローカーは、アクセストークンを使用して、Facebook からユーザーの id を要求します。 その後、ユーザーの id を使用して Cosmos DB からリソーストークンを要求します。これは、認証されたユーザーのパーティション分割コレクションへの読み取り/書き込みアクセス権を付与するために使用されます。
1. アプリケーションはリソーストークンを使用して、リソース Xamarin.Forms トークンによって定義されたアクセス許可を持つ Cosmos DB リソースに直接アクセスします。

> [!NOTE]
> リソーストークンの有効期限が切れると、後続のドキュメントデータベース要求では、401の承認されていない例外が発生します。 この時点で、 Xamarin.Forms アプリケーションは id を再確立し、新しいリソーストークンを要求する必要があります。

Cosmos DB パーティション分割の詳細については、「 [Azure Cosmos DB をパーティション分割およびスケールインする方法](/azure/cosmos-db/partition-data/)」を参照してください。 Cosmos DB アクセス制御の詳細については、「 [SQL API での](/rest/api/documentdb/access-control-on-documentdb-resources/)Cosmos DB データとアクセス制御[へのアクセスのセキュリティ保護](/azure/cosmos-db/secure-access-to-data/)」を参照してください。

## <a name="setup"></a>セットアップ

リソーストークンブローカーをアプリケーションに統合するプロセス Xamarin.Forms は次のとおりです。

1. アクセス制御を使用する Cosmos DB アカウントを作成します。 詳細については、「 [Azure Cosmos DB 構成](#azure-cosmos-db-configuration)」を参照してください。
1. リソーストークンブローカーをホストするための Azure App Service を作成します。 詳細については、「 [Azure App Service 構成](#azure-app-service-configuration)」を参照してください。
1. 認証を実行する Facebook アプリを作成します。 詳細については、「 [Facebook アプリの構成](#facebook-app-configuration)」を参照してください。
1. Facebook で簡単な認証を実行するように Azure App Service を構成します。 詳細については、「 [Azure App Service 認証の構成](#azure-app-service-authentication-configuration)」を参照してください。
1. Xamarin.FormsAzure App Service と Cosmos DB と通信するようにサンプルアプリケーションを構成します。 詳細については、「 [ Xamarin.Forms アプリケーションの構成](#xamarinforms-application-configuration)」を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB 構成

アクセス制御を使用する Cosmos DB アカウントを作成するプロセスは次のとおりです。

1. Cosmos DB アカウントを作成します。 詳細については、「 [Create a Azure Cosmos DB account](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)」を参照してください。
1. Cosmos DB アカウントに、のパーティションキーを指定して、という名前の新しいコレクションを作成し `UserItems` `/userid` ます。

### <a name="azure-app-service-configuration"></a>Azure App Service 構成

Azure App Service でリソーストークンブローカーをホストするプロセスは次のとおりです。

1. Azure portal で、新しい App Service web アプリを作成します。 詳細については、「 [App Service Environment での web アプリの作成](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)」を参照してください。
1. Azure portal で、web アプリの [アプリの設定] ブレードを開き、次の設定を追加します。
    - `accountUrl`–この値は、Cosmos DB アカウントの [キー] ブレードの Cosmos DB アカウント URL である必要があります。
    - `accountKey`–この値は、Cosmos DB アカウントの [キー] ブレードの Cosmos DB マスターキー (プライマリまたはセカンダリ) にする必要があります。
    - `databaseId`–値は Cosmos DB データベースの名前にする必要があります。
    - `collectionId`–値は Cosmos DB コレクションの名前 (この場合は) である必要があり `UserItems` ます。
    - `hostUrl`–この値は、App Service アカウントの [概要] ブレードにある web アプリの URL にする必要があります。

    次のスクリーンショットは、この構成を示しています。

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. リソーストークンブローカーソリューションを Azure App Service web アプリに発行します。

### <a name="facebook-app-configuration"></a>Facebook アプリの構成

認証を実行する Facebook アプリを作成するプロセスは次のとおりです。

1. Facebook アプリを作成します。 詳細については、Facebook デベロッパーセンターでの[アプリの登録と構成](https://developers.facebook.com/docs/apps/register)に関する説明を参照してください。
1. Facebook ログイン製品をアプリに追加します。 詳細については、Facebook デベロッパーセンターの「[アプリまたは Web サイトに Facebook ログインを追加する](https://developers.facebook.com/docs/facebook-login)」を参照してください。
1. 次のように Facebook ログインを構成します。
   - クライアントの OAuth ログインを有効にします。
   - Web OAuth ログインを有効にします。
   - 有効な OAuth リダイレクト URI を App Service web アプリの URI (追加) に設定し `/.auth/login/facebook/callback` ます。

  次のスクリーンショットは、この構成を示しています。

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

詳細については、「[アプリケーションを Facebook に登録する](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)」を参照してください。

### <a name="azure-app-service-authentication-configuration"></a>認証構成の Azure App Service

App Service 簡単な認証を構成するためのプロセスは次のとおりです。

1. Azure Portal で、App Service web アプリに移動します。
1. Azure Portal で、[認証/承認] ブレードを開き、次の構成を実行します。
    - App Service 認証を有効にする必要があります。
    - 要求が認証されていないときに実行するアクションは、 **Facebook を使用してログイン**するように設定する必要があります。

    次のスクリーンショットは、この構成を示しています。

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

また、Facebook アプリと通信して認証フローを有効にするように、App Service web アプリを構成する必要があります。 これを実現するには、facebook id プロバイダーを選択し、facebook デベロッパーセンターの Facebook アプリ設定から**アプリ ID**と**アプリシークレット**の値を入力します。 詳細については、「[アプリケーションに Facebook 情報を追加する](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)」を参照してください。

### <a name="xamarinforms-application-configuration"></a>Xamarin.Formsアプリケーションの構成

サンプルアプリケーションを構成するためのプロセス Xamarin.Forms は次のとおりです。

1. ソリューションを開き Xamarin.Forms ます。
1. を開き、 `Constants.cs` 次の定数の値を更新します。
    - `EndpointUri`–この値は、Cosmos DB アカウントの [キー] ブレードの Cosmos DB アカウント URL である必要があります。
    - `DatabaseName`–この値は、ドキュメントデータベースの名前にする必要があります。
    - `CollectionName`–この値は、ドキュメントデータベースコレクション (この場合は) の名前にする必要があり `UserItems` ます。
    - `ResourceTokenBrokerUrl`–この値は、App Service アカウントの [概要] ブレードにあるリソーストークンブローカー web アプリの URL にする必要があります。

## <a name="initiating-login"></a>ログインを開始しています

サンプルアプリケーションでは、次のコード例に示すように、ブラウザーを id プロバイダーの URL にリダイレクトすることによって、ログインプロセスを開始します。

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

これにより、Azure App Service と Facebook の間で OAuth 認証フローが開始され、Facebook ログインページが表示されます。

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

ログインは、iOS の [**キャンセル**] ボタンを押すか、Android の [**戻る**] ボタンを押すことで取り消すことができます。この場合、ユーザーは認証されず、id プロバイダーのユーザーインターフェイスが画面から削除されます。

## <a name="obtaining-a-resource-token"></a>リソーストークンを取得する

認証に成功すると、 `WebRedirectAuthenticator.Completed` イベントが発生します。 このイベントを処理するコード例を次に示します。

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

認証が成功すると、アクセストークンとして使用できるようになり `AuthenticatorCompletedEventArgs.Account` ます。 アクセストークンが抽出され、リソーストークンブローカーの API に対する GET 要求で使用され `resourcetoken` ます。

API は、 `resourcetoken` アクセストークンを使用して Facebook からユーザーの id を要求します。これは、Cosmos DB からリソーストークンを要求するために使用されます。 ドキュメントデータベース内のユーザーに対して有効なアクセス許可ドキュメントが既に存在する場合は、そのドキュメントが取得され、リソーストークンを含む JSON ドキュメントがアプリケーションに返され Xamarin.Forms ます。 ユーザーに対して有効なアクセス許可ドキュメントが存在しない場合は、ドキュメントデータベースにユーザーとアクセス許可が作成され、リソーストークンはアクセス許可ドキュメントから抽出され、 Xamarin.Forms JSON ドキュメント内のアプリケーションに返されます。

> [!NOTE]
> ドキュメントデータベースユーザーは、ドキュメントデータベースに関連付けられたリソースであり、各データベースには0個以上のユーザーを含めることができます。 ドキュメントデータベースのアクセス許可は、ドキュメントデータベースユーザーに関連付けられたリソースであり、各ユーザーに0個以上のアクセス許可を含めることができます。 アクセス許可リソースは、ドキュメントなどのリソースにアクセスしようとしたときにユーザーが必要とするセキュリティトークンへのアクセスを提供します。

API が `resourcetoken` 正常に完了した場合は、応答で HTTP 状態コード 200 (OK) と共に、リソーストークンを含む JSON ドキュメントが送信されます。 次の JSON データは、一般的な成功応答メッセージを示しています。

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed`イベントハンドラーは API から応答を読み取り、 `resourcetoken` リソーストークンとユーザー id を抽出します。次に、リソーストークンが引数としてコンストラクターに渡されます。これにより、 `DocumentClient` Cosmos DB にアクセスするために使用されるエンドポイント、資格情報、および接続ポリシーがカプセル化され、Cosmos DB に対して要求を構成および実行するために使用されます。 リソーストークンは、リソースに直接アクセスするために各要求と共に送信され、認証されたユーザーのパーティション分割コレクションへの読み取り/書き込みアクセスが許可されることを示します。

## <a name="retrieving-documents"></a>ドキュメントの取得

認証されたユーザーにのみ属するドキュメントを取得するには、ユーザーの id をパーティションキーとして含むドキュメントクエリを作成し、次のコード例に示すようにします。

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

クエリは、認証されたユーザーに属するすべてのドキュメントを指定したコレクションから非同期に取得し、それらを `List<TodoItem>` 表示するためにコレクションに配置します。

メソッドは、 `CreateDocumentQuery<T>` `Uri` ドキュメントのクエリ対象のコレクションを表す引数とオブジェクトを指定し `FeedOptions` ます。 オブジェクトは、 `FeedOptions` クエリによって返される項目の数に制限がなく、ユーザーの id をパーティションキーとして返すことを指定します。 これにより、ユーザーのパーティション分割されたコレクション内のドキュメントだけが結果に返されます。

> [!NOTE]
> リソーストークンブローカーによって作成されたアクセス許可ドキュメントは、アプリケーションによって作成されたドキュメントと同じドキュメントコレクションに格納されていることに注意して Xamarin.Forms ください。 したがって、ドキュメントクエリには、 `Where` ドキュメントコレクションに対するクエリにフィルター述語を適用する句が含まれています。 この句により、ドキュメントコレクションからアクセス許可ドキュメントが返されないようにします。

ドキュメントコレクションからドキュメントを取得する方法の詳細については、「[ドキュメントコレクションドキュメントの取得](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#retrieving-document-collection-documents)」を参照してください。

## <a name="inserting-documents"></a>挿入 (ドキュメントを)

ドキュメントコレクションにドキュメントを挿入する前に、 `TodoItem.UserId` 次のコード例に示すように、パーティションキーとして使用されている値を使用してプロパティを更新する必要があります。

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

これにより、ユーザーのパーティション分割されたコレクションにドキュメントが挿入されるようになります。

ドキュメントコレクションへのドキュメントの挿入の詳細については、「ドキュメント[コレクションへのドキュメントの挿入](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting-a-document-into-a-document-collection)」を参照してください。

## <a name="deleting-documents"></a>ドキュメントの削除

パーティションキー値は、次のコード例に示すように、パーティション分割されたコレクションからドキュメントを削除するときに指定する必要があります。

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

これにより、Cosmos DB は、ドキュメントを削除するパーティション分割されたコレクションを認識します。

ドキュメントコレクションからドキュメントを削除する方法の詳細については、「ドキュメント[コレクションからドキュメントを削除](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting-a-document-from-a-document-collection)する」を参照してください。

## <a name="summary"></a>まとめ

この記事では、アクセス制御をパーティション分割されたコレクションと組み合わせる方法を説明しました。これにより、ユーザーはアプリケーション内の自分のドキュメントデータベースドキュメントにしかアクセスでき Xamarin.Forms ません。 ユーザーの id をパーティションキーとして指定すると、パーティション分割されたコレクションはそのユーザーのドキュメントのみを格納できます。

## <a name="related-links"></a>関連リンク

- [Todo Azure Cosmos DB Auth (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Azure Cosmos DB ドキュメント データベースの使用](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Azure Cosmos DB データへのアクセスのセキュリティ保護](/azure/cosmos-db/secure-access-to-data/)
- [SQL API のアクセス制御](/rest/api/documentdb/access-control-on-documentdb-resources/)。
- [Azure Cosmos DB でのパーティション分割とスケーリングの方法](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB クライアントライブラリ](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
