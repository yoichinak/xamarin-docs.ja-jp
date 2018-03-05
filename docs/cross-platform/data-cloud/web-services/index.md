---
title: "Web サービスの概要"
description: "このガイドでは、別の web サービスのテクノロジを使用する方法を示します。 説明のトピックには、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信が含まれます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 76934b56503c381b40081d2ac82a785a7bb86fa2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-web-services"></a>Web サービスの概要

_このガイドでは、別の web サービスのテクノロジを使用する方法を示します。説明のトピックには、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信が含まれます。_

正しく機能するには多くのモバイル アプリケーションは、クラウドに依存し、一般的なシナリオは、モバイル アプリケーションに web サービスを統合するためです。 Xamarin プラットフォームは、別の web サービス テクノロジの使用をサポートし、RESTful、ASMX、および Windows Communication Foundation (WCF) サービスを使用するための組み込みのおよびサード パーティ製のサポートが含まれています。

この記事では、次のトピックについて説明します。

- [REST サービス](#rest)
- [ASP.Net Web サービス (ASMX)](#asmx)
- [WCF サービス](#wcf)

Xamarin.Forms を使用してお客様の場合は、完全な例を各でこれらのテクノロジを使用して、 [Xamarin.Forms Web Services](~/xamarin-forms/data-cloud/index.md)ドキュメント。

> [!IMPORTANT]
> **Xamarin.iOS に関する注意:** iOS 9 の場合は、アプリのトランスポート セキュリティ (ATS) 強制セキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリ間で機密情報の誤った情報開示を回避します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。


ことができますオプトアウト ATS を使用することがない場合、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 アプリケーションを更新することによってこれを行う**Info.plist**ファイル。 詳細については、次を参照してください。[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)です。



<a name="rest" />

## <a name="rest"></a>REST

Representational State Transfer (REST) は、web サービスを構築するためのアーキテクチャのスタイルです。 REST 要求は web ページを取得して、サーバーにデータを送信する web ブラウザーを使用して同じ HTTP 動詞を使用して HTTP 経由で行われます。 動詞は次のとおりです。

- **取得**– この操作は web サービスからのデータの取得に使用します。
- **POST** – この操作は、web サービスでデータの新しい項目の作成に使用します。
- **PUT** – この操作は、web サービス上のデータ項目の更新に使用します。
- **修正プログラム**– この操作は、項目を変更する方法に関する一連の命令を記述することで、web サービス上のデータ項目の更新を使用します。 この動作は、サンプル アプリケーションでは使用されません。
- **削除**– この操作を使用して、web サービス上のデータ項目を削除します。

Web サービスの REST に準拠している Api は、RESTful Api と呼ばれますを使用して定義されます。

- ベース URI。
- GET、POST、PUT、PATCH、DELETE などの HTTP メソッド。
- JavaScript Object Notation (JSON) など、データのメディアの種類。

REST のシンプルさがモバイル アプリケーションで web サービスにアクセスするための主な方法を行うことができました。

## <a name="consuming-rest-services"></a>REST サービス

多数のライブラリや REST サービスを利用するために使用できるクラスがあるし、次のサブセクションでは、それらについて話し合うです。 REST サービスの使用の詳細については、次を参照してください。 [RESTful Web サービスを使用](~/xamarin-forms/data-cloud/consuming/rest.md)です。

### <a name="httpclient"></a>HttpClient

[Microsoft HTTP Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http)提供、`HttpClient`クラスは、HTTP 経由で要求を送受信するために使用します。 HTTP 要求を送信し、URI で識別されるリソースから HTTP 応答を受信の機能を提供します。 各要求は、非同期操作として送信されます。 非同期操作の詳細については、次を参照してください。[非同期サポートの概要](~/cross-platform/platform/async.md)です。

`HttpResponseMessage`クラスは、HTTP 要求が行われた後に、web サービスから受信した HTTP 応答メッセージを表します。 応答、状態コード、ヘッダー、および本文を含むに関する情報が含まれています。 `HttpContent`クラスなどを表します HTTP 本体およびコンテンツ ヘッダーは、`Content-Type`と`Content-Encoding`です。 いずれかを使用してコンテンツを読み取ることができます、`ReadAs`メソッドなど`ReadAsStringAsync`と`ReadAsByteArrayAsync`データの形式に基づいて、します。

詳細については、`HttpClient`クラスを参照してください[HTTPClient オブジェクトを作成する](~/xamarin-forms/data-cloud/consuming/rest.md)です。

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Web サービスを呼び出す`HTTPWebRequest`が含まれます。

-  特定の URI に対して要求インスタンスを作成しています。
-  要求インスタンスでさまざまな HTTP プロパティを設定します。
-  取得する、`HttpWebResponse`要求からします。
-  応答からデータを読み取っています。

次のコードが米国からデータを取得する例。各国語医学図書館の web サービス:

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

上記の例では、作成、 `HttpWebRequest` JSON として書式設定されたデータが返されます。 データが返される、 `HttpWebResponse`、元となる、`StreamReader`データの読み取りに取得できます。

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

REST サービスを別のアプローチを使用して、 [RestSharp](http://restsharp.org/)ライブラリです。 RestSharp は、未加工の文字列コンテンツ、または C# の場合、逆シリアル化されたオブジェクトとして結果を取得するためのサポートなど、HTTP 要求をカプセル化します。 たとえば、次のコードが要求を u. s.各国語の医学図書館の web サービス、および取得結果を JSON として文字列の形式。

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` 未加工の JSON 文字列を取得するメソッドが、`RestSharp.RestResponse.Content`プロパティ (C#) オブジェクトに変換します。 Web サービスから返されたデータを逆シリアル化はこの記事の後半で説明されています。

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

クラス ライブラリ (BCL) など、モノ ベースで使用できるクラスだけでなく`HttpWebRequest`、およびサード パーティ製 c# ライブラリ RestSharp などのプラットフォーム固有のクラスも使用できますの web サービスです。 たとえば、iOS で、`NSUrlConnection`と`NSMutableUrlRequest`クラスを使用できます。

次のコード例は、米国を呼び出す方法を示しています。IOS クラスを使用する各国語の医学図書館の web サービス:

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

一般に、web サービスを使用するためのプラットフォーム固有のクラスは、ネイティブ コードを c# に移植がされるシナリオに制限する必要があります。 可能であれば、web サービス アクセス コード ポータブルでなければなりません共有クロスプラット フォームができるようにします。

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Web サービスを呼び出すための別のオプションは、[サービス スタック](http://www.servicestack.net/)ライブラリです。 たとえば、次のコードはサービス スタックを使用する方法を示しています。`IServiceClient.GetAsync`サービス要求を発行する方法。

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
> **注:** XML または JSON 標準に準拠していないを使用する重要な場合があります ServiceStack と RestSharp 簡単に呼び出すし、消費などのツールは、サービスを REST、中に_DataContract_シリアル化規則。 必要に応じて、要求を起動し、次に説明 ServiceStack.Text ライブラリを使用して明示的に適切なシリアル化を処理します。


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Rest ベースのデータの消費

通常、rESTful web サービスは、クライアントにデータを返す JSON メッセージを使用します。 JSON はテキスト ベースのデータ交換形式が生成されますが、ペイロードを最適化するがよりも少ない帯域データを送信するときに発生します。 このセクションでは、rest ベースの応答を JSON およびプレーン Old XML (POX) を使用するためのメカニズムが検査されます。

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Xamarin プラットフォームは、JSON のサポートをすぐに付属します。 使用して、 `JsonObject`、次のコード例に示すように、結果を取得することができます。

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

ただし、注意すべき重要なを`System.Json`ツールは、データ全体をメモリに読み込みます。

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[NewtonSoft JSON.NET ライブラリ](http://www.newtonsoft.com/json)広く使用されているライブラリをシリアル化すると、JSON メッセージを逆シリアル化します。 次のコード例では、JSON メッセージを c# オブジェクトに逆シリアル化に JSON.NET を使用する方法を示します。

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

ServiceStack.Text は ServiceStack ライブラリを使用するように設計 JSON シリアル化ライブラリです。 次のコード例は、JSON を使用して解析する方法を示しています、 `ServiceStack.Text.JsonObject`:

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

XML ベースの REST web サービスを使用するが発生した場合は次のコード例に示すように LINQ to XML を XML を解析し、設定を c# オブジェクト、インラインで使用できます。

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

Asmx サービスは、簡易オブジェクト アクセス プロトコル (SOAP) を使用してメッセージを送信する web サービスをビルドする機能を提供します。 SOAP は、構築および web サービスにアクセスするためのプラットフォームや言語に依存しないプロトコルです。 ASMX サービスのコンシューマーは、プラットフォーム、オブジェクト モデル、またはサービスの実装に使用するプログラミング言語に関する知識は必要はありません。 のみ、SOAP メッセージを送受信する方法を理解する必要があります。

SOAP メッセージは、次の要素を含む XML ドキュメントを示します。

- という名前のルート要素*エンベロープ*SOAP メッセージとして XML ドキュメントを識別します。
- 省略可能な*ヘッダー*認証データなどのアプリケーションに固有の情報を格納する要素。 場合、*ヘッダー*要素が存在の最初の子要素があります、*エンベロープ*要素。
- 必要な*本文*受信者のためのもので、SOAP メッセージを格納する要素。
- 省略可能な*フォールト*エラー メッセージを示すために使用される要素。 場合、*フォールト*要素が存在する必要がありますの子要素、*本文*要素。

SOAP は、HTTP、SMTP、TCP、UDP など、多くのトランスポート プロトコルで動作できます。 ただし、ASMX サービスは HTTP 経由で操作のみが可能です。 Xamarin プラットフォームが over HTTP を標準 SOAP 1.1 の実装をサポートし、これには、標準の ASMX サービス構成の多くのサポートが含まれます。

### <a name="generating-a-proxy"></a>プロキシを生成します。

A*プロキシ*のため、アプリケーション、サービスへの接続を ASMX サービスを使用する生成する必要があります。 メソッドと関連付けられているサービス構成を定義するサービス メタデータを使用して、プロキシが構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントとして公開されます。 プロキシは、プラットフォーム固有のプロジェクトに web サービスの web 参照を追加する Visual Studio for Mac または Visual Studio を使用して作成されています。

Web サービスの URL には、ホストされているリモート ソースまたはローカル ファイル システム リソースを使用してアクセスできるかを指定できます、`file:///`例については、パスのプレフィックス。

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[ ![](images/add-webreference-dialog.png "Web サービスの URL には、ホストされているリモート ソースまたはファイルのパス プレフィックスを使用してアクセスできるローカル ファイル システム リソースかを指定できます。")](images/add-webreference-dialog.png)

これには、プロジェクトの Web またはサービス参照 フォルダーに、プロキシが生成されます。 プロキシが生成されるので、コードには変更できません。

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>プロジェクトにプロキシを手動で追加します。

互換性のあるツールを使用して生成された既存のプロキシがあれば、この出力は、プロジェクトの一部として含まれている場合に使用できます。 Mac 用 Visual Studio で使用して、**ファイルを追加しています.** プロキシを追加するメニュー オプション。 さらに、必要があります*System.Web.Services.dll*を使用して明示的に参照できる、**参照を追加しています.** ダイアログ ボックス。

### <a name="consuming-the-proxy"></a>プロキシの使用

生成されたプロキシ クラスでは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*、開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始しを実装するオブジェクトを返します、`IAsyncResult`インターフェイスです。 呼び出した後*BeginOperationName*、スレッド プールのスレッドで非同期操作の実行中のスレッドの呼び出しに関する命令を実行するアプリケーションを続行できます。

呼び出しごとに*BeginOperationName*、アプリケーションを呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*同期 web サービス メソッドによって返される同じ型です。 次のコード例では、この例を示します。

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

タスク並列ライブラリ (TPL) は同じで、非同期操作をカプセル化して APM 開始/終了メソッドのペアを使用するプロセスを簡略化できます`Task`オブジェクト。 このカプセル化はの複数のオーバー ロードによって提供される、`Task.Factory.FromAsync`メソッドです。 このメソッドを作成、`Task`を実行する、`TodoService.EndGetTodoItems`メソッドを 1 回、`TodoService.BeginGetTodoItems`メソッドが完了するで、`null`パラメーターにデータが渡されるないことを示す、`BeginGetTodoItems`を委任します。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx)msdn です。

ASMX サービスの使用の詳細については、次を参照してください。 [ASP.NET Web サービス (ASMX) を消費して](~/xamarin-forms/data-cloud/consuming/asmx.md)です。

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF は、サービス指向アプリケーションを構築するための Microsoft の統一フレームワークです。 セキュリティで保護された、信頼性、トランザクション、および相互運用可能な分散アプリケーションを構築する開発者ができるようにします。

WCF では、さまざまな異なるコントラクトで、次のサービスについて説明します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を成してデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **コントラクトをエラー** – を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**: サービスをサポートする操作を指定し、各操作と対話するために必要なメッセージです。 また、各サービスでの操作に関連付けることができるすべてのカスタム エラー動作を指定します。

ASP.NET Web サービス (ASMX) と、WCF の違いがあるが、WCF が、同じ機能を備えた ASMX – HTTP 経由の SOAP メッセージをサポートしていることを理解することが重要です。

一般に、Xamarin プラットフォームでは、同じクライアント側 Silverlight ランタイムに付属する WCF のサブセットをサポートしています。 これには、WCF の最も一般的なエンコード、およびプロトコルの実装が含まれます: HTTP 経由の SOAP メッセージのテキストでエンコードされたトランスポート プロトコルを使用して、`BasicHttpBinding`クラスです。 さらに、WCF のサポートには、プロキシを生成するための Windows 環境でのみ使用できるツールの使用が必要です。

Xamarin プラットフォームを使用して、WCF を使用する方法の詳細については、web でサービス、`BasicHttpBinding`クラスを参照してください[チュートリアル - WCF 操作](walkthrough-working-with-wcf.md)です。

### <a name="generating-a-proxy"></a>プロキシを生成します。

A*プロキシ*を使用した WCF サービス、により、アプリケーション、サービスへの接続を生成する必要があります。 メソッドと関連付けられているサービス構成を定義するサービス メタデータを使用して、プロキシが構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 .NET 標準ライブラリに、web サービスのサービス参照を追加する Visual Studio 2017 で Microsoft WCF Web サービス参照のプロバイダーを使用して、プロキシを構築できます。

Visual Studio 2017 で Microsoft WCF Web サービス参照のプロバイダーを使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、次を参照してください。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)です。

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

バインドを使用して、トランスポート、エンコーディング、およびアプリケーションとサービスが互いに通信するために必要なプロトコルの詳細を指定します。 `BasicHttpBinding`テキストでエンコードされた SOAP メッセージを HTTP トランスポート プロトコルを介して送信されることを指定します。 パブリッシュされた複数のインスタンスがあること、WCF サービスの異なるインスタンスに接続するアプリケーションをエンドポイント アドレスの指定できます。

### <a name="consuming-the-proxy"></a>プロキシの使用

生成されたプロキシ クラスでは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 という名前の 2 つの方法として、このパターンで非同期操作が実装されている*BeginOperationName*と*EndOperationName*、これを開始および非同期の操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始しを実装するオブジェクトを返します、`IAsyncResult`インターフェイスです。 呼び出した後*BeginOperationName*、スレッド プールのスレッドで非同期操作の実行中のスレッドの呼び出しに関する命令を実行するアプリケーションを続行できます。

呼び出しごとに*BeginOperationName*、アプリケーションを呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*同期 web サービス メソッドによって返される同じ型です。 次のコード例では、この例を示します。

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

タスク並列ライブラリ (TPL) は同じで、非同期操作をカプセル化して APM 開始/終了メソッドのペアを使用するプロセスを簡略化できます`Task`オブジェクト。 このカプセル化はの複数のオーバー ロードによって提供される、`Task.Factory.FromAsync`メソッドです。 このメソッドを作成、`Task`を実行する、`TodoServiceClient.EndGetTodoItems`メソッドを 1 回、`TodoServiceClient.BeginGetTodoItems`メソッドが完了するで、`null`パラメーターにデータが渡されるないことを示す、`BeginGetTodoItems`を委任します。 値、最後に、`TaskCreationOptions`列挙体を作成およびタスクの実行の既定の動作を使用することを指定します。

APM の詳細については、次を参照してください。[非同期プログラミング モデル](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx)msdn です。

WCF サービスの使用の詳細については、次を参照してください。 [Windows Communication Foundation (WCF) Web サービスを使用](~/xamarin-forms/data-cloud/consuming/wcf.md)です。

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>トランスポート セキュリティを使用します。

WCF サービスは、メッセージの傍受を防ぐためにトランスポート レベルのセキュリティを採用可能性があります。 Xamarin プラットフォームでは、SSL を使用してトランスポート レベルのセキュリティを採用しているバインディングをサポートします。 ただし、場合があります、スタックが予期しない動作につながる証明書を検証する必要。 登録することによって、検証をオーバーライドすることができます、`ServerCertificateValidationCallback`次のコード例に示すように、サービスを呼び出す前にデリゲートします。

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

これにより、サーバー側の証明書の検証を無視しているときに、トランスポートの暗号化が維持されます。 ただし、この方法は効果的に証明書に関連付けられている信頼の問題を無視し、適切なことができない可能性があります。 詳細については、次を参照してください。[信頼されたルート Respectfully を使用して](http://www.mono-project.com/UsingTrustedRootsRespectfully)で[モノラル project.com](http://www.mono-project.com)です。

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>クライアント資格情報のセキュリティを使用します。

WCF サービスには、サービス クライアントの資格情報を使用して認証される可能性があります。 Xamarin プラットフォームは、クライアントが SOAP メッセージ エンベロープ内の資格情報を送信できるようにする Ws-security プロトコルをサポートしていません。 ただし、Xamarin プラットフォームでは、適切なを指定して、HTTP 基本認証資格情報をサーバーに送信する機能はサポート`ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

次に、基本認証資格情報を指定できます。

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

上記の例では、「の種類は 0 の領域不足になりました」というメッセージが表示される場合向上することが 0 の種類の領域の数を追加して、`–aot “trampolines={number of trampolines}”`ビルドに渡す引数。 詳細については、次を参照してください。[トラブルシューティング](~/ios/troubleshooting/troubleshooting.md#trampolines)です。

HTTP 基本認証の詳細については、REST web サービスのコンテキストでは参照[RESTful Web サービス認証](~/xamarin-forms/data-cloud/authentication/rest.md)です。

## <a name="summary"></a>まとめ

このガイドでは、別の web サービスのテクノロジを使用する方法を示しました。 説明のトピックには、REST サービス、SOAP サービス、および Windows Communication Foundation サービスとの通信が含まれます。

## <a name="related-links"></a>関連リンク

- [Web サービスのサンプル](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Xamarin.Forms で web サービス](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel メタデータ ユーティリティ ツール (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/en-us/library/system.servicemodel.basichttpbinding.aspx)
