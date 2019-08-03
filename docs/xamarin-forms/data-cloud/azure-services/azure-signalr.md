---
title: Azure SignalR Service と Xamarin. フォーム
description: Azure SignalR Service の概要と Xamarin. フォームを使用した Azure Functions
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: a4d0f5c5ceefcfe9a36a5fcf10c6fb4937c1db90
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739209"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Azure SignalR Service と Xamarin. フォーム

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresignalr/)

ASP.NET Core SignalR は、リアルタイム通信をアプリケーションに追加するプロセスを簡略化するアプリケーションモデルです。 Azure SignalR サービスを使用すると、スケーラブルな SignalR アプリケーションを迅速に開発およびデプロイできます。 Azure Functions は、イベントドリブンでスケーラブルなアプリケーションを形成するために組み合わせることができる、有効期間が短く、サーバーレスのコードメソッドです。

この記事とサンプルでは、Azure SignalR Service と Azure Functions を Xamarin. Forms と組み合わせて、接続されたクライアントにリアルタイムメッセージを配信する方法を示します。

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Azure SignalR サービスと Azure Functions アプリを作成する

このサンプルアプリケーションは、3つの主要なコンポーネントで構成されています。 Azure SignalR Service hub、2つの機能を持つ Azure Functions インスタンス、およびメッセージを送受信できるモバイルアプリケーションです。 これらのコンポーネントは次のように動作します。

1. モバイルアプリケーションは、SignalR ハブに関する情報を取得するために、 **Negotiate** Azure 関数を呼び出します。
1. モバイルアプリケーションは、ネゴシエーション情報を使用して自身を SignalR hub に登録し、接続を形成します。
1. 登録後、モバイルアプリケーションは、Azure の**トーク**関数にメッセージを送信します。
1. **Talk**関数は、受信メッセージを SignalR hub に渡します。
1. SignalR hub は、接続されているすべてのモバイルアプリケーションインスタンス (元の送信者を含む) にメッセージをブロードキャストします。

> [!NOTE]
> サンプルアプリケーションの**Negotiate**および**Talk**関数は、Visual Studio 2019 と Azure ランタイムツールを使用してローカルで実行できます。 ただし、Azure SignalR サービスをローカルにエミュレートすることはできません。ローカルでホストされている Azure Functions をテストのために物理または仮想デバイスに公開するのは困難です。 Azure Functions を Azure Functions アプリインスタンスにデプロイすることをお勧めします。これにより、クロスプラットフォームテストが可能になります。 デプロイの詳細については、「 [Visual Studio 2019 での Azure Functions のデプロイ](#deploy-azure-functions-with-visual-studio-2019)」を参照してください。

### <a name="create-an-azure-signalr-service"></a>Azure SignalR サービスを作成する

Azure portal の左上隅にある **[リソースの作成]** を選択し、 **SignalR**を検索することで、Azure SignalR サービスを作成できます。 Azure SignalR サービスは、free レベルで作成できます。 Azure SignalR サービスは、**サーバーレス**サービスモードである必要があります。 既定または従来のサービスモードを誤って選択した場合は、後で Azure SignalR サービスのプロパティで変更できます。

次のスクリーンショットは、新しい Azure SignalR サービスの作成を示しています。

![Azure portal での Azure SignalR Service の作成のスクリーンショット](azure-signalr-images/azure-signalr-create.png "Azure SignalR サービスの作成")

Azure SignalR サービスの**キー**セクションには、作成された**接続文字列**が含まれています。これは、Azure Functions アプリを SignalR hub に接続するために使用されます。 次のスクリーンショットは、Azure SignalR サービスで接続文字列を見つける場所を示しています。

![Azure portal の Azure SignalR 接続文字列のスクリーンショット](azure-signalr-images/azure-signalr-connection-string.png "Azure SignalR 接続文字列")

この接続文字列は、 [Visual Studio 2019 を使用して Azure Functions をデプロイ](#deploy-azure-functions-with-visual-studio-2019)するために使用されます。

### <a name="create-an-azure-functions-app"></a>Azure Functions アプリを作成する

サンプルアプリケーションをテストするには、Azure portal で新しい Azure Functions アプリを作成する必要があります。 この URL はサンプルアプリケーション**Constants.cs**ファイルで使用されるため、**アプリ名**をメモしておきます。 次のスクリーンショットは、"xdocsfunctions" という新しい Azure Functions アプリの作成を示しています。

[![Azure Functions アプリの作成のスクリーンショット](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Azure functions は、Visual Studio 2019 から Azure Functions アプリインスタンスにデプロイできます。 次のセクションでは、サンプルアプリケーションの2つの関数を Azure Functions アプリインスタンスに展開する方法について説明します。

### <a name="build-azure-functions-in-visual-studio-2019"></a>Visual Studio 2019 でのビルド Azure Functions

サンプルアプリケーションには、**チャットサーバー**と呼ばれるクラスライブラリが含まれています。これには、 **Negotiate.cs**と**Talk.cs**という名前のファイルに2つのサーバーレス Azure Functions が含まれます。

関数`Negotiate`は`AccessToken` 、プロパティと`Url`プロパティを含む`SignalRConnectionInfo`オブジェクトを使用して、web 要求に応答します。 モバイルアプリケーションは、これらの値を使用して自身を SignalR hub に登録します。 次のコードは、 `Negotiate`関数を示しています。

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

関数`Talk`は、ポスト本文にメッセージオブジェクトを提供する HTTP POST 要求に応答します。 ポスト本文はに変換`SignalRMessage`され、SignalR hub に転送されます。 次のコードは、 `Talk`関数を示しています。

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

Azure functions と Azure Functions アプリの詳細については、 [Azure Functions のドキュメント](/azure/azure-functions/)を参照してください。

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Visual Studio 2019 で Azure Functions をデプロイする

Visual Studio 2019 では、Azure Functions アプリに関数をデプロイできます。 Azure でホストされる関数は、すべてのデバイスにアクセス可能なテストエンドポイントを提供することで、クロスプラットフォームのテストを容易にします。

Sample functions アプリを右クリックして **[発行]** を選択すると、ダイアログが起動し、Azure Functions アプリに関数が発行されます。 前の手順に従って Azure Function App を設定した場合は、 **[既存のものを選択]** を選択して、サンプルアプリケーションを Azure Functions アプリに発行できます。 次のスクリーンショットは、Visual Studio 2019 の [発行] ダイアログオプションを示しています。

![Visual Studio 2019 の [オプションの発行] ダイアログ](azure-signalr-images/vs-publish-target.png "Visual Studio 2019 の発行オプション")

Microsoft アカウントにサインインすると、発行先として Azure Functions アプリを検索して選択できます。 次のスクリーンショットは、Visual Studio 2019 の発行ダイアログでの Azure Functions アプリの例を示しています。

![Visual Studio 2019 の [発行] ダイアログの Azure Functions アプリ](azure-signalr-images/vs-app-selection.png "Visual Studio 2019 の [発行] ダイアログの Azure Functions アプリ")

Azure Functions アプリインスタンスを選択すると、ターゲット Azure Functions アプリに関するサイトの URL、構成、およびその他の情報が表示されます。 **[Azure App Service 設定の編集]** を選択し、 **[リモート]** フィールドに接続文字列を入力します。 この接続文字列は、Azure SignalR サービスに接続するために**Negotiate**関数と**Talk**関数によって使用され、Azure portal の Azure SignalR サービスの**キー**セクションで使用できます。 接続文字列の詳細については、「 [Azure SignalR サービスの作成](#create-an-azure-signalr-service)」を参照してください。

接続文字列を入力したら、 **[発行]** をクリックして、関数を Azure Functions アプリにデプロイできます。 完了すると、関数が Azure portal の Azure Functions アプリに一覧表示されます。 次のスクリーンショットは、Azure portal で公開されている関数を示しています。

![Azure Functions アプリで公開された関数](azure-signalr-images/azure-functions-deployed.png "Azure Functions アプリで公開された関数")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Azure SignalR Service と Xamarin. Forms の統合

Azure SignalR service と Xamarin. Forms アプリケーションの統合は、3つのイベントに割り当てられたイベント`MainPage`ハンドラーを使用してクラスでインスタンス化される SignalR サービスクラスです。 これらのイベントハンドラーの詳細については、「 [Xamarin. Forms での SignalR service クラスの使用](#use-the-signalr-service-class-in-xamarinforms)」を参照してください。

サンプルアプリケーションには、Azure Functions アプリの URL エンドポイントでカスタマイズする必要がある**Constants.cs**クラスが含まれています。 `HostName`プロパティの値を Azure Functions アプリのアドレスに設定します。 次のコードは、 **Constants.cs**プロパティと値の`HostName`例を示しています。

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
> サンプル`Username`アプリケーションの**Constants.cs**ファイルのプロパティは、デバイスの`RuntimePlatform`値をユーザー名として使用します。 これにより、デバイスをクロスプラットフォームでテストし、どのデバイスがメッセージを送信しているかを識別しやすくなります。 実際のアプリケーションでは、この値は、サインアップまたはサインインプロセス中に収集される一意のユーザー名になる可能性があります。

### <a name="the-signalr-service-class"></a>SignalR service クラス

サンプル`SignalRService`アプリケーションの**チャットクライアント**プロジェクトのクラスは、Azure Functions アプリ内の関数を呼び出して Azure SignalR サービスに接続する実装を示しています。

`SignalRService`クラスのメソッドは、Azure SignalR サービスに接続されているクライアントにメッセージを送信するために使用されます。 `SendMessageAsync` このメソッドは、JSON でシリアル化`Message`されたオブジェクトをポストペイロードとして含む、Azure Functions アプリでホストされている**トーク**関数に対して HTTP POST 要求を実行します。 **Talk**関数は、接続されているすべてのクライアントにブロードキャストするために、メッセージを Azure SignalR サービスに渡します。 `SendMessageAsync` メソッドのコードを次に示します。

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

`SignalRService`クラス`ConnectAsync`のメソッドは、Azure Functions アプリでホストされている**Negotiate**関数に対して HTTP GET 要求を実行します。 **Negotiate**関数は、 `NegotiateInfo`クラスのインスタンスに逆シリアル化された JSON を返します。 オブジェクトが取得されると、 `HubConnection`クラスのインスタンスを使用して Azure SignalR サービスに直接登録するために使用されます。 `NegotiateInfo`

ASP.NET Core SignalR は、開いている接続から受信したデータをメッセージに変換し、開発者がメッセージの種類を定義し、イベントハンドラーを種類別に受信メッセージにバインドできるようにします。 メソッド`ConnectAsync`は、サンプルアプリケーションの**Constants.cs**ファイルで定義されているメッセージ名のイベントハンドラーを登録します。これは、既定では "newmessage" です。

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

メソッドは、前のコードに示すように`ConnectAsync` 、メッセージのイベントハンドラーとしてバインドされます。 `AddNewMessage` メッセージを受信`AddNewMessage`すると、 `JObject`として提供されたメッセージデータを使用してメソッドが呼び出されます。 メソッド`AddNewMessage`は、を`JObject` `Message`クラスのインスタンスに変換し、バインドされている`NewMessageReceived`場合は、のハンドラーを呼び出します。 `AddNewMessage` メソッドのコードを次に示します。

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

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>SignalR のサービスクラスを使用する

SignalR サービスクラスを Xamarin で使用することによって、クラス`SignalRService`のイベントを分離`MainPage`コードクラスにバインドすることができます。

`SignalRService`クラスのイベントは、SignalR 接続が正常に完了したときに発生します。 `Connected` `SignalRService`クラスのイベントは、SignalR 接続が失敗したときに発生します。 `ConnectionFailed` イベントハンドラーメソッドは、 `MainPage`コンストラクター内の両方のイベントにバインドされます。 `SignalR_ConnectionChanged` このイベントハンドラーは、接続`success`引数に基づいて [接続] ボタンと [送信] ボタンの状態を更新し、 `AddMessage`メソッドを使用して、イベントによって提供されるメッセージをチャットキューに追加します。 次のコードは、 `SignalR_ConnectionChanged`イベントハンドラーメソッドを示しています。

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

`SignalRService`クラスのイベントは、Azure SignalR サービスから新しいメッセージを受信したときに発生します。 `NewMessageReceived` イベントハンドラーメソッドは、 `MainPage`コンストラクター内の`NewMessageReceived`イベントにバインドされます。 `SignalR_NewMessageReceived` このイベントハンドラーは、受信`Message`したオブジェクトを文字列に変換し、 `AddMessage`メソッドを使用してそれをチャットキューに追加します。 次のコードは、 `SignalR_NewMessageReceived`イベントハンドラーメソッドを示しています。

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

メソッド`AddMessage`は、新しいメッセージを`Label`オブジェクトとしてチャットキューに追加します。 `AddMessage`メソッドは、メイン ui スレッドの外部からイベントハンドラーによって呼び出されることが多いため、メインスレッドで UI 更新が強制的に実行され、例外が発生しないようにします。 `AddMessage` メソッドのコードを次に示します。

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

SignalR chat アプリケーションは、次のような iOS、Android、UWP でテストできます。

1. Azure SignalR サービスを作成しました。
1. Azure Functions アプリを作成しました。
1. Azure Functions アプリエンドポイントを使用して**Constants.cs**ファイルをカスタマイズします。

これらの手順が完了し、アプリケーションが実行されたら、 **[接続]** ボタンをクリックすると、Azure SignalR サービスとの接続が形成されます。 メッセージを入力して **[送信]** ボタンをクリックすると、接続されているモバイルアプリケーションのチャットキューにメッセージが表示されます。

## <a name="related-links"></a>関連リンク

* [Xamarin と SignalR を使用したリアルタイムモバイルアプリの構築](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [SignalR 入門](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Azure Functions の紹介](/azure/azure-functions/functions-overview)
* [Azure Functions のドキュメント](/azure/azure-functions/)
* [MVVM SignalR chat のサンプル](https://github.com/lbugnion/sample-xamarin-signalr)
