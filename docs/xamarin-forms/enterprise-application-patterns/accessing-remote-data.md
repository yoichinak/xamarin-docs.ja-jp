---
title: リモート データにアクセスします。
description: この章では、eShopOnContainers のモバイル アプリがコンテナー化されたマイクロ サービスからデータにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 3a46b939fa87cd6535c9f86c46981c098542e7c9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105481"
---
# <a name="accessing-remote-data"></a>リモート データにアクセスします。

Web ベース ソリューションの多くの最新のリモート クライアント アプリケーションの機能を提供する、web サーバーでホストされている、web サービスを使用します。 Web サービスを公開する操作は、web API を構成します。

クライアント アプリでは、データや、API により公開される操作を実装する方法を知ることがなく、web API を利用できる必要があります。 これは、API が商務使用、およびクライアント アプリと web サービスの間で交換されるデータの構造をデータ形式に同意するクライアント アプリと web サービスを有効にする一般的な標準が必要です。

## <a name="introduction-to-representational-state-transfer"></a>Representational State Transfer の概要

Representational State Transfer (REST) は、ハイパー メディアに基づき分散システムを構築するためのアーキテクチャ スタイルです。 REST モデルの主な利点は、オープン スタンダードに基づくが、モデルや、特定の実装にアクセスするクライアント アプリの実装をバインドしないことができます。 したがって、REST web サービスは、Microsoft ASP.NET Core MVC を使用して実装でき、任意の言語と HTTP 要求の生成および HTTP 応答を解析できるツールセットを使用してクライアント アプリを開発する可能性があります。

REST モデルでは、ナビゲーション スキームを使用して、リソースと呼ばれる、ネットワーク経由でのオブジェクトとサービスを表します。 残りの部分を通常実装するシステムでは、これらのリソースにアクセスするのに要求を送信するのに HTTP プロトコルを使用します。 このようなシステムでは、クライアント アプリは、リソースを識別する URI とそのリソースに対して実行する操作を示す HTTP メソッド (GET、POST、PUT、または DELETE) などの形式で要求を送信します。 HTTP 要求の本文には、操作を実行するために必要なすべてのデータが含まれています。

> [!NOTE]
> 残りの部分では、ステートレスの要求モデルを定義します。 したがって、HTTP 要求では、独立系である必要があり、任意の順序で発生する可能性があります。

REST からの応答での要求の標準 HTTP ステータス コードを使用します。 たとえば、検索したり、指定したリソースの削除に失敗した要求は、HTTP 状態コード 404 (Not Found) を含む応答を返すようにするときを有効なデータを返す要求は HTTP 応答コード 200 (OK) を含める必要があります。

RESTful web API は、接続されているリソースのセットを公開し、それらのリソースを操作し、それらの間を簡単に移動するアプリを有効にするコア操作を提供します。 このため、一般的な RESTful web API を構成する Uri 指向です、公開するデータと機能が提供される使用してこのデータを操作する HTTP。

HTTP 要求と、web サーバーから対応する応答メッセージでクライアント アプリが含まれるデータは、さまざまなメディアの種類と呼ばれる形式で表示できます。 処理できるメディアの種類を指定できるクライアント アプリでは、メッセージの本文でデータを返す要求を送信するときに、`Accept`要求のヘッダー。 Web サーバーでは、このメディアの種類をサポートする場合を含む応答に応答できます、`Content-Type`ヘッダーがメッセージの本文内のデータの形式を指定します。 応答メッセージを解析し、メッセージの本文に結果を適切に解釈するには、クライアント アプリの責任です。

REST の詳細については、[API の設計](/azure/architecture/best-practices/api-design/)と[API 実装](/azure/architecture/best-practices/api-implementation/)を参照してください。

## <a name="consuming-restful-apis"></a>RESTful Api を使用

EShopOnContainers のモバイル アプリでは、モデル-ビュー-ビューモデル (MVVM) パターンと、アプリで使用されるドメイン エンティティ パターン表現のモデル要素を使用します。 EShopOnContainers 参照アプリケーションのコント ローラーおよびリポジトリ クラスは、そのまま使用し、これらのモデル オブジェクトの多くを返します。 そのため、これらは、モバイル アプリとコンテナー化されたマイクロ サービス間で渡されるすべてのデータを保持するデータ転送オブジェクト (Dto) として使用されます。 Dto を使用してデータを渡すし、web サービスからデータを受信する主な利点は、単一のリモート呼び出しでより多くのデータを送信することで、アプリできます減らすことにする必要があるリモート呼び出しの数です。

### <a name="making-web-requests"></a>Web 要求を作成する

EShopOnContainers のモバイル アプリでは、`HttpClient`メディアの種類として使用されている JSON での HTTP 経由で要求をクラス。 このクラスは、HTTP 要求を非同期的に送信するための機能を提供し、リソースを識別する URI から HTTP 応答を受信します。 `HttpResponseMessage`クラスは、HTTP 要求が行われた後に、REST API から受信した HTTP 応答メッセージを表します。 状態コード、ヘッダー、および任意の本文を含む、応答に関する情報が含まれています。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`します。 いずれかを使用して、コンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式によって異なります。

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>GET 要求を行う

`CatalogService`カタログ マイクロ サービスからのデータの取得プロセスを管理するクラスを使用します。 `RegisterDependencies`メソッドで、`ViewModelLocator`クラス、`CatalogService`に対して型マッピングとクラスが登録されて、 `ICatalogService` Autofac 依存関係の注入コンテナーを持つ型。 その後のインスタンス、`CatalogViewModel`クラスを作成すると、そのコンス トラクターを受け入れる、`ICatalogService`型のインスタンスを返す、Autofac を解決する、`CatalogService`クラス。 依存関係の挿入の詳細については、[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)を参照してください。

図 10-1 で表示するため、カタログ マイクロ サービスからカタログ データを読み取るクラスの相互作用を示しています、`CatalogView`します。

[![](accessing-remote-data-images/catalogdata.png "カタログ マイクロ サービスからデータを取得する")](accessing-remote-data-images/catalogdata-large.png#lightbox "カタログ マイクロ サービスからデータを取得します。")

**図 10-1**: カタログ マイクロ サービスからデータを取得します。

ときに、`CatalogView`への移動が、`OnInitialize`メソッドで、`CatalogViewModel`クラスが呼び出されます。 次のコード例に示すように、このメソッドがカタログ マイクロ サービスからカタログ データを取得します。

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

このメソッドは、`GetCatalogAsync`のメソッド、`CatalogService`に挿入されたインスタンス、 `CatalogViewModel` Autofac をします。 次のコード例は、`GetCatalogAsync`メソッド。

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

このメソッドは、要求に送信するリソースを識別する URI を構築し、使用、`RequestProvider`に結果を返す前に、リソースに GET HTTP メソッドの呼び出しにクラス、`CatalogViewModel`します。 `RequestProvider`クラスには、そのリソースに対して実行する操作を示す HTTP メソッドのリソースを識別する URI の形式で要求を送信する機能が含まれていて、操作を実行するために必要なすべてのデータを格納している本文。 方法については`RequestProvider`クラスが挿入されますが、`CatalogService class`を参照してください[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)します。

次のコード例は、`GetAsync`メソッドで、`RequestProvider`クラス。

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

このメソッドは、`CreateHttpClient`のインスタンスを返すメソッド、`HttpClient`適切なヘッダー セットを持つクラス。 格納されている応答と共に、URI で識別されるリソースへの非同期の GET 要求を送信し、`HttpResponseMessage`インスタンス。 `HandleResponse`メソッドが呼び出されて、応答に成功 HTTP 状態コードが含まれていない場合に例外をスローします。 JSON に変換された文字列として、応答を読み取る、`CatalogRoot`オブジェクト、およびに返される、`CatalogService`します。

`CreateHttpClient`メソッドが次のコード例に示すようにします。

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

このメソッドの新しいインスタンスを作成、`HttpClient`クラス、およびセット、`Accept`によって行われた要求のヘッダー、`HttpClient`インスタンス`application/json`JSON を使用して書式設定するすべての応答のコンテンツが期待していることを示します。 アクセス トークンは、引数として渡された場合、`CreateHttpClient`メソッドに追加されて、`Authorization`によって行われた要求のヘッダー、`HttpClient`インスタンス、文字列で始まる`Bearer`します。 承認の詳細については、[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)を参照してください。

ときに、`GetAsync`メソッドで、`RequestProvider`クラスが呼び出す`HttpClient.GetAsync`、`Items`メソッドで、 `CatalogController` Catalog.API プロジェクト内のクラスが呼び出される、次のコード例に示した。

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

JSON のコレクションが書式設定し、このメソッドは、EntityFramework を使用して SQL database からカタログ データを取得し、成功 HTTP 状態コードを含む応答メッセージとして返します`CatalogItem`インスタンス。

#### <a name="making-a-post-request"></a>POST 要求を行う

`BasketService`バスケット マイクロ サービスでの更新プロセスをデータの取得を管理するクラスを使用します。 `RegisterDependencies`メソッドで、`ViewModelLocator`クラス、`BasketService`に対して型マッピングとクラスが登録されて、 `IBasketService` Autofac 依存関係の注入コンテナーを持つ型。 その後のインスタンス、`BasketViewModel`クラスを作成すると、そのコンス トラクターを受け入れる、`IBasketService`型のインスタンスを返す、Autofac を解決する、`BasketService `クラス。 依存関係の挿入の詳細については、[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)を参照してください。

図 10-2 で表示されるバスケット データを送信するクラスの相互作用を示しています、 `BasketView`、バスケット マイクロ サービスにします。

[![](accessing-remote-data-images/basketdata.png "買い物かごマイクロ サービスへのデータ送信")](accessing-remote-data-images/basketdata-large.png#lightbox "バスケット マイクロ サービスにデータを送信します。")

**図 10-2**: バスケット マイクロ サービスにデータを送信します。

項目が、買い物かごに追加されると、`ReCalculateTotalAsync`メソッドで、`BasketViewModel`クラスが呼び出されます。 このメソッドでは、バスケット内の項目の合計値を更新し、次のコード例に示すようにバスケット マイクロ サービスにバスケット データを送信します。

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

このメソッドは、`UpdateBasketAsync`のメソッド、`BasketService`に挿入されたインスタンス、 `BasketViewModel` Autofac をします。 次のメソッドに示す、`UpdateBasketAsync`メソッド。

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

このメソッドは、要求に送信するリソースを識別する URI を構築し、使用、`RequestProvider`に結果を返す前に、リソースに対して POST HTTP メソッドの呼び出しにクラス、`BasketViewModel`します。 バスケット マイクロ サービスへの要求を承認するために、アクセス トークンは、認証プロセス中に、IdentityServer から取得したので注意が必要です。 承認の詳細については、[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)を参照してください。

次のコード例を示していますの 1 つ、`PostAsync`メソッド、`RequestProvider`クラス。

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

このメソッドは、`CreateHttpClient`のインスタンスを返すメソッド、`HttpClient`適切なヘッダー セットを持つクラス。 バスケットのシリアル化されたデータを JSON 形式、およびに格納されている応答で送信されると、URI で識別されるリソースへの非同期の POST 要求を送信し、`HttpResponseMessage`インスタンス。 `HandleResponse`メソッドが呼び出されて、応答に成功 HTTP 状態コードが含まれていない場合に例外をスローします。 応答から JSON に変換された文字列としては読み取り専用、`CustomerBasket`オブジェクト、およびに返される、`BasketService`します。 詳細については、`CreateHttpClient`メソッドを参照してください[取得要求を行う](#making_a_get_request)します。

ときに、`PostAsync`メソッドで、`RequestProvider`クラスが呼び出す`HttpClient.PostAsync`、`Post`メソッドで、 `BasketController` Basket.API プロジェクト内のクラスが呼び出される、次のコード例に示した。

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

このメソッドのインスタンスを使用して、`RedisBasketRepository`バスケット データ、Redis キャッシュを保持するクラスし、成功 HTTP 状態コードと、JSON を含む応答メッセージを書式設定は、それを返します`CustomerBasket`インスタンス。

#### <a name="making-a-delete-request"></a>削除要求を行う

図 10-3 バスケット マイクロ サービスからのバスケット データを削除するクラスの相互作用を示しています、`CheckoutView`します。

![](accessing-remote-data-images/checkoutdata.png "買い物かごマイクロ サービスからのデータを削除しても")

**図 10-3**: バスケット マイクロ サービスからデータを削除します。

チェック アウト プロセスが呼び出されたときに、`CheckoutAsync`メソッドで、`CheckoutViewModel`クラスが呼び出されます。 このメソッドでは、次のコード例に示すように、買い物かごを消去する前に新しい注文を作成します。

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

このメソッドは、`ClearBasketAsync`のメソッド、`BasketService`に挿入されたインスタンス、 `CheckoutViewModel` Autofac をします。 次のメソッドに示す、`ClearBasketAsync`メソッド。

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

このメソッドは、要求が送信されるリソースを識別する URI を構築し、使用、`RequestProvider`クラスは、リソースに対して DELETE HTTP メソッドを呼び出します。 バスケット マイクロ サービスへの要求を承認するために、アクセス トークンは、認証プロセス中に、IdentityServer から取得したので注意が必要です。 承認の詳細については、[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)を参照してください。

次のコード例は、`DeleteAsync`メソッドで、`RequestProvider`クラス。

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

このメソッドは、`CreateHttpClient`のインスタンスを返すメソッド、`HttpClient`適切なヘッダー セットを持つクラス。 これは、後、URI で識別されるリソースへの非同期の削除要求を送信します。 詳細については、`CreateHttpClient`メソッドを参照してください[取得要求を行う](#making_a_get_request)します。

ときに、`DeleteAsync`メソッドで、`RequestProvider`クラスが呼び出す`HttpClient.DeleteAsync`、`Delete`メソッドで、 `BasketController` Basket.API プロジェクト内のクラスが呼び出される、次のコード例に示した。

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

このメソッドのインスタンスを使用して、`RedisBasketRepository`バスケット データ Redis キャッシュから削除するクラス。

## <a name="caching-data"></a>キャッシュされたデータ

近い場所には、高速のストレージに頻繁にアクセスされるデータをキャッシュすることによって、アプリのパフォーマンスを向上することができます、アプリ。 高速のストレージは、元のソースよりもアプリに近い場所に配置する場合、キャッシュは、応答を大幅に向上できますデータを取得するときにタイムアウトします。

キャッシュの最も一般的な形式は、リード スルー キャッシュでキャッシュを参照することで、アプリがデータを取得します。 データは、キャッシュには、データ ストアから取得があり、キャッシュに追加します。 アプリでは、キャッシュ アサイド パターンでは、リード スルー キャッシュを実装できます。 このパターンは、項目がキャッシュに現在あるかどうかを判断します。 項目がない場合、キャッシュ内が、データ ストアから読み取り、キャッシュに追加します。 詳細については、次を参照してください。、[キャッシュ アサイド](/azure/architecture/patterns/cache-aside/)パターン。

> [!TIP]
> キャッシュ データを頻繁に読み取られ、めったに変更します。 このデータは、必要に応じて、初めてのアプリによって取得されるキャッシュに追加できます。 つまり、アプリは、データ ストアからデータを 1 回だけをフェッチする必要があり、以降のアクセスは、キャッシュを使用して満たすことができること。

分散アプリケーション、eShopOnContainers 参照アプリケーションなどでは、次のキャッシュの一方または両方。

-   共有キャッシュを複数のプロセスやマシンがアクセスできます。
-   データがローカルでアプリを実行しているデバイス上に保持されているプライベート キャッシュします。

EShopOnContainers のモバイル アプリでは、データがローカルで、アプリのインスタンスを実行しているデバイス上に保持されているプライベート キャッシュを使用します。 EShopOnContainers 参照アプリケーションで使用されるキャッシュについては、[.NET マイクロ サービス: コンテナー化された .NET アプリケーション アーキテクチャ](https://aka.ms/microservicesebook)を参照してください。

> [!TIP]
> いつでも消える可能性がある一時的なデータ ストアとしてキャッシュを考えます。 元のデータ ストアだけでなく、キャッシュ データが維持されることを確認します。 キャッシュが使用できなくなった場合、データ損失の可能性がありますが、最小化されます。

### <a name="managing-data-expiration"></a>データの有効期限を管理します。

キャッシュされたデータが元のデータが一貫性のある常にすることを期待するは実用的ではありません。 これはキャッシュされた後、キャッシュ データが古くなる原因と元のデータ ストア内のデータを変更する可能性があります。 そのため、アプリする必要があります、キャッシュ内のデータに限り、最新の状態があることを確認するための戦略を実装がもを検出して、キャッシュ内のデータが古くなるときに発生する状況を処理します。 大部分のキャッシュ メカニズムでは、データ、期限切れにするように構成するキャッシュを有効にして、データが古くなってな期間を短くためです。

> [!TIP]
> 既定の有効期限を設定するキャッシュを構成したときにします。 多くのキャッシュは、有効期限が切れると、データを無効にし、指定した期間にアクセスできない場合、キャッシュから削除を実装します。 ただし、注意が必要、有効期限を選択するときにします。 短すぎるで行われた場合、は、データが早すぎる有効期限、キャッシュの利点も制限されます。 行われた場合、長すぎますが古くなるデータのリスクです。 そのため、有効期限は、データを使用するアプリのアクセスのパターンに一致する必要があります。

キャッシュされたデータの有効期限が切れるし、キャッシュから削除する必要がありますが、アプリを取得する必要があります、元のデータからのデータは格納し、キャッシュに配置します。

データが長すぎる期間を継続できる場合、キャッシュがいっぱいにこともできます。 そのため、要求をキャッシュに新しい項目を追加する必要がありますと呼ばれるプロセスでいくつかの項目を削除*削除*します。 キャッシュ サービスは、通常最も最近使用単位でデータを削除します。 最近の使用、および最初の 1 つ目アウトなど、他の削除ポリシーがあります。詳細については、[キャッシュのガイダンス](/azure/architecture/best-practices/caching/)を参照してください。

<a name="caching_images" />

### <a name="caching-images"></a>イメージのキャッシュ

EShopOnContainers のモバイル アプリでは、キャッシュから利点を得られるリモート製品画像を消費します。 これらのイメージが表示されます、 [ `Image` ](xref:Xamarin.Forms.Image)コントロール、および`CachedImage`コントロールによって提供される、 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/)ライブラリ。

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image)コントロールはダウンロードしたイメージのキャッシュをサポートします。 キャッシュは既定で有効になっているし、24 時間のローカルでイメージを格納します。 さらに、有効期限を構成できます、 [ `CacheValidity` ](xref:Xamarin.Forms.UriImageSource.CacheValidity)プロパティ。 詳細については、[イメージ キャッシュのダウンロード](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)を参照してください。

FFImageLoading の`CachedImage`コントロールは、Xamarin.Forms の置換[ `Image` ](xref:Xamarin.Forms.Image)補助機能を有効にする追加のプロパティを提供するコントロール。 この機能は、の間では、コントロールは、エラーをサポートしていると、画像のプレース ホルダーを読み込み中に、構成可能なキャッシュを提供します。 次のコード例は、eShopOnContainers のモバイル アプリでの使用方法を示しています、`CachedImage`を制御、`ProductTemplate`をで使用されるデータ テンプレートは、 [ `ListView` ](xref:Xamarin.Forms.ListView)を制御、 `CatalogView`:

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

`CachedImage`コントロール セット、`LoadingPlaceholder`と`ErrorPlaceholder`プラットフォーム固有のイメージのプロパティ。 `LoadingPlaceholder`プロパティで指定されるイメージの中に表示するイメージを指定します、`Source`プロパティを取得すると、および`ErrorPlaceholder`プロパティは、イメージを取得中にエラーが発生した場合に表示されるイメージを指定します。指定された、`Source`プロパティ。

名前が示すとおり、`CachedImage`コントロールの値によって指定された時間、デバイス上のリモートのイメージのキャッシュ、`CacheDuration`プロパティ。 このプロパティの値が明示的に設定されていない、ときに、30 日間の既定値が適用されます。

## <a name="increasing-resilience"></a>回復力の向上

リモート サービスとリソースと通信するすべてのアプリは、一時的な障害に敏感である必要があります。 一時的な障害には、サービスへのネットワーク接続の喪失、サービス、またはサービスがビジー状態のときに発生したタイムアウトの一時使用不可の状態が含まれます。 これらの障害は自己修復では多くの場合と、適切な遅延後に、アクションが繰り返される場合が成功する可能性があります。

予測可能なすべての状況で十分にテストされている場合でも一過性の障害、アプリの見かけ上の品質に大きな影響を与えることができます。 リモート サービスと通信するアプリが確実に動作するためには、次のすべての操作を実行できますがある場合があります。

-   エラーを検出し、障害が一時的なする可能性があるかどうかを判断します。
-   一時的な操作の再試行回数を追跡し、障害があると判断した場合は、操作を再試行してください。
-   再試行すると、失敗した後に実行するには、各試行とアクションの間の遅延の数を指定します。 適切な再試行戦略を使用します。

この一時的な障害処理は、再試行パターンを実装するコードでのリモート サービスへのアクセス試行をすべてをラップすることによって実現できます。

### <a name="retry-pattern"></a>再試行パターン

アプリでは、リモート サービスに要求を送信するときにエラーが検出されたは、次の方法のいずれかでエラーを処理できます。

-   操作を再試行しています。 アプリは、失敗した要求をすぐに再試行でした。
-   遅延後に操作を再試行しています。 アプリは、要求を再試行する前に、適切な時間待機する必要があります。
-   操作をキャンセルしています。 アプリケーションは、操作をキャンセルし、例外を報告する必要があります。

再試行戦略は、アプリのビジネス要件に合わせて調整する必要があります。 たとえば、再試行回数を最適化し、再試行の間隔をしようとしている操作に重要ですが。 操作がユーザーの操作の一部である場合は、再試行間隔短いファイル名とユーザーの応答を待機しないようにしようとしています。 いくつか再試行のみにする必要があります。 操作がキャンセルするか、ワークフローを再開コストがかかったり、時間がかかるは、実行時間の長いワークフローの一部と場合、は、現在の試行まで待機し、複数回再試行する適切なは。

> [!NOTE]
> 回数と再試行の数が多いとの間の最小限の遅延が、積極的な再試行戦略に近い、または容量で実行しているリモート サービスが低下する可能性があります。 さらに、このような再試行戦略もに影響する可能性、アプリの応答性、失敗した操作を実行する継続的にしようとした場合。

要求に引き続き試行回数後失敗した場合は、同じリソースをさらに要求を禁止して、エラーを報告するには、アプリのことをお勧めします。 次に、一定の期間後、アプリ要求を実行できる 1 つまたは複数成功した場合に表示するリソースにします。 詳細については、[サーキット ブレーカー パターン](#circuit_breaker_pattern)を参照してください。

> [!TIP]
> 無限の再試行メカニズムを実装しません。 有限回数を使用するか、実装、[サーキット ブレーカー](/azure/architecture/patterns/circuit-breaker/)パターン、サービスが回復できるようにします。

EShopOnContainers のモバイル アプリは、再試行パターンが rest ベースの web 要求を行うときに現在実装していません。 ただし、`CachedImage`により提供される、コントロール、 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/)ライブラリは、イメージの読み込みを再試行して一時的なエラー処理をサポートしています。 画像の読み込みが失敗した場合、再試行が行われます。 試行回数がで指定された、`RetryCount`プロパティ、および再試行により指定された遅延の後に発生、`RetryDelay`プロパティ。 これらのプロパティ値に明示的が設定されていない、既定値が適用されます: 3 の場合、`RetryCount`プロパティ、および場合は 250 ミリ秒、`RetryDelay`プロパティ。 詳細については、`CachedImage`コントロールを参照してください[イメージのキャッシュ](#caching_images)します。

EShopOnContainers 参照アプリケーションでは、再試行パターンを実装します。 詳細については、再試行パターンとを結合する方法について説明を含む、`HttpClient`クラスを参照してください[.NET マイクロ サービス: コンテナー化された .NET アプリケーション アーキテクチャ](https://aka.ms/microservicesebook)します。

再試行パターンの詳細については、次を参照してください。、[再試行](/azure/architecture/patterns/retry/)パターン。

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>サーキット ブレーカー パターン

場合によっては、修正にかかる予想のイベントが原因のエラーが発生します。 このような障害の範囲は、接続の部分的な損失からサービスの完全なエラーです。 このような場合は、成功し、代わりにする必要があります、操作が失敗したことに同意し、それに応じてこのエラーを処理すると思われる操作を再試行するアプリも無意味です。

サーキット ブレーカー パターンは、エラーが解決されたかどうかを検出するためにアプリを実現しつつ、失敗する可能性がある操作を実行しようとして繰り返しからアプリを回避できます。

> [!NOTE]
> サーキット ブレーカー パターンの目的は、再試行パターンによって異なります。 再試行パターンでは、アプリが成功しているという想定で、操作を再試行します。 サーキット ブレーカー パターンは、アプリが失敗する可能性がある操作を実行することを防ぎます。

サーキット ブレーカーは、障害が発生する操作のプロキシとして機能します。 プロキシは、発生した最近のエラーの数を監視し、この情報を使用して、続行するには、または例外を直ちに戻る操作を許可するかどうかを決定する必要があります。

EShopOnContainers のモバイル アプリは現在、サーキット ブレーカー パターンを実装しません。 ただし、eShopOnContainers では。 詳細については、[.NET マイクロ サービス: コンテナー化された .NET アプリケーション アーキテクチャ](https://aka.ms/microservicesebook)を参照してください。

> [!TIP]
> 再試行、サーキット ブレーカー パターンを結合します。 アプリは、サーキット ブレーカーを介して操作を呼び出す再試行パターンを使用して、再試行、サーキット ブレーカー パターンを組み合わせることができます。 ただし、再試行ロジックはサーキット ブレーカーによって返される例外に対応して、サーキット ブレーカーは、障害が一時的でないことを示している場合に再試行を中止する必要があります。

サーキット ブレーカー パターンの詳細については、次を参照してください。、[サーキット ブレーカー](/azure/architecture/patterns/circuit-breaker/)パターン。

## <a name="summary"></a>まとめ

Web ベース ソリューションの多くの最新のリモート クライアント アプリケーションの機能を提供する、web サーバーでホストされている、web サービスを使用します。 Web サービスを公開する操作を構成して web API、およびクライアント アプリは、データや、API により公開される操作を実装する方法を知ることがなく、web API を利用できる必要があります。

近い場所には、高速のストレージに頻繁にアクセスされるデータをキャッシュすることによって、アプリのパフォーマンスを向上することができます、アプリ。 アプリでは、キャッシュ アサイド パターンでは、リード スルー キャッシュを実装できます。 このパターンは、項目がキャッシュに現在あるかどうかを判断します。 項目がない場合、キャッシュ内が、データ ストアから読み取り、キャッシュに追加します。

Web Api を通信するときにアプリは一時的な障害に敏感である必要があります。 一時的な障害には、サービスへのネットワーク接続の喪失、サービス、またはサービスがビジー状態のときに発生したタイムアウトの一時使用不可の状態が含まれます。 これらの障害は自動修正では多くの場合、し、アクションは、適切な遅延後に繰り返されますし、そのが成功する可能性があります。 そのため、アプリでは、一時的なエラー処理機構を実装するコードに web API へのアクセス試行をすべてをラップする必要があります。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
