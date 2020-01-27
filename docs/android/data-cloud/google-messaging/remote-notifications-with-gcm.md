---
title: Google Cloud Messaging を使用したリモート通知
description: このチュートリアルでは、Google Cloud Messaging を使用して、Xamarin アプリケーションでリモート通知 (プッシュ通知とも呼ばれます) を実装する方法について、順を追って説明します。 ここでは、Google Cloud Messaging (GCM) との通信に実装する必要があるさまざまなクラスについて説明します。 GCM にアクセスするために Android マニフェストにアクセス許可を設定する方法について説明し、サンプルテストプログラムを使用したエンドツーエンドのメッセージングを示します。
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/02/2019
ms.openlocfilehash: b621d61584cc39f669c662db3d1df264d9a92eb9
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723773"
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google Cloud Messaging を使用したリモート通知

> [!WARNING]
> Google 非推奨 GCM (2018 年4月10日時点)。 次のドキュメントとサンプルプロジェクトは、管理されなくなる可能性があります。 Google の GCM サーバーとクライアント Api は、2019年5月29日の時点で削除される予定です。 Google は、GCM アプリを焼討 Base Cloud Messaging (FCM) に移行することを推奨しています。 GCM の廃止と移行の詳細については、「 [Google Cloud Messaging-非推奨](https://developers.google.com/cloud-messaging/)」を参照してください。
>
> Xamarin での焼討 Base Cloud Messaging を使用したリモート通知の使用を開始するには、「 [FCM を使用したリモート通知](remote-notifications-with-fcm.md)」を参照してください。

_このチュートリアルでは、Google Cloud Messaging を使用して、Xamarin アプリケーションでリモート通知 (プッシュ通知とも呼ばれます) を実装する方法について、順を追って説明します。ここでは、Google Cloud Messaging (GCM) との通信に実装する必要があるさまざまなクラスについて説明します。 GCM にアクセスするために Android マニフェストにアクセス許可を設定する方法について説明し、サンプルテストプログラムを使用したエンドツーエンドのメッセージングを示します。_

## <a name="gcm-notifications-overview"></a>GCM 通知の概要

このチュートリアルでは、Google Cloud Messaging (GCM) を使用してリモート通知 (*プッシュ通知*とも呼ばれます) を実装する Xamarin Android アプリケーションを作成します。 リモートメッセージングに GCM を使用するさまざまなインテントサービスとリスナーサービスを実装し、アプリケーションサーバーをシミュレートするコマンドラインプログラムを使用して実装をテストします。

このチュートリアルを続行するには、Google の GCM サーバーを使用するために必要な資格情報を取得する必要があります。このプロセスについては、 [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)をご説明します。
特に、このチュートリアルで示されているコード例に挿入するには、 *API キー*と*送信者 ID*が必要です。

GCM 対応の Xamarin Android クライアントアプリを作成するには、次の手順を実行します。

1. GCM サーバーとの通信に必要な追加のパッケージをインストールします。
2. GCM サーバーにアクセスするためのアプリのアクセス許可を構成します。
3. Google Play 開発者サービスの存在を確認するコードを実装します。
4. 登録トークンに対して GCM とネゴシエートする登録インテントサービスを実装します。
5. GCM からの登録トークンの更新をリッスンするインスタンス ID リスナーサービスを実装します。
6. GCM を介してアプリサーバーからリモートメッセージを受信する GCM リスナーサービスを実装します。

このアプリでは、*トピックメッセージング*と呼ばれる新しい GCM 機能を使用します。 トピック「メッセージング」では、アプリサーバーは個々のデバイスの一覧ではなく、トピックにメッセージを送信します。 そのトピックにサブスクライブしているデバイスは、プッシュ通知としてトピックメッセージを受信できます。

クライアントアプリの準備ができたら、GCM 経由でクライアントアプリにC#プッシュ通知を送信するコマンドラインアプリケーションを実装します。

## <a name="walkthrough"></a>チュートリアル

まず、 **RemoteNotifications**という名前の新しい空のソリューションを作成しましょう。 次に、 **Android アプリ**テンプレートに基づく新しい android プロジェクトをこのソリューションに追加してみましょう。 この project **ClientApp**を呼び出しましょう。 (Xamarin Android プロジェクトの作成に慣れていない場合は、「 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md)」を参照してください)。**ClientApp**プロジェクトには、GCM を介してリモート通知を受信する Xamarin クライアントアプリケーションのコードが含まれます。

### <a name="add-required-packages"></a>必要なパッケージの追加

クライアントアプリコードを実装する前に、GCM との通信に使用する複数のパッケージをインストールする必要があります。 また、まだインストールされていない場合は、デバイスに Google Play ストアアプリケーションを追加する必要があります。

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin Google Play 開発者サービス GCM パッケージを追加する

Google Cloud Messaging からメッセージを受信するには、 [Google Play 開発者サービス](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/)framework がデバイス上に存在している必要があります。 このフレームワークがないと、Android アプリケーションは GCM サーバーからメッセージを受信できません。 Google Play 開発者サービスは、Android デバイスの電源が入っている間はバックグラウンドで実行され、GCM からのメッセージを静かにリッスンします。 これらのメッセージが到着すると、Google Play 開発者サービスはメッセージをインテントに変換し、登録されているアプリケーションにこれらのインテントをブロードキャストします。

Visual Studio で、[参照] を右クリックし **> [NuGet パッケージの管理**] をクリックします。Visual Studio for Mac で、[パッケージ] を右クリックし **> [パッケージの追加...** ] をクリックします。**Xamarin GOOGLE PLAY 開発者サービス GCM**を検索し、このパッケージを**ClientApp**プロジェクトにインストールします。

[Google Play 開発者サービスのインストール ![](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

**Xamarin Google Play 開発者サービス-GCM**をインストールすると、 **xamarin Google Play 開発者サービス Base**が自動的にインストールされます。 エラーが発生した場合は、プロジェクトの [ *Android の最小ターゲット*] 設定を **[SDK バージョンを使用したコンパイル]** 以外の値に変更し、NuGet のインストールを再試行してください。

次に、 **MainActivity.cs**を編集し、次の `using` ステートメントを追加します。

```csharp
using Android.Gms.Common;
using Android.Util;
```

これにより、Google Play 開発者サービス GMS パッケージ内の型がコードで使用できるようになります。また、GMS でトランザクションを追跡するために使用するログ機能が追加されます。

#### <a name="google-play-store"></a>Google Play ストア

GCM からメッセージを受信するには、Google Play ストアアプリケーションがデバイスにインストールされている必要があります。 (Google Play アプリケーションがデバイスにインストールされている場合、Google Play ストアもインストールされるため、テストデバイスに既にインストールされている可能性があります)。Google Play がないと、Android アプリケーションは GCM からメッセージを受信できません。
デバイスに Google Play ストアアプリをまだインストールしていない場合は、 [Google Play](https://support.google.com/googleplay)の web サイトにアクセスして Google Play をダウンロードしてインストールしてください。

または、テストデバイスではなく、Android 2.2 以降を実行している Android エミュレーターを使用することもできます (Android エミュレーターに Google Play ストアをインストールする必要はありません)。 ただし、エミュレーターを使用する場合は、Wi-fi を使用して GCM に接続する必要があります。また、このチュートリアルの後半で説明するように、Wi-fi ファイアウォールでいくつかのポートを開く必要があります。

### <a name="set-the-package-name"></a>パッケージ名を設定する

[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)では、GCM 対応アプリのパッケージ名を指定しています (このパッケージ名は、API キーと送信者 ID に関連付けられている*アプリケーション ID*としても機能します)。 **ClientApp**プロジェクトのプロパティを開き、パッケージ名をこの文字列に設定してみましょう。 この例では、パッケージ名を `com.xamarin.gcmexample`に設定します。

[パッケージ名の設定 ![](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

このパッケージ名が Google Developer console に入力したパッケージ名と*完全*に一致しない場合、クライアントアプリは GCM から登録トークンを受け取ることができないことに注意してください。

### <a name="add-permissions-to-the-android-manifest"></a>Android マニフェストにアクセス許可を追加する

Android アプリケーションで Google Cloud Messaging から通知を受信するには、次のアクセス許可が構成されている必要があります。

- `com.google.android.c2dm.permission.RECEIVE` &ndash; は、Google Cloud Messaging からメッセージを登録および受信するためのアクセス許可をアプリに付与します。 (`c2dm` はどういう意味ですか。 これは、_クラウドからデバイスへのメッセージング_を意味します。これは、現在非推奨とされている GCM です。
    GCM では、多くのアクセス許可文字列で `c2dm` が引き続き使用されます)。

- `android.permission.WAKE_LOCK` &ndash; (オプション) を使用すると、メッセージをリッスンしている間にデバイスの CPU がスリープ状態になるのを防ぐことができます。

- `android.permission.INTERNET` &ndash; は、クライアントアプリが GCM と通信できるようにインターネットアクセスを許可します。

- *package_name*`.permission.C2D_MESSAGE` &ndash; は、アプリケーションを Android に登録し、すべての C2D (クラウドからデバイス) メッセージを排他的に受信するためのアクセス許可を要求します。 *Package_name*プレフィックスは、アプリケーション ID と同じです。

これらのアクセス許可は、Android マニフェストで設定します。 **Androidmanifest**を編集して、内容を次の xml に置き換えてみましょう。

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

上記の XML では、 *YOUR_PACKAGE_NAME*をクライアントアプリプロジェクトのパッケージ名に変更します。 たとえば、 `com.xamarin.gcmexample`のようにします。

### <a name="check-for-google-play-services"></a>Google Play 開発者サービスの確認

このチュートリアルでは、UI に1つの `TextView` を持つベアボーンアプリを作成します。 このアプリは GCM との対話を直接示すものではありません。 代わりに、[出力] ウィンドウで GCM によるアプリのハンドシェイクを確認し、通知トレイに新しい通知が到着したことを確認します。

まず、メッセージ領域のレイアウトを作成してみましょう。 **.Resources**を編集し、内容を次の xml に置き換えます。

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

**メインの axml**を保存して閉じます。

クライアントアプリが起動したら、GCM への接続を試行する前に Google Play 開発者サービスが使用可能であることを確認します。 **MainActivity.cs**を編集し、``count`` インスタンス変数の宣言を次のインスタンス変数宣言に置き換えます。

```csharp
TextView msgText;
```

次に、次のメソッドを**Mainactivity**クラスに追加します。

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

このコードは、Google Play 開発者サービス APK がインストールされているかどうかをデバイスで確認します。 インストールされていない場合は、メッセージ領域にメッセージが表示され、ユーザーは APK を Google Play ストアからダウンロードするように指示します (または、デバイスのシステム設定で有効にすることができます)。 クライアントアプリの起動時にこのチェックを実行するため、`OnCreate`の最後にこのメソッドの呼び出しを追加します。

次に、`OnCreate` メソッドを次のコードに置き換えます。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

このコードは Google Play 開発者サービス APK の存在を確認し、その結果をメッセージ領域に書き込みます。

アプリを完全にリビルドして実行してみましょう。 次のスクリーンショットのような画面が表示されます。

[![Google Play 開発者サービスを使用できます](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

この結果が得られない場合は、Google Play 開発者サービス APK がデバイスにインストールされていることと、前に説明したように**Xamarin Google Play 開発者サービス-GCM**パッケージが**ClientApp**プロジェクトに追加されていることを確認してください。 ビルドエラーが発生した場合は、ソリューションをクリーンアップし、プロジェクトを再度ビルドしてみてください。

次に、GCM に接続して登録トークンを取得するコードを記述します。

### <a name="register-with-gcm"></a>GCM に登録する

アプリはアプリサーバーからリモート通知を受信する前に、GCM に登録し、登録トークンを取得する必要があります。 GCM にアプリケーションを登録する作業は、作成する `IntentService` によって処理されます。 `IntentService` では、次の手順を実行します。

1. では、 [InstanceID](https://developers.google.com/instance-id/) API を使用して、クライアントアプリがアプリサーバーにアクセスすることを承認するセキュリティトークンを生成します。 返されると、GCM から登録トークンが返されます。

2. アプリサーバーに登録トークンを転送します (アプリサーバーで必要な場合)。

3. 1つ以上の通知トピックチャネルをサブスクライブします。

この `IntentService`を実装した後、GCM から登録トークンを取得したかどうかをテストします。

**RegistrationIntentService.cs**という名前の新しいファイルを追加し、テンプレートコードを次のコードに置き換えます。

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

上記のサンプルコードでは、 *YOUR_SENDER_ID*をクライアントアプリプロジェクトの送信者 ID 番号に変更します。 プロジェクトの送信者 ID を取得するには、次のようにします。

1. [Google Cloud Console](https://console.cloud.google.com/)にログインし、プルダウンメニューからプロジェクト名を選択します。 プロジェクトに対して表示される **[プロジェクト情報]** ウィンドウで、 **[プロジェクト設定へのジャンプ]** をクリックします。

    [XamarinGCM プロジェクトの選択 ![](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2. **[設定]** ページで、プロジェクトの送信者 ID &ndash;**プロジェクト番号**を見つけます。

    [表示されるプロジェクト番号 ![](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

アプリの実行が開始されたら、`RegistrationIntentService` を開始します。 **MainActivity.cs**を編集し、Google Play 開発者サービスの存在を確認した後に `RegistrationIntentService` が開始されるように `OnCreate` メソッドを変更します。

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

次に、`RegistrationIntentService` の各セクションを見て、そのしくみを理解してみましょう。

まず、次の属性を使用して `RegistrationIntentService` に注釈を付け、サービスがシステムによってインスタンス化されないことを示します。

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService` コンストラクターは、デバッグを容易*にするため*に、ワーカースレッドに名前を設定します。

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

`RegistrationIntentService` のコア機能は、`OnHandleIntent` メソッドに存在します。 このコードを使用して、アプリケーションを GCM に登録する方法を見てみましょう。

#### <a name="request-a-registration-token"></a>登録トークンを要求する

`OnHandleIntent` は、まず、GCM から登録トークンを要求するために、Google の[InstanceID. GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;)メソッドを呼び出します。 このコードを `lock` にラップすることで、複数の登録インテントが同時に発生する可能性を防ぐことができます。 `lock` &ndash;、これらのインテントを順番に処理することが保証されます。 登録トークンの取得に失敗した場合は、例外がスローされ、エラーがログに記録されます。 登録が成功した場合、`token` は GCM から返された登録トークンに設定されます。

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

#### <a name="forward-the-registration-token-to-the-app-server"></a>アプリサーバーに登録トークンを転送する

登録トークンを取得した場合 (つまり、例外がスローされなかった場合)、`SendRegistrationToAppServer` を呼び出して、アプリケーションによって管理されているサーバー側アカウント (存在する場合) にユーザーの登録トークンを関連付けます。 この実装はアプリサーバーの設計に依存しているため、空のメソッドを次に示します。

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

場合によっては、アプリサーバーにユーザーの登録トークンは必要ありません。この場合、このメソッドは省略できます。 登録トークンがアプリサーバーに送信されると、`SendRegistrationToAppServer` は、トークンがサーバーに送信されたかどうかを示すブール値を保持する必要があります。 このブール値が false の場合、`SendRegistrationToAppServer` によってアプリ &ndash; サーバーにトークンが送信されます。それ以外の場合は、前の呼び出しでトークンがアプリサーバーに既に送信されています。

#### <a name="subscribe-to-the-notification-topic"></a>通知トピックを購読する

次に、`Subscribe` メソッドを呼び出して、通知トピックをサブスクライブする GCM を指定します。 `Subscribe`では、GcmPubSub Subscribe API を呼び出して、`/topics/global`下のすべてのメッセージに対してクライアントアプリをサブスクライブします。

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

アプリサーバーは、受信する場合は、`/topics/global` に通知メッセージを送信する必要があります。 アプリサーバーとクライアントアプリがこれらの名前に同意する限り、`/topics` の下のトピック名は任意のものにすることができます。 (ここでは、`global` 名前を選択して、アプリサーバーでサポートされているすべてのトピックでメッセージを受信することを示しています)。

#### <a name="implement-an-instance-id-listener-service"></a>インスタンス ID リスナーサービスを実装する

登録トークンは一意であり、セキュリティで保護されています。ただし、アプリの再インストールやセキュリティの問題が発生した場合は、クライアントアプリ (GCM) で登録トークンの更新が必要になることがあります。 このため、GCM からのトークン更新要求に応答する `InstanceIdListenerService` を実装する必要があります。

**InstanceIdListenerService.cs**という名前の新しいファイルを追加し、テンプレートコードを次のコードに置き換えます。

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

次の属性を使用して `InstanceIdListenerService` に注釈を設定して、サービスがシステムによってインスタンス化されず、GCM 登録トークン (*インスタンス ID*とも呼ばれます) の更新要求を受け取ることができることを示します。

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

サービスの `OnTokenRefresh` メソッドは、新しい登録トークンをインターセプトできるように、`RegistrationIntentService` を開始します。

#### <a name="test-registration-with-gcm"></a>GCM を使用した登録のテスト

アプリを完全にリビルドして実行してみましょう。 GCM から登録トークンを正常に受信した場合は、[出力] ウィンドウに登録トークンが表示されます。 例:

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>ダウンストリームメッセージの処理

これまでに実装したコードは、"セットアップ" コードのみです。Google Play 開発者サービスがインストールされているかどうかを確認し、GCM およびアプリサーバーとネゴシエートして、リモート通知を受信するクライアントアプリを準備します。 ただし、下流の通知メッセージを実際に受信して処理するコードはまだ実装していません。 これを行うには、 *GCM リスナーサービス*を実装する必要があります。 このサービスは、アプリサーバーからトピックメッセージを受信し、ローカルで通知としてブロードキャストします。 このサービスを実装した後、GCM にメッセージを送信するテストプログラムを作成し、実装が正しく機能するかどうかを確認できるようにします。

#### <a name="add-a-notification-icon"></a>通知アイコンの追加

まず、通知が開始されたときに通知領域に表示される小さいアイコンを追加してみましょう。 [このアイコン](remote-notifications-with-gcm-images/ic-stat-ic-notification.png)をプロジェクトにコピーするか、独自のカスタムアイコンを作成することができます。 このアイコンファイルには**ic_stat_button_click**という名前を指定し、 **Resources/アブル**フォルダーにコピーします。 このアイコンファイルをプロジェクトに含めるには、 **[既存の項目の追加 >]** を忘れずに使用してください。

#### <a name="implement-a-gcm-listener-service"></a>GCM リスナーサービスを実装する

**GcmListenerService.cs**という名前の新しいファイルを追加し、テンプレートコードを次のコードに置き換えます。

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

`GcmListenerService` の各セクションを見て、そのしくみを理解してみましょう。

まず、属性を使用して `GcmListenerService` に注釈を設定し、このサービスがシステムによってインスタンス化されないことを示します。また、目的のフィルターを含めて、GCM メッセージを受信することを指定します。

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

`GcmListenerService` が GCM からメッセージを受信すると、`OnMessageReceived` メソッドが呼び出されます。 このメソッドは、渡された `Bundle`からメッセージの内容を抽出し、メッセージの内容をログに記録し (出力ウィンドウで表示できるように)、`SendNotification` を呼び出して、受信したメッセージの内容を含むローカル通知を開始します。

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` 方法では、`Notification.Builder` を使用して通知を作成した後、`NotificationManager` を使用して通知を開始します。 実質的には、リモート通知メッセージがローカル通知に変換され、ユーザーに提示されます。
`Notification.Builder` と `NotificationManager`の使用方法の詳細については、「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」を参照してください。

#### <a name="declare-the-receiver-in-the-manifest"></a>マニフェストで受信側を宣言する

GCM からメッセージを受信するには、その前に、Android マニフェストで GCM リスナーを宣言する必要があります。 **Androidmanifest**を編集し、`<application>` セクションを次の xml に置き換えます。

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

上記の XML では、 *YOUR_PACKAGE_NAME*をクライアントアプリプロジェクトのパッケージ名に変更します。 このチュートリアルの例では、パッケージ名は `com.xamarin.gcmexample`です。

この XML の各設定について、次のように説明します。

|設定|説明|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|受信したプッシュ通知メッセージをキャプチャして処理する GCM レシーバーをアプリが実装することを宣言します。|
|`com.google.android.c2dm.permission.SEND`|GCM サーバーだけがアプリにメッセージを直接送信できることを宣言します。|
|`com.google.android.c2dm.intent.RECEIVE`|アプリが GCM からのブロードキャストメッセージを処理することをアドバタイズするインテントフィルター。|
|`com.google.android.c2dm.intent.REGISTRATION`|アプリが新しい登録インテントを処理することをアドバタイズするインテントフィルター (つまり、インスタンス ID リスナーサービスを実装していること)。|

また、XML で指定するのではなく、これらの属性を使用して `GcmListenerService` を装飾することもできます。ここでは、コードサンプルがより簡単になるように、 **Androidmanifest .xml**でそれらを指定します。

### <a name="create-a-message-sender-to-test-the-app"></a>アプリケーションをテストするためのメッセージ送信者を作成する

ここでは、 C#デスクトップコンソールアプリケーションプロジェクトをソリューションに追加し、 **MessageSender**というファイルを呼び出します。 このコンソールアプリケーションを使用して、アプリケーションサーバー &ndash; シミュレートします。これにより、GCM 経由で**ClientApp**に通知メッセージが送信されます。

#### <a name="add-the-jsonnet-package"></a>Json.NET パッケージを追加する

このコンソールアプリでは、クライアントアプリに送信する通知メッセージを含む JSON ペイロードを作成しています。 **MessageSender**の**Json.NET**パッケージを使用して、GCM で必要な Json オブジェクトを簡単に作成できるようにします。 Visual Studio で、[参照] を右クリックし **> [NuGet パッケージの管理**] をクリックします。Visual Studio for Mac で、[パッケージ] を右クリックし **> [パッケージの追加...** ] をクリックします。

**Json.NET**パッケージを検索して、プロジェクトにインストールしてみましょう。

[Json.NET パッケージのインストール ![](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)

#### <a name="add-a-reference-to-systemnethttp"></a>System .Net. Http への参照を追加します。

また、テストメッセージを GCM に送信するための `HttpClient` をインスタンス化できるように、`System.Net.Http` への参照を追加する必要もあります。 MessageSender プロジェクトで、参照 を右クリックし、**参照の追加 >** をクリックして、  が表示されるまで下へ**スクロールします**。 **[System .net. Http]** の横にチェックマークを付け、[ **OK]** をクリックします。

#### <a name="implement-code-that-sends-a-test-message"></a>テストメッセージを送信するコードを実装する

**MessageSender**で、 **Program.cs**を編集し、内容を次のコードに置き換えます。

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

上記のコードでは、 *YOUR_API_KEY*をクライアントアプリプロジェクトの API キーに変更します。

このテストアプリサーバーは、次の JSON 形式のメッセージを GCM に送信します。

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM は、このメッセージをクライアントアプリに転送します。 **MessageSender**をビルドし、コンソールウィンドウを開いて、コマンドラインから実行できるようにしましょう。

### <a name="try-it"></a>試してみる

これで、クライアントアプリをテストする準備ができました。 エミュレーターを使用している場合、またはデバイスが Wi-fi 経由で GCM と通信している場合は、GCM メッセージを取得するために、ファイアウォールで次の TCP ポートを開く必要があります: 5228、5229、および5230。

クライアントアプリを起動し、[出力] ウィンドウを確認します。 `RegistrationIntentService` が GCM から登録トークンを正常に受信した後、出力ウィンドウには、次のようなログ出力を含むトークンが表示されます。

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

この時点で、クライアントアプリはリモート通知メッセージを受信する準備ができています。 コマンドラインから**MessageSender**プログラムを実行して、クライアントアプリに "Hello, Xamarin" 通知メッセージを送信します。
**MessageSender**プロジェクトをまだビルドしていない場合は、ここで作成します。

Visual Studio で**MessageSender**を実行するには、コマンドプロンプトを開き、 **MessageSender/bin/Debug**ディレクトリに移動して、コマンドを直接実行します。

```cmd
MessageSender.exe
```

Visual Studio for Mac で**MessageSender**を実行するには、ターミナルセッションを開き、 **MessageSender/bin/Debug**ディレクトリに変更し、mono を使用して MessageSender を実行し**ます。**

```bash
mono MessageSender.exe
```

メッセージが GCM に反映され、クライアントアプリに戻されるまでに最大で1分かかる場合があります。 メッセージが正常に受信されると、出力ウィンドウに次のような出力が表示されます。

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

さらに、通知トレイに新しい通知アイコンが表示されていることがわかります。

[デバイスに ![通知アイコンが表示される](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

通知トレイを開いて通知を表示すると、リモート通知が表示されます。

[![通知メッセージが表示されます](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

これで、アプリは最初のリモート通知を受信しました。

アプリが強制的に停止された場合、GCM メッセージは受信されなくなることに注意してください。 強制停止後に通知を再開するには、アプリを手動で再起動する必要があります。 この Android ポリシーの詳細については、「停止した[アプリケーションでのコントロールの起動](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)」および「この[スタックオーバーフローの投稿](https://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267)」を参照してください。

## <a name="summary"></a>要約

このチュートリアルでは、Xamarin Android アプリケーションでリモート通知を実装する手順について詳しく説明します。 GCM 通信に必要な追加のパッケージをインストールする方法について説明し、GCM サーバーにアクセスするためのアプリのアクセス許可を構成する方法について説明しました。
このサンプルコードでは、Google Play 開発者サービスの存在を確認する方法、登録インテントサービスを実装する方法と、登録トークンの GCM とネゴシエートするインスタンス ID リスナーサービスを実装する方法、および GCM リスナーを実装する方法を示しています。リモート通知メッセージを受信して処理するサービスです。 最後に、GCM を介してクライアントアプリにテスト通知を送信するコマンドラインテストプログラムを実装しています。

## <a name="related-links"></a>関連リンク

- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
