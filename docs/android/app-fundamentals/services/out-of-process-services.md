---
title: リモートプロセスでの Android サービスの実行
description: 一般に、Android アプリケーションのすべてのコンポーネントは同じプロセスで実行されます。 Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。 このガイドでは、Xamarin を使用して Android リモートサービスを作成および使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: f546a1403aa0af07fc69187c4cfbec8982ed7a2a
ms.sourcegitcommit: 5821c9709bf5e06e6126233932f94f9cf3524577
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75556510"
---
# <a name="running-android-services-in-remote-processes"></a>リモートプロセスでの Android サービスの実行

_一般に、Android アプリケーションのすべてのコンポーネントは同じプロセスで実行されます。Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。このガイドでは、Xamarin を使用して Android リモートサービスを作成および使用する方法について説明します。_

## <a name="out-of-process-services-overview"></a>アウトプロセスサービスの概要

アプリケーションの起動時に、Android によって、アプリケーションを実行するプロセスが作成されます。 通常、アプリケーションがこの1つのプロセスで実行するすべてのコンポーネントです。 Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。 これらの種類のサービスは、_リモートサービス_または_アウトプロセスサービス_と呼ばれます。 これらのサービスのコードは、メインアプリケーションと同じ APK に含まれます。ただし、サービスが開始されると、Android はそのサービスに対してのみ新しいプロセスを作成します。 これに対し、アプリケーションの他の部分と同じプロセスで実行されるサービスは、_ローカルサービス_と呼ばれることもあります。

一般に、アプリケーションでリモートサービスを実装する必要はありません。 多くの場合、アプリの要件にはローカルサービスが十分 (かつ望ましい) あります。 アウトプロセスには、Android で管理する必要がある独自のメモリ空間があります。 これにより、アプリケーション全体のオーバーヘッドが増加しますが、独自のプロセスでサービスを実行すると便利なシナリオがいくつかあります。

1. **共有機能**&ndash; アプリケーション開発者によっては、すべてのアプリケーション間で共有される複数のアプリと機能がある場合があります。 独自のプロセスで実行される Android サービスにその機能をパッケージ化すると、アプリケーションの保守が簡単になります。 また、独自のスタンドアロン APK でサービスをパッケージ化し、アプリケーションの他の部分とは別にデプロイすることもできます。
2. **ユーザーエクスペリエンスの向上**&ndash; アウトプロセスサービスがアプリケーションのユーザーエクスペリエンスを向上させる方法は2つあります。 最初の方法では、メモリ管理について扱います。 ガベージコレクション (GC) サイクルが発生すると、Android は GC が完了するまでプロセス内のすべてのアクティビティを一時停止します。 ユーザーは、この一時停止を "途切れている" または "jank" と感じることがあります。 サービスが独自のプロセスで実行されている場合は、アプリケーションプロセスではなく、一時停止されているサービスプロセスです。 この一時停止は、アプリケーションプロセス (したがってユーザーインターフェイス) が一時停止していないため、ユーザーにとってあまりわかりにくくなります。

    また、プロセスのメモリ要件が大きすぎると、Android はそのプロセスを終了してデバイスのリソースを解放する可能性があります。 サービスのメモリフットプリントが大きく、UI と同じプロセスで実行されている場合、Android がそれらのリソースを強制的に再利用すると、UI がシャットダウンされ、ユーザーにアプリの起動が強制されます。 ただし、独自のプロセスで実行されているサービスが Android によってシャットダウンされた場合、UI プロセスは影響を受けません。 UI では、サービスをバインド (および再起動) し、ユーザーに対して透過的に実行し、通常の動作を再開することができます。

3. **アプリケーションのパフォーマンスを向上させる**&ndash;、サービスプロセスに依存せずに UI プロセスが終了またはシャットダウンされる可能性があります。 長いスタートアップタスクをアウトプロセスサービスに移動することで、UI の起動時間が短縮される可能性があります (これは、サービスプロセスが UI が起動されたときの時間の間に維持されることを前提としています)。

多くの点で、別のプロセスで実行されているサービスへのバインドは、[ローカルサービスへのバインド](~/android/app-fundamentals/services/creating-a-service/bound-services.md)と同じです。 クライアントは `BindService` を呼び出して、サービスをバインド (必要に応じて開始) します。 クライアントとサービスの間の接続を管理するために `Android.OS.IServiceConnection` オブジェクトが作成されます。 クライアントがサービスに正常にバインドした場合、Android は、サービスでメソッドを呼び出すために使用できる `IServiceConnection` を介してオブジェクトを返します。 クライアントは、このオブジェクトを使用してサービスと対話します。 確認するには、サービスにバインドする手順を次に示します。

- サービスへのバインドに明示的なインテント &ndash; 使用する必要がある**インテントを作成**します。
- `IServiceConnection` オブジェクト &ndash; クライアントとサービスの間の仲介役として機能する **`IServiceConnection` オブジェクトを実装し、インスタンス化**します。  クライアントとサーバー間の接続を監視する役割があります。
- **`BindService` `BindService` &ndash; メソッドを呼び**出すと、前の手順で作成したインテントとサービス接続が Android にディスパッチされます。これにより、サービスの開始とクライアントとサービス間の通信の確立が行われます。

プロセス間の境界を越える必要があるため、複雑さが増します。通信は一方向 (クライアントからサーバー) であり、クライアントはサービスクラスのメソッドを直接呼び出すことができません。 サービスがクライアントと同じプロセスを実行している場合、Android には、双方向の通信を可能にする `IBinder` オブジェクトが用意されていることを思い出してください。 これは、サービスが独自のプロセスで実行されている場合には当てはまりません。 クライアントは、`Android.OS.Messenger` クラスを使用してリモートサービスと通信します。

クライアントがリモートサービスとのバインドを要求すると、Android は `Service.OnBind` ライフサイクルメソッドを呼び出します。これにより、`Messenger`によってカプセル化された内部 `IBinder` オブジェクトが返されます。 `Messenger` は、Android SDK によって提供される特殊な `IBinder` 実装に対するシンラッパーです。 `Messenger` は、2つの異なるプロセス間の通信を処理します。 開発者は、メッセージのシリアル化、プロセス境界を越えたメッセージのマーシャリング、およびクライアントでの逆シリアル化の詳細について心配していません。 この作業は、`Messenger` オブジェクトによって処理されます。 次の図は、クライアントがアウトプロセスサービスへのバインドを開始するときに関連するクライアント側の Android コンポーネントを示しています。

![サービスにバインドするクライアントの手順とコンポーネントを示す図](out-of-process-services-images/ipc-01.png "サービスにバインドするクライアントの手順とコンポーネントを示す図。")

リモートプロセスの `Service` クラスは、ローカルプロセス内のバインドされたサービスを通過する同じライフサイクルコールバックを通過します。また、関連する Api の多くは同じです。 `Service.OnCreate` は、`Handler` を初期化し、それを `Messenger` オブジェクトに挿入するために使用されます。 同様に、`OnBind` はオーバーライドされますが、`IBinder` オブジェクトを返す代わりに、サービスは `Messenger`を返します。  次の図は、クライアントがサービスにバインドされている場合のサービスの動作を示しています。

![リモートクライアントによってバインドされるときにサービスが実行する手順とコンポーネントを示す図](out-of-process-services-images/ipc-02.png "リモートクライアントによってバインドされるときにサービスが実行する手順とコンポーネントを示す図。")

サービスによって `Message` が受信されると、`Android.OS.Handler`のインスタンス内で処理されます。 サービスは、`HandleMessage` メソッドをオーバーライドする必要がある独自の `Handler` サブクラスを実装します。 このメソッドは `Messenger` によって呼び出され、`Message` をパラメーターとして受け取ります。 `Handler` は、`Message` メタデータを検査し、その情報を使用してサービスでメソッドを呼び出します。

一方向の通信は、クライアントが `Message` オブジェクトを作成し、`Messenger.Send` メソッドを使用してサービスにディスパッチするときに発生します。 `Messenger.Send` は `Message`、シリアル化されたデータを外部から Android にシリアル化します。これにより、メッセージがプロセスの境界を越えてサービスにルーティングされます。  サービスによってホストされる `Messenger` は、受信データから `Message` オブジェクトを作成します。 この `Message` は、メッセージが `Handler`に1つずつ送信されるキューに配置されます。 `Handler` は、`Message` に含まれるメタデータを検査し、`Service`に対して適切なメソッドを呼び出します。 次の図は、これらのさまざまな動作について説明しています。

![プロセス間でメッセージが渡される方法を示す図](out-of-process-services-images/ipc-03.png "プロセス間でメッセージがどのように渡されるかを示す図")

このガイドでは、アウトプロセスサービスの実装の詳細について説明します。 独自のプロセスで実行することを意図したサービスを実装する方法と、クライアントが `Messenger` framework を使用してそのサービスと通信する方法について説明します。 また、双方向の通信についても簡単に説明します。クライアントは、サービスにメッセージを送信し、サービスはクライアントにメッセージを返信します。 サービスは異なるアプリケーション間で共有できるため、このガイドでは、Android のアクセス許可を使用してサービスへのクライアントアクセスを制限する方法の1つについても説明します。

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950-分離されたプロセスとカスタムアプリケーションクラスを使用したサービスは、オーバーロードの解決に失敗](https://github.com/xamarin/xamarin-android/issues/1950)します。これは、`IsolatedProcess` が `true`に設定されている場合に、Xamarin. Android サービスが正しく起動しないことを報告します。 このガイドは、リファレンスのために用意されています。 Xamarin Android アプリケーションは、Java で記述されたアウトプロセスサービスと引き続き通信できなければなりません。

## <a name="requirements"></a>要件

このガイドは、サービスの作成に関する知識を前提としています。

以前の Android Api を対象とするアプリでは、暗黙的なインテントを使用することもできますが、このガイドでは明示的なインテントの使用についてのみ説明します。 Android 5.0 (API レベル 21) 以降を対象とするアプリでは、サービスとのバインドに明示的なインテントを使用する必要があります。これは、このガイドで説明されている手法です。「」を参照してください。

## <a name="create-a-service-that-runs-in-a-separate-process"></a>別のプロセスで実行されるサービスを作成する

前述のように、サービスが独自のプロセスで実行されているという事実は、いくつかの異なる Api が関係していることを意味します。 簡単な概要として、リモートサービスにバインドして使用する手順を次に示します。  

- `Service` 型をサブクラス化 &ndash; **`Service` サブクラスを作成**し、バインドされたサービスのライフサイクルメソッドを実装します。 また、サービスが独自のプロセスで実行されることを Android に通知するメタデータを設定する必要もあります。
- `Handler` がクライアント要求の分析、クライアントから渡されたパラメーターの抽出、およびサービスに対する適切なメソッドの呼び出しを行う **`Handler`&ndash; を実装**します。
- 前の手順で説明したように **`Messenger`&ndash; をインスタンス化**するには、前の手順で作成した `Handler` にクライアント要求をルーティングする `Messenger` クラスのインスタンスを各 `Service` で保持する必要があります。

独自のプロセスで実行することを意図したサービスは、基本的にはバインドされたサービスです。 サービスクラスは、基本 `Service` クラスを拡張し、android マニフェストでバンドルする必要のあるメタデータを含む `ServiceAttribute` で修飾されます。 まず、アウトプロセスサービスにとって重要な `ServiceAttribute` の次のプロパティを示します。

1. 他のアプリケーションがサービスと対話できるようにするには、このプロパティ &ndash; `Exported` を `true` に設定する必要があります。 このプロパティの既定値は `false` です。
2. `Process` &ndash; このプロパティを設定する必要があります。 これは、サービスが実行されるプロセスの名前を指定するために使用されます。
3. このプロパティ &ndash; `IsolatedProcess` すると、追加のセキュリティが有効になり、システムの他の部分と対話するための最小限のアクセス許可を持つ分離されたサンドボックスでサービスを実行するように Android に指示されます。 「 [Bugzilla 51940-分離されたプロセスを含むサービス」および「カスタムアプリケーションクラスは、オーバーロードの解決に失敗](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)します」を参照してください。
4. `Permission` &ndash; クライアントが要求 (および付与) するアクセス許可を指定することによって、サービスへのクライアントアクセスを制御することができます。

サービスを独自のプロセスで実行するには、`ServiceAttribute` の `Process` プロパティをサービスの名前に設定する必要があります。 外部のアプリケーションと対話するには、`Exported` プロパティを `true`に設定する必要があります。 `Exported` が `false`場合、同じ APK (同じアプリケーション) 内のクライアントと、同じプロセスで実行されているクライアントのみがサービスと対話できます。

サービスが実行されるプロセスの種類は、`Process` プロパティの値によって異なります。 Android では、次の3種類のプロセスを識別します。

- プライベート**プロセス &ndash; プライベートプロセスは**、それを開始したアプリケーションでのみ使用できます。 プロセスをプライベートとして識別するには、名前の先頭に **:** (セミコロン) を使用する必要があります。 前のコードスニペットとスクリーンショットに示されているサービスは、プライベートプロセスです。 `ServiceAttribute`の例を次のコードスニペットに示します。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

- グローバル**プロセス**&ndash; グローバルプロセスで実行されるサービスは、デバイス上で実行されているすべてのアプリケーションにアクセスできます。 グローバルプロセスは、小文字で始まる完全修飾クラス名でなければなりません。
    (サービスをセキュリティで保護するために手順を実行しない限り、他のアプリケーションはバインドして操作できます。 認証されていない使用に対してサービスをセキュリティで保護する方法については、このガイドで後ほど説明します)。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

- 分離**プロセス &ndash; 分離プロセスは**、システムの他の部分から分離された独自のサンドボックスで実行されるプロセスであり、独自のアクセス許可はありません。 分離プロセスでサービスを実行するには、次のコードスニペットに示すように、`ServiceAttribute` の `IsolatedProcess` プロパティを `true` に設定します。
    
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

`Process` プロパティの効果を確認するために、次のスクリーンショットは、独自のプライベートプロセスで実行されているサービスを示しています。

![プライベートプロセスで実行されているサービスを示すスクリーンショット](out-of-process-services-images/ipc-04.png "プライベートプロセスで実行されているサービスを示すスクリーンショット。")

次のスクリーンショットは、グローバルプロセスで実行されている `Process="com.xamarin.xample.messengerservice.timestampservice_process"` とサービスを示しています。

![グローバルプロセスで実行されているサービスのスクリーンショット](out-of-process-services-images/ipc-05.png "グローバルプロセスで実行されているサービスのスクリーンショット。")

`ServiceAttribute` が設定されたら、サービスは `Handler`を実装する必要があります。

### <a name="implementing-a-handler"></a>ハンドラーの実装

クライアント要求を処理するには、サービスが `Handler` を実装し、`HandleMessage` メソッドをオーバーライドする必要があります。 このメソッドは、クライアントからのメソッド呼び出しをカプセル化し、その呼び出しをサービスによって実行されるアクションまたはタスクに変換する `Message` インスタンスを受け取ります。 `Message` オブジェクトは `What` という名前のプロパティを公開します。このプロパティは、クライアントとサービスの間で共有され、サービスがクライアントに対して実行するタスクに関連します。

サンプルアプリケーションの次のコードスニペットは、`HandleMessage`の1つの例を示しています。 この例では、クライアントがサービスに対して要求できる2つのアクションがあります。

- 最初のアクションは_Hello, World_メッセージで、クライアントは単純なメッセージをサービスに送信しました。
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

また、`Message`でサービスのパラメーターをパッケージ化することもできます。 これについては、このガイドの後半で説明します。 次に考慮すべきトピックは、受信 `Message`s を処理する `Messenger` オブジェクトを作成することです。

### <a name="instantiating-the-messenger"></a>Messenger のインスタンス化

既に説明したように、`Message` オブジェクトを逆シリアル化し、`Handler.HandleMessage` を呼び出すことは、`Messenger` オブジェクトの役割です。 `Messenger` クラスには、クライアントがサービスにメッセージを送信するために使用する `IBinder` オブジェクトも用意されています。  

サービスが開始されると、`Messenger` がインスタンス化され、`Handler`が挿入されます。 この初期化を実行するには、サービスの `OnCreate` メソッドを実行することをお勧めします。 このコードスニペットは、独自の `Handler` と `Messenger`を初期化するサービスの一例です。

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

この時点で、最後の手順は `OnBind`をオーバーライドする `Service` です。

### <a name="implementing-serviceonbind"></a>Service. OnBind の実装

バインドされたすべてのサービスは、独自のプロセスで実行されているかどうかにかかわらず、`OnBind` メソッドを実装する必要があります。 このメソッドの戻り値は、クライアントがサービスとの対話に使用できるいくつかのオブジェクトです。 そのオブジェクトがどのようなものであるかは、サービスがローカルサービスとリモートサービスのどちらであるかによって異なります。 ローカルサービスはカスタム `IBinder` 実装を返しますが、リモートサービスは、カプセル化されているが、前のセクションで作成された `Messenger` の `IBinder` を返します。

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

これら3つの手順を完了すると、リモートサービスは完了したと見なすことができます。

## <a name="consuming-the-service"></a>サービスの利用

すべてのクライアントは、リモートサービスをバインドして使用できるようにするために、いくつかのコードを実装する必要があります。 概念的には、クライアントの観点から見ると、ローカルサービスまたはリモートサービスへのバインドにはほとんど違いがありません。 クライアントは `BindService` メソッドを呼び出し、サービスを識別するための明示的なインテントと、クライアントとサービスの間の接続の管理に役立つ `IServiceConnection` を渡します。

このコードスニペットは、リモートサービスにバインドするための**明示的なインテント**を作成する方法の例です。 インテントは、サービスを含むパッケージとサービスの名前を識別する必要があります。 この情報を設定する方法の1つとして、`Android.Content.ComponentName` オブジェクトを使用して目的を指定する方法があります。 このコードスニペットの例を次に示します。  

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

サービスがバインドされると、`IServiceConnection.OnServiceConnected` メソッドが呼び出され、クライアントへの `IBinder` が提供されます。 ただし、クライアントは `IBinder`を直接使用しません。 代わりに、その `IBinder`から `Messenger` オブジェクトをインスタンス化します。 これは、クライアントがリモートサービスとの対話に使用する `Messenger` です。

次の例では、クライアントがサービスとの接続および切断を処理する方法を示す、基本的な `IServiceConnection` 実装の例を示します。 `OnServiceConnected` メソッドがを受け取り、`IBinder`し、クライアントがその `IBinder`から `Messenger` を作成することに注意してください。

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

サービス接続と目的が作成されると、クライアントは `BindService` を呼び出してバインドプロセスを開始することができます。

```csharp
var serviceConnection = new TimestampServiceConnection(this);
BindService(serviceToStart, serviceConnection, Bind.AutoCreate);
```

クライアントがサービスに正常にバインドされ、`Messenger` を使用できるようになると、クライアントが `Messages` をサービスに送信できるようになります。

## <a name="sending-messages-to-the-service"></a>サービスへのメッセージの送信

クライアントが接続され、`Messenger` オブジェクトがある場合、`Messenger`経由で `Message` オブジェクトをディスパッチすることで、サービスと通信することができます。 この通信は一方向であり、クライアントがメッセージを送信しますが、サービスからクライアントに返されるメッセージはありません。 この点では、`Message` は起動と破棄のメカニズムです。

`Message` オブジェクトを作成する場合は、 [`Message.Obtain`](xref:Android.OS.Message)ファクトリメソッドを使用することをお勧めします。 このメソッドは、Android によって管理されているグローバルプールから `Message` オブジェクトをプルします。 `Message.Obtain` には、サービスに必要な値とパラメーターを使用して `Message` オブジェクトを初期化できるようにするオーバーロードされたメソッドもいくつかあります。  `Message` がインスタンス化されると、`Messenger.Send`を呼び出すことによってサービスにディスパッチされます。 このスニペットは、サービスプロセスに `Message` を作成してディスパッチする例の1つです。

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

`Message.Obtain` メソッドには、いくつかの異なる形式があります。 前の例では、 [`Message.Obtain(Handler h, Int32 what)`](xref:Android.OS.Message.Obtain)を使用します。 これは、アウトプロセスサービスに対する非同期要求であるためです。サービスからの応答がないため、`Handler` は `null`に設定されます。 2番目のパラメーター `Int32 what`は、`Message` オブジェクトの `.What` プロパティに格納されます。 `.What` プロパティは、サービスでメソッドを呼び出すためにサービスプロセス内のコードによって使用されます。

`Message` クラスでは、受信者に使用できる2つの追加のプロパティである `Arg1` と `Arg2`も公開されます。 これらの2つのプロパティは、クライアントとサービスの間に意味のある特別な合意値を持つ可能性がある整数値です。 たとえば、`Arg1` は顧客 ID を保持し、その顧客の注文書番号を保持 `Arg2` ことがあります。 [`Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)`](xref:Android.OS.Message.Obtain)を使用すると、`Message` の作成時に2つのプロパティを設定できます。 これらの2つの値を設定するもう1つの方法は、作成した後に、`Message` オブジェクトに対して、`.Arg` と `.Arg2` のプロパティを直接設定することです。

### <a name="passing-additional-values-to-the-service"></a>サービスに追加の値を渡す

`Bundle`を使用すると、より複雑なデータをサービスに渡すことができます。 この場合、追加の値を `Bundle` に配置し、送信する前に[`.Data` プロパティ](xref:Android.OS.Message.Data)プロパティを設定することにより、`Message` と共に送信することができます。

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```

> [!NOTE]
> 一般に、`Message` は 1 MB を超えるペイロードを持つことはできません。 サイズ制限は、Android のバージョンと、デバイスにバンドルされている Android オープンソースプロジェクト (または、ベンダーが作成した Android オープンソースプロジェクト) の実装に加えられた独自の変更によって異なる場合があります。

## <a name="returning-values-from-the-service"></a>サービスから値を返す

この時点までに説明したメッセージングアーキテクチャは一方向であり、クライアントがメッセージをサービスに送信します。 サービスがクライアントに値を返す必要がある場合は、この点について説明したすべてのものが元に戻されます。 サービスは `Message`を作成し、戻り値をパッケージ化し、`Messenger` 経由でクライアントに `Message` をディスパッチする必要があります。 ただし、サービスは独自の `Messenger`を作成しません。代わりに、クライアントが初期要求の一部として `Messenger` をインスタンス化してパッケージ化する必要があります。 サービスは、このクライアント提供の `Messenger`を使用してメッセージを `Send` します。  

双方向通信のイベントのシーケンスは次のとおりです。

1. クライアントがサービスにバインドされます。 サービスとクライアントが接続すると、クライアントによって管理される `IServiceConnection` は、`Message`をサービスに送信するために使用される `Messenger` オブジェクトへの参照を保持します。 混乱を避けるため、これは_サービスメッセンジャー_と呼ばれます。
2. クライアントは `Handler` (_クライアントハンドラー_と呼ばれます) をインスタンス化し、それを使用して独自の `Messenger` (_クライアント Messenger_) を初期化します。 サービスメッセンジャーとクライアント Messenger は、2つの異なる方向のトラフィックを処理する2つの異なるオブジェクトであることに注意してください。 サービスメッセンジャーは、クライアントからサービスへのメッセージを処理します。また、クライアント Messenger は、サービスからクライアントへのメッセージを処理します。
3. クライアントは `Message` オブジェクトを作成し、クライアント Messenger を使用して `ReplyTo` プロパティを設定します。 メッセージは、サービスメッセンジャーを使用してサービスに送信されます。
4. サービスは、クライアントからメッセージを受信し、要求された作業を実行します。
5. サービスがクライアントに応答を送信する時間が経過すると、`Message.Obtain` を使用して新しい `Message` オブジェクトが作成されます。
6. このメッセージをクライアントに送信するために、サービスはクライアントメッセージの `.ReplyTo` プロパティからクライアント Messenger を抽出し、それを使用してクライアントに `Message` を `.Send` します。
7. クライアントが応答を受信すると、その応答には、`.What` プロパティを調べることによって `Message` を処理する独自の `Handler` があります (必要に応じて、`Message`に含まれるパラメーターを抽出します)。

このサンプルコードでは、クライアントが `Message` をインスタンス化し、サービスが応答に使用する `Messenger` をパッケージ化する方法を示します。

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

サービスは、`Messenger` を抽出し、それを使用してクライアントに応答を送信するために、独自の `Handler` に何らかの変更を加える必要があります。 次のコードスニペットは、サービスの `Handler` が `Message` を作成してクライアントに送信する方法の例です。  

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

上記のコードサンプルでは、クライアントによって作成された `Messenger` インスタンスが、サービスによって受信されるオブジェクトと同じでは*ない*ことに注意してください。 これらは、通信チャネルを表す2つの異なるプロセスで実行される2つの異なる `Messenger` オブジェクトです。

## <a name="securing-the-service-with-android-permissions"></a>Android アクセス許可を使用してサービスをセキュリティで保護する

グローバルプロセスで実行されるサービスは、その Android デバイスで実行されているすべてのアプリケーションからアクセスできます。 場合によっては、このようなオープン性と可用性が望ましくないため、承認されていないクライアントからのアクセスに対してサービスをセキュリティで保護する必要があります。 リモートサービスへのアクセスを制限する方法の1つは、Android のアクセス許可を使用することです。

アクセス許可は、`Service` サブクラスを装飾 `ServiceAttribute` の `Permission` プロパティによって識別できます。 これにより、サービスにバインドするときにクライアントに付与する必要があるアクセス許可に名前が設定されます。 クライアントに適切なアクセス許可がない場合、クライアントがサービスにバインドしようとしたときに、Android によって `Java.Lang.SecurityException` がスローされます。

Android には、次の4種類のアクセス許可レベルがあります。

- **通常**&ndash; これは既定のアクセス許可レベルです。 これは、Android によって要求されたクライアントに自動的に付与できる危険度の低いアクセス許可を識別するために使用されます。 ユーザーは、これらのアクセス許可を明示的に付与する必要はありませんが、アプリの設定でアクセス許可を表示できます。
- **署名**&ndash; これは、同じ証明書で署名されたアプリケーションに Android によって自動的に付与される特別なアクセス許可のカテゴリです。 このアクセス許可は、アプリケーション開発者がアプリ間でコンポーネントやデータを簡単に共有できるように設計されています。ユーザーは、一定のかけるを必要としません。
- **Signatureorsystem** &ndash; これは、上記で説明した**署名**のアクセス許可と非常によく似ています。 このアクセス許可は、同じ証明書によって署名されたアプリに自動的に付与されるだけでなく、Android システムイメージと共にインストールされたアプリの署名に使用されたものと同じ証明書に署名されているアプリにも付与されます。 このアクセス許可は、通常、アプリケーションがサードパーティのアプリと連携できるようにするために、Android ROM 開発者によってのみ使用されます。 これは一般に、パブリックなの一般配布を目的としたアプリでは使用されません。
- **危険**な &ndash; のアクセス許可は、ユーザーにとって問題の原因となる可能性があります。 このため、**危険**なアクセス許可はユーザーによって明示的に承認される必要があります。

`signature` および `normal` のアクセス許可は Android によってインストール時に自動的に付与されるため、クライアントを含む APK の**前に**サービスをホストする apk をインストールすることが重要です。 クライアントが最初にインストールされた場合、Android はアクセス許可を付与しません。 この場合は、クライアント APK をアンインストールし、サービス APK をインストールしてから、クライアント APK を再インストールする必要があります。

Android のアクセス許可でサービスをセキュリティで保護するには、次の2つの一般的な方法があります。

1. 署名レベルのセキュリティ &ndash; 署名レベルのセキュリティを**実装**すると、サービスを保持する apk に署名するときに使用したものと同じキーで署名されたアプリケーションに対して、そのアクセス許可が自動的に付与されます。 これは、開発者が自分のアプリケーションからアクセス可能な状態を維持しながら、サービスをセキュリティで保護するための簡単な方法です。 署名レベルのアクセス許可を宣言するには、`ServiceAttribute` の `Permission` プロパティを `signature`に設定します。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2. サービスの開発者がサービスのカスタムアクセス許可を作成できるように &ndash;**カスタムアクセス許可を作成**します。 これは、開発者がサービスを他の開発者のアプリケーションと共有したい場合に最適です。 カスタムアクセス許可を実装するには多少の労力が必要であり、以下に説明します。

カスタム `normal` のアクセス許可を作成する簡単な例については、次のセクションで説明します。 Android のアクセス許可の詳細については、Google のドキュメントを参照して[& セキュリティに関するベストプラクティス](https://developer.android.com/training/articles/security-tips.html)を確認してください。 Android のアクセス許可の詳細については、android のアクセス許可の詳細については、アプリケーションマニフェストの Android ドキュメントの[アクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)に関するセクションを参照してください。

> [!NOTE]
> 一般に、Google はユーザーが混乱する可能性があるため、[カスタムアクセス許可の使用](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions)を避けることができます。

### <a name="creating-a-custom-permission"></a>カスタムアクセス許可の作成

カスタムアクセス許可を使用するには、クライアントが明示的にそのアクセス許可を要求している間、サービスによって宣言されます。

Service APK にアクセス許可を作成するには、`permission` 要素が**Androidmanifest**の `manifest` 要素に追加されます。 このアクセス許可には、`name`、`protectionLevel`、および `label` 属性が設定されている必要があります。 `name` 属性は、アクセス許可を一意に識別する文字列に設定する必要があります。 名前は、次のセクションに示すように、 **Android の設定**の**アプリ情報**ビューに表示されます。

`protectionLevel` 属性は、上記で説明した4つの文字列値のいずれかに設定する必要があります。  `label` と `description` は文字列リソースを参照する必要があり、ユーザーにわかりやすい名前と説明を提供するために使用されます。

このスニペットは、サービスを含む APK の**Androidmanifest .xml**でカスタム `permission` 属性を宣言する例です。

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

次に、クライアント APK の**Androidmanifest .xml**は、この新しいアクセス許可を明示的に要求する必要があります。 これを行うには、`users-permission` 属性を**Androidmanifest .xml**に追加します。

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

アプリケーションに付与されているアクセス許可を表示するには、Android 設定アプリを開き、 **[アプリ]** を選択します。 一覧でアプリケーションを見つけて選択します。 **[アプリ情報]** 画面で、 **[アクセス許可]** をタップすると、アプリに付与されているすべてのアクセス許可を表示するビューが表示されます。

[アプリケーションに付与されたアクセス許可を検索する方法を示すスクリーンショットを Android デバイスから ![する](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>要約

このガイドでは、リモートプロセスで Android サービスを実行する方法について詳しく説明しました。 ローカルサービスとリモートサービスの違いについて説明しました。また、リモートサービスが Android アプリの安定性とパフォーマンスに役立つ可能性がある理由についても説明しました。 リモートサービスを実装する方法と、クライアントがサービスと通信する方法について説明した後、このガイドでは、承認されたクライアントのみからサービスへのアクセスを制限する方法の1つを紹介しました。

## <a name="related-links"></a>関連リンク

- [ヘッダー](xref:Android.OS.Handler)
- [[メッセージ]](xref:Android.OS.Message)
- [Messenger](xref:Android.OS.Messenger)
- [ServiceAttribute](xref:Android.App.ServiceAttribute)
- [エクスポートされた属性](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [分離されたプロセスおよびカスタムアプリケーションクラスを含むサービスが、オーバーロードを正しく解決できない](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [プロセスとスレッド](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android マニフェスト-アクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [セキュリティのヒント](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-messengerservicedemo)
