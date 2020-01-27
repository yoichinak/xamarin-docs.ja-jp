---
title: Web サービスの概要
description: このガイドでは、さまざまな web サービステクノロジを使用する方法について説明します。 ここでは、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信について説明します。
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: ebd7cad9ef33a44dbc7aa469bb4e866bdfea2e61
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724782"
---
# <a name="introduction-to-web-services"></a>Web サービスの概要

_このガイドでは、さまざまな web サービステクノロジを使用する方法について説明します。ここでは、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信について説明します。_

正常に機能するためには、多くのモバイルアプリケーションがクラウドに依存しているため、web サービスをモバイルアプリケーションに統合するのが一般的なシナリオです。 Xamarin プラットフォームは、さまざまな web サービステクノロジの使用をサポートしています。また、RESTful、ASMX、および Windows Communication Foundation (WCF) サービスを使用するための、組み込みのサードパーティサポートが含まれています。

Xamarin. Forms を使用しているお客様には、 [xamarin. Forms Web Services](~/xamarin-forms/data-cloud/index.yml)のドキュメントに記載されている各テクノロジを使用した完全な例があります。

> [!IMPORTANT]
> IOS 9 では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。
> ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

`HTTPS` プロトコルを使用できず、インターネットリソースに対してセキュリティで保護された通信を行うことができない場合は、無効にすることができます。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)します。

## <a name="rest"></a>REST

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャ スタイルです。 REST 要求は、web ページを取得し、サーバーにデータを送信する web ブラウザーを使用して、同じ HTTP 動詞を使用して HTTP 経由で行われます。 動詞は次のとおりです。

- **GET**– この操作は web サービスからのデータの取得に使用します。
- **POST** – この操作は、web サービスでデータの新しい項目の作成に使用されます。
- **PUT** – この操作は web サービス上のデータ項目の更新に使用します。
- **PATCH**– この操作は、項目を変更する方法に関する一連の命令を記述することで、web サービス上のデータ項目の更新を使用します。 この動作は、サンプル アプリケーションでは使用されません。
- **DELETE**– この操作を使用して、web サービス上のデータ項目を削除します。

Web サービスの REST に準拠している Api では、RESTful Api と呼ばれ、使用して定義されます。

- ベース URI。
- GET、POST、PUT、PATCH、DELETE などの HTTP メソッド。
- データは、JavaScript Object Notation (JSON) などのメディアの種類。

REST の簡潔さが、モバイル アプリケーションの web サービスにアクセスする主な方法を行うことができました。

## <a name="consuming-rest-services"></a>REST サービスの使用

REST サービスを使用するために使用できるライブラリとクラスがいくつかあります。次のサブセクションで説明します。 REST サービスの使用方法の詳細については、「 [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/web-services/rest.md)」を参照してください。

### <a name="httpclient"></a>HttpClient

[MICROSOFT Http クライアントライブラリ](https://www.nuget.org/packages/Microsoft.Net.Http)には、http 経由で要求を送受信するために使用される `HttpClient` クラスが用意されています。 これは、HTTP 要求を送信し、URI で識別されるリソースから HTTP 応答を受信するための機能を提供します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)します。

`HttpResponseMessage`クラスは、HTTP 要求が行われた後に、web サービスから受信した HTTP 応答メッセージを表します。 これには、ステータスコード、ヘッダー、本文などの応答に関する情報が含まれます。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`します。 いずれかを使用して、コンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式に応じて、します。

`HttpClient` クラスの詳細については、「 [HTTPClient オブジェクトの作成](~/xamarin-forms/data-cloud/web-services/rest.md)」を参照してください。

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

`HTTPWebRequest` による web サービスの呼び出しには次のものが含まれます。

- 特定の URI の要求インスタンスを作成しています。
- 要求インスタンスのさまざまな HTTP プロパティを設定しています。
- 要求から `HttpWebResponse` を取得しています。
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

上の例では、JSON として書式設定されたデータを返す `HttpWebRequest` を作成します。 データは `HttpWebResponse`に返されます。このデータから、データを読み取るための `StreamReader` を取得できます。

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

REST サービスを使用するためのもう1つの方法は、 [RestSharp](http://restsharp.org/)ライブラリを使用することです。 RestSharp は、生文字列の内容または逆シリアル化C#されたオブジェクトとして結果を取得するためのサポートなど、HTTP 要求をカプセル化します。 たとえば、次のコードでは、医療 web サービスの米国国立ライブラリに要求を行い、結果を JSON 形式の文字列として取得します。

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` は、`RestSharp.RestResponse.Content` プロパティから生の JSON 文字列を受け取り、それをC#オブジェクトに変換するメソッドです。 Web サービスから返されたデータの逆シリアル化については、この記事の後半で説明します。

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Mono 基本クラスライブラリ (BCL) で利用できるクラス (`HttpWebRequest`など) やサードパーティ製C#のライブラリ (RestSharp など) に加えて、プラットフォーム固有のクラスを使用して web サービスを利用することもできます。 たとえば、iOS では、`NSUrlConnection` クラスと `NSMutableUrlRequest` クラスを使用できます。

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

一般に、web サービスを使用するためのプラットフォーム固有のクラスは、ネイティブコードがにC#移植されているシナリオに限定する必要があります。 可能であれば、web サービスアクセスコードは、クロスプラットフォームで共有できるように、移植可能である必要があります。

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Web サービスを呼び出すためのもう1つのオプションは、[サービススタック](https://servicestack.net)ライブラリです。 たとえば、次のコードは、サービススタックの `IServiceClient.GetAsync` メソッドを使用してサービス要求を発行する方法を示しています。

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

<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>RESTful データの使用

通常、RESTful web サービスは、クライアントにデータを返す JSON メッセージを使用します。 JSON は、コンパクトなペイロードを生成するテキストベースのデータ交換形式であり、データの送信時に帯域幅の要件が少なくなります。 このセクションでは、JSON および Plain Old-XML (POX) で RESTful 応答を使用するメカニズムについて説明します。

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Xamarin プラットフォームには、すぐに使用できる JSON のサポートが付属しています。 `JsonObject`を使用すると、次のコード例に示すように結果を取得できます。

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

ただし、`System.Json` ツールがデータ全体をメモリに読み込むことに注意することが重要です。

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[Newtonsoft JSON.NET ライブラリ](https://www.newtonsoft.com/json)は、JSON メッセージをシリアル化および逆シリアル化するために広く使用されているライブラリです。 次のコード例は、JSON.NET を使用して JSON メッセージをC#オブジェクトに逆シリアル化する方法を示しています。

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

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack は、ServiceStack ライブラリと連携するように設計された JSON シリアル化ライブラリです。 次のコード例は、`ServiceStack.Text.JsonObject`を使用して JSON を解析する方法を示しています。

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

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

XML ベースの REST web サービスを使用する場合は、次のコード例に示すように、LINQ to XML を使用C#して xml を解析し、オブジェクトをインラインに設定できます。

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

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>ASP.NET Web サービス (ASMX)

ASMX は、Simple Object Access Protocol (SOAP) を使用してメッセージを送信する web サービスを構築する機能を提供します。 SOAP は、構築、および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。 のみの SOAP メッセージを送受信する方法を理解する必要があります。

SOAP メッセージは、次の要素を含む XML ドキュメントを示します。

- という名前のルート要素*エンベロープ*SOAP メッセージとして XML ドキュメントを識別します。
- 省略可能な*ヘッダー*認証データなどのアプリケーションに固有の情報を含む要素。 場合、*ヘッダー*要素が存在するは、最初の子要素があります、*エンベロープ*要素。
- 必要な*本文*SOAP メッセージの受信者のためのものを含む要素。
- 省略可能な*フォールト*をエラー メッセージを示すために使用される要素。 場合、*フォールト*要素が存在するの子要素があります、*本文*要素。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポート プロトコルで操作できます。 ただし、ASMX サービスは HTTP 経由でのみ操作できます。 Xamarin プラットフォームは、HTTP 経由で SOAP 1.1 の標準的な実装をサポートしていて、標準の ASMX サービスの構成の多くのサポートが含まれます。

### <a name="generating-a-proxy"></a>プロキシの生成

ASMX サービスを使用するために*プロキシ*を生成する必要があります。これにより、アプリケーションはサービスに接続できるようになります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントとして公開されます。 プロキシは、Visual Studio for Mac または Visual Studio を使用して作成され、web サービスの web 参照をプラットフォーム固有のプロジェクトに追加します。

Web サービス URL は、ホストされているリモートソースまたは `file:///` パスプレフィックスを使用してアクセスできるローカルファイルシステムリソースのいずれかです。たとえば、次のようになります。

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "The web service URL can either be a hosted remote source or local file system resource accessible via the file path prefix")](images/add-webreference-dialog.png#lightbox)

これにより、プロジェクトの Web またはサービス参照フォルダーにプロキシが生成されます。 プロキシは生成されたコードであるため、変更しないでください。

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>手動によるプロジェクトへのプロキシの追加

互換性のあるツールを使用して生成された既存のプロキシがある場合は、プロジェクトの一部としてこの出力を含めることができます。 Visual Studio for Mac で、 **[ファイルの追加...]** を使用します。 プロキシを追加するメニューオプション。 さらに、 **[参照の追加...]** を使用して明示的に参照*する必要があります*。 ダイアログで、ユーザー アカウントを追加します。

### <a name="consuming-the-proxy"></a>プロキシを使用する

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 次のコード例は、この例を示しています。

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

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`Task.Factory.FromAsync`メソッド。 このメソッドは、`TodoService.BeginGetTodoItems` メソッドの完了後に `TodoService.EndGetTodoItems` メソッドを実行する `Task` を作成し、`null` パラメーターを使用して、データが `BeginGetTodoItems` デリゲートに渡されていないことを示します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

APM の詳細については、MSDN の「[非同期プログラミングモデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL および従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)」を参照してください。

ASMX サービスの使用方法の詳細については、「 [ASP.NET Web サービス (asmx) の使用](~/xamarin-forms/data-cloud/web-services/asmx.md)」を参照してください。

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統合フレームワークです。 セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。

WCF では、次を含むさまざまなコントラクトのさまざまなサービスについて説明します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を形成するデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **フォールト コントラクト**– を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**– と、メッセージが各操作と対話するために必要なサービスをサポートする操作を指定します。 また、各サービスでの操作に関連付けることができる任意のカスタム エラー動作を指定します。

ASP.NET Web サービス (ASMX) と WCF では、違いがありますが、WCF が、同じ ASMX が提供する機能: HTTP 経由の SOAP メッセージをサポートしているかを理解することが重要です。

> [!IMPORTANT]
> WCF の Xamarin プラットフォームサポートは、`BasicHttpBinding` クラスを使用して、HTTP/HTTPS 経由でテキストエンコードされた SOAP メッセージに制限されています。 さらに、WCF のサポートには、プロキシを生成する Windows 環境でのみ使用できるツールの使用が必要です。

### <a name="generating-a-proxy"></a>プロキシの生成

A*プロキシ*により、アプリケーションは、サービスに接続する WCF サービスを使用する生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 プロキシを作成するには、Visual Studio 2017 の Microsoft WCF Web Service Reference Provider を使用して、Web サービスのサービス参照を .NET Standard ライブラリに追加します。

Visual Studio 2017 での Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、次を参照してください。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)します。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>プロキシの構成

次の例に示すように、生成されたプロキシを構成すると、通常、初期化時に2つの構成引数 (SOAP 1.1/ASMX または WCF によって異なります) が使用されます。 `EndpointAddress` と、関連付けられているバインド情報です。

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

バインディングを使用して、トランスポート、エンコーディング、およびアプリケーションとサービスが互いに通信するために必要なプロトコルの詳細を指定します。 `BasicHttpBinding`テキストでエンコードされた SOAP メッセージは、HTTP トランスポート プロトコル経由で送信されることを指定します。 パブリッシュされた複数のインスタンスがあること、WCF サービスの異なるインスタンスに接続するアプリケーションをエンドポイント アドレスの指定できます。

### <a name="consuming-the-proxy"></a>プロキシを使用する

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 次のコード例は、この例を示しています。

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

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`Task.Factory.FromAsync`メソッド。 このメソッドは、`TodoServiceClient.BeginGetTodoItems` メソッドの完了後に `TodoServiceClient.EndGetTodoItems` メソッドを実行する `Task` を作成し、`null` パラメーターを使用して、データが `BeginGetTodoItems` デリゲートに渡されていないことを示します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

APM の詳細については、MSDN の「[非同期プログラミングモデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL および従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)」を参照してください。

WCF サービスの使用の詳細については、「 [Windows Communication Foundation (wcf) Web サービスの使用](~/xamarin-forms/data-cloud/web-services/wcf.md)」を参照してください。

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>トランスポートセキュリティの使用

WCF サービスでは、トランスポートレベルのセキュリティを使用して、メッセージの傍受を防ぐことができます。 Xamarin プラットフォームは、SSL を使用してトランスポートレベルのセキュリティを使用するバインドをサポートしています。 ただし、スタックで証明書の検証が必要になる場合があり、その結果、予期しない動作が発生する可能性があります。 検証をオーバーライドするには、次のコード例に示すように、サービスを呼び出す前に `ServerCertificateValidationCallback` デリゲートを登録します。

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

これは、サーバー側の証明書の検証を無視して、トランスポートの暗号化を維持します。 ただし、この方法では、証明書に関連する信頼関係の懸念が実質的に無視され、適切ではない場合があります。 詳細については、「 [Using Trusted Root Respectfully](https://www.mono-project.com/UsingTrustedRootsRespectfully) on [mono-project.com](https://www.mono-project.com)」を参照してください。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>クライアント資格情報セキュリティの使用

WCF サービスでは、資格情報を使用したサービスクライアントの認証も必要になる場合があります。 Xamarin プラットフォームでは、クライアントが SOAP メッセージエンベロープ内で資格情報を送信できるようにする WS-SECURITY プロトコルがサポートされていません。 ただし、Xamarin プラットフォームでは、適切な `ClientCredentialType`を指定することによって、HTTP 基本認証の資格情報をサーバーに送信する機能がサポートされています。

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

次に、基本認証の資格情報を指定できます。

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

上記の例では、"trampolines の型が不足しています" というメッセージが表示された場合は、ビルドに `–aot “trampolines={number of trampolines}”` 引数を追加して、型0の trampolines の数を増やすことができます。 詳細については、[トラブルシューティングのヒント](~/ios/troubleshooting/troubleshooting.md#trampolines)に関するページをご覧ください。

HTTP 基本認証の詳細については、REST web サービスのコンテキストで、「 [RESTful Web サービスの認証](~/xamarin-forms/data-cloud/authentication/rest.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の Web サービス](~/xamarin-forms/data-cloud/index.yml)
- [ServiceModel メタデータユーティリティツール (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
