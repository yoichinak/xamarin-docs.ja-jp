---
title: Google Cloud Messaging を使用したリモート通知
description: このチュートリアルでは、Xamarin.Android アプリケーションで Google Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれます) を実装する方法の詳細な手順の説明を提供します。 Google Cloud Messaging (GCM) と通信するために実装する必要があります、さまざまなクラスについて説明します、GCM へのアクセスの Android マニフェストでのアクセス許可を設定する方法について説明およびサンプルのテスト プログラムとエンド ツー エンドのメッセージングを示します。
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: e361444f2c717ff44e0771710836f156f90cfcb8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118891"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google Cloud Messaging を使用したリモート通知

_このチュートリアルでは、Xamarin.Android アプリケーションで Google Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれます) を実装する方法の詳細な手順の説明を提供します。Google Cloud Messaging (GCM) と通信するために実装する必要があります、さまざまなクラスについて説明します、GCM へのアクセスの Android マニフェストでのアクセス許可を設定する方法について説明およびサンプルのテスト プログラムとエンド ツー エンドのメッセージングを示します。_

> [!NOTE]
> GCM がによって置き換えられた[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM)。
> GCM サーバーおよびクライアント Api[非推奨とされました](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html)は利用できなく、2019 年 4 月 11 日早くとします。

## <a name="gcm-notifications-overview"></a>GCM 通知の概要

このチュートリアルで Google Cloud Messaging (GCM) を使用して、リモート通知を実装する Xamarin.Android アプリケーションを作成します (とも呼ばれます*プッシュ通知*)。 GCM を使用して、リモートのメッセージングでは、さまざまな目的、およびリスナー サービスを実装し、アプリケーション サーバーをシミュレートするコマンド ライン プログラムで今回の実装をテストします。 

Firebase Cloud Messaging (FCM) GCM の新しいバージョンであることに注意してください。 &ndash; GCM ではなく、FCM を使用して Google が強く推奨します。 GCM を使用している場合は、FCM にアップグレードすることをお勧めします。 FCM の詳細については、次を参照してください。 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)します。 

このチュートリアルを続行するには、Google の GCM サーバーを使用するために必要な資格情報を取得する必要があります。このプロセスについては[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)します。 具体的には、する必要があります、 *API キー*と*Sender ID*このチュートリアルで示すサンプル コードに挿入します。 

Xamarin.Android の GCM 対応のクライアント アプリを作成するのに、次の手順を使用します。

1.  GCM サーバーとの通信に必要な追加のパッケージをインストールします。
2.  GCM サーバーにアクセスするアプリのアクセス許可を構成します。
3.  Google play 開発者サービスの有無をチェックするコードを実装します。 
4.  登録トークン、GCM とネゴシエートし、登録インテント サービスを実装します。
5.  GCM から登録トークンの更新を待機するインスタンス ID リスナー サービスを実装します。
6.  GCM を使用してアプリ サーバーからリモートのメッセージを受信する GCM リスナー サービスを実装します。

このアプリと呼ばれる新しい GCM 機能を使用して*トピック メッセージング*します。 トピックでは、メッセージング アプリ サーバーは、個々 のデバイスの一覧ではなく、トピックにメッセージを送信します。 そのトピックにサブスクライブするデバイスは、プッシュ通知としてトピックのメッセージを受信できます。 GCM トピック メッセージングの詳細については、Google を参照してください。[トピックのメッセージングを実装する](https://developers.google.com/cloud-messaging/topic-messaging)します。 

クライアント アプリの準備ができたら、コマンド ライン実装C#GCM を使用したクライアント アプリにプッシュ通知を送信するアプリケーション。 

## <a name="walkthrough"></a>チュートリアル

まずと呼ばれる新しい空のソリューションを作成しましょう**RemoteNotifications**します。 次に、新しい Android プロジェクトに基づいているこのソリューションに追加、 **Android アプリ**テンプレート。 このプロジェクトをましょう**ClientApp**します。 (Xamarin.Android プロジェクトの作成を熟知していない場合を参照してください[Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)。)。**ClientApp** GCM を使用したリモート通知を受信する Xamarin.Android クライアント アプリケーションのコードをプロジェクトに含まれます。 

### <a name="add-required-packages"></a>必要なパッケージを追加します。

クライアント アプリ コードを実装できる、前に、GCM との通信に使用するいくつかのパッケージをインストールする必要があります。 またがインストールされていない場合、デバイスに Google Play ストア アプリケーションを追加します。

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin Google Play Services GCM パッケージを追加します。

Google Cloud Messaging からメッセージを受信する、 [Google play 開発者サービス](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/)framework デバイス上に存在する必要があります。 このフレームワークがない、Android アプリケーションは、GCM サーバーからメッセージを受信することはできません。 Google play 開発者サービスは、Android デバイスの電源が投入中に GCM からメッセージをリッスンしてサイレント モードに、バック グラウンドで実行されます。 これらのメッセージを受信する、Google play 開発者サービスはインテントに、メッセージを変換し、し、これらのインテントに登録されているアプリケーションにブロードキャストされます。 

Visual Studio で、右クリックして**参照 > NuGet パッケージの管理.**; Visual studio for Mac を右クリックして**パッケージ > パッケージを追加しています.**.検索**Xamarin Google play 開発者サービス - GCM**にこのパッケージをインストールし、 **ClientApp**プロジェクト。 

[![Google Play Services のインストール](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

インストールするときに**Xamarin Google play 開発者サービス - GCM**、 **Xamarin Google play 開発者サービスのベース**は自動的にインストールします。 エラーが発生する場合の変更、プロジェクトの*最小 Android ターゲット*以外の値に設定**SDK のバージョンを使用してコンパイル**と NuGet のインストールをもう一度やり直してください。 

次に、編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Android.Gms.Common;
using Android.Util;
```

これは、型で Google Play Services GMS パッケージを使用できるように、コードと GMS と、トランザクションを追跡するために使用するログ記録機能を追加します。 

#### <a name="google-play-store"></a>Google Play ストア

GCM からメッセージを受信するには、デバイスに Google Play ストア アプリケーションをインストールする必要があります。 (Google Play のアプリケーションはデバイスにインストールするときに Google Play ストアもインストールため、テスト デバイスに既にインストールされている可能性があります。)Google Play を Android アプリケーションは GCM からのメッセージを受信できません。 デバイスにインストールされている Google Play ストア アプリがあるまだない場合は、次を参照してください。、 [Google Play](https://support.google.com/googleplay) web サイトをダウンロードして Google Play をインストールします。 

または、2.2 以降 (Android エミュレーターに Google Play ストアをインストールする必要はありません) テスト デバイスではなく、Android を実行している Android エミュレーターを使用することができます。 ただし、エミュレーターを使用する場合は、GCM への接続に Wi-fi を使用する必要があり、このチュートリアルの後半で説明したように、Wi-fi、ファイアウォールでいくつかのポートを開く必要があります。 

### <a name="set-the-package-name"></a>パッケージ名を設定します。

[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)、GCM 対応アプリのパッケージ名を指定します (このパッケージ名としても機能、*アプリケーション ID*当社の API キーと Sender ID に関連付けられている)。 プロパティを開いてみましょう、 **ClientApp**プロジェクトし、パッケージ名をこの文字列に設定します。 この例にパッケージ名を設定します`com.xamarin.gcmexample`:

[![パッケージ名を設定します。](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

クライアント アプリがこのパッケージ名でない場合は、GCM から登録トークンを受信することがありますに注意してください。*まったく*パッケージ名、Google Developer console に入力と一致します。 

### <a name="add-permissions-to-the-android-manifest"></a>Android マニフェストへのアクセス許可を追加します。

Android アプリケーションでは、Google Cloud Messaging から通知を受信できる前に構成されている次のアクセス許可が必要です。 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; アプリを登録し、Google Cloud Messaging からメッセージを受信するアクセス許可を付与します。 (何`c2dm`という意味ですか? これの略_クラウド デバイス メッセージング_、これは、GCM、現時点で非推奨の先行します。 
    GCM を使用して引き続き`c2dm`そのアクセス許可文字列の多くでします)。 

-   `android.permission.WAKE_LOCK` &ndash; (省略可能)デバイスからの CPU をできないように、メッセージのリッスン中にスリープ状態にします。 

-   `android.permission.INTERNET` &ndash; クライアント アプリケーションが GCM と通信できるように、インターネットへのアクセスを許可します。 

-   *package_name* `.permission.C2D_MESSAGE` &ndash; Android アプリケーションを登録し、排他的すべて C2D を受信する権限を要求 (クラウド デバイスから) メッセージ。 *Package_name*プレフィックスは、アプリケーション ID と同じです。 

Android マニフェストで、これらのアクセス許可を設定します。 編集しましょう**AndroidManifest.xml**内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

上記の xml では、次のように変更します。 *YOUR_PACKAGE_NAME*クライアント アプリ プロジェクトのパッケージ名にします。 たとえば、`com.xamarin.gcmexample` のようにします。 

### <a name="check-for-google-play-services"></a>Google play 開発者サービスのチェック

このチュートリアルでは、1 つベア ボーン アプリを作り出しています`TextView`UI にします。 このアプリは、GCM との対話を直接示されません。 表示する出力ウィンドウを監視代わりに、方法、アプリのハンドシェイク、gcm と到着すると、新しい通知の通知トレイをチェックします。 

最初に、メッセージ領域のレイアウトを作成しましょう。 編集**Resources.layout.Main.axml**内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

保存**Main.axml**して閉じます。

クライアント アプリの起動時に GCM に連絡する前に Google play 開発者サービスが利用できることを確認します。 編集**MainActivity.cs**と置換、``count``インスタンスの次の変数宣言と変数宣言のインスタンスします。 

```csharp
TextView msgText;
```

次に、次のメソッドを追加、 **MainActivity**クラス。 

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "Sorry, this device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

このコードは、Google Play Services APK がインストールされているかどうかにデバイスを確認します。 インストールされていない場合は、APK を Google Play ストアからダウンロードする (またはデバイスのシステム設定で有効にする) をユーザーに指示するメッセージ領域で、メッセージが表示されます。 クライアント アプリの起動時に、このチェックを実行するため、このメソッドの呼び出しの末尾に追加します`OnCreate`します。 


次に、置換、`OnCreate`メソッドを次のコード。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

このコードでは、Google Play Services APK の存在をチェックし、メッセージ領域に結果を書き込みます。 

みましょう完全に再構築し、アプリを実行します。 次のスクリーン ショットのような画面を表示する必要があります。 

[![Google play 開発者サービスが使用可能です](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

この結果を得られない場合は、Google Play Services APK がデバイスにインストールされていることを確認、 **Xamarin Google play 開発者サービス - GCM**パッケージに追加、 **ClientApp**説明したようにプロジェクト先ほどの。 ビルド エラーが発生した場合、ソリューションをクリーニングし、プロジェクトをビルドしてもう一度お試しください。 

次に、GCM を接続し、登録トークンを取得するコードを記述します。

### <a name="register-with-gcm"></a>Register with GCM

アプリは、アプリ サーバーからリモート通知を受け取ることができます、前に、GCM に登録し、登録トークンを取得する必要があります。 GCM で、アプリケーションの登録作業は、`IntentService`を作成します。 この`IntentService`は、次の手順を実行します。 

1.  使用して、 [InstanceID](https://developers.google.com/instance-id/)アプリ サーバーにアクセスするには、このクライアント アプリを認証するセキュリティ トークンを生成する API。 代わりに、返されます、登録トークン GCM から。

2.  (アプリ サーバーを使用することが必要です) の場合は、アプリ サーバーに登録トークンを転送します。

3.  1 つまたは複数の通知のトピックでチャネルをサブスクライブします。

実装後`IntentService`をテストするかどうかが返されます、登録トークン GCM を参照してください。

という新しいファイルを追加**RegistrationIntentService.cs**次のテンプレート コードを置き換えます。


```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

上記のサンプル コードでは、次のように変更します。 *YOUR_SENDER_ID*に、クライアント アプリ プロジェクトの送信者の ID 番号。 プロジェクトの送信者 ID の取得。 

1.  ログイン、 [Google Cloud Console](https://console.cloud.google.com/)プルダウン メニューからプロジェクトの名前を選択します。 **プロジェクト情報**プロジェクトに表示されているウィンドウのをクリックして**プロジェクトの設定に移動**:

    [![XamarinGCM プロジェクトを選択します。](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  **設定** ページで、検索、**プロジェクト番号**&ndash;これは、プロジェクトの送信者 ID です。

    [![プロジェクト数が表示されます。](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

開始する、`RegistrationIntentService`アプリが実行を開始時にします。 編集**MainActivity.cs**および変更、`OnCreate`メソッドように、`RegistrationIntentService`が開始した後、Google play 開発者サービスの存在を確認します。 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

ここでの各セクションを見ていきましょう`RegistrationIntentService`しくみを理解します。 

最初に、私たちに注釈を付ける、`RegistrationIntentService`システムによってインスタンス化されませんが、サービスがあることを示すには、次の属性で。 

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService`コンス トラクターは、ワーカー スレッドを名前*RegistrationIntentService*デバッグが簡単にします。 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

コア機能`RegistrationIntentService`に存在する、`OnHandleIntent`メソッド。 GCM をアプリに登録する方法を確認するには、このコードを見てみましょう。


##### <a name="request-a-registration-token"></a>登録トークンを要求します。

`OnHandleIntent` Google のまず呼び出し[InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) GCM から登録トークンを要求するメソッド。 このコードでラップ、`lock`複数登録インテントが同時に発生する可能性を防ぐ&ndash;、`lock`これらのインテントが順番に処理されます。 登録トークンの取得に失敗した場合は、例外がスローされ、エラーがログに記録します。 登録が成功すると、 `token` GCM から返された登録トークンに設定されます。 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

##### <a name="forward-the-registration-token-to-the-app-server"></a>登録トークンをアプリ サーバーに転送します。

登録トークンを取得する場合 (つまり、例外がスローされなかった) を呼び出して`SendRegistrationToAppServer`に関連付けるユーザーの登録サーバー側のアカウント (ある場合) を使用してトークンに保持される、アプリケーションでします。 この実装は、アプリ サーバーの設計に依存するため、ここで、空のメソッドが提供されます。 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

場合によっては、アプリ サーバー必要はありません、ユーザーの登録トークンです。その場合は、このメソッドは省略できます。 登録トークンは、アプリ サーバーに送信されるときに`SendRegistrationToAppServer`トークンがサーバーに送信されたかどうかを示すブール値を維持する必要があります。 このブール値が false の場合`SendRegistrationToAppServer`トークンをアプリ サーバーから送信する&ndash;それ以外の場合、トークンは、前の呼び出しでアプリのサーバーに送信されて既にします。 


##### <a name="subscribe-to-the-notification-topic"></a>通知トピックをサブスクライブします。

次に、呼び出して、`Subscribe`メソッドを示すために GCM 通知トピックをサブスクライブすることにします。 `Subscribe`を呼び出して、 [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) API、クライアント アプリの下ですべてのメッセージをサブスクライブする`/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

アプリ サーバーは、通知メッセージを送信する必要があります`/topics/global`当社はそれらを受信する場合。 名前をトピックの「注`/topics`アプリ サーバーとクライアント アプリは、これら名前に同意する限り何もすることができます。 (ここでは、名前を選択して`global`をアプリ サーバーでサポートされているすべてのトピックにメッセージを受信することを示します)。 

サーバー側でメッセージング GCM トピックについては、Google を参照してください。[トピックにメッセージを送信](https://developers.google.com/cloud-messaging/topic-messaging)します。 


#### <a name="implement-an-instance-id-listener-service"></a>インスタンス ID リスナー サービスを実装します。

登録トークンを一意かつセキュリティで保護します。ただし、クライアント アプリ (または GCM) は、アプリの再インストールまたはセキュリティ上の問題が発生した場合、登録トークンを更新する必要があります。 このため、実装する必要があります、 `InstanceIdListenerService` GCM からのトークンの更新要求に応答します。 

という新しいファイルを追加**InstanceIdListenerService.cs**次のテンプレート コードを置き換えます。 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

注釈を付ける`InstanceIdListenerService`をサービスが、システムによってインスタンス化するのには、GCM 登録トークンを受信できることを示すために次の属性を持つ (とも呼ばれる*インスタンス ID*) 更新の要求。 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

`OnTokenRefresh`メソッドで、サービスを開始、`RegistrationIntentService`新しい登録トークンをインターセプトできるようにします。


#### <a name="test-registration-with-gcm"></a>GCM 登録をテストします。

みましょう完全に再構築し、アプリを実行します。 GCM から登録トークンを正常に受信した場合、登録トークンが、出力ウィンドウに表示されます。 例えば: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>ダウン ストリーム メッセージを処理します。 

これまでが実装されましたコードは、のみの「セットアップ」のコードです。これは、Google play 開発者サービスがインストールされているし、リモート通知を受信するため、クライアント アプリを準備するには、GCM とアプリ サーバーとネゴシエートしかどうかを確認します。 ただし、あるまだ実際に受信して、ダウン ストリームの通知メッセージを処理するコードを実装します。 これを行うには、実装する必要があります、 *GCM リスナー サービス*します。 このサービスは、アプリ サーバーからトピックのメッセージを受信し、ローカル通知としてそれらをブロードキャストします。 このサービスを実装しました GCM の場合は、実装が正しく動作を確認できるようにメッセージを送信するテスト プログラムを作成します。 


#### <a name="add-a-notification-icon"></a>通知アイコンを追加します。

まず、通知を起動するときに、通知領域に表示される小さいアイコンを追加してみましょう。 コピーすることができます[このアイコン](remote-notifications-with-gcm-images/ic-stat-ic-notification.png)をプロジェクトにするか、独自のカスタム アイコンを作成します。 アイコン ファイルという名前**ic_stat_button_click.png**にコピーし、**リソース/drawable**フォルダー。 使用してください**追加 > 既存の項目.** にこのアイコン ファイルをプロジェクトに含めます。


#### <a name="implement-a-gcm-listener-service"></a>GCM のリスナー サービスを実装します。

という新しいファイルを追加**GcmListenerService.cs**次のテンプレート コードを置き換えます。

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

各セクションを見ていきましょう、`GcmListenerService`しくみを理解します。 

最初に、私たちに注釈を付ける`GcmListenerService`をこのサービスは、システムによってインスタンス化するのには、GCM メッセージを受信するかを示す、インテント フィルターを含めることを示すために属性を使用します。 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

ときに`GcmListenerService`、GCM からメッセージを受信、`OnMessageReceived`メソッドが呼び出されます。 このメソッドから渡されるでメッセージの内容を抽出する`Bundle`、(そのため、出力ウィンドウに表示できます) のメッセージの内容をログに記録し、呼び出し`SendNotification`ローカル通知を受信したメッセージの内容を起動します。 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification`メソッドは`Notification.Builder`使用するには、通知し、作成、`NotificationManager`を通知を起動します。 実際には、ユーザーに提示するローカル通知に、リモート通知メッセージを変換します。
使用しての詳細については`Notification.Builder`と`NotificationManager`を参照してください[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)します。


#### <a name="declare-the-receiver-in-the-manifest"></a>マニフェストに、受信側を宣言します。

GCM からメッセージを受信する前に、Android マニフェストで GCM リスナー宣言する必要があります。 編集しましょう**AndroidManifest.xml**と置換、`<application>`を次の XML セクション。 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

上記の xml では、次のように変更します。 *YOUR_PACKAGE_NAME*クライアント アプリ プロジェクトのパッケージ名にします。 このチュートリアルの例ではパッケージ名は`com.xamarin.gcmexample`します。 

この XML で、各設定の動作を見てみましょう。

|設定|説明|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|アプリがキャプチャし、プッシュ通知の受信のメッセージを処理する GCM 受信者を実装することを宣言します。|
|`com.google.android.c2dm.permission.SEND`|GCM サーバーのみが、アプリに直接メッセージを送信できますを宣言します。|
|`com.google.android.c2dm.intent.RECEIVE`|インテント フィルターは、アプリが GCM からブロードキャスト メッセージを処理することをアドバタイズします。|
|`com.google.android.c2dm.intent.REGISTRATION`|広告、アプリが新しい登録インテントを処理するインテント フィルター (つまり、インスタンス ID リスナー サービスを実装しますが)。|

修飾する代わりに、 `GcmListenerService` XML; で指定するのではなく、これらの属性をここで指定しますに**AndroidManifest.xml**コード サンプルがわかりやすくなるようにします。 


### <a name="create-a-message-sender-to-test-the-app"></a>メッセージの送信者が、アプリをテストの作成します。

追加、C#デスクトップ コンソール アプリケーションをソリューションにプロジェクトし、それを呼び出す**MessageSender**します。 このコンソール アプリケーションを使用して、アプリケーション サーバーをシミュレート&ndash;ことが通知メッセージを送信**ClientApp** GCM を使用しました。 


#### <a name="add-the-jsonnet-package"></a>Json.NET パッケージを追加します。

このコンソール アプリの開発中、クライアント アプリに送信する通知メッセージを含む JSON ペイロードの。 使用して、 **Json.NET**にパッケージ化**MessageSender** GCM で必要な JSON オブジェクトを構築するが容易にします。 Visual Studio で、右クリックして**参照 > NuGet パッケージの管理.**; Visual studio for Mac を右クリックして**パッケージ > パッケージを追加しています.**. 

探してみましょう、 **Json.NET**パッケージ化し、プロジェクトにインストールします。 

[![Json.NET パッケージをインストールします。](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>System.Net.Http への参照を追加します。

参照を追加する必要がありますも`System.Net.Http`インスタンス化できるように、 `HttpClient` GCM にテスト メッセージを送信するためです。 **MessageSender**プロジェクトを右クリックして**参照 > 参照の追加**が表示されるまで下にスクロールし、 **System.Net.Http**します。 横にチェック マークを付けます**System.Net.Http**クリック**OK**します。 


#### <a name="implement-code-that-sends-a-test-message"></a>テスト メッセージを送信するコードを実装します。

**MessageSender**、編集**Program.cs**内容を次のコードに置き換えます。

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

上記のコードでは、次のように変更します。 *YOUR_API_KEY*クライアント アプリ プロジェクトの API キーにします。 

このテスト アプリのサーバーでは、GCM に次の JSON 形式のメッセージを送信します。

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM、さらに、クライアント アプリにこのメッセージを転送します。 ビルドしてみましょう**MessageSender**コマンドラインから実行しました、コンソール ウィンドウを開きます。



### <a name="try-it"></a>手順を次に示します。

これで、クライアント アプリをテストする準備ができました。 エミュレーターを使用しているかどうか、または場合は、デバイスが Wi-fi 経由で GCM との通信、GCM メッセージ取得するためにファイアウォールで、次の TCP ポートを開く必要があります: 5228、5229、および 5230 します。

クライアント アプリを起動し、出力ウィンドウをご覧ください。 後に、`RegistrationIntentService`登録が正常に受信 GCM からトークンを出力ウィンドウに表示する、トークンで、次のようなログ出力。

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

この時点でクライアント アプリがリモート通知メッセージを受信する準備ができています。 コマンドラインから実行、 **MessageSender.exe**クライアント アプリに「こんにちは, Xamarin」の通知メッセージを送信するプログラム。
まだビルドしない場合は、 **MessageSender**プロジェクトで、ようになりました。

実行する**MessageSender.exe** Visual Studio は、コマンド プロンプトを開き、変更、 **MessageSender/Bin/debug**ディレクトリ、および直接コマンドを実行します。

```cmd
MessageSender.exe
```

実行する**MessageSender.exe** Visual Studio for Mac でターミナル セッションを開き、変更**MessageSender/Bin/debug**ディレクトリ、および実行に使用して mono **MessageSender.exe** 

```bash
mono MessageSender.exe
```

GCM とクライアント アプリに反映されるまでに、メッセージの 1 分かかる場合があります。 メッセージが正常に受信した場合に、出力ウィンドウに次のような出力が表示されます。 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

さらに、通知トレイで新しい通知アイコンが表示されているかを確認する必要があります。 

[![デバイスで Notiication アイコンが表示されます。](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

通知を表示する通知トレイを開くと、リモート通知が表示されます。

[![通知メッセージが表示されます。](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

これで、アプリがその最初のリモート通知を受信しました。

アプリが強制的に停止した場合 GCM メッセージを受信する不要になったことに注意してください。 通知は、強制停止後再開、アプリは、手動再起動する必要があります。 この Android ポリシーの詳細については、次を参照してください。[停止状態のアプリケーション上のコントロールを起動](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)と[スタック オーバーフロー post](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267)します。 

 
## <a name="summary"></a>まとめ

このチュートリアルでは、Xamarin.Android アプリケーションでリモート通知を実装するための手順が詳しく説明します。 GCM の通信に必要な追加のパッケージをインストールする方法が説明されているし、GCM サーバーにアクセスするアプリのアクセス許可を構成する方法について説明しました。 これを Google play 開発者サービスの有無を確認する方法、目的のサービスを登録および登録トークン、GCM とネゴシエートし、インスタンス ID リスナー サービスを実装する方法、および GCM のリスナーを実装する方法を示したサンプル コードが用意されています受信してリモート通知メッセージを処理するサービスです。 最後に、GCM から、クライアント アプリにテスト通知を送信するコマンド ライン テスト プログラムを実装します。 


## <a name="related-links"></a>関連リンク

- [GCM RemoteNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
