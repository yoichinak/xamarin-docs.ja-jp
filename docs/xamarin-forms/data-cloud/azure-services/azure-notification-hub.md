---
title: Azure Notification Hubs と Xamarin.Forms を使用してプッシュ通知を送受信する
description: この記事では、Azure Notification Hubs を使用して、Xamarin.Forms アプリケーションにクロスプラットフォームのプッシュ通知を送信する方法について説明します。
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
- Firebase
ms.openlocfilehash: 5fd657a3d55bd26b95e79e39540dcfe5b8bce08f
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918593"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-no-locxamarinforms"></a>Azure Notification Hubs と Xamarin.Forms を使用してプッシュ通知を送受信する

[![サンプルをダウンロードする](~/media/shared/download.png)サンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

プッシュ通知により、情報がバックエンド システムからモバイル アプリケーションに配信されます。 Apple、Google、およびその他のプラットフォームにはそれぞれ独自のプッシュ通知サービス (PNS) が用意されています。 Azure Notification Hubs では、バックエンド アプリケーションが 1 つのハブと通信できるように、異なるプラットフォームにわたって通知を一元化でき、各プラットフォーム固有の PNS に通知が配信されます。

次の手順に従って、Azure Notification Hubs をモバイル アプリに統合します。

1. [プッシュ通知サービスと Azure Notification Hub を設定する](#set-up-push-notification-services-and-azure-notification-hub)。
1. [テンプレートとタグの使用方法について理解する](#register-templates-and-tags-with-the-azure-notification-hub)。
1. [クロスプラットフォーム Xamarin.Forms アプリケーションを作成する](#xamarinforms-application-functionality)。
1. [プッシュ通知用にネイティブ Android プロジェクトを構成する](#configure-the-android-application-for-notifications)。
1. [プッシュ通知用にネイティブ iOS プロジェクトを構成する](#configure-ios-for-notifications)。
1. [Azure Notification Hub を使用して通知をテストする](#test-notifications-in-the-azure-portal)。
1. [通知を送信するバックエンド アプリケーションを作成する](#create-a-notification-dispatcher)。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>プッシュ通知サービスと Azure Notification Hub を設定する

Azure Notification Hubs と Xamarin.Forms モバイル アプリの統合は、Azure Notification Hubs と Xamarin ネイティブ アプリケーションの統合と似ています。 [Azure Notification Hubs を使用した Xamarin.Android へのプッシュ通知](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)に関する記事のコンソールの手順に従って、Firebase Cloud Messaging (FCM) アプリケーションFirebaseを設定します。 Xamarin.Android のチュートリアルを使用して、次の手順を行います。

1. Android パッケージ名を定義します。たとえば、サンプルでは `com.xamarin.notifysample` を使用しています。
1. Firebase コンソールから `google-services.json` をダウンロードします。 このファイルは、後の手順で Android アプリケーションに追加します。
1. Azure Notification Hub インスタンスを作成して名前を付けます。 この記事とサンプルでは、ハブ名として `xdocsnotificationhub` を使用します。
1. FCM **サーバー キー**をコピーして、**API キー**として Azure Notification Hub の **Google (GCM/FCM)** の下に保存します。

次のスクリーンショットは、Azure Notification Hub の Google プラットフォーム構成を示しています。

![Azure Notification Hub の Google 構成のスクリーンショット](azure-notification-hub-images/fcm-notification-hub-config.png "Azure Notification Hub の Google 構成")

iOS デバイスのセットアップを完了するには、macOS コンピューターが必要です。 [Azure Notification Hubs を使用した Xamarin.iOS へのプッシュ通知](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)に関するページの最初の手順に従って、Apple Push Notification Service (APNS) を設定します。 Xamarin.iOS のチュートリアルを使用して、次の手順を行います。

1. iOS バンドル識別子を定義します。 この記事とサンプルでは、バンドル識別子として `com.xamarin.notifysample` を使用します。
1. 証明書署名要求 (CSR) ファイルを作成し、それを使用してプッシュ通知証明書を生成します。
1. Azure Notification Hub で **Apple (APNS)** の下にプッシュ通知証明書をアップロードします。

次のスクリーンショットは、Azure Notification Hub の Apple プラットフォーム構成を示しています。

![Azure Notification Hub の Apple 構成のスクリーンショット](azure-notification-hub-images/apns-notification-hub-config.png "Azure Notification Hub の Apple 構成")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Azure Notification Hub でテンプレートとタグを登録する

Azure Notification Hub では、モバイル アプリケーションをハブに登録し、テンプレートを定義してタグをサブスクライブする必要があります。 登録すると、Azure Notification Hub で、プラットフォーム固有の PNS ハンドルが識別子にリンクされます。 登録の詳細については、[登録管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)に関するページをご覧ください。

テンプレートを使用すると、デバイスで、パラメーター化されたメッセージ テンプレートを指定できます。 受信メッセージは、デバイスごと、タグごとにカスタマイズできます。 テンプレートの詳細については、[テンプレート](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)に関するページをご覧ください。

タグを使用して、ニュース、スポーツ、天気などのメッセージ カテゴリをサブスクライブできます。 わかりやすくするために、サンプル アプリケーションでは、`messageParam` という 1 つのパラメーターと `default` という 1 つのタグを持つ既定のテンプレートが定義されています。 より複雑なシステムでは、ユーザー固有のタグを使用して、異なるデバイスにわたってユーザーにメッセージを送信して、通知をカスタマイズできます。 タグの詳細については、[ルーティングとタグ式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)に関するページをご覧ください。

メッセージを正常に受信するには、各ネイティブ アプリケーションで次の手順を行う必要があります。

1. プラットフォーム PNS から PNS ハンドルまたはトークンを取得します。
1. PNS ハンドルを Azure Notification Hub に登録します。
1. 送信メッセージと同じパラメーターを含むテンプレートを指定します。
1. 送信メッセージの対象となるタグをサブスクライブします。

各プラットフォームでのこれらの手順の詳細については、「[通知用に Android アプリケーションを構成する](#configure-the-android-application-for-notifications)」セクションと「[通知用に iOS を構成する](#configure-ios-for-notifications)」セクションを参照してください。

## <a name="no-locxamarinforms-application-functionality"></a>Xamarin.Forms アプリケーションの機能

サンプルの Xamarin.Forms アプリケーションでは、プッシュ通知メッセージの一覧を表示します。 これを実現するには、`AddMessage` メソッドを使用します。このメソッドにより、指定したプッシュ通知メッセージが UI に追加されます。 また、このメソッドは、重複するメッセージが UI に追加されないようにし、任意のスレッドから呼び出すことができるようにメイン スレッドで実行します。 `AddMessage` メソッドのコードを次に示します。

```csharp
public void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (messageDisplay.Children.OfType<Label>().Where(c => c.Text == message).Any())
        {
            // Do nothing, an identical message already exists
        }
        else
        {
            Label label = new Label()
            {
                Text = message,
                HorizontalOptions = LayoutOptions.CenterAndExpand,
                VerticalOptions = LayoutOptions.Start
            };
            messageDisplay.Children.Add(label);
        }
    });
}
```

サンプル アプリケーションには、プラットフォーム プロジェクトに使用されるプロパティを定義する `AppConstants.cs` ファイルが含まれています。 このファイルを Azure Notification Hub の値を使用してカスタマイズする必要があります。 `AppConstants.cs` ファイルのコードを次に示します。

```csharp
public static class AppConstants
{
    public static string NotificationChannelName { get; set; } = "XamarinNotifyChannel";
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string ListenConnectionString { get; set; } = "< Insert your DefaultListenSharedAccessSignature >";
    public static string DebugTag { get; set; } = "XamarinNotify";
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string FCMTemplateBody { get; set; } = "{\"data\":{\"message\":\"$(messageParam)\"}}";
    public static string APNTemplateBody { get; set; } = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";
}
```

`AppConstants` の次の値をカスタマイズして、サンプル アプリケーションを Azure Notification Hub に接続します。

* `NotificationHubName`:Azure portal で作成した Azure Notification Hub の名前を使用します。
* `ListenConnectionString`:この値は、Azure Notification Hub の **[アクセス ポリシー]** の下に表示されています。

次のスクリーンショットは、これらの値が Azure portal のどこに表示されているかを示しています。

![Azure Notification Hub のアクセス ポリシーのスクリーンショット](azure-notification-hub-images/notification-hub-access-policy.png "Azure Notification Hub のアクセス ポリシー")

## <a name="configure-the-android-application-for-notifications"></a>通知用に Android アプリケーションを構成する

次の手順を行って、通知を受信して処理するように Android アプリケーションを構成します。

1. Firebase コンソールのパッケージ名と一致するように、Android の**パッケージ名**を構成します。
1. 次の NuGet パッケージをインストールして、Google Play、Firebase、Azure Notification Hubs とやりとり通信します。
    1. `Xamarin.GooglePlayServices.Base`
    1. `Xamarin.Firebase.Messaging`
    1. `Xamarin.Azure.NotificationHubs.Android`
1. FCM セットアップ中にダウンロードした `google-services.json` ファイルをプロジェクトにコピーし、ビルド アクションを `GoogleServicesJson` に設定します。
1. Firebase と通信するように `AndroidManifest.xml` を[構成](#configure-android-manifest)します。
1. `FirebaseMessagingService` を[オーバーライド](#override-firebasemessagingservice-to-handle-messages)してメッセージを処理します。
1. 受信通知を Xamarin.Forms UI に[追加](#add-incoming-notifications-to-the-xamarinforms-ui)します。

> [!NOTE]
> `GoogleServicesJson` ビルド アクションは、`Xamarin.GooglePlayServices.Base` NuGet パッケージの一部です。 Visual Studio 2019 では、起動時に使用可能なビルド アクションが設定されます。 ビルド アクションとして `GoogleServicesJson` が表示されない場合は、NuGet パッケージのインストール後に Visual Studio 2019 を再起動します。

### <a name="configure-android-manifest"></a>Android マニフェストを構成する

`application` 要素内の `receiver` 要素により、アプリから Firebase と通信することができます。 `uses-permission` 要素を使用すると、アプリでメッセージを処理し、Azure Notification Hub に登録できます。 完全な `AndroidManifest.xml` は、次の例のようになります。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="YOUR_PACKAGE_NAME" android:installLocation="auto">
  <uses-sdk android:minSdkVersion="21" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
  <application android:label="Notification Hub Sample">
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
      <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

### <a name="override-no-locfirebasemessagingservice-to-handle-messages"></a>`FirebaseMessagingService` をオーバーライドしてメッセージを処理する

Firebase に登録してメッセージを処理するには、`FirebaseMessagingService` クラスをサブクラス化します。 このサンプル アプリケーションでは、`FirebaseMessagingService` をサブクラス化する `FirebaseService` クラスを定義しています。 このクラスには、`com.google.firebase.MESSAGING_EVENT` フィルターを含む `IntentFilter` 属性がタグ付けされています。 このフィルターにより、Android では、次の処理を行うために、このクラスに受信メッセージを渡すことができます。

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    // ...
}

```

アプリケーションが起動すると、Firebase SDK によって、Firebase サーバーから一意のトークン識別子が自動的に要求されます。 要求が成功すると、`OnNewToken` メソッドが `FirebaseService` クラスで呼び出されます。 サンプル プロジェクトでは、このメソッドをオーバーライドし、Azure Notification Hubs にトークンを登録します。

```csharp
public override void OnNewToken(string token)
{
    // NOTE: save token instance locally, or log if desired

    SendRegistrationToServer(token);
}

void SendRegistrationToServer(string token)
{
    try
    {
        NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

        // register device with Azure Notification Hub using the token from FCM
        Registration registration = hub.Register(token, AppConstants.SubscriptionTags);

        // subscribe to the SubscriptionTags list with a simple template.
        string pnsHandle = registration.PNSHandle;
        TemplateRegistration templateReg = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
    }
    catch (Exception e)
    {
        Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
    }
}
```

`SendRegistrationToServer` メソッドは、Azure Notification Hub にデバイスを登録し、テンプレートを使用してタグをサブスクライブします。 このサンプル アプリケーションでは、`AppConstants.cs` ファイルで、`default` という 1 つのタグと `messageParam` という 1 つのパラメーターを持つテンプレートを定義しています。 登録、タグ、テンプレートの詳細については、「[Azure Notification Hub でテンプレートとタグを登録する](#register-templates-and-tags-with-the-azure-notification-hub)」を参照してください。

メッセージを受信すると、`OnMessageReceived` メソッドが `FirebaseService` クラスで呼び出されます。

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    base.OnMessageReceived(message);
    string messageBody = string.Empty;

    if (message.GetNotification() != null)
    {
        messageBody = message.GetNotification().Body;
    }

    // NOTE: test messages sent via the Azure portal will be received here
    else
    {
        messageBody = message.Data.Values.First();
    }

    // convert the incoming message to a local notification
    SendLocalNotification(messageBody);

    // send the incoming message directly to the MainPage
    SendMessageToMainPage(messageBody);
}

void SendLocalNotification(string body)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    intent.PutExtra("message", body);

    //Unique request code to avoid PendingIntent collision.
    var requestCode = new Random().Next();
    var pendingIntent = PendingIntent.GetActivity(this, requestCode, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new NotificationCompat.Builder(this)
        .SetContentTitle("XamarinNotify Message")
        .SetSmallIcon(Resource.Drawable.ic_launcher)
        .SetContentText(body)
        .SetAutoCancel(true)
        .SetShowWhen(false)
        .SetContentIntent(pendingIntent);

    if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
    {
        notificationBuilder.SetChannelId(AppConstants.NotificationChannelName);
    }

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}

void SendMessageToMainPage(string body)
{
    (App.Current.MainPage as MainPage)?.AddMessage(body);
}
```

受信メッセージは、`SendLocalNotification` メソッドを使用してローカル通知に変換されます。 このメソッドにより、新しい `Intent` が作成され、メッセージの内容が `string` `Extra` として `Intent` に配置されます。 ユーザーがローカル通知をタップすると、アプリがフォアグラウンドとバックグラウンドのどちらにあるかにかかわらず、`MainActivity` が起動され、`Intent` オブジェクトを介してメッセージの内容にアクセスできるようになります。

ローカル通知と `Intent` の例では、ユーザーは通知をタップする操作を行う必要があります。 これは、アプリケーションの状態が変化する前にユーザーがアクションを実行する必要がある場合に適しています。 ただし、場合によっては、ユーザーの操作を必要とせずに、メッセージ データにアクセスすることもできます。 また、前の例では、`SendMessageToMainPage` メソッドを使用して、現在の `MainPage` インスタンスにメッセージが直接送信されます。 運用環境では、両方のメソッドを 1 つのメッセージ型に対して実装すると、ユーザーが通知をタップしたときに、`MainPage` オブジェクトで、重複するメッセージが取得されます。

> [!NOTE]
> Android アプリケーションでは、バックグラウンドまたはフォアグラウンドで実行されている場合にのみプッシュ通知を受信します。 メイン `Activity` が実行されていないときにプッシュ通知を受信するには、サービスを実装する必要があります。これは、このサンプルの範囲を超えています。 詳細については、[Android サービスの作成](/xamarin/android/app-fundamentals/services/)に関するページをご覧ください

### <a name="add-incoming-notifications-to-the-no-locxamarinforms-ui"></a>Xamarin.Forms UI に受信通知を追加する

`MainActivity` クラスでは、通知を処理し、受信メッセージ データを管理するためのアクセス許可を取得する必要があります。 次のコードは、`MainActivity` の完全な実装を示しています。

```csharp
[Activity(Label = "NotificationHubSample", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, LaunchMode = LaunchMode.SingleTop)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());

        if (!IsPlayServiceAvailable())
        {
            throw new Exception("This device does not have Google Play Services and cannot receive push notifications.");
        }

        CreateNotificationChannel();
    }

    protected override void OnNewIntent(Intent intent)
    {
        if (intent.Extras != null)
        {
            var message = intent.GetStringExtra("message");
            (App.Current.MainPage as MainPage)?.AddMessage(message);
        }

        base.OnNewIntent(intent);
    }

    bool IsPlayServiceAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(AppConstants.DebugTag, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(AppConstants.DebugTag, "This device is not supported");
            }
            return false;
        }
        return true;
    }

    void CreateNotificationChannel()
    {
        // Notification channels are new as of "Oreo".
        // There is no need to create a notification channel on older versions of Android.
        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            var channelName = AppConstants.NotificationChannelName;
            var channelDescription = String.Empty;
            var channel = new NotificationChannel(channelName, channelName, NotificationImportance.Default)
            {
                Description = channelDescription
            };

            var notificationManager = (NotificationManager)GetSystemService(NotificationService);
            notificationManager.CreateNotificationChannel(channel);
        }
    }
}
```

`Activity` 属性で、アプリケーションの `LaunchMode` を `SingleTop` に設定します。 この起動モードにより、このアクティビティの 1 つのインスタンスのみを許可するように Android OS に指示されます。 この起動モードでは、受信 `Intent` データは `OnNewIntent` メソッドにルーティングされます。このメソッドでは、メッセージ データを抽出して、`AddMessage` メソッドを使用して `MainPage` インスタンスに送信します。 アプリケーションで別の起動モードを使用する場合は、`Intent` データを異なる方法で処理する必要があります。

`OnCreate` メソッドでは、`IsPlayServiceAvailable` と呼ばれるヘルパー メソッドを使用して、デバイスが Google Play サービスをサポートしていることを保証します。 Google Play サービスがサポートされていないエミュレーターまたはデバイスでは、Firebase からプッシュ通知を受信できません。

## <a name="configure-ios-for-notifications"></a>通知用に iOS を構成する

通知を受信するように iOS アプリケーションを構成するプロセスは次のとおりです。

1. プロビジョニング プロファイルで使用されている値と一致するように、`Info.plist` ファイルの**バンドル識別子**を構成します。
1. `Entitlements.plist` ファイルに **[プッシュ通知を有効にする]** オプションを追加します。
1. `Xamarin.Azure.NotificationHubs.iOS` NuGet パッケージをプロジェクトに追加します。
1. APNS で通知を[登録](#register-for-notifications-with-apns)します。
1. アプリケーションを Azure Notification Hub に[登録](#register-with-azure-notification-hub-and-subscribe-to-tags)して、タグをサブスクライブします。
1. APNS の通知を Xamarin.Forms UI に[追加](#add-apns-notifications-to-xamarinforms-ui)します。

次のスクリーンショットは、Visual Studio 内で、`Entitlements.plist` ファイルで選択されている **[プッシュ通知を有効にする]** オプションを示しています。

![プッシュ通知のエンタイトルメントのスクリーンショット](azure-notification-hub-images/push-notification-entitlement.png "プッシュ通知のエンタイトルメント")

### <a name="register-for-notifications-with-apns"></a>APNS で通知を登録する

リモート通知を登録するには、`AppDelegate.cs` ファイルの `FinishedLaunching` メソッドをオーバーライドする必要があります。 登録は、デバイスで使用されている iOS のバージョンによって異なります。 サンプル アプリケーションの iOS プロジェクトは、次の例に示すように、`RegisterForRemoteNotifications` を呼び出すように `FinishedLaunching` メソッドをオーバーライドしています。

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();
    LoadApplication(new App());

    base.FinishedLaunching(app, options);

    RegisterForRemoteNotifications();

    return true;
}

void RegisterForRemoteNotifications()
{
    // register for remote notifications based on system version
    if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
    {
        UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert |
            UNAuthorizationOptions.Sound |
            UNAuthorizationOptions.Sound,
            (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
    }
    else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
    {
        var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
        new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();
    }
    else
    {
        UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
        UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
    }
}
```

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Azure Notification Hub に登録してタグをサブスクライブする

`FinishedLaunching` メソッドでデバイスがリモート通知用に正常に登録されると、iOS では `RegisteredForRemoteNotifications` メソッドを呼び出します。 次のアクションを実行するには、このメソッドをオーバーライドする必要があります。

1. `SBNotificationHub` をインスタンス化します。
1. 既存の登録をすべて登録解除します。
1. デバイスを通知ハブに登録します。
1. テンプレートを使用して特定のタグをサブスクライブします。

デバイス、テンプレート、タグの登録の詳細については、「[Azure Notification Hub でテンプレートとタグを登録する](#register-templates-and-tags-with-the-azure-notification-hub)」を参照してください。 次のコードは、デバイスとテンプレートの登録を示しています。

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAll(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNative(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplate(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                if (errorCallback != null)
                {
                    Debug.WriteLine($"RegisterTemplateAsync error: {errorCallback}");
                }
            }
        });
    });
}
```

> [!NOTE]
> ネットワーク接続がないなどの状況では、リモート通知の登録に失敗することがあります。 登録の失敗を処理するように `FailedToRegisterForRemoteNotifications` メソッドをオーバーライドすることを選択できます。

### <a name="add-apns-notifications-to-no-locxamarinforms-ui"></a>APNS の通知を Xamarin.Forms UI に追加する

デバイスがリモート通知を受信すると、iOS では `ReceivedRemoteNotification` メソッドを呼び出します。 受信メッセージ JSON は `NSDictionary` オブジェクトに変換され、`ProcessNotification` メソッドで、ディクショナリから値を抽出して Xamarin.Forms の `MainPage` インスタンスに送信します。 `ReceivedRemoteNotifications` メソッドは、次のコードに示されるように、`ProcessNotification` を呼び出すようにオーバーライドされます。

```csharp
public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
{
    ProcessNotification(userInfo, false);
}

void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
{
    // make sure we have a payload
    if (options != null && options.ContainsKey(new NSString("aps")))
    {
        // get the APS dictionary and extract message payload. Message JSON will be converted
        // into a NSDictionary so more complex payloads may require more processing
        NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
        string payload = string.Empty;
        NSString payloadKey = new NSString("alert");
        if (aps.ContainsKey(payloadKey))
        {
            payload = aps[payloadKey].ToString();
        }

        if (!string.IsNullOrWhiteSpace(payload))
        {
            (App.Current.MainPage as MainPage)?.AddMessage(payload);
        }

    }
    else
    {
        Debug.WriteLine($"Received request to process notification but there was no payload.");
    }
}
```

## <a name="test-notifications-in-the-azure-portal"></a>Azure portal で通知をテストする

Azure Notification Hubs では、アプリケーションがテスト メッセージを受信できるかどうかを確認できます。 通知ハブの **[テスト送信]** セクションで、ターゲット プラットフォームを選択し、メッセージを送信できます。 **[タグ式に送信]** を`default` に設定すると、`default` タグ用にテンプレートを登録したアプリケーションにメッセージが送信されます。 **[送信]** ボタンをクリックすると、メッセージで到達したデバイスの数を含むレポートが生成されます。 次のスクリーンショットは、Azure portal 内の Android の通知テストを示しています。

![Azure Notification Hub のテスト メッセージのスクリーンショット](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure Notification Hub のテスト メッセージ")

### <a name="testing-tips"></a>テストのヒント

1. アプリケーションでプッシュ通知を受信できることをテストする場合は、物理デバイスを使用する必要があります。 Android および iOS の仮想デバイスは、プッシュ通知を受信するように正しく構成されていない可能性があります。
1. サンプルの Android アプリケーションでは、Firebase トークンが発行されたときにトークンとテンプレートを登録します。 テスト中に、新しいトークンを要求し、Azure Notification Hub に再登録することが必要になる場合があります。 これを強制する最良の方法は、プロジェクトをクリーンアップし、`bin` と `obj` のフォルダーを削除し、アプリケーションをデバイスからアンインストールしてから再構築とデプロイを行うことです。
1. プッシュ通知フローの多くの部分は、非同期的に実行されます。 これにより、ブレークポイントがヒットしなかったり、予期しない順序でヒットしたりする可能性があります。 アプリケーション フローを中断せずに、デバイスまたはデバッグ ログを使用して実行をトレースします。 `Constants` に指定した `DebugTag` を使用して、Android デバイスのログをフィルター処理します。
1. Visual Studio でデバッグが停止すると、アプリは強制的に閉じられます。 デバッグ プロセスの一部として起動されたメッセージ受信側またはその他のサービスは閉じられ、メッセージ イベントに応答しなくなります。

## <a name="create-a-notification-dispatcher"></a>通知ディスパッチャーを作成する

Azure Notification Hubs では、バックエンド アプリケーションから、異なるプラットフォームにわたってデバイスに通知をディスパッチできるようにします。 このサンプルは、コンソール アプリケーションを使用した通知のディスパッチを示しています。 アプリケーションには、以下のプロパティを定義する `DispatcherConstants.cs` ファイルが含まれています。

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Azure Notification Hub の構成と一致するように `DispatcherConstants.cs` ファイルを構成する必要があります。 `SubscriptionTags` プロパティの値は、クライアント アプリで使用される値と一致している必要があります。 `NotificationHubName` プロパティは、Azure Notification Hub インスタンスの名前です。 `FullAccessConnectionString` プロパティは、通知ハブの**アクセス ポリシー**内にあるアクセス キーです。 次のスクリーンショットは、Azure portal 内の `NotificationHubName` と `FullAccessConnectionString` のプロパティの場所を示しています。

![Azure Notification Hub の名前と FullAccessConnectionString のスクリーンショット](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure Notification Hub の名前と FullAccessConnectionString")

コンソール アプリケーションでは、各 `SubscriptionTags` 値をループし、`NotificationHubClient` クラスのインスタンスを使用してサブスクライバーに通知を送信します。 次のコードは、コンソール アプリケーションの `Program` クラスを示しています。

``` csharp
class Program
{
    static int messageCount = 0;

    static void Main(string[] args)
    {
        Console.WriteLine($"Press the spacebar to send a message to each tag in {string.Join(", ", DispatcherConstants.SubscriptionTags)}");
        WriteSeparator();
        while (Console.ReadKey().Key == ConsoleKey.Spacebar)
        {
            SendTemplateNotificationsAsync().GetAwaiter().GetResult();
        }
    }

    private static async Task SendTemplateNotificationsAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(DispatcherConstants.FullAccessConnectionString, DispatcherConstants.NotificationHubName);
        Dictionary<string, string> templateParameters = new Dictionary<string, string>();

        messageCount++;

        // Send a template notification to each tag. This will go to any devices that
        // have subscribed to this tag with a template that includes "messageParam"
        // as a parameter
        foreach (var tag in DispatcherConstants.SubscriptionTags)
        {
            templateParameters["messageParam"] = $"Notification #{messageCount} to {tag} category subscribers!";
            try
            {
                await hub.SendTemplateNotificationAsync(templateParameters, tag);
                Console.WriteLine($"Sent message to {tag} subscribers.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Failed to send template notification: {ex.Message}");
            }
        }

        Console.WriteLine($"Sent messages to {DispatcherConstants.SubscriptionTags.Length} tags.");
        WriteSeparator();
    }

    private static void WriteSeparator()
    {
        Console.WriteLine("==========================================================================");
    }
}
```

サンプル コンソール アプリケーションが実行されていると、Space キーを押してメッセージを送信できます。 クライアント アプリケーションを実行しているデバイスでは、適切に構成されていれば、番号付きの通知を受信する必要があります。

## <a name="related-links"></a>関連リンク

* [プッシュ通知テンプレート](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)。
* [デバイス登録管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)。
* [ルーティングとタグ式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)。
* [Xamarin.Android の Azure Notification Hubs チュートリアル](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)。
* [Xamarin.iOS の Azure Notification Hubs チュートリアル](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)。
