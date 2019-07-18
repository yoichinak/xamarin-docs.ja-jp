---
title: リモート プロセスで実行されている Android サービス
description: 一般に、Android アプリケーションのすべてのコンポーネントは、同じプロセスで実行されます。 Android サービスに注目すべき例外では、独自のプロセスで実行するように構成およびその他の Android 開発者のものを含む、他のアプリケーションと共有できます。 このガイドは作成し、Xamarin を使用して Android のリモート サービスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: db312c4c102feb98791109af19185762bb25856e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013072"
---
# <a name="running-android-services-in-remote-processes"></a>リモート プロセスで実行されている Android サービス

_一般に、Android アプリケーションのすべてのコンポーネントは、同じプロセスで実行されます。Android サービスに注目すべき例外では、独自のプロセスで実行するように構成およびその他の Android 開発者のものを含む、他のアプリケーションと共有できます。このガイドは作成し、Xamarin を使用して Android のリモート サービスを使用する方法について説明します。_

## <a name="out-of-process-services-overview"></a>アウト プロセス サービスの概要

アプリケーションが起動すると、Android は、アプリケーションを実行するためのプロセスを作成します。 通常、すべてのコンポーネント、アプリケーション プロセスで実行されますこの 1 つ。 Android サービスに注目すべき例外では、独自のプロセスで実行するように構成およびその他の Android 開発者のものを含む、他のアプリケーションと共有できます。 この種のサービスと呼びます_リモート サービス_または_アウト プロセス サービス_します。 これらのサービスのコードはメインのアプリケーションとして同じ APK に含まれるただし、サービスは起動時に Android では、そのサービスだけの新しいプロセスを作成します。 これに対し、アプリケーションの残りの部分と同じプロセスで実行されているサービスとも呼ば、_ローカル サービス_します。

一般に、アプリケーションがリモート サービスを実装する必要はありません。 ローカル サービスが十分な (と望ましい) の多くの場合、アプリの要件。 アウト プロセスが、独自のメモリ領域 Android で管理する必要があります。 全体的なアプリケーションに多くのオーバーヘッドを導入これは、独自のプロセスでサービスを実行すると便利なできますいくつかのシナリオがあります。

1. **共有機能**&ndash;一部のアプリケーション開発者は複数のアプリと機能のすべてのアプリケーション間で共有される必要があります。 独自のプロセスの実行に使用するには、アプリケーションの保守が簡略化されます Android サービスでは、その機能をパッケージ化します。 独自のスタンドアロン APK でサービスをパッケージ化し、アプリケーションの残りの部分から個別にデプロイすることもできます。
2. **ユーザー エクスペリエンスを向上させる**&ndash;は、プロセス外のサービスが、アプリケーションのユーザー エクスペリエンスを向上させること、2 つの可能な方法があります。 最初の方法は、メモリ管理について説明します。 ガベージ コレクション (GC) サイクルが発生、Android が、GC が完了するまでに、プロセスのすべてのアクティビティは一時停止します。 ユーザーは、「途切れる」または"jank"として一時停止を感じる可能性があります。 サービスが実行されている独自のプロセスです。 つまり、アプリケーション プロセスではなく、サービス プロセスが一時停止するがします。 この一時停止は、アプリケーション プロセス (とそのため、ユーザー インターフェイス) が一時停止しないとしてユーザーにあまり顕著になります。

    次に、プロセスのメモリ要件が大きすぎると、Android では、デバイスのリソースを解放するには、そのプロセスが終了します。 場合は、サービスが大量のメモリ フット プリント、UI と同じプロセスで実行されて、し、Android が強制的にそれらのリソースを解放する場合、UI がシャット ダウンする、なり、ユーザーがアプリを起動します。 ただし、Android によって、独自のプロセスで実行されている、サービスがシャット ダウン UI プロセスでも影響を受けません。 UI できますバインド (および再起動)、サービスは、ユーザー、および再開の通常の動作に透過的です。

3. **アプリケーションのパフォーマンスを向上させる** &ndash; UI プロセスを終了またはシャット ダウン サービス プロセスに依存しない可能性があります。 プロセス外のサービスのスタートアップ時間のかかるタスクを移動することによってことができます (サービスのプロセスでは、UI を起動する時間の間は保持) のこと、UI の起動時間が改善可能性があります。

別のプロセスで実行されているサービスへのバインドでは、さまざまな方法では同じ[ローカル サービスへのバインド](~/android/app-fundamentals/services/creating-a-service/bound-services.md)します。 クライアントが呼び出す`BindService`バインド (および開始するために必要な場合)、サービス。 `Android.OS.IServiceConnection`クライアントとサービス間の接続を管理するオブジェクトが作成されます。 かどうか、クライアントは、サービスに正常にバインドし、Android を使用してオブジェクトを返しますが、`IServiceConnection`サービスでメソッドを呼び出す ことができます。 クライアントは、このオブジェクトを使用して、サービスと対話します。 確認するには、次に、サービスにバインドする手順を示します。

* **インテントを作成する**&ndash;明示的インテントは、サービスにバインドするために使用する必要があります。
* **インスタンス化の実装と、`IServiceConnection`オブジェクト** &ndash; 、`IServiceConnection`オブジェクトは、クライアントとサービス間の媒介として機能します。  クライアントとサーバー間の接続の監視を担当します。
* **呼び出す、`BindService`メソッド**&ndash;呼び出す`BindService`は、インテントと Android で、サービスの開始との通信を確立する処理は前の手順で作成したサービス接続をディスパッチクライアントとサービス。

余分な複雑さのためには導入プロセス境界を越える必要: 通信が一方向 (クライアント サーバーから) と、クライアントがサービス クラスのメソッドを直接呼び出すことはできません。 サービスは、クライアントと同じプロセスを実行中は、Android が提供することを思い出してください、`IBinder`双方向通信できるようになるオブジェクト。 独自のプロセスで実行されているサービスの場合とではありません。 クライアントのリモート サービスと通信、`Android.OS.Messenger`クラス。

Android を呼び出すクライアントは、リモート サービスにバインドする要求、ときに、`Service.OnBind`ライフ サイクル メソッドは、内部戻ります`IBinder`オブジェクトによってカプセル化された、 `Messenger`。 `Messenger`シン ラッパーが特別な`IBinder`Android SDK で提供されている実装します。 `Messenger` 2 つの異なるプロセス間の通信を行います。 開発者は、メッセージをシリアル化やプロセスの境界を越えてメッセージをマーシャ リングし、クライアントで逆シリアル化の詳細を気がします。 この作業は、`Messenger`オブジェクト。 この図は、クライアントは、プロセス外のサービスへのバインドを開始したときに関係しているクライアント側の Android コンポーネントを示しています。

![クライアントのサービスへのバインド ステップとコンポーネントを示す図](out-of-process-services-images/ipc-01.png "をクライアントのサービスへのバインド ステップとコンポーネントを示す図。")

`Service`リモート プロセス内のクラスをローカルのプロセスにバインドされているサービスを通過する同じライフ サイクル コールバック通過し、関連する Api の多くは同じです。 `Service.OnCreate` 初期化するために使用、`Handler`への挿入と`Messenger`オブジェクト。 同様に、`OnBind`はオーバーライドされた場合を返す代わりに、 `IBinder` 、サービスは、オブジェクトを返します、`Messenger`します。  この図は、クライアントをバインドする場合に、サービスでの動作を示しています。

![リモート クライアントでにバインドされているときに、ステップとコンポーネント サービスを示す図が通過](out-of-process-services-images/ipc-02.png "をサービスにステップとコンポーネントを示す図を通過してリモート クライアントにバインドされている場合。")

ときに、`Message`受信のインスタンスで、サービスによってによって処理される`Android.OS.Handler`します。 サービスの実装は、独自`Handler`サブクラスをオーバーライドする必要があります`HandleMessage`メソッド。 このメソッドは、`Messenger`を受け取ると、`Message`をパラメーターとして。 `Handler`が検査され、`Message`メタ データと、その情報を使用して、サービスのメソッドを呼び出します。

一方向の通信は、クライアントを作成するときに発生します。、`Message`オブジェクトとを使用して、サービスにディスパッチされます、`Messenger.Send`メソッド。 `Messenger.Send` シリアル化、`Message`と手を Android で、プロセス境界を越えてメッセージをルーティングして、サービスにデータをシリアル化します。  `Messenger`によってホストされているサービスで作成、`Message`受信データからのオブジェクト。 これは、`Message`メッセージが送信された 1 つずつは、キューに配置されますが、`Handler`します。 `Handler`に含まれているメタデータを検査、`Message`で適切なメソッドを呼び出すと、`Service`します。 次の図は、アクションでこれらのさまざまな概念を示しています。

![プロセス間でメッセージが渡される方法を示す図](out-of-process-services-images/ipc-03.png "プロセス間でメッセージが渡される方法を示す図。")

このガイドは、プロセス外のサービスの実装の詳細について説明します。 独自のプロセスで実行することを想定してサービスを実装する方法とクライアントを使用してそのサービスと通信する方法を説明します、`Messenger`フレームワーク。 双方向の通信も簡単に説明します。 サービスとクライアントにメッセージを送信するサービスにメッセージを送信するクライアント。 サービスは、異なるアプリケーション間で共有できる、ために、このガイドも Android のアクセス許可を使用して、サービスへのクライアント アクセスを制限するための 1 つの方法を説明します。

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950 - 分離プロセスやアプリケーションのカスタム クラスとサービスが適切にオーバー ロードを解決するのには失敗](https://github.com/xamarin/xamarin-android/issues/1950)Xamarin.Android サービスが正しく起動はしないレポートと、`IsolatedProcess`に設定されている`true`します。 このガイドは、参照用に提供されます。 Xamarin.Android アプリケーションは、Java で記述されたプロセス外のサービスと通信できる必要があります。

## <a name="requirements"></a>必要条件

このガイドは、サービスの作成に関する知識を想定しています。

古い対象とするアプリを使用して暗黙的なインテントを使用することはできますが、Android Api、このガイドは、明示的インテントの使用に集中は。 Android 5.0 (API レベル 21) を対象とするアプリをサービスにバインドする明示的なインテントを使用する必要があります以降またはこれは、このガイドで説明する手法です.

## <a name="create-a-service-that-runs-in-a-separate-process"></a>別のプロセスで実行されているサービスを作成します。

前述のように、サービスは、独自のプロセスで実行されているという事実は、いくつか異なる Api が含まれることを意味します。 簡単な概要として次をバインドし、リモート サービスを使用する手順に示します。  

* **作成、`Service`サブクラス**&ndash;サブクラス、`Service`を入力し、バインドされているサービスのライフ サイクル メソッドを実装します。 サービスは、独自のプロセスで実行する Android に通知するメタデータを設定する必要もできます。
* **実装を`Handler`**  &ndash; 、`Handler`クライアント要求を分析して、クライアントから渡されたパラメーターを抽出し、サービスに適切なメソッドを呼び出すを担当します。
* **インスタンスを作成、 `Messenger`**  &ndash;上、それぞれの説明に従って`Service`のインスタンスを保持する必要があります、`Messenger`クラスへのクライアント要求をルーティングする、`Handler`前の手順で作成されました。

サービスは、独自のプロセスで実行するためのものですが、基本的には、まだバインドされているサービスです。 サービス クラスには、ベースは拡張`Service`クラスし、は、 `ServiceAttribute` Android は、Android マニフェストでバンドルする必要があるメタデータを格納しています。 次のプロパティで始まる、`ServiceAttribute`をプロセス外のサービスにとって重要。

1. `Exported` &ndash; このプロパティに設定する必要があります`true`他のアプリケーション サービスとの対話を可能にします。 このプロパティの既定値は `false` です。
2. `Process` &ndash; このプロパティを設定する必要があります。 サービスで実行されるプロセスの名前を指定に使用されます。
3. `IsolatedProcess` &ndash; このプロパティは、追加のセキュリティ、システムの残りの部分と対話する最小限のアクセス許可を持つ分離のサンド ボックスで、サービスを実行する Android の通知を有効になります。 参照してください[Bugzilla 51940 - サービスの独立したプロセスとアプリケーションのカスタム クラスがオーバー ロードを正しく解決失敗](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)します。
4. `Permission` &ndash; アクセス許可を要求する必要があります (および付与) クライアントを指定して、サービスへのクライアント アクセスを制御することになります。

独自のプロセスでは、サービスの実行に、`Process`プロパティを`ServiceAttribute`サービスの名前に設定する必要があります。 外部のアプリケーションと対話する、`Exported`にプロパティを設定する必要があります`true`します。 場合`Exported`は`false`、同じ APK (つまり、同じアプリケーション) 内のクライアントのみと同じプロセスで実行中は、サービスと対話することになります。

プロセスで、サービスは実行の種類の値によって異なります、`Process`プロパティ。 Android では、3 つの異なる種類のプロセスを識別します。

-   **プライベート プロセス**&ndash;プライベート プロセスは、1 つのみを開始したアプリケーションに提供されます。 プライベート プロセスを識別するためにその名前で始まらなければなりません、 **:** (セミコロン)。 上記のコード スニペットおよびスクリーン ショットに示すように、サービスは、プライベート プロセスです。 次のコード スニペットの例に示します、 `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **グローバルなプロセス**&ndash;グローバルなプロセスで実行されるサービスにデバイスで実行されているすべてのアプリケーションにアクセスします。 グローバルなプロセスは、小文字で始まる完全修飾クラス名である必要があります。
    (サービスをセキュリティで保護することを講じて、しない限り、他のアプリケーションがバインドし、対話します。 不正アクセスからサービスを保護する、について説明しますこのガイドで後述します。)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **プロセスの分離**&ndash;を分離プロセスは、独自のない特殊なアクセス許可と、システムの残りの部分から分離された独自サンド ボックス内で実行されるプロセス。 分離プロセスでサービスを実行する、`IsolatedProcess`のプロパティ、`ServiceAttribute`に設定されている`true`コード スニペットで示すように。
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> 参照してください[Bugzilla 51940 - サービスの独立したプロセスとアプリケーションのカスタム クラスがオーバー ロードを正しく解決失敗](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

独立したサービスは、アプリケーションと信頼されていないコードに対してデバイスをセキュリティで保護する簡単な方法です。 たとえば、アプリをダウンロードして web サイトからスクリプトを実行する可能性があります。 ここでは、分離プロセスで実行するには、信頼できないコードが Android デバイスを損なうことに対するセキュリティの強化が提供します。

> [!IMPORTANT]
> サービスをエクスポートすると、サービスの名前は変更しないでください。 サービスの名前を変更すると、サービスを使用している他のアプリケーションが中断する可能性があります。

効果を確認する、`Process`プロパティは、次のスクリーン ショットには、独自のプライベート プロセスで実行されているサービスが表示されます。

![プライベート プロセスで実行されているサービスを示すスクリーン ショット](out-of-process-services-images/ipc-04.png "をプライベート プロセスで実行されているサービスを示すスクリーン ショット。")

この次のスクリーン ショットは`Process="com.xamarin.xample.messengerservice.timestampservice_process"`とサービスのグローバルなプロセスで実行します。

![グローバルなプロセスで実行されているサービスのスクリーン ショット](out-of-process-services-images/ipc-05.png "グローバルなプロセスで実行されているサービスのスクリーン ショット。")

1 回、`ServiceAttribute`が設定されている、実装する必要のあるサービス、`Handler`します。

### <a name="implementing-a-handler"></a>ハンドラーを実装します。

クライアント要求を処理するには、サービスを実装する必要があります、`Handler`をオーバーライドし、 `HandleMessage` methodThis はメソッドは、`Message`インスタンスに、クライアントからメソッドの呼び出しをカプセル化し、タスクやアクションには、その呼び出しを変換します。サービスが実行されます。 `Message`オブジェクトと呼ばれるプロパティを公開する`What`は整数値の意味は、クライアントとサービス間で共有し、サービスがクライアントを実行するにはいくつかのタスクに関連しています。

サンプル アプリケーションから次のコード スニペットは、1 つの例を示しています。`HandleMessage`します。 この例では、サービスのクライアントが要求できる 2 つのアクションがあります。

* 最初の操作が、_こんにちは, World_メッセージ、クライアントが単純なメッセージをサービスに送信します。
* 2 番目のアクションは、サービスでメソッドを呼び出すし、文字列を取得、文字列をどのような時間が実行されているサービス開始され、どのくらいの時間を返すメッセージは、ここで。

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

サービスのパラメーターをパッケージすることも、`Message`します。 これについては、このガイドの後半で説明します。 考慮すべき次のトピックを作成、`Messenger`受信を処理するオブジェクト`Message`秒。

### <a name="instantiating-the-messenger"></a>Messenger をインスタンス化します。

前述した、逆シリアル化、`Message`オブジェクトと呼び出し`Handler.HandleMessage`の責任において、`Messenger`オブジェクト。 `Messenger`クラスも用意されています。、`IBinder`サービスにメッセージを送信するクライアントが使用するオブジェクト。  

サービスの開始時にインスタンス化するため、`Messenger`を挿入し、`Handler`します。 この初期化を実行する適切な場所は、`OnCreate`サービスのメソッド。 このコード スニペットはそれ自体を初期化するサービスの 1 つの例`Handler`と`Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

最後の手順は、この時点で、`Service`をオーバーライドする`OnBind`します。

### <a name="implementing-serviceonbind"></a>Service.OnBind を実装します。

または、独自のプロセスで実行するかどうか、バインドされたすべてのサービスを実装する必要があります、`OnBind`メソッド。 このメソッドの戻り値とは、クライアントがサービスとの対話に使用できるいくつかのオブジェクトです。 そのオブジェクトは、どのような正確には、サービスがローカル サービスまたはリモート サービスであるかどうかによって異なります。 ローカル サービスは、カスタムを返す`IBinder`実装では、リモート サービスが返す、`IBinder`をカプセル化されていますが、`Messenger`前のセクションで作成されました。

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

これら 3 つのステップが完了すると、リモート サービスできるものとなります。

## <a name="consuming-the-service"></a>サービスを使用

すべてのクライアントは、バインドし、リモート サービスを利用できるいくつかのコードを実装する必要があります。 概念的には、クライアントの観点からが、ローカル サービスまたはリモート サービスへのバインドのごくわずかな違いがあります。 クライアントが呼び出す、`BindService`サービスを識別するために明示的なインテントを渡して、メソッド、`IServiceConnection`クライアントとサービス間の接続の管理を支援します。

このコード スニペットを作成する方法の例に示します、**明示的インテント**リモート サービスにバインドするためです。 目的は、サービスおよびサービスの名前を含むパッケージを識別する必要があります。 この情報を設定する方法の 1 つが使用するには、`Android.Content.ComponentName`オブジェクトと、目的に提供します。 このコード スニペットでは、1 つの例を示します。  

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

サービスがバインドされている場合、`IServiceConnection.OnServiceConnected`メソッドが呼び出され、提供、`IBinder`クライアントにします。 ただし、クライアントは直接使用しません、`IBinder`します。 代わりに、インスタンス化するため、`Messenger`オブジェクトから`IBinder`します。 これは、`Messenger`クライアントがリモート サービスとの対話に使用します。

非常に基本的な例を次に`IServiceConnection`実装をクライアントに接続して、サービスからの切断をどのように処理する方法を示します。 注意して、`OnServiceConnected`メソッドは受信と`IBinder`、クライアントを作成し、`Messenger`から`IBinder`:

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

サービスの接続と、インテントを作成後は、クライアントが呼び出すためのことは`BindService`バインド プロセスを開始します。

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

クライアントがサービスにバインドが正常にした後、`Messenger`は、使用可能な可能性があります、クライアントから送信`Messages`サービスに。

## <a name="sending-messages-to-the-service"></a>サービスにメッセージを送信します。

クライアントが接続されているし、は、`Messenger`オブジェクト、ディスパッチによってサービスと通信することは`Message`オブジェクトを使用して、 `Messenger`。 この通信が一方向、クライアントがメッセージを送信しますが、サービスからクライアントへのメッセージの戻り値はありません。 この点で、`Message`ファイア アンド フォーゲット メカニズムです。

作成することをお勧め、`Message`オブジェクトは、使用する、 [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods)ファクトリ メソッド。 このメソッドはプル、 `Message` Android によって保持されているグローバル プールからのオブジェクト。 `Message.Obtain` メソッドがいくつかオーバー ロードも、`Message`値と、サービスに必要なパラメーターを使用して初期化するオブジェクト。  1 回、`Message`がインスタンス化すると、そのサービスにディスパッチを呼び出して`Messenger.Send`します。 このスニペットは、1 つの例の作成とディスパッチを`Message`サービス プロセス。

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

いくつかの異なる形式としては、`Message.Obtain`メソッド。 前の例では、 [ `Message.Obtain(Handler h, Int32 what)`](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/)します。 これは、プロセス外のサービスへの非同期要求なのでサービスからの応答ことはありませんので、`Handler`に設定されている`null`します。 2 番目のパラメーターでは、`Int32 what`に格納されます、`.What`のプロパティ、`Message`オブジェクト。 `.What`プロパティは、サービスでメソッドを呼び出すサービス プロセス内のコードによって使用されます。

`Message`クラスでは、受信者に使用する必要があります、2 つのプロパティも公開します:`Arg1`と`Arg2`します。 これら 2 つのプロパティは、可能性のあるいくつかの特別な合意がクライアントとサービスの間の意味を持つ値の整数値です。 たとえば、`Arg1`顧客 ID を保持する可能性がありますと`Arg2`その顧客の注文書番号を保持する可能性があります。 [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) 2 つのプロパティを設定するために使用するときに、`Message`が作成されます。 これら 2 つの値を設定する別の方法が設定するのには、`.Arg`と`.Arg2`プロパティで直接、`Message`が作成された後のオブジェクトします。

### <a name="passing-additional-values-to-the-service"></a>サービスに追加の値を渡す

使用して、サービスにさらに複雑なデータを渡すことは、`Bundle`します。 余分な値を配置するこの例を`Bundle`と共に送信されると、`Message`を設定して、 [ `.Data`プロパティ](https://developer.xamarin.com/api/property/Android.OS.Message.Data/)プロパティを送信する前にします。

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> 一般に、 `Message` 1 MB を超えるペイロードではありません。 Android のバージョンに基づいて、サイズの上限が異なる場合があり、独自の変更、仕入先の Android オープン ソース プロジェクト (AOSP) デバイスにバンドルされている実装になった可能性があります。

## <a name="returning-values-from-the-service"></a>サービスから値を返す

ここまで説明したメッセージングのアーキテクチャが一方向には、クライアントがサービスにメッセージを送信します。 サービスをクライアントに値を返す必要がある場合は、ここまで説明したことはすべて取り消されます。 サービスを作成する必要があります、 `Message`、パッケージ化された任意の戻り値、およびディスパッチ、`Message`を使用して、`Messenger`クライアントにします。 ただし、サービスは作成されません独自`Messenger`。 代わりに、クライアントをインスタンス化すると、パッケージに依存、`Messenger`最初の要求の一部として。 サービスは`Send`クライアント提供このを使用してメッセージ`Messenger`します。  

双方向通信のイベントのシーケンスは、

1. クライアントは、サービスにバインドします。 サービスとクライアントが接続するときに、`IServiceConnection`によって保持されているクライアントへの参照になります、`Messenger`送信に使用されるオブジェクト`Message`サービスにします。 混乱を避けるため、これがあると呼ばれます、_サービス Messenger_します。
2. クライアントをインスタンス化、 `Handler` (と呼ばれる、_クライアント ハンドラー_) を使用して、独自の初期化と`Messenger`(、_クライアント Messenger_)。 Messenger サービスとクライアントのメッセンジャーは 2 つの方向のトラフィックを処理する 2 つのさまざまなオブジェクトであることに注意してください。 サービス Messenger は、クライアントのメッセンジャー サービスからクライアントへのメッセージを処理するときに、クライアントからサービスにメッセージを処理します。
3. クライアントは、作成、`Message`オブジェクト、およびセット、`ReplyTo`クライアント Messenger でのプロパティ。 メッセージは、サービス、Messenger を使用して、サービスに送信し、されます。
4. サービスは、クライアントからメッセージを受信し、要求された作業を実行します。
5. クライアントへの応答を送信するサービスの時間がある場合に使用されます`Message.Obtain`新たに作成する`Message`オブジェクト。
6. サービス クライアントにこのメッセージを送信するからクライアント Messenger が抽出されます、`.ReplyTo`プロパティは、クライアントのメッセージを使用して`.Send`、`Message`クライアントにします。
7. これが、独自の応答がクライアントによって受信されると、`Handler`を処理する、`Message`を調べることによって、`.What`プロパティ (必要に応じてに含まれるすべてのパラメーターを抽出し、 `Message`)。

このサンプル コードでは、クライアントがインスタンスを作成する方法を示しています、`Message`およびパッケージ化、`Messenger`応答のために使用する必要があります。

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

サービス、行う必要がありますを変更すると、独自`Handler`を抽出する、`Messenger`クライアントへの返信を送信する際に使用します。 このコード スニペット方法の例は、サービスの`Handler`を作成、`Message`し、それをクライアントに送信します。  

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

上記のコード サンプルで、`Messenger`クライアントによって作成されるインスタンスが*いない*が、サービスによって受信するオブジェクトと同じです。 これらは、2 つの異なる`Messenger`通信チャネルを表す 2 つの個別のプロセスで実行されているオブジェクト。

## <a name="securing-the-service-with-android-permissions"></a>Android のアクセス許可を持つサービスを保護します。

グローバルなプロセスで実行されているサービスは、Android デバイスで実行されているすべてのアプリケーションでアクセスできます。 場合によっては、この開放性と可用性が望ましくないと、未承認のクライアントからのアクセスに対してサービスをセキュリティで保護する必要があります。 リモート サービスへのアクセスを制限する方法の 1 つでは、Android のアクセス許可を使用します。

アクセス許可を識別できます、`Permission`のプロパティ、`ServiceAttribute`装飾、`Service`サブクラスです。 これは、サービスにバインドする場合、クライアントを許可する必要があります権限に名前を付けます。 クライアントには、適切なアクセス許可がないかどうかは、Android がスローされます、`Java.Lang.SecurityException`クライアントがサービスにバインドしようとしたとき。

Android を提供する 4 つの異なるアクセス許可レベルがあります。

* **通常**&ndash;これは、既定のアクセス許可レベル。 Android でこれを要求するクライアントに自動的に付与される危険性の低いアクセス許可を識別するために使用されます。 ユーザーがこれらのアクセス許可を明示的に付与する必要はありませんが、アクセス許可をアプリ設定で表示できます。
* **署名**&ndash;に同じ証明書で署名されたアプリケーションを Android で自動的に付与 アクセス許可の特殊なカテゴリになります。 このアクセス許可は、コンポーネントまたは定数の承認用のユーザーを煩わせることがなく、アプリ間でデータを共有するアプリケーション開発者を簡単にできるように設計されています。
* **signatureOrSystem** &ndash;に非常に似ています、**署名**上記で説明したアクセス許可。 だけでなく、同じ証明書によって署名されているアプリに付与されて自動的に、このアクセス許可も与えられます署名されているアプリを Android システムのイメージがアプリに署名するために使用された同じ証明書がインストールされています。 このアクセス許可は通常のみによって使用 Android ROM 開発者サード パーティ製アプリを操作するためにアプリケーションを許可します。 これは広く公開の一般的な分布は、アプリでは通常使用されません。
* **危険な**&ndash;危険なアクセス許可がユーザーの問題を引き起こす可能性のあるものです。 このため、**危険な**アクセス許可をユーザーが明示的に承認する必要があります。

`signature`と`normal`によって権限が自動的に付与時にインストールされている Android では、サービスをホストする APK をインストールすることが非常に重要**する前に**クライアントを含む APK です。 クライアントが最初にインストールされている場合、Android は、権限が付与されます。 この場合は、APK のクライアントをアンインストール、サービス、APK をインストールし、APK のクライアントを再インストールする必要があります。

Android のアクセス許可を持つサービスをセキュリティで保護する 2 つの一般的な方法はあります。

1.  **署名のレベルのセキュリティを実装する**&ndash;署名のレベルのセキュリティ アクセス許可が自動的に許可されているサービスを保持する APK の署名に使用されたのと同じキーで署名されているこれらのアプリケーションを意味します。 これは、そのサービスをセキュリティで保護しながら独自のアプリケーションからアクセスできるように開発者向けの簡単な方法です。 設定して署名のレベルのアクセス許可が宣言されている、`Permission`のプロパティ、`ServiceAttribute`に`signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **カスタム アクセス許可を作成**&ndash;サービスの開発者サービスのカスタム アクセス許可を作成する可能性があります。 これは、開発者を他の開発者からのアプリケーションとサービスを共有しようとしたときに最適です。 カスタム アクセス許可を実装するために少しの労力を必要とし、以下で説明されます。

カスタムの作成の簡略化された例`normal`アクセス許可は、次のセクションで説明します。 Android のアクセス許可の詳細については、Google のドキュメントを参照してください[ベスト プラクティスとセキュリティ](https://developer.android.com/training/articles/security-tips.html)します。 Android のアクセス許可の詳細については、次を参照してください。、[アクセス許可のセクション](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)の Android のアクセス許可の詳細については、アプリケーション マニフェスト用の Android ドキュメント。

> [!NOTE]
> 一般に、 [Google がカスタム アクセス許可の使用を抑制](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions)ように、ユーザーが混乱になる可能性があります。

### <a name="creating-a-custom-permission"></a>カスタム アクセス許可の作成

カスタム アクセス許可を使用するには、クライアントは、そのアクセス許可を明示的に要求中に、サービスによって宣言されています。

APK をサービスにアクセス許可を作成、`permission`要素に追加されます、`manifest`要素**AndroidManifest.xml**します。 このアクセス許可が必要、 `name`、 `protectionLevel`、および`label`属性セットです。 `name`属性は、アクセス許可を一意に識別する文字列に設定する必要があります。 名前が表示されます、 **App Info**の表示、 **Android 設定**(ように、次のセクションを参照)。

`protectionLevel`属性は、上記に説明した 4 つの文字列値のいずれかに設定する必要があります。  `label`と`description`文字列リソースを参照する必要があり、ユーザーにわかりやすい名前と説明を提供するために使用します。

このスニペットは、カスタムの宣言の例`permission`属性**AndroidManifest.xml**サービスが含まれている APK の。

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

次に、 **AndroidManifest.xml**クライアントの APK する必要があります明示的にこの新しいアクセス許可を要求します。 これは、追加することで、`users-permission`属性を**AndroidManifest.xml**:

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

### <a name="view-the-permissions-granted-to-an-app"></a>アプリに付与されるアクセス許可を表示します。

アプリケーションに付与されているアクセス許可を表示するアプリを開き、Android の設定、および選択**アプリ**します。 検索し、一覧で、アプリケーションを選択します。 **アプリ情報**画面をタップして**アクセス許可**アプリに付与されるすべてのアクセス許可を表示するビューが表示されます。

[![アプリケーションに付与するアクセス許可を検索する方法を示す、Android デバイスのスクリーン ショット](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>まとめ

このガイドは、リモート プロセスで Android のサービスを実行する方法については高度なです。 リモート サービスを Android アプリのパフォーマンスの安定性と役に立ちます理由のいくつかの理由と共に、ローカルとリモート サービスとの違いが説明します。 リモート サービスを実装する方法と、クライアントがサービスと通信する方法を説明するには、後に許可されたクライアントのみからサービスへのアクセスを制限する方法の 1 つを提供するガイドになりました。


## <a name="related-links"></a>関連リンク

- [ハンドラー](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [メッセージ](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [エクスポートされた属性](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [オーバー ロードを解決するには、適切に分離プロセスやアプリケーションのカスタム クラスとサービスが失敗します。](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [プロセスとスレッド](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android マニフェストのアクセス許可](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [セキュリティに関するヒント](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
