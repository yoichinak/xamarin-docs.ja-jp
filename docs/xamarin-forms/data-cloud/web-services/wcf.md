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
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888849"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) Web サービスを使用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF は、サービス指向アプリケーションを構築するための Microsoft の統一されたフレームワークです。セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。_

WCF は、次のようなさまざまなコントラクトを持つサービスを記述します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を形成するデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **フォールト コントラクト**– を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**– と、メッセージが各操作と対話するために必要なサービスをサポートする操作を指定します。 また、各サービスでの操作に関連付けることができる任意のカスタム エラー動作を指定します。

ASP.NET ウェブサービス (ASMX) と WCF には違いがありますが、WCF では、ASMX が HTTP 経由の SOAP メッセージを提供するのと同じ機能をサポートしています。 ASMX サービスの使用方法の詳細については、「 [ASP.NET ウェブサービス (asmx) の使用](~/xamarin-forms/data-cloud/web-services/asmx.md)」を参照してください。

> [!IMPORTANT]
> WCF の Xamarin プラットフォームサポートは、 `BasicHttpBinding`クラスを使用して、HTTP/HTTPS 経由でテキストエンコードされた SOAP メッセージに制限されています。
>
> WCF サポートでは、プロキシを生成して TodoWCFService をホストするために、Windows 環境でのみ使用可能なツールを使用する必要があります。 IOS アプリをビルドしてテストするには、TodoWCFService を Windows コンピューターに展開するか、Azure web サービスとしてデプロイする必要があります。
>
> Xamarin Forms ネイティブアプリは、通常、コードを .NET Standard クラスライブラリと共有します。 ただし、現在、.NET Core では WCF がサポートされていないため、共有プロジェクトは従来のポータブルクラスライブラリである必要があります。 .NET Core の WCF サポートの詳細については、「[サーバーアプリ用 .Net core と .NET Framework の選択](/dotnet/standard/choosing-core-framework-server)」を参照してください。

サンプルアプリケーションソリューションには、ローカルで実行できる WCF サービスが含まれています。次のスクリーンショットを参照してください。

![](wcf-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。 ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
>
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用する

WCF サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータ モデルの詳細については、[、データのモデリング](~/xamarin-forms/data-cloud/web-services/introduction.md)を参照してください。

A*プロキシ*により、アプリケーションは、サービスに接続する WCF サービスを使用する生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 Visual Studio 2017 で .NET Standard ライブラリを web サービスのサービス参照を追加する Microsoft WCF Web Service Reference Provider を使用して、プロキシを構築できます。 Visual Studio 2017 での Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)を参照してください。

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 たとえば、`EndGetTodoItems`メソッドのコレクションを返します`TodoItem`インスタンス。 *EndOperationName*メソッドも含まれています、`IAsyncResult`パラメーターに対応する呼び出しによって返されるインスタンスに設定する必要がありますが、 *BeginOperationName*メソッド。

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`TaskFactory.FromAsync`メソッド。

APM の詳細については、[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn を参照してください。

### <a name="create-the-todoserviceclient-object"></a>TodoServiceClient オブジェクトを作成する

生成されたプロキシ クラスには、`TodoServiceClient`クラスは、HTTP 経由で WCF サービスと通信するために使用します。 識別されたサービス インスタンスの URI からの非同期操作は、web サービス メソッドを呼び出すために、機能を提供します。 非同期操作の詳細については、[非同期サポートの概要](~/cross-platform/platform/async.md)を参照してください。

`TodoServiceClient`次のコード例に示すように、WCF サービスを使用するアプリケーションに必要な限りのオブジェクトが存在するために、クラス レベルでインスタンスが宣言されています。

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

`TodoServiceClient`インスタンスはバインド情報とエンドポイント アドレスで構成します。 バインディングを使用して、トランスポート、エンコーディング、およびアプリケーションとサービスが互いに通信するために必要なプロトコルの詳細を指定します。 `BasicHttpBinding`テキストでエンコードされた SOAP メッセージは、HTTP トランスポート プロトコル経由で送信されることを指定します。 パブリッシュされた複数のインスタンスがあること、WCF サービスの異なるインスタンスに接続するアプリケーションをエンドポイント アドレスの指定できます。

サービス参照を構成する方法の詳細については、[サービス参照を構成する](~/cross-platform/data-cloud/web-services/index.md#wcf)を参照してください。

### <a name="create-data-transfer-objects"></a>データ転送オブジェクトの作成

サンプル アプリケーションを使用して、`TodoItem`モデル データへのクラス。 格納する、`TodoItem`最初生成されたプロキシに変換する必要がある、web サービス内の項目`TodoItem`型。 これは、`ToWCFServiceTodoItem`メソッドを次のコード例に示すようにします。

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

このメソッドは単に新しい作成`TodoWCFService.TodoItem`インスタンス、および各プロパティから同じプロパティを設定して、`TodoItem`インスタンス。

同様に、web サービスからデータを取得すると、ときにする必要があります変換する必要が生成されたプロキシから`TodoItem`型、`TodoItem`インスタンス。 これを実行、`FromWCFServiceTodoItem`メソッドを次のコード例に示すようにします。

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

このメソッドは、生成されたプロキシからのデータを取得する単に`TodoItem`を入力し、新しく作成設定`TodoItem`インスタンス。

### <a name="retrieve-data"></a>データの取得

`TodoServiceClient.BeginGetTodoItems`と`TodoServiceClient.EndGetTodoItems`メソッド呼び出しを使用して、 `GetTodoItems` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndGetTodoItems`メソッドを 1 回、`TodoServiceClient.BeginGetTodoItems`メソッドが完了したらで、`null`パラメーターにデータが渡されていないことを示す、`BeginGetTodoItems`デリゲートします。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

`TodoServiceClient.EndGetTodoItems`メソッドを返します、`ObservableCollection`の`TodoWCFService.TodoItem`に変換し、インスタンス、`List`の`TodoItem`表示のインスタンス。

### <a name="create-data"></a>データの作成

`TodoServiceClient.BeginCreateTodoItem`と`TodoServiceClient.EndCreateTodoItem`メソッド呼び出しを使用して、 `CreateTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndCreateTodoItem`メソッドを 1 回、`TodoServiceClient.BeginCreateTodoItem`メソッドが完了したらで、`todoItem`パラメーターに渡されるデータ、 `BeginCreateTodoItem` を指定するデリゲート`TodoItem` web サービスによって作成されます。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`FaultException`作成に失敗した場合、`TodoItem`をアプリケーションでこれを処理します。

### <a name="update-data"></a>データの更新

`TodoServiceClient.BeginEditTodoItem`と`TodoServiceClient.EndEditTodoItem`メソッド呼び出しを使用して、 `EditTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndEditTodoItem`メソッドを 1 回、`TodoServiceClient.BeginCreateTodoItem`メソッドが完了したらで、`todoItem`パラメーターに渡されるデータ、 `BeginEditTodoItem` を指定するデリゲート`TodoItem` web サービスで更新します。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`FaultException`を探すか、更新に失敗した場合、 `TodoItem`、アプリケーションでこれを処理します。

### <a name="delete-data"></a>データの削除

`TodoServiceClient.BeginDeleteTodoItem`と`TodoServiceClient.EndDeleteTodoItem`メソッド呼び出しを使用して、 `DeleteTodoItem` web サービスによって提供される操作。 これらの非同期メソッドをカプセル化、`Task`オブジェクト、次のコード例に示すようにします。

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

`Task.Factory.FromAsync`メソッドを作成、`Task`を実行する、`TodoServiceClient.EndDeleteTodoItem`メソッドを 1 回、`TodoServiceClient.BeginDeleteTodoItem`メソッドが完了したらで、`id`パラメーターに渡されるデータ、 `BeginDeleteTodoItem` を指定するデリゲート`TodoItem` web サービスによって削除されます。 値では、最後に、`TaskCreationOptions`列挙型の作成とタスクの実行の既定の動作を使用することを指定します。

Web サービスがスローされます、`FaultException`を探すか、削除に失敗した場合、`TodoItem`をアプリケーションでこれを処理します。

## <a name="configure-remote-access-to-iis-express"></a>IIS Express へのリモートアクセスを構成する
Visual Studio 2017 または Visual Studio 2019 では、追加の構成なしで、PC で UWP アプリケーションをテストできます。 Android および iOS クライアントのテストには、このセクションの追加手順が必要になる場合があります。 詳細については[、「iOS シミュレーターおよび Android エミュレーターからのローカル Web サービスへの接続](~/cross-platform/deploy-test/connect-to-local-web-services.md)」を参照してください。

既定では、IIS Express はへ`localhost`の要求にのみ応答します。 リモートデバイス (Android デバイス、iPhone、シミュレーターなど) には、ローカル WCF サービスへのアクセス権はありません。 ローカルネットワーク上の Windows 10 ワークステーション IP アドレスを把握しておく必要があります。 この例では、ワークステーションに IP アドレス`192.168.1.143`があることを前提としています。 次の手順では、リモート接続を受け入れ、物理または仮想デバイスからサービスに接続するように Windows 10 と IIS Express を構成する方法について説明します。

1. **Windows ファイアウォールに例外を追加**します。 サブネット上のアプリケーションが WCF サービスとの通信に使用できる Windows ファイアウォールを介してポートを開く必要があります。 ファイアウォールでポート49393を開く受信規則を作成します。 管理コマンドプロンプトで、次のコマンドを実行します。

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **リモート接続を受け入れるように IIS Express を構成**します。 IIS Express を構成するには、 **[solution directory\.] vs\config\applicationhost.config**で IIS Express の構成ファイルを編集します。という`site`名前`TodoWCFService`の要素を検索します。 次の XML のようになります。

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

    外部トラフィックと Android エミュレーターに`binding`ポート49393を開くには、2つの要素を追加する必要があります。 バインディングは、IIS Express `[IP address]:[port]:[hostname]`が要求に応答する方法を指定する形式を使用します。 外部要求には、 `binding`として指定する必要があるホスト名があります。 次の XML を`bindings`要素に追加し、ip アドレスを実際の ip アドレスに置き換えます。

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    変更後、要素`bindings`は次のようになります。

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

    これらの手順を完了すると、TodoWCFService を実行して、サブネット上の他のデバイスから接続できるようになります。 これをテストするには、アプリケーションを実行`http://localhost:49393/TodoService.svc`し、にアクセスします。 その URL にアクセスしたときに**無効な要求**エラーが`bindings`発生した場合、IIS Express の構成では正しくない可能性があります (要求は IIS Express に到達していますが、拒否されています)。 別のエラーが発生した場合は、アプリケーションが実行されていないか、ファイアウォールが正しく構成されていない可能性があります。

    IIS Express がサービスの実行とサービスの実行を継続できるようにするには、[**プロジェクトのプロパティ] > [Web > デバッガー**] の **[エディットコンティニュ]** オプションをオフにします。

1. **サービスへのアクセスに使用するエンドポイントデバイスをカスタマイズ**します。 この手順では、物理デバイスまたはエミュレートされたデバイスで実行されているクライアントアプリケーションを、WCF サービスにアクセスするように構成します。

    Android エミュレーターは、エミュレーターがホストコンピューターの`localhost`アドレスに直接アクセスできないようにする内部プロキシを利用します。 代わりに、エミュレーターの`10.0.2.2`アドレスが内部プロキシ経由で`localhost`ホストコンピューター上のにルーティングされます。 これらのプロキシされ`127.0.0.1`た要求は、要求ヘッダーのホスト名としてを持ちます。このため、上記の手順でこのホスト名の IIS Express バインドを作成したのはそのためです。

    IOS シミュレーターは、[リモートの Ios シミュレーター For Windows](~/tools/ios-simulator/index.md)を使用している場合でも、Mac ビルドホストで実行されます。 シミュレーターからのネットワーク要求では、ローカルネットワーク上のホスト名がホスト名として使用され`192.168.1.143`ます (この例では、実際の ip アドレスは異なる可能性があります)。 このため、上記の手順でこのホスト名の IIS Express バインドを作成したのはこのためです。

    TodoWCF (ポータブル) プロジェクトの**Constants.cs**ファイルのプロパティに、使用しているネットワークに適した値が設定されていることを確認します。`SoapUrl`

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
