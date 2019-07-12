---
title: 認証と承認
description: この章では、eShopOnContainers のモバイル アプリで認証と、コンテナー化されたマイクロ サービスに対して承認を実行する方法について説明します。
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: efaea24e559aa2f3bdfd87c1c083ce1d777dbb3f
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832162"
---
# <a name="authentication-and-authorization"></a>認証と承認

認証は、ユーザーから名とパスワードなどの資格情報を取得し、機関に対してこれらの資格情報を検証するプロセス。 資格情報が有効な場合、資格情報を送信するエンティティは、認証済みの id と見なされます。 Id が認証されると、承認プロセスは、その id が指定されたリソースへのアクセスを持つかどうかを決定します。

ASP.NET Core Identity では、Microsoft、Google などの外部認証プロバイダーの使用など、ASP.NET MVC web アプリケーションと通信する Xamarin.Forms アプリに認証と承認を統合する方法はたくさんあります。Facebook、Twitter、や認証のミドルウェア。 EShopOnContainers のモバイル アプリは、認証と承認 IdentityServer 4 を使用する識別情報のコンテナー化されたマイクロ サービスを実行します。 モバイル アプリでは、ユーザーを認証するため、またはリソースにアクセスするために、IdentityServer からセキュリティ トークンを要求します。 ユーザーの代理としてトークンを発行する IdentityServer は、のユーザーする必要がありますにサインイン IdentityServer します。 ただし、IdentityServer は提供しませんユーザー インターフェイスまたはデータベースを認証します。 そのため、eShopOnContainers 参照アプリケーションで ASP.NET Core Identity はこの目的に使用します。

## <a name="authentication"></a>認証

現在のユーザーの id を把握する必要があるときに、認証が必要です。 ユーザーを識別するための ASP.NET Core の主要なメカニズムは、開発者によって構成されているデータ ストアにユーザー情報を格納する ASP.NET Core Identity メンバーシップ システムです。 通常、このデータ ストアは、カスタム ストアまたはサード パーティ製パッケージを使用して、Azure storage、Azure Cosmos DB、またはその他の場所に id 情報を格納、EntityFramework ストアになります。

ローカル ユーザーのデータ ストアを使用する認証シナリオと、cookie などは ASP.NET MVC web アプリケーションで一般的な) を使用して要求間で id 情報を保持する、ASP.NET Core Identity は適切なソリューションです。 ただし、cookie は常にする通常の永続化してデータを送信する方法です。 たとえば、モバイル アプリからアクセスされる RESTful エンドポイントを公開する ASP.NET Core web アプリケーションはこのシナリオでは、cookie を使用できないためにベアラー トークンの認証を使用する通常必要があります。 ただし、ベアラー トークンに簡単に取得して、モバイル アプリからの web 要求の authorization ヘッダーに含まれます。

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>IdentityServer 4 を使用してベアラー トークンの発行

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4)オープン ソースの OpenID Connect と OAuth 2.0 のフレームワークが、ASP.NET Core は、ローカル ASP.NET Core Identity ユーザーのセキュリティ トークンの発行など多くの認証と承認のシナリオのために使用できます。

> [!NOTE]
> OpenID Connect と OAuth 2.0 は、さまざまな役割をことができますが、非常に似ています。

OpenID Connect、OAuth 2.0 プロトコル上に、認証レイヤーです。 OAuth 2 は、セキュリティ トークン サービスからアクセス トークンを要求し、Api と通信するために使用してアプリケーションを使用するプロトコルです。 この委任では、認証と承認を集中化できるため、クライアント アプリケーションと Api の両方で複雑さが削減されます。

OpenID Connect と OAuth 2.0 の組み合わせは、認証と API のアクセスの 2 つの基本的なセキュリティ上の懸念を結合し、IdentityServer 4 はこれらのプロトコルの実装です。

EShopOnContainers 参照アプリケーションなど、マイクロ サービスへのクライアントへの直接的な通信を使用するアプリケーションでセキュリティ トークン サービス (STS) として機能する専用認証マイクロ サービスができます、ユーザーを認証する図のように9-1。 マイクロ サービスへのクライアントへの直接的な通信の詳細については、次を参照してください。[クライアント間の通信とマイクロ サービス](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices)します。

![](authentication-and-authorization-images/authentication.png "専用認証マイクロ サービスによる認証")

**図 9-1:** 専用認証マイクロ サービスによる認証

EShopOnContainers のモバイル アプリは、id のマイクロ サービスは、IdentityServer は 4 を使用して認証を実行し、Api のアクセス制御と通信します。 そのため、モバイル アプリは、ユーザーを認証するため、またはリソースにアクセスするため、IdentityServer からトークンを要求します。

-   モバイル アプリの要求することによって実現されます IdentityServer とユーザーの認証、 *identity*トークンは、認証プロセスの結果を表します。 最低限、ユーザー、およびユーザーが認証された方法とタイミングに関する情報の識別子が含まれます。 また、追加の id データを含めることもできます。
-   モバイル アプリの要求することによって実現されます IdentityServer とのリソースへのアクセス、*アクセス*トークンは、API リソースへのアクセスを許可します。 クライアントは、アクセス トークンを要求し、それらを API に転送します。 アクセス トークンは、(存在する) 場合に、クライアントと、ユーザーに関する情報を格納します。 Api は、データへのアクセスを承認するためにその情報を使用します。

> [!NOTE]
> トークンを要求することには、IdentityServer とクライアントを登録する必要があります。

### <a name="adding-identityserver-to-a-web-application"></a>IdentityServer を Web アプリケーションに追加します。

ASP.NET Core web アプリケーション IdentityServer 4 を使用するためには、web アプリケーションの Visual Studio ソリューションに追加する必要があります。 詳細については、次を参照してください。[概要](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html)IdentityServer ドキュメント。

IdentityServer は、web アプリケーションの Visual Studio ソリューションに含まれると OpenID Connect と OAuth 2.0 エンドポイントへの要求を使用できるように処理パイプラインでは、web アプリケーションの HTTP 要求に追加する必要があります。 これは、`Configure`メソッドで、web アプリケーションの`Startup`クラスの次のコード例。

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Web アプリケーションの HTTP 要求処理パイプラインで順序が重要です。 そのため、IdentityServer は、ログイン画面を実装する UI フレームワークの前に、のパイプラインに追加する必要があります。

### <a name="configuring-identityserver"></a>IdentityServer の構成

IdentityServer で構成する必要があります、 `ConfigureServices` web アプリのメソッドに`Startup`クラスを呼び出すことによって、`services.AddIdentityServer`メソッド、eShopOnContainers 参照アプリケーションから次のコード例で示した。

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

呼び出した後、`services.AddIdentityServer`メソッド、fluent Api の追加は、以下の構成と呼ばれます。

-   資格情報の署名に使用します。
-   ユーザーが要求する API と id のリソースへのアクセスします。
-   要求トークンに接続するクライアント。
-   ASP.NET Core Identity です。

> [!TIP]
> IdentityServer 4 構成を動的に読み込みます。 IdentityServer 4 の Api では、構成オブジェクトのメモリ内のリストから IdentityServer を構成できます。 EShopOnContainers 参照アプリケーションで、これらのメモリ内コレクションは、アプリケーションにハードコーディングします。 ただし、運用環境シナリオでできる読み込まれて動的に構成ファイルから、またはデータベースからです。

ASP.NET Core Identity を使用する IdentityServer の構成方法の詳細については、次を参照してください。[を使用して ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/latest/quickstarts/8_aspnet_identity.html) IdentityServer ドキュメント。

#### <a name="configuring-api-resources"></a>API のリソースの構成

API のリソースを構成するときに、`AddInMemoryApiResources`メソッドが必要ですが、`IEnumerable<ApiResource>`コレクション。 次のコード例は、 `GetApis` eshoponcontainers には、このコレクションを提供するメソッドは、アプリケーションを参照します。

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

このメソッドは、IdentityServer は、注文とバスケット Api 保護する必要がありますを指定します。 IdentityServer がアクセスを管理するため、これらの Api を呼び出す際、トークンが必要になります。 詳細については、`ApiResource`入力を参照してください[API リソース](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html)IdentityServer 4 のドキュメント。

#### <a name="configuring-identity-resources"></a>Id リソースの構成

Id のリソースを構成するときに、`AddInMemoryIdentityResources`メソッドが必要ですが、`IEnumerable<IdentityResource>`コレクション。 Id リソースは、ユーザー ID、名、または電子メール アドレスなどのデータです。 各 id リソースは一意の名前を備え、任意のクレームの種類に割り当て可能、これで、ユーザーの id トークンが含まれます。 次のコード例は、 `GetResources` eshoponcontainers には、このコレクションを提供するメソッドは、アプリケーションを参照します。

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

OpenID Connect の仕様では、いくつかを指定します[標準 id リソース](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)します。 最小要件は、ユーザーの一意の ID を出力するためのサポートが提供されることです。 公開することでこれは、 `IdentityResources.OpenId` id リソース。

> [!NOTE]
> `IdentityResources`クラスは、OpenID Connect の仕様 (openid、電子メール、プロファイル、電話番号、およびアドレス) で定義されているスコープのすべてをサポートします。

IdentityServer は、カスタム id リソースの定義もサポートします。 詳細については、次を参照してください。[カスタム id のリソースを定義する](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources)IdentityServer ドキュメント。 詳細については、`IdentityResource`入力を参照してください[Id リソース](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html)IdentityServer 4 のドキュメント。

#### <a name="configuring-clients"></a>クライアントの構成

クライアントは、IdentityServer からトークンを要求できるアプリケーションです。 通常、各クライアントの場合、最低でも、次の設定を定義する必要があります。

-   一意のクライアント id。
-   トークン サービス (、付与の種類と呼ばれます) との相互作用の許可されています。
-   Id とアクセス トークンの送信先となる場所 (リダイレクト URI と呼ばれます)。
-   クライアントへのアクセスを許可されているリソースの一覧 (スコープとも呼ばれます)。

クライアントを構成するときに、`AddInMemoryClients`メソッドが必要ですが、`IEnumerable<Client>`コレクション。 次のコード例は、eShopOnContainers でモバイル アプリの構成を示しています。、 `GetClients` 、eshoponcontainers には、このコレクションを提供するメソッドは、アプリケーションを参照します。

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

この構成は、次のプロパティのデータを指定します。

-   `ClientId`:クライアントの一意の ID。
-   `ClientName`:クライアントは、ログ記録と同意画面で使用される名前を表示します。
-   `AllowedGrantTypes`:クライアントが IdentityServer と対話しようとした方法を指定します。 詳細については、次を参照してください。[認証フローを構成する](#configuring_the_authentication_flow)します。
-   `ClientSecrets`:トークン エンドポイントからトークンを要求するときに使用されるクライアント シークレットの資格情報を指定します。
-   `RedirectUris`:承認コードまたはトークンを返しますを許可されている Uri を指定します。
-   `RequireConsent`:同意画面が必要かどうかを指定します。
-   `RequirePkce`:認証コードを使用してクライアントが証明キーを送信する必要があるかどうかを指定します。
-   `PostLogoutRedirectUris`:ログアウト後にリダイレクトする許可された Uri を指定します。
-   `AllowedCorsOrigins`:IdentityServer が原点からのクロス オリジン呼び出しを許可できるように、クライアントの原点を指定します。
-   `AllowedScopes`:クライアントへのアクセスを持つリソースを指定します。 既定では、クライアントにはすべてのリソースへのアクセスがありません。
-   `AllowOfflineAccess`:クライアントが更新トークンを要求できるかどうかを指定します。

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>認証フローの構成

認証フローで付与タイプを指定することで、クライアントと IdentityServer との間を構成する、`Client.AllowedGrantTypes`プロパティ。 OpenID Connect と OAuth 2.0 仕様では、数などの認証フローを定義します。

-   暗黙の型。 このフローでは、ブラウザー ベースのアプリケーションは最適化され、ユーザー認証専用、または認証とアクセス トークン要求に使用する必要があります。 すべてのトークンは、ブラウザー経由で送信され、したがって更新トークンは許可されていませんなどの機能を強化します。
-   承認コード。 このフローでは、クライアント認証もサポートしているときに、ブラウザーの前面チャネルではなくのバック チャネルでトークンを取得する機能を提供します。
-   ハイブリッド。 このフローは、暗黙の組み合わせと、承認コード付与の種類。 Id トークンはブラウザー チャネル経由で送信され、認証コードなどの他のアーティファクトと共に署名プロトコルの応答が含まれています。 応答の検証が成功した後は、アクセス権を取得し、トークンを更新するバック チャネルを使用してください。

> [!TIP]
> ハイブリッドの認証フローを使用します。 ハイブリッドの認証フローは、多くのブラウザーのチャネルに適用される攻撃を軽減する、ネイティブ アプリケーションのアクセス トークンの取得 (および更新トークンの可能性があります) にすることをお勧めのフローです。

認証フローの詳細については、次を参照してください。[付与タイプ](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html)IdentityServer 4 のドキュメント。

### <a name="performing-authentication"></a>認証を実行します。

ユーザーの代理としてトークンを発行する IdentityServer は、のユーザーする必要がありますにサインイン IdentityServer します。 ただし、IdentityServer は提供しませんユーザー インターフェイスまたはデータベースを認証します。 そのため、eShopOnContainers 参照アプリケーションで ASP.NET Core Identity はこの目的に使用します。

EShopOnContainers のモバイル アプリは、IdentityServer とハイブリッドの認証フローを図 9-2 に示すように認証されます。

![](authentication-and-authorization-images/sign-in.png "サインイン プロセスの概要")

**図 9-2:** サインイン プロセスの概要

サインイン要求が行われた`<base endpoint>:5105/connect/authorize`します。 次の認証が成功は、IdentityServer は、認証コードと id トークンを含む認証の応答を返します。 承認コードに送信し、`<base endpoint>:5105/connect/token`アクセス、id、および更新トークンに応答します。

EShopOnContainers モバイル アプリ サイン アウト IdentityServer への要求を送信することによって`<base endpoint>:5105/connect/endsession`、追加のパラメーターを使用します。 サインアウトが発生した後、IdentityServer は、モバイル アプリに post ログアウト リダイレクト URI を送信することによって応答します。 図 9-3 は、このプロセスを示しています。

![](authentication-and-authorization-images/sign-out.png "サインアウト プロセスの概要")

**図 9-3:** サインアウト プロセスの概要

IdentityServer との通信を行った eShopOnContainers のモバイル アプリで、`IdentityService`クラスを実装、`IIdentityService`インターフェイス。 このインターフェイスを実装するクラスを提供する必要がありますを指定します`CreateAuthorizationRequest`、 `CreateLogoutRequest`、および`GetTokenAsync`メソッド。

#### <a name="signing-in"></a>サインイン

ユーザーがタップしたときに、**ログイン**のボタンでは、 `LoginView`、`SignInCommand`で、`LoginViewModel`クラスを実行すると、順番に実行する、`SignInAsync`メソッド。 以下のコード例はこのメソッドを示しています。

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

このメソッドは、`CreateAuthorizationRequest`メソッドで、`IdentityService`クラスは、次のコード例に示されています。

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
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
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

このメソッドは、IdentityServer の URI を作成します。[承認エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)、必要なパラメーターを使用します。 承認エンドポイントが、 `/connect/authorize` 5105 ユーザー設定として公開される基本のエンドポイントのポートします。 ユーザー設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)します。

> [!NOTE]
> EShopOnContainers のモバイル アプリの攻撃対象領域を削減するには、OAuth コード Exchange (PKCE) 拡張機能の証明キーを実装します。 PKCE は、それが傍受された場合に使用されているから認証コードを保護します。 これは、うちハッシュが承認要求で渡されると、シークレットの検証コードを生成するクライアントによって、表示されているハッシュされていない認証コードを使用するときにです。 PKCE の詳細については、次を参照してください。 [OAuth パブリック クライアントでのコードの Exchange 用の Proof Key](https://tools.ietf.org/html/rfc7636) Internet Engineering Task Force の web サイト。

返された URI が格納されている、`LoginUrl`のプロパティ、`LoginViewModel`クラス。 ときに、`IsLogin`プロパティになります`true`、 [ `WebView` ](xref:Xamarin.Forms.WebView)で、`LoginView`が表示されます。 `WebView`データ バインド、 [ `Source` ](xref:Xamarin.Forms.WebView.Source)プロパティを`LoginUrl`のプロパティ、`LoginViewModel`クラスし、そのため IdentityServer へのサインイン要求時に、`LoginUrl`プロパティに設定IdentityServer の承認エンドポイント。 IdentityServer は、この要求を受信し、ユーザーが認証されていないときに、`WebView`は図 9-4 に示すように構成されているログイン ページにリダイレクトされます。

![](authentication-and-authorization-images/login.png "ログイン ページが WebView で表示されます。")

**図 9-4:** ログイン ページが WebView で表示されます。

ログインが完了すると、 [ `WebView` ](xref:Xamarin.Forms.WebView)戻り値の URI にリダイレクトされます。 これは、`WebView`ナビゲーションが発生、`NavigateAsync`メソッドで、`LoginViewModel`次のコード例に示されているクラスを実行します。

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

このメソッドは、戻り値の URI に含まれている認証の応答を解析し、IdentityServer には有効な承認コードが存在する、要求[トークン エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)、認証コードを渡すこと、PKCE シークレットの検証方法、およびその他の必須パラメーターです。 トークン エンドポイントが、 `/connect/token` 5105 ユーザー設定として公開される基本のエンドポイントのポートします。 ユーザー設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)します。

> [!TIP]
> 戻り値の Uri を検証します。 ただし、eShopOnContainers のモバイル アプリでは、戻り値の URI を検証しませんは、戻り値の URI がオープン リダイレクト攻撃を防ぐための既知の場所を指すことを検証することをお勧めします。

トークン エンドポイントでは、有効な承認コードと PKCE シークレットの検証を受信する場合は、アクセス トークン、id トークン、更新トークンと応答します。 (これは、API リソースへのアクセスを許可するには) アクセス トークンと id トークンは、アプリケーション設定として格納し、ページ ナビゲーションを実行します。 そのため、これは eShopOnContainers のモバイル アプリでの全体的な結果: に移動されるときのユーザーが、IdentityServer で正常に認証することに、 `MainView`  ページで、これは、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)表示する、`CatalogView`として選択されているそのタブ。

ページ ナビゲーションの詳細については、次を参照してください。[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)します。 方法については[ `WebView` ](xref:Xamarin.Forms.WebView)ナビゲーションとビュー モデルのメソッドを実行するを参照してください[ビヘイビアーを使用して呼び出すナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)します。 アプリケーションの設定については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)します。

> [!NOTE]
> EShopOnContainers できますモックのサインインでモック サービスを使用するアプリが構成されている場合、`SettingsView`します。 このモードで、アプリは、IdentityServer は、代わりに、資格情報を使用してサインインするユーザーを許可すると通信しません。

#### <a name="signing-out"></a>署名アウト

ユーザーがタップしたときに、 **LOG OUT**ボタン、 `ProfileView`、`LogoutCommand`で、`ProfileViewModel`クラスを実行すると、順番に実行する、`LogoutAsync`メソッド。 このメソッドにページ ナビゲーションの実行、`LoginView`渡してページ、`LogoutParameter`インスタンスに設定`true`をパラメーターとして。 ページ ナビゲーション中に渡すパラメーターの詳細については、次を参照してください。[ナビゲーション中にパラメーターを渡す](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)します。

ビューが作成されに移動したときに、`InitializeAsync`ビューの関連するビュー モデルのメソッドを実行すると、実行し、`Logout`のメソッド、`LoginViewModel`クラスは、次のコード例に示されています。

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

このメソッドは、`CreateLogoutRequest`メソッドで、`IdentityService`をパラメーターとして、クラス、id トークンを渡すことがアプリケーションの設定から取得します。 アプリケーション設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)します。 次のコード例は、`CreateLogoutRequest` メソッドを示しています。

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

このメソッドは、IdentityServer の URI を作成します。[エンドポイントのセッションを終了](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession)、必要なパラメーターを使用します。 エンドポイントのセッションが、 `/connect/endsession` 5105 ユーザー設定として公開される基本のエンドポイントのポートします。 ユーザー設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)します。

返された URI が格納されている、`LoginUrl`のプロパティ、`LoginViewModel`クラス。 中に、`IsLogin`プロパティは`true`、 [ `WebView` ](xref:Xamarin.Forms.WebView)で、`LoginView`を表示します。 `WebView`データ バインド、 [ `Source` ](xref:Xamarin.Forms.WebView.Source)プロパティを`LoginUrl`のプロパティ、`LoginViewModel`クラス、およびサインアウト要求 IdentityServer になるときに、`LoginUrl`プロパティに設定IdentityServer の最後のセッションのエンドポイント。 IdentityServer は、ユーザーがサインインがこの要求を受け取り、サインアウトの処理が発生します。 認証は、ASP.NET Core から cookie 認証ミドルウェアによって管理される cookie で追跡されます。 そのため、IdentityServer からサインアウトは、認証 cookie を削除し、ログアウト後のリダイレクト URI がクライアントに送り返しますを送信します。

モバイル アプリで、 [ `WebView` ](xref:Xamarin.Forms.WebView)ログアウトの後のリダイレクト URI にリダイレクトされます。 これは、`WebView`ナビゲーションが発生、`NavigateAsync`メソッドで、`LoginViewModel`次のコード例に示されているクラスを実行します。

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

このメソッドは、id トークンとアプリケーションの設定 からアクセス トークンの両方をクリアし、設定、`IsLogin`プロパティを`false`、原因となる、 [ `WebView` ](xref:Xamarin.Forms.WebView)上、`LoginView`ページを非表示になります. 最後に、`LoginUrl`プロパティが、URI の IdentityServer の[承認エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)、必須のパラメーターを使用して、次回に備えて、ユーザーが開始には、サインインします。

ページ ナビゲーションの詳細については、次を参照してください。[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)します。 方法については[ `WebView` ](xref:Xamarin.Forms.WebView)ナビゲーションとビュー モデルのメソッドを実行するを参照してください[ビヘイビアーを使用して呼び出すナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)します。 アプリケーションの設定については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)します。

> [!NOTE]
> EShopOnContainers で、SettingsView モック サービスを使用するアプリが構成されている場合、モックをサインアウトしてできます。 このモードでは、アプリは、IdentityServer は通信しませんし、アプリケーションの設定からのすべてのストアド トークンがクリアされます。

<a name="authorization" />

## <a name="authorization"></a>承認

認証、ASP.NET Core の web Api が多くの場合、アクセスを承認する必要がありますこれにより、サービスを Api ように、一部の認証済みユーザーを使用できますが、すべて。

コント ローラーに Authorize 属性を適用することで、ASP.NET Core MVC ルートへのアクセス制限を実現できます。 またはアクション、コント ローラーへのアクセスを制限またはアクションを次のコード例に示すように、ユーザーを認証します。

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

コント ローラーまたはアクションでマークされているアクセスしようとしているユーザーが承認されていない場合、`Authorize`属性、MVC フレームワークは、401 (未承認) HTTP 状態コードを返します。

> [!NOTE]
> パラメーターを指定することができます、 `Authorize` API を特定のユーザーに制限する属性。 詳細については、次を参照してください。[承認](/aspnet/core/security/authorization/introduction/)します。

IdentityServer は、アクセス トークンの承認を制御を提供するように、承認ワークフローに統合できます。 このアプローチは、図 9-5 に表示されます。

![](authentication-and-authorization-images/authorization.png "アクセス トークンによる認証")

**図 9-5:** アクセス トークンによる認証

EShopOnContainers のモバイル アプリでは、id マイクロ サービスと通信し、認証プロセスの一環としてアクセス トークンを要求します。 アクセス トークンは、アクセス要求の一部として、順序付けと basket マイクロ サービスによって公開される Api に転送されます。 アクセス トークンは、クライアントと、ユーザーに関する情報を格納します。 Api は、データへのアクセスを承認するためにその情報を使用します。 IdentityServer は、Api の保護を構成する方法については、次を参照してください。 [API リソースの構成](#configuring-api-resources)します。

### <a name="configuring-identityserver-to-perform-authorization"></a>IdentityServer は承認を実行するを構成します。

IdentityServer による承認を実行するには、web アプリケーションの HTTP 要求パイプラインにその承認ミドルウェアを追加する必要があります。 ミドルウェアが追加された、`ConfigureAuth`メソッドで、web アプリケーションの`Startup`クラスから呼び出される、`Configure`メソッド、eShopOnContainers 参照アプリケーションから次のコード例に示すよう。

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

このメソッドにより、API は有効なアクセス トークンを使用してのみアクセスできます。 ミドルウェアは、信頼された発行者から送信されることを確認する受信トークンを検証し、トークンが受信する API で使用するは無効であることを検証します。 そのため、順序付けまたはバスケット コント ローラーへの参照には、401 (未承認) HTTP ステータス コードをアクセス トークンが必要であることを示すは返します。

> [!NOTE]
> IdentityServer の認証ミドルウェアは、MVC とを追加する前に、web アプリケーションの HTTP 要求パイプラインに追加する必要があります`app.UseMvc()`または`app.UseMvcWithDefaultRoute()`します。

### <a name="making-access-requests-to-apis"></a>Api へのアクセス要求の作成

次のコード例に示すように、要求するときに、順序付けと basket マイクロ サービス、アクセス トークンで、認証プロセス中、IdentityServer から取得したが、要求に含める必要があります。

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

アクセス トークンをプラットフォーム固有のストレージから取得されへの呼び出しに含まれてし、アプリケーション設定として格納されます、`GetOrderAsync`メソッドで、`OrderService`クラス。

同様に、アクセス トークンにする必要が含まれる API、IdentityServer は、データを送信する保護されている場合、次のコード例に示すように。

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

アクセス トークンがプラットフォーム固有の記憶域から取得されへの呼び出しに含まれる、`UpdateBasketAsync`メソッドで、`BasketService`クラス。

`RequestProvider`クラス、eShopOnContainers のモバイル アプリでは使用して、 `HttpClient` eShopOnContainers 参照アプリケーションによって公開される RESTful Api に要求を行うクラス。 順序付けとバスケット、承認を必要とする Api に要求を行う場合、有効なアクセス トークンが要求に含まれる場合があります。 ヘッダーにアクセス トークンを追加することでこれは、`HttpClient`の次のコード例に示すように、インスタンスします。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders`のプロパティ、`HttpClient`クラスは、各要求と一緒に送信されるヘッダーを公開し、アクセス トークンに追加されます、`Authorization`ヘッダー文字列で始まる`Bearer`します。 値、RESTful API に要求が送信されると、`Authorization`ヘッダーが抽出され、信頼された発行者から送信されたことと、ユーザーが API を呼び出す権限を持っているかどうかを判断するために使用を受け取ることを確認するために検証します。

EShopOnContainers のモバイル アプリが web 要求を使用する方法の詳細については、次を参照してください。[にアクセスするリモート データ](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)します。

## <a name="summary"></a>まとめ

ASP.NET MVC web アプリケーションと通信する Xamarin.Forms アプリへの認証と承認の統合に多くの方法はあります。 EShopOnContainers のモバイル アプリは、認証と承認 IdentityServer 4 を使用する識別情報のコンテナー化されたマイクロ サービスを実行します。 IdentityServer は、オープン ソースの OpenID Connect と OAuth 2.0 ベアラー トークン認証を実行する ASP.NET Core Identity と統合する ASP.NET Core のフレームワークです。

モバイル アプリでは、ユーザーを認証するため、またはリソースにアクセスするために、IdentityServer からセキュリティ トークンを要求します。 リソースにアクセスするときは、要求の承認を必要とする Api にアクセス トークンを含める必要があります。 IdentityServer のミドルウェアは、信頼された発行者から送信されることになり、それらが受け取る API で使用する有効なことを確認する受信のアクセス トークンを検証します。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
