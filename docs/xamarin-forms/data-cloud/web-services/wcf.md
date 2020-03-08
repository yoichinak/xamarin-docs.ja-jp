---
title: Windows Communication Foundation (WCF) Web サービスを使用する
description: この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: 28cb1573262b63cc2b0ccad9f468fe36c682718d
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915389"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) Web サービスを使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF は、サービス指向アプリケーションを構築するための Microsoft の統合フレームワークです。これにより、開発者は、セキュリティで保護された、信頼性の高い、トランザクションで相互運用可能な分散アプリケーションを構築できます。この記事では、Xamarin アプリケーションから WCF Simple Object Access Protocol (SOAP) サービスを使用する方法について説明します。_

WCF は、次のようなさまざまなコントラクトを持つサービスを記述します。

- **データコントラクト**–メッセージ内のコンテンツの基礎となるデータ構造を定義します。
- **メッセージコントラクト**–既存のデータコントラクトからメッセージを作成します。
- **エラーコントラクト**-カスタム SOAP エラーを指定できるようにします。
- **サービスコントラクト**: サービスがサポートする操作と、各操作との対話に必要なメッセージを指定します。 また、各サービスでの操作に関連付けることができる任意のカスタム エラー動作を指定します。

ASP.NET ウェブサービス (ASMX) と WCF には違いがありますが、WCF では、ASMX が HTTP 経由の SOAP メッセージを提供するのと同じ機能をサポートしています。 ASMX サービスの使用方法の詳細については、「 [ASP.NET ウェブサービス (asmx) の使用](~/xamarin-forms/data-cloud/web-services/asmx.md)」を参照してください。

> [!IMPORTANT]
> WCF の Xamarin プラットフォームサポートは、`BasicHttpBinding` クラスを使用して、HTTP/HTTPS 経由でテキストエンコードされた SOAP メッセージに制限されています。
>
> WCF サポートでは、プロキシを生成して TodoWCFService をホストするために、Windows 環境でのみ使用可能なツールを使用する必要があります。 IOS アプリをビルドしてテストするには、TodoWCFService を Windows コンピューターに展開するか、Azure web サービスとしてデプロイする必要があります。
>
> Xamarin Forms ネイティブアプリは、通常、コードを .NET Standard クラスライブラリと共有します。 ただし、現在、.NET Core では WCF がサポートされていないため、共有プロジェクトは従来のポータブルクラスライブラリである必要があります。 .NET Core の WCF サポートの詳細については、「[サーバーアプリ用 .Net core と .NET Framework の選択](/dotnet/standard/choosing-core-framework-server)」を参照してください。

サンプルアプリケーションソリューションには、ローカルで実行できる WCF サービスが含まれています。次のスクリーンショットを参照してください。

![](wcf-images/portal.png "Sample Application")

> [!NOTE]
> iOS 9 以降では、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
>
> `HTTPS` プロトコルを使用できず、インターネットリソースに対してセキュリティで保護された通信を行うことができない場合は、ATS をオプトアウトできます。 これは、アプリの**情報**ファイルを更新することで実現できます。 詳細については、「[アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用する

WCF サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータモデルの詳細については、「[データのモデリング](~/xamarin-forms/data-cloud/web-services/introduction.md)」を参照してください。

WCF サービスを使用するために*プロキシ*を生成する必要があります。これにより、アプリケーションはサービスに接続できるようになります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 Visual Studio 2017 で .NET Standard ライブラリを web サービスのサービス参照を追加する Microsoft WCF Web Service Reference Provider を使用して、プロキシを構築できます。 Visual Studio 2017 での Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、「 [ServiceModel メタデータユーティリティツール (svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)」を参照してください。

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンでは、非同期操作は、非同期操作を開始および終了する*Beginoperationname*と*EndOperationName*という2つのメソッドとして実装されます。

*Beginoperationname*メソッドは、非同期操作を開始し、`IAsyncResult` インターフェイスを実装するオブジェクトを返します。 *Beginoperationname*を呼び出した後、アプリケーションは、スレッドプールのスレッドで非同期操作を実行しながら、呼び出し元のスレッドで命令の実行を継続できます。

*Beginoperationname*の呼び出しごとに、アプリケーションも*EndOperationName*を呼び出して、操作の結果を取得する必要があります。 *EndOperationName*の戻り値は、同期 web サービスメソッドによって返される型と同じです。 たとえば、`EndGetTodoItems` メソッドは、`TodoItem` インスタンスのコレクションを返します。 *EndOperationName*メソッドには、 *beginoperationname*メソッドへの対応する呼び出しによって返されるインスタンスに設定する必要がある `IAsyncResult` パラメーターも含まれています。

タスク並列ライブラリ (TPL) を使用すると、非同期操作を同じ `Task` オブジェクトにカプセル化することで、APM の begin/end メソッドのペアを使用するプロセスを簡略化できます。 このカプセル化は、`TaskFactory.FromAsync` メソッドの複数のオーバーロードによって提供されます。

APM の詳細については、MSDN の「[非同期プログラミングモデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL および従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)」を参照してください。

### <a name="create-the-todoserviceclient-object"></a>TodoServiceClient オブジェクトを作成する

生成されたプロキシクラスは、HTTP 経由で WCF サービスと通信するために使用される `TodoServiceClient` クラスを提供します。 識別されたサービス インスタンスの URI からの非同期操作は、web サービス メソッドを呼び出すために、機能を提供します。 非同期操作の詳細については、「 [Async Support の概要](~/cross-platform/platform/async.md)」を参照してください。

`TodoServiceClient` インスタンスはクラスレベルで宣言されるので、次のコード例に示すように、アプリケーションが WCF サービスを使用する必要がある限り、オブジェクトが存在するようになります。

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` インスタンスは、バインド情報とエンドポイントアドレスを使用して構成されます。 バインディングを使用して、トランスポート、エンコーディング、およびアプリケーションとサービスが互いに通信するために必要なプロトコルの詳細を指定します。 `BasicHttpBinding` は、テキストエンコードされた SOAP メッセージが HTTP トランスポートプロトコルを介して送信されることを指定します。 パブリッシュされた複数のインスタンスがあること、WCF サービスの異なるインスタンスに接続するアプリケーションをエンドポイント アドレスの指定できます。

サービス参照の構成の詳細については、「[サービス参照の構成](~/cross-platform/data-cloud/web-services/index.md#wcf)」を参照してください。

### <a name="create-data-transfer-objects"></a>データ転送オブジェクトの作成

このサンプルアプリケーションでは、`TodoItem` クラスを使用してデータをモデル化します。 `TodoItem` 項目を web サービスに格納するには、最初に、`TodoItem` 型のプロキシによって生成されたプロキシに変換する必要があります。 これは、次のコード例に示すように、`ToWCFServiceTodoItem` メソッドによって実現されます。

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

このメソッドは、単に新しい `TodoWCFService.TodoItem` インスタンスを作成し、`TodoItem` インスタンスの同一のプロパティに各プロパティを設定します。

同様に、web サービスからデータを取得する場合は、プロキシによって生成された `TodoItem` 型から `TodoItem` インスタンスに変換する必要があります。 これは、次のコード例に示すように、`FromWCFServiceTodoItem` メソッドを使用して行います。

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

このメソッドは、`TodoItem` 型で生成されたプロキシからデータを取得し、新しく作成された `TodoItem` インスタンスにそのデータを設定するだけです。

### <a name="retrieve-data"></a>データの取得

`TodoServiceClient.BeginGetTodoItems` メソッドと `TodoServiceClient.EndGetTodoItems` メソッドを使用して、web サービスによって提供される `GetTodoItems` 操作を呼び出します。 これらの非同期メソッドは、次のコード例に示すように、`Task` オブジェクトにカプセル化されます。

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` メソッドは、`TodoServiceClient.BeginGetTodoItems` メソッドの完了後に `TodoServiceClient.EndGetTodoItems` メソッドを実行する `Task` を作成します。 `null` デリゲートには、データが渡されていないことを示す `BeginGetTodoItems` パラメーターがあります。 最後に、`TaskCreationOptions` 列挙体の値は、タスクの作成と実行の既定の動作を使用することを指定します。

`TodoServiceClient.EndGetTodoItems` メソッドは `TodoWCFService.TodoItem` インスタンスの `ObservableCollection` を返します。これは、`TodoItem` インスタンスの `List` に変換されて表示されます。

### <a name="create-data"></a>データの作成

`TodoServiceClient.BeginCreateTodoItem` メソッドと `TodoServiceClient.EndCreateTodoItem` メソッドを使用して、web サービスによって提供される `CreateTodoItem` 操作を呼び出します。 これらの非同期メソッドは、次のコード例に示すように、`Task` オブジェクトにカプセル化されます。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` メソッドは、`TodoServiceClient.BeginCreateTodoItem` メソッドの完了後に `TodoServiceClient.EndCreateTodoItem` メソッドを実行する `Task` を作成します。 `todoItem` パラメーターは、web サービスによって作成される `BeginCreateTodoItem` を指定するために `TodoItem` デリゲートに渡されるデータです。 最後に、`TaskCreationOptions` 列挙体の値は、タスクの作成と実行の既定の動作を使用することを指定します。

アプリケーションによって処理される `TodoItem`の作成に失敗した場合、web サービスは `FaultException` をスローします。

### <a name="update-data"></a>データの更新

`TodoServiceClient.BeginEditTodoItem` メソッドと `TodoServiceClient.EndEditTodoItem` メソッドを使用して、web サービスによって提供される `EditTodoItem` 操作を呼び出します。 これらの非同期メソッドは、次のコード例に示すように、`Task` オブジェクトにカプセル化されます。

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` メソッドは、`TodoServiceClient.BeginCreateTodoItem` メソッドの完了後に `TodoServiceClient.EndEditTodoItem` メソッドを実行する `Task` を作成します。 `todoItem` パラメーターは、web サービスによって更新される `BeginEditTodoItem` を指定するために `TodoItem` デリゲートに渡されるデータです。 最後に、`TaskCreationOptions` 列挙体の値は、タスクの作成と実行の既定の動作を使用することを指定します。

Web サービスは、アプリケーションによって処理される `TodoItem`の検索または更新に失敗した場合に、`FaultException` をスローします。

### <a name="delete-data"></a>データの削除

`TodoServiceClient.BeginDeleteTodoItem` メソッドと `TodoServiceClient.EndDeleteTodoItem` メソッドを使用して、web サービスによって提供される `DeleteTodoItem` 操作を呼び出します。 これらの非同期メソッドは、次のコード例に示すように、`Task` オブジェクトにカプセル化されます。

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` メソッドは、`TodoServiceClient.BeginDeleteTodoItem` メソッドの完了後に `TodoServiceClient.EndDeleteTodoItem` メソッドを実行する `Task` を作成します。 `id` パラメーターは、web サービスによって削除される `BeginDeleteTodoItem` を指定するために `TodoItem` デリゲートに渡されるデータです。 最後に、`TaskCreationOptions` 列挙体の値は、タスクの作成と実行の既定の動作を使用することを指定します。

Web サービスは、アプリケーションによって処理される `TodoItem`の検索または削除に失敗した場合に、`FaultException` をスローします。

## <a name="configure-remote-access-to-iis-express"></a>IIS Express へのリモートアクセスを構成する
Visual Studio 2017 または Visual Studio 2019 では、追加の構成なしで、PC で UWP アプリケーションをテストできます。 Android および iOS クライアントのテストには、このセクションの追加手順が必要になる場合があります。 詳細については[、「iOS シミュレーターおよび Android エミュレーターからのローカル Web サービスへの接続](~/cross-platform/deploy-test/connect-to-local-web-services.md)」を参照してください。

既定では、IIS Express は `localhost`への要求にのみ応答します。 リモートデバイス (Android デバイス、iPhone、シミュレーターなど) には、ローカル WCF サービスへのアクセス権はありません。 ローカルネットワーク上の Windows 10 ワークステーション IP アドレスを把握しておく必要があります。 この例では、ワークステーションに IP アドレス `192.168.1.143`があることを前提としています。 次の手順では、リモート接続を受け入れ、物理または仮想デバイスからサービスに接続するように Windows 10 と IIS Express を構成する方法について説明します。

1. **Windows ファイアウォールに例外を追加**します。 サブネット上のアプリケーションが WCF サービスとの通信に使用できる Windows ファイアウォールを介してポートを開く必要があります。 ファイアウォールでポート49393を開く受信規則を作成します。 管理コマンドプロンプトで、次のコマンドを実行します。

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **リモート接続を受け入れるように IIS Express を構成**します。 IIS Express を構成するには、 **[solution directory]\.vs\config\applicationhost.config**で IIS Express の構成ファイルを編集します。`TodoWCFService`という名前の `site` 要素を検索します。 次の XML のようになります。

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
        </bindings>
    </site>
    ```

    外部トラフィックと Android エミュレーターにポート49393を開くには、2つの `binding` 要素を追加する必要があります。 バインディングは、IIS Express が要求に応答する方法を指定する `[IP address]:[port]:[hostname]` 形式を使用します。 外部要求には、`binding`として指定する必要があるホスト名があります。 次の XML を `bindings` 要素に追加し、IP アドレスを実際の IP アドレスに置き換えます。

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    変更後、`bindings` 要素は次のようになります。

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
            <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
            <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
        </bindings>
    </site>
    ```

    >[!IMPORTANT]
    >既定では、IIS Express は、セキュリティ上の理由から、外部ソースからの接続を受け入れません。 リモートデバイスからの接続を有効にするには、管理アクセス許可を使用して IIS Express を実行する必要があります。 これを行う最も簡単な方法は、管理アクセス許可を使用して Visual Studio 2017 を実行することです。 TodoWCFService を実行すると、管理者権限で IIS Express が起動します。

    これらの手順を完了すると、TodoWCFService を実行して、サブネット上の他のデバイスから接続できるようになります。 これをテストするには、アプリケーションを実行し、`http://localhost:49393/TodoService.svc`にアクセスします。 その URL にアクセスするときに**無効な要求**エラーが発生した場合、IIS Express 構成で `bindings` が正しくない可能性があります (要求は IIS Express に到達していますが、拒否されています)。 別のエラーが発生した場合は、アプリケーションが実行されていないか、ファイアウォールが正しく構成されていない可能性があります。

    IIS Express がサービスの実行とサービスの実行を継続できるようにするには、[**プロジェクトのプロパティ] > [Web > デバッガー**] の **[エディットコンティニュ]** オプションをオフにします。

1. **サービスへのアクセスに使用するエンドポイントデバイスをカスタマイズ**します。 この手順では、物理デバイスまたはエミュレートされたデバイスで実行されているクライアントアプリケーションを、WCF サービスにアクセスするように構成します。

    Android エミュレーターは、エミュレーターがホストコンピューターの `localhost` アドレスに直接アクセスできないようにする内部プロキシを利用します。 代わりに、エミュレーターのアドレス `10.0.2.2` が内部プロキシ経由でホストコンピューター上の `localhost` にルーティングされます。 これらのプロキシされた要求は、要求ヘッダーのホスト名として `127.0.0.1` します。このため、上記の手順でこのホスト名の IIS Express バインドを作成したのはそのためです。

    IOS シミュレーターは、[リモートの Ios シミュレーター For Windows](~/tools/ios-simulator/index.md)を使用している場合でも、Mac ビルドホストで実行されます。 シミュレーターからのネットワーク要求では、ローカルネットワーク上のホスト名がホスト名として使用されます (この例では `192.168.1.143`ますが、実際の IP アドレスは異なる可能性があります)。 このため、上記の手順でこのホスト名の IIS Express バインドを作成したのはこのためです。

    TodoWCF (ポータブル) プロジェクトの**Constants.cs**ファイルの [`SoapUrl`] プロパティに、使用しているネットワークに適した値が設定されていることを確認します。

    ```csharp
    public static string SoapUrl
    {
        get
        {
            var defaultUrl = "http://localhost:49393/TodoService.svc";

            if (Device.RuntimePlatform == Device.Android)
            {
                defaultUrl = "http://10.0.2.2:49393/TodoService.svc";
            }
            else if (Device.RuntimePlatform == Device.iOS)
            {
                defaultUrl = "http://192.168.1.143:49393/TodoService.svc";
            }

            return defaultUrl;
        }
    }
    ```

    適切なエンドポイントを使用して**Constants.cs**を構成すると、物理デバイスまたは仮想デバイスから、Windows 10 ワークステーションで実行されている TodoWCFService に接続できるようになります。

## <a name="related-links"></a>関連リンク

- [TodoWCF (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [方法: Windows Communication Foundation クライアントを作成する](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel メタデータユーティリティツール (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
