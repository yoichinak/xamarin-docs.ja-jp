---
title: Xamarin. Android のバインドされたサービス
description: バインドされたサービスは、クライアント (Android アクティビティなど) が対話できるクライアントサーバーインターフェイスを提供する Android サービスです。 このガイドでは、バインドされたサービスの作成に関連する主要なコンポーネントと、それを Xamarin Android アプリケーションで使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: ae1e8332f1d62a4690863a97f63c0c1bef1ee127
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119089"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin. Android のバインドされたサービス

_バインドされたサービスは、クライアント (Android アクティビティなど) が対話できるクライアントサーバーインターフェイスを提供する Android サービスです。このガイドでは、バインドされたサービスの作成に関連する主要なコンポーネントと、それを Xamarin Android アプリケーションで使用する方法について説明します。_

## <a name="bound-services-overview"></a>バインドされたサービスの概要

クライアントがサービスと直接対話するためのクライアントサーバーインターフェイスを提供するサービスは、バインドされた_サービス_と呼ばれます。  サービスの1つのインスタンスに同時に接続されている複数のクライアントが存在する場合があります。 バインドされたサービスとクライアントは相互に分離されます。 代わりに、Android には、2つの間の接続の状態を管理する一連の中間オブジェクトが用意されています。 この状態は、 [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection)インターフェイスを実装するオブジェクトによって維持されます。  このオブジェクトは、クライアントによって作成され、パラメーターと[`BindService`](xref:Android.Content.Context.BindService*)してメソッドに渡されます。 は`BindService` 、任意[`Android.Content.Context`](xref:Android.Content.Context)のオブジェクト (アクティビティなど) で使用できます。 これは、サービスを起動してクライアントをバインドするための Android オペレーティングシステムへの要求です。 クライアントが`BindService`メソッドを使用してサービスにバインドするには、次の3つの方法があります。

- **サービスバインダー**サービスバインダーは、 [`Android.OS.IBinder`](xref:Android.OS.IBinder)インターフェイスを実装するクラスです。 &ndash; ほとんどの[`Android.OS.Binder`](xref:Android.OS.Binder)アプリケーションは、このインターフェイスを直接実装するのではなく、クラスを拡張します。 これは最も一般的な方法であり、サービスとクライアントが同じプロセス内に存在する場合に適しています。
- **Messenger を使用する**&ndash;この手法は、サービスが別のプロセスに存在する可能性がある場合に適しています。 代わりに、を使用[`Android.OS.Messenger`](xref:Android.OS.Messenger)して、サービス要求をクライアントとサービスの間でマーシャリングします。 要求を`Messenger`処理するサービスに[が作成されます。`Android.OS.Handler`](xref:Android.OS.Handler) これについては、別のガイドで説明します。
- **Android インターフェイス定義言語 (AIDL) の使用** &ndash; [Aidl](https://developer.android.com/guide/components/aidl) は、このガイドでは説明されていない高度な手法です。

クライアントがサービスにバインドされると、2つの間の通信はオブジェクト`Android.OS.IBinder`を介して行われます。  このオブジェクトは、クライアントがサービスと対話できるようにするインターフェイスを担います。 各 Xamarin. Android アプリケーションでは、このインターフェイスを最初から実装する必要はありません。 [`Android.OS.Binder`](xref:Android.OS.Binder) Android SDK には、クライアントとサービスの間でオブジェクトをマーシャリングするために必要なほとんどのコードを処理するクラスが用意されています。

クライアントがサービスを使用して実行される場合は、 `UnbindService`メソッドを呼び出すことによって、クライアントをバインド解除する必要があります。 最後のクライアントがサービスからバインド解除されると、Android はバインドされたサービスを停止して破棄します。

この図は、アクティビティ、サービス接続、バインダー、およびサービスが相互にどのように関連しているかを示しています。

![サービスコンポーネントが相互にどのように関連しているかを示す図](bound-services-images/bound-services-02.png "サービスコンポーネントが相互にどのように関連しているかを示す図。")

このガイドでは、クラスを拡張`Service`して、バインドされたサービスを実装する方法について説明します。 また、クライアントがサービス`IServiceConnection`と通信`Binder`できるようにするための実装と拡張についても説明します。 サンプル アプリと呼ばれる 1 つの Xamarin.Android プロジェクトとソリューションを含むをこのガイドに付属 **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** します。 これは、サービスを実装する方法と、そのサービスにアクティビティをバインドする方法を示す、非常に基本的なアプリケーションです。 バインドされたサービスには、メソッド`GetFormattedTimestamp`が1つだけ含まれる非常に単純な API があります。これは、サービスが開始されたときにユーザーに通知する文字列を返し、その実行時間を示します。 このアプリでは、ユーザーが手動でサービスのバインドを解除してバインドすることもできます。

[![Android フォンで実行されているアプリケーションのスクリーンショット](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>バインドされたサービスの実装と使用

Android アプリケーションでバインドされたサービスを使用するには、次の3つのコンポーネントを実装する必要があります。

1. **クラス`Service`を拡張し、ライフサイクルコールバックメソッド** &ndash;を実装します。このクラスには、サービスに要求される作業を実行するコードが含まれます。 詳細については、以下で詳しく説明します。
2. このインターフェイスを&ndash; **実装`IServiceConnection`するクラスを作成**します。このインターフェイスは、サービスへの接続が変更されたときにクライアントに通知するために、Android によって呼び出されるコールバックメソッドを提供します。つまり、クライアントが接続または切断されています。処理. サービス接続は、クライアントがサービスと直接対話するために使用できるオブジェクトへの参照も提供します。 この参照は、_バインダー_と呼ばれます。
3. _バインダー_の実装を実装&ndash;  **`IBinder`するクラスを作成**するクライアントがサービスとの通信に使用する API を提供します。 バインダーは、バインドされたサービスへの参照を提供することにより、メソッドを直接呼び出すことができます。また、バインダーは、バインドされたサービスをアプリケーションからカプセル化して非表示にするクライアント API を提供できます。 は`IBinder` 、リモートプロシージャ呼び出しに必要なコードを提供する必要があります。 `IBinder`インターフェイスを直接実装する必要はありません (または推奨されません)。 代わりに、 `Binder` `IBinder`アプリケーションは、で必要な基本機能のほとんどを提供する型を拡張する必要があります。
4. **サービスの開始とバインド**&ndash;サービス接続、バインダー、サービスが作成されたら、Android アプリケーションはサービスを開始し、それにバインドします。

これらの各手順については、以降のセクションで詳しく説明します。

### <a name="extend-the-service-class"></a>クラスを`Service`拡張する

装飾を使用してサービスを作成するには、 `Service` [`ServiceAttribute`](xref:Android.App.ServiceAttribute)クラスをにサブクラス化し、を使用する必要があります。 この属性は、Xamarin. Android ビルドツールによって使用されます。アクティビティと同様に、アプリの**Androidmanifest .xml**ファイルにサービスを適切に登録するために、バインドされたサービスには、の重要なイベントに関連付けられた独自のライフサイクルとコールバックメソッドがあります。ライフサイクルです。 次の一覧は、サービスが実装する一般的なコールバックメソッドの例を示しています。

- `OnCreate`&ndash;このメソッドは、サービスをインスタンス化しているため、Android によって呼び出されます。 これは、サービスが有効期間中に必要とする変数またはオブジェクトを初期化するために使用されます。 このメソッドは省略可能です。
- `OnBind`&ndash;このメソッドは、すべてのバインドされたサービスで実装する必要があります。 これは、最初のクライアントがサービスに接続しようとしたときに呼び出されます。 クライアントがサービスと対話できる`IBinder`ように、のインスタンスを返します。 サービスが実行`IBinder`されている限り、このオブジェクトは、サービスにバインドするための将来のクライアント要求を処理するために使用されます。
- `OnUnbind`&ndash;このメソッドは、バインドされたすべてのクライアントがバインド解除されたときに呼び出されます。 このメソッド`true`から制御が戻ると、サービスは後`OnRebind`で、新しいクライアントが`OnUnbind`それにバインドしたときにに渡されたインテントを使用してを呼び出します。 この操作は、サービスがバインド解除された後も実行を継続する場合に行います。 これは、最近バインドされていないサービスも開始され`StopService`た`StopSelf`サービスであるか、または呼び出されなかった場合に発生します。 このようなシナリオで`OnRebind`は、を使用して目的を取得できます。 既定値は`false`を返します。これは何も行いません。 任意。
- `OnDestroy`&ndash;このメソッドは、Android がサービスを破棄するときに呼び出されます。 リソースの解放などの必要なクリーンアップは、このメソッドで実行する必要があります。 任意。

バインドされたサービスのキーライフサイクルイベントは、次の図のようになります。

![ライフサイクルメソッドが呼び出される順序を示す図](bound-services-images/bound-services-01.png "ライフサイクルメソッドが呼び出される順序を示す図。")

次のコードスニペットは、このガイドに付属しているコンパニオンアプリケーションから、Xamarin. Android でバインドされたサービスを実装する方法を示しています。

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

このサンプル`OnCreate`では、クライアントによって要求されるタイムスタンプを取得して書式設定するためのロジックを保持するオブジェクトを、メソッドによって初期化します。 最初のクライアントがサービスにバインドしようとすると、Android は`OnBind`メソッドを呼び出します。 このサービスは、クライアント`TimestampBinder`が実行中のサービスのこのインスタンスにアクセスできるようにするオブジェクトをインスタンス化します。 クラス`TimestampBinder`については、次のセクションで説明します。

### <a name="implementing-ibinder"></a>IBinder の実装

前述のように`IBinder` 、オブジェクトは、クライアントとサービス間の通信チャネルを提供します。 Android アプリケーションでは`IBinder`インターフェイスを実装しない[`Android.OS.Binder`](xref:Android.OS.Binder)でください。を拡張する必要があります。 クラス`Binder`は、(別のプロセスで実行されている可能性がある) バインダーオブジェクトをサービスからクライアントにマーシャリングするために必要なインフラストラクチャの多くを提供します。 ほとんどの場合、 `Binder`サブクラスは少数のコード行にすぎないため、サービスへの参照をラップします。 この例では`TimestampBinder` 、にをクライアント`TimestampService`に公開するプロパティがあります。

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

これ`Binder`により、サービスでパブリックメソッドを呼び出すことができるようになります。次に例を示します。

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

が`Binder`拡張されたら、すべてを接続する`IServiceConnection`ためにを実装する必要があります。

### <a name="creating-the-service-connection"></a>サービス接続の作成

が表示されます | 導入 | 公開 | `Binder`クライアントにオブジェクトを接続します。 `IServiceConnection` インターフェイスの`IServiceConnection`実装に加えて、クラスはを拡張`Java.Lang.Object`する必要があります。 また、サービス接続では、クライアントがに`Binder`アクセスして (したがって、バインドされたサービスと通信する) 何らかの方法を提供する必要もあります。

このコードは、付随するサンプルプロジェクトからのものであり、 `IServiceConnection`次のように実装することができます。

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

バインドプロセスの一環として、Android は`OnServiceConnected`メソッドを呼び出し、バインドされるサービス`name`のと`binder`サービス自体への参照を保持するを提供します。 この例では、サービス接続に2つのプロパティがあります。1つはバインダーへの参照を保持し、もう1つはクライアントがサービスに接続されている場合はのブール値フラグです。

`OnServiceDisconnected`メソッドは、クライアントとサービス間の接続が予期せず失われたり、破損したりした場合にのみ呼び出されます。 このメソッドを使用すると、クライアントはサービスの中断に応答する可能性があります。  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>明示的なインテントを使用してサービスを開始およびバインドする

バインドされたサービスを使用するには、クライアント (アクティビティなど) がを実装`Android.Content.IServiceConnection`し、 `BindService`メソッドを呼び出すオブジェクトをインスタンス化する必要があります。 `BindService`サービスが`true`にバインドされている場合`false`は、それ以外の場合はを返します。 `BindService` メソッドは 3 つのパラメーターを受け取ります。

- インテントは、接続先のサービスを明示的に識別する必要があります。 **`Intent`** &ndash;
- **オブジェクト`IServiceConnection`**  。このオブジェクトは、バインドされたサービスの開始時および停止時にクライアントに通知するコールバックメソッドを&ndash;提供する仲介役です。
- 列挙このパラメーターは、オブジェクトをバインドするときにシステムによって使用されるフラグのセットです。 **[`Android.Content.Bind`](xref:Android.Content.Bind)** &ndash; 最も一般的に使用される[`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate)値はで、サービスがまだ実行されていない場合は自動的に開始されます。

次のコードスニペットは、明示的なインテントを使用して、アクティビティ内のバインドされたサービスを開始する方法の例です。

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
> Android 5.0 (API レベル 21) 以降では、明示的なインテントを使用してサービスにバインドすることのみが可能です。

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>サービス接続とバインダーに関するアーキテクチャメモ。

いくつかの OOP purists は、[デメテルの法則](https://en.wikipedia.org/wiki/Law_of_Demeter)に`TimestampBinder`違反するため、クラスの以前の実装を disapprove 可能性があります。これは、最も単純な形式の状態では、"知らない人と話すことはない" ということです。友人と話し合っただけです。 この特定の実装は、 `TimestampService`具象クラスをすべてのクライアントに公開します。

厳密に言うと、クライアントがを認識し、その具象クラス`TimestampService`をクライアントに公開する必要はありません。これにより、アプリケーションが不安定になり、有効期間の経過と共に維持することが難しくなります。 別の方法として、 `GetFormattedTimestamp()`メソッドを公開するインターフェイスと、(またはサービス接続クラス) を使用し`Binder`てサービスへのプロキシ呼び出しを使用する方法があります。  

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

この特定の例では、アクティビティがサービス自体でメソッドを呼び出すことができます。

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>関連リンク

- [Android.App.Service](xref:Android.App.Service)
- [Android.Content.Bind](xref:Android.Content.Bind)
- [Android.Content.Context](xref:Android.Content.Context)
- [Android.Content.IServiceConnection](xref:Android.Content.IServiceConnection)
- [Android.OS.Binder](xref:Android.OS.Binder)
- [Android.OS.IBinder](xref:Android.OS.IBinder)
- [BoundServiceDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)
