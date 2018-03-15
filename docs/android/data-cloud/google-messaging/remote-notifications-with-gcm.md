---
title: "Google Cloud Messaging でリモートの通知"
description: "このチュートリアルでは、Xamarin.Android アプリケーションで Google Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれる) を実装する方法の詳細な手順の説明を提供します。 Google Cloud Messaging (GCM) と通信するために実装する必要があるさまざまなクラスについて説明、GCM へのアクセス用の Android のマニフェストにアクセス許可を設定する方法について説明し、サンプル プログラムをテストすると、エンド ツー エンドのメッセージングを示しています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 823fad163e837adab5490446c23ab2f492679114
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google Cloud Messaging でリモートの通知

_このチュートリアルでは、Xamarin.Android アプリケーションで Google Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれる) を実装する方法の詳細な手順の説明を提供します。Google Cloud Messaging (GCM) と通信するために実装する必要があるさまざまなクラスについて説明、GCM へのアクセス用の Android のマニフェストにアクセス許可を設定する方法について説明し、サンプル プログラムをテストすると、エンド ツー エンドのメッセージングを示しています。_

## <a name="gcm-notifications-overview"></a>GCM 通知の概要

このチュートリアルで Google Cloud Messaging (GCM) を使用してリモートの通知を実装する Xamarin.Android アプリケーションを作成します (とも呼ばれる*プッシュ通知*)。 リモート メッセージングの GCM を使用するさまざまな目的およびリスナー サービスを実装し、アプリケーション サーバーをシミュレートするコマンド ライン プログラムによる実装をテストします。 

GCM の新しいバージョンを Firebase クラウド メッセージング (FCM) であることに注意してください&ndash;GCM ではなく FCM を使用して Google が強く推奨します。 GCM を使用している場合は、FCM にアップグレードすることをお勧めします。 FCM の詳細については、次を参照してください。 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)です。 

このチュートリアルを続行するには、Google の GCM サーバーを使用するために必要な資格情報を取得する必要があります。このプロセスは」で説明されて[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)です。 具体的には、必要があります、 *API キー*と*送信者 ID*このチュートリアルで示すサンプル コードに挿入します。 

GCM を有効にした Xamarin.Android クライアント アプリを作成するのには、次の手順を使用します。

1.  GCM サーバーとの通信に必要なその他のパッケージをインストールします。
2.  GCM サーバーにアクセスするアプリのアクセス許可を構成します。
3.  Google Play サービスの存在をチェックするコードを実装します。 
4.  登録トークン、GCM とネゴシエートする登録インテント サービスを実装します。
5.  GCM から登録トークンの更新を待機するインスタンス ID リスナー サービスを実装します。
6.  GCM を使用してアプリ サーバーからリモートのメッセージを受け取る GCM リスナー サービスを実装します。

このアプリが新しい GCM と呼ばれる機能を使用して*トピック メッセージング*です。 トピックは次のメッセージング、アプリ サーバーは、個々 のデバイスの一覧ではなく、トピックにメッセージを送信します。 そのトピックにサブスクライブしているデバイスは、プッシュ通知としてトピック メッセージを受信できます。 GCM トピック メッセージングの詳細については、Google を参照してください。[トピックのメッセージングを実装する](https://developers.google.com/cloud-messaging/topic-messaging)です。 

クライアント アプリの準備ができたら、コマンド ライン c# アプリケーション GCM 経由でクライアント アプリにプッシュ通知を送信することを実装します。 

## <a name="walkthrough"></a>チュートリアル

作業を開始すると呼ばれる新しい空のソリューションを作成しましょう**RemoteNotifications**です。 次に、基になっているこのソリューションに新しい Android プロジェクトを追加してみましょう、 **Android アプリ**テンプレート。 このプロジェクトをとしましょう**ClientApp**です。 (Xamarin.Android プロジェクトを作成するのに慣れていない場合を参照してください[Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)。)。**ClientApp** GCM 経由のリモートの通知を受信する Xamarin.Android クライアント アプリケーションのコードをプロジェクトに含まれます。 

### <a name="add-required-packages"></a>必要なパッケージを追加します。

クライアント アプリ コードを実装できること、前に、GCM との通信に使用するいくつかのパッケージをインストールして必要があります。 またがインストールされていない場合、デバイスに Google Play ストア アプリケーションを追加します。

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin の Google Play サービス GCM パッケージを追加します。

Google Cloud Messaging からメッセージを受信する、 [Google Play サービス](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/)フレームワークは、デバイス上に存在する必要があります。 このフレームワークがない Android アプリケーションは、GCM サーバーからメッセージを受信することはできません。 Google Play サービスは、Android デバイスの電源が投入中 GCM からのメッセージをリッスンしてサイレント モードでに、バック グラウンドで実行されます。 これらのメッセージが到着したとき Google Play サービスは意図的に、メッセージを変換し、それらの登録されているアプリケーションにこのインテントをブロードキャストします。 

Visual Studio を右クリックして**参照 > NuGet パッケージの管理.**; for Mac を Visual Studio で右クリック**パッケージ > パッケージを追加しています.**.検索**Xamarin Google Play サービス - GCM**にこのパッケージをインストールし、 **ClientApp**プロジェクト。 

[![Google Play サービスをインストールします。](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

インストールするときに**Xamarin Google Play サービス - GCM**、 **Xamarin Google Play サービスのベース**は自動的にインストールします。 エラーが発生した場合は、変更、プロジェクトの*ターゲットに最低限の Android*以外の値に設定**SDK のバージョンを使用してコンパイル**と NuGet のインストールをもう一度やり直してください。 

次に、編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Android.Gms.Common;
using Android.Util;
```

これにより、型 Google プレイ サービス GMS パッケージで使用しているコード、および GMS と、トランザクションを追跡するために使用するログ記録機能を追加します。 

#### <a name="google-play-store"></a>Google Play ストア

GCM からメッセージを受信するには、Google Play ストア アプリケーションをデバイスにインストールする必要があります。 (Google Play のアプリケーションがデバイスにインストールされているときに Google Play ストアもインストールため、テスト デバイスに既にインストールされている可能性があります。)Google Play Android アプリケーションは GCM からのメッセージを受信できません。 デバイスにインストールされている、Google Play ストア アプリがない場合は、次を参照してください。、 [Google Play](https://support.google.com/googleplay) web サイトをダウンロードして Google Play をインストールします。 

または、テスト デバイス (Android エミュレーターに Google Play ストアをインストールする必要はありません) ではなく 2.2 以降、Android を実行している Android エミュレーターを使用することができます。 ただし、エミュレーターを使用する場合は、GCM に接続する Wi-fi を使用する必要があり、このチュートリアルの後半で説明したように、Wi-fi ファイアウォールにいくつかのポートを開く必要があります。 

### <a name="set-the-package-name"></a>パッケージ名を設定します。

[Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)、GCM を有効にしたアプリのパッケージ名を指定して (このパッケージの名前としても、*アプリケーション ID*マイクロソフトの API キーと送信者 ID に関連付けられている)。 プロパティを開いてみましょう、 **ClientApp**プロジェクトし、パッケージ名をこの文字列に設定します。 この例では、パッケージ名に設定`com.xamarin.gcmexample`:

[![パッケージ名を設定します。](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

クライアント アプリがこのパッケージ名がない場合は、GCM から登録トークンを受信することはできませんに注意してください*まったく*Google 開発者コンソールに入力して、パッケージ名と一致します。 

### <a name="add-permissions-to-the-android-manifest"></a>Android マニフェストへのアクセス許可を追加します。

Android アプリケーションでは、Google Cloud Messaging から通知を受信できる前に構成されている次のアクセス許可が必要です。 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; このアプリを登録し、Google Cloud Messaging からメッセージを受信する権限を付与します。 (何`c2dm`という意味ですか? この_Device Messaging をクラウド_、ここで推奨されなくなりました GCM 前身であります。 
    GCM が現在も使用して`c2dm`そのアクセス許可文字列の多くでします)。 

-   `android.permission.WAKE_LOCK` &ndash; (省略可能)メッセージをリッスンしているときにスリープ状態にデバイスからの CPU を防止します。 

-   `android.permission.INTERNET` &ndash; GCM とクライアント アプリケーションが通信できるように、インターネット アクセスを付与します。 

-   *package_name* `.permission.C2D_MESSAGE` &ndash; Android アプリケーションを登録し、排他的すべて C2D を受信する権限を要求 (デバイスにクラウド) メッセージ。 *Package_name*プレフィックスは、アプリケーション ID と同じです。 

Android マニフェストに、これらのアクセス許可を設定します。 編集しましょう**AndroidManifest.xml**し、内容を次の XML に置き換えます。 

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

上記の XML では変更*YOUR_PACKAGE_NAME*クライアント アプリケーション プロジェクトのパッケージ名にします。 たとえば、`com.xamarin.gcmexample` のようにします。 

### <a name="check-for-google-play-services"></a>Google Play サービスのチェック

このチュートリアルで作成しています、最低限のアプリを 1 つの`TextView`UI にします。 このアプリは、GCM との対話を直接示されません。 表示する出力ウィンドウを見るおを代わりに、方法、GCM と、アプリのハンドシェイクおよび受信したとき、新しい通知の通知トレイをチェックします。 

最初に、メッセージ領域のレイアウトを作成してみましょう。 編集**Resources.layout.Main.axml**し、内容を次の XML に置き換えます。 

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

クライアント アプリの起動時、GCM を連絡する前に Google Play サービスが利用可能なであることを確認します。 編集**MainActivity.cs**と置換、``count``インスタンスの次の変数宣言で変数の宣言をインスタンス化します。 

```csharp
TextView msgText;
```

次のメソッドを次に、追加、 **MainActivity**クラス。 

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

このコードは、Google プレイ サービス APK がインストールされているかどうかにデバイスを確認します。 インストールされていない場合、APK、Google Play ストアからダウンロード (または、デバイスのシステム設定で有効にする) をユーザーに指示するメッセージ領域にメッセージが表示されます。 このメソッドの呼び出しの末尾に追加されます、クライアント アプリケーションが開始されるときに、このチェックを実行する必要があるため`OnCreate`です。 


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

このコードでは、Google プレイ サービス APK の存在を確認し、結果をメッセージ領域に書き込みます。 

みましょう完全に再構築し、アプリを実行します。 次のスクリーン ショットのような画面が表示されます。 

[![Google Play サービスが利用可能です](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

この結果が得られない場合は、Google プレイ サービス APK がデバイスにインストールされていることを確認してください、 **Xamarin Google Play サービス - GCM**パッケージに追加、 **ClientApp**前述のようにプロジェクト。先ほどの。 ビルド エラーが発生した場合、ソリューションをクリーンアップし、プロジェクトを再度ビルドしてください。 

次に、GCM に連絡し、登録トークンを取得するコードを作成します。

### <a name="register-with-gcm"></a>GCM と登録します。

アプリは、アプリ サーバーからリモートの通知を受け取ることができます、前に、GCM 登録および登録トークンを取得があります。 GCM でアプリケーションの登録作業はによって処理される、`IntentService`作成します。 当社`IntentService`は、次の手順を実行します。 

1.  使用して、 [InstanceID](https://developers.google.com/instance-id/)アプリ サーバーにアクセスするクライアント アプリケーションを承認するセキュリティ トークンを生成する API。 代わりに、登録トークンから返さ GCM です。

2.  アプリケーション サーバーを使用することが必要です) 場合は、アプリ サーバーの登録トークンを転送します。

3.  1 つまたは複数の通知トピック チャネルをサブスクライブします。

この実装後`IntentService`をテストするかどうかは、登録トークンから返さ GCM を参照してください。

という名前の新しいファイルを追加**RegistrationIntentService.cs**テンプレート コードを次に置き換えます。


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

上記のサンプル コードで次のように変更します。 *YOUR_SENDER_ID* 、送信者の ID 番号、クライアント アプリケーション プロジェクトにします。 プロジェクトの送信者 ID を取得します。 

1.  ログイン、 [Google Cloud コンソール](https://console.cloud.google.com/)プルダウン メニューから、プロジェクト名を選択します。 **プロジェクト情報**プロジェクトに表示されているウィンドウのをクリックして**プロジェクトの設定を移動**:

    [![XamarinGCM プロジェクトを選択します。](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  **設定** ページで、検索、**プロジェクト番号**&ndash;これは、プロジェクトの送信者 ID:

    [![プロジェクトの数が表示されます。](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

開始する、`RegistrationIntentService`アプリが実行を開始するときにします。 編集**MainActivity.cs**および変更、`OnCreate`メソッドできるように、 `RegistrationIntentService` Google Play サービスの存在を確認した後に開始します。 

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

ここでの各セクションを見てみましょう`RegistrationIntentService`しくみを理解します。 

最初に、ここに注釈を付ける、`RegistrationIntentService`サービスが、システムで、インスタンス化しないようにことを示す次の属性を持つ。 

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService`コンス トラクターは、ワーカー スレッドを名前*RegistrationIntentService*デバッグを容易にします。 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

コア機能`RegistrationIntentService`に存在する、`OnHandleIntent`メソッドです。 GCM と、アプリの登録を表示するには、このコードを見てみましょう。


##### <a name="request-a-registration-token"></a>トークンを要求する登録

`OnHandleIntent` Google のまず呼び出し[InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) GCM から登録トークンを要求します。 このコードをラップして、`lock`複数登録インテントが同時に発生する可能性を防ぐ&ndash;、`lock`これらインテントが順番に処理されます。 登録トークンを取得することが失敗した場合は、例外がスローされ、エラー ログに記録します。 登録が成功すると、 `token` GCM からもどったら登録トークンに設定されています。 

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

登録トークンを取得する場合 (つまり、例外がスローされなかった)、呼び出す`SendRegistrationToAppServer`にユーザーの登録を関連付けるにはサーバー側のアカウント (存在する場合) を使用してトークンに保持される、アプリケーションでします。 この実装は、アプリ サーバーの設計に依存しているため、空のメソッドはここで説明します。 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

場合によっては、アプリ サーバー必要はありません、ユーザーの登録トークンです。その場合は、このメソッドは省略できます。 アプリ サーバーに登録トークンが送信される`SendRegistrationToAppServer`をトークンがサーバーに送信されたかどうかを示すブール値を維持する必要があります。 このブール値が false の場合`SendRegistrationToAppServer`トークンをアプリ サーバーに送信&ndash;それ以外の場合、トークンは、前の呼び出しで、アプリ サーバーに送信されました既にです。 


##### <a name="subscribe-to-the-notification-topic"></a>通知のトピックを定期受信します。

次と呼ば、`Subscribe`メソッドを示すために GCM 通知のトピックを定期受信するようにします。 `Subscribe`を呼び出して、 [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;)下にあるすべてのメッセージに、クライアント アプリケーションをサブスクライブする API `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

アプリケーション サーバーが通知メッセージを送信する必要があります`/topics/global`を受信する場合。 トピックの名前を`/topics`アプリ サーバーとクライアント アプリケーションは、これらの名前について合意限りするは、何も指定できます。 (ここでは、名前を選択して`global`アプリケーション サーバーでサポートされているすべてのトピックにメッセージを受信することを指定します)。 

サーバー側でメッセージング GCM トピックについては、Google を参照してください。[トピックにメッセージを送信](https://developers.google.com/cloud-messaging/topic-messaging)です。 


#### <a name="implement-an-instance-id-listener-service"></a>インスタンス ID リスナー サービスを実装します。

登録トークンを一意かつセキュリティで保護します。ただし、クライアント アプリケーション (または GCM) は、アプリの再インストールまたはセキュリティ上の問題が発生した場合、登録トークンを更新する必要があります。 このため、実装する必要があります、 `InstanceIdListenerService` GCM からのトークンの更新要求に応答します。 

という名前の新しいファイルを追加**InstanceIdListenerService.cs**テンプレート コードを次に置き換えます。 

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

注釈を付ける`InstanceIdListenerService`サービスは、システムで、インスタンス化しないようにする、GCM 登録トークンを受信できることを示すために次の属性を持つ (とも呼ばれる*インスタンス ID*) 更新要求。 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

`OnTokenRefresh`サービスでメソッドを開始、`RegistrationIntentService`新しい登録トークンをインターセプトできるようにします。


#### <a name="test-registration-with-gcm"></a>GCM と登録をテストします。

みましょう完全に再構築し、アプリを実行します。 GCM から正常に登録トークンを受信する場合、登録トークンが、出力ウィンドウに表示されます。 例: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>ダウン ストリーム メッセージを処理します。 

これまでに実装したコードは「設定」コードのみです。Google Play サービスがインストールされているし、リモートの通知を受信するため、クライアント アプリを準備するには、GCM と、アプリ サーバーとネゴシエートかどうかを確認します。 ただし、あるまだを実際に受信して、ダウン ストリームの通知メッセージを処理するコードを実装します。 これを行うには、実装する必要があります、 *GCM リスナー サービス*です。 このサービスは、アプリ サーバーからトピック メッセージを受信し、通知としてそれらをローカルにブロードキャストします。 このサービスの実装は後に、メッセージを送信 GCM 場合は、実装が正常に動作を確認できるため、テスト プログラムを作成します。 


#### <a name="add-a-notification-icon"></a>通知アイコンを追加します。

最初の通知が起動されたときに、通知領域に表示される小さいアイコンを追加してみましょう。 コピーすることができます[このアイコン](remote-notifications-with-gcm-images/ic-stat-ic-notification.png)をプロジェクトにするか、独自のカスタム アイコンを作成します。 という名前をアイコン ファイル**ic_stat_button_click.png**にコピーし、**リソース/描画**フォルダーです。 使用してください**追加 > 既存アイテム.**にこのアイコン ファイルをプロジェクトに含めます。


#### <a name="implement-a-gcm-listener-service"></a>GCM リスナー サービスを実装します。

という名前の新しいファイルを追加**GcmListenerService.cs**テンプレート コードを次に置き換えます。

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

各セクションを見てみましょう、`GcmListenerService`しくみを理解します。 

最初に、ここに注釈を付ける`GcmListenerService`属性を示し、このサービスは、システムで、インスタンス化しないように、GCM メッセージを受信したを示すために意図的にフィルターを用意しています。 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

ときに`GcmListenerService`GCM からメッセージを受信、`OnMessageReceived`メソッドが呼び出されます。 このメソッドから渡されたメッセージの内容を抽出する`Bundle`、メッセージのコンテンツ (したがって、出力ウィンドウに表示できます) をログに記録および呼び出し`SendNotification`を受信したメッセージの内容を含むローカル通知を起動します。 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification`メソッドを使用`Notification.Builder`使用するには、通知し、作成、`NotificationManager`を通知を起動します。 実際には、ローカルの通知をユーザーに提示するのにリモートの通知メッセージを変換します。
使用しての詳細については`Notification.Builder`と`NotificationManager`を参照してください[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)です。


#### <a name="declare-the-receiver-in-the-manifest"></a>マニフェストに、受信側を宣言します。

GCM からメッセージを受信してできます、前に、Android のマニフェストに GCM リスナーに宣言する必要があります。 編集しましょう**AndroidManifest.xml**と置換、`<application>`を次の XML セクション。 

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

上記の XML では変更*YOUR_PACKAGE_NAME*クライアント アプリケーション プロジェクトのパッケージ名にします。 このチュートリアルの例では、パッケージ名は`com.xamarin.gcmexample`します。 

この XML で、各設定の動作内容を見てみましょう。

|設定|説明|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|アプリをキャプチャしてプッシュ通知の受信のメッセージを処理する GCM 受信者が実装されているを宣言します。|
|`com.google.android.c2dm.permission.SEND`|GCM サーバーのみが、アプリに直接メッセージを送信できることを宣言します。|
|`com.google.android.c2dm.intent.RECEIVE`|インテント フィルターが、アプリケーションの動作が GCM からブロードキャスト メッセージを処理することをアドバタイズします。|
|`com.google.android.c2dm.intent.REGISTRATION`|目的のフィルターをアドバタイズして、アプリが新しい登録インテントを処理する (つまり、インスタンス ID のリスナー サービスを実装しましたが)。|

代わりに、装飾できます`GcmListenerService`XML; で指定するのではなく、これらの属性とここで指定で**AndroidManifest.xml**コード サンプルは、実行しやすいようにします。 


### <a name="create-a-message-sender-to-test-the-app"></a>アプリケーションをテストするメッセージの送信者を作成します。

C# デスクトップ コンソール アプリケーション プロジェクトをソリューションに追加し、それを呼び出してしましょう**MessageSender**です。 このコンソール アプリケーションを使用して、アプリケーション サーバーをシミュレート&ndash;ことが通知メッセージを送信**ClientApp** GCM を介してします。 


#### <a name="add-the-jsonnet-package"></a>Json.NET のパッケージを追加します。

このコンソール アプリケーションでクライアント アプリを送信する通知メッセージを含む JSON ペイロードが進めです。 使用して、 **Json.NET**にパッケージ化**MessageSender** GCM で必要な JSON オブジェクトを構築するが容易にします。 Visual Studio を右クリックして**参照 > NuGet パッケージの管理.**; for Mac を Visual Studio で右クリック**パッケージ > パッケージを追加しています.**. 

探してみましょう、 **Json.NET**をパッケージ化し、プロジェクトにインストールします。 

[![Json.NET のパッケージをインストールします。](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>System.Net.Http への参照を追加します。

参照を追加する必要がありますも`System.Net.Http`インスタンス化できるように、 `HttpClient` GCM に、テスト メッセージを送信するためです。 **MessageSender**プロジェクトを右クリックして**参照 > 参照の追加**が表示されるまで下にスクロールし、 **System.Net.Http**です。 横にチェック マークを付けます**System.Net.Http**  をクリック**OK**です。 


#### <a name="implement-code-that-sends-a-test-message"></a>テスト メッセージを送信するコードを実装します。

**MessageSender**、編集**Program.cs**し、内容を次のコードに置き換えます。

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

上記のコードで次のように変更します。 *YOUR_API_KEY* API キーを、クライアント アプリケーション プロジェクトにします。 

このテスト アプリのサーバーでは、GCM に次の JSON 形式のメッセージを送信します。

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM は、さらに、クライアント アプリケーションには、このメッセージを転送します。 構築してみましょう**MessageSender**コマンドラインから実行ここで、コンソール ウィンドウを開きます。



### <a name="try-it"></a>手順を次に示します。

これで、クライアント アプリをテストする準備ができました。 エミュレーターを使用しているかどうか、またはデバイスが Wi-fi 経由で GCM と通信している場合は、GCM メッセージを介して取得するためにファイアウォールで、次の TCP ポートを開く必要があります: 5228、5229、および 5230 です。

クライアント アプリを起動し、出力ウィンドウを確認します。 後に、`RegistrationIntentService`登録が正常に受信 GCM からトークンを出力ウィンドウが表示トークンで、次のようなログ出力。

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

この時点でクライアント アプリケーションは、リモートの通知メッセージを受信する準備ができています。 コマンドラインから実行、 **MessageSender.exe**クライアント アプリに「こんにちは, Xamarin」通知メッセージを送信するプログラムです。
まだビルドしていない場合、 **MessageSender**プロジェクトで、それを実行します。

実行する**MessageSender.exe** Visual Studio は、コマンド プロンプトを開き、変更、 **MessageSender/Bin/debug**ディレクトリ、および直接コマンドを実行します。

```cmd
MessageSender.exe
```

実行する**MessageSender.exe** Mac 用 Visual Studio で、ターミナル セッションを開き、変更**MessageSender/Bin/debug**ディレクトリ、および実行に使用するモノラル**MessageSender.exe** 

```bash
mono MessageSender.exe
```

GCM とクライアント アプリまで戻しますを伝達するメッセージの 1 分かかる可能性があります。 メッセージを正常に受信する場合は、出力ウィンドウでは、次のような出力を参照してくださいお必要があります。 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

さらに、新しい通知アイコンが通知のトレイに表示されているが発生する必要があります。 

[![デバイスに表示される Notiication アイコン](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

通知を表示する通知トレイを開くと、リモートの通知が表示されます。

[![通知メッセージが表示されます。](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

以上で、アプリがその最初のリモートの通知を受信しました。

場合は、アプリでは、強制停止 GCM メッセージを受信する不要になったことに注意してください。 通知は、強制停止後に再開、アプリは、手動再起動する必要があります。 この Android ポリシーの詳細については、次を参照してください。[停止状態のアプリケーション上のコントロールを起動して](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)され、この[スタックのオーバーフロー post](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267)です。 

 
## <a name="summary"></a>まとめ

このチュートリアルでは、Xamarin.Android アプリケーションでリモートの通知を実装するための手順が詳しく説明します。 GCM 通信に必要なその他のパッケージをインストールする方法が説明されているし、GCM サーバーにアクセスするアプリのアクセス許可を構成する方法を説明します。 Google Play サービスの存在をチェックする方法、目的のサービスを登録および登録トークン、GCM とネゴシエートするインスタンス ID のリスナー サービスを実装する方法、および GCM リスナーを実装する方法を示すサンプル コードを提供受信およびリモートの通知メッセージを処理するサービスです。 最後に、GCM を通じて、クライアント アプリケーションの動作にテスト通知を送信するコマンド ライン テスト プログラムを実装します。 


## <a name="related-links"></a>関連リンク

- [GCM RemoteNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
