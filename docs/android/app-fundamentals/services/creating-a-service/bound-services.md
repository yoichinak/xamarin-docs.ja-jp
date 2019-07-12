---
title: Xamarin.Android でのサービスをバインドします。
description: バインドされているサービスは、(Android アクティビティ) などのクライアントが対話できるクライアントとサーバー間のインターフェイスを提供する Android のサービスです。 このガイドは、バインドされているサービスと、Xamarin.Android アプリケーションで使用する方法の作成に関連する主要なコンポーネントについて説明します。
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: 490331663d94a1e3130fc794a11a52acdacca014
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829741"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin.Android でのサービスをバインドします。

_バインドされているサービスは、(Android アクティビティ) などのクライアントが対話できるクライアントとサーバー間のインターフェイスを提供する Android のサービスです。このガイドは、バインドされているサービスと、Xamarin.Android アプリケーションで使用する方法の作成に関連する主要なコンポーネントについて説明します。_

## <a name="bound-services-overview"></a>バインドされているサービスの概要

サービスと直接通信するクライアント用のクライアント/サーバー インターフェイスを提供するサービスと呼びます_サービスのバインド_します。  複数のクライアントが同時に、サービスの 1 つのインスタンスに接続されていることができます。 バインドされているサービスとクライアントは、互いから分離されます。 代わりに、Android は、一連の 2 つの間の接続の状態を管理する中間のオブジェクトを提供します。 この状態が実装するオブジェクトによって維持されて、 [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)インターフェイス。  このオブジェクトがクライアントによって作成され、パラメーターとして渡される、 [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/)メソッド。 `BindService`がいずれかで利用[ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context)オブジェクト (活動) など。 これは、サービスを開始し、それをクライアントに連結する Android のオペレーティング システムに要求します。 クライアントに 3 つの方法を使用してサービスにバインドがある、`BindService`メソッド。

* **サービス バインダー** &ndash;サービス バインダーが実装するクラス、 [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder)インターフェイス。 ほとんどのアプリケーションはこのインターフェイスを直接実装しません、拡張は、代わりに、 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder)クラス。 最も一般的なアプローチは、これは、同じプロセスで、サービスとクライアントが存在する場合に適してします。
* **Messenger を使用して**&ndash;この手法は、別のプロセスで、サービスが存在する場合に適しています。 使用するサービスとクライアントとサービス要求が代わりに、マーシャ リング、 [ `Android.OS.Messenger`](https://developer.xamarin.com/api/type/Android.OS.Messenger)します。 [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler)を処理するサービスで作成されたが、`Messenger`要求。 これについては、別のガイドで説明します。
* **Android インターフェイス定義言語 (AIDL) を使用して** &ndash; [AIDL](https://developer.android.com/guide/components/aidl)はこのガイドでは説明できませんの高度な手法です。

クライアントは、サービスにバインドされていますが、両者間の通信が経由で発生する`Android.OS.IBinder`オブジェクト。  このオブジェクトは、インターフェイス、クライアント、サービスと対話できるようにする責任を負います。 最初からこのインターフェイスを実装するには、各 Xamarin.Android アプリケーションの必要はありません、Android SDK には、 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder)クラスの間でオブジェクトをマーシャ リングに必要なコードのほとんどを処理します。クライアントとサービス。

クライアントでは、サービス処理が終わったらに呼び出すことによって、そこから解除する必要があります、`UnbindService`メソッド。 最後のクライアントがサービスからバインドされていないと、Android は停止し、バインドされているサービスを破棄します。

この図は、アクティビティ、サービスの接続、バインダーでは、およびサービスはすべてが相互に関連する方法を示しています。

![サービス コンポーネントが相互に関連付ける方法を示す図](bound-services-images/bound-services-02.png "サービス コンポーネントが相互に関連付ける方法を示す図。")

このガイドは、拡張する方法を説明する、`Service`バインドされているサービスを実装するクラス。 実装も含まれます`IServiceConnection`と拡張`Binder`クライアントがサービスと通信できるようにします。 サンプル アプリと呼ばれる 1 つの Xamarin.Android プロジェクトとソリューションを含むをこのガイドに付属 **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** します。 これは、非常に基本的なアプリケーションをサービスを実装する方法とアクティビティを連結する方法を示します。 バインドされているサービスが 1 つのメソッドで非常に単純な API`GetFormattedTimestamp`サービスが開始されたときに、ユーザーに指示する文字列とどのくらいの時間実行されたが返されます。 アプリでは、手動でバインドを解除し、サービスにバインドするユーザーもできます。

[![Android フォンで実行されているアプリケーションのスクリーン ショット](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>バインドされているサービスの実装と

Android アプリケーションのバインドされているサービスを使用するために実装する必要が 3 つのコンポーネントがあります。

1. **拡張、`Service`クラスし、ライフ サイクルのコールバック メソッドを実装**&ndash;サービスの要求が作業を実行するコードをこのクラスが含まれます。 これについては、以下で詳しく説明します。
2. **実装するクラスを作成`IServiceConnection`**  &ndash;コールバック メソッドが呼び出されるこのインターフェイスを提供して、サービスへの接続が変更されたときにクライアントに通知を Android、つまりクライアントが接続または切断する、サービス。 サービスの接続には、クライアントがサービスと直接対話に使用できるオブジェクトへの参照が提供されます。 この参照と呼ばれる、_バインダー_します。
3. **実装するクラスを作成`IBinder`**  &ndash; A_バインダー_実装には、クライアントがサービスとの通信に使用する API が用意されています。 バインダー入力するか、バインドされている、サービスへの参照を直接呼び出されるメソッドを許可または、バインダーは、クライアントをカプセル化し、アプリケーションからバインドされているサービスを非表示にする API を提供できます。 `IBinder`リモート プロシージャ呼び出しのために必要なコードを提供する必要があります。 必要な (または推奨される) ではありませんを実装する、`IBinder`インターフェイスを直接します。 代わりにアプリケーションを拡張する必要があります、`Binder`に必要な基本機能のほとんどを提供する型、`IBinder`します。
4. **サービスへのバインドの開始と** &ndash; Android アプリケーションがサービスを開始すると、それへのバインドがサービスの接続、バインダー、およびサービスが作成されるとします。

これらの各手順については、さらに詳しくは、次のセクションで説明します。

### <a name="extend-the-service-class"></a>拡張、`Service`クラス

Xamarin.Android を使用してサービスを作成するにはサブクラス化するために必要な`Service`を使用してクラスを装飾して、 [ `ServiceAttribute`](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)します。 Xamarin.Android のビルド ツールによって、アプリのサービスを適切に登録する属性が使用される**AndroidManifest.xml**ファイル アクティビティと同様、バインドされているサービスが、独自のライフ サイクル メソッドとコールバック メソッドに関連付けられている、it のライフ サイクルの重要なイベント。 一部のサービスを実装する一般的なコールバック メソッドの例を次に示します。

* `OnCreate` &ndash; このメソッドが、サービスがインスタンス化する、Android によって呼び出されます。 すべての変数またはの有効期間中に、サービスに必要なオブジェクトを初期化するために使用されます。 このメソッドは省略可能です。
* `OnBind` &ndash; このメソッドは、バインドされたすべてのサービスによって実装する必要があります。 クライアントが最初に、サービスに接続しようとするときに呼び出されます。 インスタンスが返されます`IBinder`クライアントがサービスと対話できるようにします。 サービスが実行されている限り、`IBinder`オブジェクトは、サービスにバインドするには、将来のクライアント要求を満たすために使用されます。
* `OnUnbind` &ndash; バインドされたすべてのクライアントがバインドされていない場合は、このメソッドが呼び出されます。 返すことによって`true`からこのメソッドは、サービスを呼び出すことが後で`OnRebind`に渡される目的で`OnUnbind`に新しいクライアントをバインドするとします。 サービスは、そのバインドが解除された後に実行を継続する場合に、これを行うとします。 これは、現象は、最近にバインドされていないサービスが開始されるサービスでも場合と`StopService`または`StopSelf`呼び出されていません。 このようなシナリオで`OnRebind`を取得することを目的に使用します。 既定値を返します`false`、何もしません。 任意。
* `OnDestroy` &ndash; Android は、サービスを破棄するときに、このメソッドが呼び出されます。 このメソッドで、リソースの解放など、必要なクリーンアップを実行する必要があります。 任意。

この図にバインドされているサービスのキーのライフ サイクル イベントを示します。

![ライフ サイクル メソッドが呼び出される順序を示す図](bound-services-images/bound-services-01.png "ライフ サイクル メソッドが呼び出される順序を示す図。")

このガイドに付属するコンパニオン アプリケーションから、次のコード スニペットでは、Xamarin.Android でバインドされているサービスを実装する方法を示します。

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

サンプルでは、`OnCreate`メソッドを取得し、タイムスタンプ、クライアントによって要求されるかを書式設定のロジックを保持するオブジェクトを初期化します。 最初のクライアントがサービスにバインドしようとすると、Android が呼び出す、`OnBind`メソッド。 このサービスがインスタンス化、`TimestampBinder`クライアントが実行中のサービスのこのインスタンスにアクセスできるようにするオブジェクト。 `TimestampBinder`クラスは、次のセクションで説明します。

### <a name="implementing-ibinder"></a>IBinder を実装します。

前述のように、`IBinder`オブジェクトは、クライアントとサービス間の通信チャネルを提供します。 Android アプリケーションを実装しないでください、 `IBinder` 、インターフェイス、 [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/)拡張する必要があります。 `Binder`クラスが必要なマーシャ リングするには、必要なインフラストラクチャの多くに、クライアントに (別のプロセスで実行される可能性があります) をサービスからバインダー オブジェクトを提供します。 ほとんどの場合、`Binder`サブクラスのみ、わずか数行のコードは、およびサービスへの参照をラップします。 この例で`TimestampBinder`を公開するプロパティを持つ、`TimestampService`クライアント。

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

これは、`Binder`可能がサービスにパブリック メソッドを呼び出す例。

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

1 回`Binder`されましたが、拡張する必要がありますを実装`IServiceConnection`すべてをまとめて接続します。

### <a name="creating-the-service-connection"></a>サービス接続を作成します。

`IServiceConnection`が表示されます | 導入 | 公開 | 接続、`Binder`クライアント オブジェクトです。 実装するだけでなく、`IServiceConnection`インターフェイス、クラスを拡張する必要があります`Java.Lang.Object`します。 サービスの接続は、クライアントがアクセスできる何らかの方法も提供する必要があります、 `Binder` (およびそのため、バインドされているサービスとの通信)。

このコードに付属するサンプル プロジェクトを実装する方法の 1 つは、 `IServiceConnection`:

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

Android を呼び出し、バインド プロセスの一部として、`OnServiceConnected`メソッドを提供する、`name`バインドされているサービスの`binder`サービス自体への参照を保持しています。 サービス接続では、この例では、1 つかどうか、クライアントがサービスに接続されている場合、バインダーのブール フラグへの参照を保持する 2 つのプロパティがあります。

`OnServiceDisconnected`メソッドは、クライアントとサービス間の接続が予期せず紛失したり壊れた場合にのみ呼び出されます。 このメソッドは、クライアントはサービスの中断に対応する機会をできます。  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>開始して、明示的なインテントによるサービスへのバインド

バインドされているサービスを使用するには (アクティビティ) などのクライアントに実装するオブジェクトをインスタンス化`Android.Content.IServiceConnection`を呼び出すと、`BindService`メソッド。 `BindService` 戻ります`true`場合は、サービスには、バインドされた`false`でない場合。 `BindService` メソッドは 3 つのパラメーターを受け取ります。

* **`Intent`**  &ndash;インテントがへの接続にサービスを明示的に識別する必要があります。
* **`IServiceConnection`オブジェクト**&ndash;このオブジェクトがバインドされているサービスが開始および停止時にクライアントに通知するコールバック メソッドを提供する媒介手段です。
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) 列挙型**&ndash;このパラメーターは、フラグのセットは、オブジェクトをバインドする場合に、システムで使用します。 最もよく使用される値は[ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/)、既に実行されていない場合を自動的にサービスが開始されます。

次のコード スニペットでは、明示的なインテントを使用して、アクティビティにバインドされているサービスを開始する方法の例を示します。

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
> Android 5.0 (API レベル 21) 以降では、明示的なインテントによるサービスにバインドすることのみです。

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>サービスの接続とバインダーのアーキテクチャについてはします。

以前の実装の OOP 主義者が非承認、`TimestampBinder`クラスの違反があるため、[デメテルの](https://en.wikipedia.org/wiki/Law_of_Demeter)する、最も単純な形式で状態"は見知らぬ; 対話しません。のみ友人に話しかける"。 この特定の実装は、具象型を公開する`TimestampService`クラスすべてのクライアントにします。

厳密に言うと、クライアントについて知っておく必要はありません、`TimestampService`クライアントにその具象クラスを公開し、アプリケーションのより不安定での有効期間にわたって管理するは困難であるとします。 別の方法は、公開するインターフェイスを使用する、`GetFormattedTimestamp()`メソッド、およびプロキシ呼び出しを通じてサービスに、 `Binder` (またはサービスは、可能な限り)。  

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

この例は、サービス自体でメソッドを呼び出すアクティビティを許可します。

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
