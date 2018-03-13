---
title: "Azure のモバイル アプリからプッシュ通知を送信します。"
description: "Azure Notification Hubs は、モバイル プッシュ通知を送信する任意のバックエンドから任意のモバイル プラットフォームでは、異なるプラットフォーム通知システムと通信すること、バックエンドの複雑さを排除しながら、スケーラブルなプッシュ インフラストラクチャを提供します。 この記事では、Azure Notification Hubs を使用して、Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: f0f767179a9280d7a6c6d7ce8125696d5e664cba
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Azure のモバイル アプリからプッシュ通知を送信します。

_Azure Notification Hubs は、モバイル プッシュ通知を送信する任意のバックエンドから任意のモバイル プラットフォームでは、異なるプラットフォーム通知システムと通信すること、バックエンドの複雑さを排除しながら、スケーラブルなプッシュ インフラストラクチャを提供します。この記事では、Azure Notification Hubs を使用して、Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信する方法について説明します。_

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure プッシュ通知ハブと Xamarin.Forms で[Xamarin 大学](https://university.xamarin.com/)**

プッシュ通知を使用して、アプリケーションによるエンゲージメントなど使用量を増やすには、モバイル デバイスにアプリケーションのバックエンド システムからメッセージなどの情報を提供します。 通知を送信できますでいつでも、ユーザーが対象のアプリケーションを使用していない場合でもです。

バックエンド システムは、次の図に示すように、Platform Notification Systems (PNS) 経由のモバイル デバイスにプッシュ通知を送信します。

[![](azure-images/pns.png "プラットフォーム通知システム")](azure-images/pns-large.png#lightbox "プラットフォーム通知システム")

プッシュ通知を送信するには、は、バックエンド システムは、クライアント アプリケーションのインスタンスに通知を送信するプラットフォーム固有の PNS を接続します。 これが著しく増加バックエンドの複雑なクロス プラットフォームのプッシュ通知に必要なときに、バックエンドは、各プラットフォームに応じた PNS API とプロトコルを使用する必要があるためです。

Azure Notification Hubs は、次の図に示すように、単一の API 呼び出しで送信されるクロスプラット フォームの通知を許可するが、異なるプラットフォーム通知システムの詳細を抽象化このような複雑さを排除します。

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

プッシュ通知を送信するにはするには、は、バックエンド システムのみ連絡先 Azure 通知ハブをさらに、異なるプラットフォーム通知システムと通信して、そのため、バックエンドの複雑さを削減は、その送信プッシュ通知を記述します。

Azure のモバイル アプリでは、notification hubs を使用してプッシュ通知の組み込みサポートがあります。 Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信するためのプロセスは次のとおりです。

1. Xamarin.Forms アプリケーションは、PNS ハンドルを返すを登録します。
1. Azure Mobile Apps インスタンスは、対象とするデバイスのハンドルを指定する、Azure 通知ハブに通知を送信します。
1. Azure 通知ハブは、適切なデバイスの PNS に通知を送信します。
1. PNS は、指定されたデバイスに通知を送信します。
1. Xamarin.Forms アプリケーションは、通知を処理し、それを表示します。

サンプル アプリケーションでは、Azure Mobile Apps インスタンス内にデータを格納、todo リスト アプリケーションを示します。 新しい項目は、Azure Mobile Apps インスタンスに追加するたびに、Xamarin.Forms アプリケーションにプッシュ通知が送信します。 次のスクリーン ショットは、受信したプッシュ通知を表示する各プラットフォームを示しています。

[![](azure-images/screenshots.png "サンプル アプリケーションのプッシュ通知を受け取る")](azure-images/screenshots-large.png#lightbox "サンプル アプリケーションのプッシュ通知の受信")

Azure Notification Hubs の詳細については、次を参照してください。 [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)と[Xamarin.Forms アプリにプッシュ通知を追加](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/)です。

## <a name="azure-and-platform-notification-system-setup"></a>Azure とプラットフォーム通知システムのセットアップ

Azure Mobile Apps インスタンスへの Azure 通知ハブの統合のプロセスは次のとおりです。

1. Azure Mobile Apps インスタンスを作成します。 詳細については、次を参照してください。 [Azure Mobile App を消費して](~/xamarin-forms/data-cloud/consuming/azure.md)です。
1. 通知ハブを構成します。 詳細については、次を参照してください。[通知ハブの構成](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub)です。
1. プッシュ通知を送信する、Azure Mobile Apps インスタンスを更新します。 詳細については、次を参照してください。[サーバー プロジェクトを更新してプッシュ通知を送信](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications)です。
1. 各 PNS に登録します。
1. 通知ハブを各 PNS との通信を構成します。

次のセクションでは、各プラットフォーム用の追加のセットアップ手順を説明します。

### <a name="ios"></a>iOS

次の手順は、Apple Push Notification サービス (APNS) から Azure 通知ハブを使用するために実行する必要があります。

1. キーチェーン アクセス ツールを使用してプッシュ証明書の要求の署名証明書を生成します。 詳細については、次を参照してください。[プッシュ証明書の証明書署名要求ファイルを生成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate)Azure ドキュメント センターにします。
1. Apple のデベロッパー センターでプッシュ通知のサポートについては、Xamarin.Forms アプリケーションを登録します。 詳細については、次を参照してください。[プッシュ通知用のアプリを登録](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications)Azure ドキュメント センターにします。
1. プッシュ通知有効になっているプロビジョニング プロファイルを作成、Xamarin.Forms アプリケーションの Apple Developer Center にします。 詳細については、次を参照してください。[プロビジョニング プロファイルをアプリの作成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app)Azure ドキュメント センターにします。
1. APNS と通信するために、通知ハブを構成します。 詳細については、次を参照してください。 [apns 通知ハブの構成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns)です。
1. Xamarin.Forms を使用するアプリケーション、新しいアプリ ID とプロビジョニング プロファイルを構成します。 詳細については、次を参照してください。 [Xamarin Studio での iOS プロジェクトを構成する](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio)または[Visual Studio での iOS プロジェクトを構成する](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio)Azure ドキュメント センターにします。

### <a name="android"></a>Android

次の手順は、Firebase クラウド メッセージング (FCM) から Azure 通知ハブを使用するために実行する必要があります。

1. FCM に登録します。 サーバー API キーと、クライアント ID は、自動的に生成されにパックされて、`google-services.json`ダウンロードされるファイルです。 詳細については、次を参照してください。[を有効にする Firebase クラウド メッセージング (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm)です。
1. FCM 通信するために、通知ハブを構成します。 詳細については、次を参照してください。 [FCM を使用してプッシュ要求を送信する、モバイル アプリのバックアップが終了構成](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm)です。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次の手順は、Azure 通知ハブから Windows 通知サービス (WNS) を使用するために実行する必要があります。

1. Windows 通知サービス (WNS) に登録します。 詳細については、次を参照してください。 [WNS でプッシュ通知の Windows アプリの登録](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns)Azure ドキュメント センターにします。
1. WNS で通信するために、通知ハブを構成します。 詳細については、次を参照してください。 [wns 通知ハブの構成](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns)Azure ドキュメント センターにします。

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Xamarin.Forms アプリケーションへのプッシュ通知のサポートの追加

次のセクションでは、プッシュ通知をサポートする各プラットフォームに固有のプロジェクトで必要な実装について説明します。

### <a name="ios"></a>iOS

IOS アプリケーションのプッシュ通知のサポートを実装するためのプロセスは次のとおりです。

1. Apple Push Notification サービス (APNS) での登録、`AppDelegate.FinishedLaunching`メソッドです。 詳細については、次を参照してください。 [Apple プッシュ通知システムへの登録](#ios_register)です。
1. 実装、`AppDelegate.RegisteredForRemoteNotifications`登録応答を処理するメソッド。 詳細については、次を参照してください。[登録応答の処理](#ios_registration_response)です。
1. 実装、`AppDelegate.DidReceiveRemoteNotification`プッシュ通知の受信を処理するメソッド。 詳細については、次を参照してください。[プッシュ通知の受信処理](#ios_process_incoming)です。

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Apple Push Notification サービスを登録します。

IOS アプリケーションには、プッシュ通知を受け取ることができます、前で、Apple Push Notification サービス (APNS)、これは、一意のデバイス トークンを生成し、アプリケーションに戻るに登録する必要があります。 登録が呼び出される、`FinishedLaunching`内の上書き、`AppDelegate`クラス。

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

APNS と iOS アプリケーションの登録時に、プッシュ通知を受信することを望むの種類ことを指定する必要があります。 `RegisterUserNotificationSettings`メソッドは、アプリケーションは受信すると、通知の種類を登録と、 `RegisterForRemoteNotifications` APNS からプッシュ通知を受け取るを登録するメソッド。

> [!NOTE]
> 呼び出しに失敗、`RegisterUserNotificationSettings`メソッドは、アプリケーションによって受信されるサイレント モードでプッシュ通知になります。

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>登録応答の処理

APNS 登録要求は、バック グラウンドで発生します。 IOS を呼び出すが、応答を受信すると、`RegisteredForRemoteNotifications`内の上書き、`AppDelegate`クラス。

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

このメソッドは、JSON として単純な通知メッセージのテンプレートを作成し、通知ハブからテンプレート通知を受信するデバイスを登録します。

> [!NOTE]
> `FailedToRegisterForRemoteNotifications`まったくネットワーク接続などの状況を処理するオーバーライドを実装する必要があります。 これはユーザーが中に、アプリケーションを開始可能性がありますので重要オフラインです。

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>プッシュ通知の受信処理

`DidReceiveRemoteNotification`内の上書き、`AppDelegate`クラスは、アプリケーションが実行されていると、通知を受信したときに呼び出されるときに、プッシュ通知の受信を処理するために使用します。

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

`userInfo`ディクショナリが含まれる、`aps`キー、値が、`alert`残りの通知データを使用してディクショナリ。 このディクショナリを取得すると、使用、 `string`  ダイアログ ボックスに表示される通知メッセージです。

> [!NOTE]
> アプリケーションを起動する場合は、アプリケーションは、プッシュ通知が到着したときに実行されていない、ですが、`DidReceiveRemoteNotification`方法で、通知は処理されません。 代わりに、通知のペイロードを取得してから適切に応答、`WillFinishLaunching`または`FinishedLaunching`をオーバーライドします。

APNS の詳細については、次を参照してください。 [iOS でのプッシュ通知](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)です。

### <a name="android"></a>Android

Android アプリケーションのプッシュ通知のサポートを実装するためのプロセスは次のとおりです。

1. 追加、 [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet は、Android のプロジェクトにパッケージ化し、Android 7.0 またはそれ以上のアプリケーションのターゲット バージョンを設定します。
1. 追加、`google-services.json`ファイル、Android プロジェクトのルートに Firebase コンソールからダウンロードされ、そのビルド アクション設定**GoogleServicesJson**です。 詳細については、次を参照してください。 [Google サービスの JSON ファイルを追加](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)です。
1. Android マニフェスト内の受信機を宣言することで登録 Firebase クラウドのメッセージング (FCM) ファイル、および実装することによって、`FirebaseRegistrationService.OnTokenRefresh`メソッドです。 詳細については、次を参照してください。 [Firebase Cloud Messaging で登録](#android_register_fcm)です。
1. Azure 通知ハブの登録、`AzureNotificationHubService.RegisterAsync`メソッドです。 詳細については、次を参照してください。 [Azure 通知ハブに登録する](#android_register_azure)です。
1. 実装、`FirebaseNotificationService.OnMessageReceived`プッシュ通知の受信を処理するメソッド。 詳細については、次を参照してください。[のプッシュ通知の内容を表示する](#android_displaying_notification)です。

Firebase Cloud Messaging の詳細については、次を参照してください。 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)と[Firebase Cloud Messaging でリモート通知](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)です。

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase を登録するクラウドのメッセージング

Android アプリケーションでは、プッシュ通知を受信できるように、これは登録トークンを生成し、アプリケーションに返す FCM に登録する必要があります。 登録トークンの詳細については、次を参照してください。 [FCM で登録](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration)です。

これを実現します。

- Android マニフェスト内の受信機を宣言します。 詳細については、次を参照してください。 [Android マニフェスト内の受信機を宣言する](#declaring_a_receiver)です。
- Firebase インスタンスの ID サービスを実装します。 詳細については、次を参照してください。 [Firebase インスタンスの ID サービスを実装する](#implementing-firebase-instance-id-service)です。

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Android マニフェスト内の受信機を宣言します。

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

- 内部宣言`FirebaseInstanceIdInternalReceiver`を安全にサービスを開始するために使用する実装。
- 宣言、`FirebaseInstanceIdReceiver`アプリ インスタンスごとに一意の識別子を提供する実装。 この受信者は認証し、操作を認証します。

`FirebaseInstanceIdReceiver`受信`FirebaseInstanceId`と`FirebaseMessaging`イベントから派生したクラスに配信および`FirebasesInstanceIdService`です。

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Firebase インスタンスの ID サービスを実装します。

FCM をアプリケーションに登録するには、派生クラスを`FirebaseInstanceIdService`クラスです。 このクラスは、FCM にアクセスするクライアント アプリケーションを承認するセキュリティ トークンの生成を担当します。 サンプル アプリケーションで、`FirebaseRegistrationService`クラスから派生、`FirebaseInstanceIdService`クラスし、次のコード例に示します。

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

`OnTokenRefresh`アプリケーションが FCM から登録トークンを受信すると、メソッドが呼び出されます。 メソッドからのトークンの取得、 `FirebaseInstanceId.Instance.Token` FCM によって非同期的に更新されるプロパティです。 `OnTokenRefresh`頻度の低いメソッドは、アプリケーションがインストールまたはアンインストールすると、アプリケーションには、インスタンス ID が消去されるときに、ユーザーが、アプリケーションのデータを削除すると場合にのみ、トークンが更新されるため、またはトークンのセキュリティになったときセキュリティが侵害されました。 さらに、アプリケーションが更新されるあるそのトークン、定期的に通常 6 か月ごと FCM インスタンスの ID サービスを要求します。

`OnTokenRefresh`メソッドも呼び出します、`SendRegistrationTokenToAzureNotificationHub`メソッドで、Azure 通知ハブで、ユーザーの登録トークンを関連付けるために使用します。

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Azure 通知ハブに登録します。

`AzureNotificationHubService`クラスを提供、`RegisterAsync`メソッドで、Azure 通知ハブをユーザーの登録トークンを関連付けます。 次のコード例は、`RegisterAsync`によって呼び出されるメソッド、`FirebaseRegistrationService`クラスのユーザーの登録トークンが変更されたとき。

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

このメソッドは、Firebase 登録トークンを使用して、通知ハブからテンプレート通知を受信するには、JSON、およびレジスタとして単純な通知メッセージのテンプレートを作成します。 これにより、Azure 通知ハブから送信された通知が登録トークンによって表されるデバイスを対象とします。

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>プッシュ通知の内容を表示します。

プッシュ通知の内容を表示するには、派生クラスを`FirebaseMessagingService`クラスです。 このクラスが含まれていますが、オーバーライド可能な`OnMessageReceived`アプリケーション FCM から通知を受信するときに、呼び出される、メソッドが、アプリケーションがフォア グラウンドで実行されていることを指定します。 サンプル アプリケーションで、`FirebaseNotificationService`クラスから派生、`FirebaseMessagingService`クラス、し、次のコード例に示します。

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

アプリケーションは FCM から通知を受け取ると、`OnMessageReceived`メソッドは、コンテンツ、メッセージを抽出し、呼び出し、`SendNotification`メソッドです。 このメソッドは、メッセージの内容を通知領域に表示される通知で、アプリケーションの実行中に起動されるローカルの通知に変換します。

##### <a name="handling-notification-intents"></a>通知の目的の処理

通知をタップすると、通知メッセージに付属するすべてのデータを使用可能で、 `Intent` extras です。 次のコードでは、このデータを抽出することができます。

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

アプリケーションのランチャー`Intent`ため、このコードに付属するデータをログイン、ユーザーがその通知メッセージをタップしたときに発生、`Intent`出力ウィンドウをします。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) の前にアプリケーションは、Windows 通知サービス (WNS と)、通知チャネルを返すことを登録する必要がありますプッシュ通知を受信できます。 登録はにより呼び出され、`InitNotificationsAsync`メソッドで、`App`クラス。

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

このメソッドは、プッシュ通知チャネルを取得、JSON として通知メッセージのテンプレートを作成し、通知ハブからテンプレート通知を受信するデバイスを登録します。

`InitNotificationsAsync`メソッドが呼び出されて、`OnLaunched`内の上書き、`App`クラス。

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

これにより、プッシュ通知登録を作成または、アプリケーションを起動すると、そのため、WNS プッシュ チャネルが常にアクティブであることを確認するたびに更新します。

プッシュ通知を受信したときに自動的に表示されますとして、*トースト*– モードレス ウィンドウ メッセージを含むです。

## <a name="summary"></a>まとめ

この記事では、Azure Notification Hubs を使用して、Azure Mobile Apps インスタンスから Xamarin.Forms アプリケーションにプッシュ通知を送信する方法が示されています。 Azure Notification Hubs は、モバイル プッシュ通知を送信する任意のバックエンドから任意のモバイル プラットフォームでは、異なるプラットフォーム通知システムと通信すること、バックエンドの複雑さを排除しながら、スケーラブルなプッシュ インフラストラクチャを提供します。


## <a name="related-links"></a>関連リンク

- [Azure のモバイル アプリの使用](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure 通知ハブ](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Xamarin.Forms アプリにプッシュ通知を追加します。](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [IOS でのプッシュ通知](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
