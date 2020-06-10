---
title: "認証と承認" 説明: "この章では、eShopOnContainers モバイルアプリがコンテナー化されたマイクロサービスに対して認証と承認を実行する方法について説明します。"
ms. 製品: xamarin ms. assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 08/08/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="authentication-and-authorization"></a>認証と承認

認証とは、ユーザーからの名前やパスワードなどの id 資格情報を取得し、それらの資格情報を機関に対して検証するプロセスです。 資格情報が有効な場合、資格情報を送信したエンティティは認証済み id と見なされます。 Id が認証されると、その id が特定のリソースにアクセスできるかどうかが承認プロセスによって判断されます。

ASP.NET MVC web アプリケーションと通信するアプリに認証と承認を統合する方法は多数あります。これには、 Xamarin.Forms ASP.NET Core id、外部認証プロバイダー (Microsoft、Google、Facebook、Twitter など)、認証ミドルウェアなどが含まれます。 EShopOnContainers モバイルアプリは、ユーザー id を使用するコンテナー化された id マイクロサービスで認証と承認を実行します。 モバイルアプリは、ユーザーを認証したり、リソースにアクセスしたりするために、ユーザーのセキュリティトークンを要求します。 ユーザーの代わりにユーザーがトークンを発行するようにするには、ユーザーがユーザーをユーザーに登録する必要があります。 ただし、ユーザーインターフェイスは認証用のユーザーインターフェイスまたはデータベースを提供しません。 そのため、eShopOnContainers 参照アプリケーションでは、ASP.NET Core Id がこの目的に使用されます。

## <a name="authentication"></a>認証

アプリケーションが現在のユーザーの id を認識している必要がある場合は、認証が必要です。 ユーザーを識別するための主なメカニズムは、ASP.NET Core Id メンバーシップシステムで、開発者が構成したデータストアにユーザー情報が格納されます。これには、ASP.NET Core ます。 通常、このデータストアは EntityFramework ストアになりますが、カスタムストアまたはサードパーティのパッケージを使用して、Azure storage、Azure Cosmos DB、またはその他の場所に id 情報を格納することができます。

ローカルユーザーデータストアを使用する認証シナリオでは、ASP.NET MVC web アプリケーションの一般的なように、cookie を介して要求間で id 情報を保持する認証シナリオでは、ASP.NET Core Id が適切なソリューションです。 ただし、cookie は常にデータを永続化および転送するための自然な手段ではありません。 たとえば、モバイルアプリからアクセスされる RESTful エンドポイントを公開する ASP.NET Core web アプリケーションは、通常、ベアラートークン認証を使用する必要があります。これは、このシナリオでは cookie を使用できないためです。 ただし、ベアラートークンは、モバイルアプリから作成された web 要求の authorization ヘッダーに簡単に取得して含めることができます。

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>サーバー4を使用したベアラートークンの発行

ユーザー Id [4](https://github.com/IdentityServer/IdentityServer4)は、オープンソースの OpenID Connect および OAuth 2.0 フレームワーク ASP.NET Core 用です。ローカル ASP.NET Core id ユーザーのセキュリティトークンを発行するなど、多くの認証と承認のシナリオで使用できます。

> [!NOTE]
> OpenID Connect と OAuth 2.0 はよく似ていますが、さまざまな役割があります。

OpenID Connect は、OAuth 2.0 プロトコル上の認証レイヤーです。 OAuth 2 は、アプリケーションが Security Token Service からアクセストークンを要求し、Api との通信に使用できるようにするプロトコルです。 この委任により、認証と承認を一元化できるため、クライアントアプリケーションと Api の両方の複雑さが軽減されます。

OpenID Connect と OAuth 2.0 の組み合わせによって、認証と API アクセスの2つの基本的なセキュリティ上の懸念が組み合わされています。また、このプロトコルの実装は、サーバー4です。

EShopOnContainers 参照アプリケーションなどのクライアントからマイクロサービスへの直接通信を使用するアプリケーションでは、図9-1 に示すように、セキュリティトークンサービス (STS) として機能する専用の認証マイクロサービスを使用して、ユーザーを認証できます。 クライアントからマイクロサービスへの直接通信の詳細については、「[クライアントとマイクロサービス間の通信](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication-between-client-and-microservices)」を参照してください。

![](authentication-and-authorization-images/authentication.png "Authentication by a dedicated authentication microservice")

**図 9-1:** 専用認証マイクロサービスによる認証

EShopOnContainers モバイルアプリは、id マイクロサービスと通信します。これは、認証を実行するためにユーザー id を使用し、Api のアクセス制御を行います。 そのため、モバイルアプリは、ユーザーを認証したり、リソースにアクセスしたりするために、ユーザーのトークンを要求します。

- ユーザーの認証は、認証プロセスの結果を表す*id*トークンを要求するモバイルアプリによって実現されます。 最低でも、ユーザーの識別子と、ユーザーが認証された方法とタイミングに関する情報が含まれています。 また、追加の id データを含めることもできます。
- サービスを使用してリソースにアクセスするには、*アクセス*トークンを要求するモバイルアプリを使用します。これにより、API リソースにアクセスできるようになります。 クライアントはアクセストークンを要求し、API に転送します。 アクセストークンには、クライアントとユーザーに関する情報 (存在する場合) が含まれます。 Api は、その情報を使用して、データへのアクセスを承認します。

> [!NOTE]
> クライアントは、トークンを要求できるようにする前に、クライアントをサーバーに登録する必要があります。

### <a name="adding-identityserver-to-a-web-application"></a>Web アプリケーションへのサーバーの追加

ASP.NET Core web アプリケーションで、サーバー4を使用するには、web アプリケーションの Visual Studio ソリューションに追加する必要があります。 詳細については、「サーバーのドキュメント」の「[概要](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html)」を参照してください。

Web アプリケーションの Visual Studio ソリューションに提供されたサーバーは、web アプリケーションの HTTP 要求処理パイプラインに追加して、OpenID Connect および OAuth 2.0 エンドポイントへの要求を処理できるようにする必要があります。 これは、次の `Configure` `Startup` コード例に示すように、web アプリケーションのクラスのメソッドで実現されます。

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Web アプリケーションの HTTP 要求処理パイプラインでは順序が重要です。 そのため、ログイン画面を実装する UI フレームワークの前に、ユーザーがパイプラインに追加されている必要があります。

### <a name="configuring-identityserver"></a>サーバーの構成

`ConfigureServices` `Startup` `services.AddIdentityServer` EShopOnContainers reference アプリケーションの次のコード例に示すように、メソッドを呼び出すことによって、web アプリケーションのクラスのメソッドで、サービスを構成する必要があります。

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

メソッドを呼び出した後 `services.AddIdentityServer` 、次を構成するために、追加の Fluent api が呼び出されます。

- 署名に使用される資格情報。
- ユーザーがアクセスを要求する API および id リソース。
- 要求トークンに接続するクライアント。
- ASP.NET Core Id。

> [!TIP]
> サーバー4の構成を動的に読み込みます。 サーバー4の Api を使用すると、構成オブジェクトのメモリ内の一覧から、サーバーを構成できます。 EShopOnContainers reference アプリケーションでは、これらのメモリ内コレクションはアプリケーションにハードコーディングされています。 ただし、運用環境のシナリオでは、構成ファイルまたはデータベースから動的に読み込むことができます。

ASP.NET Core Id を使用するようにユーザーを構成する方法の詳細については、「[ユーザーによる ASP.NET Core id の使用](https://identityserver4.readthedocs.io/en/latest/quickstarts/6_aspnet_identity.html)」のドキュメントを参照してください。

#### <a name="configuring-api-resources"></a>API リソースの構成

API リソースを構成する場合、メソッドには `AddInMemoryApiResources` コレクションが必要 `IEnumerable<ApiResource>` です。 次のコード例は、 `GetApis` eShopOnContainers reference アプリケーションでこのコレクションを提供するメソッドを示しています。

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

このメソッドは、サービスが注文とバスケットの Api を保護する必要があることを指定します。 そのため、これらの Api を呼び出す場合は、サービスによって管理されるアクセストークンが必要になります。 型の詳細については `ApiResource` 、「Azure server 4 のドキュメント」の[API リソース](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html)を参照してください。

#### <a name="configuring-identity-resources"></a>Id リソースの構成

Id リソースを構成する場合、メソッドには `AddInMemoryIdentityResources` コレクションが必要 `IEnumerable<IdentityResource>` です。 Id リソースとは、ユーザー ID、名前、電子メールアドレスなどのデータのことです。 各 id リソースには一意の名前が付けられ、任意のクレームの種類を割り当てることができます。これは、ユーザーの id トークンに含まれます。 次のコード例は、 `GetResources` eShopOnContainers reference アプリケーションでこのコレクションを提供するメソッドを示しています。

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

OpenID Connect の仕様では、[標準の id リソース](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)が指定されています。 最小要件は、ユーザーの一意の ID を生成するためのサポートが用意されていることです。 これは、id リソースを公開することで実現され `IdentityResources.OpenId` ます。

> [!NOTE]
> クラスは、 `IdentityResources` Openid connect 仕様で定義されているすべてのスコープ (openid、電子メール、プロファイル、電話、およびアドレス) をサポートしています。

また、カスタム id リソースの定義もサポートしています。 詳細については、「[カスタム id リソースの定義](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources)」を参照してください。 型の詳細については `IdentityResource` 、「Identity server 4」ドキュメントの「 [Identity Resource](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) 」を参照してください。

#### <a name="configuring-clients"></a>クライアントの構成

クライアントは、サーバーからトークンを要求できるアプリケーションです。 通常、次の設定は、各クライアントに対して最小値として定義する必要があります。

- 一意のクライアント ID。
- トークンサービスとの許可された対話 (許可の種類と呼ばれます)。
- Id トークンとアクセストークンが送信される場所 (リダイレクト URI と呼ばれます)。
- クライアントがアクセスを許可されているリソースの一覧 (スコープと呼ばれます)。

クライアントを構成する場合、メソッドには `AddInMemoryClients` コレクションが必要 `IEnumerable<Client>` です。 次のコード例は、 `GetClients` eShopOnContainers 参照アプリケーションでこのコレクションを提供するメソッドでの eShopOnContainers モバイルアプリの構成を示しています。

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

この構成では、次のプロパティのデータを指定します。

- `ClientId`: クライアントの一意の ID。
- `ClientName`: ログおよび同意画面に使用されるクライアントの表示名。
- `AllowedGrantTypes`: クライアントが、サーバーと対話する方法を指定します。 詳細について[は、「認証フローの構成](#configuring-the-authentication-flow)」を参照してください。
- `ClientSecrets`: トークンエンドポイントからトークンを要求するときに使用されるクライアントシークレットの資格情報を指定します。
- `RedirectUris`: トークンまたは認証コードを返すために使用できる Uri を指定します。
- `RequireConsent`: 同意画面が必要かどうかを指定します。
- `RequirePkce`: 認証コードを使用するクライアントが証明キーを送信する必要があるかどうかを指定します。
- `PostLogoutRedirectUris`: ログアウト後にリダイレクトすることが許可されている Uri を指定します。
- `AllowedCorsOrigins`: クライアントの配信元を指定します。これにより、配信元からのクロスオリジン呼び出しが許可されるようになります。
- `AllowedScopes`: クライアントがアクセスできるリソースを指定します。 既定では、クライアントはリソースにアクセスできません。
- `AllowOfflineAccess`: クライアントが更新トークンを要求できるかどうかを指定します。

#### <a name="configuring-the-authentication-flow"></a>認証フローの構成

クライアントとサーバー間の認証フローは、プロパティで許可の種類を指定することによって構成でき `Client.AllowedGrantTypes` ます。 OpenID Connect と OAuth 2.0 仕様では、次のような多数の認証フローが定義されています。

- [Implicit] このフローは、ブラウザーベースのアプリケーション用に最適化されているため、ユーザー認証のみ、または認証とアクセストークン要求のどちらかを使用する必要があります。 すべてのトークンがブラウザーを介して送信されるため、更新トークンなどの高度な機能は使用できません。
- 認証コード。 このフローでは、クライアント認証もサポートしながら、ブラウザーのフロントチャネルとは対照的に、バックチャネルでトークンを取得する機能が提供されます。
- [ハイブリッド] : このフローは、暗黙的および承認のコード許可の種類を組み合わせたものです。 Id トークンは、ブラウザーチャネルを介して送信されます。また、認証コードなどの他の成果物と共に、署名済みのプロトコル応答が含まれています。 応答が正常に検証されたら、バックチャネルを使用してアクセストークンと更新トークンを取得する必要があります。

> [!TIP]
> ハイブリッド認証フローを使用します。 ハイブリッド認証フローは、ブラウザーチャネルに適用される多数の攻撃を軽減します。また、アクセストークン (および場合によってはトークンを更新することもあります) を取得する必要があるネイティブアプリケーションに推奨されるフローです。

認証フローの詳細については、「[付与の種類](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html)」を参照してください。

### <a name="performing-authentication"></a>認証の実行

ユーザーの代わりにユーザーがトークンを発行するようにするには、ユーザーがユーザーをユーザーに登録する必要があります。 ただし、ユーザーインターフェイスは認証用のユーザーインターフェイスまたはデータベースを提供しません。 そのため、eShopOnContainers 参照アプリケーションでは、ASP.NET Core Id がこの目的に使用されます。

EShopOnContainers モバイルアプリは、図9-2 に示すハイブリッド認証フローを使用して、identity Server で認証を行います。

![](authentication-and-authorization-images/sign-in.png "High-level overview of the sign-in process")

**図 9-2:** サインインプロセスの大まかな概要

へのサインイン要求が行われ `<base endpoint>:5105/connect/authorize` ます。 成功した認証の後、ユーザーは認証コードと id トークンを含む認証応答を返します。 次に、認証コードがに送信され `<base endpoint>:5105/connect/token` 、アクセス、id、および更新トークンで応答します。

EShopOnContainers モバイルアプリは、追加のパラメーターを使用してに要求を送信することによって、サーバーからサインアウトし `<base endpoint>:5105/connect/endsession` ます。 サインアウトが発生すると、サーバーは、post ログアウトリダイレクト URI をモバイルアプリに送り返すことで応答します。 図9-3 はこのプロセスを示しています。

![](authentication-and-authorization-images/sign-out.png "High-level overview of the sign-out process")

**図 9-3:** サインアウトプロセスの大まかな概要

EShopOnContainers モバイルアプリでは、インターフェイスを実装するクラスによって、サービスとの通信が実行され `IdentityService` `IIdentityService` ます。 このインターフェイスは、実装するクラスが、、およびの各メソッドを提供する必要があることを指定し `CreateAuthorizationRequest` `CreateLogoutRequest` `GetTokenAsync` ます。

#### <a name="signing-in"></a>サインイン

ユーザーがの [**ログイン**] ボタンをタップすると、クラスのが `LoginView` `SignInCommand` `LoginViewModel` 実行され、さらにメソッドが実行され `SignInAsync` ます。 以下のコード例はこのメソッドを示しています。

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

このメソッドは、 `CreateAuthorizationRequest` `IdentityService` 次のコード例に示すように、クラスのメソッドを呼び出します。

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.Callback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

このメソッドは、必要なパラメーターを使用して、identity Server の[承認エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)の URI を作成します。 認証エンドポイントは、 `/connect/authorize` ユーザー設定として公開された基本エンドポイントのポート5105にあります。 ユーザー設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!NOTE]
> EShopOnContainers モバイルアプリの攻撃対象領域を減らすには、OAuth にコード交換 (PKCE) 拡張の証明キーを実装します。 PKCE では、承認コードが傍受された場合に使用されるのを防ぐことができます。 これは、クライアントが秘密検証ツールを生成し、そのハッシュが承認要求で渡され、認証コードの適用時にハッシュされて表示されることによって実現されます。 PKCE の詳細については、インターネット技術標準化委員会の web サイトの「 [OAuth パブリッククライアントによるコード交換のための証明キー](https://tools.ietf.org/html/rfc7636) 」を参照してください。

返された URI は、クラスのプロパティに格納され `LoginUrl` `LoginViewModel` ます。 プロパティがになると、内のが表示されるようになり `IsLogin` `true` [`WebView`](xref:Xamarin.Forms.WebView) `LoginView` ます。 データは、 `WebView` その [`Source`](xref:Xamarin.Forms.WebView.Source) プロパティを `LoginUrl` クラスのプロパティにバインド `LoginViewModel` します。これにより、 `LoginUrl` プロパティが "identity server" 認証エンドポイントに設定されている場合に、"サーバーにサインイン" 要求が行われます。 ユーザーがこの要求を受信し、ユーザーが認証されていない場合、は、 `WebView` 図9-4 に示すように、構成されたログインページにリダイレクトされます。

![](authentication-and-authorization-images/login.png "Login page displayed by the WebView")

**図 9-4:** WebView によって表示されるログインページ

ログインが完了すると、が [`WebView`](xref:Xamarin.Forms.WebView) 戻り URI にリダイレクトされます。 この `WebView` ナビゲーションにより、 `NavigateAsync` `LoginViewModel` 次のコード例に示すように、クラスのメソッドが実行されます。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

このメソッドは、戻り URI に含まれている認証応答を解析し、有効な認証コードが存在することを示します。これにより、認証コード、PKCE シークレット検証、およびその他の必要なパラメーターを渡して、サーバーの[トークンエンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)に要求を送信します。 トークンエンドポイントは、 `/connect/token` ユーザー設定として公開されているベースエンドポイントのポート5105にあります。 ユーザー設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!TIP]
> リターン Uri を検証します。 EShopOnContainers モバイルアプリでは、リターン URI は検証されませんが、オープンリダイレクト攻撃を防ぐために、戻り値 URI が既知の場所を参照していることを検証することをお勧めします。

トークンエンドポイントは、有効な認証コードと PKCE シークレット検証ツールを受け取ると、アクセストークン、id トークン、および更新トークンで応答します。 (API リソースへのアクセスを許可する) アクセストークンと id トークンは、アプリケーション設定として格納され、ページナビゲーションが実行されます。 したがって、eShopOnContainers モバイルアプリの全体的な影響は次のようになります。ユーザーがユーザーの認証に成功した場合は、ページに移動されます `MainView` 。これは、が選択した [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) `CatalogView` タブとして表示されるです。

ページナビゲーションの詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。 ナビゲーションによって [`WebView`](xref:Xamarin.Forms.WebView) ビューモデルメソッドが実行される方法については、「[動作を使用したナビゲーションの呼び出し](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking-navigation-using-behaviors)」を参照してください。 アプリケーション設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!NOTE]
> また、eShopOnContainers は、アプリがでモックサービスを使用するように構成されている場合にも、モックサインインを許可し `SettingsView` ます。 このモードでは、アプリは、ユーザーが任意の資格情報を使用してサインインできるようにするために、ユーザーがサービスと通信しません。

#### <a name="signing-out"></a>サインアウト

ユーザーがの [**ログアウト**] ボタンをタップすると、クラスのが `ProfileView` `LogoutCommand` `ProfileViewModel` 実行され、さらにメソッドが実行され `LogoutAsync` ます。 このメソッドは、ページへのページナビゲーションを実行し `LoginView` 、 `LogoutParameter` に設定されたインスタンスを `true` パラメーターとして渡します。 ページナビゲーション中にパラメーターを渡す方法の詳細については、「[ナビゲーション中のパラメーターの引き渡し](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing-parameters-during-navigation)」を参照してください。

ビューを作成して移動すると、 `InitializeAsync` ビューに関連付けられたビューモデルのメソッドが実行され、 `Logout` 次の `LoginViewModel` コード例に示すように、クラスのメソッドが実行されます。

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

このメソッドは、 `CreateLogoutRequest` クラスのメソッドを呼び出し `IdentityService` 、アプリケーション設定から取得された id トークンをパラメーターとして渡します。 アプリケーション設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。 次のコード例は、`CreateLogoutRequest` メソッドを示しています。

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

このメソッドは、必要なパラメーターを使用して、サービスの[エンドセッションエンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession)への URI を作成します。 エンドセッションエンドポイントは、 `/connect/endsession` ユーザー設定として公開されているベースエンドポイントのポート5105にあります。 ユーザー設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

返された URI は、クラスのプロパティに格納され `LoginUrl` `LoginViewModel` ます。 `IsLogin`プロパティがの場合、内のが `true` [`WebView`](xref:Xamarin.Forms.WebView) `LoginView` 表示されます。 データは、 `WebView` その [`Source`](xref:Xamarin.Forms.WebView.Source) プロパティを `LoginUrl` クラスのプロパティにバインド `LoginViewModel` します。これにより、 `LoginUrl` プロパティが [サービスの終了] セッションエンドポイントに設定されている場合に、このサーバーにサインアウト要求を送信します。 ユーザーがサインインしている場合、ユーザーがこの要求を受信すると、サインアウトが発生します。 認証は、ASP.NET Core からクッキー認証ミドルウェアによって管理される cookie を使用して追跡されます。 このため、サーバーからサインアウトすると、認証クッキーが削除され、ログアウト後のリダイレクト URI がクライアントに返されます。

モバイルアプリでは、は [`WebView`](xref:Xamarin.Forms.WebView) ログアウト後のリダイレクト URI にリダイレクトされます。 この `WebView` ナビゲーションにより、 `NavigateAsync` `LoginViewModel` 次のコード例に示すように、クラスのメソッドが実行されます。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

このメソッドは、アプリケーション設定から id トークンとアクセストークンの両方をクリアし、 `IsLogin` プロパティをに設定し `false` ます。これにより、ページ上のが非表示に [`WebView`](xref:Xamarin.Forms.WebView) `LoginView` なります。 最後に、 `LoginUrl` プロパティは、ユーザーが次回サインインを開始するときに備えて、必要なパラメーターを指定して、ユーザーの[認証エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)の URI に設定されます。

ページナビゲーションの詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。 ナビゲーションによって [`WebView`](xref:Xamarin.Forms.WebView) ビューモデルメソッドが実行される方法については、「[動作を使用したナビゲーションの呼び出し](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking-navigation-using-behaviors)」を参照してください。 アプリケーション設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!NOTE]
> また、eShopOnContainers は、SettingsView でモックサービスを使用するようにアプリが構成されている場合に、モックサインアウトを許可します。 このモードでは、アプリは、サービスと通信しません。その代わり、アプリケーション設定から格納されているトークンをクリアします。

## <a name="authorization"></a>承認

認証後、ASP.NET Core web Api は、多くの場合、アクセスを承認する必要があります。これにより、サービスは、認証された一部のユーザーだけが Api を使用できるようになります。

ASP.NET Core MVC ルートへのアクセスを制限するには、次のコード例に示すように、承認属性をコントローラーまたはアクションに適用します。これにより、コントローラーまたはアクションへのアクセスが認証されたユーザーに制限されます。

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

承認されていないユーザーが、属性でマークされたコントローラーまたはアクションにアクセスしようとすると、 `Authorize` MVC フレームワークは 401 (未承認) HTTP 状態コードを返します。

> [!NOTE]
> 属性でパラメーターを指定して、 `Authorize` API を特定のユーザーに制限することができます。 詳細については、「[承認](/aspnet/core/security/authorization/introduction/)」を参照してください。

認証ワークフローには、アクセストークンによって制御承認が提供されるように、サービスを統合することができます。 このアプローチを図9-5 に示します。

![](authentication-and-authorization-images/authorization.png "Authorization by access token")

**図 9-5:** アクセストークンによる承認

EShopOnContainers モバイルアプリは、id マイクロサービスと通信し、認証プロセスの一環としてアクセストークンを要求します。 アクセストークンは、アクセス要求の一部として、順序付けおよびバスケットマイクロサービスによって公開される Api に転送されます。 アクセストークンには、クライアントとユーザーに関する情報が含まれています。 Api は、その情報を使用して、データへのアクセスを承認します。 Api を保護するようにサービスを構成する方法の詳細については、「 [Api リソースの構成](#configuring-api-resources)」を参照してください。

### <a name="configuring-identityserver-to-perform-authorization"></a>認証を実行するようにサーバーを構成する

サーバーで承認を実行するには、その承認ミドルウェアを web アプリケーションの HTTP 要求パイプラインに追加する必要があります。 ミドルウェアは、 `ConfigureAuth` メソッドから呼び出される web アプリケーションのクラスのメソッドに追加され `Startup` `Configure` ます。これは、eShopOnContainers reference アプリケーションの次のコード例で示されています。

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

このメソッドは、有効なアクセストークンを使用してのみ API にアクセスできることを保証します。 ミドルウェアは、受信トークンを検証して信頼された発行者から送信されることを確認し、トークンが受信する API と共に使用できることを検証します。 したがって、注文またはバスケットコントローラーを参照すると、401 (未承認) の HTTP 状態コードが返され、アクセストークンが必要であることが示されます。

> [!NOTE]
> またはを使用して MVC を追加する前に、サービスの承認ミドルウェアを web アプリケーションの HTTP 要求パイプラインに追加する必要があり `app.UseMvc()` `app.UseMvcWithDefaultRoute()` ます。

### <a name="making-access-requests-to-apis"></a>Api へのアクセス要求の作成

注文とバスケットのマイクロサービスに要求を行うときは、認証プロセス中に、次のコード例に示すように、認証プロセス中に、サービスから取得したアクセストークンを要求に含める必要があります。

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

アクセストークンはアプリケーション設定として格納され、プラットフォーム固有のストレージから取得され、クラスのメソッドの呼び出しに含まれ `GetOrderAsync` `OrderService` ます。

同様に、次のコード例に示すように、サービスによって保護されている API にデータを送信するときに、アクセストークンを含める必要があります。

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

アクセストークンはプラットフォーム固有のストレージから取得され、クラスのメソッドの呼び出しに含まれ `UpdateBasketAsync` `BasketService` ます。

`RequestProvider`EShopOnContainers モバイルアプリのクラスは、クラスを使用し `HttpClient` て、eShopOnContainers 参照アプリケーションによって公開されている RESTful api に要求を行います。 承認が必要な注文とバスケットの Api に対して要求を行う場合は、要求に有効なアクセストークンを含める必要があります。 これを実現するには、 `HttpClient` 次のコード例に示すように、インスタンスのヘッダーにアクセストークンを追加します。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders`クラスのプロパティは、 `HttpClient` 各要求と共に送信されるヘッダーを公開し、アクセストークンは `Authorization` 文字列で始まるヘッダーに追加され `Bearer` ます。 要求が RESTful API に送信されると、ヘッダーの値が抽出され、信頼された `Authorization` 発行者から送信されることを確認するために検証され、ユーザーがそれを受け取る API を呼び出すためのアクセス許可を持っているかどうかを判断するために使用されます。

EShopOnContainers モバイルアプリが web 要求を作成する方法の詳細については、「[リモートデータへのアクセス](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)」を参照してください。

## <a name="summary"></a>まとめ

Xamarin.FormsASP.NET MVC web アプリケーションと通信するアプリに認証と承認を統合する方法は多数あります。 EShopOnContainers モバイルアプリは、ユーザー id を使用するコンテナー化された id マイクロサービスで認証と承認を実行します。 ユーザー Id は、ASP.NET Core Id と統合してベアラートークン認証を実行する ASP.NET Core 用のオープンソース OpenID Connect および OAuth 2.0 フレームワークです。

モバイルアプリは、ユーザーを認証したり、リソースにアクセスしたりするために、ユーザーのセキュリティトークンを要求します。 リソースにアクセスする場合は、承認を必要とする Api に対する要求にアクセストークンを含める必要があります。 ユーザーが信頼された発行者から送信されたことを確認するために、ユーザーが受信アクセストークンを検証し、それらのトークンを受信する API と共に使用することが有効であることを確認します。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
