---
title: Xamarin.Android でのサービスをバインドします。
description: バインドされているサービス (Android のアクティビティ) などのクライアントが対話するクライアントとサーバー インターフェイスを提供する Android のサービスです。 このガイドでは、バインドされているサービスと Xamarin.Android アプリケーションで使用する方法の作成に関連する主要なコンポーネントについて説明します。
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/04/2018
ms.openlocfilehash: f4fe1bd753260f05dedb452655572d290c0781d0
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33850575"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin.Android でのサービスをバインドします。

_バインドされているサービス (Android のアクティビティ) などのクライアントが対話するクライアントとサーバー インターフェイスを提供する Android のサービスです。このガイドでは、バインドされているサービスと Xamarin.Android アプリケーションで使用する方法の作成に関連する主要なコンポーネントについて説明します。_

## <a name="bound-services-overview"></a>バインドされているサービスの概要

サービスと直接通信するクライアント用のクライアントとサーバー インターフェイスを提供するサービスと呼びます_services をバインド_です。  複数のクライアントが同時に、サービスの 1 つのインスタンスに接続されていることができます。 バインドされているサービスとクライアントは、互いから分離されます。 代わりに、Android では、一連の 2 つの間の接続の状態を管理する中間のオブジェクトを提供します。 この状態を実装するオブジェクトによって保持される、 [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)インターフェイスです。  このオブジェクトが、クライアントによって作成され、パラメーターとして渡される、 [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/)メソッドです。 `BindService`は、任意[ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context)オブジェクト (アクティビティ) などです。 サービスを開始し、それをクライアントに連結する Android オペレーティング システムに要求することをお勧めします。 クライアントに 3 つの方法を使用してサービスにバインドがある、`BindService`メソッド。

* **サービス バインダー** &ndash;サービス バインダーを実装するクラス、 [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder)インターフェイスです。 ほとんどのアプリケーションが直接このインターフェイスを実装しませんが、それらが拡張する代わりに、 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder)クラスです。 これは最も一般的な方法し、は同じプロセスで、サービスとクライアントが存在する場合に適してします。
* **Messenger を使用して**&ndash;この手法は個別のプロセスでサービスが存在する場合に適しています。 経由でサービスとクライアントとサービス要求が代わりに、マーシャ リング、 [ `Android.OS.Messenger`](https://developer.xamarin.com/api/type/Android.OS.Messenger)です。 [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler)を処理するサービスで作成された、`Messenger`要求します。 これについては、別のガイドで説明します。
* **AIDL を使用して**&ndash;これで Android のほとんどの開発者の中心となるテロを襲うされ、このガイドで扱われていません。

2 つの間の通信は、クライアントがサービスにバインドされているを通じて行われます`Android.OS.IBinder`オブジェクト。  このオブジェクトは、サービスと対話するクライアントを許可するインターフェイスを担当します。 Android SDK は、各 Xamarin.Android アプリケーションを最初からこのインターフェイスを実装する必要はありません、 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder)クラスは、オブジェクト間のマーシャ リングに必要なコードのほとんどを行いますクライアントとサービス。

クライアントでは、サービス処理が終わったらする必要がありますバインド解除から呼び出すことによって、`UnbindService`メソッドです。 最後のクライアントは、サービスからバインド解除されましたが、いったん Android が停止し、バインドされているサービスを破棄します。

この図は、アクティビティ、サービスの接続、バインダー、およびサービスはすべてが相互に関連する方法を示しています。

![サービス コンポーネントが相互に関連付ける方法を示す図](bound-services-images/bound-services-02.png "サービス コンポーネントが相互に関連付ける方法を示す図。")

このガイドには、拡張する方法を検討、`Service`バインドされているサービスを実装するクラス。 実装にも適用されます`IServiceConnection`と拡張`Binder`クライアント サービスと通信するために使用できるようにします。 サンプル アプリに伴うこのガイドと呼ばれる 1 つの Xamarin.Android プロジェクトとソリューションを含む**[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** です。 これは、非常に基本的なアプリケーション、サービスを実装する方法と、アクティビティをバインドする方法について説明しています。 バインドされているサービスが 1 つのメソッドと非常に単純な API`GetFormattedTimestamp`サービスが開始されたときに、ユーザーに通知する文字列と時間が実行されたが返されます。 アプリでは、ユーザーが手動でバインドを解除し、サービスにバインドすることもできます。

[![Android フォンで実行されているアプリケーションのスクリーン ショット](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>バインドされているサービスの実装と

Android アプリケーションでバインドされているサービスを使用するために実装する必要が 3 つのコンポーネントがあります。

1. **拡張、`Service`クラスし、ライフ サイクルのコールバック メソッドを実装する**&ndash;このクラスは、サービスの要求される作業を実行するコードに含まれます。 これについてで詳しく説明します。
2. **実装するクラスを作成`IServiceConnection`**  &ndash;コールバック メソッドが呼び出されるこのインターフェイスを提供して、サービスへの接続が変更されたときに、クライアントに通知する Android、つまり、クライアントが接続または切断されたに、サービス。 サービスの接続には、クライアントがサービスと直接対話するために使用できるオブジェクトへの参照も提供されます。 この参照と呼ばれる、_バインダー_です。
3. **実装するクラスを作成`IBinder`**  &ndash; A_バインダー_実装には、クライアントがサービスと通信するために使用する API が用意されています。 バインダーを提供するか、バインドされているサービスへの参照を直接呼び出されるメソッドを許可するか、バインダーは、クライアントをカプセル化し、アプリケーションからバインドされているサービスを非表示にする API を提供できます。 `IBinder`リモート プロシージャ呼び出しのために必要なコードを提供する必要があります。 または必要はありません (推奨) を実装する、`IBinder`インターフェイスを直接です。 代わりにアプリケーションを拡張する必要があります、`Binder`で必要な基本機能のほとんどを提供する型、`IBinder`です。
4. **開始とサービスへのバインド**&ndash;サービスの接続、バインダー、およびサービスを作成した後、Android アプリケーションは、サービスを開始して、それへのバインドにします。

これらの各手順については、さらに詳しくは、次のセクションで説明します。

### <a name="extend-the-service-class"></a>拡張、`Service`クラス

Xamarin.Android を使用してサービスを作成する必要があるサブクラスに`Service`を装飾してを使用してクラス、 [ `ServiceAttribute`](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)です。 属性が適切で、アプリのサービスに登録する Xamarin.Android ビルド ツールによって使用される**AndroidManifest.xml**ファイルのアクティビティと同様、バインドされているサービスが、独自のライフ サイクル、およびコールバック メソッドに関連付けられている、ライフ サイクルの重要なイベントです。 サービスを実装する一般的なコールバック メソッドのいくつかの例を次に示します。

* `OnCreate` &ndash; このメソッドが、サービスをインスタンス化するには、Android によって呼び出されます。 すべての変数またはその有効期間中に、サービスで必要となるオブジェクトを初期化するために使用されます。 このメソッドは省略可能です。
* `OnBind` &ndash; このメソッドは、バインドのすべてのサービスで実装する必要があります。 最初のクライアントがサービスに接続しようとするときに呼び出されます。 インスタンスが返されます`IBinder`できるように、クライアントが、サービスとやり取りすることがあります。 サービスが実行されている限り、`IBinder`オブジェクトは、サービスにバインドするには、将来のクライアント要求を満たすために使用されます。
* `OnUnbind` &ndash; このメソッドは、バインドのすべてのクライアントがバインドされていないときに呼び出されます。 返すことによって`true`このメソッドからサービスを呼び出すことが後で`OnRebind`に渡される目的で`OnUnbind`新しいクライアントにバインドするときにします。 これを実行するサービスは、そのバインドが解除された後に実行が継続する場合。 なら、最近にバインドされていないサービスも開始されるサービスでは、これが起こると`StopService`または`StopSelf`呼び出されていません。 このようなシナリオでは、`OnRebind`を取得することを目的に使用します。 既定値を返します`false`、何もしません。 任意。
* `OnDestroy` &ndash; このメソッドは、Android がサービスを破棄するときに呼び出されます。 この方法では、リソースを解放するなど、必要なクリーンアップを実行してください。 任意。

バインドされているサービスのキーのライフ サイクル イベントは、このダイアグラムに表示されます。

![ライフ サイクル メソッドが呼び出される順序を示すダイアグラム](bound-services-images/bound-services-01.png "ライフ サイクル メソッドが呼び出される順序を表示する図。")

このガイドに付属している関連アプリケーションから、次のコード スニペットは、Xamarin.Android にバインドされているサービスを実装する方法を示しています。

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

このサンプルで、`OnCreate`メソッドは、クライアントによって要求されると、タイムスタンプを書式設定を取得してロジックを保持するオブジェクトを初期化します。 Android を呼び出すが、最初のクライアントは、サービスにバインドするとき、`OnBind`メソッドです。 このサービスがインスタンス化され、`TimestampServiceBinder`実行中のサービスのこのインスタンスにアクセスするようにクライアントを許可するオブジェクト。 `TimestampServiceBinder`クラスは、次のセクションで説明します。

### <a name="implementing-ibinder"></a>IBinder を実装します。

前述のように、`IBinder`オブジェクトは、クライアントとサービス間の通信チャネルを提供します。 Android アプリケーションを実装しないでください、 `IBinder` 、インターフェイス、 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/)拡張する必要があります。 `Binder`クラスはマーシャ リングのために必要なは、必要なインフラストラクチャの大半に (別のプロセスで実行される可能性があります) をサービスからクライアントへのバインダー オブジェクトを提供します。 ほとんどの場合、`Binder`サブクラスのみ数行のコードは、およびサービスへの参照をラップします。 この例では`TimestampBinder`を公開するプロパティを持つ、`TimestampService`クライアント。

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

これは、`Binder`により、サービスのパブリック メソッドを呼び出す例を示します。

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

1 回`Binder`されましたを拡張する必要がある実装`IServiceConnection`すべて同時に接続します。

### <a name="creating-the-service-connection"></a>サービス接続を作成します。

`IServiceConnection`が表示されます | 導入 | 公開 | 接続、`Binder`クライアント オブジェクトです。 実装するだけでなく、`IServiceConnection`インターフェイス、クラスを拡張する必要があります`Java.Lang.Object`です。 サービスの接続は、クライアントがアクセスできるいくつかの方法も提供する必要があります、 `Binder` (とそのため、バインドされているサービスと通信)。

このコードに付属するサンプル プロジェクトを実装する方法の 1 つ`IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

Android を呼び出し、バインド プロセスの一部として、`OnServiceConnected`メソッドを提供する、`name`バインドされているサービスの`binder`サービス自体への参照を保持します。 この例では、サービスの接続をバインダーのブール フラグへの参照を保持しているか、サービスにクライアントが接続されている場合、2 つのプロパティがします。

`OnServiceDisconnected`メソッドは、クライアントとサービス間の接続が予期せず失われたか、破損した場合にのみ呼び出されます。 このメソッドは、サービスの中断に対応する機会にクライアントを使用します。  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>開始と明示的な目的のサービスへのバインド

(アクティビティ) などのクライアントにバインドされているサービスを使用して、実装するオブジェクトをインスタンス化する必要があります`Android.Content.IServiceConnection`を呼び出すと、`BindService`メソッドです。` BindService` 返されます`true`サービスが、バインドされている場合`false`されていない場合。 `BindService` メソッドは 3 つのパラメーターを受け取ります。

* **`Intent`**  &ndash;を目的としたが、どのサービスへの接続を明示的に識別する必要があります。
* **`IServiceConnection`オブジェクト**&ndash;このオブジェクトは、バインドされているサービスが開始および停止時にクライアントに通知するコールバック メソッドを提供する媒介手段です。
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) enum** &ndash;フラグのセットは、このパラメーターは、システムによってときに、オブジェクトにバインドするために使用します。 最もよく使用される値は[ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/)、まだ実行されていない場合を自動的にサービスが開始されます。

次のコード スニペットは、明示的な目的を使用して、アクティビティにバインドされているサービスを開始する方法の例を示します。

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> 明示的な目的のサービスにバインドすることだけが Android 5.0 (API レベル 21) を開始します。

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>サービスの接続とバインダーのアーキテクチャについてはします。

OOP 主義者はの以前の実装のしない可能性があります、`TimestampBinder`の違反がクラス、[デメテルの](https://en.wikipedia.org/wiki/Law_of_Demeter)、最も単純な形式で示す"しないと通信する知らない人です。のみと通信できる友人"です。 この特定の実装は、具象型を公開`TimestampService`クラスのすべてのクライアントにします。

厳密には、クライアントについて知っておく必要はありません、`TimestampService`とクライアントをその具象クラスを公開することができますアプリケーションより当てとその有効期間にわたって管理するは困難です。 別の方法は、公開するインターフェイスを使用する、`GetFormattedTimestamp()`メソッド、およびプロキシによるを通じてサービスの呼び出し、 `Binder` (または可能なサービス接続クラス)。  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

この例は、サービス自体でメソッドを呼び出すアクティビティを使用します。

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>関連リンク

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
