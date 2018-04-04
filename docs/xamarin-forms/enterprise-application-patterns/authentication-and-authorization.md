---
title: 認証と承認
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 9c6f3ae19b3e1b89220cbdf0985f4bdf789f2209
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="authentication-and-authorization"></a>認証と承認

認証とは、ユーザー名やパスワードなどの資格情報を取得し、機関に対してこれらの資格情報を検証するプロセスです。 資格情報が有効な場合は、資格情報を送信するエンティティは、認証済み id と見なされます。 Id が認証されると、承認プロセスは、その id が指定されたリソースへのアクセスを持つかどうかを判断します。

ASP.NET Core Id、Google、Microsoft などの外部認証プロバイダーの使用など、ASP.NET MVC web アプリケーションと通信する Xamarin.Forms アプリへの認証と承認の統合に多くの方法はあります。Facebook、Twitter、や認証のミドルウェア。 EShopOnContainers モバイル アプリでは、認証とによる IdentityServer 4 を使用するコンテナー化 identity マイクロ サービスを実行します。 モバイル アプリでは、ユーザーを認証するため、またはリソースにアクセスするために、IdentityServer からセキュリティ トークンを要求します。 ユーザーの代理としてトークンを発行する IdentityServer のユーザー必要がありますにサインイン IdentityServer です。 ただし、IdentityServer しないユーザー インターフェイスまたはデータベースの情報認証します。 したがって、eShopOnContainers 参照アプリケーションの ASP.NET Core Id はこの目的に使用します。

## <a name="authentication"></a>認証

アプリケーションは、現在のユーザーの id を知っておく必要がある場合、認証が必要です。 ユーザーを識別するための ASP.NET Core の主要なメカニズムは、開発者によって構成されているデータ ストアにユーザー情報を格納する ASP.NET Core Id メンバーシップ システムです。 通常、このデータ ストアに EntityFramework ストアでは、Azure ストレージ、Azure Cosmos DB、またはその他の場所に id 情報を格納するカスタム ストアまたはサードパーティ製パッケージを利用できますが、します。

ローカル ユーザーのデータ ストアを使用する認証シナリオとの間 (で ASP.NET MVC web アプリケーションで一般的な) の cookie を使用して要求の id 情報を保持する、ASP.NET Core Id は適切なソリューションです。 ただし、cookie は常に永続化して、データの送信の通常の方法 たとえば、モバイル アプリからアクセスされる rest ベースのエンドポイントを公開する ASP.NET Core web アプリケーションはこのシナリオでは cookie を使用することはできませんのでベアラー トークンの認証を使用する通常必要があります。 ただし、ベアラー トークン簡単に取得して、モバイル アプリからの web 要求の承認ヘッダーに含まれます。

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>ベアラー トークンの発行 IdentityServer 4 を使用します。

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4)はオープン ソースの OpenID Connect および OAuth 2.0 framework、ASP.NET core は、ローカルの ASP.NET Core Id ユーザーに対してセキュリティ トークンの発行など多くの認証と承認のシナリオに使用できます。

> [!NOTE]
> OpenID Connect および OAuth 2.0 は、さまざまな役割を持ちながら非常に似ています。

OpenID Connect は、OAuth 2.0 プロトコルの上に、認証レイヤーです。 OAuth 2 は、セキュリティ トークン サービスからアクセス トークンを要求し、Api と通信するために使用してアプリケーションを使用するプロトコルです。 この委任は、認証と承認を一元化できますので、クライアント アプリケーションと Api の両方の複雑さを軽減します。

OpenID Connect と OAuth 2.0 の組み合わせは、認証と API アクセスの 2 つの基本的なセキュリティ上の問題を組み合わせるし、IdentityServer 4 は、これらのプロトコルの実装です。

EShopOnContainers 参照アプリケーションなどの直接のクライアント-マイクロ-サービスの通信を使用するアプリケーションでセキュリティ トークン サービス (STS) として機能する専用の認証マイクロ サービス使えますユーザーを認証する図に示すように9-1。 マイクロ サービスでのクライアントへの直接的な通信の詳細については、次を参照してください。[クライアント間の通信と Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices)です。

![](authentication-and-authorization-images/authentication.png "専用認証マイクロ サービスによる認証")

**図 9-1:**専用認証マイクロ サービスによる認証

EShopOnContainers モバイル アプリは、identity のマイクロ サービスは、IdentityServer 4 を使用して認証を実行し、Api のアクセス制御と通信します。 そのため、モバイル アプリは、ユーザーを認証するため、またはリソースにアクセスするため、IdentityServer からトークンを要求します。

-   IdentityServer を持つユーザーを認証には、要求しているモバイル アプリ、 *identity*トークンで、認証プロセスの結果を表します。 最低限、ユーザー、およびユーザーが認証された方法とタイミングに関する情報の識別子が含まれます。 また、追加の id データを含めることもできます。
-   IdentityServer でリソースにアクセスするには、要求しているモバイル アプリ、*アクセス*トークンで、API のリソースにアクセスできるようにします。 クライアントは、アクセス トークンを要求し、それらを API に転送します。 アクセス トークンは、(存在する場合) に、クライアントと、ユーザーに関する情報を格納します。 Api では、データへのアクセスを承認するためにその情報を使用します。

> [!NOTE]
> トークンを要求する前に、IdentityServer にクライアントを登録する必要があります。

### <a name="adding-identityserver-to-a-web-application"></a>Web アプリケーションに IdentityServer を追加します。

ASP.NET Core web アプリケーション IdentityServer 4 を使用するためには、web アプリケーションの Visual Studio ソリューションに追加する必要があります。 詳細については、次を参照してください。[セットアップと概要](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html)、IdentityServer マニュアルを参照します。

IdentityServer が web アプリケーションの Visual Studio ソリューションに含まれていると、OpenID Connect および OAuth 2.0 エンドポイントへの要求を使用できるようにそのは web アプリケーションの HTTP 要求処理パイプラインに追加する必要があります。 これを実現する、`Configure`メソッドで web アプリケーションの`Startup`クラスに、次のコード例に示されています。

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Web アプリケーションの HTTP 要求処理パイプラインで順序が重要です。 したがって、IdentityServer は、ログイン画面を実装する UI フレームワークの前にパイプラインに追加する必要があります。

### <a name="configuring-identityserver"></a>IdentityServer を構成します。

IdentityServer を構成する必要があります、`ConfigureServices`メソッドで web アプリケーションの`Startup`クラスを呼び出して、 `services.AddIdentityServer` eShopOnContainers の参照をアプリケーションから次のコード例で示したように、メソッド。

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

呼び出した後、`services.AddIdentityServer`メソッドを追加 fluent Api が呼び出された、以下を構成します。

-   資格情報の署名に使用します。
-   ユーザーの要求が API と id のリソースへのアクセスします。
-   要求トークンを接続するクライアント。
-   ASP.NET Core の Id。

>💡 **ヒント:**: IdentityServer 4 の構成を動的に読み込みます。 構成オブジェクトのメモリ内の一覧から IdentityServer を構成するため IdentityServer 4 の Api を使用します。 アプリケーションでは、eShopOnContainers 参照、メモリ内のこれらのコレクションが、アプリケーションにハードコーディングします。 ただし、実稼働のシナリオの可能性があるロード動的に構成ファイルまたはデータベースからです。

ASP.NET Core の Id を使用する IdentityServer の構成については、次を参照してください。[を使用して ASP.NET Core Id](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) 、IdentityServer マニュアルを参照します。

#### <a name="configuring-api-resources"></a>API のリソースの構成

API のリソースを構成するときに、`AddInMemoryApiResources`メソッドが必要ですが、`IEnumerable<ApiResource>`コレクション。 次のコード例は、 `GetApis` eShopOnContainers 内のこのコレクションを提供するメソッドは、アプリケーションを参照します。

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

このメソッドは、注文とバスケット Api IdentityServer を保護する必要があることを指定します。 IdentityServer がアクセスを管理するため、これらの Api への呼び出しを行うときにトークンが必要になります。 詳細については、`ApiResource`を入力しを参照してください[API リソース](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource)IdentityServer 4 マニュアルを参照します。

#### <a name="configuring-identity-resources"></a>Identity リソースを構成します。

Identity リソースを構成するときに、`AddInMemoryIdentityResources`メソッドが必要ですが、`IEnumerable<IdentityResource>`コレクション。 Identity リソースは、ユーザー ID、名、または電子メール アドレスなどのデータです。 各 id リソースは、一意の名前を持ち、任意のクレームの種類に代入できますが、ユーザーの id トークンに含まれます。 次のコード例は、 `GetResources` eShopOnContainers 内のこのコレクションを提供するメソッドは、アプリケーションを参照します。

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

OpenID Connect の仕様では、いくつかを指定します[標準 identity リソース](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)です。 最小要件は、ユーザーの一意の ID を出力するためのサポートが指定されています。 これは、公開することで実現、 `IdentityResources.OpenId` id リソース。

> [!NOTE]
> `IdentityResources`すべて (openid、電子メール、プロファイル、電話、およびアドレス)、OpenID Connect 仕様で定義されたスコープのクラスをサポートしています。

IdentityServer は、カスタム id のリソースの定義もサポートします。 詳細については、次を参照してください。[カスタム id のリソースを定義する](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources)、IdentityServer マニュアルを参照します。 詳細については、`IdentityResource`を入力しを参照してください[Id リソース](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html)IdentityServer 4 マニュアルを参照します。

#### <a name="configuring-clients"></a>クライアントを構成します。

クライアントは、IdentityServer からトークンを要求できるアプリケーションです。 通常、各クライアントの場合、最低でも、次の設定を定義する必要があります。

-   一意のクライアント id。
-   トークン サービス (付与タイプと呼ばれます) との相互作用の許可されています。
-   Id およびアクセス トークンに送信されている場所 (リダイレクト URI と呼ばれます)。
-   クライアントへのアクセスを許可されているリソースの一覧 (スコープと呼ばれます)。

クライアントを構成するときに、`AddInMemoryClients`メソッドが必要ですが、`IEnumerable<Client>`コレクション。 次のコード例に eShopOnContainers モバイル アプリの構成を示しています、 `GetClients` eShopOnContainers 内のこのコレクションを提供するメソッドは、アプリケーションを参照します。

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

この構成は、次のプロパティのデータを指定します。

-   `ClientId`: クライアントの一意の ID。
-   `ClientName`: クライアントは、ログ記録および同意画面を使用する名前を表示します。
-   `AllowedGrantTypes`: クライアントが IdentityServer と対話する方法を指定します。 詳細については、次を参照してください。[認証フローを構成する](#configuring_the_authentication_flow)です。
-   `ClientSecrets`: トークン エンドポイントからトークンを要求するときに使用されるクライアント シークレットの資格情報を指定します。
-   `RedirectUris`: トークンや承認コードが返されるに許可された Uri を指定します。
-   `RequireConsent`: 同意画面が必要かどうかを指定します。
-   `RequirePkce`: 認証コードを使用してクライアントが証明キーを送信する必要があるかどうかを指定します。
-   `PostLogoutRedirectUris`: ログアウト後にリダイレクトする許可された Uri を指定します。
-   `AllowedCorsOrigins`: IdentityServer は原点からのクロス オリジン呼び出しを許可できるように、クライアントの原点を指定します。
-   `AllowedScopes`: クライアントにアクセスできるリソースを指定します。 既定では、クライアントには、リソースへのアクセスがありません。
-   `AllowOfflineAccess`: クライアントが更新トークンを要求できるかどうかを指定します。

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>認証フローの構成

認証フローで付与タイプを指定することによって、クライアントと IdentityServer 間を構成する、`Client.AllowedGrantTypes`プロパティです。 OpenID Connect および OAuth 2.0 の仕様などの認証フローの数を定義します。

-   暗黙の型。 このフローは、ブラウザー ベース アプリケーション用に最適化され、ユーザー認証専用、または認証とアクセス トークン要求に使用する必要があります。 すべてのトークンは、ブラウザー経由で送信され、したがって更新トークンは許可されていないなどの機能を強化します。
-   認証コードです。 このフローもクライアントの認証をサポートしながら、ブラウザーの前面チャネルではなく、バック チャネルのトークンを取得する機能を提供します。
-   ハイブリッド。 このフローは、暗黙の組み合わせと、認証コード付与の種類。 Id トークンは、ブラウザー チャネル経由で送信され、認証コードなどの他の成果物と符号付きのプロトコルの応答が含まれています。 応答の検証が成功した後は、アクセス権を取得および更新トークンをバック チャネルを使用してください。

> [!TIP]
> ハイブリッド認証フローを使用します。 ハイブリッド認証フローは、多くのブラウザー チャネルに適用される攻撃の軽減、ネイティブ アプリケーションのアクセス トークンを取得 (および場合によってトークンを更新) することをお勧めフローです。

認証フローの詳細については、次を参照してください。[付与タイプ](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html)、IdentityServer 4 マニュアルを参照します。

### <a name="performing-authentication"></a>認証を実行します。

ユーザーの代理としてトークンを発行する IdentityServer のユーザー必要がありますにサインイン IdentityServer です。 ただし、IdentityServer しないユーザー インターフェイスまたはデータベースの情報認証します。 したがって、eShopOnContainers 参照アプリケーションの ASP.NET Core Id はこの目的に使用します。

EShopOnContainers モバイル アプリでは、図 9-2 に示すように、ハイブリッド認証フローを IdentityServer で認証されます。

![](authentication-and-authorization-images/sign-in.png "サインイン プロセスの概要")

**図 9-2:**サインイン プロセスの概要

サインイン要求が行われる`<base endpoint>:5105/connect/authorize`です。 次の認証が成功した IdentityServer は認証コードと id トークンを含む認証の応答を返します。 認証コードに送信し、`<base endpoint>:5105/connect/token`アクセス、id、および更新トークンに応答します。

EShopOnContainers モバイル アプリ記号アウト IdentityServer の要求を送信することによって`<base endpoint>:5105/connect/endsession`、追加のパラメーターを使用します。 サインアウトが発生した後、IdentityServer はモバイル アプリに post ログアウト リダイレクト URI を送信することによって応答します。 図 9-3 は、このプロセスを示しています。

![](authentication-and-authorization-images/sign-out.png "サインアウト プロセスの概要")

**図 9-3:**サインアウト プロセスの概要

EShopOnContainers のモバイル アプリで IdentityServer との通信は、`IdentityService`クラスを実装する、`IIdentityService`インターフェイスです。 このインターフェイスを実装するクラスを提供する必要があります指定`CreateAuthorizationRequest`、 `CreateLogoutRequest`、および`GetTokenAsync`メソッドです。

#### <a name="signing-in"></a>サインイン

ユーザーがタップしたときに、**ログイン**のボタンでは、 `LoginView`、`SignInCommand`で、`LoginViewModel`クラスを実行すると、順番が実行される、`SignInAsync`メソッドです。 次のコード例では、このメソッドを示します。

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

このメソッドを呼び出して、`CreateAuthorizationRequest`メソッドで、`IdentityService`クラスは、次のコード例に示されています。

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

このメソッドは、IdentityServer の URI を作成[承認エンドポイント](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html)、必要なパラメーターを使用します。 承認エンドポイントである`/connect/authorize`ポートでのユーザー設定として公開されている基本のエンドポイントの 5105 です。 ユーザー設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)です。

> [!NOTE]
> EShopOnContainers モバイル アプリの攻撃対象領域を削減するには、OAuth をコード Exchange (PKCE) 拡張機能の証明キーの実装です。 PKCE では、認証コードがそれが傍受された場合に使用されていることを防止できます。 これのハッシュが承認要求で渡されると、秘密の検証コードを生成するクライアントによってこれを実現し、表示がハッシュされていない認証コードを引き換えるときにします。 PKCE の詳細については、次を参照してください。 [OAuth パブリック クライアントによってコードの Exchange 用の証明キー](https://tools.ietf.org/html/rfc7636) 、インターネット技術標準化委員会の web サイトです。

返された URI が格納されている、`LoginUrl`のプロパティ、`LoginViewModel`クラスです。 ときに、`IsLogin`プロパティになります`true`、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)で、`LoginView`可視になります。 `WebView`データ バインド、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/)プロパティを`LoginUrl`のプロパティ、`LoginViewModel`クラス、および、したがって IdentityServer へのサインイン要求時に、`LoginUrl`プロパティに設定IdentityServer の承認エンドポイントです。 IdentityServer がこの要求を受信し、ユーザーが認証されていない、`WebView`図 9-4 で表示されている構成済みのログイン ページにリダイレクトされます。

![](authentication-and-authorization-images/login.png "WebView によって表示されるログイン ページ")

**図 9-4:** WebView によって表示されるログイン ページ

ログインが完了した後、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)戻り値の URI にリダイレクトされます。 これは、`WebView`ナビゲーションが発生、`NavigateAsync`メソッドで、`LoginViewModel`次のコード例に示されているクラスを実行します。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

このメソッドは、戻り値の URI に含まれている認証の応答を解析し、有効な承認コードが存在であることが要求を IdentityServer の[トークン エンドポイント](https://identityserver4.readthedocs.io/en/release/endpoints/token.html)、認証コードを渡すこと、PKCE シークレットの検証方法、およびその他のパラメーターが必要です。 トークン エンドポイントである`/connect/token`ポートでのユーザー設定として公開されている基本のエンドポイントの 5105 です。 ユーザー設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)です。

>💡 **ヒント:**: 検証の Uri を返します。 EShopOnContainers モバイル アプリでは、戻り値の URI を検証しませんは、戻り値の URI が開くリダイレクト攻撃を防ぐため、既知の場所を指すことを検証することをお勧めします。

トークン エンドポイントでは、有効な認証コードと PKCE シークレット検証機能を受信する場合は、アクセス トークン、id トークンと更新トークンを返します。 Id トークンとアクセス トークン (API リソースへのアクセスを許可する) は、アプリケーション設定として格納し、され、ページ ナビゲーションが行われます。 そのため、これは eShopOnContainers モバイル アプリでの全体的な影響: に移動したことをユーザーが IdentityServer で正常に認証すること、 `MainView`  ページで、これは、 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)表示する、`CatalogView`として、選択されているタブです。

ページ ナビゲーション方法については、次を参照してください。[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)です。 方法については[ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)ナビゲーションにより、ビュー モデル メソッドが実行されるを参照してください[動作を使用して呼び出すナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)です。 アプリケーションの設定については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)です。

> [!NOTE]
> EShopOnContainers こともできますモック サイン イン モック サービスを使用するアプリが構成されている場合、`SettingsView`です。 このモードでは、アプリが IdentityServer、代わりに、資格情報を使用してサインインするユーザーを許可する通信しません。

#### <a name="signing-out"></a>署名アウト

ユーザーがタップしたときに、 **LOG OUT**ボタンをクリックして、 `ProfileView`、`LogoutCommand`で、`ProfileViewModel`クラスを実行すると、順番が実行される、`LogoutAsync`メソッドです。 このメソッドを実行するページ ナビゲーション、 `LoginView`  ページで、渡す、`LogoutParameter`に設定インスタンス`true`をパラメーターとして。 ページ ナビゲーション中に渡すパラメーターの詳細については、次を参照してください。[ナビゲーション中にパラメーターを渡す](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)です。

ビューが作成されに移動したときに、`InitializeAsync`ビューの関連するビューのモデルのメソッドを実行すると、しが実行される、`Logout`のメソッド、`LoginViewModel`クラスは、次のコード例に示されています。

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

このメソッドを呼び出して、`CreateLogoutRequest`メソッドで、`IdentityService`をパラメーターとして、クラス、id トークンを渡すことがアプリケーションの設定から取得します。 アプリケーション設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)です。 次のコード例は、`CreateLogoutRequest`メソッド。

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

このメソッドは、IdentityServer の URI を作成します。[エンドポイントのセッションを終了](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession)、必要なパラメーターを使用します。 終了セッション エンドポイントである`/connect/endsession`ポートでのユーザー設定として公開されている基本のエンドポイントの 5105 です。 ユーザー設定の詳細については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)です。

返された URI が格納されている、`LoginUrl`のプロパティ、`LoginViewModel`クラスです。 中に、`IsLogin`プロパティは`true`、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)で、`LoginView`は表示します。 `WebView`データ バインド、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/)プロパティを`LoginUrl`のプロパティ、`LoginViewModel`クラス、およびサインアウト要求 IdentityServer になるときに、`LoginUrl`プロパティに設定IdentityServer の最後のセッションのエンドポイントです。 IdentityServer は、ユーザーがサインインしているがこの要求を受け取り、サインアウトが行われます。 認証は、ASP.NET Core から cookie 認証ミドルウェアによって管理されているクッキーを使って追跡されます。 したがって、IdentityServer からサインアウト認証 cookie を削除し、URI でクライアントに送り返し post ログアウト リダイレクトを送信します。

モバイル アプリで、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) post ログアウト リダイレクト URI にリダイレクトされます。 これは、`WebView`ナビゲーションが発生、`NavigateAsync`メソッドで、`LoginViewModel`次のコード例に示されているクラスを実行します。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

このメソッドは、id トークンとアプリケーションの設定 からアクセス トークンの両方をクリアし、設定、`IsLogin`プロパティを`false`、これにより、 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)上、`LoginView`ページを非表示になります. 最後に、`LoginUrl`プロパティが、URI の IdentityServer の[承認エンドポイント](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html)、必須のパラメーターを使用して準備するため、次回のために、ユーザーを開始、サインインします。

ページ ナビゲーション方法については、次を参照してください。[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)です。 方法については[ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)ナビゲーションにより、ビュー モデル メソッドが実行されるを参照してください[動作を使用して呼び出すナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)です。 アプリケーションの設定については、次を参照してください。[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)です。

> [!NOTE]
> EShopOnContainers こともできますモック サインアウト、SettingsView のモック サービスを使用するアプリが構成されている場合。 このモードでは、アプリは IdentityServer と通信しませんし、代わりにアプリケーション設定 からのすべてのストアド トークンをクリアします。

<a name="authorization" />

## <a name="authorization"></a>承認

認証後、ASP.NET Core web Api が多くの場合、アクセスを承認する必要がありますをにより、サービス Api を使用する一部の認証済みユーザーに利用可能なすべてではなくにします。

コント ローラーに承認属性を適用して ASP.NET Core MVC ルートへのアクセス制限を実現できます。 またはアクション、コント ローラーへのアクセスを制限またはアクションを認証されたユーザー、次のコード例に示すようにします。

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

権限のないユーザーがコント ローラーまたはアクションでマークされているアクセスを試みると、`Authorize`属性、MVC フレームワークは、401 (未承認) HTTP ステータス コードを返します。

> [!NOTE]
> パラメーターを指定することができます、 `Authorize` API ユーザーを制限する特定の属性です。 詳細については、次を参照してください。[承認](/aspnet/core/security/authorization/introduction/)です。

IdentityServer は、アクセス トークンの承認を制御を提供するように、承認ワークフローに統合できます。 このアプローチには、図 9-5 が表示されます。

![](authentication-and-authorization-images/authorization.png "アクセス トークンによる認証")

**図 9-5:**アクセス トークンによる認証

EShopOnContainers モバイル アプリでは、identity マイクロ サービスと通信し、認証プロセスの一環としてアクセス トークンを要求します。 アクセス トークンは、アクセス要求の一部として、順序付けとバスケット microservices によって公開される Api に転送されます。 アクセス トークンは、クライアント、およびユーザーに関する情報を格納します。 Api では、データへのアクセスを承認するためにその情報を使用します。 Api を保護する IdentityServer を構成する方法については、次を参照してください。 [API リソースの構成](#configuring-api-resources)です。

### <a name="configuring-identityserver-to-perform-authorization"></a>承認を行う IdentityServer を構成します。

IdentityServer による承認を実行するには、web アプリケーションの HTTP 要求パイプラインへの認証ミドルウェアを追加してください。 ミドルウェアが追加された、`ConfigureAuth`メソッドで web アプリケーションの`Startup`から起動されるクラス、`Configure`メソッド、eShopOnContainers の参照をアプリケーションから次のコード例に示すよう。

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

このメソッドは、API が有効なアクセス トークンにだけアクセスできることを確認します。 ミドルウェアは、信頼できる発行者から送信されることを確認する受信トークンを検証し、トークンが有効で、それを受信する API で使用することを検証します。 したがって、順序付けまたはバスケット コント ローラーを参照するには、401 (未承認) HTTP ステータス コード、アクセス トークンが必要であることを示すは返します。

> [!NOTE]
> MVC を追加する前に、IdentityServer の認証ミドルウェアを web アプリケーションの HTTP 要求パイプラインに追加する必要があります`app.UseMvc()`または`app.UseMvcWithDefaultRoute()`です。

### <a name="making-access-requests-to-apis"></a>Api へのアクセス要求を作成します。

要求を行う、順序付けとバスケット microservices、アクセス トークンを認証プロセス中に IdentityServer から取得したときにの次のコード例に示すように、要求に含める必要があります。

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

アクセス トークンをプラットフォーム固有の記憶域から取得されへの呼び出しに含まれているが、アプリケーション設定として格納されて、`GetOrderAsync`メソッドで、`OrderService`クラスです。

同様に、アクセス トークンは必ず必要 API を保護された、IdentityServer にデータを送信するときに次のコード例に示すように。

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

アクセス トークンがプラットフォーム固有の記憶域から取得されへの呼び出しに含まれる、`UpdateBasketAsync`メソッドで、`BasketService`クラスです。

`RequestProvider`クラス、eShopOnContainers のモバイル アプリでは使用して、 `HttpClient` eShopOnContainers 参照アプリケーションによって公開される RESTful Api に要求するクラス。 順序やバスケット Api は、認証を必要とすることが要求したときに有効なアクセス トークンが要求に含める必要があります。 ヘッダーにアクセス トークンを追加することによってこれは、`HttpClient`インスタンス、次のコード例で示したようにします。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders`のプロパティ、`HttpClient`クラスは、各要求と一緒に送信されるヘッダーを公開し、アクセス トークンに追加、`Authorization`ヘッダー文字列で始まる`Bearer`です。 値、rest ベースの API に要求が送信されたとき、`Authorization`ヘッダーが抽出され、検証、信頼できる発行者から送信されてきたし、ユーザーが API を呼び出す権限を持っているかどうかを判断するために使用する受信したことを確認します。

EShopOnContainers モバイル アプリを使用して web 要求方法の詳細については、次を参照してください。[リモート データにアクセスする](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)です。

## <a name="summary"></a>まとめ

ASP.NET MVC web アプリケーションと通信する Xamarin.Forms アプリへの認証と承認の統合に多くの方法はあります。 EShopOnContainers モバイル アプリでは、認証とによる IdentityServer 4 を使用するコンテナー化 identity マイクロ サービスを実行します。 IdentityServer は、ベアラー トークンの認証を実行する ASP.NET Core Id と統合する ASP.NET core オープン ソースの OpenID Connect および OAuth 2.0 フレームワークです。

モバイル アプリでは、ユーザーを認証するため、またはリソースにアクセスするために、IdentityServer からセキュリティ トークンを要求します。 リソースにアクセスするときに、アクセス トークンを承認を必要とする Api への要求に含める必要があります。 IdentityServer のミドルウェアは、信頼できる発行者から送信されることが受信する、API で使用するのには有効であることを確認する受信のアクセス トークンを検証します。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
