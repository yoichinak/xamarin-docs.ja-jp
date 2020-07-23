---
title: Web サービスの概要
description: このガイドでは、さまざまな web サービステクノロジを使用する方法について説明します。 ここでは、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信について説明します。
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 164c059b60c1b5b2aadb2cb348c6b5407da63928
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934707"
---
# <a name="introduction-to-web-services"></a>Web サービスの概要

_このガイドでは、さまざまな web サービステクノロジを使用する方法について説明します。ここでは、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信について説明します。_

正常に機能するためには、多くのモバイルアプリケーションがクラウドに依存しているため、web サービスをモバイルアプリケーションに統合するのが一般的なシナリオです。 Xamarin プラットフォームは、さまざまな web サービステクノロジの使用をサポートしています。また、RESTful、ASMX、および Windows Communication Foundation (WCF) サービスを使用するための、組み込みのサードパーティサポートが含まれています。

Xamarin. Forms を使用しているお客様には、 [xamarin. Forms Web Services](~/xamarin-forms/data-cloud/index.yml)のドキュメントに記載されている各テクノロジを使用した完全な例があります。

> [!IMPORTANT]
> IOS 9 では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。
> IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。

`HTTPS`インターネットリソースに対してプロトコルとセキュリティで保護された通信を使用できない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="rest"></a>REST

表現は、web サービスを構築するためのアーキテクチャスタイルです。 REST 要求は、web ブラウザーが web ページの取得やサーバーへのデータの送信に使用するのと同じ HTTP 動詞を使用して HTTP 経由で実行されます。 動詞は次のとおりです。

- **GET** –この操作は、web サービスからデータを取得するために使用されます。
- **POST** -この操作は、web サービスでデータの新しい項目を作成するために使用されます。
- **PUT** –この操作は、web サービス上のデータ項目を更新するために使用されます。
- **PATCH** –この操作は、項目の変更方法に関する一連の命令を記述することによって、web サービス上のデータ項目を更新するために使用されます。 この動詞は、サンプルアプリケーションでは使用されません。
- [**削除**] –この操作は、web サービス上のデータの項目を削除するために使用されます。

REST に準拠する Web サービス Api は RESTful Api と呼ばれ、次のものを使用して定義されます。

- ベース URI。
- GET、POST、PUT、PATCH、DELETE などの HTTP メソッド。
- データのメディアの種類 (JavaScript Object Notation (JSON) など)。

REST の簡潔さは、モバイルアプリケーションで web サービスにアクセスするための主要な方法として役立ちました。

## <a name="consuming-rest-services"></a>REST サービスの使用

REST サービスを使用するために使用できるライブラリとクラスがいくつかあります。次のサブセクションで説明します。 REST サービスの使用方法の詳細については、「 [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/web-services/rest.md)」を参照してください。

### <a name="httpclient"></a>HttpClient

[MICROSOFT Http クライアントライブラリ](https://www.nuget.org/packages/Microsoft.Net.Http)は、 `HttpClient` http を介して要求を送受信するために使用されるクラスを提供します。 これは、HTTP 要求を送信し、URI で識別されるリソースから HTTP 応答を受信するための機能を提供します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、「 [Async Support の概要](~/cross-platform/platform/async.md)」を参照してください。

クラスは、 `HttpResponseMessage` http 要求が行われた後に web サービスから受信した http 応答メッセージを表します。 これには、ステータスコード、ヘッダー、本文などの応答に関する情報が含まれます。 クラスは、 `HttpContent` HTTP 本文とコンテンツヘッダー (やなど) を表し `Content-Type` `Content-Encoding` ます。 コンテンツは、 `ReadAs` `ReadAsStringAsync` `ReadAsByteArrayAsync` データの形式に応じて、やなどの任意のメソッドを使用して読み取ることができます。

クラスの詳細については `HttpClient` 、「 [Httpclient オブジェクトの作成](~/xamarin-forms/data-cloud/web-services/rest.md)」を参照してください。

<a name="Using_HTTPWebRequest"></a>

### <a name="httpwebrequest"></a>HTTPWebRequest

で web サービスを呼び出すには、次の作業が `HTTPWebRequest` 必要です。

- 特定の URI の要求インスタンスを作成しています。
- 要求インスタンスのさまざまな HTTP プロパティを設定しています。
- `HttpWebResponse`要求からを取得しています。
- 応答からデータを読み取っています。

たとえば、次のコードは、米国国立医療 web サービスの米国国内ライブラリからデータを取得します。

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

上の例では、 `HttpWebRequest` JSON として書式設定されたデータを返すを作成します。 データはで返され `HttpWebResponse` ます。このデータを `StreamReader` 取得して、データを読み取ることができます。

<a name="Using_RESTSHARP"></a>

### <a name="restsharp"></a>RestSharp

REST サービスを使用するためのもう1つの方法は、 [RestSharp](http://restsharp.org/)ライブラリを使用することです。 RestSharp は、未加工の文字列コンテンツまたは逆シリアル化された C# オブジェクトとして結果を取得するためのサポートなど、HTTP 要求をカプセル化します。 たとえば、次のコードでは、医療 web サービスの米国国立ライブラリに要求を行い、結果を JSON 形式の文字列として取得します。

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm`は、プロパティから生の JSON 文字列を受け取り、 `RestSharp.RestResponse.Content` それを C# オブジェクトに変換するメソッドです。 Web サービスから返されたデータの逆シリアル化については、この記事の後半で説明します。

<a name="Using_NSUrlconnection"></a>

### <a name="nsurlconnection"></a>N・ Lconnection

Mono 基本クラスライブラリ (BCL) で利用できるクラス、RestSharp などのサードパーティの C# ライブラリに加えて、 `HttpWebRequest` web サービスを使用するためにプラットフォーム固有のクラスを使用することもできます。 たとえば、iOS では、 `NSUrlConnection` クラスと `NSMutableUrlRequest` クラスを使用できます。

次のコード例は、iOS クラスを使用して、医療 web サービスの米国国立ライブラリを呼び出す方法を示しています。

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

一般に、web サービスを使用するためのプラットフォーム固有のクラスは、ネイティブコードが C# に移植されるシナリオに限定する必要があります。 可能であれば、web サービスアクセスコードは、クロスプラットフォームで共有できるように、移植可能である必要があります。

<a name="Using_ServiceStack_Client"></a>

### <a name="servicestack"></a>ServiceStack

Web サービスを呼び出すためのもう1つのオプションは、[サービススタック](https://servicestack.net)ライブラリです。 たとえば、次のコードは、サービススタックのメソッドを使用してサービス要求を発行する方法を示してい `IServiceClient.GetAsync` ます。

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> ServiceStack や RestSharp などのツールを使用すると REST サービスを簡単に呼び出すことができますが、標準の_DataContract_シリアル化規則に準拠していない XML または JSON を使用するのは簡単ではないことがあります。 必要に応じて、要求を呼び出し、後で説明する ServiceStack. テキストライブラリを使用して、適切なシリアル化を明示的に処理します。

<a name="Options_for_consuming_RESTful_data"></a>

## <a name="consuming-restful-data"></a>RESTful データの使用

RESTful web サービスは通常、JSON メッセージを使用して、クライアントにデータを返します。 JSON は、コンパクトなペイロードを生成するテキストベースのデータ交換形式であり、データの送信時に帯域幅の要件が少なくなります。 このセクションでは、JSON および Plain Old-XML (POX) で RESTful 応答を使用するメカニズムについて説明します。

<a name="Using_System.JSON"></a>

### <a name="systemjson"></a>System.JS

Xamarin プラットフォームには、すぐに使用できる JSON のサポートが付属しています。 を使用する `JsonObject` と、次のコード例に示すように結果を取得できます。

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

ただし、ツールによって `System.Json` データ全体がメモリに読み込まれることに注意することが重要です。

<a name="Using_JSON.NET"></a>

### <a name="jsonnet"></a>JSON.NET

[Newtonsoft JSON.NET ライブラリ](https://www.newtonsoft.com/json)は、JSON メッセージをシリアル化および逆シリアル化するために広く使用されているライブラリです。 次のコード例は、JSON.NET を使用して JSON メッセージを C# オブジェクトに逆シリアル化する方法を示しています。

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text"></a>

### <a name="servicestacktext"></a>ServiceStack。テキスト

ServiceStack は、ServiceStack ライブラリと連携するように設計された JSON シリアル化ライブラリです。 次のコード例は、を使用して JSON を解析する方法を示してい `ServiceStack.Text.JsonObject` ます。

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq"></a>

### <a name="systemxmllinq"></a>System.Xml.Linq

XML ベースの REST web サービスを使用する場合は、次のコード例に示すように、LINQ to XML を使用して XML を解析し、C# オブジェクトをインラインに設定できます。

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx"></a>

## <a name="aspnet-web-service-asmx"></a>ASP.NET Web サービス (ASMX)

ASMX は、Simple Object Access Protocol (SOAP) を使用してメッセージを送信する web サービスを構築する機能を提供します。 SOAP は、web サービスを構築してアクセスするための、プラットフォームに依存しない、言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、サービスの実装に使用されるプラットフォーム、オブジェクトモデル、プログラミング言語について何も知る必要はありません。 SOAP メッセージを送受信する方法を理解する必要があるだけです。

SOAP メッセージは、次の要素を含む XML ドキュメントです。

- SOAP メッセージとして XML ドキュメントを識別する、 *Envelope*という名前のルート要素。
- 認証データなどのアプリケーション固有の情報を格納する*ヘッダー*要素 (省略可能)。 *Header*要素が存在する場合は、 *Envelope*要素の最初の子要素である必要があります。
- 受信者を対象とした SOAP メッセージを含む必須の*Body*要素。
- エラーメッセージを示すために使用されるオプションの*Fault*要素。 *Fault*要素が存在する場合は、 *Body*要素の子要素である必要があります。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポートプロトコルに対して動作します。 ただし、ASMX サービスは HTTP 経由でのみ動作します。 Xamarin プラットフォームは、HTTP を介した標準的な SOAP 1.1 実装をサポートしています。これには、標準的な ASMX サービス構成の多くがサポートされています。

### <a name="generating-a-proxy"></a>プロキシの生成

ASMX サービスを使用するために*プロキシ*を生成する必要があります。これにより、アプリケーションはサービスに接続できるようになります。 プロキシは、メソッドと関連付けられたサービス構成を定義するサービスメタデータを使用することによって構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントとして公開されます。 プロキシは、Visual Studio for Mac または Visual Studio を使用して作成され、web サービスの web 参照をプラットフォーム固有のプロジェクトに追加します。

Web サービス URL は、ホストされているリモートソースまたはパスプレフィックスを使用してアクセスできるローカルファイルシステムリソースのいずれか `file:///` です。たとえば、次のようになります。

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![Web サービス URL は、ホストされているリモートソースまたはファイルパスプレフィックスを使用してアクセスできるローカルファイルシステムリソースのいずれかになります。](images/add-webreference-dialog.png)](images/add-webreference-dialog.png#lightbox)

これにより、プロジェクトの Web またはサービス参照フォルダーにプロキシが生成されます。 プロキシは生成されたコードであるため、変更しないでください。

<a name="Manually_adding_a_proxy_to_a_project"></a>

#### <a name="manually-adding-a-proxy-to-a-project"></a>手動によるプロジェクトへのプロキシの追加

互換性のあるツールを使用して生成された既存のプロキシがある場合は、プロジェクトの一部としてこの出力を含めることができます。 Visual Studio for Mac で、[**ファイルの追加...** ] を使用します。 プロキシを追加するメニューオプション。 さらに、[**参照の追加**] を使用して明示的に参照する*System.Web.Services.dll*が必要です。 ] ダイアログで、ユーザー アカウントを追加します。

### <a name="consuming-the-proxy"></a>プロキシを使用する

生成されたプロキシクラスは、非同期プログラミングモデル (APM) デザインパターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンでは、非同期操作は、非同期操作を開始および終了する*Beginoperationname*と*EndOperationName*という2つのメソッドとして実装されます。

*Beginoperationname*メソッドは、非同期操作を開始し、インターフェイスを実装するオブジェクトを返し `IAsyncResult` ます。 *Beginoperationname*を呼び出した後、アプリケーションは、スレッドプールのスレッドで非同期操作を実行しながら、呼び出し元のスレッドで命令の実行を継続できます。

*Beginoperationname*の呼び出しごとに、アプリケーションも*EndOperationName*を呼び出して、操作の結果を取得する必要があります。 *EndOperationName*の戻り値は、同期 web サービスメソッドによって返される型と同じです。 次のコード例は、この例を示しています。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

タスク並列ライブラリ (TPL) を使用すると、非同期操作を同じオブジェクトにカプセル化することで、APM の begin/end メソッドのペアを使用するプロセスを簡略化でき `Task` ます。 このカプセル化は、メソッドの複数のオーバーロードによって提供され `Task.Factory.FromAsync` ます。 このメソッドは、メソッドが完了した後にメソッドを実行するを作成します。これには、 `Task` `TodoService.EndGetTodoItems` `TodoService.BeginGetTodoItems` `null` データがデリゲートに渡されていないことを示すパラメーターがあり `BeginGetTodoItems` ます。 最後に、列挙体の値は、 `TaskCreationOptions` タスクの作成と実行の既定の動作を使用することを指定します。

APM の詳細については、MSDN の「[非同期プログラミングモデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL および従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)」を参照してください。

ASMX サービスの使用方法の詳細については、「 [ASP.NET Web サービス (asmx) の使用](~/xamarin-forms/data-cloud/web-services/asmx.md)」を参照してください。

<a name="wcf"></a>

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統合フレームワークです。 これにより、開発者は、セキュリティで保護された、信頼性の高い、トランザクションで相互運用可能な分散アプリケーションを構築できます。

WCF では、次のようなさまざまなコントラクトを持つサービスが記述されています。

- **データコントラクト**–メッセージ内のコンテンツの基礎となるデータ構造を定義します。
- **メッセージコントラクト**–既存のデータコントラクトからメッセージを作成します。
- **エラーコントラクト**-カスタム SOAP エラーを指定できるようにします。
- **サービスコントラクト**: サービスがサポートする操作と、各操作との対話に必要なメッセージを指定します。 また、各サービスの操作に関連付けることができるカスタムのエラー動作も指定します。

ASP.NET ウェブサービス (ASMX) と WCF には違いがありますが、WCF では、ASMX が HTTP 経由の SOAP メッセージを提供するのと同じ機能をサポートしていることを理解しておくことが重要です。

> [!IMPORTANT]
> WCF の Xamarin プラットフォームサポートは、クラスを使用して、HTTP/HTTPS 経由でテキストエンコードされた SOAP メッセージに制限されてい `BasicHttpBinding` ます。 さらに、WCF サポートでは、プロキシを生成するために Windows 環境でのみ使用可能なツールを使用する必要があります。

### <a name="generating-a-proxy"></a>プロキシの生成

WCF サービスを使用するために*プロキシ*を生成する必要があります。これにより、アプリケーションはサービスに接続できるようになります。 プロキシは、メソッドと関連付けられたサービス構成を定義するサービスメタデータを使用することによって構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 プロキシを作成するには、Visual Studio 2017 の Microsoft WCF Web Service Reference Provider を使用して、Web サービスのサービス参照を .NET Standard ライブラリに追加します。

Visual Studio 2017 の Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりに、ServiceModel Metadata Utility Tool (svcutil.exe) を使用することもできます。 詳細については、「 [ServiceModel メタデータユーティリティツール (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)」を参照してください。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security"></a>

### <a name="configuring-the-proxy"></a>プロキシの構成

`EndpointAddress`次の例に示すように、生成されたプロキシを構成すると、一般に、初期化中に (SOAP 1.1/ASMX または WCF に応じて) 2 つの構成引数を受け取ります。

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

バインディングは、アプリケーションとサービスが相互に通信するために必要なトランスポート、エンコーディング、およびプロトコルの詳細を指定するために使用されます。 は、 `BasicHttpBinding` テキストエンコードされた SOAP メッセージが HTTP トランスポートプロトコルを介して送信されることを指定します。 エンドポイントアドレスを指定すると、アプリケーションは、複数の公開されたインスタンスがある場合に、WCF サービスの異なるインスタンスに接続できます。

### <a name="consuming-the-proxy"></a>プロキシを使用する

生成されたプロキシクラスは、非同期プログラミングモデル (APM) デザインパターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンでは、非同期操作は、非同期操作を開始および終了する*Beginoperationname*と*EndOperationName*という2つのメソッドとして実装されます。

*Beginoperationname*メソッドは、非同期操作を開始し、インターフェイスを実装するオブジェクトを返し `IAsyncResult` ます。 *Beginoperationname*を呼び出した後、アプリケーションは、スレッドプールのスレッドで非同期操作を実行しながら、呼び出し元のスレッドで命令の実行を継続できます。

*Beginoperationname*の呼び出しごとに、アプリケーションも*EndOperationName*を呼び出して、操作の結果を取得する必要があります。 *EndOperationName*の戻り値は、同期 web サービスメソッドによって返される型と同じです。 次のコード例は、この例を示しています。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

タスク並列ライブラリ (TPL) を使用すると、非同期操作を同じオブジェクトにカプセル化することで、APM の begin/end メソッドのペアを使用するプロセスを簡略化でき `Task` ます。 このカプセル化は、メソッドの複数のオーバーロードによって提供され `Task.Factory.FromAsync` ます。 このメソッドは、メソッドが完了した後にメソッドを実行するを作成します。これには、 `Task` `TodoServiceClient.EndGetTodoItems` `TodoServiceClient.BeginGetTodoItems` `null` データがデリゲートに渡されていないことを示すパラメーターがあり `BeginGetTodoItems` ます。 最後に、列挙体の値は、 `TaskCreationOptions` タスクの作成と実行の既定の動作を使用することを指定します。

APM の詳細については、MSDN の「[非同期プログラミングモデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL および従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)」を参照してください。

WCF サービスの使用の詳細については、「 [Windows Communication Foundation (wcf) Web サービスの使用](~/xamarin-forms/data-cloud/web-services/wcf.md)」を参照してください。

<a name="Calling_a_WCF_Service_with_Transport_Security"></a>

#### <a name="using-transport-security"></a>トランスポートセキュリティの使用

WCF サービスでは、トランスポートレベルのセキュリティを使用して、メッセージの傍受を防ぐことができます。 Xamarin プラットフォームは、SSL を使用してトランスポートレベルのセキュリティを使用するバインドをサポートしています。 ただし、スタックで証明書の検証が必要になる場合があり、その結果、予期しない動作が発生する可能性があります。 検証をオーバーライドするには、 `ServerCertificateValidationCallback` 次のコード例に示すように、サービスを呼び出す前にデリゲートを登録します。

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

これは、サーバー側の証明書の検証を無視して、トランスポートの暗号化を維持します。 ただし、この方法では、証明書に関連する信頼関係の懸念が実質的に無視され、適切ではない場合があります。 詳細については、「 [Using Trusted Root Respectfully](https://www.mono-project.com/UsingTrustedRootsRespectfully) on [mono-project.com](https://www.mono-project.com)」を参照してください。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security"></a>

#### <a name="using-client-credential-security"></a>クライアント資格情報セキュリティの使用

WCF サービスでは、資格情報を使用したサービスクライアントの認証も必要になる場合があります。 Xamarin プラットフォームでは、クライアントが SOAP メッセージエンベロープ内で資格情報を送信できるようにする WS-SECURITY プロトコルがサポートされていません。 ただし、Xamarin プラットフォームでは、適切なを指定することによって、HTTP 基本認証の資格情報をサーバーに送信する機能がサポートされてい `ClientCredentialType` ます。

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

次に、基本認証の資格情報を指定できます。

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

HTTP 基本認証の詳細については、REST web サービスのコンテキストで、「 [RESTful Web サービスの認証](~/xamarin-forms/data-cloud/authentication/rest.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin. Forms の Web サービス](~/xamarin-forms/data-cloud/index.yml)
- [ServiceModel メタデータユーティリティツール (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
