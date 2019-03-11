---
title: Web サービスの概要
description: このガイドでは、別の web サービス テクノロジを使用する方法を示します。 サービスの REST、SOAP サービス、および Windows Communication Foundation サービスとの通信について書かれています。
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
---

# <a name="introduction-to-web-services"></a>Web サービスの概要

_このガイドでは、別の web サービス テクノロジを使用する方法を示します。サービスの REST、SOAP サービス、および Windows Communication Foundation サービスとの通信について書かれています。_

正常に機能は多くのモバイル アプリケーションは、クラウドに依存し、一般的なシナリオは、モバイル アプリケーションに web サービスを統合するためです。 Xamarin プラットフォームは別の web サービス テクノロジの使用をサポートし、RESTful、ASMX、および Windows Communication Foundation (WCF) サービスを使用するための組み込みとサード パーティのサポートが含まれています。

Xamarin.Forms を使用してお客様の場合は、それぞれでこれらのテクノロジを使用して完全な例は、 [Xamarin.Forms Web Services](~/xamarin-forms/data-cloud/index.md)ドキュメント。

> [!IMPORTANT]
> Ios 9 では、App Transport Security (ATS) は、機密情報の偶発的漏えいを防ぐをセキュリティで保護された接続 (アプリのバック エンド サーバーの場合) などのインターネット リソースと、アプリの間に適用されます。
>  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。

できますオプトアウトする ATS の場合、使用することはできません、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)します。

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

さまざまなライブラリと REST サービスを利用するために使用できるクラスがあるし、次のサブセクションでは、それらについて説明します。 REST サービスの使用に関する詳細については、次を参照してください。 [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)します。

### <a name="httpclient"></a>HttpClient

[Microsoft HTTP Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http)提供、`HttpClient`クラスは、HTTP 経由で要求を送受信するために使用します。 HTTP 要求を送信し、URI で識別されるリソースから HTTP 応答を受信機能を提供します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)します。

`HttpResponseMessage`クラスは、HTTP 要求が行われた後に、web サービスから受信した HTTP 応答メッセージを表します。 応答にステータス コード、ヘッダー、本文などに関する情報が含まれています。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`します。 いずれかを使用して、コンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式に応じて、します。

詳細については、`HttpClient`クラスを参照してください[HTTPClient オブジェクトを作成する](~/xamarin-forms/data-cloud/consuming/rest.md)します。

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Web サービスを呼び出す`HTTPWebRequest`が含まれます。

-  特定の URI に対して要求のインスタンスを作成しています。
-  要求インスタンスのさまざまな HTTP プロパティを設定します。
-  取得する、`HttpWebResponse`要求からします。
-  応答からデータを読み取っています。

次のコードが、米国からデータを取得する例。National ライブラリの医学 web サービス:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
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

上記の例では、作成、 `HttpWebRequest` JSON として書式設定されたデータが返されます。 データが返されます、 `HttpWebResponse`、元となる、`StreamReader`データの読み取りに取得できます。

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

REST サービスを別のアプローチを使用して、 [RestSharp](http://restsharp.org/)ライブラリ。 RestSharp 未加工の文字列コンテンツ、または、逆シリアル化されたとして結果を取得するためのサポートなど、HTTP 要求をカプセル化C#オブジェクト。 たとえば、次のコードは要求を米国書式指定文字列を国のライブラリの医学 web サービス、および JSON として結果を取得します。

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` 未加工の JSON 文字列を取得するメソッドは、`RestSharp.RestResponse.Content`プロパティに変換し、C#オブジェクト。 Web サービスから返されるデータを逆シリアル化は、この記事の後半で説明します。

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

クラス ライブラリ (BCL) など、Mono ベースで使用できるクラスだけでなく`HttpWebRequest`、およびサード パーティ製C#ライブラリ、RestSharp などのプラットフォーム固有のクラスが web サービスを使用するために使用できるもします。 たとえば、iOS で、`NSUrlConnection`と`NSMutableUrlRequest`クラスを使用できます。

次のコード例は、米国を呼び出す方法を示しています。IOS クラスを使用して各国語のライブラリの医学 web サービス:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
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

Web サービスを使用するためのプラットフォーム固有のクラスをネイティブ コードを移植する場所のシナリオに限定は一般に、C#します。 可能であれば、web サービスへのアクセス コード ポータブルでなければなりませんクロス プラットフォームの共有ができるようにします。

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Web サービスを呼び出すためのもう 1 つのオプションは、[サービス スタック](http://www.servicestack.net/)ライブラリ。 次のコードがサービスのスタックを使用する方法を示しますたとえば、`IServiceClient.GetAsync`サービス要求を発行するメソッド。

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
> XML または JSON 標準に準拠していないを使用する重要な場合があります ServiceStack と RestSharp 簡単に呼び出すし、消費などのツールは、サービスを REST、中に_DataContract_シリアル化規則。 必要に応じて、要求を呼び出すし、以下で説明する ServiceStack.Text ライブラリを使用して明示的に適切なシリアル化を処理します。


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>RESTful データの使用

通常、RESTful web サービスは、クライアントにデータを返す JSON メッセージを使用します。 JSON は、テキスト ベースのデータ交換形式が生成されますが、ペイロードを最適化する狭い帯域幅の要件の結果データを送信するときにします。 このセクションでは、JSON やプレーンから古い XML (POX) での RESTful の応答を使用するためのメカニズムが検査されます。

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Xamarin プラットフォームは、JSON のサポートをすぐに同梱されています。 使用して、 `JsonObject`、次のコード例に示すように、結果を取得できます。

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

ただし、注意すべき重要ながいる、`System.Json`ツールは、データ全体をメモリに読み込みます。

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[NewtonSoft JSON.NET ライブラリ](http://www.newtonsoft.com/json)は、広く使用されているシリアル化して、JSON メッセージを逆シリアル化ライブラリです。 次のコード例に JSON メッセージを逆シリアル化に JSON.NET を使用する方法を示しています、C#オブジェクト。

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

ServiceStack.Text は、JSON シリアル化ライブラリ ServiceStack ライブラリを使用するように設計です。 次のコード例を使用して JSON を解析する方法を示しています、 `ServiceStack.Text.JsonObject`:

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

XML ベースの REST web サービスの使用が発生した場合、XML を解析し、設定に LINQ to XML を使用できる、C#の次のコード例に示すインラインでのオブジェクトします。

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

ASMX では、簡易オブジェクト アクセス プロトコル (SOAP) を使用してメッセージを送信する web サービスをビルドする機能を提供します。 SOAP は、構築、および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスを実装するために使用するプログラミング言語について何も知る必要はありません。 のみの SOAP メッセージを送受信する方法を理解する必要があります。

SOAP メッセージは、次の要素を含む XML ドキュメントを示します。

- という名前のルート要素*エンベロープ*SOAP メッセージとして XML ドキュメントを識別します。
- 省略可能な*ヘッダー*認証データなどのアプリケーションに固有の情報を含む要素。 場合、*ヘッダー*要素が存在するは、最初の子要素があります、*エンベロープ*要素。
- 必要な*本文*SOAP メッセージの受信者のためのものを含む要素。
- 省略可能な*フォールト*をエラー メッセージを示すために使用される要素。 場合、*フォールト*要素が存在するの子要素があります、*本文*要素。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポート プロトコルで操作できます。 ただし、ASMX サービスは HTTP 経由でのみ操作できます。 Xamarin プラットフォームは、HTTP 経由で SOAP 1.1 の標準的な実装をサポートしていて、標準の ASMX サービスの構成の多くのサポートが含まれます。

### <a name="generating-a-proxy"></a>プロキシを生成します。

A*プロキシ*により、アプリケーションは、サービスに接続する ASMX サービスを使用する生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントとして公開されます。 Visual Studio for Mac または Visual Studio を使用して、プラットフォーム固有のプロジェクトを web サービスの web 参照を追加して、プロキシが構築されます。

Web サービスの URL には、リモート ホスト型のソースまたはローカル ファイル システム リソースを使用してアクセスできるかを指定できます、`file:///`例については、パスのプレフィックス。

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "Web サービスの URL には、リモート ホスト型のソースまたはファイル パスのプレフィックスを使用してアクセスできるローカル ファイル システムのリソースかを指定できます。")](images/add-webreference-dialog.png#lightbox)

これには、プロジェクトの Web またはサービス参照フォルダーで、プロキシが生成されます。 プロキシが生成されるので、コード変更しないでください。

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>手動でプロキシをプロジェクトに追加します。

互換性のあるツールを使用して生成された既存のプロキシがあれば、この出力は、プロジェクトの一部として含まれている場合に使用できます。 Visual studio for Mac では、使用、**ファイルを追加しています.** プロキシを追加するメニュー オプション。 さらに、必要があります*System.Web.Services.dll*を使用して明示的に参照できる、**の参照を追加しています.** ダイアログ ボックス。

### <a name="consuming-the-proxy"></a>プロキシの使用

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 次のコード例では、この例を示します。

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

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`Task.Factory.FromAsync`メソッド。 このメソッドを作成、`Task`を実行する、`TodoService.EndGetTodoItems`メソッドを 1 回、`TodoService.BeginGetTodoItems`メソッドが完了したらで、`null`パラメーターにデータが渡されていないことを示す、`BeginGetTodoItems`を委任します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn です。

ASMX サービスの使用に関する詳細については、次を参照してください。 [ASP.NET Web サービス (ASMX) を消費して](~/xamarin-forms/data-cloud/consuming/asmx.md)します。

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統一されたフレームワークです。 セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。

WCF では、次を含むさまざまなコントラクトのさまざまなサービスについて説明します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を形成するデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **フォールト コントラクト**– を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**– と、メッセージが各操作と対話するために必要なサービスをサポートする操作を指定します。 また、各サービスでの操作に関連付けることができる任意のカスタム エラー動作を指定します。

ASP.NET Web サービス (ASMX) と WCF では、違いがありますが、WCF が、同じ ASMX が提供する機能: HTTP 経由の SOAP メッセージをサポートしているかを理解することが重要です。

> [!IMPORTANT]
> HTTP または HTTPS を使用して経由で WCF の Xamarin プラットフォームのサポートはテキストでエンコードされた SOAP メッセージに制限されます、`BasicHttpBinding`クラス。 さらに、WCF のサポートには、プロキシを生成する Windows 環境でのみ使用できるツールの使用が必要です。

### <a name="generating-a-proxy"></a>プロキシを生成します。

A*プロキシ*により、アプリケーションは、サービスに接続する WCF サービスを使用する生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 Visual Studio 2017 で .NET Standard Library に web サービスのサービス参照を追加する Microsoft WCF Web Service Reference Provider を使用して、プロキシを構築できます。

Visual Studio 2017 での Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、次を参照してください。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)します。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>プロキシを構成します。

生成されたプロキシの構成は一般に引数を受け取る 2 つの構成 (SOAP 1.1/ASMX または WCF) によって異なりますの初期化中に:`EndpointAddress`や関連付けられたバインドについては、次の例で示すようにします。

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

### <a name="consuming-the-proxy"></a>プロキシの使用

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 次のコード例では、この例を示します。

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

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`Task.Factory.FromAsync`メソッド。 このメソッドを作成、`Task`を実行する、`TodoServiceClient.EndGetTodoItems`メソッドを 1 回、`TodoServiceClient.BeginGetTodoItems`メソッドが完了したらで、`null`パラメーターにデータが渡されていないことを示す、`BeginGetTodoItems`を委任します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn です。

WCF サービスの使用に関する詳細については、次を参照してください。 [Windows Communication Foundation (WCF) Web サービスの使用](~/xamarin-forms/data-cloud/consuming/wcf.md)します。

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>トランスポート セキュリティを使用します。

WCF サービスは、メッセージのインターセプションから保護するためのトランスポート レベルのセキュリティを使用して可能性があります。 Xamarin プラットフォームでは、SSL を使用してトランスポート レベルのセキュリティを採用しているバインディングをサポートします。 ただし、予期しない動作が証明書を検証する必要があります、スタックの場合があります。 登録することによって、検証をオーバーライドできます、`ServerCertificateValidationCallback`次のコード例に示すように、サービスを呼び出す前にデリゲートします。

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

これにより、サーバー側の証明書の検証を無視しているときに、トランスポートの暗号化が維持されます。 ただし、この方法は効果的に証明書に関連付けられている信頼の懸念事項を無視し、適切なことができない可能性があります。 詳細については、次を参照してください。[信頼されたルート Respectfully を使用して](https://www.mono-project.com/UsingTrustedRootsRespectfully)で[mono project.com](https://www.mono-project.com)します。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>クライアント資格情報のセキュリティを使用します。

WCF サービスには、サービス クライアントの資格情報を使用して認証される可能性があります。 Xamarin プラットフォームは、クライアントが SOAP メッセージ エンベロープ内の資格情報を送信できるようにする Ws-security プロトコルをサポートしていません。 ただし、Xamarin プラットフォームでは、適切なを指定して、サーバーに HTTP 基本認証資格情報を送信する機能はサポート`ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

次に、基本認証資格情報を指定できます。

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

上記の例では、「0 の種類の領域不足になりました」というメッセージが表示される場合は、向上することが 0 の種類の領域の数を追加して、`–aot “trampolines={number of trampolines}”`ビルドへの引数。 詳細については、[トラブルシューティングのヒント](~/ios/troubleshooting/troubleshooting.md#trampolines)に関するページをご覧ください。

HTTP 基本認証での詳細については、REST web サービスのコンテキストでは参照[RESTful Web サービスを認証](~/xamarin-forms/data-cloud/authentication/rest.md)します。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms での web サービス](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel メタデータ ユーティリティ ツール (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
