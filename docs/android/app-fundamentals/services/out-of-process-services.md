---
title: "リモート プロセスで実行されている Android サービス"
description: "一般に、Android アプリケーションのすべてのコンポーネントは、同じプロセスで実行されます。 Android のサービスは、この主な例外、独自のプロセスで実行するように構成およびその他の Android 開発者のものを含む、他のアプリケーションと共有できます。 このガイドでは、作成して、Xamarin を使用して Android のリモート サービスを使用する方法を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: ebbb4b527b27b87bb6357723978e730304658720
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2018
---
# <a name="running-android-services-in-remote-processes"></a>リモート プロセスで実行されている Android サービス

_一般に、Android アプリケーションのすべてのコンポーネントは、同じプロセスで実行されます。Android のサービスは、この主な例外、独自のプロセスで実行するように構成およびその他の Android 開発者のものを含む、他のアプリケーションと共有できます。このガイドでは、作成して、Xamarin を使用して Android のリモート サービスを使用する方法を説明します。_

## <a name="out-of-process-services-overview"></a>アウト プロセス サービスの概要の

アプリケーション起動すると、Android は、アプリケーションを実行するためのプロセスを作成します。 通常、すべてのコンポーネント、アプリケーションで実行されますこの 1 つのプロセス。 Android のサービスは、この主な例外、独自のプロセスで実行するように構成およびその他の Android 開発者のものを含む、他のアプリケーションと共有できます。 この種のサービスと呼びます_リモート サービス_または_アウト プロセス サービス_です。 これらのサービスのコードがメイン アプリケーションとして同じ APK に含まれるただし、サービスを起動すると Android では、そのサービスだけの新しいプロセスを作成します。 これに対し、他のアプリケーションと同じプロセスで実行されているサービスとも呼ば、_ローカル サービス_です。

一般に、リモート サービスを実装するアプリケーションの必要はありません。 ローカル サービスは、十分な (と望ましい) の多くの場合、アプリの要件です。 アウト プロセスが、それ自身のメモリ領域 Android によって管理する必要があります。 全体的なアプリケーションに複数のオーバーヘッドが生じるこれは、ことができます独自のプロセスで、サービスを実行すると便利ないくつかのシナリオがあります。

1. **共有機能**&ndash;一部のアプリケーション開発者は、複数のアプリおよびすべてのアプリケーション間で共有される機能を必要があります。 独自のプロセスの実行に使用するには、アプリケーションの保守が簡略化されます Android のサービスにこの機能をパッケージ化します。 独自のスタンドアロン APK でサービスをパッケージ化し、アプリケーションの残りの部分から個別に配置することもできます。
2. **ユーザー エクスペリエンスを向上させる**&ndash;はアウト プロセス サービスが、アプリケーションのユーザー エクスペリエンスを向上できます 2 つの可能な方法があります。 最初の方法は、メモリ管理について説明します。 ガベージ コレクション (GC) サイクルが発生、GC が完了するまでは、Android にプロセスのすべてのアクティビティが一時停止されます。 ユーザーは、「途切れること」または"jank"として一時停止を感じる可能性があります。 サービスを実行する場合は、サービス プロセスが一時停止しているアプリケーション プロセスではなく、独自のプロセスです。 一時停止はアプリケーション プロセス (とそのため、ユーザー インターフェイス) 一時停止されていないとユーザーにはるかに小さくなります。

    次に、プロセスのメモリ要件が大きくなりすぎた場合 Android は、デバイスのリソースを解放するには、そのプロセスを強制終了可能性があります。 サービスに大量のメモリ フット プリントは、UI と同じプロセスで実行されている場合は、し、Android が強制的にそれらのリソースをクリアするときに、UI はシャット ダウン、アプリを起動するユーザーを強制します。 ただし、独自のプロセスで実行されている、サービスは、Android によってシャット ダウンされた場合でも UI プロセスは影響を受けません。 UI ことができますバインド (および再起動)、サービスは、ユーザー、および再開正常な機能に透過的です。

3. **アプリケーションのパフォーマンスを向上させる** &ndash; UI プロセスを終了またはシャット ダウン サービス プロセスに依存しない可能性があります。 時間のかかるスタートアップ タスクをアウト プロセス サービスに移動、(サービスのプロセスでは、UI を起動する時間の間は保持) する場合こと、UI の起動時間が改善状況によって異なることができます。

別のプロセスで実行されているサービスへのバインディングでは、さまざまな方法でと同じ[ローカル サービスへのバインド](~/android/app-fundamentals/services/creating-a-service/bound-services.md)です。 クライアントが呼び出す`BindService`バインド (を開始すると、必要な場合) をサービスします。 `Android.OS.IServiceConnection`クライアントとサービス間の接続を管理するオブジェクトが作成されます。 かどうか、クライアントは、サービスに正常にバインドし、Android を使用してオブジェクトを返しますが、`IServiceConnection`を使用して、サービスのメソッドを呼び出すことができます。 クライアントは、このオブジェクトを使用して、サービスと対話します。 確認するには、サービスにバインドする手順を次に示します。

* **目的の作成**&ndash;明示的な目的はサービスへのバインディングに使用する必要があります。
* **実装し、インスタンス化、`IServiceConnection`オブジェクト** &ndash; 、`IServiceConnection`オブジェクトは、クライアントとサービス間の媒介として機能します。  クライアントとサーバー間の接続を監視するために行います。
* **呼び出す、`BindService`メソッド**&ndash;呼び出す`BindService`目的および Android で、サービスの開始との通信を確立する処理は前の手順で作成したサービスの接続にディスパッチされますクライアントとサービス。

プロセス境界を越える必要性は余分な複雑さを導入します。 通信が一方向 (クライアントからサーバー) と、クライアントはサービス クラスのメソッドを直接呼び出すことはできません。 サービスは、クライアントと同じプロセスを実行中は、Android が提供する取り消し、`IBinder`の双方向通信できるようになるオブジェクト。 これは、独自のプロセスで実行されているサービスの場合とではありません。 リモート サービスでクライアントが通信するのヘルプ、`Android.OS.Messenger`クラスです。

Android を呼び出すクライアントを要求すると、リモート サービス バインド、`Service.OnBind`ライフ サイクル メソッドは、内部戻ります`IBinder`オブジェクトによってカプセル化される、`Messenger`です。 `Messenger`のシン ラッパーが特殊な上`IBinder`Android SDK によって提供されている実装します。 `Messenger` 2 つの異なるプロセスの間の通信を行います。 開発者は、メッセージをシリアル化する、プロセス境界を越えてメッセージをマーシャ リングし、クライアントの逆シリアル化の詳細関係ありません。 この作業はによって処理される、`Messenger`オブジェクト。 この図は、クライアントが、アウト プロセス サービスへのバインドを開始したときに、含まれているクライアント側の Android コンポーネントを示しています。

![バインド、サービスにクライアントの手順を実行し、コンポーネントを示す図](out-of-process-services-images/ipc-01.png "バインド サービスにクライアントの手順を実行してコンポーネントを示す図。")

`Service`リモート プロセス内のクラスを同じライフ サイクルが実行するコールバック ローカル プロセスにバインドされているサービスを通過、通過し、関連する Api の多くは同じです。 `Service.OnCreate` 初期化するために使用される、`Handler`への挿入と`Messenger`オブジェクト。 同様に、`OnBind`はオーバーライドされた場合を返す代わりに、 `IBinder` 、サービスは、オブジェクトを返します、`Messenger`です。  この図は、クライアントをバインドする場合に、サービスでの動作を示しています。

![リモート クライアントによってバインドされるときを示す図の手順とコンポーネント サービスを通過](out-of-process-services-images/ipc-02.png "を示す図の手順とコンポーネント サービスを通過してリモート クライアントにバインドされているときにします。")

ときに、`Message`受信のインスタンスで、サービスによって処理される`Android.OS.Handler`です。 サービスの実装は、独自`Handler`サブクラスをオーバーライドする必要があります`HandleMessage`メソッドです。 このメソッドを呼び出す、`Messenger`を受け取ると、`Message`をパラメーターとして。 `Handler`が検査され、`Message`メタ データその情報を使用して、サービスでメソッドを呼び出すとします。

クライアントを作成するときに発生する一方向の通信、`Message`オブジェクトを使用して、サービスにディスパッチ、`Messenger.Send`メソッドです。 `Messenger.Send` シリアル化、`Message`と手の Android で、プロセス境界を越えてメッセージをルーティングして、サービスからデータをシリアル化します。  `Messenger`によってホストされているサービスで作成、`Message`受信データからのオブジェクト。 これは、`Message`メッセージが送信された 1 つずつは、キューに配置されますが、`Handler`です。 `Handler`に含まれているメタデータを検査、`Message`に対して適切なメソッドを呼び出すと、`Service`です。 次の図は、これらのさまざまな概念の動作を示しています。

![プロセス間でメッセージを渡す方法を示す図](out-of-process-services-images/ipc-03.png "プロセス間でメッセージを渡す方法を示す図。")

このガイドでは、アウト プロセス サービスの実装の詳細について説明します。 クライアントが通信する方法を使用してそのサービスを独自のプロセスで実行するものでは、サービスを実装する方法を説明します、`Messenger`フレームワークです。 双方向の通信も簡単に説明します。 サービスとクライアントにメッセージを送信するサービスにメッセージを送信するクライアント。 サービスは、異なるアプリケーション間で共有できる、ため、このガイドも Android 権限を使用して、サービスへのクライアント アクセスを制限するための 1 つの方法を説明します。

> [!IMPORTANT]
> [Bugzilla 51940 のオーバー ロードを正常に解決するのには分離プロセスおよびカスタム アプリケーションのクラスとサービスが失敗する](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)Xamarin.Android サービスが正しく起動をしないレポートと、`IsolatedProcess`に設定されている`true`です。 このガイドの参照が指定されます。 Xamarin.Android アプリケーションは、Java で記述されたプロセス外のサービスと通信できる必要があります。

## <a name="requirements"></a>要件

このガイドは、サービスの作成に関する知識を前提とします。

古い対象とするアプリでの暗黙的なインテントを使用することはできますが、Android Api では、このガイドは、明示的なインテントの使用に集中はします。 Android 5.0 (API レベル 21) を対象とするアプリケーション、サービスにバインドする明示的な目的を使用する必要があります以降またはこれは、このガイドで説明する手法です.

## <a name="create-a-service-that-runs-in-a-separate-process"></a>別のプロセスで実行されているサービスを作成します。

前述のように、サービスが、独自のプロセスで実行されているファクトは、いくつか異なる Api が含まれることを意味します。 簡単な概要としてバインドし、リモート サービスを使用する手順を次に示します。  

* **作成、`Service`サブクラス**&ndash;サブクラス、`Service`を入力し、バインドされているサービスのライフ サイクル メソッドを実装します。 サービスは、独自のプロセスで実行する Android お手伝いするメタデータを設定する必要もできます。
* **実装、 `Handler`**  &ndash; 、`Handler`はクライアントからの要求を分析する、クライアントから渡されたパラメーターを抽出し、サービスに適切なメソッドの呼び出しを担当します。
* **インスタンスを作成、 `Messenger`**  &ndash;前述のとおり、各`Service`のインスタンスを管理する必要があります、`Messenger`へのクライアント要求をルーティングするクラス、`Handler`前の手順で作成されました。

独自のプロセスで実行するものでは、サービスは、基本的には、まだバインドされているサービスです。 ベースのサービス クラスが継承されます`Service`クラスおよびで修飾された、 `ServiceAttribute` Android は、Android のマニフェストにバンドルする必要があるメタ データを格納します。 次のプロパティで始まる、`ServiceAttribute`アウト プロセス サービスに重要な。

1. `Exported` &ndash; このプロパティに設定する必要があります`true`他のアプリケーション、サービスとの対話に使用できるようにします。 このプロパティの既定値は `false` です。
2. `Process` &ndash; このプロパティを設定する必要があります。 これを使用して、サービスで実行されるプロセスの名前を指定します。
3. `IsolatedProcess` &ndash; このプロパティは、システムの残りの部分で iteract する最小限の権限を持つ分離のサンド ボックスで、サービスを実行する Android を示す追加のセキュリティを有効になります。 参照してください[オーバー ロードを解決するには、適切に分離プロセスおよびカスタム アプリケーションのクラスと Bugzilla 51940 - サービスが失敗する](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)です。
4. `Permission` &ndash; クライアント要求する必要があります (および付与) できる権限を指定することで、サービスへのクライアント アクセスを制御することができます。

独自のプロセスでは、サービスを実行する、`Process`プロパティを`ServiceAttribute`サービスの名前に設定する必要があります。 外部のアプリケーションと対話する、`Exported`プロパティに設定する必要があります`true`です。 場合`Exported`は`false`、同じ APK (つまり、同じアプリケーション) でのみのクライアントと同じプロセスで実行できるよう、サービスとの対話になります。

プロセスで、サービスは実行の種類の値によって異なります、`Process`プロパティです。 Android では、次の 3 つの異なる種類のプロセスを識別します。

-   **プライベート プロセス**&ndash;プライベート プロセスは、1 つだけがそれを開始したアプリケーションを使用できます。 プライベート プロセスを識別するための名前で始まらなければなりません、 **:** (セミコロンで区切ります)。 前のコード スニペットとスクリーン ショットで示されているサービスは、プライベート プロセスです。 次のコード スニペットの例に示します、 `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **グローバル プロセス**&ndash;グローバル プロセス内で実行されるサービスにデバイスで実行されているすべてのアプリケーションにアクセスします。 グローバル プロセスは、小文字の文字で始まる完全修飾クラス名である必要があります。
    (サービスをセキュリティで保護するには、手順を実行、しない限り、他のアプリケーション可能性がありますバインドし、対話します。 不正アクセスからサービスのセキュリティ保護については説明このガイドで後述します。)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **プロセスを分離**&ndash;分離プロセスは、独自のない特殊なアクセス許可と、システムの残りの部分から分離された独自サンド ボックス内で実行されるプロセスです。 分離のプロセスでサービスを実行する、`IsolatedProcess`のプロパティ、`ServiceAttribute`に設定されている`true`このコード スニペットに示すように。
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> 参照してください[オーバー ロードを解決するには、適切に分離プロセスおよびカスタム アプリケーションのクラスと Bugzilla 51940 - サービスが失敗します。](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

分離されたサービスは、アプリケーションおよび信頼されていないコードに対して、デバイスを保護する簡単な方法です。 たとえば、アプリをダウンロードして、web サイトからスクリプトを実行します。 この場合、分離プロセスで実行する、追加の信頼できないコードが Android デバイスを損なうことに対するセキュリティ層を提供します。

> [!IMPORTANT]
> サービスはエクスポート後、サービスの名前は変更しないでください。 サービスの名前を変更すると、サービスを使用している他のアプリケーションが壊れる可能性があります。

影響を確認すること、`Process`プロパティには、次のスクリーン ショットは、独自のプライベート プロセスで実行されているサービスを示しています。

![プライベート プロセスで実行されているサービスを示すスクリーン ショット](out-of-process-services-images/ipc-04.png "プライベート プロセスで実行されているサービスを示すスクリーン ショット。")

この次のスクリーン ショットは`Process="com.xamarin.xample.messengerservice.timestampservice_process"`とグローバル プロセスで実行されているサービス。

![グローバルのプロセスで実行されているサービスのスクリーン ショット](out-of-process-services-images/ipc-05.png "グローバル プロセスで実行されているサービスのスクリーン ショット。")

1 回、`ServiceAttribute`が設定されている、実装する必要のあるサービス、`Handler`です。

### <a name="implementing-a-handler"></a>ハンドラーを実装します。

クライアント要求を処理するには、サービスを実装する必要があります、`Handler`をオーバーライドし、 `HandleMessage` methodThis はメソッドは、`Message`インスタンスがどのクライアントからのメソッド呼び出しをカプセル化し、何らかのアクションには、その呼び出しを変換またはサービスが実行するタスク。 `Message`オブジェクトと呼ばれるプロパティを公開する`What`は整数値の意味は、クライアントとサービス間で共有し、サービスが、クライアントに対して実行するいくつかのタスクに関連します。

サンプル アプリケーションから次のコード スニペットは、1 つの例を示しています。`HandleMessage`です。 この例では、サービスのクライアントが要求できる 2 つのアクションがあります。

* 最初の操作が、_こんにちは, World_メッセージ、クライアントが、単純なメッセージ サービスに送信します。
* 2 番目のアクションは、サービスに対してメソッドを呼び出すし、文字列を取得、文字列はメッセージが実行されたサービス期間、および開始時刻を返しますが、ここでは。

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
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

サービスのパッケージ パラメーターをすることも、`Message`です。 これについては、このガイドの後半で説明します。 考慮すべき次のトピックを作成する、`Messenger`受信を処理するオブジェクト`Message`s。

### <a name="instantiating-the-messenger"></a>メッセンジャーをインスタンス化します。

既に説明したよう、逆シリアル化、`Message`オブジェクトと呼び出し`Handler.HandleMessage`の responsibilty は、`Messenger`オブジェクト。 `Messenger`クラスも用意されています、`IBinder`オブジェクトをクライアントがサービスにメッセージを送信する使用されます。  

インスタンス化され、サービスが開始されるときに、`Messenger`を挿入し、`Handler`です。 この初期化を実行するに適してが上、`OnCreate`サービスのメソッドです。 このコード スニペットはそれ自体を初期化するサービスの 1 つの例`Handler`と`Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

最後の手順は、この時点で、`Service`をオーバーライドする`OnBind`です。

### <a name="implementing-serviceonbind"></a>Service.OnBind を実装します。

か、それぞれ独自のプロセスで実行するかどうか、バインドされたすべてのサービスを実装する必要があります、`OnBind`メソッドです。 このメソッドの戻り値は、クライアントがサービスとの対話に使用できるいくつかのオブジェクトです。 正確にそのオブジェクトは、サービスがローカル サービスまたはリモート サービスがあるかどうかによって異なります。 ローカル サービスは、カスタムを返す`IBinder`実装では、リモート サービスが返されます、`IBinder`はカプセル化されているが、`Messenger`前のセクションで作成されました。

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

これら 3 つのステップが完了した後、リモート サービス完了したと見なすです。

## <a name="consuming-the-service"></a>サービスの使用

すべてのクライアントをバインドし、リモート サービスを利用できる一部のコードを実装する必要があります。 概念的には、クライアントの視点からはローカル サービスまたはリモート サービスへのバインドの非常にいくつかの相違です。 クライアントを起動、`BindService`サービスの識別に明示的なインテントを渡して、メソッドと`IServiceConnection`クライアントとサービス間の接続の管理を支援します。

このコード スニペットを作成する方法の例は、**明示的な目的とした**リモート サービスにバインドするためです。 目的は、サービスとサービスの名前を含むパッケージを識別する必要があります。 この情報を設定する方法の 1 つを使用して、`Android.Content.ComponentName`オブジェクトに提供を目的とします。 このコード スニペットは、1 つの例を示します。  

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

サービスがバインドされている場合、`IServiceConnection.OnServiceConnected`メソッドが呼び出され、提供、`IBinder`クライアントにします。 ただし、クライアントが直接使用されません、`IBinder`です。 代わりに、インスタンス化され、`Messenger`オブジェクトをから`IBinder`です。 これは、`Messenger`クライアントがリモート サービスとの対話に使用します。

非常に基本的な例を次に示します`IServiceConnection`クライアントに接続して、サービスからの切断をどのように処理する方法を示していますの実装です。 注意して、`OnServiceConnected`メソッドは受信と`IBinder`、クライアントを作成し、`Messenger`から`IBinder`:

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

サービスの接続と、目的としたが作成されると、可能であれば、クライアントが呼び出すための`BindService`バインディング プロセスを開始します。

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

クライアントがサービスにバインドが正常に後および`Messenger`は、使用可能なことができますをクライアントが送信`Messages`サービスにします。

## <a name="sending-messages-to-the-service"></a>サービスにメッセージを送信します。

クライアントが接続されていると、`Messenger`オブジェクトをディスパッチすることで、サービスと通信することは`Message`オブジェクトを使用して、`Messenger`です。 この通信が一方向、クライアントがメッセージを送信がクライアントにサービスから応答メッセージはありません。 この点で、`Message`火災忘れたメカニズムです。

作成することをお勧め、`Message`オブジェクトは、使用する、 [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods)ファクトリ メソッド。 このメソッドは、取得、 `Message` Android によって管理されているグローバル プールからのオブジェクト。 `Message.Obtain` または一部オーバー ロードされたメソッドを許可する、`Message`サービスに必要なパラメーターと値で初期化されるオブジェクト。  1 回、`Message`がインスタンス化すると、そのサービスにディスパッチを呼び出して`Messenger.Send`です。 このスニペットの作成とディスパッチの 1 つの例とは、`Message`サービス プロセス。

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

いくつかの異なる形式の`Message.Obtain`メソッドです。 前の例では、 [ `Message.Obtain(Handler h, Int32 what)`](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/)です。 これは、アウト プロセス サービスへの非同期要求であるため行われません、サービスからの応答をそのため、`Handler`に設定されている`null`です。 2 番目のパラメーターでは、`Int32 what`に格納されます、`.What`のプロパティ、`Message`オブジェクト。 `.What`プロパティは、サービスでメソッドを呼び出すサービス プロセス内のコードによって使用されます。

`Message`クラスも、recipent を使用する必要があります、2 つのプロパティを公開:`Arg1`と`Arg2`です。 これら 2 つのプロパティは、可能性のある一部の特殊合意がクライアントとサービス間の意味を持つ値の整数値です。 たとえば、`Arg1`顧客 ID を保持することがあると`Arg2`その顧客の購買発注書番号を保持できます。 [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/)を 2 つのプロパティを設定するために使用するときに、`Message`を作成します。 これら 2 つの値を設定する別の方法は、設定する、`.Arg`と`.Arg2`プロパティに直接、`Message`が作成された後のオブジェクトします。

### <a name="passing-additional-values-to-the-service"></a>サービスに追加の値を渡す

使用して、サービスにより複雑なデータを渡すことは、`Bundle`です。 余分な値を配置するこの例では、`Bundle`と共に送信されると、`Message`を設定して、 [ `.Data`プロパティ](https://developer.xamarin.com/api/property/Android.OS.Message.Data/)送信する前にプロパティです。

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> 一般に、 `Message` 1 MB を超えるペイロードにはできません。 Android のバージョンに基づいて、サイズの上限が異なる場合があり、独自の変更、仕入先の実装の Android オープン ソース プロジェクト (AOSP) デバイスがバンドルされていることになった可能性があります。

## <a name="returning-values-from-the-service"></a>サービスからの戻り値

この点を説明しましたのメッセージング アーキテクチャが一方向、クライアントがサービスにメッセージを送信します。 サービスにクライアントに値を返す必要がある場合は、現時点までに説明されているすべて取り消されます。 サービスを作成する必要があります、`Message`パッケージ化して、任意の戻り値、およびディスパッチ、`Message`を介して、`Messenger`クライアントにします。 ただし、サービスは作成されません独自`Messenger`。 代わりに、クライアントをインスタンス化するとパッケージ依存、`Messenger`最初の要求の一部として。 サービスは`Send`このクライアントに用意されているを使用してメッセージを`Messenger`です。  

双方向通信のイベントのシーケンスでは、これです。

1. クライアントは、サービスにバインドします。 サービスとクライアントが接続するときに、`IServiceConnection`によって保持される、クライアントへの参照になります、`Messenger`送信に使用されるオブジェクト`Message`サービスにします。 混乱を避けるためには、これが参照されますとして、_サービス Messenger_です。
2. クライアントをインスタンス化、 `Handler` (と呼ばれる、_クライアント ハンドラー_) 独自の初期化に使用して`Messenger`(、_クライアント Messenger_)。 メッセンジャー サービスおよびクライアント Messenger は 2 つの異なる方向でトラフィックを処理する 2 つのさまざまなオブジェクトであることに注意してください。 サービス Messenger は、クライアント Messenger は、サービスからクライアントへのメッセージを処理中に、クライアントからサービスへのメッセージを処理します。
3. クライアントを作成、`Message`オブジェクト、およびセット、`ReplyTo`クライアント Messenger を持つプロパティです。 メッセージは、サービス Messenger を使用して、サービスに送信されます。
4. サービスは、クライアントからメッセージを受信し、要求された作業を実行します。
5. 使用する際にサービスがクライアントに応答を送信するため、`Message.Obtain`新しい`Message`オブジェクト。
6. クライアントにこのメッセージを送信するサービスからクライアント Messenger を抽出、`.ReplyTo`クライアントのプロパティ メッセージし、使用する`.Send`、`Message`クライアントに返送します。
7. 応答がクライアントによって受信されると、独自`Handler`を処理する、`Message`を調べることによって、`.What`プロパティ (必要に応じてに含まれるすべてのパラメーターを抽出して、 `Message`)。

このサンプル コードでは、クライアントがどのようにインスタンス化されでは、`Message`をパッケージ化と、`Messenger`応答のために使用する必要があります。

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

サービス、行う必要がありますをいくつかの変更独自`Handler`を抽出する、`Messenger`クライアントへの応答の送信に使用するとします。 このコード スニペット方法の例は、サービスの`Handler`を作成、`Message`し、それをクライアントに送信します。  

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

上記のコード サンプルで、`Messenger`クライアントによって作成されるインスタンスは*いない*サービスによって受信される、同じオブジェクトです。 これらは 2 つの異なる`Messenger`通信チャネルを表す 2 つの別個のプロセスで実行されているオブジェクト。

## <a name="securing-the-service-with-android-permissions"></a>Android アクセス許可を持つサービスを保護します。

グローバル プロセスで実行されるサービスは、Android デバイスで実行されているすべてのアプリケーションでアクセスできます。 場合によっては、このオープン性と可用性望ましくなく、承認されていないクライアントからのアクセスに対してサービスをセキュリティで保護する必要があります。 リモート サービスへのアクセスを制限する方法の 1 つでは、Android のアクセス許可を使用します。

アクセス許可を特定するのには`Permission`のプロパティ、`ServiceAttribute`装飾を`Service`サブクラスです。 これは、サービスにバインドするときに、クライアントに付与する必要があります権限に名前を付けます。 クライアントに適切なアクセス許可がないかどうかは、Android がスローされます、`Java.Lang.SecurityException`クライアントがサービスにバインドしようとしたときにします。

Android を提供する 4 つの異なるアクセス許可レベルがあります。

* **通常**&ndash;これは、既定のアクセス許可レベル。 これを使用して、Android によってこれを要求するクライアントに自動的に付与される危険性の低いアクセス許可を識別します。 ユーザーはこれらのアクセス許可を明示的に付与する必要はありませんが、アクセス許可は、アプリの設定で表示できます。
* **署名**&ndash;に与えられる自動的に Android によって同じ証明書で署名されているアプリケーションのアクセス許可の特殊なカテゴリです。 このアクセス許可は、コンポーネントまたは定数の承認についてユーザーを発行しないことがなく、アプリ間でデータを共有するアプリケーション開発者に簡単にできるように設計されています。
* **signatureOrSystem** &ndash;に非常に似ています、**署名**上アクセス許可について説明します。 だけでなく自動的に付与された、同じ証明書によって署名されているアプリに、このアクセス許可も付与されます署名されているアプリを Android のシステム イメージにインストールされているアプリの署名に使用された同じ証明書。 このアクセス許可は通常のみ開発者によって使用 Android ROM サード パーティ製アプリを操作するには、そのアプリケーションを許可します。 これは広く公開の一般的な分布は、アプリでは通常使用されません。
* **危険な**&ndash;危険なアクセス許可されるユーザーの問題が発生する可能性があります。 このため、**危険な**アクセス許可がユーザーによって明示的に承認する必要があります。

`signature`と`normal`によって権限が自動的に付与インストール時に、Android APK ホスティング、サービスをインストールすることが非常に重要**する前に**クライアントを含む APK です。 クライアントが最初にインストールされている場合、Android は、アクセス許可を付与されません。 この場合、クライアントをアンインストールする APK、APK、サービスをインストールおよび APK クライアントを再インストールする必要があります。

ストアには、Android のアクセス許可を持つサービスをセキュリティで保護する 2 つの一般的な方法があります。

1.  **署名のレベルのセキュリティを実装する**&ndash;署名のレベルのセキュリティ アクセス許可を自動的に付与されたにそれらのアプリケーション、サービスを保持する APK の署名に使用されたのと同じキーで署名されていることを意味します。 これは、開発者がサービスをセキュリティで保護しながら、独自のアプリケーションからアクセスできるようにする簡単な方法です。 設定して署名のレベルのアクセス許可が宣言されている、`Permission`のプロパティ、`ServiceAttribute`に`signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **カスタム アクセス許可を作成**&ndash;サービスのカスタム アクセス許可を作成するサービスの開発者のことができます。 これは、開発者が他の開発者からのアプリケーションとそのサービスを共有するときに最適です。 カスタム アクセス許可を実装する少し多くの労力を必要とし、以下で説明されます。

カスタムを作成する簡単な例`normal`権限は、次のセクションについて説明します。 Android アクセス許可の詳細については、Google のドキュメントを参照してください[ベスト プラクティスとセキュリティ](https://developer.android.com/training/articles/security-tips.html)です。 Android アクセス許可の詳細については、次を参照してください。、[アクセス許可のセクション](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)Android のアクセス許可の詳細については、アプリケーション マニフェスト用の Android ドキュメントのです。

> [!NOTE]
> 一般に、 [Google ではカスタム アクセス許可の使用を推奨されない](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions)ようにユーザーが混乱証明することがあります。

### <a name="creating-a-custom-permission"></a>カスタム アクセス許可を作成します。

カスタム アクセス許可を使用するのには、そのサービスによって、クライアントがそのアクセス許可を明示的に要求中に宣言されているがします。

APK、サービスにアクセス許可を作成する、`permission`に要素を追加、`manifest`内の要素**AndroidManifest.xml**です。 このアクセス許可が必要、 `name`、 `protectionLevel`、および`label`属性セットです。 `name`属性は、アクセス許可を一意に識別する文字列に設定する必要があります。 名前が表示されます、**アプリ情報**の表示、 **Android 設定**に示すように、次のセクション)。

`protectionLevel`属性は、上記に説明した 4 つの文字列値のいずれかに設定する必要があります。  `label`と`description`文字列リソースを参照する必要がありますを使用して、ユーザーにわかりやすい名前と説明を提供します。

このスニペットは、カスタムの宣言の例`permission`属性**AndroidManifest.xml**サービスを含む APK の。

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

次に、 **AndroidManifest.xml**クライアント APK 必要があります明示的に要求のこの新しいアクセス許可。 これは、追加することで、`users-permission`属性を**AndroidManifest.xml**:

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

### <a name="view-the-permissions-granted-to-an-app"></a>アプリに許可する権限を表示します。

アプリケーションに付与されているアクセス許可を表示するには、Android 設定アプリを開くし、選択**アプリ**です。 検索し、一覧で、アプリケーションを選択します。 **アプリ情報**画面をタップ**権限**アプリに付与されるすべてのアクセス許可を表示するビューが表示されます。

[![アプリケーションに付与されるアクセス許可を見つける方法を示す、Android デバイスのスクリーン ショット](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>まとめ

このガイドでは、Android のサービスをリモート プロセスで実行する方法についての高度な検討をでした。 リモート サービスの Android アプリのパフォーマンスの安定性と役に立ちます理由のいくつかの理由と共に、ローカルとリモート サービスとの違いが説明します。 リモート サービスを実装する方法と、クライアントがサービスと通信する方法を説明した後、ガイドがしました承認されたクライアントのみからサービスへのアクセスを制限する 1 つの方法を提供します。


## <a name="related-links"></a>関連リンク

- [ハンドラー](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [メッセージ](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [エクスポート属性](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [オーバー ロードを正常に解決するのには分離プロセスおよびカスタム アプリケーションのクラスとサービスが失敗します。](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [プロセスとスレッド](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android マニフェストのアクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [セキュリティのヒント](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
