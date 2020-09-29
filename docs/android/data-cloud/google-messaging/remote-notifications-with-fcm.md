---
title: Firebase クラウド メッセージングによるリモート通知
description: このチュートリアルでは、アドインアプリケーションで、焼討 Base Cloud Messaging を使用してリモート通知 (プッシュ通知とも呼ばれます) を実装する方法を順を追って説明します。 ここでは、焼討 Base Cloud Messaging (FCM) との通信に必要なさまざまなクラスを実装する方法を示し、FCM にアクセスするために Android マニフェストを構成する方法の例を示し、焼討 Base コンソールを使用して下流のメッセージングをデモンストレーションします。
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/31/2018
ms.openlocfilehash: 702ca70e220d8e4d28a1a2ddc6be40daae052d58
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456045"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Firebase クラウド メッセージングによるリモート通知

_このチュートリアルでは、アドインアプリケーションで、焼討 Base Cloud Messaging を使用してリモート通知 (プッシュ通知とも呼ばれます) を実装する方法を順を追って説明します。ここでは、焼討 Base Cloud Messaging (FCM) との通信に必要なさまざまなクラスを実装する方法を示し、FCM にアクセスするために Android マニフェストを構成する方法の例を示し、焼討 Base コンソールを使用して下流のメッセージングをデモンストレーションします。_

## <a name="fcm-notifications-overview"></a>FCM 通知の概要

このチュートリアルでは、FCM メッセージングの要点を示すために、 **Fcmclient** と呼ばれる基本的なアプリを作成します。 **Fcmclient** は、Google Play 開発者サービスの存在を確認し、fcm から登録トークンを受け取り、焼討 base コンソールから送信するリモート通知を表示して、トピックメッセージをサブスクライブします。

[![アプリのスクリーンショットの例](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

次のトピックについて詳しく説明します。

1. バックグラウンド通知

2. トピックメッセージ

3. フォアグラウンド通知

このチュートリアルでは、 **Fcmclient** に機能を段階的に追加し、それをデバイスまたはエミュレーターで実行して、fcm との対話方法を理解します。 ログ記録を使用して、FCM サーバーでライブアプリトランザクションを監視します。また、FCM メッセージからの通知の生成方法については、「焼討 Base Console notification GUI」に入力します。

## <a name="requirements"></a>必要条件

これは、焼討 Base Cloud Messaging から送信できる [さまざまな種類のメッセージ](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) を理解するのに役立ちます。 メッセージのペイロードによって、クライアントアプリがメッセージを受信して処理する方法が決まります。

このチュートリアルを続行するには、Google の FCM サーバーを使用するために必要な資格情報を取得する必要があります。このプロセスについては、「 [焼討 Base Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm)」で説明されています。
特に、このチュートリアルで示されている例のコードで使用するために、ファイル ** のgoogle-services.js** をダウンロードする必要があります。 まだ焼討ベースコンソールでプロジェクトを作成していない場合 (またはファイル ** にgoogle-services.js** をまだダウンロードしていない場合) は、「 [焼討 base Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)」を参照してください。

このサンプルアプリを実行するには、互換性が付属している Android テストデバイスまたはエミュレーターが必要です。 焼討 base Cloud Messaging では、Android 4.0 以降で実行されているクライアントがサポートされています。また、これらのデバイスには Google Play ストアアプリもインストールする必要があります (Google Play 開発者サービス9.2.1 以降が必要です)。 デバイスに Google Play ストアアプリをまだインストールしていない場合は、 [Google Play](https://support.google.com/googleplay) の web サイトにアクセスしてダウンロードし、インストールします。 または、テストデバイスではなく Google Play 開発者サービスをインストールした Android SDK エミュレーターを使用することもできます (Android SDK エミュレーターを使用している場合は、Google Play ストアをインストールする必要はありません)。

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

まず、 **Fcmclient**という名前の新しい空の Xamarin Android プロジェクトを作成します。 Xamarin Android プロジェクトの作成に慣れていない場合は、「 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md)」を参照してください。
新しいアプリが作成されたら、次の手順として、パッケージ名を設定し、FCM との通信に使用されるいくつかの NuGet パッケージをインストールします。

### <a name="set-the-package-name"></a>パッケージ名を設定する

「 [焼討 Base Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)」で、fcm 対応アプリのパッケージ名を指定しました。 このパッケージ名は、 [API キー](firebase-cloud-messaging.md#fcm-in-action-api-key)に関連付けられている[*アプリケーション ID*](./firebase-cloud-messaging.md#fcm-in-action-app-id)としても機能します。 このパッケージ名を使用するようにアプリを構成します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **Fcmclient**プロジェクトのプロパティを開きます。

2. [ **Android マニフェスト** ] ページで、パッケージ名を設定します。

次の例では、パッケージ名がに設定されてい `com.xamarin.fcmexample` ます。

[![パッケージ名の設定](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

**Android マニフェスト**を更新するときに、 `Internet` アクセス許可が有効になっていることも確認してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **Fcmclient**プロジェクトのプロパティを開きます。

2. [ **Android アプリケーション** ] ページで、パッケージ名を設定します。

次の例では、パッケージ名がに設定されてい `com.xamarin.fcmexample` ます。

[![パッケージ名の設定](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

**Android マニフェスト**を更新するときに、アクセス許可が有効になっていることを確認し `INTERNET` ます ([**必要なアクセス許可**] の下)。

-----

> [!IMPORTANT]
> このパッケージ名が、焼討 Base コンソールに入力されたパッケージ名と *完全* に一致しない場合、クライアントアプリは fcm から登録トークンを受け取ることができません。

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play 開発者サービス基本パッケージを追加する

焼討 Base Cloud Messaging は Google Play 開発者サービスに依存しているため、xamarin [Google Play 開発者サービス Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) NuGet パッケージを Xamarin. Android プロジェクトに追加する必要があります。 バージョン29.0.0.2 以降が必要になります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio で、[参照] を右クリックし **> [NuGet パッケージの管理**] をクリックします。

2. [ **参照** ] タブをクリックし、 **GooglePlayServices**を検索します。

3. このパッケージを **Fcmclient** プロジェクトにインストールします。

    [![Google Play 開発者サービスベースのインストール](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で、[パッケージ] を右クリックし **> [パッケージの追加...**] をクリックします。

2. **Xamarin.GooglePlayServices.Base** を検索します。

3. このパッケージを **Fcmclient** プロジェクトにインストールします。

    [![Google Play 開発者サービスベースのインストール](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet のインストール中にエラーが発生した場合は、 **Fcmclient** プロジェクトを閉じて再度開いてから、nuget のインストールを再試行してください。

**GooglePlayServices**をインストールすると、必要なすべての依存関係もインストールされます。 **MainActivity.cs**を編集し、次のステートメントを追加し `using` ます。

```csharp
using Android.Gms.Common;
```

このステートメントにより、 `GoogleApiAvailability` **GooglePlayServices** 内のクラスが **fcmclient** コードで使用できるようになります。
`GoogleApiAvailability` は Google Play 開発者サービスの存在を確認するために使用されます。

### <a name="add-the-xamarin-firebase-messaging-package"></a>Xamarin 焼討 Base メッセージングパッケージを追加する

FCM からメッセージを受信するには、 [Xamarin 焼討 Base Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet パッケージをアプリケーションプロジェクトに追加する必要があります。 このパッケージがないと、Android アプリケーションは FCM サーバーからメッセージを受信できません。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio で、[参照] を右クリックし **> [NuGet パッケージの管理**] をクリックします。

2. 「 **Xamarin. メッセージ**」を検索します。

3. このパッケージを **Fcmclient** プロジェクトにインストールします。

    [![Xamarin 焼討 Base メッセージングのインストール](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で、[パッケージ] を右クリックし **> [パッケージの追加...**] をクリックします。

2. 「 **Xamarin. メッセージ**」を検索します。

3. このパッケージを **Fcmclient** プロジェクトにインストールします。

    [![Xamarin 焼討 Base メッセージングのインストール](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

**Xamarin. Messaging**をインストールすると、必要なすべての依存関係もインストールされます。

次に、 **MainActivity.cs** を編集し、次のステートメントを追加し `using` ます。

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

最初の2つのステートメントは、 **Xamarin.** の NuGet パッケージの型を **fcmclient** コードで使用できるようにします。 **Android. Util** では、FMS でトランザクションを観察するために使用されるログ機能が追加されます。

### <a name="add-the-google-services-json-file"></a><a name="add-googleplayservices-json"></a>Google Services JSON ファイルを追加する

次の手順では、ファイルの **google-services.js** をプロジェクトのルートディレクトリに追加します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **google-services.js**をプロジェクトフォルダーにコピーします。

2. アプリプロジェクトに**google-services.js**を追加します (**ソリューションエクスプローラー**で [すべての**ファイルを表示**] をクリックし、[google-services.js] を右クリックし**て**、[**プロジェクトに含める**] を選択します)。

3. **ソリューション エクスプローラー** ウィンドウで、**[google-services.json]** を選択します。

4. [ **プロパティ** ] ペインで、[ **ビルド] アクション** を [  **GoogleServicesJson**] に設定します。

    [![ビルドアクションを GoogleServicesJson に設定する](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > **GoogleServicesJson**ビルドアクションが表示されない場合は、ソリューションを保存して閉じ、再度開きます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **google-services.js**をプロジェクトフォルダーにコピーします。

2. アプリプロジェクトに **google-services.jsを** 追加します。

3. **google-services.js**右クリックします。

4. **ビルドアクション**を**GoogleServicesJson**に設定します。

    [![ビルドアクションを GoogleServicesJson に設定する](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

**google-services.json**がプロジェクトに追加され ( **GoogleServicesJson**ビルドアクションが設定されている場合)、ビルドプロセスはクライアント ID と[API キー](./firebase-cloud-messaging.md#fcm-in-action-api-key)を抽出し、 **obj/Debug/android/AndroidManifest.xml**に存在するマージ/生成された**AndroidManifest.xml**にこれらの資格情報を追加します。 このマージプロセスでは、FCM サーバーへの接続に必要なすべてのアクセス許可とその他の FCM 要素が自動的に追加されます。

## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Google Play 開発者サービスを確認し、通知チャネルを作成します

Google では、Google Play 開発者サービス機能にアクセスする前に、Android アプリで APK Google Play 開発者サービスが存在するかどうかを確認することをお勧めします (詳細については、「 [Google Play サービスの確認](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)」を参照してください)。

最初に、アプリの UI の初期レイアウトが作成されます。 **Resources/layout/Main**を編集し、その内容を次の xml に置き換えます。

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

これ `TextView` は Google Play 開発者サービスがインストールされているかどうかを示すメッセージを表示するために使用されます。 変更内容を **メインの axml**に保存します。

**MainActivity.cs**を編集し、次のインスタンス変数をクラスに追加し `MainActivity` ます。

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

これらの変数とは、 `CHANNEL_ID` `NOTIFICATION_ID` [`CreateNotificationChannel`](#create-notification-channel-code) このチュートリアルの後半で追加されるメソッドで使用され `MainActivity` ます。

次の例では、 `OnCreate` メソッドは、アプリが FCM サービスを使用しようとする前に Google Play 開発者サービスが使用可能であることを確認します。
次のメソッドを `MainActivity` クラスに追加します。

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

このコードは、Google Play 開発者サービス APK がインストールされているかどうかをデバイスで確認します。 インストールされていない場合は、 `TextBox` Google Play ストアから APK をダウンロードするようにユーザーに指示するメッセージ (または、デバイスのシステム設定で有効にする) が表示されます。

<a name="create-notification-channel-code"></a>Android 8.0 (API レベル 26) 以降で実行されているアプリでは、通知を発行するための [_通知チャネル_](~/android/app-fundamentals/notifications/local-notifications.md) を作成する必要があります。  通知チャネルを作成するクラスに次のメソッドを追加し `MainActivity` ます (必要な場合)。

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

`OnCreate` メソッドを次のコードに置き換えます。

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

`IsPlayServicesAvailable` は、 `OnCreate` アプリが起動するたびに Google Play 開発者サービスチェックが実行されるように、の末尾で呼び出されます。 `CreateNotificationChannel`Android 8 以降を実行しているデバイスの通知チャネルが存在することを確認するために、メソッドが呼び出されます。 アプリにメソッドがある場合は `OnResume` 、からもを呼び出す必要があり `IsPlayServicesAvailable` `OnResume` ます。 アプリを完全にリビルドして実行します。 すべてが正しく構成されている場合は、次のスクリーンショットのような画面が表示されます。

[![アプリは Google Play 開発者サービスが使用可能であることを示します](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

この結果が得られない場合は、Google Play 開発者サービス APK がデバイスにインストールされていることを確認します (詳細については、「 [Google Play 開発者サービスのセットアップ](https://developers.google.com/android/guides/setup)」を参照してください)。
また、前に説明したように、 **Fcmclient**プロジェクトに**Xamarin. Google. service.** .............................

## <a name="add-the-instance-id-receiver"></a>インスタンス ID レシーバーを追加する

次の手順では、を拡張するサービスを追加して、 `FirebaseInstanceIdService` [焼討基本登録トークン](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token)の作成、ローテーション、および更新を処理します。 サービスは、 `FirebaseInstanceIdService` FCM がデバイスにメッセージを送信できるようにするために必要です。 `FirebaseInstanceIdService`サービスがクライアントアプリに追加されると、アプリは自動的に FCM メッセージを受信し、アプリが backgrounded されるたびに通知として表示されます。

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android マニフェストで受信者を宣言する

**AndroidManifest.xml**を編集し、セクションに次の要素を挿入し `<receiver>` `<application>` ます。

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

この XML は、次のことを行います。

- `FirebaseInstanceIdReceiver`各アプリインスタンスの一意の[識別子](https://developers.google.com/instance-id/)を提供する実装を宣言します。 また、この受信者はアクションを認証および承認します。

- `FirebaseInstanceIdInternalReceiver`サービスを安全に開始するために使用される内部実装を宣言します。

- [アプリ ID](./firebase-cloud-messaging.md#fcm-in-action-app-id)は、[プロジェクトに追加](#add-googleplayservices-json)されたファイルの**google-services.js**に格納されます。 Xamarin の消火ベースのバインドでは、トークンはアプリ ID に置き換えられます。 `${applicationId}` アプリ id を提供するために、クライアントアプリで追加のコードは必要ありません。

は、 `FirebaseInstanceIdReceiver` イベントとイベントを受け取り、 `WakefulBroadcastReceiver` `FirebaseInstanceId` `FirebaseMessaging` 派生元のクラスに配信するです `FirebaseInstanceIdService` 。

### <a name="implement-the-firebase-instance-id-service"></a>焼討 Base インスタンス ID サービスを実装する

アプリケーションを FCM に登録する作業は、指定したカスタムサービスによって処理され `FirebaseInstanceIdService` ます。
`FirebaseInstanceIdService` では、次の手順を実行します。

1. [インスタンス ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID)を使用して、クライアントアプリが fcm とアプリサーバーにアクセスすることを承認するセキュリティトークンを生成します。 返されると、アプリは FCM から [登録トークン](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) を取得します。

2. アプリサーバーで登録トークンが必要な場合は、アプリサーバーに登録トークンを転送します。

**MyFirebaseIIDService.cs**という名前の新しいファイルを追加し、テンプレートコードを次のコードに置き換えます。

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

このサービスは `OnTokenRefresh` 、登録トークンが最初に作成または変更されたときに呼び出されるメソッドを実装します。 `OnTokenRefresh`を実行すると、プロパティから最新のトークンが取得され `FirebaseInstanceId.Instance.Token` ます (fcm によって非同期的に更新されます)。 この例では、更新されたトークンがログに記録されるため、出力ウィンドウで表示できます。

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` 次のような場合にトークンを更新するために使用されます。

- アプリがインストールまたはアンインストールされたとき。

- ユーザーがアプリデータを削除したとき。

- アプリがインスタンス ID を消去するとき。

- トークンのセキュリティが侵害された場合。

Google の [インスタンス id](https://developers.google.com/instance-id/guides/android-implementation) のドキュメントに従って、Fcm instance id サービスは、アプリがトークンを定期的に更新するように要求します (通常は6か月ごと)。

`OnTokenRefresh` また、は `SendRegistrationToAppServer` を呼び出して、アプリケーションによって管理されているサーバー側アカウント (存在する場合) にユーザーの登録トークンを関連付けます。

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

この実装はアプリサーバーの設計に依存しているため、この例では空のメソッド本体が用意されています。 アプリサーバーで FCM 登録情報が必要な場合は、を変更して、 `SendRegistrationToAppServer` ユーザーの fcm インスタンス ID トークンを、アプリで管理されている任意のサーバー側アカウントに関連付けます。 (トークンはクライアントアプリに対して非透過的であることに注意してください)。

トークンがアプリサーバーに送信されるとき、は、 `SendRegistrationToAppServer` トークンがサーバーに送信されたかどうかを示すブール値を保持する必要があります。 このブール値が false の場合、は `SendRegistrationToAppServer` アプリサーバーにトークンを送信し &ndash; ます。それ以外の場合、トークンは既に以前の呼び出しでアプリサーバーに送信されています。 場合によっては (この `FCMClient` 例など)、アプリサーバーはトークンを必要としないため、この例ではこの方法は必要ありません。

## <a name="implement-client-app-code"></a>クライアントアプリコードの実装

受信側サービスが配置されたので、クライアントアプリコードを記述してこれらのサービスを利用できます。 次のセクションでは、登録トークン ( *インスタンス ID トークン*とも呼ばれます) をログに記録するためのボタンが UI に追加され、 `MainActivity` `Intent` アプリが通知から起動されたときに情報を表示するためのコードがに追加されています。

[![アプリ画面に追加された [ログトークン] ボタン](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>ログトークン

この手順で追加したコードは、デモンストレーションのみを目的としてい &ndash; ます。実稼働クライアントアプリでは、登録トークンをログに記録する必要がありません。 **Resources/layout/Main**を編集し、要素の直後に次の宣言を追加し `Button` `TextView` ます。

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

このコードは、[ **ログトークン** ] ボタンがタップされたときに、現在のトークンを [出力] ウィンドウに記録します。

### <a name="handle-notification-intents"></a>通知インテントを処理する

ユーザーが **Fcmclient**から発行された通知をタップすると、その通知メッセージに付随するデータはすべて、エクストラで使用できるようになり `Intent` ます。 **MainActivity.cs**を編集し、メソッドの先頭 `OnCreate` (の呼び出しの前) に次のコードを追加し `IsPlayServicesAvailable` ます。

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

`Intent`ユーザーが通知メッセージをタップすると、アプリのランチャーが発生します。そのため、このコードでは、に含まれるすべてのデータが `Intent` 出力ウィンドウに記録されます。 別のを起動する必要がある場合は `Intent` 、 `click_action` 通知メッセージのフィールドをそのフィールドに設定する必要があり `Intent` ます (が指定されて `Intent` いない場合はランチャーが使用され `click_action` ます)。

## <a name="background-notifications"></a>バックグラウンド通知

**Fcmclient**アプリをビルドして実行します。 [ **Log Token** ] ボタンが表示されます。

[![[ログトークン] ボタンが表示されます](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

[ **ログトークン** ] ボタンをタップします。 IDE の出力ウィンドウに次のようなメッセージが表示されます。

[![インスタンス ID トークンが出力ウィンドウに表示されます](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

**トークン**でラベル付けされた長い文字列は、焼討 base コンソールに貼り付けるインスタンス ID トークンです &ndash; 。この文字列を選択してクリップボードにコピーします。 インスタンス ID トークンが表示されない場合は、メソッドの先頭に次の行を追加して、 `OnCreate` ** のgoogle-services.js** が正しく解析されたことを確認します。

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

出力ウィンドウに記録された値はgoogle-services.jsに記録された `google_app_id` 値と一致している必要があり `mobilesdk_app_id` ます。 **google-services.json**

### <a name="send-a-message"></a>メッセージの送信

[焼討 Base コンソール](https://console.firebase.google.com)にサインインし、プロジェクトを選択して [**通知**] をクリックし、[**最初のメッセージを送信する**] をクリックします。

[![最初のメッセージを送信するボタン](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

[ **メッセージの作成** ] ページで、メッセージテキストを入力し、[ **単一デバイス**] を選択します。 IDE 出力ウィンドウからインスタンス ID トークンをコピーし、焼討 Base コンソールの **Fcm 登録トークン** フィールドに貼り付けます。

[![メッセージの作成ダイアログ](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android デバイス (またはエミュレーター) で、[Android の **概要** ] ボタンをタップしてホーム画面に触れることによって、アプリの背景を表示します。 デバイスの準備ができたら、焼討 Base コンソールで [ **メッセージの送信** ] をクリックします。

[![[メッセージの送信] ボタン](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

[ **メッセージの確認** ] ダイアログが表示されたら、[ **送信**] をクリックします。
通知アイコンは、デバイス (またはエミュレーター) の通知領域に表示されます。

[![通知アイコンが表示されます](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

通知アイコンを開いてメッセージを表示します。 通知メッセージは、次のように、焼討 Base コンソールの **メッセージテキスト** フィールドに入力されたものと同じである必要があります。

[![デバイスに通知メッセージが表示される](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

通知アイコンをタップして **Fcmclient** アプリを起動します。 `Intent` **Fcmclient**に送信されたすべての内容が、IDE の出力ウィンドウに表示されます。

[![キー、メッセージ ID、および折りたたみキーからのインテントリスト](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

この例では、 **from** キーがアプリの焼討 base プロジェクト番号 (この例では) に設定され、 `41590732` **collapse_key** がパッケージ名 (**com. xamarin. fcmexample**) に設定されています。
メッセージが表示されない場合は、デバイス (またはエミュレーター) で **Fcmclient** アプリを削除してみて、上記の手順を繰り返します。

> [!NOTE]
> アプリを強制的に閉じると、FCM によって通知の配信が停止されます。 Android では、停止したアプリケーションのコンポーネントが誤って、または不必要に起動されるのを防ぐことができます。 (この動作の詳細については、「停止した [アプリケーションでのコントロールの起動](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)」を参照してください)。このため、アプリを実行するたびに手動でアンインストールし、デバッグセッションから停止する必要が &ndash; あります。これにより、FCM によって新しいトークンが生成され、メッセージが引き続き受信されるようになります。

### <a name="add-a-custom-default-notification-icon"></a>カスタムの既定の通知アイコンを追加する

前の例では、通知アイコンはアプリケーションアイコンに設定されています。 次の XML は、通知用のカスタムの既定のアイコンを構成します。 Android では、通知アイコンが明示的に設定されていないすべての通知メッセージに対して、このカスタムの既定のアイコンが表示されます。

カスタムの既定の通知アイコンを追加するには、 **リソース/** 作成したディレクトリにアイコンを追加し、 **AndroidManifest.xml**を編集して、セクションに次の要素を挿入し `<meta-data>` `<application>` ます。

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

この例では、 **Resources/描画可能/ic \_ stat \_ \_notification.png** にある通知アイコンが、カスタムの既定の通知アイコンとして使用されます。 **AndroidManifest.xml**でカスタムの既定のアイコンが構成されておらず、通知ペイロードでアイコンが設定されていない場合、Android は通知アイコンとしてアプリケーションアイコンを使用します (上の通知アイコンのスクリーンショットを参照)。

## <a name="handle-topic-messages"></a>トピックメッセージを処理する

これまでに記述したコードは、登録トークンを処理し、リモート通知機能をアプリに追加します。 次の例では、 *トピックメッセージ* をリッスンし、リモート通知としてユーザーに転送するコードを追加します。 トピックメッセージは、特定のトピックをサブスクライブする1つ以上のデバイスに送信される FCM メッセージです。 トピックメッセージの詳細については、「 [トピックメッセージング](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)」を参照してください。

### <a name="subscribe-to-a-topic"></a>トピックのサブスクライブ

**Resources/layout/Main**を編集し、前の要素の直後に次の宣言を追加し `Button` `Button` ます。

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

この XML は、[ **通知の購読** ] ボタンをレイアウトに追加します。
**MainActivity.cs**を編集し、メソッドの末尾に次のコードを追加し `OnCreate` ます。

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

このコードでは、レイアウトの [ **Notification へのサブスクライブ**] ボタンを探し、そのクリックハンドラーを呼び出して、サブスクライブしたトピックである news を渡しているコードに割り当て `FirebaseMessaging.Instance.SubscribeToTopic` ます_news_。 ユーザーが [ **サブスクライブ** ] ボタンをタップすると、アプリは _ニュース_ トピックをサブスクライブします。 次のセクションでは、アドインのコンソール通知 GUI から _ニュース_ トピックメッセージが送信されます。

### <a name="send-a-topic-message"></a>トピックメッセージを送信する

アプリをアンインストールしてリビルドし、もう一度実行してください。 [ **通知の登録** ] ボタンをクリックします。

[![[通知の購読] ボタン](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

アプリが正常にサブスクライブしている場合は、IDE の出力ウィンドウに [ **同期は成功しまし** た] と表示されます。

[![出力ウィンドウにトピック「同期が成功しました」メッセージが表示されます](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

トピックメッセージを送信するには、次の手順に従います。

1. [焼討 Base] コンソールで、[ **新しいメッセージ**] をクリックします。

2. [ **メッセージの作成** ] ページで、メッセージテキストを入力し、[  **トピック**] を選択します。

3. **トピック**プルダウンメニューで、組み込みトピック [ **news**] を選択します。

    [![ニューストピックを選択する](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4. Android デバイス (またはエミュレーター) で、[Android の **概要** ] ボタンをタップしてホーム画面に触れることによって、アプリの背景を表示します。

5. デバイスの準備ができたら、焼討 Base コンソールで [ **メッセージの送信** ] をクリックします。

6. IDE の出力ウィンドウで、ログ出力の次の **トピック** を確認します。

    [![/トピック/ニュースからのメッセージが表示されます。](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

このメッセージが [出力] ウィンドウに表示された場合は、Android デバイスの通知領域に通知アイコンも表示されます。 通知アイコンを開いて、トピックメッセージを表示します。

[![トピックメッセージは通知として表示されます。](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

メッセージが表示されない場合は、デバイス (またはエミュレーター) で **Fcmclient** アプリを削除してみて、上記の手順を繰り返します。

## <a name="foreground-notifications"></a>フォアグラウンド通知

事前に接地したアプリで通知を受信するには、を実装する必要があり `FirebaseMessagingService` ます。 このサービスは、データペイロードを受信したり、上流のメッセージを送信したりするためにも必要です。 次の例は、生成されたアプリを拡張するサービスを実装して `FirebaseMessagingService` &ndash; 、フォアグラウンドで実行中のリモート通知を処理できるようにする方法を示しています。

### <a name="implement-firebasemessagingservice"></a>焼討 Basemessagingservice を実装する

`FirebaseMessagingService`サービスは、消火ベースからのメッセージの受信と処理を担当します。 各アプリは、この型をサブクラス化し、をオーバーライドして `OnMessageReceived` 受信メッセージを処理する必要があります。 アプリがフォアグラウンドにある場合、 `OnMessageReceived` コールバックは常にメッセージを処理します。

> [!NOTE]
> アプリでは、受信した焼討 Base Cloud メッセージを処理するのに10秒しかありません。 これよりも時間がかかる作業は、 [Android ジョブスケジューラ](~/android/platform/android-job-scheduler.md) や [焼討ベースジョブディスパッチャー](~/android/platform/firebase-job-dispatcher.md)などのライブラリを使用してバックグラウンドで実行するようにスケジュールする必要があります。

**MyFirebaseMessagingService.cs**という名前の新しいファイルを追加し、テンプレートコードを次のコードに置き換えます。

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

`MESSAGING_EVENT`新しい FCM メッセージが次のように送信されるように、インテントフィルターを宣言する必要があることに注意してください `MyFirebaseMessagingService` 。

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

クライアントアプリが FCM からメッセージを受信すると、は、 `OnMessageReceived` `RemoteMessage` メソッドを呼び出すことによって、渡されたオブジェクトからメッセージの内容を抽出し `GetNotification` ます。 次に、メッセージの内容をログに記録して、IDE の出力ウィンドウで表示できるようにします。

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> にブレークポイントを設定した場合 `FirebaseMessagingService` 、FCM によってメッセージが配信されるため、デバッグセッションでこれらのブレークポイントにヒットするかどうかを確認できます。

### <a name="send-another-message"></a>別のメッセージを送信する

アプリをアンインストールし、再構築して再実行し、次の手順に従って別のメッセージを送信します。

1. [焼討 Base] コンソールで、[ **新しいメッセージ**] をクリックします。

2. [ **メッセージの作成** ] ページで、メッセージテキストを入力し、[  **単一デバイス**] を選択します。

3. IDE 出力ウィンドウからトークン文字列をコピーし、前と同じように、焼討 Base コンソールの **Fcm 登録トークン** フィールドに貼り付けます。

4. アプリがフォアグラウンドで実行されていることを確認し、次に、焼討 Base コンソールで [ **メッセージの送信** ] をクリックします。

    [![コンソールからの別のメッセージの送信](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5. [ **メッセージの確認** ] ダイアログが表示されたら、[ **送信**] をクリックします。

6. 受信メッセージは、IDE の出力ウィンドウに記録されます。

    [![出力ウィンドウに出力されるメッセージ本文](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)

### <a name="add-a-local-notification-sender"></a>ローカル通知の送信者を追加する

この残りの例では、受信した FCM メッセージは、アプリがフォアグラウンドで実行されている間に起動されるローカル通知に変換されます。 **MyFirebaseMessageService.cs**を編集し、次のステートメントを追加し `using` ます。

```csharp
using FCMClient;
using System.Collections.Generic;
```

次のメソッドを `MyFirebaseMessagingService` に追加します。

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

この通知をバックグラウンド通知から区別するために、このコードは、アプリケーションアイコンとは異なるアイコンを使用して通知をマークします。 ファイル [ic \_ stat \_ Ic \_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) を **Resources/アブル** に追加し、 **fcmclient** プロジェクトに含めます。

この `SendNotification` メソッドは、を使用して `NotificationCompat.Builder` 通知を作成し、 `NotificationManagerCompat` 通知を開始するために使用されます。 通知は、 `PendingIntent` ユーザーがアプリを開いて、に渡された文字列の内容を表示できるようにするを保持し `messageBody` ます。 の詳細について `NotificationCompat.Builder` は、「 [ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」を参照してください。

`SendNotification`メソッドの最後にメソッドを呼び出し `OnMessageReceived` ます。

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

これらの変更の結果として、 `SendNotification` は、アプリがフォアグラウンドにある間に通知が受信されるたびに実行され、通知領域に通知が表示されます。

アプリがバックグラウンドで実行されている場合、メッセージの処理方法は [メッセージのペイロード](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) によって決まります。

- **通知** &ndash; メッセージは **システムトレイ**に送信されます。 ローカル通知が表示されます。 ユーザーが通知をタップすると、アプリが起動します。
- **データ** &ndash; メッセージはによって処理され `OnMessageReceived` ます。
- **両方** &ndash; 通知とデータペイロードの両方を含むメッセージがシステムトレイに配信されます。 アプリが起動すると、 `Extras` `Intent` アプリを起動するために使用されたのにデータペイロードが表示されます。

この例では、アプリが backgrounded の場合、 `SendNotification` メッセージにデータペイロードがある場合はが実行されます。 それ以外の場合は、バックグラウンド通知 (このチュートリアルで既に説明したもの) が起動されます。

### <a name="send-the-last-message"></a>最後のメッセージを送信する

アプリをアンインストールして再構築し、もう一度実行してから、次の手順を使用して最後のメッセージを送信します。

1. [焼討 Base] コンソールで、[ **新しいメッセージ**] をクリックします。

2. [ **メッセージの作成** ] ページで、メッセージテキストを入力し、[ **単一デバイス**] を選択します。

3. IDE 出力ウィンドウからトークン文字列をコピーし、前と同じように、焼討 Base コンソールの **Fcm 登録トークン** フィールドに貼り付けます。

4. アプリがフォアグラウンドで実行されていることを確認し、次に、焼討 Base コンソールで [ **メッセージの送信** ] をクリックします。

    [![フォアグラウンドメッセージの送信](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

今回は、出力ウィンドウに記録されたメッセージも新しい通知にパッケージ化されてい &ndash; ます。アプリがフォアグラウンドで実行されている間、通知アイコンが通知トレイに表示されます。

[![フォアグラウンドメッセージの通知アイコン](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

通知を開くと、焼討 Base コンソール通知 GUI から送信された最後のメッセージが表示されます。

[![前景の通知を前面に表示するアイコン](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)

## <a name="disconnecting-from-fcm"></a>FCM からの切断

トピックの購読を解除するには、[焼討 Basemessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging)クラスで[UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29)メソッドを呼び出します。 たとえば、前の手順で購読した _ニュース_ トピックからサブスクライブを解除するために、次のハンドラーコードを使用して、登録 **解除** ボタンをレイアウトに追加することができます。

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

FCM からデバイスの登録を解除するには、[消火 baseinstanceid](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId)クラスの[deleteinstanceid](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29)メソッドを呼び出して、インスタンス ID を削除します。 次に例を示します。

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

このメソッドを呼び出すと、インスタンス ID とそれに関連付けられているデータが削除されます。 その結果、デバイスへの FCM データの定期的な送信が停止されます。

## <a name="troubleshooting"></a>トラブルシューティング

次に、Xamarin. Android での焼討 Base Cloud Messaging の使用時に発生する可能性がある問題と回避策について説明します。

### <a name="firebaseapp-is-not-initialized"></a>焼討 Baseapp は初期化されていません

場合によっては、次のエラーメッセージが表示されることがあります。

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

これは既知の問題であり、ソリューションをクリーニングしてプロジェクトをリビルドする (**ビルド > クリーンソリューション**を構築し、 **ソリューションをビルド > リビルド**する) ことで回避できます。

## <a name="summary"></a>まとめ

このチュートリアルでは、Xamarin Android アプリケーションでの焼討 Base Cloud Messaging リモート通知の実装手順について詳しく説明します。 ここでは、FCM 通信に必要なパッケージをインストールする方法と、FCM サーバーにアクセスするために Android マニフェストを構成する方法について説明しました。 Google Play 開発者サービスの存在を確認する方法を示すサンプルコードが用意されています。 この例では、登録トークンに対して FCM とネゴシエートするインスタンス ID リスナーサービスを実装する方法を説明しました。また、アプリの backgrounded 中にこのコードがバックグラウンド通知を作成する方法についても説明しました。 ここでは、トピックメッセージをサブスクライブする方法について説明し、アプリケーションがフォアグラウンドで実行されているときにリモート通知を受信して表示するために使用されるメッセージリスナーサービスの実装例を示しました。

## <a name="related-links"></a>関連リンク

- [FCMNotifications (サンプル)](/samples/xamarin/monodroid-samples/firebase-fcmnotifications)
- [Firebase クラウド メッセージング](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [FCM メッセージについて](https://firebase.google.com/docs/cloud-messaging/concept-options)