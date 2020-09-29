---
title: リモートプロセスでの Android サービスの実行
description: 一般に、Android アプリケーションのすべてのコンポーネントは同じプロセスで実行されます。 Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。 このガイドでは、Xamarin を使用して Android リモートサービスを作成および使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 14db376fbb40575cc3b11c5b23f0af6d32370503
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455335"
---
# <a name="running-android-services-in-remote-processes"></a>リモートプロセスでの Android サービスの実行

_一般に、Android アプリケーションのすべてのコンポーネントは同じプロセスで実行されます。Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。このガイドでは、Xamarin を使用して Android リモートサービスを作成および使用する方法について説明します。_

## <a name="out-of-process-services-overview"></a>アウトプロセスサービスの概要

アプリケーションの起動時に、Android によって、アプリケーションを実行するプロセスが作成されます。 通常、アプリケーションがこの1つのプロセスで実行するすべてのコンポーネントです。 Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。 これらの種類のサービスは、 _リモートサービス_ または _アウトプロセスサービス_と呼ばれます。 これらのサービスのコードは、メインアプリケーションと同じ APK に含まれます。ただし、サービスが開始されると、Android はそのサービスに対してのみ新しいプロセスを作成します。 これに対し、アプリケーションの他の部分と同じプロセスで実行されるサービスは、 _ローカルサービス_と呼ばれることもあります。

一般に、アプリケーションでリモートサービスを実装する必要はありません。 多くの場合、アプリの要件にはローカルサービスが十分 (かつ望ましい) あります。 アウトプロセスには、Android で管理する必要がある独自のメモリ空間があります。 これにより、アプリケーション全体のオーバーヘッドが増加しますが、独自のプロセスでサービスを実行すると便利なシナリオがいくつかあります。

1. **共有機能** &ndash; アプリケーション開発者によっては、すべてのアプリケーション間で共有される複数のアプリと機能を持つ場合があります。 独自のプロセスで実行される Android サービスにその機能をパッケージ化すると、アプリケーションの保守が簡単になります。 また、独自のスタンドアロン APK でサービスをパッケージ化し、アプリケーションの他の部分とは別にデプロイすることもできます。
2. **ユーザーエクスペリエンス** &ndash; の向上アウトプロセスサービスがアプリケーションのユーザーエクスペリエンスを向上させる方法は2つあります。 最初の方法では、メモリ管理について扱います。 ガベージコレクション (GC) サイクルが発生すると、Android は GC が完了するまでプロセス内のすべてのアクティビティを一時停止します。 ユーザーは、この一時停止を "途切れている" または "jank" と感じることがあります。 サービスが独自のプロセスで実行されている場合は、アプリケーションプロセスではなく、一時停止されているサービスプロセスです。 この一時停止は、アプリケーションプロセス (したがってユーザーインターフェイス) が一時停止していないため、ユーザーにとってあまりわかりにくくなります。

    また、プロセスのメモリ要件が大きすぎると、Android はそのプロセスを終了してデバイスのリソースを解放する可能性があります。 サービスのメモリフットプリントが大きく、UI と同じプロセスで実行されている場合、Android がそれらのリソースを強制的に再利用すると、UI がシャットダウンされ、ユーザーにアプリの起動が強制されます。 ただし、独自のプロセスで実行されているサービスが Android によってシャットダウンされた場合、UI プロセスは影響を受けません。 UI では、サービスをバインド (および再起動) し、ユーザーに対して透過的に実行し、通常の動作を再開することができます。

3. **アプリケーションのパフォーマンス** &ndash; の向上UI プロセスは、サービスプロセスとは無関係に終了またはシャットダウンされる場合があります。 長いスタートアップタスクをアウトプロセスサービスに移動することで、UI の起動時間が短縮される可能性があります (これは、サービスプロセスが UI が起動されたときの時間の間に維持されることを前提としています)。

多くの点で、別のプロセスで実行されているサービスへのバインドは、 [ローカルサービスへのバインド](~/android/app-fundamentals/services/creating-a-service/bound-services.md)と同じです。 クライアントはを呼び出して、 `BindService` サービスをバインドします (必要に応じて開始します)。 `Android.OS.IServiceConnection`クライアントとサービスの間の接続を管理するオブジェクトが作成されます。 クライアントがサービスに正常にバインドした場合、Android はを介してオブジェクトを返します。このオブジェクトを使用して、 `IServiceConnection` サービスでメソッドを呼び出すことができます。 クライアントは、このオブジェクトを使用してサービスと対話します。 確認するには、サービスにバインドする手順を次に示します。

- インテントを作成する**Create an Intent** &ndash;サービスにバインドするには、明示的なインテントを使用する必要があります。
- オブジェクトの実装**と `IServiceConnection` インスタンス化** &ndash;オブジェクトは、 `IServiceConnection` クライアントとサービスの間の仲介役として機能します。  クライアントとサーバー間の接続を監視する役割があります。
- ** `BindService` メソッド** &ndash; を呼び出すを呼び出す `BindService` と、前の手順で作成したインテントとサービス接続が Android にディスパッチされます。これにより、サービスの開始とクライアントとサービス間の通信の確立が行われます。

プロセス間の境界を越える必要があるため、複雑さが増します。通信は一方向 (クライアントからサーバー) であり、クライアントはサービスクラスのメソッドを直接呼び出すことができません。 サービスがクライアントと同じプロセスを実行している場合、Android では、 `IBinder` 双方向の通信を可能にするオブジェクトが提供されることに注意してください。 これは、サービスが独自のプロセスで実行されている場合には当てはまりません。 クライアントは、クラスのヘルプを使用してリモートサービスと通信し `Android.OS.Messenger` ます。

クライアントがリモートサービスとのバインドを要求すると、Android は `Service.OnBind` ライフサイクルメソッドを呼び出します。このメソッドは `IBinder` 、によってカプセル化された内部オブジェクトを返し `Messenger` ます。 は、 `Messenger` `IBinder` Android SDK によって提供される特殊な実装に対するシンラッパーです。 は、 `Messenger` 2 つの異なるプロセス間の通信を行います。 開発者は、メッセージのシリアル化、プロセス境界を越えたメッセージのマーシャリング、およびクライアントでの逆シリアル化の詳細について心配していません。 この作業は、オブジェクトによって処理され `Messenger` ます。 次の図は、クライアントがアウトプロセスサービスへのバインドを開始するときに関連するクライアント側の Android コンポーネントを示しています。

![サービスにバインドするクライアントの手順とコンポーネントを示す図](out-of-process-services-images/ipc-01.png "サービスにバインドするクライアントの手順とコンポーネントを示す図。")

`Service`リモートプロセスのクラスは、ローカルプロセス内のバインドされたサービスを通過する同じライフサイクルコールバックを通過します。また、関連する api の多くは同じです。 `Service.OnCreate` は、を初期化 `Handler` し、それをオブジェクトに挿入するために使用され `Messenger` ます。 同様に、 `OnBind` はオーバーライドされますが、オブジェクトを返す代わりに `IBinder` 、サービスはを返し `Messenger` ます。  次の図は、クライアントがサービスにバインドされている場合のサービスの動作を示しています。

![リモートクライアントによってバインドされるときにサービスが実行する手順とコンポーネントを示す図](out-of-process-services-images/ipc-02.png "リモートクライアントによってバインドされるときにサービスが実行する手順とコンポーネントを示す図。")

`Message`がサービスによって受信されると、のインスタンスのによって処理され `Android.OS.Handler` ます。 サービスは、 `Handler` メソッドをオーバーライドする必要がある独自のサブクラスを実装し `HandleMessage` ます。 このメソッドは、によって呼び出され、を `Messenger` `Message` パラメーターとして受け取ります。 は `Handler` `Message` メタデータを検査し、その情報を使用してサービスでメソッドを呼び出します。

一方向の通信は、クライアントがオブジェクトを作成 `Message` し、メソッドを使用してそのオブジェクトをサービスにディスパッチするときに発生し `Messenger.Send` ます。 `Messenger.Send` は、 `Message` シリアル化されたデータをから Android にシリアル化して、プロセスの境界を越えてサービスにメッセージをルーティングします。  `Messenger`サービスによってホストされているは、 `Message` 受信データからオブジェクトを作成します。 これ `Message` はキューに格納され、メッセージはに一度に1つずつ送信され `Handler` ます。 は、に `Handler` 含まれるメタデータを検査 `Message` し、で適切なメソッドを呼び出し `Service` ます。 次の図は、これらのさまざまな動作について説明しています。

![プロセス間でメッセージが渡される方法を示す図](out-of-process-services-images/ipc-03.png "プロセス間でメッセージがどのように渡されるかを示す図")

このガイドでは、アウトプロセスサービスの実装の詳細について説明します。 独自のプロセスで実行することを意図したサービスを実装する方法と、フレームワークを使用してクライアントがそのサービスと通信する方法について説明し `Messenger` ます。 また、双方向の通信についても簡単に説明します。クライアントは、サービスにメッセージを送信し、サービスはクライアントにメッセージを返信します。 サービスは異なるアプリケーション間で共有できるため、このガイドでは、Android のアクセス許可を使用してサービスへのクライアントアクセスを制限する方法の1つについても説明します。

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950-分離プロセスおよびカスタムアプリケーションクラスを使用したサービスは、オーバーロードを正しく解決でき](https://github.com/xamarin/xamarin-android/issues/1950) ません `IsolatedProcess` 。がに設定されている場合、Xamarin. Android サービスが正しく起動しないことを報告し `true` ます。 このガイドは、リファレンスのために用意されています。 Xamarin Android アプリケーションは、Java で記述されたアウトプロセスサービスと引き続き通信できなければなりません。

## <a name="requirements"></a>必要条件

このガイドは、サービスの作成に関する知識を前提としています。

以前の Android Api を対象とするアプリでは、暗黙的なインテントを使用することもできますが、このガイドでは明示的なインテントの使用についてのみ説明します。 Android 5.0 (API レベル 21) 以降を対象とするアプリでは、サービスとのバインドに明示的なインテントを使用する必要があります。これは、このガイドで説明されている手法です。「」を参照してください。

## <a name="create-a-service-that-runs-in-a-separate-process"></a>別のプロセスで実行されるサービスを作成する

前述のように、サービスが独自のプロセスで実行されているという事実は、いくつかの異なる Api が関係していることを意味します。 簡単な概要として、リモートサービスにバインドして使用する手順を次に示します。  

- ** `Service` サブクラス** &ndash; を作成する型をサブクラス化 `Service` し、バインドされたサービスのライフサイクルメソッドを実装します。 また、サービスが独自のプロセスで実行されることを Android に通知するメタデータを設定する必要もあります。
- **を実装 `Handler` する**&ndash; `Handler` は、クライアント要求を分析し、クライアントから渡されたパラメーターを抽出し、サービスで適切なメソッドを呼び出す役割を担います。
- **をインスタンス `Messenger` 化する**&ndash;前に説明したように、各は、 `Service` `Messenger` 前の手順で作成したにクライアント要求をルーティングするクラスのインスタンスを維持する必要があり `Handler` ます。

独自のプロセスで実行することを意図したサービスは、基本的にはバインドされたサービスです。 サービスクラスは、基本クラスを拡張し、android `Service` `ServiceAttribute` マニフェストでバンドルする必要のあるメタデータを含むで修飾されます。 まず、アウトプロセスサービスにとって重要なの次のプロパティを `ServiceAttribute` 示します。

1. `Exported`&ndash; `true` 他のアプリケーションがサービスと対話できるようにするには、このプロパティをに設定する必要があります。 このプロパティの既定値は `false` です。
2. `Process`&ndash;このプロパティを設定する必要があります。 これは、サービスが実行されるプロセスの名前を指定するために使用されます。
3. `IsolatedProcess`このプロパティを使用すると &ndash; 、追加のセキュリティが有効になり、システムの他の部分と対話するための最小限のアクセス許可で、分離されたサンドボックスでサービスを実行するように Android に指示します。 「 [Bugzilla 51940-分離されたプロセスを含むサービス」および「カスタムアプリケーションクラスは、オーバーロードの解決に失敗](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)します」を参照してください。
4. `Permission`クライアントが &ndash; 要求する (および付与される) 必要があるアクセス許可を指定することによって、サービスへのクライアントアクセスを制御することができます。

サービスを独自のプロセスで実行するには、 `Process` のプロパティを `ServiceAttribute` サービスの名前に設定する必要があります。 外部のアプリケーションと対話するには、 `Exported` プロパティをに設定する必要があり `true` ます。 `Exported`が `false` の場合、同じ apk (同じアプリケーション) 内のクライアントと、同じプロセス内で実行されているクライアントのみがサービスと対話できます。

サービスが実行されるプロセスの種類は、プロパティの値によって異なり `Process` ます。 Android では、次の3種類のプロセスを識別します。

- **プライベートプロセス** &ndash; プライベートプロセスは、それを開始したアプリケーションでのみ使用できます。 プロセスをプライベートとして識別するには、名前の先頭に **:** (セミコロン) を使用する必要があります。 前のコードスニペットとスクリーンショットに示されているサービスは、プライベートプロセスです。 の例を次のコードスニペットに示し `ServiceAttribute` ます。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

- **グローバルプロセス** &ndash; グローバルプロセスで実行されるサービスは、デバイス上で実行されているすべてのアプリケーションにアクセスできます。 グローバルプロセスは、小文字で始まる完全修飾クラス名でなければなりません。
    (サービスをセキュリティで保護するために手順を実行しない限り、他のアプリケーションはバインドして操作できます。 認証されていない使用に対してサービスをセキュリティで保護する方法については、このガイドで後ほど説明します)。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

- **分離プロセス** &ndash; 分離プロセスは、システムの他の部分から分離された独自のサンドボックスで実行されるプロセスで、独自のアクセス許可はありません。 分離されたプロセスでサービスを実行するには、次 `IsolatedProcess` の `ServiceAttribute` `true` コードスニペットに示すように、のプロパティをに設定します。
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> 「 [Bugzilla 51940-分離されたプロセスを含むサービス」および「カスタムアプリケーションクラスでオーバーロードを正しく解決でき](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)ない」を参照してください。

分離サービスは、信頼されていないコードに対してアプリケーションとデバイスをセキュリティで保護する簡単な方法です。 たとえば、アプリは web サイトからスクリプトをダウンロードして実行できます。 この場合、分離されたプロセスでこれを実行すると、信頼されていないコードに対するセキュリティ層が追加され、Android デバイスが侵害されます。

> [!IMPORTANT]
> サービスがエクスポートされると、サービスの名前は変更されません。 サービスの名前を変更すると、そのサービスを使用している他のアプリケーションが中断される可能性があります。

プロパティの効果を確認するために `Process` 、次のスクリーンショットは、独自のプライベートプロセスで実行されているサービスを示しています。

![プライベートプロセスで実行されているサービスを示すスクリーンショット](out-of-process-services-images/ipc-04.png "プライベートプロセスで実行されているサービスを示すスクリーンショット。")

次のスクリーンショットは、 `Process="com.xamarin.xample.messengerservice.timestampservice_process"` グローバルプロセスで実行されているサービスを示しています。

![グローバルプロセスで実行されているサービスのスクリーンショット](out-of-process-services-images/ipc-05.png "グローバルプロセスで実行されているサービスのスクリーンショット。")

`ServiceAttribute`が設定されると、サービスはを実装する必要があり `Handler` ます。

### <a name="implementing-a-handler"></a>ハンドラーの実装

クライアント要求を処理するには、サービスがを実装し、メソッドをオーバーライドする必要があり `Handler` `HandleMessage` ます。 このメソッドは、 `Message` クライアントからのメソッド呼び出しをカプセル化し、その呼び出しをサービスが実行する何らかのアクションまたはタスクに変換するインスタンスを受け取ります。 オブジェクトは、 `Message` という名前のプロパティを公開します。このプロパティは、 `What` クライアントとサービスの間で共有され、サービスがクライアントに対して実行するタスクに関連します。

サンプルアプリケーションの次のコードスニペットは、の1つの例を示して `HandleMessage` います。 この例では、クライアントがサービスに対して要求できる2つのアクションがあります。

- 最初のアクションは _Hello, World_ メッセージで、クライアントは単純なメッセージをサービスに送信しました。
- 2番目のアクションは、サービスのメソッドを呼び出し、文字列を取得します。この例では、文字列は、サービスの開始時刻と実行時間を返すメッセージです。

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client has sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrieve a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

でサービスのパラメーターをパッケージ化することもでき `Message` ます。 これについては、このガイドの後半で説明します。 次に考慮すべきトピックは、 `Messenger` 受信したを処理するオブジェクトを作成することです `Message` 。

### <a name="instantiating-the-messenger"></a>Messenger のインスタンス化

既に説明したように、オブジェクトを逆シリアル化し、を `Message` 呼び出すこと `Handler.HandleMessage` は、オブジェクトの役割です `Messenger` 。 クラスには、 `Messenger` `IBinder` クライアントがサービスにメッセージを送信するために使用するオブジェクトも用意されています。  

サービスが開始されると、がインスタンス化され `Messenger` 、が挿入され `Handler` ます。 この初期化は、サービスのメソッドに対して実行することをお勧めし `OnCreate` ます。 次のコードスニペットは、独自のとを初期化するサービスの一例です `Handler` `Messenger` 。

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

この時点で、最後の手順はがオーバーライドされることです `Service` `OnBind` 。

### <a name="implementing-serviceonbind"></a>Service. OnBind の実装

バインドされたすべてのサービスは、独自のプロセスで実行されているかどうかにかかわらず、メソッドを実装する必要があり `OnBind` ます。 このメソッドの戻り値は、クライアントがサービスとの対話に使用できるいくつかのオブジェクトです。 そのオブジェクトがどのようなものであるかは、サービスがローカルサービスとリモートサービスのどちらであるかによって異なります。 ローカルサービスはカスタム実装を返しますが `IBinder` 、リモートサービスはカプセル化されたを返しますが、 `IBinder` 前の `Messenger` セクションで作成されたを返します。

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

これら3つの手順を完了すると、リモートサービスは完了したと見なすことができます。

## <a name="consuming-the-service"></a>サービスの利用

すべてのクライアントは、リモートサービスをバインドして使用できるようにするために、いくつかのコードを実装する必要があります。 概念的には、クライアントの観点から見ると、ローカルサービスまたはリモートサービスへのバインドにはほとんど違いがありません。 クライアントはメソッドを呼び出し `BindService` 、サービスを識別するための明示的なインテントと、 `IServiceConnection` クライアントとサービスの間の接続の管理に役立つを渡します。

このコードスニペットは、リモートサービスにバインドするための **明示的なインテント** を作成する方法の例です。 インテントは、サービスを含むパッケージとサービスの名前を識別する必要があります。 この情報を設定する方法の1つとして、オブジェクトを使用して目的を指定する方法があり `Android.Content.ComponentName` ます。 このコードスニペットの例を次に示します。  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

サービスがバインドされると、 `IServiceConnection.OnServiceConnected` メソッドが呼び出され、が `IBinder` クライアントに提供されます。 ただし、クライアントはを直接使用しません `IBinder` 。 代わりに、そのからオブジェクトをインスタンス化 `Messenger` `IBinder` します。 これは、 `Messenger` クライアントがリモートサービスとの対話に使用するです。

次の例では、 `IServiceConnection` クライアントがサービスとの接続および切断を処理する方法を示す基本的な実装の例を示します。 `OnServiceConnected`メソッドはとを受け取り、 `IBinder` クライアントはそのからを作成し `Messenger` `IBinder` ます。

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null;
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

サービス接続と目的が作成されると、クライアントはを呼び出してバインドプロセスを開始することができ `BindService` ます。

```csharp
var serviceConnection = new TimestampServiceConnection(this);
BindService(serviceToStart, serviceConnection, Bind.AutoCreate);
```

クライアントがサービスに正常にバインドされ、が使用可能になると、 `Messenger` クライアントがサービスに送信される可能性があり `Messages` ます。

## <a name="sending-messages-to-the-service"></a>サービスへのメッセージの送信

クライアントが接続され、オブジェクトがある `Messenger` 場合、を介してオブジェクトをディスパッチすることで、サービスと通信することができ `Message` `Messenger` ます。 この通信は一方向であり、クライアントがメッセージを送信しますが、サービスからクライアントに返されるメッセージはありません。 この点で、は `Message` 起動と破棄のメカニズムです。

オブジェクトを作成する `Message` 場合は、ファクトリメソッドを使用することをお勧めし [`Message.Obtain`](xref:Android.OS.Message) ます。 このメソッドは、 `Message` Android によって管理されているグローバルプールからオブジェクトをプルします。 `Message.Obtain` には、 `Message` サービスに必要な値とパラメーターを使用してオブジェクトを初期化できるようにするオーバーロードされたメソッドもいくつかあります。  `Message`がインスタンス化されると、を呼び出すことによってサービスにディスパッチされ `Messenger.Send` ます。 このスニペットは、を作成してサービスプロセスにディスパッチする例の1つです `Message` 。

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

メソッドには、いくつかの異なる形式があり `Message.Obtain` ます。 前の例では、を使用して [`Message.Obtain(Handler h, Int32 what)`](xref:Android.OS.Message.Obtain) います。 これは、アウトプロセスサービスに対する非同期要求であるためです。サービスからの応答がないため、 `Handler` はに設定され `null` ます。 2番目のパラメーターは、 `Int32 what` オブジェクトのプロパティに格納され `.What` `Message` ます。 プロパティは、サービス `.What` プロセスのコードによって、サービスでメソッドを呼び出すために使用されます。

また、クラスは、 `Message` 受信者に使用できる2つの追加プロパティである `Arg1` とも公開し `Arg2` ます。 これらの2つのプロパティは、クライアントとサービスの間に意味のある特別な合意値を持つ可能性がある整数値です。 たとえば、は `Arg1` 顧客 ID を保持し、 `Arg2` その顧客の注文書番号を保持する場合があります。 は [`Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)`](xref:Android.OS.Message.Obtain) 、の作成時に、2つのプロパティを設定するために使用でき `Message` ます。 この2つの値を設定するもう1つの方法は、 `.Arg` `.Arg2` オブジェクトが作成された後に、プロパティとプロパティを直接設定することです `Message` 。

### <a name="passing-additional-values-to-the-service"></a>サービスに追加の値を渡す

を使用すると、より複雑なデータをサービスに渡すことができ `Bundle` ます。 この場合、追加の値をに配置し、と共に `Bundle` 送信 `Message` する前に[ `.Data` プロパティ](xref:Android.OS.Message.Data)プロパティを設定することによって送信できます。

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```

> [!NOTE]
> 一般に、1 `Message` mb を超えるペイロードを持つことはできません。 サイズ制限は、Android のバージョンと、デバイスにバンドルされている Android オープンソースプロジェクト (または、ベンダーが作成した Android オープンソースプロジェクト) の実装に加えられた独自の変更によって異なる場合があります。

## <a name="returning-values-from-the-service"></a>サービスから値を返す

この時点までに説明したメッセージングアーキテクチャは一方向であり、クライアントがメッセージをサービスに送信します。 サービスがクライアントに値を返す必要がある場合は、この点について説明したすべてのものが元に戻されます。 サービスはを作成し `Message` 、戻り値をパッケージ化し、を介してをクライアントにディスパッチする必要があり `Message` `Messenger` ます。 ただし、サービスはそれ自体を作成するのでは `Messenger` なく、 `Messenger` 最初の要求の一部として、クライアントのインスタンス化とパッケージ化に依存します。 サービスは、 `Send` このクライアントによって提供されるを使用してメッセージを表示し `Messenger` ます。  

双方向通信のイベントのシーケンスは次のとおりです。

1. クライアントがサービスにバインドされます。 サービスとクライアントが接続すると、 `IServiceConnection` クライアントによって管理されるは、を `Messenger` サービスに送信するために使用されるオブジェクトへの参照を保持し `Message` ます。 混乱を避けるため、これは _サービスメッセンジャー_と呼ばれます。
2. クライアントは、 `Handler` ( _クライアントハンドラー_と呼ばれます) をインスタンス化し、それを使用して独自の `Messenger` ( _クライアント Messenger_) を初期化します。 サービスメッセンジャーとクライアント Messenger は、2つの異なる方向のトラフィックを処理する2つの異なるオブジェクトであることに注意してください。 サービスメッセンジャーは、クライアントからサービスへのメッセージを処理します。また、クライアント Messenger は、サービスからクライアントへのメッセージを処理します。
3. クライアントはオブジェクトを作成 `Message` し、プロパティを `ReplyTo` クライアント Messenger に設定します。 メッセージは、サービスメッセンジャーを使用してサービスに送信されます。
4. サービスは、クライアントからメッセージを受信し、要求された作業を実行します。
5. サービスがクライアントに応答を送信する時刻になると、はを使用して `Message.Obtain` 新しいオブジェクトを作成し `Message` ます。
6. このメッセージをクライアントに送信するために、サービスはクライアントメッセージのプロパティからクライアント Messenger を抽出 `.ReplyTo` し、それを `.Send` `Message` クライアントに返します。
7. 応答がクライアントによって受信されると、クライアントは、 `Handler` `Message` プロパティを調べて (必要に応じて、に格納されているパラメーターを抽出して) 処理する独自のを持ち `.What` `Message` ます。

このサンプルコードでは、クライアントがをインスタンス化 `Message` し、サービスが応答に使用するをパッケージ化する方法を示し `Messenger` ます。

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

サービスは、を抽出し、それ `Handler` を使用してクライアントに応答を送信するために、自身に何らかの変更を加える必要があり `Messenger` ます。 このコードスニペットは、サービスが `Handler` を作成してクライアントに送信する方法の例です `Message` 。  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

上記のコードサンプルで `Messenger` は、クライアントによって作成されたインスタンスが、サービスによって受信されるオブジェクトと同じでは *ない* ことに注意してください。 これらは `Messenger` 、通信チャネルを表す2つの異なるプロセスで実行される2つの異なるオブジェクトです。

## <a name="securing-the-service-with-android-permissions"></a>Android アクセス許可を使用してサービスをセキュリティで保護する

グローバルプロセスで実行されるサービスは、その Android デバイスで実行されているすべてのアプリケーションからアクセスできます。 場合によっては、このようなオープン性と可用性が望ましくないため、承認されていないクライアントからのアクセスに対してサービスをセキュリティで保護する必要があります。 リモートサービスへのアクセスを制限する方法の1つは、Android のアクセス許可を使用することです。

権限は `Permission` 、サブクラスを装飾するのプロパティによって識別でき `ServiceAttribute` `Service` ます。 これにより、サービスにバインドするときにクライアントに付与する必要があるアクセス許可に名前が設定されます。 クライアントに適切なアクセス許可がない場合、 `Java.Lang.SecurityException` クライアントがサービスにバインドしようとすると、Android からがスローされます。

Android には、次の4種類のアクセス許可レベルがあります。

- **標準** &ndash; これは、既定のアクセス許可レベルです。 これは、Android によって要求されたクライアントに自動的に付与できる危険度の低いアクセス許可を識別するために使用されます。 ユーザーは、これらのアクセス許可を明示的に付与する必要はありませんが、アプリの設定でアクセス許可を表示できます。
- **署名** &ndash; これは、同じ証明書で署名されたアプリケーションに Android によって自動的に付与される特別なアクセス許可のカテゴリです。 このアクセス許可は、アプリケーション開発者がアプリ間でコンポーネントやデータを簡単に共有できるように設計されています。ユーザーは、一定のかけるを必要としません。
- **Signatureorsystem** &ndash; これは、上記で説明した **署名** のアクセス許可とよく似ています。 このアクセス許可は、同じ証明書によって署名されたアプリに自動的に付与されるだけでなく、Android システムイメージと共にインストールされたアプリの署名に使用されたものと同じ証明書に署名されているアプリにも付与されます。 このアクセス許可は、通常、アプリケーションがサードパーティのアプリと連携できるようにするために、Android ROM 開発者によってのみ使用されます。 これは一般に、パブリックなの一般配布を目的としたアプリでは使用されません。
- **危険** &ndash; 危険なアクセス許可は、ユーザーにとって問題の原因となる可能性があります。 このため、 **危険** なアクセス許可はユーザーによって明示的に承認される必要があります。

`signature`およびの `normal` アクセス許可は、Android によってインストールされるときに自動的に付与されるため、クライアントを含む apk の**前に**、サービスをホストする apk をインストールすることが重要です。 クライアントが最初にインストールされた場合、Android はアクセス許可を付与しません。 この場合は、クライアント APK をアンインストールし、サービス APK をインストールしてから、クライアント APK を再インストールする必要があります。

Android のアクセス許可でサービスをセキュリティで保護するには、次の2つの一般的な方法があります。

1. **署名レベルのセキュリティ** &ndash; を実装する署名レベルのセキュリティとは、サービスを保持する APK への署名に使用されたのと同じキーで署名されたアプリケーションに対して、アクセス許可が自動的に付与されることを意味します。 これは、開発者が自分のアプリケーションからアクセス可能な状態を維持しながら、サービスをセキュリティで保護するための簡単な方法です。 シグネチャレベルのアクセス許可は `Permission` 、のプロパティをに設定することによって宣言され `ServiceAttribute` `signature` ます。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2. **カスタムアクセス許可** &ndash; を作成するサービスの開発者は、サービスのカスタムアクセス許可を作成することができます。 これは、開発者がサービスを他の開発者のアプリケーションと共有したい場合に最適です。 カスタムアクセス許可を実装するには多少の労力が必要であり、以下に説明します。

カスタムアクセス許可を作成する簡単な例に `normal` ついては、次のセクションで説明します。 Android のアクセス許可の詳細については、Google のドキュメントを参照して [& セキュリティに関するベストプラクティス](https://developer.android.com/training/articles/security-tips.html)を確認してください。 Android のアクセス許可の詳細については、android のアクセス許可の詳細については、アプリケーションマニフェストの Android ドキュメントの [アクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) に関するセクションを参照してください。

> [!NOTE]
> 一般に、Google はユーザーが混乱する可能性があるため、 [カスタムアクセス許可の使用](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) を避けることができます。

### <a name="creating-a-custom-permission"></a>カスタムアクセス許可の作成

カスタムアクセス許可を使用するには、クライアントが明示的にそのアクセス許可を要求している間、サービスによって宣言されます。

Service APK にアクセス許可を作成するに `permission` は、要素を `manifest` **AndroidManifest.xml**の要素に追加します。 このアクセス許可には、、、およびの各属性が設定されている必要があり `name` `protectionLevel` `label` ます。 属性は、 `name` アクセス許可を一意に識別する文字列に設定する必要があります。 名前は、次のセクションに示すように、 **Android の設定**の**アプリ情報**ビューに表示されます。

属性は、 `protectionLevel` 上記で説明した4つの文字列値のいずれかに設定する必要があります。  とは、 `label` `description` 文字列リソースを参照する必要があり、ユーザーにわかりやすい名前と説明を提供するために使用されます。

このスニペットは、 `permission` サービスを含む APK の **AndroidManifest.xml** でカスタム属性を宣言する例です。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

次に、クライアント APK の **AndroidManifest.xml** は、この新しいアクセス許可を明示的に要求する必要があります。 これを行うには `users-permission` 、属性を **AndroidManifest.xml**に追加します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>アプリに付与されたアクセス許可を表示する

アプリケーションに付与されているアクセス許可を表示するには、Android 設定アプリを開き、[ **アプリ**] を選択します。 一覧でアプリケーションを見つけて選択します。 [ **アプリ情報** ] 画面で、[ **アクセス許可** ] をタップすると、アプリに付与されているすべてのアクセス許可を表示するビューが表示されます。

[![アプリケーションに付与されたアクセス許可を検索する方法を示す、Android デバイスのスクリーンショット](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>まとめ

このガイドでは、リモートプロセスで Android サービスを実行する方法について詳しく説明しました。 ローカルサービスとリモートサービスの違いについて説明しました。また、リモートサービスが Android アプリの安定性とパフォーマンスに役立つ可能性がある理由についても説明しました。 リモートサービスを実装する方法と、クライアントがサービスと通信する方法について説明した後、このガイドでは、承認されたクライアントのみからサービスへのアクセスを制限する方法の1つを紹介しました。

## <a name="related-links"></a>関連リンク

- [Handler](xref:Android.OS.Handler)
- [メッセージ](xref:Android.OS.Message)
- [メッセンジャー](xref:Android.OS.Messenger)
- [ServiceAttribute](xref:Android.App.ServiceAttribute)
- [エクスポートされた属性](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [分離されたプロセスおよびカスタムアプリケーションクラスを含むサービスが、オーバーロードを正しく解決できない](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [プロセスとスレッド](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android マニフェスト-アクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [セキュリティのヒント](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (サンプル)](/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-messengerservicedemo)