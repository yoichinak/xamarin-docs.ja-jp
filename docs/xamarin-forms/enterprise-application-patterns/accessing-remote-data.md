---
title: "リモートデータへのアクセス" の説明: "この章では、eShopOnContainers モバイルアプリがコンテナー化されたマイクロサービスからデータにアクセスする方法について説明します。"
ms. 製品: xamarin ms. assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 08/07/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="accessing-remote-data"></a>リモート データへのアクセス

多くの最新の web ベースソリューションでは、web サーバーによってホストされる web サービスを使用して、リモートクライアントアプリケーションの機能を提供しています。 Web サービスにより公開される操作によって Web API が構成されています。

クライアントアプリは、API が公開するデータや操作がどのように実装されているかを把握していなくても、web API を利用できる必要があります。 このためには、API が一般的な標準に準拠している必要があります。これにより、クライアントアプリと web サービスは、使用するデータ形式と、クライアントアプリと web サービスの間で交換されるデータの構造に同意することができます。

## <a name="introduction-to-representational-state-transfer"></a>表現の状態転送の概要

表現は、ハイパーメディアに基づいて分散システムを構築するためのアーキテクチャスタイルです。 REST モデルの主な利点は、オープンスタンダードに基づいており、モデルの実装や、それにアクセスするクライアントアプリを特定の実装にバインドしないことです。 したがって、REST web サービスは Microsoft ASP.NET コア MVC を使用して実装できます。クライアントアプリは、HTTP 要求を生成して HTTP 応答を解析できる任意の言語およびツールセットを使用して開発できます。

REST モデルでは、ナビゲーションスキームを使用して、リソースと呼ばれるネットワーク上のオブジェクトとサービスを表します。 REST を実装するシステムは、通常、これらのリソースにアクセスするための要求を送信するために HTTP プロトコルを使用します。 このようなシステムでは、クライアントアプリはリソースを識別する URI 形式で要求を送信し、そのリソースに対して実行される操作を示す HTTP メソッド (GET、POST、PUT、DELETE など) を送信します。 HTTP 要求の本文には、操作を実行するために必要なすべてのデータが含まれています。

> [!NOTE]
> REST は、ステートレスな要求モデルを定義します。 そのため、HTTP 要求は独立している必要があり、任意の順序で発生する可能性があります。

REST 要求からの応答では、標準の HTTP 状態コードが使用されます。 たとえば、有効なデータを返す要求には HTTP 応答コード 200 (OK) を含める必要があり、指定したリソースの確認および削除に失敗した要求は HTTP 状態コード 404 (Not Found) を含む応答を返す必要があります。

RESTful web API は、接続されたリソースのセットを公開し、アプリがリソースを操作してそれらの間を簡単に移動できるようにするための主要な操作を提供します。 このため、一般的な RESTful web API を構成する Uri は、公開するデータに向いており、HTTP によって提供される機能を使用してこのデータを操作します。

HTTP 要求でクライアントアプリによって含まれるデータと、web サーバーからの対応する応答メッセージは、メディアの種類と呼ばれるさまざまな形式で表現できます。 クライアントアプリは、メッセージの本文でデータを返す要求を送信するときに、要求のヘッダーで処理できるメディアの種類を指定でき `Accept` ます。 Web サーバーがこのメディアの種類をサポートしている場合は、 `Content-Type` メッセージ本文のデータ形式を指定するヘッダーを含む応答で応答できます。 次に、応答メッセージを解析し、メッセージ本文の結果を適切に解釈するクライアントアプリの役割を担います。

REST の詳細については、「 [api の設計](/azure/architecture/best-practices/api-design/)と[api の実装](/azure/architecture/best-practices/api-implementation/)」を参照してください。

## <a name="consuming-restful-apis"></a>RESTful Api の使用

EShopOnContainers モバイルアプリは、モデルビュービューモデル (MVVM) パターンを使用し、パターンのモデル要素はアプリで使用されるドメインエンティティを表します。 EShopOnContainers 参照アプリケーションのコントローラークラスとリポジトリクラスは、これらのモデルオブジェクトの多くを受け入れて返します。 そのため、モバイルアプリとコンテナー化されたマイクロサービスの間で渡されるすべてのデータを保持するデータ転送オブジェクト (Dto) として使用されます。 Dto を使用して web サービスとの間でデータをやり取りする主な利点は、1回のリモート呼び出しでより多くのデータを送信することにより、アプリは、実行する必要があるリモート呼び出しの数を減らすことができることです。

### <a name="making-web-requests"></a>Web 要求を作成する

EShopOnContainers モバイルアプリでは、クラスを使用して `HttpClient` HTTP 経由で要求を行い、JSON をメディアの種類として使用します。 このクラスは、HTTP 要求を非同期的に送信し、URI で識別されたリソースから HTTP 応答を受信するための機能を提供します。 クラスは、 `HttpResponseMessage` http 要求が行われた後に REST API から受信した http 応答メッセージを表します。 これには、ステータスコード、ヘッダー、および任意の本文を含む、応答に関する情報が含まれます。 クラスは、 `HttpContent` HTTP 本文とコンテンツヘッダー (やなど) を表し `Content-Type` `Content-Encoding` ます。 コンテンツは、 `ReadAs` `ReadAsStringAsync` `ReadAsByteArrayAsync` データの形式に応じて、やなどの任意のメソッドを使用して読み取ることができます。

#### <a name="making-a-get-request"></a>GET 要求を行う

クラスは、 `CatalogService` カタログマイクロサービスからのデータ取得プロセスを管理するために使用されます。 クラスの `RegisterDependencies` メソッドでは、クラスは、 `ViewModelLocator` `CatalogService` `ICatalogService` Autofac 依存関係挿入コンテナーを持つ型に対する型マッピングとして登録されます。 次に、クラスのインスタンス `CatalogViewModel` が作成されると、そのコンストラクターは、 `ICatalogService` クラスのインスタンスを返す Autofac に解決される型を受け入れ `CatalogService` ます。 依存関係の挿入の詳細については、「[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection)」を参照してください。

図10-1 は、によって表示されるようにカタログマイクロサービスからカタログデータを読み取るクラスの相互作用を示して `CatalogView` います。

[![](accessing-remote-data-images/catalogdata.png "Retrieving data from the catalog microservice")](accessing-remote-data-images/catalogdata-large.png#lightbox "Retrieving data from the catalog microservice")

**図 10-1**: カタログマイクロサービスからのデータの取得

`CatalogView`に移動すると、クラスの `OnInitialize` メソッド `CatalogViewModel` が呼び出されます。 このメソッドは、次のコード例に示すように、カタログマイクロサービスからカタログデータを取得します。

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

このメソッドは `GetCatalogAsync` `CatalogService` 、Autofac によってに挿入されたインスタンスのメソッドを呼び出し `CatalogViewModel` ます。 次のコード例は、`GetCatalogAsync` メソッドを示しています。

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

このメソッドは、要求が送信されるリソースを識別する URI を構築し、クラスを使用して `RequestProvider` リソースの GET HTTP メソッドを呼び出し、結果をに返し `CatalogViewModel` ます。 クラスには、 `RequestProvider` リソースを識別する URI の形式で要求を送信する機能、そのリソースに対して実行される操作を示す HTTP メソッド、および操作を実行するために必要なデータを含む本文が含まれます。 クラスをに挿入する方法については `RequestProvider` `CatalogService class` 、「[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection)」を参照してください。

次のコード例は、クラスのメソッドを示してい `GetAsync` `RequestProvider` ます。

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

このメソッドは、 `CreateHttpClient` 適切なヘッダーが設定されたクラスのインスタンスを返すメソッドを呼び出します `HttpClient` 。 次に、URI で識別されるリソースに非同期の GET 要求を送信し、応答をインスタンスに格納し `HttpResponseMessage` ます。 次に、 `HandleResponse` メソッドが呼び出されます。これは、応答に成功 HTTP 状態コードが含まれていない場合に例外をスローします。 次に、応答が文字列として読み取られ、JSON からオブジェクトに変換され、 `CatalogRoot` に返され `CatalogService` ます。

`CreateHttpClient`メソッドを次のコード例に示します。

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

このメソッドは、クラスの新しいインスタンスを作成 `HttpClient` し、 `Accept` インスタンスによって行われたすべての要求のヘッダーをに設定します。これは `HttpClient` `application/json` 、JSON を使用して応答のコンテンツを書式設定することを想定していることを示します。 その後、アクセストークンが引数としてメソッドに渡された場合は、 `CreateHttpClient` インスタンスに `Authorization` よって行われたすべての要求のヘッダーに、文字列がプレフィックスとして付加され `HttpClient` `Bearer` ます。 承認の詳細については、「[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)」を参照してください。

クラスの `GetAsync` メソッドが `RequestProvider` を呼び出すと `HttpClient.GetAsync` 、 `Items` 次の `CatalogController` コード例に示すように、Catalog. API プロジェクトのクラスのメソッドが呼び出されます。

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

このメソッドは、EntityFramework を使用して SQL データベースからカタログデータを取得し、成功 HTTP 状態コードと JSON 形式のインスタンスのコレクションを含む応答メッセージとして返し `CatalogItem` ます。

#### <a name="making-a-post-request"></a>POST 要求を行う

クラスは、 `BasketService` バスケットマイクロサービスを使用してデータの取得と更新のプロセスを管理するために使用されます。 クラスの `RegisterDependencies` メソッドでは、クラスは、 `ViewModelLocator` `BasketService` `IBasketService` Autofac 依存関係挿入コンテナーを持つ型に対する型マッピングとして登録されます。 次に、クラスのインスタンス `BasketViewModel` が作成されると、そのコンストラクターは、 `IBasketService` クラスのインスタンスを返す Autofac に解決される型を受け入れ `BasketService` ます。 依存関係の挿入の詳細については、「[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection)」を参照してください。

図10-2 は、によって表示されるバスケットデータを送信するクラスの相互作用を `BasketView` バスケットマイクロサービスに示しています。

[![](accessing-remote-data-images/basketdata.png "Sending data to the basket microservice")](accessing-remote-data-images/basketdata-large.png#lightbox "Sending data to the basket microservice")

**図 10-2**: バスケットマイクロサービスへのデータの送信

買い物かごに項目が追加されると、 `ReCalculateTotalAsync` クラスのメソッド `BasketViewModel` が呼び出されます。 このメソッドは、次のコード例に示すように、バスケット内の項目の合計値を更新し、バスケットデータをバスケットマイクロサービスに送信します。

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

このメソッドは `UpdateBasketAsync` `BasketService` 、Autofac によってに挿入されたインスタンスのメソッドを呼び出し `BasketViewModel` ます。 メソッドを次のメソッドに示し `UpdateBasketAsync` ます。

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

このメソッドは、要求が送信されるリソースを識別する URI を構築し、クラスを使用して `RequestProvider` リソースの POST HTTP メソッドを呼び出し、結果をに返し `BasketViewModel` ます。 バスケットマイクロサービスへの要求を承認するには、認証プロセス中に、サーバーから取得したアクセストークンが必要です。 承認の詳細については、「[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)」を参照してください。

次のコード例は、クラスのメソッドの1つを示してい `PostAsync` `RequestProvider` ます。

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

このメソッドは、 `CreateHttpClient` 適切なヘッダーが設定されたクラスのインスタンスを返すメソッドを呼び出します `HttpClient` 。 次に、URI によって識別されるリソースに非同期 POST 要求を送信します。これには、JSON 形式で送信されるシリアル化されたバスケットデータと、インスタンスに格納されている応答が含まれ `HttpResponseMessage` ます。 次に、 `HandleResponse` メソッドが呼び出されます。これは、応答に成功 HTTP 状態コードが含まれていない場合に例外をスローします。 次に、応答が文字列として読み取られ、JSON からオブジェクトに変換され、 `CustomerBasket` に返され `BasketService` ます。 メソッドの詳細については `CreateHttpClient` 、「 [GET 要求の作成](#making-a-get-request)」を参照してください。

クラスの `PostAsync` メソッドが `RequestProvider` を呼び出すと `HttpClient.PostAsync` 、 `Post` 次の `BasketController` コード例に示すように、バスケットの API プロジェクトのクラスのメソッドが呼び出されます。

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

このメソッドは、クラスのインスタンスを使用して、 `RedisBasketRepository` バスケットデータを Redis cache に保存し、成功 HTTP 状態コードと JSON 形式のインスタンスを含む応答メッセージとしてそれを返し `CustomerBasket` ます。

#### <a name="making-a-delete-request"></a>削除要求を行う

図10-3 は、のバスケットマイクロサービスからバスケットデータを削除するクラスの相互作用を示して `CheckoutView` います。

![](accessing-remote-data-images/checkoutdata.png "Deleteing data from the basket microservice")

**図 10-3**: バスケットマイクロサービスからのデータの削除

チェックアウトプロセスが呼び出されると、 `CheckoutAsync` クラスのメソッド `CheckoutViewModel` が呼び出されます。 このメソッドは、次のコード例に示すように、買い物かごをクリアする前に新しい注文を作成します。

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

このメソッドは `ClearBasketAsync` `BasketService` 、Autofac によってに挿入されたインスタンスのメソッドを呼び出し `CheckoutViewModel` ます。 メソッドを次のメソッドに示し `ClearBasketAsync` ます。

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

このメソッドは、要求の送信先となるリソースを識別する URI を構築し、クラスを使用して `RequestProvider` リソースの DELETE HTTP メソッドを呼び出します。 バスケットマイクロサービスへの要求を承認するには、認証プロセス中に、サーバーから取得したアクセストークンが必要です。 承認の詳細については、「[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)」を参照してください。

次のコード例は、クラスのメソッドを示してい `DeleteAsync` `RequestProvider` ます。

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

このメソッドは、 `CreateHttpClient` 適切なヘッダーが設定されたクラスのインスタンスを返すメソッドを呼び出します `HttpClient` 。 次に、URI によって識別されるリソースに非同期の DELETE 要求を送信します。 メソッドの詳細については `CreateHttpClient` 、「 [GET 要求の作成](#making-a-get-request)」を参照してください。

クラスの `DeleteAsync` メソッドが `RequestProvider` を呼び出すと `HttpClient.DeleteAsync` 、 `Delete` 次の `BasketController` コード例に示すように、バスケットの API プロジェクトのクラスのメソッドが呼び出されます。

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

このメソッドは、クラスのインスタンスを使用して、 `RedisBasketRepository` Redis cache からバスケットデータを削除します。

## <a name="caching-data"></a>キャッシュされたデータ

アプリのパフォーマンスは、頻繁にアクセスされるデータをアプリの近くにある高速ストレージにキャッシュすることで改善できます。 高速ストレージが元のソースよりもアプリの近くにある場合、データを取得するときにキャッシュを使用すると応答時間が大幅に短縮されます。

キャッシュの最も一般的な形式は、キャッシュを参照することによってアプリがデータを取得するリードスルーキャッシュです。 データがキャッシュにない場合は、データ ストアから取得され、キャッシュに追加されます。 アプリでは、キャッシュアサイドパターンを使用してリードキャッシュを実装できます。 このパターンは、項目が現在キャッシュ内にあるかどうかを判断します。 項目がキャッシュ内にない場合は、データストアから読み取られ、キャッシュに追加されます。 詳細については、[キャッシュアサイド](/azure/architecture/patterns/cache-aside/)パターンを参照してください。

> [!TIP]
> 頻繁に読み取られ、変更頻度が低いデータをキャッシュします。 このデータは、アプリによって初めて取得されるときに、必要に応じてキャッシュに追加できます。 つまり、アプリはデータストアからデータを1回だけフェッチする必要があり、その後のアクセスはキャッシュを使用して満たすことができます。

EShopOnContainers reference アプリケーションなどの分散アプリケーションは、次のいずれかまたは両方のキャッシュを提供する必要があります。

- 共有キャッシュ。複数のプロセスまたはコンピューターからアクセスできます。
- プライベートキャッシュ。アプリを実行しているデバイス上でデータがローカルに保持されます。

EShopOnContainers モバイルアプリでは、アプリのインスタンスを実行しているデバイス上でデータがローカルに保持されるプライベートキャッシュを使用します。 EShopOnContainers reference アプリケーションで使用されるキャッシュの詳細については、「 [.Net マイクロサービス: コンテナー化された .Net アプリケーションのアーキテクチャ](https://aka.ms/microservicesebook)」を参照してください。

> [!TIP]
> キャッシュは、いつでも消失する可能性がある一時的なデータストアと考えることができます。 データが元のデータストアおよびキャッシュに保持されていることを確認します。 キャッシュが使用できなくなった場合、データが失われる可能性は最小限に抑えられます。

### <a name="managing-data-expiration"></a>データの有効期限の管理

キャッシュされたデータは常に元のデータと一貫性があるとは限りません。 元のデータストアのデータはキャッシュされた後に変更される可能性があるため、キャッシュされたデータは古くなっています。 そのため、アプリでは、キャッシュ内のデータを可能な限り最新の状態に保つための戦略を実装する必要がありますが、キャッシュ内のデータが古くなったときに発生する状況を検出して処理することもできます。 ほとんどのキャッシュメカニズムでは、データの有効期限が切れるようにキャッシュを構成できます。そのため、データが古くなっている可能性がある期間を短縮できます。

> [!TIP]
> キャッシュを構成するときの既定の有効期限を設定します。 多くのキャッシュは、有効期限を実装します。これは、データを無効にし、指定された期間にわたってアクセスされない場合にキャッシュから削除します。 ただし、有効期限を選択するときは注意が必要です。 短すぎると、データが短時間で期限切れになり、キャッシュの利点が減少します。 時間が長すぎると、データのリスクが古くなります。 そのため、有効期限は、データを使用するアプリのアクセスパターンと一致している必要があります。

キャッシュされたデータの有効期限が切れると、キャッシュから削除され、アプリは元のデータストアからデータを取得してキャッシュに戻す必要があります。

また、データが長期間保持される可能性がある場合は、キャッシュがいっぱいになる可能性もあります。 そのため、新しい項目をキャッシュに追加する要求では、*削除と呼ば*れるプロセス内の一部の項目を削除する必要がある場合があります。 キャッシュサービスは、通常、最近使用されていないデータを削除します。 ただし、最近使用したものと先入れ先出しを含む削除ポリシーは他にもあります。詳細については、「[キャッシュのガイダンス](/azure/architecture/best-practices/caching/)」を参照してください。

### <a name="caching-images"></a>イメージのキャッシュ

EShopOnContainers モバイルアプリは、キャッシュされることによるメリットが得られるリモートの製品イメージを消費します。 これらのイメージは、 [`Image`](xref:Xamarin.Forms.Image) コントロールと、 `CachedImage` [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/)ライブラリによって提供されるコントロールによって表示されます。

コントロールは、 Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) ダウンロードされたイメージのキャッシュをサポートしています。 キャッシュは既定で有効になり、イメージは24時間ローカルに保存されます。 また、プロパティを使用して有効期限を構成することもでき [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) ます。 詳細については、「ダウンロードした[イメージのキャッシュ](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)」を参照してください。

FFImageLoading の `CachedImage` コントロールは、 Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) 追加の機能を有効にする追加のプロパティを提供するコントロールの代替手段です。 この機能の中で、コントロールは構成可能なキャッシュを提供し、エラーをサポートし、イメージのプレースホルダーを読み込みます。 次のコード例では、eShopOnContainers モバイルアプリで、でコントロール `CachedImage` `ProductTemplate` によって使用されるデータテンプレートであるコントロールを使用する方法を示し [`ListView`](xref:Xamarin.Forms.ListView) `CatalogView` ます。

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

コントロールは、 `CachedImage` `LoadingPlaceholder` プロパティと `ErrorPlaceholder` プロパティをプラットフォーム固有のイメージに設定します。 プロパティは、 `LoadingPlaceholder` プロパティによって指定されたイメージを取得するときに表示されるイメージを指定します。プロパティは、プロパティ `Source` `ErrorPlaceholder` で指定されたイメージを取得しようとしたときにエラーが発生した場合に表示されるイメージを指定し `Source` ます。

名前が示すように、 `CachedImage` コントロールは、プロパティの値によって指定された時間に対して、デバイス上のリモートイメージをキャッシュし `CacheDuration` ます。 このプロパティ値が明示的に設定されていない場合、既定値の30日が適用されます。

## <a name="increasing-resilience"></a>復元性の向上

リモートサービスとリソースと通信するすべてのアプリは、一時的な障害に対して重要である必要があります。 一時的な障害には、サービスへのネットワーク接続の一時的な損失、サービスが一時的に利用できない、サービスがビジー状態のときに発生するタイムアウトなどがあります。 多くの場合、これらの障害は自己修正であり、適切な遅延の後でアクションが繰り返されると成功する可能性があります。

一時的な障害は、予測可能なすべての状況で完全にテストされている場合でも、アプリの知覚品質に大きな影響を与える可能性があります。 リモートサービスと通信するアプリが確実に動作するようにするには、次のすべてを実行できる必要があります。

- 発生した障害を検出し、障害が一時的なものである可能性があるかどうかを判断します。
- エラーが一時的である可能性があると判断し、操作が再試行された回数を追跡する場合は、操作を再試行してください。
- 適切な再試行戦略を使用します。これにより、再試行の回数、各試行間の遅延、および失敗した後に実行するアクションが指定されます。

この一時的なエラー処理は、再試行パターンを実装するコードで、リモートサービスへのすべての試行をラップすることによって実現できます。

### <a name="retry-pattern"></a>再試行パターン

アプリがリモートサービスに要求を送信しようとしたときにエラーが検出された場合は、次のいずれかの方法でエラーを処理できます。

- 操作を再試行しています。 アプリは、失敗した要求を直ちに再試行できます。
- 遅延後に操作を再試行しています。 アプリは、要求を再試行する前に、適切な時間待機する必要があります。
- 操作を取り消しています。 アプリケーションは操作をキャンセルし、例外を報告する必要があります。

再試行戦略は、アプリのビジネス要件に合わせて調整する必要があります。 たとえば、試行される操作に対する再試行回数と再試行間隔を最適化することが重要です。 操作がユーザー操作の一部である場合は、再試行の間隔を短くする必要があります。また、ユーザーが応答を待機するのを避けるために、数回の再試行だけを試行します。 操作が長時間実行されるワークフローの一部であり、ワークフローの取り消しまたは再起動に負荷がかかる場合や時間がかかる場合は、再試行の間隔を長くして、再試行することが適切です。

> [!NOTE]
> 試行間の待ち時間を最小限に抑え、大量の再試行を行う積極的な再試行戦略では、容量の近くまたは大部分で実行されているリモートサービスが低下する可能性があります。 また、このような再試行戦略は、失敗した操作を継続的に実行しようとしている場合に、アプリの応答性に影響を与える可能性もあります。

多くの再試行の後も要求が失敗する場合は、同じリソースに対する要求がそれ以上失敗しないようにすることをお勧めします。 その後、一定期間が経過すると、アプリはリソースに対して1つ以上の要求を行って、成功したかどうかを確認できます。 詳細については、「[Circuit Breaker Pattern (Circuit Breaker パターン)](#circuit-breaker-pattern)」をご覧ください。

> [!TIP]
> 再試行が際限なく繰り返される設計は確実に避けてください。 有限の回数の再試行を使用するか、サービスが復旧できるように[サーキットブレーカー](/azure/architecture/patterns/circuit-breaker/)パターンを実装します。

EShopOnContainers モバイルアプリは、RESTful web 要求を行うときに、再試行パターンを現在実装していません。 ただし、 `CachedImage` [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/)ライブラリによって提供されるコントロールは、イメージの読み込みを再試行することによって一時的なエラー処理をサポートします。 イメージの読み込みに失敗した場合は、さらに試行されます。 試行回数はプロパティによって指定され `RetryCount` ます。再試行は、プロパティによって指定された遅延の後に行われ `RetryDelay` ます。 これらのプロパティ値が明示的に設定されていない場合、プロパティには既定値が適用され、プロパティには250ミリ秒が適用され `RetryCount` `RetryDelay` ます。 コントロールの詳細については `CachedImage` 、「[イメージのキャッシュ](#caching-images)」を参照してください。

EShopOnContainers 参照アプリケーションは、再試行パターンを実装します。 再試行パターンをクラスと組み合わせる方法の詳細については、 `HttpClient` 「 [.Net マイクロサービス: コンテナー化された .net アプリケーションのアーキテクチャ](https://aka.ms/microservicesebook)」を参照してください。

再試行パターンの詳細については、[再試行](/azure/architecture/patterns/retry/)パターンを参照してください。

### <a name="circuit-breaker-pattern"></a>遮断器のパターン

場合によっては、修正に時間がかかるイベントが原因でエラーが発生することがあります。 このような障害は、一部の接続がサービスの完全な障害に対して部分的に失われる可能性があります。 このような状況では、アプリが成功する可能性が低い操作を再試行することはできません。代わりに、操作が失敗したことを受け入れ、それに応じてこのエラーを処理する必要があります。

サーキットブレーカーパターンを使用すると、失敗する可能性のある操作をアプリが繰り返し試行するのを防ぐことができます。また、アプリケーションでエラーが解決されたかどうかを検出することもできます。

> [!NOTE]
> サーキットブレーカーパターンの目的は、再試行パターンとは異なります。 再試行パターンを使用すると、アプリは成功したと想定して操作を再試行できます。 サーキットブレーカーパターンは、アプリが失敗する可能性のある操作を実行できないようにします。

サーキット ブレーカーは、失敗する可能性のある操作のプロキシとして機能します。 プロキシは、発生した最近のエラーの数を監視し、この情報を使用して操作の続行を許可するか、例外を直ちに返すかを決定します。

EShopOnContainers モバイルアプリは、現在、サーキットブレーカーパターンを実装していません。 ただし、eShopOnContainers はそのように動作します。 詳細については、「 [.Net マイクロサービス: コンテナー化された .Net アプリケーションのアーキテクチャ](https://aka.ms/microservicesebook)」を参照してください。

> [!TIP]
> 再試行とサーキットブレーカーのパターンを結合します。 アプリでは、再試行パターンを使ってサーキットブレーカーを介して操作を呼び出すことにより、再試行とサーキットブレーカーのパターンを組み合わせることができます。 ただし、再試行ロジックは、サーキット ブレーカーによって返されるすべての例外から大きな影響を受け、エラーが一時的なものではないことが示されると、再試行回数を破棄します。

サーキットブレーカーパターンの詳細については、「[サーキットブレーカー](/azure/architecture/patterns/circuit-breaker/)パターン」を参照してください。

## <a name="summary"></a>まとめ

多くの最新の web ベースソリューションでは、web サーバーによってホストされる web サービスを使用して、リモートクライアントアプリケーションの機能を提供しています。 Web サービスが公開する操作は、web API を構成します。また、クライアントアプリは、API が公開するデータや操作の実装方法を把握していなくても、web API を利用できなければなりません。

アプリのパフォーマンスは、頻繁にアクセスされるデータをアプリの近くにある高速ストレージにキャッシュすることで改善できます。 アプリでは、キャッシュアサイドパターンを使用してリードキャッシュを実装できます。 このパターンは、項目が現在キャッシュ内にあるかどうかを判断します。 項目がキャッシュ内にない場合は、データストアから読み取られ、キャッシュに追加されます。

Web Api と通信する場合、アプリは一時的な障害に対して重要である必要があります。 一時的な障害には、サービスへのネットワーク接続の一時的な損失、サービスが一時的に利用できない、サービスがビジー状態のときに発生するタイムアウトなどがあります。 多くの場合、これらの障害は自己修正であり、適切な遅延の後でアクションが繰り返されると、成功する可能性があります。 そのため、アプリケーションは、一時的なエラー処理機構を実装するコードで、web API へのすべてのアクセス試行をラップする必要があります。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
