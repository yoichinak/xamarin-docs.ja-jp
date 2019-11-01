---
title: Xamarin. Android のバインドされたサービス
description: バインドされたサービスは、クライアント (Android アクティビティなど) が対話できるクライアントサーバーインターフェイスを提供する Android サービスです。 このガイドでは、バインドされたサービスの作成に関連する主要なコンポーネントと、それを Xamarin Android アプリケーションで使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/04/2018
ms.openlocfilehash: a3b0e8499d208f209de481163a236e5241c83ee6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024990"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin. Android のバインドされたサービス

_バインドされたサービスは、クライアント (Android アクティビティなど) が対話できるクライアントサーバーインターフェイスを提供する Android サービスです。このガイドでは、バインドされたサービスの作成に関連する主要なコンポーネントと、それを Xamarin Android アプリケーションで使用する方法について説明します。_

## <a name="bound-services-overview"></a>バインドされたサービスの概要

クライアントがサービスと直接対話するためのクライアントサーバーインターフェイスを提供するサービスは、バインドされた_サービス_と呼ばれます。  サービスの1つのインスタンスに同時に接続されている複数のクライアントが存在する場合があります。 バインドされたサービスとクライアントは相互に分離されます。 代わりに、Android には、2つの間の接続の状態を管理する一連の中間オブジェクトが用意されています。 この状態は、 [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection)インターフェイスを実装するオブジェクトによって維持されます。  このオブジェクトは、クライアントによって作成され、 [`BindService`](xref:Android.Content.Context.BindService*)メソッドにパラメーターとして渡されます。 `BindService` は、任意の[`Android.Content.Context`](xref:Android.Content.Context)オブジェクト (アクティビティなど) で使用できます。 これは、サービスを起動してクライアントをバインドするための Android オペレーティングシステムへの要求です。 クライアントが `BindService` メソッドを使用してサービスにバインドするには、次の3つの方法があります。

- サービスバインダー &ndash;**サービス**バインダーは、 [`Android.OS.IBinder`](xref:Android.OS.IBinder)インターフェイスを実装するクラスです。 ほとんどのアプリケーションは、このインターフェイスを直接実装するのではなく、 [`Android.OS.Binder`](xref:Android.OS.Binder)クラスを拡張します。 これは最も一般的な方法であり、サービスとクライアントが同じプロセス内に存在する場合に適しています。
- **Messenger を使用**する &ndash; この手法は、サービスが別のプロセスに存在する可能性がある場合に適しています。 代わりに、サービス要求は、 [`Android.OS.Messenger`](xref:Android.OS.Messenger)を介してクライアントとサービスの間でマーシャリングされます。 サービスには、`Messenger` 要求を処理する[`Android.OS.Handler`](xref:Android.OS.Handler)が作成されます。 これについては、別のガイドで説明します。
- **Android インターフェイス定義言語 (aidl)** &ndash; [aidl](https://developer.android.com/guide/components/aidl)は、このガイドでは説明されていない高度な手法です。

クライアントがサービスにバインドされると、2つの間の通信は `Android.OS.IBinder` オブジェクトを介して行われます。  このオブジェクトは、クライアントがサービスと対話できるようにするインターフェイスを担います。 各 Xamarin. Android アプリケーションでは、このインターフェイスを最初から実装する必要はありません。 Android SDK には、クライアントとサービスの間でオブジェクトをマーシャリングするために必要なほとんどのコードを処理する[`Android.OS.Binder`](xref:Android.OS.Binder)クラスが用意されています。

クライアントがサービスを使用して実行される場合は、`UnbindService` メソッドを呼び出すことによって、クライアントをバインド解除する必要があります。 最後のクライアントがサービスからバインド解除されると、Android はバインドされたサービスを停止して破棄します。

この図は、アクティビティ、サービス接続、バインダー、およびサービスが相互にどのように関連しているかを示しています。

![サービスコンポーネントが相互にどのように関連しているかを示す図](bound-services-images/bound-services-02.png "サービスコンポーネントが相互にどのように関連しているかを示す図。")

このガイドでは、`Service` クラスを拡張して、バインドされたサービスを実装する方法について説明します。 また、クライアントがサービスと通信できるようにするために、`IServiceConnection` の実装と `Binder` の拡張についても説明します。 サンプルアプリは、このガイドに付属しています。このガイドには、 **[Boundservicedemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** と呼ばれる単一の Xamarin Android プロジェクトを含むソリューションが含まれています。 これは、サービスを実装する方法と、そのサービスにアクティビティをバインドする方法を示す、非常に基本的なアプリケーションです。 バインドされたサービスには、メソッドが1つだけ含まれる非常に単純な API があります。 `GetFormattedTimestamp` は、サービスが開始されたときにユーザーに通知する文字列と、その実行時間を示します。 このアプリでは、ユーザーが手動でサービスのバインドを解除してバインドすることもできます。

[Android フォンで実行されているアプリケーションのスクリーンショットを![します。](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>バインドされたサービスの実装と使用

Android アプリケーションでバインドされたサービスを使用するには、次の3つのコンポーネントを実装する必要があります。

1. **`Service` クラスを拡張し、ライフサイクルコールバックメソッドを実装し**ます &ndash; このクラスには、サービスに要求される作業を実行するコードが含まれます。 詳細については、以下で詳しく説明します。
2. &ndash; **`IServiceConnection`を実装するクラスを作成**します。このインターフェイスは、サービスへの接続が変更されたときにクライアントに通知するために Android によって呼び出されるコールバックメソッドを提供します。つまり、クライアントがサービスに接続または切断されたことを示します。 サービス接続は、クライアントがサービスと直接対話するために使用できるオブジェクトへの参照も提供します。 この参照は、_バインダー_と呼ばれます。
3. &ndash; **`IBinder`を実装するクラスを作成**する_バインダー_の実装は、クライアントがサービスとの通信に使用する API を提供します。 バインダーは、バインドされたサービスへの参照を提供することにより、メソッドを直接呼び出すことができます。また、バインダーは、バインドされたサービスをアプリケーションからカプセル化して非表示にするクライアント API を提供できます。 `IBinder` は、リモートプロシージャ呼び出しに必要なコードを提供する必要があります。 `IBinder` インターフェイスを直接実装する必要はありません (または推奨されません)。 代わりに、アプリケーションは、`IBinder` に必要な基本機能の大部分を提供する `Binder` の種類を拡張する必要があります。
4. サービス接続、バインダー、およびサービスを作成した後に、サービス**を開始してバインド**する &ndash;、Android アプリケーションはサービスを開始し、それにバインドします。

これらの各手順については、以降のセクションで詳しく説明します。

### <a name="extend-the-service-class"></a>`Service` クラスを拡張する

装飾を使用してサービスを作成するには、`Service` をサブクラス化し、 [`ServiceAttribute`](xref:Android.App.ServiceAttribute)でクラスをする必要があります。 この属性は、Xamarin. Android ビルドツールによって使用されます。アクティビティと同様に、アプリの**Androidmanifest .xml**ファイルにサービスを適切に登録するために、バインドされたサービスには、の重要なイベントに関連付けられた独自のライフサイクルとコールバックメソッドがあります。ライフサイクルです。 次の一覧は、サービスが実装する一般的なコールバックメソッドの例を示しています。

- このメソッドは、サービスをインスタンス化しているときに Android によって呼び出さ &ndash; `OnCreate` ます。 これは、サービスが有効期間中に必要とする変数またはオブジェクトを初期化するために使用されます。 このメソッドは省略可能です。
- `OnBind` &ndash; このメソッドは、すべてのバインドされたサービスで実装する必要があります。 これは、最初のクライアントがサービスに接続しようとしたときに呼び出されます。 クライアントがサービスと対話できるように、`IBinder` のインスタンスを返します。 サービスが実行されている限り、`IBinder` オブジェクトを使用して、サービスにバインドするための将来のクライアント要求を処理します。
- `OnUnbind` &ndash; このメソッドは、バインドされたすべてのクライアントがバインド解除されたときに呼び出されます。 このメソッドから `true` を返すことにより、サービスは後で、新しいクライアントがバインドされるときに `OnUnbind` に渡されたインテントを使用して `OnRebind` を呼び出します。 この操作は、サービスがバインド解除された後も実行を継続する場合に行います。 これは、最近バインドされていないサービスも開始されたサービスであり、`StopService` または `StopSelf` が呼び出されなかった場合に発生します。 このようなシナリオでは、`OnRebind` を使用して目的を取得できます。 既定値は `false` を返します。これは何も行いません。 省略可能です。
- `OnDestroy` &ndash; このメソッドは、Android がサービスを破棄するときに呼び出されます。 リソースの解放などの必要なクリーンアップは、このメソッドで実行する必要があります。 省略可能です。

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

このサンプルでは、`OnCreate` メソッドによって、クライアントによって要求されるタイムスタンプを取得して書式設定するためのロジックを保持するオブジェクトが初期化されます。 最初のクライアントがサービスにバインドしようとすると、Android は `OnBind` メソッドを呼び出します。 このサービスは、クライアントが実行中のサービスのこのインスタンスにアクセスできるようにする `TimestampBinder` オブジェクトをインスタンス化します。 `TimestampBinder` クラスについては、次のセクションで説明します。

### <a name="implementing-ibinder"></a>IBinder の実装

前述のように、`IBinder` オブジェクトは、クライアントとサービス間の通信チャネルを提供します。 Android アプリケーションでは `IBinder` インターフェイスを実装しないでください。 [`Android.OS.Binder`](xref:Android.OS.Binder)を拡張する必要があります。 `Binder` クラスは、(別のプロセスで実行されている可能性がある) バインダーオブジェクトをサービスからクライアントにマーシャリングするために必要なインフラストラクチャの多くを提供します。 ほとんどの場合、`Binder` サブクラスは数行のコードにすぎないため、サービスへの参照をラップします。 この例では、`TimestampBinder` には `TimestampService` をクライアントに公開するプロパティがあります。

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

この `Binder` により、サービスでパブリックメソッドを呼び出すことができるようになります。例えば：

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

`Binder` が拡張されたら、すべての接続に `IServiceConnection` を実装する必要があります。

### <a name="creating-the-service-connection"></a>サービス接続の作成

`IServiceConnection` によって表示される | 導入 | 公開 | `Binder` オブジェクトをクライアントに接続します。 `IServiceConnection` インターフェイスの実装に加えて、クラスは `Java.Lang.Object`を拡張する必要があります。 また、サービス接続では、クライアントが `Binder` にアクセスできる何らかの方法 (したがって、バインドされたサービスと通信する) も提供する必要があります。

このコードは、付随するサンプルプロジェクトからのものであり、`IServiceConnection` を実装する方法の1つとして考えられます。

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

バインドプロセスの一環として、Android は `OnServiceConnected` メソッドを呼び出し、バインドされるサービスの `name` と、サービス自体への参照を保持する `binder` を提供します。 この例では、サービス接続に2つのプロパティがあります。1つはバインダーへの参照を保持し、もう1つはクライアントがサービスに接続されている場合はのブール値フラグです。

`OnServiceDisconnected` メソッドは、クライアントとサービス間の接続が予期せず失われたり、破損したりした場合にのみ呼び出されます。 このメソッドを使用すると、クライアントはサービスの中断に応答する可能性があります。  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>明示的なインテントを使用してサービスを開始およびバインドする

バインドされたサービスを使用するには、クライアント (アクティビティなど) が `Android.Content.IServiceConnection` を実装するオブジェクトをインスタンス化し、`BindService` メソッドを呼び出す必要があります。 サービスがバインドされている場合、`BindService` は `true` を返します。サービスがバインドされていない場合は `false` します。 `BindService` メソッドは 3 つのパラメーターを受け取ります。

- インテント &ndash; **`Intent` は、** 接続先のサービスを明示的に識別する必要があります。
- このオブジェクト &ndash; **`IServiceConnection` オブジェクト**は、バインドされたサービスが開始および停止されたときにクライアントに通知するコールバックメソッドを提供する仲介者です。
- **[`Android.Content.Bind`](xref:Android.Content.Bind)列挙型**&ndash; このパラメーターは、オブジェクトをバインドするときにシステムによって使用されるフラグのセットです。 最も一般的に使用される値は[`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate)です。サービスがまだ実行されていない場合は、サービスが自動的に開始されます。

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

一部の OOP purists は、[デメテルの法則](https://en.wikipedia.org/wiki/Law_of_Demeter)に違反しているため、`TimestampBinder` クラスの以前の実装を disapprove 可能性があります。これは、最も単純な形式の "" とは言えません。友人と話し合っただけです。 この特定の実装は、具象 `TimestampService` クラスをすべてのクライアントに公開します。

厳密に言うと、クライアントが `TimestampService` について認識し、その具象クラスをクライアントに公開する必要はありません。これにより、アプリケーションが不安定になり、有効期間の経過に伴う管理が困難になります。 別の方法として、`GetFormattedTimestamp()` メソッドを公開するインターフェイスと、`Binder` (またはサービス接続クラス) を使用してサービスへのプロキシ呼び出しを使用する方法があります。  

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
- [BoundServiceDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)
