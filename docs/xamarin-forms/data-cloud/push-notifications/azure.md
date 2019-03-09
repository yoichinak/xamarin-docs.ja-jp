---
title: Azure Mobile Apps からプッシュ通知を送信します。
description: この記事では、Azure Notification Hubs を使用して Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信する方法について説明します。
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 92c068ceb3d382ed4612318dc987d950ec7e7ef2
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672548"
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Azure Mobile Apps からプッシュ通知を送信します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)

_Azure Notification Hubs は、バックエンドを異なるプラットフォーム通知システムと通信することの複雑さを排除しながら、任意のモバイル プラットフォームに任意のバックエンドからモバイル プッシュ通知を送信するため、スケーラブルなプッシュ インフラストラクチャを提供します。この記事では、Azure Notification Hubs を使用して Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信する方法について説明します。_

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure プッシュ通知ハブと、Xamarin.Forms で[Xamarin University](https://university.xamarin.com/)**

プッシュ通知を使用して、アプリケーションの engagement と使用率を向上させるには、モバイル デバイスでアプリケーションのバックエンド システムから、メッセージなどの情報を提供します。 ユーザーが対象となるアプリケーションを使用していない場合でも、通知の送信でいつでも、することができます。

バックエンド システムは、次の図に示すように、プラットフォーム通知システム (PNS) からのモバイル デバイスにプッシュ通知を送信します。

[![](azure-images/pns.png "プラットフォーム通知システム")](azure-images/pns-large.png#lightbox "プラットフォーム通知システム")

バックエンド システムをプッシュ通知を送信するには、クライアント アプリケーションのインスタンスに通知を送信するプラットフォーム固有 PNS にアクセスします。 これが大幅に向上、バックエンドの複雑なクロス プラットフォームのプッシュ通知が必要な場合は、ため、バックエンドは、各プラットフォーム固有 PNS API とプロトコルを使用する必要があります。

Azure Notification Hubs は、次の図に示すように、単一の API 呼び出しと共に送信されるクロスプラット フォーム通知を許可する、異なるプラットフォーム通知システムの詳細を抽象化によってこの複雑さを排除します。

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

プッシュ通知を送信するには、は、バックエンド システムのみ連絡先、Azure Notification Hub、さらに、異なるプラットフォーム通知システムと通信して、そのため、バックエンドの複雑性を減少は、その送信プッシュ通知を記述します。

Azure Mobile Apps では、notification hubs を使用してプッシュ通知の組み込みサポートがあります。 Azure Mobile Apps インスタンスから、Xamarin.Forms アプリケーションにプッシュ通知を送信するためのプロセスは次のとおりです。

1. Xamarin.Forms アプリケーションは、PNS ハンドルを返します。 これを登録します。
1. Azure Mobile Apps のインスタンスは、対象とするデバイスのハンドルを指定する、Azure Notification Hub に通知を送信します。
1. Azure Notification Hub では、適切なデバイスの PNS に通知を送信します。
1. PNS は、指定したデバイスに通知を送信します。
1. Xamarin.Forms アプリケーションは、通知を処理し、それを表示します。

サンプル アプリケーションでは、Azure Mobile Apps インスタンスにデータが格納されている todo リスト アプリケーションを示します。 新しい項目は、Azure Mobile Apps インスタンスに追加するたびに、Xamarin.Forms アプリケーションにプッシュ通知が送信されます。 次のスクリーン ショットでは、受信したプッシュ通知を表示する各プラットフォームを示します。

[![](azure-images/screenshots.png "サンプル アプリケーションのプッシュ通知を受信")](azure-images/screenshots-large.png#lightbox "サンプル アプリケーションのプッシュ通知の受信")

Azure Notification Hubs の詳細については、次を参照してください。 [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)と[Xamarin.Forms アプリにプッシュ通知を追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/)します。

## <a name="azure-and-platform-notification-system-setup"></a>Azure とプラットフォーム通知システムのセットアップ

Azure Mobile Apps インスタンスに Azure Notification Hub を統合するためのプロセスは次のとおりです。

1. Azure Mobile Apps インスタンスを作成します。 詳細については、[Azure Mobile App の使用](~/xamarin-forms/data-cloud/consuming/azure.md) を参照してください。
1. 通知ハブを構成します。 詳細については、次を参照してください。[通知ハブを構成](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-hub)します。
1. プッシュ通知を送信する Azure Mobile Apps のインスタンスを更新します。 詳細については、次を参照してください。[プッシュ通知を送信するサーバー プロジェクトを更新](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications)します。
1. 各 PNS に登録します。
1. 各 PNS との通信に、通知ハブを構成します。

次のセクションでは、各プラットフォームの追加のセットアップ手順についてを説明します。

### <a name="ios"></a>iOS

Apple Push Notification Service (APNS) から Azure Notification Hub を使用する次の手順を実行する必要があります。

1. キーチェーン アクセス ツールを使用してプッシュ証明書の要求の署名証明書を生成します。 詳細については、次を参照してください。[プッシュ通知証明書の証明書署名要求ファイルを生成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate)Azure ドキュメント センターにします。
1. Apple Developer Center でのプッシュ通知のサポート用の Xamarin.Forms アプリケーションを登録します。 詳細については、次を参照してください。[アプリ プッシュ通知を登録](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications)Azure ドキュメント センターにします。
1. Apple Developer Center でプッシュ通知が有効なプロビジョニング プロファイルの Xamarin.Forms アプリケーションを作成します。 詳細については、次を参照してください。[アプリのプロビジョニング プロファイルを作成する](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app)Azure ドキュメント センターにします。
1. APNS との通信に、通知ハブを構成します。 詳細については、次を参照してください。 [APNS 用通知ハブを構成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns)します。
1. 新しいアプリ ID とプロビジョニング プロファイルを使用する、Xamarin.Forms アプリケーションを構成します。 詳細については、次を参照してください。 [Xamarin Studio で iOS プロジェクトを構成する](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio)または[Visual Studio で iOS プロジェクトを構成する](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio)Azure ドキュメント センターにします。

### <a name="android"></a>Android

Firebase Cloud Messaging (FCM) から Azure Notification Hub を使用する、次の手順実行する必要があります。

1. FCM に登録します。 サーバーの API キーとクライアント ID は、自動的に生成されるとにパックされて、`google-services.json`ダウンロードされるファイル。 詳細については、次を参照してください。[を有効にする Firebase Cloud Messaging (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm)します。
1. FCM との通信に、通知ハブを構成します。 詳細については、次を参照してください。 [FCM を使用してプッシュ要求を送信するには、Mobile Apps バックエンド終了構成](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm)します。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次の手順は、Azure Notification Hub からの Windows 通知サービス (WNS) を使用する実行する必要があります。

1. Windows 通知サービス (WNS) に登録します。 詳細については、次を参照してください。 [WNS によるプッシュ通知用の Windows アプリを登録](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns)Azure ドキュメント センターにします。
1. WNS との通信に、通知ハブを構成します。 詳細については、次を参照してください。 [WNS 用通知ハブを構成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns)Azure ドキュメント センターにします。

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Xamarin.Forms アプリケーションにプッシュ通知のサポートを追加します。

次のセクションでは、各プラットフォーム固有プロジェクトでプッシュ通知をサポートするために必要な実装について説明します。

### <a name="ios"></a>iOS

IOS アプリケーションにプッシュ通知のサポートを実装するためのプロセスは次のとおりです。

1. 登録すると、Apple Push Notification Service (APNS) で、`AppDelegate.FinishedLaunching`メソッド。 詳細については、次を参照してください。 [Apple プッシュ通知システムに登録する](#ios_register)します。
1. 実装、`AppDelegate.RegisteredForRemoteNotifications`登録応答を処理するメソッド。 詳細については、次を参照してください。[登録応答の処理](#ios_registration_response)します。
1. 実装、`AppDelegate.DidReceiveRemoteNotification`受信したプッシュ通知を処理するメソッド。 詳細については、次を参照してください。[プッシュ通知の受信処理](#ios_process_incoming)します。

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Apple プッシュ通知サービスに登録します。

IOS アプリケーションがプッシュ通知を受信する前で、Apple Push Notification Service (APNS) は、一意のデバイス トークンを生成し、アプリケーションに戻すことに登録する必要があります。 登録が呼び出される、`FinishedLaunching`で上書き、`AppDelegate`クラス。

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

APNS で iOS アプリケーションの登録時に、受信するプッシュ通知の種類を指定する必要があります。 `RegisterUserNotificationSettings`メソッドは、アプリケーションが受け取ると、通知の種類を登録で、`RegisterForRemoteNotifications`を APNS からプッシュ通知を受信登録するメソッド。

> [!NOTE]
> 呼び出して、`RegisterUserNotificationSettings`メソッドは、アプリケーションによって受け取られるサイレント プッシュ通知になります。

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>登録応答の処理

APNS 登録要求は、バック グラウンドで発生します。 IOS を呼び出すが、応答が受信したときに、`RegisteredForRemoteNotifications`で上書き、`AppDelegate`クラス。

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

このメソッドは、JSON として単純な通知メッセージ テンプレートを作成し、通知ハブからテンプレート通知を受信するデバイスを登録します。

> [!NOTE]
> `FailedToRegisterForRemoteNotifications`ネットワーク接続がないなどの状況を処理するためにオーバーライドを実装する必要があります。 これは、ユーザーは、中に、アプリケーションを開始可能性がありますので、重要なはオフラインです。

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>受信したプッシュ通知の処理

`DidReceiveRemoteNotification`で上書き、`AppDelegate`クラスは、アプリケーションが実行されていると、通知を受信したときに呼び出される場合は、受信したプッシュ通知を処理するために使用します。

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

`userInfo`ディクショナリに含まれる、`aps`キー、値が、`alert`通知の残りのデータのディクショナリ。 このディクショナリを取得すると、使用、 `string`  ダイアログ ボックスに表示されている通知メッセージ。

> [!NOTE]
> プッシュ通知が到着すると、アプリケーションが実行されていない場合、アプリケーションは起動されますが、`DidReceiveRemoteNotification`メソッドは、通知を処理しません。 代わりに、通知ペイロードを取得してから適切に応答、`WillFinishLaunching`または`FinishedLaunching`よりも優先されます。

APNS の詳細については、次を参照してください。 [ios Push Notifications](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)します。

### <a name="android"></a>Android

Android アプリケーションにプッシュ通知のサポートを実装するためのプロセスは次のとおりです。

1. 追加、 [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet は、Android プロジェクトにパッケージ化し、Android 7.0 以上のアプリケーションのターゲット バージョンを設定します。
1. 追加、`google-services.json`ファイル、Android プロジェクトのルートに、Firebase コンソールからダウンロードされ、ビルド アクションを設定**GoogleServicesJson**します。 詳細については、次を参照してください。 [、Google Services JSON ファイルを追加](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)します。
1. Android マニフェストでの受信者を宣言することでレジスタ Firebase Cloud Messaging (FCM) でファイルを開き、実装によって、`FirebaseRegistrationService.OnTokenRefresh`メソッド。 詳細については、次を参照してください。 [Firebase Cloud Messaging と登録](#android_register_fcm)します。
1. Azure 通知ハブに登録、`AzureNotificationHubService.RegisterAsync`メソッド。 詳細については、次を参照してください。 [Azure 通知ハブへの登録](#android_register_azure)します。
1. 実装、`FirebaseNotificationService.OnMessageReceived`受信したプッシュ通知を処理するメソッド。 詳細については、次を参照してください。[プッシュ通知の内容を表示する](#android_displaying_notification)します。

Firebase Cloud Messaging の詳細については、次を参照してください。 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)と[Firebase Cloud Messaging を使用したリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)します。

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>登録 firebase Cloud Messaging

Android アプリケーションがプッシュ通知を受信する前に、FCM 登録トークンを生成し、アプリケーションに戻すことはこれが登録する必要があります。 登録トークンの詳細については、次を参照してください。 [FCM 登録](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration)します。

これは、によって実現されます。

- Android マニフェストでの受信者を宣言します。 詳細については、次を参照してください。 [Android マニフェストで、受信側を宣言する](#declaring_a_receiver)します。
- Firebase インスタンス ID サービスを実装します。 詳細については、次を参照してください。 [Firebase インスタンス ID サービスを実装する](#implementing-firebase-instance-id-service)します。

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Android マニフェストで、受信側の宣言

編集**AndroidManifest.xml**し、次の挿入`<receiver>`要素を`<application>`要素。

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

この XML は、次の操作を実行します。

- 内部宣言`FirebaseInstanceIdInternalReceiver`安全にサービスを開始するために使用する実装。
- 宣言を`FirebaseInstanceIdReceiver`アプリ インスタンスごとに一意の識別子を提供する実装。 この受信者は認証し、アクションを承認します。

`FirebaseInstanceIdReceiver`受信`FirebaseInstanceId`と`FirebaseMessaging`イベントから派生したクラスに配信および`FirebasesInstanceIdService`します。

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Firebase インスタンス ID サービスを実装します。

クラスを派生することによって実現されます FCM で、アプリケーションの登録、`FirebaseInstanceIdService`クラス。 このクラスは、FCM にアクセスするクライアント アプリケーションを認証するセキュリティ トークンを生成する責任を負います。 サンプル アプリケーションでは、`FirebaseRegistrationService`クラスから派生、`FirebaseInstanceIdService`クラスし、次のコード例に示します。

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

`OnTokenRefresh`アプリケーションが FCM から登録トークンを受け取ったときに、メソッドが呼び出されます。 メソッドからのトークンの取得、`FirebaseInstanceId.Instance.Token`プロパティは、FCM によって非同期に更新されます。 `OnTokenRefresh`メソッドが呼び出される頻度、またはアプリケーションをインストールまたはアンインストールすると、アプリケーション、インスタンス ID が消去されるときに、ユーザーがアプリケーションのデータを削除するときにのみ、トークンが更新されるため、トークンのセキュリティになったとき侵害されたとします。 さらに、あるアプリケーションのトークンに定期的に更新、通常 6 か月ごと、FCM インスタンス ID サービスを要求します。

`OnTokenRefresh`メソッドも呼び出して、`SendRegistrationTokenToAzureNotificationHub`メソッドは、Azure 通知ハブで、ユーザーの登録トークンを関連付けるために使用します。

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Azure Notification Hub に登録します。

`AzureNotificationHubService`クラスには、`RegisterAsync`メソッドで、ユーザーの登録トークンを Azure Notification Hub に関連付けます。 次のコード例は、`RegisterAsync`メソッドによって呼び出される、`FirebaseRegistrationService`クラスのユーザーの登録トークンが変更されたとき。

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

このメソッドは、Firebase 登録トークンを使用して、通知ハブからテンプレート通知を受信するには、JSON、およびレジスタとして単純な通知メッセージ テンプレートを作成します。 これにより、Azure Notification Hub から送信されたすべての通知が登録トークンによって表されるデバイスを対象とします。

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>プッシュ通知の内容を表示します。

クラスを派生することによって実現がプッシュ通知の内容を表示する、`FirebaseMessagingService`クラス。 このクラスが含まれていますが、オーバーライド可能な`OnMessageReceived`アプリケーションが FCM から通知を受け取ったときに呼び出され、メソッドが、アプリケーションがフォア グラウンドで実行されていることを提供します。 サンプル アプリケーションでは、`FirebaseNotificationService`クラスから派生、`FirebaseMessagingService`クラスし、次のコード例に示します。

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

アプリケーションが FCM から通知を受信するとき、`OnMessageReceived`メソッドは、メッセージのコンテンツを抽出し、呼び出し、`SendNotification`メソッド。 このメソッドは、メッセージの内容を通知領域に表示された通知で、アプリケーションの実行中に起動されたローカル通知に変換します。

##### <a name="handling-notification-intents"></a>通知のインテントの処理

通知メッセージに付属するすべてのデータで利用可能になって、ユーザーが通知をタップしたとき、 `Intent` extras します。 次のコードでは、このデータを抽出することができます。

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

アプリケーションのランチャー`Intent`ため、このコードに付属するデータをログイン、ユーザーがその通知メッセージをタップしたときに発生、`Intent`出力ウィンドウにします。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) の前にアプリケーションで、Windows 通知サービス (WNS)、通知チャネルが返されますを登録する必要があります、プッシュ通知を受信できます。 登録は、によって呼び出される、`InitNotificationsAsync`メソッドで、`App`クラス。

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

このメソッドは、プッシュ通知チャネルを取得、JSON として通知メッセージ テンプレートを作成し、通知ハブからテンプレート通知を受信するデバイスを登録します。

`InitNotificationsAsync`メソッドが呼び出されて、`OnLaunched`で上書き、`App`クラス。

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

これにより、プッシュ通知登録が作成または更新のたびに、アプリケーションを起動すると、そのため、WNS プッシュ チャネルが常にアクティブであることを確認します。

として自動的に表示されます、プッシュ通知を受信したときに、*トースト*– メッセージを含むモードレス ウィンドウです。

## <a name="summary"></a>まとめ

この記事では、Azure Notification Hubs を使用して Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信する方法を説明します。 Azure Notification Hubs は、バックエンドを異なるプラットフォーム通知システムと通信することの複雑さを排除しながら、任意のモバイル プラットフォームに任意のバックエンドからモバイル プッシュ通知を送信するため、スケーラブルなプッシュ インフラストラクチャを提供します。


## <a name="related-links"></a>関連リンク

- [Azure Mobile Apps の使](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Xamarin.Forms アプリにプッシュ通知を追加します。](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [IOS でプッシュ通知](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
