---
title: Xamarin. Android のバインドされたサービス
description: バインドされたサービスは、クライアント (Android アクティビティなど) が対話できるクライアントサーバーインターフェイスを提供する Android サービスです。 このガイドでは、バインドされたサービスの作成に関連する主要なコンポーネントと、それを Xamarin Android アプリケーションで使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/04/2018
ms.openlocfilehash: 3aacb0f9b3aca0c8b64e01eb79270e73750c9f3d
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455504"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin. Android のバインドされたサービス

_バインドされたサービスは、クライアント (Android アクティビティなど) が対話できるクライアントサーバーインターフェイスを提供する Android サービスです。このガイドでは、バインドされたサービスの作成に関連する主要なコンポーネントと、それを Xamarin Android アプリケーションで使用する方法について説明します。_

## <a name="bound-services-overview"></a>バインドされたサービスの概要

クライアントがサービスと直接対話するためのクライアントサーバーインターフェイスを提供するサービスは、バインドされた  _サービス_と呼ばれます。  サービスの1つのインスタンスに同時に接続されている複数のクライアントが存在する場合があります。 バインドされたサービスとクライアントは相互に分離されます。 代わりに、Android には、2つの間の接続の状態を管理する一連の中間オブジェクトが用意されています。 この状態は、インターフェイスを実装するオブジェクトによって維持され [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection) ます。  このオブジェクトは、クライアントによって作成され、パラメーターとしてメソッドに渡され [`BindService`](xref:Android.Content.Context.BindService*) ます。 は、 `BindService` 任意の [`Android.Content.Context`](xref:Android.Content.Context) オブジェクト (アクティビティなど) で使用できます。 これは、サービスを起動してクライアントをバインドするための Android オペレーティングシステムへの要求です。 クライアントがメソッドを使用してサービスにバインドするには、次の3つの方法があり `BindService` ます。

- **サービスバインダー** &ndash; サービスバインダーは、インターフェイスを実装するクラスです [`Android.OS.IBinder`](xref:Android.OS.IBinder) 。 ほとんどのアプリケーションは、このインターフェイスを直接実装するのではなく、クラスを拡張します [`Android.OS.Binder`](xref:Android.OS.Binder) 。 これは最も一般的な方法であり、サービスとクライアントが同じプロセス内に存在する場合に適しています。
- Messenger を使用**する** &ndash;この手法は、サービスが別のプロセスに存在する可能性がある場合に適しています。 代わりに、を使用して、サービス要求をクライアントとサービスの間でマーシャリングし [`Android.OS.Messenger`](xref:Android.OS.Messenger) ます。 [`Android.OS.Handler`](xref:Android.OS.Handler)要求を処理するサービスにが作成され `Messenger` ます。 これについては、別のガイドで説明します。
- **Android インターフェイス定義言語 (AIDL)** &ndash; の使用 [Aidl](https://developer.android.com/guide/components/aidl) は、このガイドでは説明されていない高度な手法です。

クライアントがサービスにバインドされると、2つの間の通信はオブジェクトを介して行われ `Android.OS.IBinder` ます。  このオブジェクトは、クライアントがサービスと対話できるようにするインターフェイスを担います。 各 Xamarin. Android アプリケーションでは、このインターフェイスを最初から実装する必要はありません。 Android SDK には、 [`Android.OS.Binder`](xref:Android.OS.Binder) クライアントとサービスの間でオブジェクトをマーシャリングするために必要なほとんどのコードを処理するクラスが用意されています。

クライアントがサービスを使用して実行される場合は、メソッドを呼び出すことによって、クライアントをバインド解除する必要があり `UnbindService` ます。 最後のクライアントがサービスからバインド解除されると、Android はバインドされたサービスを停止して破棄します。

この図は、アクティビティ、サービス接続、バインダー、およびサービスが相互にどのように関連しているかを示しています。

![サービスコンポーネントが相互にどのように関連しているかを示す図](bound-services-images/bound-services-02.png "サービスコンポーネントが相互にどのように関連しているかを示す図。")

このガイドでは、クラスを拡張して、バインドされたサービスを実装する方法について説明し `Service` ます。 また、 `IServiceConnection` `Binder` クライアントがサービスと通信できるようにするための実装と拡張についても説明します。 サンプルアプリは、このガイドに付属しています。このガイドには、 **[Boundservicedemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** と呼ばれる単一の Xamarin Android プロジェクトを含むソリューションが含まれています。 これは、サービスを実装する方法と、そのサービスにアクティビティをバインドする方法を示す、非常に基本的なアプリケーションです。 バインドされたサービスには、メソッドが1つだけ含まれる非常に単純な API があり `GetFormattedTimestamp` ます。これは、サービスが開始されたときにユーザーに通知する文字列を返し、その実行時間を示します。 このアプリでは、ユーザーが手動でサービスのバインドを解除してバインドすることもできます。

[![Android フォンで実行されているアプリケーションのスクリーンショット](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>バインドされたサービスの実装と使用

Android アプリケーションでバインドされたサービスを使用するには、次の3つのコンポーネントを実装する必要があります。

1. ** `Service` クラスを拡張し、ライフサイクルコールバックメソッド** &ndash; を実装するこのクラスには、サービスに要求される作業を実行するコードが含まれます。 詳細については、以下で詳しく説明します。
2. **を実装 `IServiceConnection` するクラスを作成します**。&ndash;このインターフェイスは、サービスへの接続が変更されたときにクライアントに通知するために、Android によって呼び出されるコールバックメソッドを提供します。つまり、クライアントがサービスに接続または切断されています。 サービス接続は、クライアントがサービスと直接対話するために使用できるオブジェクトへの参照も提供します。 この参照は、 _バインダー_と呼ばれます。
3. **を実装 `IBinder` するクラスを作成します**。&ndash;_バインダー_の実装は、クライアントがサービスとの通信に使用する API を提供します。 バインダーは、バインドされたサービスへの参照を提供することにより、メソッドを直接呼び出すことができます。また、バインダーは、バインドされたサービスをアプリケーションからカプセル化して非表示にするクライアント API を提供できます。 は、 `IBinder` リモートプロシージャ呼び出しに必要なコードを提供する必要があります。 インターフェイスを直接実装する必要はありません (または推奨されません) `IBinder` 。 代わりに、アプリケーションは、 `Binder` で必要な基本機能のほとんどを提供する型を拡張する必要があり `IBinder` ます。
4. サービスの開始**とバインド** &ndash;サービス接続、バインダー、サービスが作成されたら、Android アプリケーションはサービスを開始し、それにバインドします。

これらの各手順については、以降のセクションで詳しく説明します。

### <a name="extend-the-service-class"></a>クラスを拡張する `Service`

装飾を使用してサービスを作成するには、クラスをにサブクラス化し、を使用する必要が `Service` [`ServiceAttribute`](xref:Android.App.ServiceAttribute) あります。 この属性は、Xamarin. Android ビルドツールがアクティビティと同様に **AndroidManifest.xml** サービスを適切に登録するために使用します。バインドされたサービスには、ライフサイクルの重要なイベントに関連付けられた独自のライフサイクルとコールバックメソッドがあります。 次の一覧は、サービスが実装する一般的なコールバックメソッドの例を示しています。

- `OnCreate`&ndash;このメソッドは、サービスをインスタンス化しているため、Android によって呼び出されます。 これは、サービスが有効期間中に必要とする変数またはオブジェクトを初期化するために使用されます。 このメソッドは省略可能です。
- `OnBind`&ndash;このメソッドは、すべてのバインドされたサービスで実装する必要があります。 これは、最初のクライアントがサービスに接続しようとしたときに呼び出されます。 `IBinder`クライアントがサービスと対話できるように、のインスタンスを返します。 サービスが実行されている限り、このオブジェクトは、 `IBinder` サービスにバインドするための将来のクライアント要求を処理するために使用されます。
- `OnUnbind`&ndash;このメソッドは、バインドされたすべてのクライアントがバインド解除されたときに呼び出されます。 `true`このメソッドから制御が戻ると、サービスは後で、 `OnRebind` `OnUnbind` 新しいクライアントがそれにバインドしたときにに渡されたインテントを使用してを呼び出します。 この操作は、サービスがバインド解除された後も実行を継続する場合に行います。 これは、最近バインドされていないサービスも開始されたサービスである  `StopService` か、または呼び出されなかった場合に発生し  `StopSelf` ます。 このようなシナリオでは、を  `OnRebind` 使用して目的を取得できます。 既定値はを返し  `false` ます。これは何も行いません。 任意。
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

このサンプルでは、 `OnCreate` クライアントによって要求されるタイムスタンプを取得して書式設定するためのロジックを保持するオブジェクトを、メソッドによって初期化します。 最初のクライアントがサービスにバインドしようとすると、Android はメソッドを呼び出し `OnBind` ます。 このサービスは、 `TimestampBinder` クライアントが実行中のサービスのこのインスタンスにアクセスできるようにするオブジェクトをインスタンス化します。 `TimestampBinder`クラスについては、次のセクションで説明します。

### <a name="implementing-ibinder"></a>IBinder の実装

前述のように、オブジェクトは、 `IBinder` クライアントとサービス間の通信チャネルを提供します。 Android アプリケーションではインターフェイスを実装しない `IBinder` でください [`Android.OS.Binder`](xref:Android.OS.Binder) 。を拡張する必要があります。 クラスは、 `Binder` (別のプロセスで実行されている可能性がある) バインダーオブジェクトをサービスからクライアントにマーシャリングするために必要なインフラストラクチャの多くを提供します。 ほとんどの場合、 `Binder` サブクラスは少数のコード行にすぎないため、サービスへの参照をラップします。 この例では、にを `TimestampBinder` クライアントに公開するプロパティがあり `TimestampService` ます。

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

これに `Binder` より、サービスでパブリックメソッドを呼び出すことができるようになります。次に例を示します。

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

`Binder`が拡張されたら、すべてを接続するためにを実装する必要があり `IServiceConnection` ます。

### <a name="creating-the-service-connection"></a>サービス接続の作成

`IServiceConnection`が表示されます | 導入 | 公開 | `Binder` クライアントにオブジェクトを接続します。 インターフェイスの実装に加え `IServiceConnection` て、クラスはを拡張する必要があり `Java.Lang.Object` ます。 また、サービス接続では、クライアントがにアクセスし `Binder` て (したがって、バインドされたサービスと通信する) 何らかの方法を提供する必要もあります。

このコードは、付随するサンプルプロジェクトからのものであり、次のように実装することができ `IServiceConnection` ます。

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

バインドプロセスの一環として、Android はメソッドを呼び出し `OnServiceConnected` 、 `name` バインドされるサービスのと `binder` サービス自体への参照を保持するを提供します。 この例では、サービス接続に2つのプロパティがあります。1つはバインダーへの参照を保持し、もう1つはクライアントがサービスに接続されている場合はのブール値フラグです。

メソッドは、 `OnServiceDisconnected` クライアントとサービス間の接続が予期せず失われたり、破損したりした場合にのみ呼び出されます。 このメソッドを使用すると、クライアントはサービスの中断に応答する可能性があります。  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>明示的なインテントを使用してサービスを開始およびバインドする

バインドされたサービスを使用するには、クライアント (アクティビティなど) がを実装し、メソッドを呼び出すオブジェクトをインスタンス化する必要があり `Android.Content.IServiceConnection` `BindService` ます。 `BindService``true`サービスがにバインドされている場合は、それ以外の場合はを返し `false` ます。 `BindService` メソッドは 3 つのパラメーターを受け取ります。

- **A `Intent` **&ndash;インテントは、接続先のサービスを明示的に識別する必要があります。
- **A `IServiceConnection` オブジェクト** &ndash; このオブジェクトは、バインドされたサービスの開始時および停止時にクライアントに通知するコールバックメソッドを提供する仲介役です。
- ** [`Android.Content.Bind`](xref:Android.Content.Bind) 列挙型** &ndash;このパラメーターは、オブジェクトをバインドするときにシステムによって使用されるフラグのセットです。 最も一般的に使用される値は [`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate) で、サービスがまだ実行されていない場合は自動的に開始されます。

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

いくつかの OOP purists は、デメテルの法則に違反するため、クラスの以前の実装を disapprove 可能性があります `TimestampBinder` 。これは、最も単純な形式の状態では、"知らない人と話すことはない" ということです。 [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter)友人と話し合っただけです。 この特定の実装は、具象 `TimestampService` クラスをすべてのクライアントに公開します。

厳密に言うと、クライアントがを認識し、その `TimestampService` 具象クラスをクライアントに公開する必要はありません。これにより、アプリケーションが不安定になり、有効期間の経過と共に維持することが難しくなります。 別の方法として、メソッドを公開するインターフェイス `GetFormattedTimestamp()` と、 `Binder` (またはサービス接続クラス) を使用してサービスへのプロキシ呼び出しを使用する方法があります。  

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

- [Android. App. サービス](xref:Android.App.Service)
- [Android. コンテンツ. Bind](xref:Android.Content.Bind)
- [Android. コンテンツコンテキスト](xref:Android.Content.Context)
- [IServiceConnection](xref:Android.Content.IServiceConnection)
- [Android. OS バインダー](xref:Android.OS.Binder)
- [Android. OS. IBinder](xref:Android.OS.IBinder)
- [BoundServiceDemo (サンプル)](/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)