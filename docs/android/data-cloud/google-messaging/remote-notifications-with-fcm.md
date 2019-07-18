---
title: リモート通知 firebase Cloud Messaging
description: このチュートリアルでは、Xamarin.Android アプリケーションで Firebase Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれます) を実装する方法の詳細な手順の説明を提供します。 Firebase Cloud Messaging (FCM) との通信に必要な、FCM にアクセスするために Android マニフェストを構成する方法の例をまとめたものを Firebase を使用して、ダウン ストリームのメッセージングを示すさまざまなクラスを実装する方法を示していますコンソール。
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: a50a2014e28becacb2c9f4965b7f3377be57ab16
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830317"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>リモート通知 firebase Cloud Messaging

_このチュートリアルでは、Xamarin.Android アプリケーションで Firebase Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれます) を実装する方法の詳細な手順の説明を提供します。Firebase Cloud Messaging (FCM) との通信に必要な、FCM にアクセスするために Android マニフェストを構成する方法の例をまとめたものを Firebase を使用して、ダウン ストリームのメッセージングを示すさまざまなクラスを実装する方法を示していますコンソール。_

## <a name="fcm-notifications-overview"></a>FCM 通知の概要

このチュートリアルで、基本的なアプリと呼ばれる**FCMClient** FCM メッセージングの基本事項を説明するために作成されます。 **FCMClient** Google play 開発者サービスの存在を確認します、FCM から登録トークンを受け取る、Firebase コンソールから送信するリモート通知が表示されます、トピック メッセージをサブスクライブします。

[![アプリのスクリーン ショットの例](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

次のトピックも紹介します。

1.  バック グラウンド通知

2.  トピックのメッセージ

3.  フォア グラウンドの通知

このチュートリアルでは、機能を段階的に追加されます**FCMClient**し、デバイスまたは FCM と対話する方法を理解するためのエミュレーターで実行します。 ライブ アプリ トランザクションが、FCM サーバーとミラーリング監視サーバーにログ記録を使用して、Firebase コンソール通知 GUI に入力した FCM メッセージから通知を生成する方法がわかります。

## <a name="requirements"></a>必要条件


理解するおくと役、[さまざまな種類のメッセージの](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages)Firebase Cloud Messaging で送信することができます。 メッセージのペイロードは、クライアント アプリを受信およびメッセージの処理方法が決まります。

このチュートリアルを続行するには、Google の FCM サーバーを使用するために必要な資格情報を取得する必要があります。このプロセスについては[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm)します。
具体的には、ダウンロードする必要があります、 **google-services.json**このチュートリアルで説明するコード例で使用するファイル。 かどうかはまだ作成していないプロジェクトで、Firebase コンソール (まだダウンロードしていない場合や、 **google-services.json**ファイル) を参照してください[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)します。

例のアプリを実行するには、テストの Android デバイスまたは firebase の互換性のあるエミュレーターを必要があります。 Firebase Cloud Messaging で Android 4.0 以降を実行しているクライアントをサポートしているし、これらのデバイスには、Google Play ストア アプリがインストールされている必要があります (Google Play Services 9.2.1 または以降が必要です)。 デバイスにインストールされている Google Play ストア アプリがあるまだない場合は、次を参照してください。、 [Google Play](https://support.google.com/googleplay) web サイトをダウンロードしてインストールします。 または、テスト デバイス (Android SDK エミュレーターを使用している場合は、Google Play ストアをインストールする必要はありません) の代わりにインストールされている Google play 開発者サービスで、Android SDK エミュレーターを使用できます。

## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

という名前の新しい空の Xamarin.Android プロジェクトの作成を開始する**FCMClient**します。 Xamarin.Android プロジェクトの作成に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)します。
新しいアプリを作成した後、次の手順は、パッケージ名を設定し、FCM との通信に使用されるいくつかの NuGet パッケージをインストールするのには。

### <a name="set-the-package-name"></a>パッケージ名を設定します。

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)、FCM 対応アプリのパッケージ名を指定します。 このパッケージ名としても機能、 [*アプリケーション ID* ](./firebase-cloud-messaging.md#fcm-in-action-app-id)関連付けられている、 [API キー](firebase-cloud-messaging.md#fcm-in-action-api-key)します。 このパッケージ名を使用するアプリを構成します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  プロパティを開き、 **FCMClient**プロジェクト。

2.  **Android マニフェスト** ページで、パッケージ名を設定します。

次の例では、パッケージ名に設定されて`com.xamarin.fcmexample`:

[![パッケージ名を設定します。](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

更新する際、 **Android マニフェスト**、ことを確認のチェックも、`Internet`アクセス許可を有効にします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  プロパティを開き、 **FCMClient**プロジェクト。

2.  **Android アプリケーション** ページで、パッケージ名を設定します。

次の例では、パッケージ名に設定されて`com.xamarin.fcmexample`:

[![パッケージ名を設定します。](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

更新する際、 **Android マニフェスト**、ことを確認のチェックも、`INTERNET`アクセス許可が有効になっている (**アクセス許可が必要な**)。

-----

> [!IMPORTANT]
> クライアント アプリはこのパッケージ名でない場合は、FCM から登録トークンを受信することはできません*まったく*Firebase コンソールに入力されたパッケージ名と一致します。

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play サービス基本パッケージを追加します。

Firebase Cloud Messaging、Google play 開発者サービス、依存しているため、 [Xamarin Google play 開発者サービスのベース](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/)Xamarin.Android プロジェクトに NuGet パッケージを追加する必要があります。 バージョン 29.0.0.2 必要がありますまたはそれ以降。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Visual Studio で、右クリックして**参照 > NuGet パッケージの管理.** .

2.  をクリックして、**参照**タブし、検索**Xamarin.GooglePlayServices.Base**します。

3.  このパッケージをインストール、 **FCMClient**プロジェクト。

    [![Google Play Services ベースのインストール](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  Visual Studio for Mac では、右クリックして**パッケージ > パッケージを追加しています.** .

2.  検索**Xamarin.GooglePlayServices.Base**します。

3.  このパッケージをインストール、 **FCMClient**プロジェクト。

    [![Google Play Services ベースのインストール](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet のインストール中にエラーが発生する場合は閉じます、 **FCMClient**プロジェクト、それを再度開き、NuGet のインストールを再試行します。

インストールするときに**Xamarin.GooglePlayServices.Base**、すべての必要な依存関係もインストールされます。 編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Android.Gms.Common;
```

このステートメントを使用する、`GoogleApiAvailability`クラス**Xamarin.GooglePlayServices.Base**できる**FCMClient**コード。
`GoogleApiAvailability` Google play 開発者サービスの有無を確認に使用されます。

### <a name="add-the-xamarin-firebase-messaging-package"></a>Xamarin Firebase Messaging パッケージを追加します。

FCM からメッセージを受信する、[メッセージング - Xamarin Firebase](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/)アプリ プロジェクトに NuGet パッケージを追加する必要があります。 このパッケージは、Android アプリケーションは FCM サーバーからのメッセージを受信できません。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Visual Studio で、右クリックして**参照 > NuGet パッケージの管理.** .

2. 検索**Xamarin.Firebase.Messaging**します。

3.  このパッケージをインストール、 **FCMClient**プロジェクト。

    [![Xamarin Firebase のメッセージングをインストールします。](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  Visual Studio for Mac では、右クリックして**パッケージ > パッケージを追加しています.** .

2.  検索**Xamarin.Firebase.Messaging**します。

3.  このパッケージをインストール、 **FCMClient**プロジェクト。

    [![Xamarin Firebase のメッセージングをインストールします。](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

インストールするときに**Xamarin.Firebase.Messaging**、すべての必要な依存関係もインストールされます。

次に、編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

最初の 2 つのステートメント内の型を作成する、 **Xamarin.Firebase.Messaging** NuGet パッケージを使用可能な**FCMClient**コード。 **Android.Util** FMS とトランザクションの確認に使用されるログ記録機能を追加します。

### <a name="add-googleplayservices-json"></a>Google Services JSON ファイルを追加します。

次の手順が追加するには、 **google-services.json**ファイルをプロジェクトのルート ディレクトリ。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  コピー **google-services.json**プロジェクト フォルダーにします。

2.  追加**google-services.json**アプリ プロジェクトを (をクリックして**すべてのファイル**で、**ソリューション エクスプ ローラー**を右クリックして**google-services.json**を選択し、**プロジェクトに含める**)。

3.  選択**google-services.json**で、**ソリューション エクスプ ローラー**ウィンドウ。

4.  **プロパティ**ペインで、設定、**ビルド アクション**に**GoogleServicesJson**:

    [![ビルド アクションを GoogleServicesJson に設定します。](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > 場合、 **GoogleServicesJson**ビルド アクションが示されていません、保存、ソリューションを閉じ、再度開きます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  コピー **google-services.json**プロジェクト フォルダーにします。

2.  追加**google-services.json**アプリ プロジェクトにします。

3.  右クリックして**google-services.json**します。

4.  設定、**ビルド アクション**に**GoogleServicesJson**:

    [![ビルド アクションを GoogleServicesJson に設定します。](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

ときに**google-services.json** 、プロジェクトに追加されます (および**GoogleServicesJson**ビルド アクションが設定されている)、ビルド プロセスは、クライアント ID を抽出しますおよび[API キー](./firebase-cloud-messaging.md#fcm-in-action-api-key)し。結合生成されるこれらの資格情報を追加します。 **AndroidManifest.xml**に存在する**obj/Debug/android/AndroidManifest.xml**します。 このマージ プロセスは、すべてのアクセス許可と FCM サーバーへの接続に必要なその他の FCM 要素に自動的に追加します。


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Google play 開発者サービスを確認し、通知チャネルを作成

Google Android アプリを Google play 開発者サービスの機能にアクセスする前に、Google Play Services APK の存在に確認をお勧めします (詳細については、次を参照してください。 [Google play 開発者サービスの確認](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play))。

アプリの UI の初期レイアウトを最初に作成されます。 編集**Resources/layout/Main.axml**内容を次の XML に置き換えます。

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

これは、 `TextView` Google play 開発者サービスがインストールされているかどうかを示すメッセージを表示するために使用されます。 変更を保存**Main.axml**します。


編集**MainActivity.cs**し、次のインスタンス変数を追加して、`MainActivity`クラス。

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

変数`CHANNEL_ID`と`NOTIFICATION_ID`メソッドで使用される[ `CreateNotificationChannel` ](#create-notification-channel-code)に追加される`MainActivity`このチュートリアルで後でします。


次の例では、`OnCreate`メソッドは、アプリは FCM サービスを使用する前に Google play 開発者サービスが利用できることを確認します。
次のメソッドを追加、`MainActivity`クラス。

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
            msgText.Text = "This device is not supported";
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

このコードは、Google Play Services APK がインストールされているかどうかにデバイスを確認します。 メッセージが表示されますがインストールされていない場合、 `TextBox` APK を Google Play ストアからダウンロードする (またはデバイスのシステム設定で有効にする) のユーザーに指示します。

<a name="create-notification-channel-code"></a>Android 8.0 (API レベル 26) 以降を実行しているアプリを作成する必要があります、 [_通知チャネル_](~/android/app-fundamentals/notifications/local-notifications.md)通知を発行します。  次のメソッドを追加、`MainActivity`クラス (必要に応じて) 通知チャネルが作成されます。

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channel = new NotificationChannel(CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

`OnCreate` メソッドを次のコードで置き換えます。

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable` 最後に呼び出されます`OnCreate`Google play 開発者サービスは、各実行時間のアプリが起動を確認できるようにします。 メソッド`CreateNotificationChannel`は Android 8 を実行しているデバイスの通知チャネルが存在することを確認すると呼ばれる以上。 アプリがある場合、`OnResume`メソッドを呼び出す必要が`IsPlayServicesAvailable`から`OnResume`もします。 完全に再構築し、アプリを実行します。 すべてが正しく構成されて、次のスクリーン ショットのような画面が表示されます。

[![アプリは、Google play 開発者サービスが使用可能なであることを示します](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

この結果を得られない場合は、Google Play Services APK がデバイスにインストールされていることを確認します (詳細については、次を参照してください。[設定を Google play 開発者サービス](https://developers.google.com/android/guides/setup))。
追加したこと確認することも、 **Xamarin.Google.Play.Services.Base**をパッケージ化、 **FCMClient**前述のようにプロジェクトします。


## <a name="add-the-instance-id-receiver"></a>インスタンス ID のレシーバーを追加します。

次の手順が拡張するサービスを追加するには`FirebaseInstanceIdService`ハンドルの作成、回転、および更新[Firebase 登録トークン](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token)します。 `FirebaseInstanceIdService`サービスがデバイスにメッセージを送信できる FCM 必要です。 ときに、`FirebaseInstanceIdService`サービスは、アプリは自動的に FCM メッセージを受信し、アプリが backgrounded たびに通知として表示して、クライアント アプリに追加されます。

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android マニフェストで、受信側を宣言します。

編集**AndroidManifest.xml**し、次の挿入`<receiver>`要素を`<application>`セクション。

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

この XML は、次を行います。

-   宣言を`FirebaseInstanceIdReceiver`実装を提供する、[一意識別子](https://developers.google.com/instance-id/)の各アプリ インスタンス。 この受信者は認証し、アクションを承認します。

-   内部宣言`FirebaseInstanceIdInternalReceiver`安全にサービスを開始するために使用する実装。

-   [アプリ ID](./firebase-cloud-messaging.md#fcm-in-action-app-id)に格納されている場合は、 **google-services.json**がファイル[プロジェクトに追加](#add-googleplayservices-json)します。 Xamarin.Android Firebase バインドがトークンを置き換える`${applicationId}`; アプリ ID を持つ追加のコードは必要ありません、クライアント アプリでアプリ ID を指定するには

`FirebaseInstanceIdReceiver`は、`WakefulBroadcastReceiver`を受け取る`FirebaseInstanceId`と`FirebaseMessaging`イベントから派生したクラスに配信および`FirebaseInstanceIdService`します。

### <a name="implement-the-firebase-instance-id-service"></a>Firebase インスタンス ID サービスを実装します。

FCM で、アプリケーションの登録作業は、カスタム`FirebaseInstanceIdService`提供するサービス。
`FirebaseInstanceIdService` 次の手順を実行します。

1.  使用して、[インスタンス ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) FCM とアプリのサーバーにアクセスするクライアント アプリを承認するセキュリティ トークンを生成します。 代わりに、アプリが返される、[登録トークン](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token)FCM から。

2.  アプリのサーバーで必要な場合は、アプリ サーバーに登録トークンを転送します。

という新しいファイルを追加**MyFirebaseIIDService.cs**次のテンプレート コードを置き換えます。

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

このサービスを実装して、`OnTokenRefresh`登録トークンが最初に作成または変更されたときに呼び出されるメソッド。 ときに`OnTokenRefresh`実行から最新のトークンを取得、`FirebaseInstanceId.Instance.Token`プロパティ (これは、FCM によっては非同期的に更新されます)。 この例で、出力ウィンドウに表示できますように更新トークンが記録されます。

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` 呼び出される頻度の低い: 次の状況でトークンの更新に使用されます。

-   ときに、アプリがインストールまたはアンインストールします。

-   ときに、ユーザーは、アプリ データを削除します。

-   アプリに消去されるインスタンスの id。

-   トークンのセキュリティが侵害された場合。

Google のに従って[インスタンス ID](https://developers.google.com/instance-id/guides/android-implementation)ドキュメントについては、FCM インスタンス ID サービスが要求に、アプリがトークン定期的に (通常は、6 か月ごと) を更新します。

`OnTokenRefresh` 呼び出して`SendRegistrationToAppServer`に関連付けるユーザーの登録トークン サーバー側のアカウント (ある場合) で保持されているアプリケーションで。

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

この実装は、アプリ サーバーの設計に依存するため、この例では、空のメソッド本体が提供されます。 アプリケーション サーバーは、FCM 登録情報を必要とする場合は変更`SendRegistrationToAppServer`アプリによって管理される任意のサーバー側アカウントとユーザーの FCM インスタンス ID トークンを関連付ける。 (トークンがクライアント アプリに不透明なことに注意してください)。

トークンは、アプリ サーバーに送信されるときに`SendRegistrationToAppServer`トークンがサーバーに送信されたかどうかを示すブール値を維持する必要があります。 このブール値が false の場合`SendRegistrationToAppServer`トークンをアプリ サーバーから送信する&ndash;それ以外の場合、トークンは、前の呼び出しでアプリのサーバーに送信されて既にします。 場合によっては (このなど`FCMClient`例)、アプリのサーバーでは、トークンは必要はありません。 したがって、このメソッドはこの例では必要はありません。

## <a name="implement-client-app-code"></a>クライアント アプリ コードを実装します。

受信側サービスが準備できたので、これらのサービスを活用するためにクライアント アプリ コードを記述できます。 次のセクションで、ボタンは、登録トークンをログオン UI に追加されます (とも呼ばれます、*インスタンス ID トークン*) により多くのコードを追加および`MainActivity`を表示する`Intent`からアプリを起動したときに情報を通知:

[![ログ アプリの画面に追加トークン ボタン](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>ログのトークン

デモの目的でのみこの手順で追加したコードは、&ndash;運用環境のクライアント アプリは登録トークンを記録する必要がありません。 編集**Resources/layout/Main.axml**し、以下の追加`Button`宣言の直後に、`TextView`要素。

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

`MainActivity.OnCreate` メソッドの末尾に次のコードを追加します。

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

このコードでは、現在のトークンをログ出力ウィンドウにときに、**ログ トークン** ボタンをタップします。

### <a name="handle-notification-intents"></a>通知のインテントを処理します。

ユーザーがから発行された通知をタップした**FCMClient**、その通知に付属するすべてのデータでメッセージを利用`Intent`extras します。 編集**MainActivity.cs**の先頭に次のコードを追加し、`OnCreate`メソッド (呼び出しの前に`IsPlayServicesAvailable`)。

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

アプリの起動ツール`Intent`ため、このコードに付属するデータをログイン、ユーザーがその通知メッセージをタップしたときに発生、`Intent`出力ウィンドウにします。 場合は、異なる`Intent`起動する必要があります、`click_action`に通知メッセージのフィールドを設定する必要があります`Intent`(ランチャー`Intent`ない場合に使用`click_action`が指定されて)。


## <a name="background-notifications"></a>バック グラウンド通知

ビルドし、実行、 **FCMClient**アプリ。 **ログ トークン**ボタンが表示されます。

[![トークン [ログ] ボタンが表示されます。](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

タップして、**ログ トークン**ボタンをクリックします。 IDE の [出力] ウィンドウで、次のようなメッセージを表示する必要があります。

[![出力ウィンドウに表示されるインスタンス ID トークン](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

長い文字列が付いた**トークン**Firebase コンソールに貼り付けるはインスタンスの ID トークンは、&ndash;を選択し、この文字列をクリップボードにコピーします。 インスタンス ID トークンが表示されない場合は、先頭に次の行を追加、`OnCreate`ことを確認するメソッド**google-services.json**正しく解析されました。

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id`出力ウィンドウに出力値が一致する必要があります、`mobilesdk_app_id`で記録された値**google-services.json**します。

### <a name="send-a-message"></a>メッセージを送信します。

サインイン、 [Firebase Console](https://console.firebase.google.com)、プロジェクトを選択**通知**、 をクリック**最初のメッセージを送信**:

[![送信の最初のメッセージ ボタン](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

**Compose メッセージ**ページで、メッセージ テキストを入力して選択**1 つのデバイス**します。 IDE の [出力] ウィンドウからインスタンス ID のトークンをコピーして貼り付けます、 **FCM 登録トークン**Firebase コンソールのフィールド。

[![メッセージ ダイアログ ボックスを作成します。](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android デバイス (またはエミュレーターで)、Android をタップして、アプリがバック グラウンド**概要**ボタンとホーム画面に触れることです。 デバイス準備ができたら、クリックして**メッセージの送信**Firebase コンソールで。

[![送信メッセージ ボタン](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

ときに、**レビュー メッセージ**ダイアログが表示されたら、をクリックして**送信**します。
通知アイコンは、デバイス (またはエミュレーター) の通知領域に表示する必要があります。

[![通知アイコンが表示されます。](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

メッセージを表示する通知アイコンを開きます。 通知メッセージに入力した内容が正確にする必要があります、**メッセージ テキスト**Firebase コンソールのフィールド。

[![デバイスでは、通知メッセージを表示してください。](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

起動する通知アイコンをタップ、 **FCMClient**アプリ。 `Intent`に送信される extras **FCMClient** IDE の出力ウィンドウに表示されます。

[![キー、メッセージ ID、および折りたたみキーから、インテント エクストラ リスト](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

この例で、**から**キーは、アプリの Firebase プロジェクト番号に設定されます (この例で`41590732`)、および**collapse_key**パッケージ名に設定されている (**com.xamarin.fcmexample**)。
メッセージを受け取らない場合は、削除することをお試しください、 **FCMClient**デバイス (またはエミュレーター) にアプリを上記の手順を繰り返します。


> [!NOTE]
> キューブを強制終了アプリとして設定すると場合、FCM は通知の送信を停止します。 Android では、バック グラウンド サービス ブロードキャストが不必要にまたは誤ってが停止状態のアプリケーションのコンポーネントを起動できなくなります。 (この動作の詳細については、次を参照してください[停止状態のアプリケーション上のコントロールを起動](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)。)。このためは、必要に応じて実行するたびに、アプリを手動でアンインストールし、デバッグ セッションを停止&ndash;これにより、FCM メッセージが受信するために継続されるように、新しいトークンを生成します。

### <a name="add-a-custom-default-notification-icon"></a>カスタムの既定の通知アイコンを追加します。

前の例では、通知アイコンは、アプリケーションのアイコンに設定されます。 次の XML では、カスタムの既定のアイコンの通知を構成します。 Android では、通知アイコンが明示的に設定されていないすべての通知メッセージの場合は、このカスタムの既定アイコンが表示されます。

カスタムの既定の通知アイコンを追加するには、アイコンを追加、**リソース/drawable**ディレクトリ、編集**AndroidManifest.xml**、し、次`<meta-data>`要素をに`<application>`セクション。

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

この例で、通知アイコンに存在するで**ic/リソース/drawable\_stat\_ic\_notification.png**はカスタムの既定の通知アイコンとして使用されます。 カスタムの既定のアイコンがで構成されていない場合**AndroidManifest.xml**と通知ペイロードでアイコンが設定されていない、(上記の通知アイコン スクリーン ショットに表示) と Android に通知アイコンとしてアプリケーションのアイコンが使用されます。

## <a name="handle-topic-messages"></a>トピックのメッセージを処理します。

これまでに記述されたコードでは、登録トークンを処理し、リモート通知機能をアプリに追加します。 次の例では、リッスンするコードを追加します*トピック メッセージ*し、ユーザーにリモートとして通知を転送します。 トピックのメッセージは、特定のトピックにサブスクライブする 1 つまたは複数のデバイスに送信される FCM メッセージです。 トピックのメッセージの詳細については、次を参照してください。[トピック メッセージング](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)します。

### <a name="subscribe-to-a-topic"></a>トピックをサブスクライブします。

編集**Resources/layout/Main.axml**し、以下の追加`Button`直後に、前の宣言`Button`要素。

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

この XML を追加、**通知をサブスクライブする**レイアウトにボタンをクリックします。
編集**MainActivity.cs**の末尾に次のコードを追加し、`OnCreate`メソッド。

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

このコードを検索、**通知をサブスクライブする**レイアウトでボタンをクリックし、そのクリック ハンドラーを呼び出すコードに代入`FirebaseMessaging.Instance.SubscribeToTopic`、サブスクライブしているトピックの「を渡して、_ニュース_します。 ユーザーがタップしたときに、**購読**ボタン、アプリをサブスクライブする、_ニュース_トピック。 次のセクションで、_ニュース_トピックのメッセージは、Firebase コンソール通知 GUI から送信されます。

### <a name="send-a-topic-message"></a>トピックのメッセージを送信します。

アプリをアンインストール、再構築し、再度実行します。 をクリックして、**通知をサブスクライブする**ボタンをクリックします。

[![サブスクライブ通知ボタン](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

アプリが正常にサブスクライブしている場合は表示**トピックの同期が成功した**IDE では、出力ウィンドウ。

[![出力ウィンドウには、トピックの同期が成功したメッセージが表示されます。](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

トピックのメッセージを送信するのにには、次の手順を使用します。

1.  Firebase コンソールで、次のようにクリックします。**新しいメッセージ**します。

2.  **Compose メッセージ**ページで、メッセージ テキストを入力して選択**トピック**します。

3.  **トピック**プルダウン メニューでは、組み込みのトピックを選択して**ニュース**:

    [![ニュース トピックを選択します。](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Android デバイス (またはエミュレーターで)、Android をタップして、アプリがバック グラウンド**概要**ボタンとホーム画面に触れることです。

5.  デバイス準備ができたら、クリックして**メッセージの送信**Firebase コンソールでします。

6.  IDE 出力ウィンドウを表示する確認 **/トピック/ニュース**ログの出力。

    [![/Topic/news からのメッセージが表示されます。](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

このメッセージが出力ウィンドウに表示される、ときに通知アイコンも Android デバイスで通知領域に表示されます。 トピックのメッセージを表示する通知アイコンを開きます。

[![トピックのメッセージが通知として表示されます。](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

メッセージを受け取らない場合は、削除することをお試しください、 **FCMClient**デバイス (またはエミュレーター) にアプリを上記の手順を繰り返します。

## <a name="foreground-notifications"></a>フォア グラウンドの通知

実装する必要があります foregrounded アプリで通知を受信する`FirebaseMessagingService`します。 このサービスは、データ ペイロードを受信するため、アップ ストリームのメッセージを送信するために必要なもできます。 次の例を拡張するサービスを実装する方法を説明する`FirebaseMessagingService`&ndash;作成されたアプリは、フォア グラウンドで実行中に、リモート通知を処理できます。

### <a name="implement-firebasemessagingservice"></a>FirebaseMessagingService を実装します。

`FirebaseMessagingService`サービスが受信および Firebase からメッセージを処理する責任を負います。 この型とオーバーライド アプリごとにサブクラス必要があります、`OnMessageReceived`受信メッセージを処理します。 アプリがフォア グラウンドにあるときに、`OnMessageReceived`コールバックは、メッセージを常に処理します。

> [!NOTE]
> アプリでは、Firebase Cloud の受信メッセージを処理するための 10 秒があるだけです。 このライブラリを使用してバック グラウンド実行をスケジュールする必要がありますよりも長くかかる作業、 [Android ジョブ スケジューラ](~/android/platform/android-job-scheduler.md)または[Firebase ジョブ ディスパッチャー](~/android/platform/firebase-job-dispatcher.md)します。

という新しいファイルを追加**MyFirebaseMessagingService.cs**次のテンプレート コードを置き換えます。

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

なお、`MESSAGING_EVENT`に新しい FCM メッセージが送られるように、インテント フィルターを宣言する必要があります`MyFirebaseMessagingService`:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

クライアント アプリケーションが FCM からメッセージを受信`OnMessageReceived`から渡されるでメッセージの内容を抽出します`RemoteMessage`オブジェクトを呼び出すことによってその`GetNotification`メソッド。 次に、IDE の出力ウィンドウに表示できますように、メッセージの内容が記録されます。

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> ブレークポイントを設定する場合`FirebaseMessagingService`、デバッグ セッション、FCM メッセージを配信する方法のため、これらのブレークポイントをヒットしない可能性があります。


### <a name="send-another-message"></a>別のメッセージを送信します。

アプリをアンインストール、再構築、もう一度、実行および別のメッセージを送信する次の手順します。

1.  Firebase コンソールで、次のようにクリックします。**新しいメッセージ**します。

2.  **Compose メッセージ**ページで、メッセージ テキストを入力して選択**1 つのデバイス**します。

3.  IDE の [出力] ウィンドウからトークン文字列をコピーして貼り付けます、 **FCM 登録トークン**Firebase コンソールを以前と同様のフィールド。

4.  アプリがフォア グラウンドで実行されていることを確認し、クリックして**メッセージの送信**Firebase コンソールで。

    [![コンソールから別のメッセージを送信します。](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  ときに、**レビュー メッセージ**ダイアログが表示されたら、をクリックして**送信**します。

6.  IDE の出力ウィンドウには、受信メッセージが記録されます。

    [![メッセージ本文は、出力ウィンドウに出力](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>ローカル通知の送信側を追加します。

この残りの例では、受信 FCM メッセージは、アプリがフォア グラウンドで実行中に起動されたローカル通知に変換されます。 編集**MyFirebaseMessageService.cs**し、以下の追加`using`ステートメント。

```csharp
using FCMClient;
using System.Collections.Generic;
```

次のメソッドを追加`MyFirebaseMessagingService`:

<a name="sendnotification-method"></a>

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

バック グラウンド通知からこの通知を区別するためには、このコードは、アプリケーション アイコンとは異なるアイコンが表示された通知をマークします。 ファイルを追加する[ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png)に**リソース/drawable**し、含めることで、 **FCMClient**プロジェクト.

`SendNotification`メソッドは`NotificationCompat.Builder`、通知を作成して`NotificationManagerCompat`通知を起動するために使用します。 通知を保持する`PendingIntent`は、アプリを開きに渡された文字列の内容を表示するユーザーに許可する`messageBody`。 詳細については`NotificationCompat.Builder`を参照してください[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)します。

呼び出す、`SendNotification`メソッドの最後に、`OnMessageReceived`メソッド。

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

これらの変更の結果として`SendNotification`フォア グラウンドでアプリがあり、通知領域で、通知は表示中に、通知を受け取るたびに実行されます。

アプリがバック グラウンドにあるときに、[メッセージのペイロード](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages)メッセージの処理方法が決まります。

* **通知**&ndash;にメッセージを送信は、**システム トレイ**します。 ローカル通知が表示されます。 ユーザーが通知をタップしたときに、アプリが起動します。
* **データ**&ndash;によって処理されるメッセージ`OnMessageReceived`します。
* **両方**&ndash;通知とデータの両方のペイロードのメッセージは、システム トレイに配信されます。 アプリが起動したら、データ ペイロードに表示されます、`Extras`の`Intent`アプリの起動に使用されました。

この例で、アプリが backgrounded、`SendNotification`場合は、メッセージのデータ ペイロードが実行されます。 それ以外の場合、(このチュートリアルで前に示した) バック グラウンド通知が起動されます。

### <a name="send-the-last-message"></a>最後のメッセージを送信します。

アプリをアンインストールし、再構築し、もう一度、実行、次の手順を使用して、最後のメッセージを送信します。

1.  Firebase コンソールで、次のようにクリックします。**新しいメッセージ**します。

2.  **Compose メッセージ**ページで、メッセージ テキストを入力して選択**1 つのデバイス**します。

3.  IDE の [出力] ウィンドウからトークン文字列をコピーして貼り付けます、 **FCM 登録トークン**Firebase コンソールを以前と同様のフィールド。

4.  アプリがフォア グラウンドで実行されていることを確認し、クリックして**メッセージの送信**Firebase コンソールで。

    [![フォア グラウンド メッセージを送信します。](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

今回は、出力ウィンドウに記録されたメッセージは、新しい通知にはパッケージ化も&ndash;アプリがフォア グラウンドで実行中に、通知トレイの通知アイコンが表示されます。

[![前景のメッセージの通知アイコン](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

通知を開くと、Firebase コンソール通知 GUI から送信された最後のメッセージが表示されます。

[![フォア グラウンド通知がフォア グラウンド アイコンで表示](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>FCM から切断しています

トピックのサブスクリプションの解除を呼び出し、 [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29)メソッドを[FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging)クラス。 サブスクリプションの解除の例については、_ニュース_トピックでは以前では、購読、 **Unsubscribe**ボタン ハンドラーのコードを次に、レイアウトに追加できます。

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

FCM 完全デバイスの登録を解除するには、呼び出すことにより、インスタンス ID を削除、 [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29)メソッドを[FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId)クラス。 例:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

このメソッドの呼び出しでは、インスタンス ID と関連付けられているデータを削除します。 その結果、デバイスに FCM データの定期的な送信は停止します。


## <a name="troubleshooting"></a>トラブルシューティング

次の記述の問題と回避策が Xamarin.Android を Firebase Cloud Messaging を使用するときに発生する可能性があります。

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp が初期化されていません

場合によっては、このエラー メッセージを表示可能性があります。

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

これは既知の問題、ソリューションをクリーニングし、プロジェクトを再構築を回避することです (**ビルド > ソリューションのクリーン**、**ビルド > ソリューションのリビルド**)。

## <a name="summary"></a>まとめ

このチュートリアルでは、Xamarin.Android アプリケーションでリモート通知の Firebase Cloud Messaging を実装するための手順が詳しく説明します。 FCM の通信に必要な必要なパッケージをインストールする方法が説明されているし、FCM サーバーにアクセスする Android マニフェストを構成する方法について説明しました。 Google play 開発者サービスの有無を確認する方法を示したサンプル コードを提供します。 登録トークンの場合は、FCM とネゴシエートし、インスタンス ID リスナー サービスを実装する方法について説明し、アプリが backgrounded 中に、このコードがバック グラウンド通知を作成する方法について説明しました。 トピックのメッセージをサブスクライブする方法を説明し、受信して、アプリがフォア グラウンドで実行中に、リモート通知を表示するために使用するメッセージ リスナー サービスの実装例を提供します。


## <a name="related-links"></a>関連リンク

- [FCMNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [FCM メッセージについて](https://firebase.google.com/docs/cloud-messaging/concept-options)
