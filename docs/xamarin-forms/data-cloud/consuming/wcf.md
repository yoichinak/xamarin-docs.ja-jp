---
title: Windows Communication Foundation (WCF) Web サービスを使用します。
description: この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: 7106c0aed03800d3479471caab0974be3c09c1f8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61328168"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) Web サービスを使用します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF は、サービス指向アプリケーションを構築するための Microsoft の統一されたフレームワークです。セキュリティで保護された、信頼性が高く、トランザクション、および相互運用可能な分散アプリケーションを構築できます。この記事では、Xamarin.Forms アプリケーションから WCF 簡易オブジェクト アクセス プロトコル (SOAP) サービスを使用する方法を示します。_

WCF では、異なるコントラクトを含むのさまざまなサービスについて説明します。

- **データ コントラクト**– メッセージ内のコンテンツの基礎を形成するデータ構造を定義します。
- **メッセージ コントラクト**– 既存のデータ コントラクトからメッセージを作成します。
- **フォールト コントラクト**– を指定するカスタムの SOAP エラーを許可します。
- **サービス コントラクト**– と、メッセージが各操作と対話するために必要なサービスをサポートする操作を指定します。 また、各サービスでの操作に関連付けることができる任意のカスタム エラー動作を指定します。

ASP.NET Web サービス (ASMX) と、WCF が WCF のサポートの相違点、同じ機能が – ASMX を提供する HTTP 経由の SOAP メッセージ。 ASMX サービスの使用に関する詳細については、[消費の ASP.NET Web サービス (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)を参照してください。

> [!IMPORTANT]
> HTTP または HTTPS を使用して経由で WCF の Xamarin プラットフォームのサポートはテキストでエンコードされた SOAP メッセージに制限されます、`BasicHttpBinding`クラス。
>
> WCF のサポートには、プロキシを生成し、TodoWCFService をホストする Windows 環境でのみ使用できるツールの使用が必要です。 IOS アプリのテストのビルドとは、Windows コンピューター、または Azure の web サービスとしての TodoWCFService を展開する必要があります。
>
> 通常、Xamarin Forms のネイティブ アプリは、.NET Standard クラス ライブラリを使用したコードを共有します。 ただし、.NET Core は現在サポートしていません WCF のため、共有プロジェクトは従来のポータブル クラス ライブラリである必要があります。 .NET Core での WCF のサポートについては、次を参照してください。[サーバー アプリ用 .NET Core と .NET Framework の使い分け](/dotnet/standard/choosing-core-framework-server)します。

サンプル アプリケーション ソリューションには、ローカルで実行することができますし、次のスクリーン ショットに示した WCF サービスが含まれています。

![](wcf-images/portal.png "サンプル アプリケーション")

> [!NOTE]
> iOS 9 以降ではは、アプリのトランスポート セキュリティ (ATS) は、機密情報の誤った情報開示を回避をセキュリティで保護された接続 (アプリのバック エンド サーバーなど) のインターネット リソースと、アプリの間に強制します。  ATS が iOS 9 用にビルドされたアプリで既定で有効になるために、すべての接続は ATS セキュリティ要件に応じたされます。 接続はこれらの要件を満たしていない場合は、例外で失敗します。
>
> 使用することができない場合の ATS を選択することができます、`HTTPS`プロトコルし、インターネット リソースのための通信をセキュリティで保護します。 これは、アプリの更新することで実現できます**Info.plist**ファイル。 詳細については、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)を参照してください。

## <a name="consume-the-web-service"></a>Web サービスを使用します。

WCF サービスは、次の操作を提供します。

|操作|説明|パラメーター|
|--- |--- |--- |
|GetTodoItems|To Do アイテムのリストの取得|
|CreateTodoItem|新しい to do 項目を作成します。|XML シリアル化する TodoItem|
|EditTodoItem|To Do アイテムの更新|XML シリアル化する TodoItem|
|DeleteTodoItem|To Do アイテムの削除|XML シリアル化する TodoItem|

アプリケーションで使用されるデータ モデルの詳細については、[、データのモデリング](~/xamarin-forms/data-cloud/walkthrough.md)を参照してください。

A*プロキシ*により、アプリケーションは、サービスに接続する WCF サービスを使用する生成する必要があります。 プロキシは、メソッドと関連付けられているサービスの構成を定義するサービスのメタデータを使用して構築されます。 このメタデータは、web サービスによって生成される Web サービス記述言語 (WSDL) ドキュメントの形式で公開されます。 Visual Studio 2017 で .NET Standard ライブラリを web サービスのサービス参照を追加する Microsoft WCF Web Service Reference Provider を使用して、プロキシを構築できます。 Visual Studio 2017 での Microsoft WCF Web Service Reference Provider を使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)を参照してください。

生成されたプロキシ クラスは、非同期プログラミング モデル (APM) デザイン パターンを使用する web サービスを使用するためのメソッドを提供します。 このパターンで非同期操作はという 2 つのメソッドとして実装*BeginOperationName*と*EndOperationName*を開始し、非同期操作を終了します。

*BeginOperationName*メソッドが非同期操作を開始し、実装するオブジェクトを返します、`IAsyncResult`インターフェイス。 呼び出した後*BeginOperationName*アプリケーションがスレッド プールのスレッドで非同期操作の実行中に、スレッドの呼び出しに関する命令の実行を継続することができます。

呼び出しごとに*BeginOperationName*、アプリケーションが呼び出す必要がありますも*EndOperationName*操作の結果を取得します。 戻り値*EndOperationName*は同期 web サービス メソッドによって返される、同じ型です。 たとえば、`EndGetTodoItems`メソッドのコレクションを返します`TodoItem`インスタンス。 *EndOperationName*メソッドも含まれています、`IAsyncResult`パラメーターに対応する呼び出しによって返されるインスタンスに設定する必要がありますが、 *BeginOperationName*メソッド。

タスク並列ライブラリ (TPL) は、同じ非同期操作をカプセル化して APM 開始/終了メソッドのペアを利用する場合のプロセスを簡略化できます`Task`オブジェクト。 このカプセル化が複数のオーバー ロードによって提供される、`TaskFactory.FromAsync`メソッド。

APM の詳細については、[非同期プログラミング モデル](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)と[TPL と従来の .NET Framework 非同期プログラミング](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)msdn を参照してください。

### <a name="create-the-todoserviceclient-object"></a>TodoServiceClient オブジェクトを作成します。

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

### <a name="create-data-transfer-objects"></a>データ転送オブジェクトを作成します。

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

### <a name="retrieve-data"></a>データを取得します。

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

### <a name="create-data"></a>データを作成します。

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

### <a name="delete-data"></a>データを削除します。

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

## <a name="configure-remote-access-to-iis-express"></a>IIS Express へのリモート アクセスを構成します。
Visual Studio 2017 または Visual Studio 2019 では、追加の構成なしで PC 上の UWP アプリケーションをテストできる必要があります。 Android および iOS クライアントをテストすると、このセクションでは、追加の手順が必要があります。 参照してください[iOS シミュレーターと Android のエミュレーターからローカルの Web サービスに接続する](~/cross-platform/deploy-test/connect-to-local-web-services.md)詳細についてはします。

既定では、IIS Express のみ要求に応答する`localhost`します。 リモート デバイス (Android デバイス、iPhone シミュレーターもなど) には、ローカルの WCF サービスへのアクセスはありません。 ローカル ネットワーク上、Windows 10 のワークステーションの IP アドレスを知っている必要があります。 この例では、ワークステーションに、IP アドレスがあることを想定`192.168.1.143`します。 次の手順では、リモート接続を許可し、物理または仮想デバイスからサービスに接続する Windows 10 および IIS Express を構成する方法について説明します。

1. **Windows ファイアウォールに例外を追加**します。 ご利用のサブネット上のアプリケーションが WCF サービスとの通信に使用できる Windows ファイアウォールでポートを開く必要があります。 ファイアウォールでポート 49393 を開く受信の規則を作成します。 管理コマンド プロンプトをからには、このコマンドを実行します。
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **IIS Express の構成を受け入れるリモート接続する**します。 IIS Express を構成での IIS Express の構成ファイルを編集して **[ソリューションのディレクトリ]\.vs\config\applicationhost.config**します。検索、`site`名前を持つ要素`TodoWCFService`します。 次の xml のようにする必要があります。

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

    2 つ追加する必要があります`binding`ポート 49393 外部トラフィックを Android エミュレーターの要素を開きます。 バインドを使用して、`[IP address]:[port]:[hostname]`方法、要求に応答が IIS Express を指定する形式。 外部からの要求がホスト名として指定する必要がありますが、`binding`します。 次の XML を追加、`bindings`要素、IP アドレスに置き換えて、独自の IP アドレス。

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    変更した後、`bindings`要素に次のようになります。

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
    >既定では、IIS Express は受け入れませんセキュリティ上の理由から、外部ソースからの接続。 リモート デバイスからの接続を有効にする必要があります IIS Express 管理者権限で実行します。 これを行う最も簡単な方法では、Visual Studio 2017 を管理者権限で実行します。 これは IIS Express を起動管理アクセス許可を持つ、TodoWCFService を実行している場合。

    次の手順は完了には、TodoWCFService を実行し、サブネットの他のデバイスから接続できる必要があります。 これをテストするには、アプリケーションを実行しにアクセスして`http://localhost:49393/TodoService.svc`します。 表示された場合、 **Bad Request**エラー、その URL にアクセスすると、 `bindings` IIS Express の構成 (要求は、IIS Express に達したが拒否されています) で正しくない可能性があります。 別のエラーが発生した場合、アプリケーションが実行されていないか、ファイアウォールが正しく構成されていないことがあります。

    IIS Express を実行して、サービスを提供しているを維持できるように、オフにする、**エディット コンティニュ**オプション**プロジェクトのプロパティ > Web > デバッガー**します。

1. **サービスにアクセスするデバイスを使用して、エンドポイントのカスタマイズ**します。 この手順では、クライアント アプリケーションの構成、物理またはエミュレートされたデバイスで実行されている WCF サービスにアクセスする必要があります。

    Android エミュレーターをエミュレーターが、ホスト コンピューターに直接アクセスすることを妨げる内部のプロキシを利用して`localhost`アドレス。 代わりに、アドレス`10.0.2.2`にエミュレーターでは、ルーティング`localhost`内部プロキシを介してホスト コンピューターにします。 これらのプロキシ処理された要求は必要があります。`127.0.0.1`として、ホスト名、要求ヘッダーには、これは、上記の手順でこのホスト名に対して IIS Express のバインドを作成する理由は。

    使用している場合でも、iOS シミュレーターは Mac では実行されますが、ホストを構築、[リモートの iOS シミュレーターの Windows](~/tools/ios-simulator/index.md)します。 シミュレーターからのネットワーク要求では、ホスト名とローカル ネットワーク上、ワークステーションの ip アドレスが (この例では`192.168.1.143`、実際の IP アドレスが異なる可能性がありますが)。 これは、上記の手順でこのホスト名に対して IIS Express のバインドを作成したためです。

    確認、`SoapUrl`プロパティ、 **Constants.cs** TodoWCF (ポータブル) プロジェクトのファイルがあるが、ネットワークの正しい値。

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

    構成した後、 **Constants.cs** 、適切なエンドポイントとおく必要がある物理または仮想デバイスから、Windows 10 のワークステーションで実行されている TodoWCFService に接続できません。

## <a name="related-links"></a>関連リンク

- [TodoWCF (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [方法: Windows Communication Foundation クライアントを作成します。](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel メタデータ ユーティリティ ツール (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
