---
title: リモート データへのアクセス
description: この章では、eShopOnContainers モバイル アプリがコンテナー化 microservices からデータにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a140560731cc68dd85c97dc5a89aedcb32abd405
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242090"
---
# <a name="accessing-remote-data"></a>リモート データへのアクセス

最新 web ベースのソリューションの多くを行うリモート クライアント アプリケーションの機能を提供する、web サーバーでホストされる web サービスを使用します。 Web サービスによって公開される操作は、web API を構成します。

クライアント アプリでは、データまたは API で公開される操作の実装方法がわからなくても、web API を利用できる必要があります。 これは、API は、一般的な標準を使用、およびクライアント アプリと web サービスの間で交換されるデータの構造にどのデータ形式が一致するクライアント アプリケーションと web サービスを有効にすることが必要です。

## <a name="introduction-to-representational-state-transfer"></a>Representational State Transfer の概要

Representational State Transfer (REST) は、hypermedia に基づく分散システムを構築するためのアーキテクチャのスタイルです。 残りの部分のモデルの主な利点は、オープン スタンダードに基づくが、モデルや、特定の実装にアクセスするクライアント アプリケーションの実装をバインドしないことです。 したがって、Microsoft ASP.NET Core MVC を使用して、REST web サービスを実装することが、任意の言語との HTTP 要求を生成し、HTTP 応答を解析するツールセットを使用してクライアント アプリを開発する可能性があります。

残りの部分のモデルでは、ナビゲーション スキームを使用して、リソースと呼ばれる、ネットワーク経由でのオブジェクトとサービスを表します。 通常、残りの部分を実装するシステムでは、これらのリソースにアクセスするのに要求を送信するのに HTTP プロトコルを使用します。 このようなシステムでは、クライアント アプリは、リソースを識別する URI とそのリソースに対して実行する操作を示す HTTP メソッド (GET、POST、PUT、または DELETE) などの形式で要求を送信します。 HTTP 要求の本文には、操作を実行するために必要なすべてのデータが含まれています。

> [!NOTE]
> 残りの部分では、ステートレスな要求モデルを定義します。 そのため、HTTP 要求は独立している必要があり、任意の順序で発生する可能性があります。

残りの部分からの応答の要求は標準 HTTP ステータス コードを使用します。 たとえば、検索または指定されたリソースの削除に失敗した要求が HTTP ステータス コード 404 (Not Found) を含んでいる応答を返す必要がありますを有効なデータを返す要求は HTTP 応答コード 200 (OK) を含める必要があります。

RESTful web API は、接続されているリソースのセットを公開し、アプリでそれらのリソースを操作し、それらの間を簡単に移動できるようにする主要な操作を提供します。 このため、一般的な RESTful web API を構成する Uri 指向ですが公開するデータと機能が提供される使用してこのデータを操作する HTTP。

HTTP 要求、および web サーバーから対応する応答メッセージにクライアント アプリが含まれているデータは、さまざまなメディアの種類と呼ばれる形式で表示する可能性があります。 クライアント アプリでは、メッセージの本文でデータを返す要求を送信するときに処理できるメディアの種類を指定できます、`Accept`要求のヘッダー。 Web サーバーは、このメディアの種類をサポートする場合を含む応答に応答できます、`Content-Type`ヘッダーをメッセージの本文内のデータの形式を指定します。 応答メッセージを解析し、メッセージの本文に結果を適切に解釈するクライアント アプリケーションの役割です。

REST の詳細については、次を参照してください。 [API の設計](/azure/architecture/best-practices/api-design/)と[API 実装](/azure/architecture/best-practices/api-implementation/)です。

## <a name="consuming-restful-apis"></a>RESTful Api の使用

EShopOnContainers モバイル アプリでは、モデル View-viewmodel (MVVM) パターンとドメイン エンティティが、アプリで使用されるパターン表しますのモデル要素を使用します。 EShopOnContainers 参照アプリケーションのコント ローラーおよびリポジトリ クラスは、そのまま使用し、これらのモデル オブジェクトの多くを返します。 したがって、これらは、モバイル アプリとコンテナー化 microservices 間で渡されるすべてのデータを保持するデータ転送オブジェクト (Dto) として使用されます。 Dtos の使用を使用してデータを渡すし、web サービスからデータを受信する主な利点は、ことを単一のリモート呼び出しでより多くのデータを送信することで、アプリ減らせるできるようにする必要があるリモートの呼び出しの数です。

### <a name="making-web-requests"></a>Web 要求を作成する

EShopOnContainers モバイル アプリの使用、`HttpClient`メディアの種類として使用されている JSON での HTTP 経由で要求を作成するクラス。 このクラスは、HTTP 要求を非同期的に送信するための機能を提供し、リソースを識別する URI から HTTP 応答を受信します。 `HttpResponseMessage`クラスは、HTTP 要求が行われた後に、REST API から受信した HTTP 応答メッセージを表します。 応答、状態コード、ヘッダー、およびの本文を含むに関する情報が含まれています。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`です。 いずれかを使用してコンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式によって異なります。

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>GET 要求を行う

`CatalogService`カタログ マイクロ サービスからデータの取得プロセスを管理するクラスを使用します。 `RegisterDependencies`メソッドで、`ViewModelLocator`クラス、`CatalogService`に対して型マッピングとクラスが登録されている、 `ICatalogService` Autofac 依存性の注入コンテナーを持つ型。 その後のインスタンス、`CatalogViewModel`クラスを作成すると、そのコンス トラクターを受け入れる、`ICatalogService`入力、いる Autofac が解決するのインスタンスを返す、`CatalogService`クラスです。 依存関係の挿入の詳細については、次を参照してください。[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)です。

図 10-1 で表示するためのカタログ マイクロ サービスからカタログ データを読み取るクラスの相互作用を示しています、`CatalogView`です。

[![](accessing-remote-data-images/catalogdata.png "カタログ マイクロ サービスからデータを取得する")](accessing-remote-data-images/catalogdata-large.png#lightbox "カタログ マイクロ サービスからデータを取得します。")

**図 10-1**: カタログ マイクロ サービスからデータを取得します。

ときに、`CatalogView`への移動が、`OnInitialize`メソッドで、`CatalogViewModel`クラスが呼び出されます。 次のコード例に示すように、このメソッドがカタログ マイクロ サービスからカタログ データを取得します。

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

このメソッドは、`GetCatalogAsync`のメソッド、`CatalogService`に挿入されたインスタンス、 `CatalogViewModel` Autofac でします。 次のコード例は、`GetCatalogAsync`メソッド。

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

このメソッドは、要求に送信するリソースを識別する URI をビルドし、使用、`RequestProvider`に結果を返す前に、リソースに対して GET HTTP メソッドを呼び出すためのクラス、`CatalogViewModel`です。 `RequestProvider`クラスには、リソース、そのリソースに対して実行する操作を示す HTTP メソッドを識別する URI の形式で要求を送信する機能が含まれ、操作を実行するために必要なすべてのデータを格納している本文。 方法に関する情報の`RequestProvider`クラスが挿入、`CatalogService class`を参照してください[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)です。

次のコード例は、`GetAsync`メソッドで、`RequestProvider`クラス。

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

このメソッドは、`CreateHttpClient`のインスタンスを返すメソッド、`HttpClient`適切なヘッダー セットを持つクラス。 格納されている応答に、URI で識別されるリソースへの非同期 GET 要求を送信し、`HttpResponseMessage`インスタンス。 `HandleResponse`メソッドが呼び出され、応答には、成功 HTTP ステータス コードにはが含まれていない場合に例外をスローします。 JSON から変換された文字列としての応答が読み込まれる、`CatalogRoot`オブジェクトに返されると、`CatalogService`です。

`CreateHttpClient`メソッドに次のコード例に示します。

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

このメソッドは、の新しいインスタンスを作成、`HttpClient`クラス、およびセット、`Accept`によって行われたすべての要求のヘッダー、`HttpClient`インスタンスを`application/json`JSON を使用して書式設定するすべての応答のコンテンツが期待していることを示します。 その後、アクセス トークンがへの引数として渡された場合、`CreateHttpClient`メソッドに追加される、`Authorization`によって行われたすべての要求のヘッダー、`HttpClient`文字列で始まるインスタンス`Bearer`です。 承認の詳細については、次を参照してください。[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)です。

ときに、`GetAsync`メソッドで、`RequestProvider`クラス`HttpClient.GetAsync`、`Items`メソッドで、 `CatalogController` Catalog.API プロジェクト内のクラスが呼び出され、次のコード例で示した。

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

JSON のコレクションが書式設定し、このメソッドは、ある EntityFramework を使用して SQL データベースからカタログ データを取得し、成功 HTTP ステータス コードを含む応答メッセージとして返します`CatalogItem`インスタンス。

#### <a name="making-a-post-request"></a>POST 要求を行う

`BasketService`データの取得を管理およびバスケット マイクロ サービスとプロセスを更新するクラスを使用します。 `RegisterDependencies`メソッドで、`ViewModelLocator`クラス、`BasketService`に対して型マッピングとクラスが登録されている、 `IBasketService` Autofac 依存性の注入コンテナーを持つ型。 その後のインスタンス、`BasketViewModel`クラスを作成すると、そのコンス トラクターを受け入れる、`IBasketService`入力、いる Autofac が解決するのインスタンスを返す、`BasketService `クラスです。 依存関係の挿入の詳細については、次を参照してください。[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)です。

図 10-2 に表示されるバスケット データを送信するクラスの相互作用を示しています、 `BasketView`、バスケット マイクロ サービスをします。

[![](accessing-remote-data-images/basketdata.png "バスケット マイクロ サービスへのデータ送信")](accessing-remote-data-images/basketdata-large.png#lightbox "データ バスケット マイクロ サービスを送信します。")

**図 10-2**: バスケット マイクロ サービスにデータを送信します。

項目が、買い物かごに追加されると、`ReCalculateTotalAsync`メソッドで、`BasketViewModel`クラスが呼び出されます。 このメソッドは、バスケット内の項目の合計値を更新し、次のコード例に示すように、バスケット データをバスケット マイクロ サービスを送信します。

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

このメソッドは、`UpdateBasketAsync`のメソッド、`BasketService`に挿入されたインスタンス、 `BasketViewModel` Autofac でします。 メソッドを示します、`UpdateBasketAsync`メソッド。

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

このメソッドは、要求に送信するリソースを識別する URI をビルドし、使用、`RequestProvider`に結果を返す前に、リソースに対して POST HTTP メソッドを呼び出すためのクラス、`BasketViewModel`です。 バスケット マイクロ サービスへの要求を承認するために、アクセス トークンが認証プロセス中に IdentityServer から取得したことが必要です。 承認の詳細については、次を参照してください。[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)です。

次のコード例では、いずれかを示します、`PostAsync`内のメソッド、`RequestProvider`クラス。

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

このメソッドは、`CreateHttpClient`のインスタンスを返すメソッド、`HttpClient`適切なヘッダー セットを持つクラス。 JSON 形式、およびに格納されている応答で送信するバスケットのシリアル化されたデータ URI で識別されるリソースへの非同期の POST 要求を送信し、`HttpResponseMessage`インスタンス。 `HandleResponse`メソッドが呼び出され、応答には、成功 HTTP ステータス コードにはが含まれていない場合に例外をスローします。 JSON から変換された文字列としての応答の読み取り、`CustomerBasket`オブジェクトに返されると、`BasketService`です。 詳細については、`CreateHttpClient`メソッドを参照してください[取得要求を行う](#making_a_get_request)です。

ときに、`PostAsync`メソッドで、`RequestProvider`クラス`HttpClient.PostAsync`、`Post`メソッドで、 `BasketController` Basket.API プロジェクト内のクラスが呼び出され、次のコード例で示した。

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

このメソッドのインスタンスを使用して、 `RedisBasketRepository` Redis cache へバスケット データを保持するクラスし、の HTTP ステータス コード、成功、および JSON を含む応答メッセージの形式を返します、`CustomerBasket`インスタンス。

#### <a name="making-a-delete-request"></a>DELETE 要求を行う

図 10-3 バスケット マイクロ サービスからバスケット データを削除するクラスの相互作用を示しています、`CheckoutView`です。

![](accessing-remote-data-images/checkoutdata.png "バスケット マイクロ サービスからデータを削除しても")

**図 10-3**: バスケット マイクロ サービスからデータを削除します。

チェック アウト プロセスが呼び出されたときに、`CheckoutAsync`メソッドで、`CheckoutViewModel`クラスが呼び出されます。 このメソッドは、次のコード例に示すように、買い物かごの中をクリアする前に、新しい注文を作成します。

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

このメソッドは、`ClearBasketAsync`のメソッド、`BasketService`に挿入されたインスタンス、 `CheckoutViewModel` Autofac でします。 メソッドを示します、`ClearBasketAsync`メソッド。

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

このメソッドは、要求が送信されるリソースを識別する URI をビルドし、使用、`RequestProvider`リソースに対して DELETE HTTP メソッドを呼び出すためのクラスです。 バスケット マイクロ サービスへの要求を承認するために、アクセス トークンが認証プロセス中に IdentityServer から取得したことが必要です。 承認の詳細については、次を参照してください。[承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)です。

次のコード例は、`DeleteAsync`メソッドで、`RequestProvider`クラス。

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

このメソッドは、`CreateHttpClient`のインスタンスを返すメソッド、`HttpClient`適切なヘッダー セットを持つクラス。 URI で識別されるリソースへの非同期削除要求を送信します。 詳細については、`CreateHttpClient`メソッドを参照してください[取得要求を行う](#making_a_get_request)です。

ときに、`DeleteAsync`メソッドで、`RequestProvider`クラス`HttpClient.DeleteAsync`、`Delete`メソッドで、 `BasketController` Basket.API プロジェクト内のクラスが呼び出され、次のコード例で示した。

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

このメソッドのインスタンスを使用して、`RedisBasketRepository`バスケット データを Redis キャッシュから削除するクラス。

## <a name="caching-data"></a>キャッシュされたデータ

閉じる配置されている高速記憶域を頻繁にアクセスされるデータをキャッシュすることによって、アプリのパフォーマンスを向上できるアプリです。 高速のストレージは、元のソースよりもアプリに近い場所にあるかどうかは、応答を大幅に向上できるキャッシュ データを取得するときにタイムアウトします。

キャッシュの最も一般的な形式は、リード スルー キャッシュでキャッシュを参照することで、アプリがデータを取得する場所です。 データがない場合、キャッシュ内がデータ ストアから取得されたあり、キャッシュに追加します。 アプリは、リード スルー キャッシュ キャッシュア サイド パターンを実装できます。 このパターンでは、アイテムが現在キャッシュ内にするかどうかを判断します。 項目がない場合、キャッシュ内がデータ ストアから読み取り、キャッシュに追加します。 詳細については、次を参照してください。、[キャッシュ アサイド](/azure/architecture/patterns/cache-aside/)パターン。

> [!TIP]
> 頻繁に読み取ることはめったに変更データをキャッシュします。 このデータは、必要に応じて、初めてアプリによって取得されるキャッシュに追加できます。 つまり、アプリがデータ ストアからデータを 1 回だけをフェッチする必要があるキャッシュを使用してその後でアクセスを満たすことができます。

分散アプリケーション、eShopOnContainers 参照アプリケーションなどでは、次のキャッシュの一方または両方。

-   共有キャッシュ、複数のプロセスまたはコンピューターでアクセスできます。
-   アプリを実行しているデバイス上のデータのローカルでの保持プライベート キャッシュします。

EShopOnContainers モバイル アプリでは、プライベート キャッシュを使用し、アプリのインスタンスを実行しているデバイス上のデータのローカルでの保持を使用します。 EShopOnContainers の参照をアプリケーションで使用されるキャッシュについては、次を参照してください。 [.NET Microservices: .NET アプリケーションのコンテナーをアーキテクチャ](https://aka.ms/microservicesebook)です。

> [!TIP]
> キャッシュのいつでも非表示になりでしたの一時的なデータ ストアとして考えます。 元のデータ ストアだけでなく、キャッシュ データが維持されることを確認します。 キャッシュが使用できなくなった場合、データの損失の可能性は、最小化されます。

### <a name="managing-data-expiration"></a>データの有効期限の管理

キャッシュされたデータは常に一貫している元のデータが現実的ではありません。 これはキャッシュされた後、キャッシュされたデータを古くなることが原因と、元のデータ ストア内のデータを変更できます。 そのため、アプリ必要があります、キャッシュ内のデータが、可能な限り最新の状態であることを確認するための戦略を実装するもを検出して、キャッシュ内のデータが古くなるときに発生する状況に対処します。 大部分のキャッシュ メカニズムは、キャッシュ データを期限切れを構成することを有効にし、そのためデータが古くなるな期間を短縮します。

> [!TIP]
> 既定の有効期限を設定、キャッシュの構成時の時間します。 多くのキャッシュは、有効期限が切れると、データを無効にし、アクセスできない場合は、指定した期間がキャッシュから削除を実装します。 ただし、注意が必要、有効期間を選択するときにします。 これが行われる短すぎる場合データ有効期限が切れるが早すぎるとキャッシュの利点が縮小されます。 これが行われた場合長すぎる、なって失効データのリスクです。 そのため、有効期限は、データを使用するアプリのアクセスのパターンに一致する必要があります。

キャッシュされたデータの有効期限が切れるし、キャッシュから削除するか、アプリを取得する必要があります、元のデータからのデータは格納し、キャッシュに配置します。

データの長期間の残りが許可された場合、キャッシュがいっぱいにこともできます。 そのため、要求をキャッシュに新しい項目を追加する必要がありますと呼ばれるプロセスで一部の項目を削除する*削除*です。 キャッシュ サービスは、通常最も使わごとにデータを削除します。 ただし、他の削除ポリシーを含むほとんどの最近使用した、および最初先出しです。詳細については、次を参照してください。[キャッシュ ガイダンス](/azure/architecture/best-practices/caching/)です。

<a name="caching_images" />

### <a name="caching-images"></a>イメージのキャッシュ

EShopOnContainers モバイル アプリでは、キャッシュされてから利点を得られるリモート製品の画像を使用します。 これらのイメージが表示されます、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)コントロール、および`CachedImage`により提供されるコントロール、 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/)ライブラリです。

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)コントロールはダウンロードしたイメージのキャッシュをサポートします。 キャッシュが既定では、有効にし、24 時間、ローカルにイメージを格納します。 さらに、有効期限を構成できます、 [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/)プロパティです。 詳細については、次を参照してください。[イメージ キャッシュのダウンロード](~/xamarin-forms/user-interface/images.md#Image_Caching)です。

FFImageLoading の`CachedImage`コントロールは、Xamarin.Forms の代わりに[ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)補助機能を有効にする追加のプロパティを提供するコントロール。 この機能の間では、コントロールは、エラーをサポートして、イメージ プレース ホルダーを読み込み中に、構成可能なキャッシュを提供します。 次のコード例は、eShopOnContainers モバイル アプリでの使用方法を示しています、`CachedImage`内の制御、 `ProductTemplate`、どのによって使用されるデータ テンプレート、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)で制御、 `CatalogView`:

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

`CachedImage`コントロール セット、`LoadingPlaceholder`と`ErrorPlaceholder`プラットフォーム固有のイメージのプロパティです。 `LoadingPlaceholder`プロパティで指定されたイメージの中に表示されるイメージを指定します、`Source`プロパティを取得すると、および`ErrorPlaceholder`プロパティは、画像を取得しようとするときにエラーが発生した場合に表示されるイメージを指定します。によって指定された、`Source`プロパティです。

名前が示すように、`CachedImage`コントロールでは、デバイス上のリモートのイメージをキャッシュの値によって指定された時間、`CacheDuration`プロパティです。 このプロパティの値が明示的に設定されていない、30 日間の既定値が適用されます。

## <a name="increasing-resilience"></a>回復力を増やす

リモート サービスとリソースと通信するすべてのアプリは、一時的なエラーを区別する必要があります。 一時的なエラーには、一時的にサービスへのネットワーク接続の切断、一時が利用できないサービス、またはサービスがビジー状態であるときに発生するためのタイムアウトが含まれます。 これらのエラーと自己修復は多くの場合、および適切な遅延後に、アクションが繰り返される場合が成功する可能性があります。

すべての予測可能な環境で十分にテストされている場合でも一時的なエラーはアプリの見かけ上の品質に大きな影響を与えることができます。 リモート サービスと通信するアプリケーションが確実に動作するためには、次のすべての操作を実行できますがある場合があります。

-   エラーを検出し、一時的なする可能性がありますが、エラーが発生します。
-   エラーが一時的であるしの操作が再試行された回数を追跡する可能性があると判断した場合は、操作を再試行してください。
-   再試行回数、失敗した後に実行するには、各試行と操作の間の遅延の数を指定する、適切な再試行戦略を使用します。

この一時的なエラー処理は、再試行パターンを実装するコードでのリモート サービスにアクセスするすべての試みをラップすることによって実現できます。

### <a name="retry-pattern"></a>パターンを再試行してください。

アプリでは、リモート サービスに要求を送信するときに問題が検出されたは、次の方法のいずれかで障害を処理できます。

-   操作を再試行しています。 アプリでは、すぐに失敗した要求を再試行可能性があります。
-   遅延後に操作を再試行しています。 アプリは、要求を再試行する前に、適切な時間待機する必要があります。
-   操作をキャンセルしています。 アプリケーションは、操作をキャンセルし、例外を報告する必要があります。

再試行戦略は、アプリのビジネス要件に合わせて調整する必要があります。 たとえば、することが重要再試行回数を最適化し、間隔しようとしている操作を再試行してください。 操作がユーザーの操作の一部である場合は、再試行の間隔は短いファイル名とユーザーの応答を待機しないようにしようとしました。 いくつか再試行のみにする必要があります。 場合、操作を実行時間の長いワークフローの一部をキャンセルするか、ワークフローの再起動コストがかかるか時間がかかる場合は、ここは、しばらくの間の試行に対応し、複数回再試行します。

> [!NOTE]
> 回数と再試行の数が多い間の最小限の遅延で、積極的な再試行戦略には、閉じるまたは能力で実行しているリモート サービスが低下する可能性があります。 さらに、このような再試行戦略もに影響する可能性、アプリの応答性が継続的に障害が発生した操作を実行しようとしている場合。

試行回数後の要求がまだ失敗する場合は、アプリをさらに要求が同じリソースに送られることを防ぐために、エラーを報告することをお勧めします。 、一定の期間の後にアプリことができます 1 つまたは複数の要求が正常に実行しているかどうかに表示するリソースにします。 詳細については、次を参照してください。[遮断器パターン](#circuit_breaker_pattern)です。

> [!TIP]
> 無限再試行メカニズムを実装することはありません。 再試行の有限数を使用するか、実装、[遮断器](/azure/architecture/patterns/circuit-breaker/)を回復するサービスを許可するパターン。

EShopOnContainers モバイル アプリは、再試行パターンが RESTful web 要求を行うときに現在実装していません。 ただし、`CachedImage`により提供される、コントロール、 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/)ライブラリは、イメージの読み込みの再試行によって一時的なエラー処理をサポートしています。 画像の読み込みが失敗した場合、さらに実行されます。 試行回数が指定された、`RetryCount`プロパティ、および再試行で指定された遅延後に発生、`RetryDelay`プロパティです。 これらのプロパティ値は明示的に設定されている場合、その既定値は値が適用されます: 3 の`RetryCount`プロパティ、およびの 250 ms、`RetryDelay`プロパティです。 詳細については、`CachedImage`を制御しを参照してください[キャッシュ イメージ](#caching_images)です。

EShopOnContainers の参照をアプリケーションでは、再試行パターンを実装します。 詳細についてなどの再試行パターンと結合する方法については、`HttpClient`クラスを参照してください[.NET Microservices: .NET アプリケーションのコンテナーをアーキテクチャ](https://aka.ms/microservicesebook)です。

再試行パターンの詳細については、次を参照してください。、[再試行](/azure/architecture/patterns/retry/)パターン。

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>遮断器のパターン

場合によっては、予想される長くかかるイベントを解決するためエラーが発生します。 これらの障害の範囲は、接続の部分的な切断からサービスの完全な障害です。 このような場合は、アプリが成功したと代わりにする必要があります、操作が失敗したことに同意し、それに応じてこのエラーを処理する可能性がある操作をやり直してくださいにも意味がありません。

遮断器パターンすると、アプリが失敗しても、また、エラーが解決されたかどうかを検出するためにアプリを有効にする可能性がある操作を実行しようとして繰り返し防止できます。

> [!NOTE]
> 遮断器のパターンの目的は、再試行パターンによって異なります。 再試行パターンでは、アプリを成功を見込んで内の操作を再試行してください。 遮断器のパターンでは、アプリが失敗する可能性がある操作を実行することを防ぎます。

遮断器は、操作が失敗するためのプロキシとして機能します。 プロキシを発生した最近のエラーの数を監視し、この情報を使用して、続行するには、例外をすぐに返すと操作を許可するかどうかを決定します。

EShopOnContainers モバイル アプリは現在、遮断器パターンを実装しません。 ただし、eShopOnContainers がします。 詳細については、次を参照してください。 [.NET Microservices: .NET アプリケーションのコンテナーをアーキテクチャ](https://aka.ms/microservicesebook)です。

> [!TIP]
> 再試行と遮断器のパターンを結合します。 アプリは、遮断器で操作を再試行パターンを使用して、再試行および遮断器パターンを組み合わせることができます。 ただし、再試行ロジックは、遮断器によって返されるすべての例外に小文字を区別して、遮断器は、エラーが一時的なものではないことを示している場合の再試行回数を破棄する必要があります。

遮断器のパターンの詳細については、次を参照してください。、[遮断器](/azure/architecture/patterns/circuit-breaker/)パターン。

## <a name="summary"></a>まとめ

最新 web ベースのソリューションの多くを行うリモート クライアント アプリケーションの機能を提供する、web サーバーでホストされる web サービスを使用します。 Web サービスによって公開される操作を構成して、web API、およびクライアント アプリは、データまたは API で公開される操作の実装方法がわからなくても、web API を利用できる必要があります。

閉じる配置されている高速記憶域を頻繁にアクセスされるデータをキャッシュすることによって、アプリのパフォーマンスを向上できるアプリです。 アプリは、リード スルー キャッシュ キャッシュア サイド パターンを実装できます。 このパターンでは、アイテムが現在キャッシュ内にするかどうかを判断します。 項目がない場合、キャッシュ内がデータ ストアから読み取り、キャッシュに追加します。

Web Api と通信している場合、アプリが一時的なエラーに機密性の高いする必要があります。 一時的なエラーには、一時的にサービスへのネットワーク接続の切断、一時が利用できないサービス、またはサービスがビジー状態であるときに発生するためのタイムアウトが含まれます。 これらのエラーと自己修復は多くの場合とで適切な遅延後に、アクションが繰り返される場合が考えられますが成功します。 そのため、アプリでは、一時的なエラー処理メカニズムを実装するコードでは、web API へのアクセス試行をすべてをラップする必要があります。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
