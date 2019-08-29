---
title: リモートプロセスでの Android サービスの実行
description: 一般に、Android アプリケーションのすべてのコンポーネントは同じプロセスで実行されます。 Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。 このガイドでは、Xamarin を使用して Android リモートサービスを作成および使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2794a1d23cd7c1eab9cf4e94eaa805ad2b8bca61
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119131"
---
# <a name="running-android-services-in-remote-processes"></a>リモートプロセスでの Android サービスの実行

_一般に、Android アプリケーションのすべてのコンポーネントは同じプロセスで実行されます。Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。このガイドでは、Xamarin を使用して Android リモートサービスを作成および使用する方法について説明します。_

## <a name="out-of-process-services-overview"></a>アウトプロセスサービスの概要

アプリケーションの起動時に、Android によって、アプリケーションを実行するプロセスが作成されます。 通常、アプリケーションがこの1つのプロセスで実行するすべてのコンポーネントです。 Android services は、独自のプロセスで実行するように構成し、他の Android 開発者からのものを含め、他のアプリケーションと共有するように構成できるという点で重要な例外です。 これらの種類のサービスは、_リモートサービス_または_アウトプロセスサービス_と呼ばれます。 これらのサービスのコードは、メインアプリケーションと同じ APK に含まれます。ただし、サービスが開始されると、Android はそのサービスに対してのみ新しいプロセスを作成します。 これに対し、アプリケーションの他の部分と同じプロセスで実行されるサービスは、_ローカルサービス_と呼ばれることもあります。

一般に、アプリケーションでリモートサービスを実装する必要はありません。 多くの場合、アプリの要件にはローカルサービスが十分 (かつ望ましい) あります。 アウトプロセスには、Android で管理する必要がある独自のメモリ空間があります。 これにより、アプリケーション全体のオーバーヘッドが増加しますが、独自のプロセスでサービスを実行すると便利なシナリオがいくつかあります。

1. **共有機能**&ndash;アプリケーション開発者によっては、すべてのアプリケーション間で共有される複数のアプリと機能を持つ場合があります。 独自のプロセスで実行される Android サービスにその機能をパッケージ化すると、アプリケーションの保守が簡単になります。 また、独自のスタンドアロン APK でサービスをパッケージ化し、アプリケーションの他の部分とは別にデプロイすることもできます。
2. **ユーザーエクスペリエンスの向上**&ndash;アウトプロセスサービスがアプリケーションのユーザーエクスペリエンスを向上させる方法は2つあります。 最初の方法では、メモリ管理について扱います。 ガベージコレクション (GC) サイクルが発生すると、Android は GC が完了するまでプロセス内のすべてのアクティビティを一時停止します。 ユーザーは、この一時停止を "途切れている" または "jank" と感じることがあります。 サービスが独自のプロセスで実行されている場合は、アプリケーションプロセスではなく、一時停止されているサービスプロセスです。 この一時停止は、アプリケーションプロセス (したがってユーザーインターフェイス) が一時停止していないため、ユーザーにとってあまりわかりにくくなります。

    また、プロセスのメモリ要件が大きすぎると、Android はそのプロセスを終了してデバイスのリソースを解放する可能性があります。 サービスのメモリフットプリントが大きく、UI と同じプロセスで実行されている場合、Android がそれらのリソースを強制的に再利用すると、UI がシャットダウンされ、ユーザーにアプリの起動が強制されます。 ただし、独自のプロセスで実行されているサービスが Android によってシャットダウンされた場合、UI プロセスは影響を受けません。 UI では、サービスをバインド (および再起動) し、ユーザーに対して透過的に実行し、通常の動作を再開することができます。

3. **アプリケーションのパフォーマンスの向上**&ndash; UI プロセスは、サービスプロセスとは無関係に終了またはシャットダウンされる場合があります。 長いスタートアップタスクをアウトプロセスサービスに移動することで、UI の起動時間が短縮される可能性があります (これは、サービスプロセスが UI が起動されたときの時間の間に維持されることを前提としています)。

多くの点で、別のプロセスで実行されているサービスへのバインドは、[ローカルサービスへのバインド](~/android/app-fundamentals/services/creating-a-service/bound-services.md)と同じです。 クライアントはを呼び出し`BindService`て、サービスをバインドします (必要に応じて開始します)。 クライアントとサービスの間の接続を管理するオブジェクトが作成されます。`Android.OS.IServiceConnection` クライアントがサービスに正常にバインドした場合、Android はを介してオブジェクト`IServiceConnection`を返します。このオブジェクトを使用して、サービスでメソッドを呼び出すことができます。 クライアントは、このオブジェクトを使用してサービスと対話します。 確認するには、サービスにバインドする手順を次に示します。

- **インテントを作成**する&ndash;サービスにバインドするには、明示的なインテントを使用する必要があります。
- **オブジェクトを実装および`IServiceConnection`インスタンス化** &ndash;するオブジェクトは`IServiceConnection` 、クライアントとサービスの間の仲介役として機能します。  クライアントとサーバー間の接続を監視する役割があります。
- **`BindService`メソッド**を呼び出すと、前の手順で作成したインテントとサービス接続がAndroidにディスパッチされます。これにより、サービスの開始と通信の確立が行われます。&ndash; `BindService`クライアントとサービス。

プロセス間の境界を越える必要があるため、複雑さが増します。通信は一方向 (クライアントからサーバー) であり、クライアントはサービスクラスのメソッドを直接呼び出すことができません。 サービスがクライアントと同じプロセスを実行している場合、Android では、 `IBinder`双方向の通信を可能にするオブジェクトが提供されることに注意してください。 これは、サービスが独自のプロセスで実行されている場合には当てはまりません。 クライアントは、 `Android.OS.Messenger`クラスのヘルプを使用してリモートサービスと通信します。

クライアントがリモートサービスとのバインドを要求すると、Android は`Service.OnBind`ライフサイクルメソッドを呼び出します。このメソッドは、 `Messenger`によってカプセル化された内部`IBinder`オブジェクトを返します。 は、Android SDK によって提供さ`IBinder`れる特殊な実装に対するシンラッパーです。`Messenger` は`Messenger` 、2つの異なるプロセス間の通信を行います。 開発者は、メッセージのシリアル化、プロセス境界を越えたメッセージのマーシャリング、およびクライアントでの逆シリアル化の詳細について心配していません。 この作業は、 `Messenger`オブジェクトによって処理されます。 次の図は、クライアントがアウトプロセスサービスへのバインドを開始するときに関連するクライアント側の Android コンポーネントを示しています。

![サービスにバインドするクライアントの手順とコンポーネントを示す図](out-of-process-services-images/ipc-01.png "サービスにバインドするクライアントの手順とコンポーネントを示す図。")

リモートプロセスのクラスは、ローカルプロセス内のバインドされたサービスを通過する同じライフサイクルコールバックを通過します。また、関連する api の多くは同じです。 `Service` `Service.OnCreate`は、を`Handler`初期化し、それをオブジェクト`Messenger`に挿入するために使用されます。 同様に`OnBind` 、はオーバーライドされますが、 `IBinder`オブジェクトを返す代わりに、サービス`Messenger`はを返します。  次の図は、クライアントがサービスにバインドされている場合のサービスの動作を示しています。

![リモートクライアントによってバインドされるときにサービスが実行する手順とコンポーネントを示す図](out-of-process-services-images/ipc-02.png "リモートクライアントによってバインドされるときにサービスが実行する手順とコンポーネントを示す図。")

がサービスによって受信されると、の`Android.OS.Handler`インスタンスのによって処理されます。 `Message` サービスは、 `HandleMessage`メソッドをオーバーライド`Handler`する必要がある独自のサブクラスを実装します。 このメソッドは、 `Messenger`によって呼び出され、をパラメーター `Message`として受け取ります。 `Handler`は`Message`メタデータを検査し、その情報を使用してサービスでメソッドを呼び出します。

一方向の`Messenger.Send`通信は、クライアントがオブジェクトを`Message`作成し、メソッドを使用してそのオブジェクトをサービスにディスパッチするときに発生します。 `Messenger.Send`は、 `Message`シリアル化されたデータをから Android にシリアル化して、プロセスの境界を越えてサービスにメッセージをルーティングします。  サービス`Messenger`によってホストされているは`Message` 、受信データからオブジェクトを作成します。 これ`Message`はキューに格納され、メッセージは`Handler`に一度に1つずつ送信されます。 は、に含まれる`Message`メタデータを検査し、で適切なメソッド`Service`を呼び出します。 `Handler` 次の図は、これらのさまざまな動作について説明しています。

![プロセス間でメッセージが渡される方法を示す図](out-of-process-services-images/ipc-03.png "プロセス間でメッセージがどのように渡されるかを示す図")

このガイドでは、アウトプロセスサービスの実装の詳細について説明します。 独自のプロセスで実行することを意図したサービスを実装する方法と、 `Messenger`フレームワークを使用してクライアントがそのサービスと通信する方法について説明します。 また、双方向の通信についても簡単に説明します。クライアントは、サービスにメッセージを送信し、サービスはクライアントにメッセージを返信します。 サービスは異なるアプリケーション間で共有できるため、このガイドでは、Android のアクセス許可を使用してサービスへのクライアントアクセスを制限する方法の1つについても説明します。

> [!IMPORTANT]
> `IsolatedProcess` [Bugzilla 51940/GitHub 1950-分離プロセスおよびカスタムアプリケーションクラスを使用したサービスは、オーバーロードを正しく解決でき](https://github.com/xamarin/xamarin-android/issues/1950)ません。がに`true`設定されている場合、Xamarin. Android サービスが正しく起動しないことを報告します。 このガイドは、リファレンスのために用意されています。 Xamarin Android アプリケーションは、Java で記述されたアウトプロセスサービスと引き続き通信できなければなりません。

## <a name="requirements"></a>必要条件

このガイドは、サービスの作成に関する知識を前提としています。

以前の Android Api を対象とするアプリでは、暗黙的なインテントを使用することもできますが、このガイドでは明示的なインテントの使用についてのみ説明します。 Android 5.0 (API レベル 21) 以降を対象とするアプリでは、サービスとのバインドに明示的なインテントを使用する必要があります。これは、このガイドで説明されている手法です。「」を参照してください。

## <a name="create-a-service-that-runs-in-a-separate-process"></a>別のプロセスで実行されるサービスを作成する

前述のように、サービスが独自のプロセスで実行されているという事実は、いくつかの異なる Api が関係していることを意味します。 簡単な概要として、リモートサービスにバインドして使用する手順を次に示します。  

- **サブ`Service`クラス**サブ&ndash; クラス`Service`を作成し、バインドされたサービスのライフサイクルメソッドを実装します。 また、サービスが独自のプロセスで実行されることを Android に通知するメタデータを設定する必要もあります。
- `Handler` **`Handler`** を実装します。は、クライアント要求の分析、クライアントから渡されたパラメーターの抽出、およびサービスに&ndash;対する適切なメソッドの呼び出しを行います。
- 前の手順で説明した`Service`よう&ndash;に **、を`Messenger`インスタンス化**します。 `Messenger`各は、前の`Handler`手順で作成したにクライアント要求をルーティングするクラスのインスタンスを保持する必要があります。

独自のプロセスで実行することを意図したサービスは、基本的にはバインドされたサービスです。 サービスクラスは、基本`Service`クラスを拡張し、android マニフェストでバンドルする必要のあるメタデータ`ServiceAttribute`を含むで修飾されます。 まず、アウトプロセスサービスにとって重要`ServiceAttribute`なの次のプロパティを示します。

1. `Exported`他のアプリケーションがサービスと`true`対話できるようにするには、このプロパティをに設定する必要があります。 &ndash; このプロパティの既定値は `false` です。
2. `Process`&ndash;このプロパティを設定する必要があります。 これは、サービスが実行されるプロセスの名前を指定するために使用されます。
3. `IsolatedProcess`&ndash;このプロパティを使用すると、追加のセキュリティが有効になり、システムの他の部分と対話するための最小限のアクセス許可で、分離されたサンドボックスでサービスを実行するように Android に指示します。 「 [Bugzilla 51940-分離されたプロセスを含むサービス」および「カスタムアプリケーションクラスは、オーバーロードの解決に失敗](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)します」を参照してください。
4. `Permission`&ndash;クライアントが要求する (および付与される) 必要があるアクセス許可を指定することによって、サービスへのクライアントアクセスを制御することができます。

サービスを独自のプロセス`Process`で実行するには、の`ServiceAttribute`プロパティをサービスの名前に設定する必要があります。 外部のアプリケーションと対話するに`Exported`は、プロパティをに`true`設定する必要があります。 `Exported` が`false`の場合、同じ apk (同じアプリケーション) 内のクライアントと、同じプロセス内で実行されているクライアントのみがサービスと対話できます。

サービスが実行されるプロセスの種類は、 `Process`プロパティの値によって異なります。 Android では、次の3種類のプロセスを識別します。

- **プライベートプロセス**&ndash;プライベートプロセスは、それを開始したアプリケーションでのみ使用できます。 プロセスをプライベートとして識別するには、名前の先頭に **:** (セミコロン) を使用する必要があります。 前のコードスニペットとスクリーンショットに示されているサービスは、プライベートプロセスです。 の例を次の`ServiceAttribute`コードスニペットに示します。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

- **グローバルプロセス**&ndash;グローバルプロセスで実行されるサービスは、デバイス上で実行されているすべてのアプリケーションにアクセスできます。 グローバルプロセスは、小文字で始まる完全修飾クラス名でなければなりません。
    (サービスをセキュリティで保護するために手順を実行しない限り、他のアプリケーションはバインドして操作できます。 認証されていない使用に対してサービスをセキュリティで保護する方法については、このガイドで後ほど説明します)。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

- **分離プロセス**&ndash;分離プロセスは、システムの他の部分から分離された独自のサンドボックスで実行されるプロセスで、独自のアクセス許可はありません。 分離されたプロセス`IsolatedProcess`でサービスを実行するには、次のコードスニペットに示すように、 `ServiceAttribute`のプロパティをに`true`設定します。
    
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

プロパティの`Process`効果を確認するために、次のスクリーンショットは、独自のプライベートプロセスで実行されているサービスを示しています。

![プライベートプロセスで実行されているサービスを示すスクリーンショット](out-of-process-services-images/ipc-04.png "プライベートプロセスで実行されているサービスを示すスクリーンショット。")

次のスクリーンショット`Process="com.xamarin.xample.messengerservice.timestampservice_process"`は、グローバルプロセスで実行されているサービスを示しています。

![グローバルプロセスで実行されているサービスのスクリーンショット](out-of-process-services-images/ipc-05.png "グローバルプロセスで実行されているサービスのスクリーンショット。")

が設定されると、サービスはを`Handler`実装する必要があります。 `ServiceAttribute`

### <a name="implementing-a-handler"></a>ハンドラーの実装

クライアント要求を処理するには、サービスで`Handler`を実装し`HandleMessage` 、メソッドをオーバーライドする必要が`Message`あります。このメソッドは、クライアントからのメソッド呼び出しをカプセル化し、その呼び出しを何らかのアクションまたはタスクに変換するインスタンスを使用します。サービスが実行する。 オブジェクト`Message`は、という名前`What`のプロパティを公開します。このプロパティは、クライアントとサービスの間で共有され、サービスがクライアントに対して実行するタスクに関連します。

サンプルアプリケーションの次のコードスニペットは、の1つ`HandleMessage`の例を示しています。 この例では、クライアントがサービスに対して要求できる2つのアクションがあります。

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
                // The client as sent a simple Hello, say in the Android Log.
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

でサービスのパラメーターをパッケージ化することもでき`Message`ます。 これについては、このガイドの後半で説明します。 次に考慮すべきトピックは、 `Messenger`受信`Message`したを処理するオブジェクトを作成することです。

### <a name="instantiating-the-messenger"></a>Messenger のインスタンス化

既に説明したよう`Message`に、オブジェクト`Handler.HandleMessage`を逆シリアル化し`Messenger` 、を呼び出すことは、オブジェクトの役割です。 クラスには、クライアント`IBinder`がサービスにメッセージを送信するために使用するオブジェクトも用意されています。 `Messenger`  

サービスが開始されると、がインスタンス`Messenger`化され`Handler`、が挿入されます。 この初期化は、サービスの`OnCreate`メソッドに対して実行することをお勧めします。 次のコードスニペットは、独自`Handler`のとを`Messenger`初期化するサービスの一例です。

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

この時点で、最後の手順はがオーバーライド`Service` `OnBind`されることです。

### <a name="implementing-serviceonbind"></a>Service. OnBind の実装

バインドされた`OnBind`すべてのサービスは、独自のプロセスで実行されているかどうかにかかわらず、メソッドを実装する必要があります。 このメソッドの戻り値は、クライアントがサービスとの対話に使用できるいくつかのオブジェクトです。 そのオブジェクトがどのようなものであるかは、サービスがローカルサービスとリモートサービスのどちらであるかによって異なります。 ローカルサービスはカスタム`IBinder`実装を返しますが、リモートサービスはカプセル化`Messenger`さ`IBinder`れたを返しますが、前のセクションで作成されたを返します。

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

これら3つの手順を完了すると、リモートサービスは完了したと見なすことができます。

## <a name="consuming-the-service"></a>サービスの利用

すべてのクライアントは、リモートサービスをバインドして使用できるようにするために、いくつかのコードを実装する必要があります。 概念的には、クライアントの観点から見ると、ローカルサービスまたはリモートサービスへのバインドにはほとんど違いがありません。 クライアントは`BindService`メソッドを呼び出し、サービス`IServiceConnection`を識別するための明示的なインテントと、クライアントとサービスの間の接続の管理に役立つを渡します。

このコードスニペットは、リモートサービスにバインドするための**明示的なインテント**を作成する方法の例です。 インテントは、サービスを含むパッケージとサービスの名前を識別する必要があります。 この情報を設定する方法の1つと`Android.Content.ComponentName`して、オブジェクトを使用して目的を指定する方法があります。 このコードスニペットの例を次に示します。  

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

サービスがバインドされると、 `IServiceConnection.OnServiceConnected`メソッドが呼び出され、が`IBinder`クライアントに提供されます。 ただし、クライアントはを直接使用`IBinder`しません。 代わりに、その`IBinder`からオブジェクトを`Messenger`インスタンス化します。 これは、 `Messenger`クライアントがリモートサービスとの対話に使用するです。

次の例では、クライアントがサービス`IServiceConnection`との接続および切断を処理する方法を示す基本的な実装の例を示します。 メソッドは`OnServiceConnected`と`IBinder`を受け取り、クライアントはその`Messenger` `IBinder`からを作成します。

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

        IsConnected = service != null
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

サービス接続と目的が作成されると、クライアントはを呼び出し`BindService`てバインドプロセスを開始することができます。

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

クライアントがサービスに正常にバインド`Messenger`され、が使用可能になると、クライアントがサービスに送信`Messages`される可能性があります。

## <a name="sending-messages-to-the-service"></a>サービスへのメッセージの送信

クライアントが接続され、 `Messenger`オブジェクトがある場合、を`Messenger`介してオブジェクトをディスパッチ`Message`することで、サービスと通信することができます。 この通信は一方向であり、クライアントがメッセージを送信しますが、サービスからクライアントに返されるメッセージはありません。 この`Message`点で、は起動と破棄のメカニズムです。

オブジェクトを`Message`作成する場合は、 [`Message.Obtain`](xref:Android.OS.Message)ファクトリメソッドを使用することをお勧めします。 このメソッドは、Android `Message`によって管理されているグローバルプールからオブジェクトをプルします。 `Message.Obtain`には、サービスに必要な値`Message`とパラメーターを使用してオブジェクトを初期化できるようにするオーバーロードされたメソッドもいくつかあります。  がインスタンス化されると、を呼び出す`Messenger.Send`ことによってサービスにディスパッチされます。 `Message` このスニペットは、を`Message`作成してサービスプロセスにディスパッチする例の1つです。

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

メソッドには、 `Message.Obtain`いくつかの異なる形式があります。 前の例では[`Message.Obtain(Handler h, Int32 what)`](xref:Android.OS.Message.Obtain)、を使用しています。 これは、アウトプロセスサービスに対する非同期要求であるためです。サービスからの応答がないため、 `Handler`はに`null`設定されます。 2番目の`Int32 what`パラメーターは、 `Message`オブジェクトの`.What`プロパティに格納されます。 `.What`プロパティは、サービスプロセスのコードによって、サービスでメソッドを呼び出すために使用されます。

また`Message` 、クラスは、 `Arg1`受信者に使用できる2つの追加プロパティであると`Arg2`も公開します。 これらの2つのプロパティは、クライアントとサービスの間に意味のある特別な合意値を持つ可能性がある整数値です。 たとえば、 `Arg1`は顧客 ID を保持し`Arg2` 、その顧客の注文書番号を保持する場合があります。 は、の`Message`作成時に、2つのプロパティを設定するために使用[できます。`Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)`](xref:Android.OS.Message.Obtain) この2つの値を設定するもう1つ`.Arg`の`.Arg2`方法は、 `Message`オブジェクトが作成された後に、プロパティとプロパティを直接設定することです。

### <a name="passing-additional-values-to-the-service"></a>サービスに追加の値を渡す

を`Bundle`使用すると、より複雑なデータをサービスに渡すことができます。 この場合、追加の値をに配置`Bundle`し、 `Message`と共に送信する前に[ `.Data`プロパティ](xref:Android.OS.Message.Data)プロパティを設定することによって送信できます。

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

この時点までに説明したメッセージングアーキテクチャは一方向であり、クライアントがメッセージをサービスに送信します。 サービスがクライアントに値を返す必要がある場合は、この点について説明したすべてのものが元に戻されます。 サービスはを作成`Message`し、戻り値をパッケージ化し、を`Message`介し`Messenger`てをクライアントにディスパッチする必要があります。 ただし、サービスはそれ自体`Messenger`を作成するのではなく、最初の要求の一部として、 `Messenger`クライアントのインスタンス化とパッケージ化に依存します。 サービスは、 `Send`このクライアントによって提供`Messenger`されるを使用してメッセージを表示します。  

双方向通信のイベントのシーケンスは次のとおりです。

1. クライアントがサービスにバインドされます。 サービスとクライアントが接続すると、クライアント`IServiceConnection`によって管理されるは、をサービスに送信`Messenger` `Message`するために使用されるオブジェクトへの参照を保持します。 混乱を避けるため、これは_サービスメッセンジャー_と呼ばれます。
2. クライアントは、 `Handler` (_クライアントハンドラー_と呼ばれます) をインスタンス化し、それを`Messenger`使用して独自の (_クライアント Messenger_) を初期化します。 サービスメッセンジャーとクライアント Messenger は、2つの異なる方向のトラフィックを処理する2つの異なるオブジェクトであることに注意してください。 サービスメッセンジャーは、クライアントからサービスへのメッセージを処理します。また、クライアント Messenger は、サービスからクライアントへのメッセージを処理します。
3. クライアントは`Message`オブジェクトを作成し、 `ReplyTo`プロパティをクライアント Messenger に設定します。 メッセージは、サービスメッセンジャーを使用してサービスに送信されます。
4. サービスは、クライアントからメッセージを受信し、要求された作業を実行します。
5. サービスがクライアントに応答を送信する時刻になると、はを使用`Message.Obtain`して新しい`Message`オブジェクトを作成します。
6. このメッセージをクライアントに送信するために、サービスはクライアントメッセージの`.ReplyTo`プロパティからクライアント Messenger を抽出し、 `.Send`それ`Message`をクライアントに返します。
7. 応答がクライアントによって受信されると、クライアントは`Handler` 、 `.What`プロパティを調べ`Message`て (必要に応じて、 `Message`に格納されているパラメーターを抽出して) 処理する独自のを持ちます。

このサンプルコードでは、 `Message`クライアントがをインスタンス化し、サービスが応答に使用するを`Messenger`パッケージ化する方法を示します。

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

サービスは、 `Messenger`を抽出し、それを`Handler`使用してクライアントに応答を送信するために、自身に何らかの変更を加える必要があります。 このコードスニペットは、サービス`Handler`がを`Message`作成してクライアントに送信する方法の例です。  

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

上記`Messenger`のコードサンプルでは、クライアントによって作成されたインスタンスが、サービスによって受信されるオブジェクトと同じでは*ない*ことに注意してください。 これらは、通信`Messenger`チャネルを表す2つの異なるプロセスで実行される2つの異なるオブジェクトです。

## <a name="securing-the-service-with-android-permissions"></a>Android アクセス許可を使用してサービスをセキュリティで保護する

グローバルプロセスで実行されるサービスは、その Android デバイスで実行されているすべてのアプリケーションからアクセスできます。 場合によっては、このようなオープン性と可用性が望ましくないため、承認されていないクライアントからのアクセスに対してサービスをセキュリティで保護する必要があります。 リモートサービスへのアクセスを制限する方法の1つは、Android のアクセス許可を使用することです。

権限は、 `Permission` `ServiceAttribute` `Service`サブクラスを装飾するのプロパティによって識別できます。 これにより、サービスにバインドするときにクライアントに付与する必要があるアクセス許可に名前が設定されます。 クライアントに適切なアクセス許可がない場合、クライアントがサービスにバインド`Java.Lang.SecurityException`しようとすると、Android からがスローされます。

Android には、次の4種類のアクセス許可レベルがあります。

- **標準**&ndash;これは、既定のアクセス許可レベルです。 これは、Android によって要求されたクライアントに自動的に付与できる危険度の低いアクセス許可を識別するために使用されます。 ユーザーは、これらのアクセス許可を明示的に付与する必要はありませんが、アプリの設定でアクセス許可を表示できます。
- **署名**&ndash;これは、同じ証明書で署名されたアプリケーションに Android によって自動的に付与される特別なアクセス許可のカテゴリです。 このアクセス許可は、アプリケーション開発者がアプリ間でコンポーネントやデータを簡単に共有できるように設計されています。ユーザーは、一定のかけるを必要としません。
- **Signatureorsystem**これは、上記で説明した署名のアクセス許可とよく似ています。 &ndash; このアクセス許可は、同じ証明書によって署名されたアプリに自動的に付与されるだけでなく、Android システムイメージと共にインストールされたアプリの署名に使用されたものと同じ証明書に署名されているアプリにも付与されます。 このアクセス許可は、通常、アプリケーションがサードパーティのアプリと連携できるようにするために、Android ROM 開発者によってのみ使用されます。 これは一般に、パブリックなの一般配布を目的としたアプリでは使用されません。
- **危険**&ndash;危険なアクセス許可は、ユーザーにとって問題の原因となる可能性があります。 このため、**危険**なアクセス許可はユーザーによって明示的に承認される必要があります。

`signature` および`normal`のアクセス許可は、Android によってインストールされるときに自動的に付与されるため、クライアントを含む apk の**前に**、サービスをホストする apk をインストールすることが重要です。 クライアントが最初にインストールされた場合、Android はアクセス許可を付与しません。 この場合は、クライアント APK をアンインストールし、サービス APK をインストールしてから、クライアント APK を再インストールする必要があります。

Android のアクセス許可でサービスをセキュリティで保護するには、次の2つの一般的な方法があります。

1. **署名レベルのセキュリティを実装**する&ndash;署名レベルのセキュリティとは、サービスを保持する apk への署名に使用されたのと同じキーで署名されたアプリケーションに対して、アクセス許可が自動的に付与されることを意味します。 これは、開発者が自分のアプリケーションからアクセス可能な状態を維持しながら、サービスをセキュリティで保護するための簡単な方法です。 シグネチャレベルのアクセス許可は、 `Permission` `ServiceAttribute`のプロパティをに設定`signature`することによって宣言されます。

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2. **カスタムアクセス許可を作成**する&ndash;サービスの開発者は、サービスのカスタムアクセス許可を作成することができます。 これは、開発者がサービスを他の開発者のアプリケーションと共有したい場合に最適です。 カスタムアクセス許可を実装するには多少の労力が必要であり、以下に説明します。

カスタム`normal`アクセス許可を作成する簡単な例については、次のセクションで説明します。 Android のアクセス許可の詳細については、Google のドキュメントを参照して[& セキュリティに関するベストプラクティス](https://developer.android.com/training/articles/security-tips.html)を確認してください。 Android のアクセス許可の詳細については、android のアクセス許可の詳細については、アプリケーションマニフェストの Android ドキュメントの[アクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)に関するセクションを参照してください。

> [!NOTE]
> 一般に、Google はユーザーが混乱する可能性があるため、[カスタムアクセス許可の使用](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions)を避けることができます。

### <a name="creating-a-custom-permission"></a>カスタムアクセス許可の作成

カスタムアクセス許可を使用するには、クライアントが明示的にそのアクセス許可を要求している間、サービスによって宣言されます。

Service apk でアクセス許可を作成するには`permission` 、要素を**androidmanifest**の`manifest`要素に追加します。 このアクセス許可には`name`、 `protectionLevel`、、 `label`およびの各属性が設定されている必要があります。 `name`属性は、アクセス許可を一意に識別する文字列に設定する必要があります。 名前は、次のセクションに示すように、 **Android の設定**の**アプリ情報**ビューに表示されます。

属性`protectionLevel`は、上記で説明した4つの文字列値のいずれかに設定する必要があります。  `label` と`description`は、文字列リソースを参照する必要があり、ユーザーにわかりやすい名前と説明を提供するために使用されます。

このスニペットは、サービスを含む apk `permission`の**androidmanifest .xml**でカスタム属性を宣言する例です。

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

次に、クライアント APK の**Androidmanifest .xml**は、この新しいアクセス許可を明示的に要求する必要があります。 これを行うには、 `users-permission`属性を**androidmanifest .xml**に追加します。

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

[![アプリケーションに付与されたアクセス許可を検索する方法を示す、Android デバイスのスクリーンショット](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Summary

このガイドでは、リモートプロセスで Android サービスを実行する方法について詳しく説明しました。 ローカルサービスとリモートサービスの違いについて説明しました。また、リモートサービスが Android アプリの安定性とパフォーマンスに役立つ可能性がある理由についても説明しました。 リモートサービスを実装する方法と、クライアントがサービスと通信する方法について説明した後、このガイドでは、承認されたクライアントのみからサービスへのアクセスを制限する方法の1つを紹介しました。


## <a name="related-links"></a>関連リンク

- [ヘッダー](xref:Android.OS.Handler)
- [メッセージ](xref:Android.OS.Message)
- [Messenger](xref:Android.OS.Messenger)
- [ServiceAttribute](xref:Android.App.ServiceAttribute)
- [エクスポートされた属性](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [分離されたプロセスおよびカスタムアプリケーションクラスを含むサービスが、オーバーロードを正しく解決できない](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [プロセスとスレッド](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android マニフェスト-アクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [セキュリティのヒント](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-messengerservicedemo)
