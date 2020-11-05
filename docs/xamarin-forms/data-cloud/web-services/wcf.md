---
title: Windows Communication Foundation (WCF) Web サービスを使用する
description: この記事では、アプリケーションから WCF Simple Object Access Protocol (SOAP) サービスを使用する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8d6b489ff31333e87c28796c7de49bf0e59bff9d
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373576"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) Web サービスを使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF は、サービス指向アプリケーションを構築するための Microsoft の統合フレームワークです。これにより、開発者は、セキュリティで保護された、信頼性の高い、トランザクションで相互運用可能な分散アプリケーションを構築できます。この記事では、アプリケーションから WCF Simple Object Access Protocol (SOAP) サービスを使用する方法について説明し Xamarin.Forms ます。_

WCF は、次のようなさまざまなコントラクトを持つサービスを記述します。

- **データコントラクト** –メッセージ内のコンテンツの基礎となるデータ構造を定義します。
- **メッセージコントラクト** –既存のデータコントラクトからメッセージを作成します。
- **エラーコントラクト** -カスタム SOAP エラーを指定できるようにします。
- **サービスコントラクト** : サービスがサポートする操作と、各操作との対話に必要なメッセージを指定します。 また、各サービスの操作に関連付けることができるカスタムのエラー動作も指定します。

ASP.NET ウェブサービス (ASMX) と WCF には違いがありますが、WCF では、ASMX が HTTP 経由の SOAP メッセージを提供するのと同じ機能をサポートしています。 ASMX サービスの使用方法の詳細については、「 [ASP.NET ウェブサービス (asmx) の使用](~/xamarin-forms/data-cloud/web-services/asmx.md)」を参照してください。

> [!IMPORTANT]
> WCF の Xamarin プラットフォームサポートは、クラスを使用して、HTTP/HTTPS 経由でテキストエンコードされた SOAP メッセージに制限されてい `BasicHttpBinding` ます。
>
> WCF サポートでは、プロキシを生成して TodoWCFService をホストするために、Windows 環境でのみ使用可能なツールを使用する必要があります。 IOS アプリをビルドしてテストするには、TodoWCFService を Windows コンピューターに展開するか、Azure web サービスとしてデプロイする必要があります。
>
> Xamarin Forms ネイティブアプリは、通常、コードを .NET Standard クラスライブラリと共有します。 ただし、現在、.NET Core では WCF がサポートされていないため、共有プロジェクトは従来のポータブルクラスライブラリである必要があります。 .NET Core の WCF サポートの詳細については、「 [サーバーアプリ用 .Net core と .NET Framework の選択](/dotnet/standard/choosing-core-framework-server)」を参照してください。

サンプルアプリケーションソリューションには、ローカルで実行できる WCF サービスが含まれています。次のスクリーンショットを参照してください。

![サンプル アプリケーション](wcf-images/portal.png)

> [!NOTE]
> IOS 9 以降では、アプリトランスポートセキュリティ (ATS) によって、インターネットリソース (アプリのバックエンドサーバーなど) とアプリの間にセキュリティで保護された接続が適用されるため、機密情報が誤って開示されるのを防ぐことができます。 IOS 9 用に構築されたアプリでは、ATS が既定で有効になっているため、すべての接続は、ATS のセキュリティ要件の対象となります。 接続がこれらの要件を満たしていない場合、例外が発生して失敗します。
>
> `HTTPS`インターネットリソースに対してプロトコルとセキュリティで保護された通信を使用できない場合は、ATS をオプトアウトできます。 これは、アプリの **情報** ファイルを更新することで実現できます。 詳細については、「 [アプリトランスポートセキュリティ](~/ios/app-fundamentals/ats.md)」を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用する

WCF サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成する|XML シリアル化 TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化 TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化 TodoItem|

アプリケーションで使用されるデータモデルの詳細については、「 [データのモデリング](~/xamarin-forms/data-cloud/web-services/introduction.md)」を参照してください。

WCF サービスを使用するために *プロキシ* を生成する必要があります。これにより、アプリケーションはサービスに接続できるようになります。 プロキシは、メソッドと関連付けられたサービス構成を定義するサービスメタデータを使用することによって構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 プロキシを作成するには、Visual Studio 2017 の Microsoft WCF Web Service Reference Provider を使用して、Web サービスのサービス参照を .NET Standard ライブラリに追加します。 Visual Studio 2017 の Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりに、ServiceModel Metadata Utility Tool (svcutil.exe) を使用することもできます。 詳細については、「 [ServiceModel メタデータユーティリティツール (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)」を参照してください。

生成されたプロキシクラスは、非同期プログラミングモデル (APM) デザインパターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンでは、非同期操作は、非同期操作を開始および終了する *Beginoperationname* と *EndOperationName* という2つのメソッドとして実装されます。

*Beginoperationname* メソッドは、非同期操作を開始し、インターフェイスを実装するオブジェクトを返し `IAsyncResult` ます。 *Beginoperationname* を呼び出した後、アプリケーションは、スレッドプールのスレッドで非同期操作を実行しながら、呼び出し元のスレッドで命令の実行を継続できます。

*Beginoperationname* の呼び出しごとに、アプリケーションも *EndOperationName* を呼び出して、操作の結果を取得する必要があります。 *EndOperationName* の戻り値は、同期 web サービスメソッドによって返される型と同じです。 たとえば、メソッドは、 `EndGetTodoItems` インスタンスのコレクションを返し `TodoItem` ます。 *EndOperationName* メソッドには、 `IAsyncResult` *beginoperationname* メソッドへの対応する呼び出しによって返されるインスタンスに設定する必要があるパラメーターも含まれています。

タスク並列ライブラリ (TPL) を使用すると、非同期操作を同じオブジェクトにカプセル化することで、APM の begin/end メソッドのペアを使用するプロセスを簡略化でき `Task` ます。 このカプセル化は、メソッドの複数のオーバーロードによって提供され `TaskFactory.FromAsync` ます。

APM の詳細については、MSDN の「 [非同期プログラミングモデル](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) と [TPL および従来の .NET Framework 非同期プログラミング](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming) 」を参照してください。

### <a name="create-the-todoserviceclient-object"></a>TodoServiceClient オブジェクトを作成する

生成されたプロキシクラスは、 `TodoServiceClient` HTTP 経由で WCF サービスと通信するために使用されるクラスを提供します。 URI で識別されるサービスインスタンスの非同期操作として web サービスメソッドを呼び出すための機能を提供します。 非同期操作の詳細については、「 [Async Support の概要](~/cross-platform/platform/async.md)」を参照してください。

`TodoServiceClient`インスタンスはクラスレベルで宣言されるので、次のコード例に示すように、アプリケーションが WCF サービスを使用する必要がある限り、オブジェクトが存在するようになります。

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

`TodoServiceClient`インスタンスは、バインド情報とエンドポイントアドレスを使用して構成されます。 バインディングは、アプリケーションとサービスが相互に通信するために必要なトランスポート、エンコーディング、およびプロトコルの詳細を指定するために使用されます。 は、 `BasicHttpBinding` テキストエンコードされた SOAP メッセージが HTTP トランスポートプロトコルを介して送信されることを指定します。 エンドポイントアドレスを指定すると、アプリケーションは、複数の公開されたインスタンスがある場合に、WCF サービスの異なるインスタンスに接続できます。

サービス参照の構成の詳細については、「 [サービス参照の構成](~/cross-platform/data-cloud/web-services/index.md#wcf)」を参照してください。

### <a name="create-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプルアプリケーションでは、クラスを使用して `TodoItem` データをモデル化します。 Web サービスに項目を格納するには、 `TodoItem` 最初にプロキシ生成型に変換する必要があり `TodoItem` ます。 これは、次の `ToWCFServiceTodoItem` コード例に示すように、メソッドによって実現されます。

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

このメソッドは、単に新しいインスタンスを作成 `TodoWCFService.TodoItem` し、インスタンスからの同一のプロパティに各プロパティを設定し `TodoItem` ます。

同様に、web サービスからデータを取得する場合は、プロキシによって生成された型からインスタンスにデータを変換する必要があり `TodoItem` `TodoItem` ます。 これは、次の `FromWCFServiceTodoItem` コード例に示すように、メソッドを使用して行います。

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

このメソッドは、単純にプロキシによって生成された型からデータを取得 `TodoItem` し、新しく作成されたインスタンスに設定し `TodoItem` ます。

### <a name="retrieve-data"></a>データを取得する

`TodoServiceClient.BeginGetTodoItems`メソッドと `TodoServiceClient.EndGetTodoItems` メソッドは、 `GetTodoItems` web サービスによって提供される操作を呼び出すために使用されます。 これらの非同期メソッドは、 `Task` 次のコード例に示すように、オブジェクトにカプセル化されます。

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

メソッドは、メソッドが完了した後にメソッドを `Task.Factory.FromAsync` 実行するを作成します。これには、 `Task` `TodoServiceClient.EndGetTodoItems` `TodoServiceClient.BeginGetTodoItems` `null` データがデリゲートに渡されていないことを示すパラメーターがあり `BeginGetTodoItems` ます。 最後に、列挙体の値は、 `TaskCreationOptions` タスクの作成と実行の既定の動作を使用することを指定します。

メソッドは、 `TodoServiceClient.EndGetTodoItems` インスタンスのを返します `ObservableCollection` 。このインスタンスは、インスタンス `TodoWCFService.TodoItem` のに変換され `List` て表示され `TodoItem` ます。

### <a name="create-data"></a>データの作成

`TodoServiceClient.BeginCreateTodoItem`メソッドと `TodoServiceClient.EndCreateTodoItem` メソッドは、 `CreateTodoItem` web サービスによって提供される操作を呼び出すために使用されます。 これらの非同期メソッドは、 `Task` 次のコード例に示すように、オブジェクトにカプセル化されます。

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

メソッドは、メソッドが完了した後にメソッドを `Task.Factory.FromAsync` 実行するを作成し `Task` `TodoServiceClient.EndCreateTodoItem` `TodoServiceClient.BeginCreateTodoItem` `todoItem` ます。パラメーターは、 `BeginCreateTodoItem` `TodoItem` web サービスによって作成されるを指定するためにデリゲートに渡されるデータです。 最後に、列挙体の値は、 `TaskCreationOptions` タスクの作成と実行の既定の動作を使用することを指定します。

`FaultException` `TodoItem` アプリケーションによって処理されるの作成に失敗した場合、web サービスはをスローします。

### <a name="update-data"></a>データの更新

`TodoServiceClient.BeginEditTodoItem`メソッドと `TodoServiceClient.EndEditTodoItem` メソッドは、 `EditTodoItem` web サービスによって提供される操作を呼び出すために使用されます。 これらの非同期メソッドは、 `Task` 次のコード例に示すように、オブジェクトにカプセル化されます。

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

メソッドは、メソッドが完了した後にメソッドを `Task.Factory.FromAsync` 実行するを作成し `Task` `TodoServiceClient.EndEditTodoItem` `TodoServiceClient.BeginCreateTodoItem` `todoItem` ます。パラメーターは、 `BeginEditTodoItem` `TodoItem` web サービスによって更新されるを指定するためにデリゲートに渡されるデータです。 最後に、列挙体の値は、 `TaskCreationOptions` タスクの作成と実行の既定の動作を使用することを指定します。

Web サービスは、 `FaultException` `TodoItem` アプリケーションによって処理されるの検索または更新に失敗した場合、をスローします。

### <a name="delete-data"></a>データの削除

`TodoServiceClient.BeginDeleteTodoItem`メソッドと `TodoServiceClient.EndDeleteTodoItem` メソッドは、 `DeleteTodoItem` web サービスによって提供される操作を呼び出すために使用されます。 これらの非同期メソッドは、 `Task` 次のコード例に示すように、オブジェクトにカプセル化されます。

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

メソッドは、メソッドが完了した後にメソッドを `Task.Factory.FromAsync` 実行するを作成し `Task` `TodoServiceClient.EndDeleteTodoItem` `TodoServiceClient.BeginDeleteTodoItem` `id` ます。パラメーターは、 `BeginDeleteTodoItem` `TodoItem` web サービスによって削除されるを指定するためにデリゲートに渡されるデータです。 最後に、列挙体の値は、 `TaskCreationOptions` タスクの作成と実行の既定の動作を使用することを指定します。

Web サービスは、 `FaultException` `TodoItem` アプリケーションによって処理されるを検索または削除できない場合、をスローします。

## <a name="configure-remote-access-to-iis-express"></a>IIS Express へのリモートアクセスを構成する
Visual Studio 2017 または Visual Studio 2019 では、追加の構成なしで、PC で UWP アプリケーションをテストできます。 Android および iOS クライアントのテストには、このセクションの追加手順が必要になる場合があります。 詳細については [、「iOS シミュレーターおよび Android エミュレーターからのローカル Web サービスへの接続](~/cross-platform/deploy-test/connect-to-local-web-services.md) 」を参照してください。

既定では、IIS Express はへの要求にのみ応答 `localhost` します。 リモートデバイス (Android デバイス、iPhone、シミュレーターなど) には、ローカル WCF サービスへのアクセス権はありません。 ローカルネットワーク上の Windows 10 ワークステーション IP アドレスを把握しておく必要があります。 この例では、ワークステーションに IP アドレスがあることを前提としてい `192.168.1.143` ます。 次の手順では、リモート接続を受け入れ、物理または仮想デバイスからサービスに接続するように Windows 10 と IIS Express を構成する方法について説明します。

1. **Windows ファイアウォールに例外を追加** します。 サブネット上のアプリケーションが WCF サービスとの通信に使用できる Windows ファイアウォールを介してポートを開く必要があります。 ファイアウォールでポート49393を開く受信規則を作成します。 管理コマンドプロンプトで、次のコマンドを実行します。

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **リモート接続を受け入れるように IIS Express を構成** します。 IIS Express を構成するには、 **[ソリューションディレクトリ] \.vs\config\applicationhost.config** で IIS Express の構成ファイルを編集します。という `site` 名前の要素を検索 `TodoWCFService` します。 次の XML のようになります。

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

    `binding`外部トラフィックと Android エミュレーターにポート49393を開くには、2つの要素を追加する必要があります。 バインディングは、 `[IP address]:[port]:[hostname]` IIS Express が要求に応答する方法を指定する形式を使用します。 外部要求には、として指定する必要があるホスト名があり `binding` ます。 次の XML を要素に追加 `bindings` し、ip アドレスを実際の ip アドレスに置き換えます。

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    変更後、 `bindings` 要素は次のようになります。

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

    これらの手順を完了すると、TodoWCFService を実行して、サブネット上の他のデバイスから接続できるようになります。 これをテストするには、アプリケーションを実行し、にアクセスし `http://localhost:49393/TodoService.svc` ます。 その URL にアクセスしたときに **無効な要求** エラーが発生した場合、 `bindings` IIS Express の構成では正しくない可能性があります (要求は IIS Express に到達していますが、拒否されています)。 別のエラーが発生した場合は、アプリケーションが実行されていないか、ファイアウォールが正しく構成されていない可能性があります。

    IIS Express がサービスの実行とサービスの実行を継続できるようにするには、[ **プロジェクトのプロパティ] > [Web > デバッガー** ] の [ **エディットコンティニュ** ] オプションをオフにします。

1. **サービスへのアクセスに使用するエンドポイントデバイスをカスタマイズ** します。 この手順では、物理デバイスまたはエミュレートされたデバイスで実行されているクライアントアプリケーションを、WCF サービスにアクセスするように構成します。

    Android エミュレーターは、エミュレーターがホストコンピューターのアドレスに直接アクセスできないようにする内部プロキシを利用し `localhost` ます。 代わりに、エミュレーターのアドレス `10.0.2.2` が `localhost` 内部プロキシ経由でホストコンピューター上のにルーティングされます。 これらのプロキシされた要求は、 `127.0.0.1` 要求ヘッダーのホスト名としてを持ちます。このため、上記の手順でこのホスト名の IIS Express バインドを作成したのはそのためです。

    IOS シミュレーターは、 [リモートの Ios シミュレーター For Windows](~/tools/ios-simulator/index.md)を使用している場合でも、Mac ビルドホストで実行されます。 シミュレーターからのネットワーク要求では、ローカルネットワーク上のホスト名がホスト名として使用されます (この例では `192.168.1.143` 、実際の ip アドレスは異なる可能性があります)。 このため、上記の手順でこのホスト名の IIS Express バインドを作成したのはこのためです。

    `SoapUrl`TodoWCF (ポータブル) プロジェクトの **Constants.cs** ファイルのプロパティに、使用しているネットワークに適した値が設定されていることを確認します。

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

    適切なエンドポイントを使用して **Constants.cs** を構成すると、物理デバイスまたは仮想デバイスから、Windows 10 ワークステーションで実行されている TodoWCFService に接続できるようになります。

## <a name="related-links"></a>関連リンク

- [TodoWCF (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [方法 : Windows Communication Foundation クライアントを作成する](/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel メタデータユーティリティツール (svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)