---
title: Xamarin.Forms での azure SignalR サービス
description: Azure SignalR サービスと Xamarin.Forms の Azure Functions の概要します。
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: d79ffb844a65c145fd6cb22a5a3e1dfc7a6bfe41
ms.sourcegitcommit: 0f78ec17210b915b43ddab75937de8063e472c70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67418125"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Xamarin.Forms での azure SignalR サービス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureSignalR)

ASP.NET Core SignalR は、リアルタイム通信をアプリケーションに追加するプロセスを簡素化するアプリケーション モデルです。 Azure SignalR サービスは、SignalR のスケーラブルなアプリケーションの迅速な開発および展開できます。 Azure Functions は、フォーム、スケーラブルなイベント ドリブンのアプリケーションに組み合わせることができます、短時間でサーバーレス コード メソッドです。

この記事とサンプルは、接続されているクライアントにリアルタイム メッセージを配信するには、Azure SignalR サービスと Xamarin.Forms の Azure Functions を結合する方法を示します。

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Azure SignalR サービスと Azure Functions アプリを作成します。

サンプル アプリケーションは、次の 3 つの主要コンポーネントで構成されます。 Azure SignalR サービスをハブ、2 つの関数を使用して Azure Functions インスタンスおよび送信およびメッセージを受信できるモバイル アプリケーション。 これらのコンポーネントは次のようにやり取りします。

1. モバイル アプリケーションが呼び出す、**ネゴシエート**SignalR ハブに関する情報を取得する Azure 関数。
1. モバイル アプリケーションでは、ネゴシエーション情報を使用して、SignalR ハブの登録にし、接続を形成します。
1. モバイル アプリケーションにメッセージが送信、登録した後、**説明**Azure 関数。
1. **説明**関数は、SignalR ハブに受信メッセージを渡します。
1. SignalR ハブでは、元の送信者を含む、すべてのモバイル アプリケーションが接続されているインスタンスにメッセージをブロードキャストします。

> [!NOTE]
> **ネゴシエート**と**説明**サンプル アプリケーションでの関数は、Visual Studio 2019 と Azure ランタイム ツールを使用してローカルで実行できます。 ただし、Azure SignalR サービスをローカルにエミュレートすることはできませんし、テスト用の物理または仮想デバイスを Azure Functions をローカルにホストされているを公開することは困難です。 これにより、クロスプラット フォームでのテストとして、Azure 関数アプリ インスタンスを Azure Functions をデプロイすることをお勧めします。 配置の詳細については、次を参照してください。 [Visual Studio 2019 で Azure Functions のデプロイ](#deploy-azure-functions-with-visual-studio-2019)します。

### <a name="create-an-azure-signalr-service"></a>Azure SignalR サービスを作成します。

Azure SignalR サービスを選択して作成できます**リソースの作成**、Azure portal の検索の左上隅で**SignalR**します。 Azure SignalR サービスは、free レベルで作成できます。 Azure SignalR サービスがである必要があります**サーバーレス**サービスのモード。 誤って既定または従来のサービスのモードを選択した場合は、後で Azure SignalR サービスのプロパティで変更できます。

次のスクリーン ショットは、新しい Azure SignalR サービスの作成を示しています。

![Azure portal で Azure SignalR サービスのスクリーン ショットの作成](azure-signalr-images/azure-signalr-create.png "Azure SignalR サービスを作成します。")

作成されたら、**キー** Azure SignalR サービスのセクションには、**接続文字列**、SignalR ハブに、Azure 関数アプリの接続に使用します。 次のスクリーン ショットでは、Azure SignalR サービスの接続文字列を検索する場所を示します。

![Azure portal で Azure SignalR のスクリーン ショットの接続文字列](azure-signalr-images/azure-signalr-connection-string.png "Azure SignalR 接続文字列")

この接続文字列を使用する[Visual Studio 2019 で Azure Functions のデプロイ](#deploy-azure-functions-with-visual-studio-2019)します。

### <a name="create-an-azure-functions-app"></a>Azure Functions アプリを作成します。

サンプル アプリケーションをテストするには、Azure portal で新しい Azure 関数アプリを作成する必要があります。 書き留めて、**アプリ名**サンプル アプリケーションで使用すると、この URL **Constants.cs**ファイル。 次のスクリーン ショットは、"xdocsfunctions"と呼ばれる新しい Azure 関数アプリの作成を示しています。

[![Azure 関数アプリのスクリーン ショットの作成](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Azure 関数は、Visual Studio 2019 から Azure 関数アプリ インスタンスにデプロイできます。 次のセクションでは、Azure Functions アプリのインスタンスにサンプル アプリケーションで 2 つの関数の展開について説明します。

### <a name="build-azure-functions-in-visual-studio-2019"></a>Visual Studio 2019 で Azure Functions を作成します。

サンプル アプリケーションにはと呼ばれるクラス ライブラリが含まれています**ChatServer**と呼ばれるファイルの 2 つのサーバーレス Azure 関数を含む**Negotiate.cs**と**Talk.cs**します。

`Negotiate`関数は web 要求に応答を`SignalRConnectionInfo`オブジェクトを含む、`AccessToken`プロパティと`Url`プロパティ。 モバイル アプリケーションでは、これらの値を使用して、SignalR ハブに登録します。 次のコードは、`Negotiate`関数。

```csharp
[FunctionName("Negotiate")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous,"get",Route = "negotiate")]
    HttpRequest req,
    [SignalRConnectionInfo(HubName = "simplechat")]
    SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

`Talk`関数は POST の本文内のメッセージ オブジェクトを提供する HTTP POST 要求に応答します。 POST の本文に変換されますが、 `SignalRMessage` SignalR hub に転送します。 次のコードは、`Talk`関数。

```csharp
[FunctionName("Talk")]
public static async Task<IActionResult> Run(
    [HttpTrigger(
        AuthorizationLevel.Anonymous,
        "post",
        Route = "talk")]
    HttpRequest req,
    [SignalR(HubName = "simplechat")]
    IAsyncCollector<SignalRMessage> questionR,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    try
    {
        string json = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic obj = JsonConvert.DeserializeObject(json);

        var name = obj.name.ToString();
        var text = obj.text.ToString();

        var jObject = new JObject(obj);

        await questionR.AddAsync(
            new SignalRMessage
            {
                Target = "newMessage",
                Arguments = new[] { jObject }
            });

        return new OkObjectResult($"Hello {name}, your message was '{text}'");
    }
    catch (Exception ex)
    {
        return new BadRequestObjectResult("There was an error: " + ex.Message);
    }
}
```

Azure functions と Azure Functions アプリの詳細については、次を参照してください。 [Azure Functions のドキュメント](/azure/azure-functions/)します。

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Visual Studio 2019 で Azure Functions をデプロイします。

Visual Studio 2019 関数を Azure Functions アプリにデプロイすることができます。 Azure でホストされる関数では、クロス プラットフォームのすべてのデバイスのアクセス可能なテストのエンドポイントを提供することによってテストが容易になります。

サンプルの関数アプリを右クリックして**発行**関数を Azure Functions アプリに発行する ダイアログを起動します。 Azure Function App をセットアップする前の手順を実行する場合、選択できます**既存の**Azure 関数アプリにサンプル アプリケーションを公開します。 次のスクリーン ショットでは、Visual Studio 2019 で、発行ダイアログ オプションを示します。

![Visual Studio 2019 の発行の選択ダイアログ](azure-signalr-images/vs-publish-target.png "Visual Studio 2019 でオプションのパブリッシュ")

Microsoft アカウントにサインインするとは、検索し、発行先として Azure 関数アプリを選択できます。 次のスクリーン ショットでは、Visual Studio 2019 の公開ダイアログで Azure Functions アプリの使用例を示しています。

![Visual Studio 2019 の公開ダイアログで、Azure Functions アプリ](azure-signalr-images/vs-app-selection.png "Visual Studio 2019 の公開ダイアログで Azure 関数アプリ")

Azure 関数アプリを選択した後、インスタンス、サイトの URL、構成、および Azure Functions アプリのターゲットに関するその他の情報が表示されます。 選択**Azure App Service の設定の編集**で接続文字列を入力し、**リモート**フィールド。 接続文字列を使って、**ネゴシエート**と**説明**Azure SignalR サービスに接続する関数し、記載されて、**キー** Azure SignalR のセクションAzure portal のサービスです。 接続文字列の詳細については、次を参照してください。 [Azure SignalR サービスの作成](#create-an-azure-signalr-service)です。

接続文字列を入力した後は、クリックして**発行**関数を Azure Functions アプリにデプロイします。 完了すると、関数は、Azure portal で Azure 関数アプリで表示されます。 次のスクリーン ショットでは、Azure portal で公開された関数を示しています。

![関数は、Azure 関数アプリで公開されている](azure-signalr-images/azure-functions-deployed.png "関数は、Azure 関数アプリで公開されています。")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Xamarin.Forms での Azure SignalR サービスを統合します。

Azure SignalR サービスと、Xamarin.Forms アプリケーションの統合は、SignalR サービス クラスでインスタンス化される、`MainPage`の 3 つのイベントに割り当てられているイベント ハンドラー クラス。 これらのイベント ハンドラーの詳細については、次を参照してください。 [SignalR サービス クラスを使用して、Xamarin.Forms で](#use-the-signalr-service-class-in-xamarinforms)します。

サンプル アプリケーションが含まれています、 **Constants.cs**クラス Azure Functions アプリの URL エンドポイントをカスタマイズする必要があります。 値を設定、`HostName`プロパティを Azure Functions アプリのアドレス。 次のコードは、 **Constants.cs**例を使用してプロパティ`HostName`値。

```csharp
public static class Constants
{
    public static string HostName { get; set; } = "https://example-functions-app.azurewebsites.net/";

    // Used to differentiate message types sent via SignalR. This
    // sample only uses a single message type.
    public static string MessageName { get; set; } = "newMessage";

    public static string Username
    {
        get
        {
            return $"{Device.RuntimePlatform} User";
        }
    }
}
```

> [!NOTE]
> `Username`サンプル アプリケーションでプロパティ**Constants.cs**ファイルは、デバイスの`RuntimePlatform`ユーザー名と値。 これにより、簡単にクロスプラット フォーム デバイスでのテストし、メッセージを送信するデバイスを識別できます。 実際のアプリケーションでは、この値は一意のユーザー名と言えるでしょうをサインアップ時に収集されたまたは、プロセスにサインインします。

### <a name="the-signalr-service-class"></a>SignalR サービス クラス

`SignalRService`クラス、 **ChatClient**サンプル アプリケーションでプロジェクトを Azure SignalR サービスに接続するための Azure 関数アプリ内の関数を呼び出す実装を示しています。

`SendMessageAsync`メソッドで、 `SignalRService` Azure SignalR サービスに接続されているクライアントにメッセージを送信するクラスを使用します。 このメソッドに HTTP POST 要求を実行する、**説明**JSON シリアル化を含めて、Azure 関数アプリでホストされている関数`Message`POST ペイロードとしてのオブジェクト。 **説明**関数は、クライアントが接続されているすべてのブロードキャストの Azure SignalR サービスにメッセージを渡します。 `SendMessageAsync` メソッドのコードを次に示します。

```csharp
public async Task SendMessageAsync(string username, string message)
{
    IsBusy = true;

    var newMessage = new Message
    {
        Name = username,
        Text = message
    };

    var json = JsonConvert.SerializeObject(newMessage);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    var result = await client.PostAsync($"{Constants.HostName}/api/talk", content);

    IsBusy = false;
}
```

`ConnectAsync`メソッドで、`SignalRService`クラスは、HTTP GET 要求を実行、**ネゴシエート**Azure 関数アプリでホストされている関数。 **ネゴシエート**関数がのインスタンスに逆シリアル化は、JSON を返す、`NegotiateInfo`クラス。 1 回、`NegotiateInfo`オブジェクトを取得のインスタンスを使用して Azure SignalR サービスと直接登録するために、`HubConnection`クラス。

ASP.NET Core SignalR は、メッセージに、開いている接続からの受信データを変換し、メッセージの種類を定義し、着信メッセージの種類によってイベント ハンドラーをバインドできます。 `ConnectAsync`メソッドは、サンプル アプリケーションので定義されたメッセージの名前のイベント ハンドラーを登録します。 **Constants.cs**ファイル。 これは既定では、"新しいメッセージ"。

`ConnectAsync` メソッドのコードを次に示します。

```csharp
public async Task ConnectAsync()
{
    try
    {
        IsBusy = true;

        string negotiateJson = await client.GetStringAsync($"{Constants.HostName}/api/negotiate");
        NegotiateInfo negotiate = JsonConvert.DeserializeObject<NegotiateInfo>(negotiateJson);
        HubConnection connection = new HubConnectionBuilder()
            .WithUrl(negotiate.Url, options =>
            {
                options.AccessTokenProvider = async () => negotiate.AccessToken;
            })
            .Build();

        connection.On<JObject>(Constants.MessageName, AddNewMessage);
        await connection.StartAsync();

        IsConnected = true;
        IsBusy = false;

        Connected?.Invoke(this, true, "Connection successful.");
    }
    catch (Exception ex)
    {
        ConnectionFailed?.Invoke(this, false, ex.Message);
    }
}
```

`AddNewMessage`メソッドがイベント ハンドラーとしてバインドされている、`ConnectAsync`メッセージが上記のコードで表示されます。 メッセージが受信したときに、`AddNewMessage`として提供されるメッセージ データ メソッドを呼び出すと、`JObject`します。 `AddNewMessage`メソッドに変換、`JObject`のインスタンスに、`Message`クラスし、のハンドラーが呼び出され、`NewMessageReceived`バインドされている場合。 `AddNewMessage` メソッドのコードを次に示します。

```csharp
public void AddNewMessage(JObject message)
{
    Message messageModel = new Message
    {
        Name = message.GetValue("name").ToString(),
        Text = message.GetValue("text").ToString(),
        TimeReceived = DateTime.Now
    };

    NewMessageReceived?.Invoke(this, messageModel);
}
```

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>Xamarin.Forms で SignalR サービス クラスを使用します。

Xamarin.Forms で SignalR サービス クラスを利用するには、バインド、`SignalRService`内のイベントのクラス、`MainPage`分離コード クラス。

`Connected`内のイベント、 `SignalRService` SignalR 接続が正常に完了したときに、クラスが発生します。 `ConnectionFailed`内のイベント、`SignalRService`クラスは、SignalR 接続が失敗したときに発生します。 `SignalR_ConnectionChanged`イベント ハンドラー メソッドが両方のイベントにバインドされている、`MainPage`コンス トラクター。 このイベント ハンドラー、接続に基づく接続と送信ボタンの状態を更新する`success`引数、チャットのキューを使用するイベントによって提供されるメッセージを追加し、`AddMessage`メソッド。 次のコードは、`SignalR_ConnectionChanged`イベント ハンドラー メソッド。

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

`NewMessageReceived`内のイベント、`SignalRService`クラスは、Azure SignalR サービスから新しいメッセージが受信したときに発生します。 `SignalR_NewMessageReceived`にイベント ハンドラー メソッドがバインドされている、`NewMessageReceived`内のイベント、`MainPage`コンス トラクター。 このイベント ハンドラー、受信を変換する`Message`オブジェクトを文字列にし、それを使用して、チャット キューに追加、`AddMessage`メソッド。 次のコードは、`SignalR_NewMessageReceived`イベント ハンドラー メソッド。

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

`AddMessage`メソッドとして、新しいメッセージの追加、`Label`チャット キューにオブジェクト。 `AddMessage`多くの場合、メソッドは、メイン UI スレッドの外部からのイベント ハンドラーによって例外を回避するメイン スレッドで発生する UI の更新を強制するようにします。 `AddMessage` メソッドのコードを次に示します。

```csharp
void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        Label label = new Label
        {
            Text = message,
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        };

        messageList.Children.Add(label);
    });
}
```

## <a name="test-the-application"></a>アプリケーションをテストする

ある場合に、iOS、Android、UWP で SignalR チャット アプリケーションをテストできます。

1. Azure SignalR サービスを作成します。
1. Azure Functions アプリを作成します。
1. カスタマイズされた、 **Constants.cs** Azure Functions アプリのエンドポイントを持つファイル。

これらの手順を完了し、クリックすると、アプリケーションを実行すると、 **Connect** Azure SignalR サービスとの接続をボタン フォーム。 クリックして、メッセージを入力して、**送信**チャットのキューにいずれかに表示されるメッセージで結果をボタンには、モバイル アプリケーションが接続されています。

## <a name="related-links"></a>関連リンク

* [Xamarin と SignalR によるリアルタイムのモバイル アプリの作成](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [SignalR 入門](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Azure Functions の紹介](/azure/azure-functions/functions-overview)
* [Azure Functions のドキュメント](/azure/azure-functions/)
* [MVVM の SignalR チャットのサンプル](https://github.com/lbugnion/sample-xamarin-signalr)
