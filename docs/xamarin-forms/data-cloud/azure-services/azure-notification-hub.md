---
title: Azure Notification Hubs と Xamarin.Forms を使用したプッシュ通知を送受信します。
description: この記事では、Azure Notification Hubs を使用して、Xamarin.Forms アプリケーションにクロスプラット フォーム プッシュ通知を送信する方法について説明します。
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: 474398922bf00e3a430166d8b2e073d200e6ed6e
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659329"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Azure Notification Hubs と Xamarin.Forms を使用したプッシュ通知を送受信します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureNotificationHub)

バックエンド システムから通知の提供情報をモバイル アプリケーションにプッシュします。 Apple、Google、およびその他のプラットフォームごと、独自のプッシュ通知サービス (PNS) があります。 Azure Notification Hubs では、バックエンド アプリケーションは、各プラットフォーム固有 PNS に通知を配布するを処理する 1 つのハブと通信できるように、プラットフォーム間で通知を一元化することができます。

次の手順に従って、モバイル アプリに Azure Notification Hubs を統合します。

1. [プッシュ通知サービスと Azure Notification Hub セットアップ](#set-up-push-notification-services-and-azure-notification-hub)します。
1. [タグとテンプレートを使用する方法を理解する](#register-templates-and-tags-with-the-azure-notification-hub)します。
1. [クロスプラット フォーム-Xamarin.Forms アプリケーションの作成](#xamarinforms-application-functionality)です。
1. [プッシュ通知用のネイティブの Android プロジェクトを構成](#configure-the-android-application-for-notifications)します。
1. [プッシュ通知用のネイティブの iOS プロジェクトを構成する](#configure-ios-for-notifications)します。
1. [Azure Notification Hub を使用して通知をテスト](#test-notifications-in-the-azure-portal)します。
1. [通知を送信するバックエンド アプリケーションを作成する](#create-a-notification-dispatcher)します。

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>プッシュ通知サービスと Azure Notification Hub を設定します。

Xamarin.Forms モバイル アプリと Azure Notification Hubs の統合は、Xamarin のネイティブ アプリケーションと Azure Notification Hubs の統合に似ています。 セットアップ、 **FCM アプリケーション**Firebase コンソールの手順に従って[Azure Notification Hubs を使用して Xamarin.Android へのプッシュ通知](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)します。 Xamarin.Android のチュートリアルを使用して、次の手順を完了するには。

1. Android パッケージ名をなどを定義`com.xamarin.notifysample`サンプルで使用されます。
1. ダウンロード**google-services.json** Firebase コンソールから。 このファイルは、後の手順で Android アプリケーションを追加します。
1. Azure Notification Hub インスタンスを作成し、名前を付けます。 この記事とサンプルの使用`xdocsnotificationhub`ハブ名として。
1. コピー、FCM**サーバー キー**として保存、 **API キー**  **Google (GCM/FCM)** Azure 通知ハブにします。

次のスクリーン ショットは、Azure Notification Hub の Google プラットフォームの構成を示しています。

![Azure Notification Hub Google 構成のスクリーン ショット](azure-notification-hub-images/fcm-notification-hub-config.png "Azure 通知ハブの Google の構成")

MacOS コンピューターの iOS デバイスのセットアップを完了する必要があります。 最初の手順に従って、APNS 設定[Azure Notification Hubs を使用して Xamarin.iOS へのプッシュ通知](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)します。 Xamarin.iOS チュートリアルを使用して、次の手順を完了するには。

1. IOS バンドル識別子を定義します。 この記事とサンプルの使用`com.xamarin.notifysample`バンドル識別子として。
1. 証明書署名要求 (CSR) ファイルを作成し、プッシュ通知証明書の生成に使用します。
1. プッシュ通知証明書をアップロード**Apple (APNS)** Azure 通知ハブにします。

次のスクリーン ショットは、Azure Notification Hub の Apple プラットフォームの構成を示しています。

![Azure Notification Hub の Apple の構成のスクリーン ショット](azure-notification-hub-images/apns-notification-hub-config.png "Azure 通知ハブの Apple の構成")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>テンプレートとタグを Azure Notification Hub に登録します。

Azure Notification Hub では、モバイル アプリケーションがハブに登録し、テンプレートを定義し、タグにサブスクライブする必要があります。 登録は、プラットフォーム固有の PNS ハンドルを Azure Notification Hub の識別子にリンクします。 登録の詳細については、次を参照してください。[登録管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)します。

テンプレートを使用すると、デバイスをパラメーター化されたメッセージ テンプレートを指定します。 タグあたりのデバイスあたりの受信メッセージをカスタマイズできます。 テンプレートの詳細については、次を参照してください。[テンプレート](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)します。

タグは、ニュース、スポーツ、天気などのメッセージのカテゴリを購読できます。 サンプル アプリケーションは、わかりやすいようにと呼ばれる 1 つのパラメーターで既定のテンプレートを定義します`messageParam`と 1 つのタグと呼ばれる`default`します。 複雑なシステムでは、ユーザーを個人用に設定された通知デバイス間でメッセージをユーザー固有のタグを使用できます。 タグの詳細については、次を参照してください。[ルーティングとタグ式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)します。

正常にメッセージを受信するには、各ネイティブ アプリケーションは、これらの手順を実行する必要があります。

1. プラットフォームの PNS から PNS ハンドルまたはトークンを取得します。
1. Azure Notification Hub の PNS ハンドルを登録します。
1. 送信メッセージと同じパラメーターを格納しているテンプレートを指定します。
1. 送信メッセージの対象となるタグをサブスクライブします。

詳細でプラットフォームごとに次の手順が説明されている、[通知用の Android アプリケーションを構成](#configure-the-android-application-for-notifications)と[通知用の iOS 構成](#configure-ios-for-notifications)セクション。

## <a name="xamarinforms-application-functionality"></a>Xamarin.Forms アプリケーションの機能

Xamarin.Forms のサンプル アプリケーションは、プッシュ通知メッセージの一覧を表示します。 これにより実現されます、`AddMessage`メソッドは、UI に、指定されたプッシュ通知メッセージを追加します。 このメソッドは、重複するメッセージが、UI に追加されないようにし、メイン スレッドで実行されるため、任意のスレッドから呼び出せることができます。 `AddMessage` メソッドのコードを次に示します。

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

サンプル アプリケーションが含まれています、 **AppConstants.cs**ファイルで、プラットフォーム プロジェクトで使用されるプロパティを定義します。 このファイルは、Azure 通知ハブからの値をカスタマイズする必要があります。 次のコードは、 **AppConstants.cs**ファイル。

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

次の値をカスタマイズ`AppConstants`サンプル アプリケーション、Azure 通知ハブを接続します。

* `NotificationHubName`:Azure portal で作成した Azure Notification Hub の名前を使用します。
* `ListenConnectionString`:Azure 通知ハブにこの値がある**アクセス ポリシー**します。

次のスクリーン ショットでは、Azure portal でこれらの値の位置を示します。

![Azure Notification Hub のアクセス ポリシーのスクリーン ショット](azure-notification-hub-images/notification-hub-access-policy.png "Azure Notification Hub へのアクセス ポリシー")

## <a name="configure-the-android-application-for-notifications"></a>通知を Android アプリケーションを構成します。

受信および処理通知を Android アプリケーションを構成するのには、次の手順を完了するには。

1. Android の構成**パッケージ名**Firebase コンソールでパッケージ名と一致します。
1. Google Play、Firebase と Azure Notification Hubs との対話には、次の NuGet パッケージをインストールします。
    1. Xamarin.GooglePlayServices.Base します。
    1. Xamarin.firebase.messaging。
    1. Xamarin.Azure.NotificationHubs.Android します。
1. コピー、 `google-services.json` FCM セットアップ プロジェクトをダウンロードし、ビルド アクションを設定ファイル`GoogleServicesJson`します。
1. [Firebase との通信に AndroidManifest.xml 構成](#configure-android-manifest)します。
1. [アプリケーションを登録 Firebase と Azure Notification Hub を使用して、 `FirebaseInstanceIdService`](#register-using-a-custom-firebaseinstanceidservice)します。
1. [メッセージを処理する`FirebaseMessagingService`](#process-messages-with-a-firebasemessagingservice)します。
1. [Xamarin.Forms UI に受信した通知を追加](#add-incoming-notifications-to-the-xamarinforms-ui)します。

> [!NOTE]
> **GoogleServicesJson**ビルド アクションの一部である、 **Xamarin.GooglePlayServices.Base** NuGet パッケージ。 Visual Studio 2019 は、起動中に使用可能なビルド アクションを設定します。 表示されない場合**GoogleServicesJson**ビルド アクションでは、NuGet パッケージのインストール後に Visual Studio 2019 を再起動します。

### <a name="configure-android-manifest"></a>Android マニフェストを構成します。

`receiver`内の要素、`application`要素をアプリに Firebase との通信を許可します。 `uses-permission`要素には、アプリがメッセージを処理し、Azure 通知ハブに登録ができるようにします。 完全な**AndroidManifest.xml**よう次の例になります。

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

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>カスタム FirebaseInstanceIdService を使用した登録します。

Firebase は、PNS 上のデバイスを一意に識別するトークンを発行します。 トークンは、長い有効期間があるが、場合によっては更新されます。 トークンが発行または更新されるときに、アプリケーションを Azure Notification Hub をその新しいトークンを登録する必要があります。 登録がから派生したクラスのインスタンスによって処理される`FirebaseInstanceIdService`します。

サンプル アプリケーションで`FirebaseRegistrationService`クラスから継承`FirebaseInstanceIdService`します。 このクラスには、`IntentFilter`を含む`com.google.firebase.INSTANCE_ID_EVENT`、Android OS を自動的に呼び出すことができます`OnTokenRefresh`Firebase によってトークンを発行するときにします。

次のコードは、カスタム`FirebaseInstanceIdService`サンプル アプリケーションから。

```csharp
[Service]
[IntentFilter(new [] { "com.google.firebase.INSTANCE_ID_EVENT"})]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    public override void OnTokenRefresh()
    {
        string token = FirebaseInstanceId.Instance.Token;

        // NOTE: logging the token is not recommended in production but during
        // development it is useful to test messages directly from Firebase
        Log.Info(AppConstants.DebugTag, $"Token received: {token}");

        SendRegistrationToServer(token);
    }

    void SendRegistrationToServer(string token)
    {
        try
        {
            NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

            // register device with Azure Notification Hub using the token from FCM
            Registration reg = hub.Register(token, AppConstants.SubscriptionTags);

            // subscribe to the SubscriptionTags list with a simple template.
            string pnsHandle = reg.PNSHandle;
            var cats = string.Join(", ", reg.Tags);
            var temp = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
        }
        catch (Exception e)
        {
            Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
        }
    }
}
```

`SendRegistrationToServer`メソッドで、 `FirebaseRegistrationClass` Azure 通知ハブにデバイスを登録し、テンプレートを使用してタグをサブスクライブしています。 サンプル アプリケーションと呼ばれる 1 つのタグを定義する`default`と 1 つのパラメーターを持つテンプレートと呼ばれる`messageParam`で、 **AppConstants.cs**ファイル。 登録、タグ、およびテンプレートの詳細については、次を参照してください[テンプレートとタグを、Azure Notification Hub に登録。](#register-templates-and-tags-with-the-azure-notification-hub)

### <a name="process-messages-with-a-firebasemessagingservice"></a>FirebaseMessagingService でメッセージの処理

受信メッセージのルーティング、`FirebaseMessagingService`インスタンス、ローカルの通知に変換できます。 サンプル アプリケーションでは Android プロジェクトと呼ばれるクラスが含まれている`FirebaseService`から継承する`FirebaseMessagingService`します。 このクラスには、`IntentFilter`を含む`com.google.firebase.MESSAGING_EVENT`、Android OS を自動的に呼び出すことができます`OnMessageReceived`プッシュ通知のメッセージを受信したとき。

次の例は、`FirebaseService`サンプル アプリケーションから。

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
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
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

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
}
```

受信メッセージとローカル通知に変換されます、`SendLocalNotification`メソッド。 このメソッドは、新しい作成`Intent`、メッセージのコンテンツを配置し、`Intent`として、 `string` `Extra`します。 アプリがフォア グラウンドまたはバック グラウンドであるかどうか、ユーザーがローカルの通知をタップすると、`MainActivity`が起動され、メッセージのコンテンツにアクセスする、`Intent`オブジェクト。

ローカル通知と`Intent`例が、ユーザーの通知をタップする操作を実行する必要があります。 ユーザーは、アプリケーションの状態を変更する前にアクションを実行する必要がありますが望ましいです。 ただし、場合によってはユーザーの操作を必要とせず、メッセージ データにアクセスしたい場合があります。 前の例は、現在に直接メッセージを送信することも`MainPage`インスタンス、`SendMessageToMainPage`メソッド。 1 つのメッセージの種類では、2 つのメソッドを実装する場合、運用環境で、`MainPage`オブジェクト、ユーザーが通知をタップした場合は、重複するメッセージが表示されます。

> [!NOTE]
> フォア グラウンドまたはバック グラウンドで実行されている場合、Android アプリケーションはプッシュ通知を受信のみです。 プッシュ通知を受信するときに、メイン`Activity`が実行されていない場合、このサンプルの範囲外である、サービスを実装する必要があります。 詳細については、次を参照してください[Android サービスの作成。](/xamarin/android/app-fundamentals/services/)

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Xamarin.Forms UI に受信した通知を追加します。

`MainActivity`クラスは、通知を処理し、受信メッセージ データを管理するアクセス許可を取得する必要があります。 次のコードは、完全な`MainActivity`実装。

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

        if (IsPlayServiceAvailable() == false)
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

`Activity`属性は、アプリケーション、設定`LaunchMode`に`SingleTop`します。 この起動モードでは、このアクティビティの 1 つのインスタンスのみを許可する Android OS を指示します。 この起動モードは、受信で`Intent`にデータをルーティング、`OnNewIntent`メソッドでは、メッセージ データを抽出しに送信します、`MainPage`インスタンスを通じて、`AddMessage`メソッド。 これを処理する必要があります、アプリケーションでは、さまざまな起動モードを使用する場合`Intent`データが異なります。

`OnCreate`メソッドが呼び出されるヘルパー メソッドを使用して`IsPlayServiceAvailable`デバイスが Google Play Service をサポートしていることを確認します。 エミュレーターまたは Google Play Service をサポートしていないデバイスは、Firebase からプッシュ通知を受信することはできません。

## <a name="configure-ios-for-notifications"></a>IOS の通知を構成します。

通知を受信する iOS アプリケーションを構成するためのプロセスは次のとおりです。

1. 構成、**バンドル識別子**で、 **Info.plist**プロビジョニング プロファイルで使用される値と一致するファイル。
1. 追加、**プッシュ通知を有効にする**オプションを**Entitlements.plist**ファイル。
1. 追加、 **Xamarin.Azure.NotificationHubs.iOS**をプロジェクトに NuGet パッケージ。
1. [APNS での通知に登録](#register-for-notifications-with-apns)します。
1. [アプリケーションを Azure Notification Hub に登録し、タグにサブスクライブ](#register-with-azure-notification-hub-and-subscribe-to-tags)します。
1. [Xamarin.Forms UI に APNS 通知を追加](#add-apns-notifications-to-xamarinforms-ui)します。

次のスクリーン ショット、**プッシュ通知を有効にする**で選択したオプション、 **Entitlements.plist** Visual Studio 内のファイル。

![プッシュ通知の権利のスクリーン ショット](azure-notification-hub-images/push-notification-entitlement.png "プッシュ通知の権利")

### <a name="register-for-notifications-with-apns"></a>APNS での通知に登録します。

`FinishedLaunching`メソッドで、 **AppDelegate.cs**リモート通知に登録するファイルをオーバーライドする必要があります。 登録は、デバイスで使用されている iOS のバージョンによって異なります。 サンプル アプリケーションでの iOS プロジェクトをオーバーライド、`FinishedLaunching`メソッドを呼び出す`RegisterForRemoteNotifications`次の例に示すようにします。

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

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Azure 通知ハブに登録し、タグを購読

デバイスが中にリモート通知の登録が正常に時に、`FinishedLaunching`メソッド、iOS の呼び出しは、`RegisteredForRemoteNotifications`メソッド。 このメソッドをオーバーライドして、次の操作を実行する必要があります。

1. インスタンスを作成、`SBNotificationHub`します。
1. 既存の登録は任意の登録を解除します。
1. 通知ハブにデバイスを登録します。
1. テンプレートを使用して特定のタグをサブスクライブします。

デバイス、テンプレート、およびタグの登録の詳細については、次を参照してください。[テンプレートとタグに、Azure Notification Hub 登録](#register-templates-and-tags-with-the-azure-notification-hub)します。 次のコードでは、テンプレート、デバイスの登録を示します。

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAllAsync(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplateAsync(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
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
> リモート通知を登録することができます、ネットワーク接続がないなどの状況で失敗します。 オーバーライドすることができます、`FailedToRegisterForRemoveNotifications`登録エラーを処理するメソッド。

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Xamarin.Forms UI に APNS 通知を追加します。

デバイスがリモート通知、iOS の呼び出しを受信すると、`ReceivedRemoteNotification`メソッド。 受信メッセージの JSON の変換、`NSDictionary`オブジェクト、および`ProcessNotification`メソッドがディクショナリから値を抽出し、Xamarin.Forms の送信`MainPage`インスタンス。 `ReceivedRemoteNotifications`メソッドをオーバーライドして、呼び出す`ProcessNotification`次のコードに示すようにします。

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

## <a name="test-notifications-in-the-azure-portal"></a>Azure portal で通知をテストします。

Azure Notification Hubs では、アプリケーションがテスト メッセージを受信できることを確認することができます。 **テスト送信**通知ハブのセクションでは、ターゲット プラットフォームを選択し、メッセージを送信することができます。 設定、**タグ式に送信**に`default`用のテンプレートが登録されているアプリケーションにメッセージを送信、`default`タグ。 クリックすると、**送信**ボタンには、メッセージになるデバイスの数を含むレポートが生成されます。 次のスクリーン ショットでは、Azure portal で、Android の通知のテストを示しています。

![Azure Notification Hub のテスト メッセージのスクリーン ショット](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure Notification Hub のテスト メッセージ")

### <a name="testing-tips"></a>テストのヒント

1. アプリケーションにプッシュ通知を受信できることをテストする場合は、物理デバイスを使用する必要があります。 Android および iOS の仮想デバイスは、プッシュ通知を受信正しく構成されていない可能性があります。
1. サンプルの Android アプリケーションでは、Firebase トークンが発行されるトークンに、テンプレートを登録します。 テスト中には、新しいトークンを要求し、Azure 通知ハブに再登録する必要があります。 強制的に実行する最善の方法が、プロジェクトをクリーン、削除するには、`bin`と`obj`フォルダーの再構築および展開する前に、デバイスから、アプリケーションをアンインストールします。
1. プッシュ通知フローの多くの部分は、非同期的に実行されます。 これにより、ブレークポイントがヒットまたは予期しない順序で検出されるしていない可能性があります。 アプリケーション フローを中断することがなく実行をトレースするデバイスやデバッグのログを使用します。 Android デバイス ログを使用して、フィルター処理、`DebugTag`で指定されている`Constants`します。

## <a name="create-a-notification-dispatcher"></a>通知のディスパッチャーを作成します。

Azure Notification Hubs は、プラットフォーム間でのデバイスに通知をディスパッチするバックエンド アプリケーションを有効にします。 通知ディスパッチでこのサンプルで、 **NotificationDispatcher**コンソール アプリケーションです。 アプリケーションが含まれています、 **DispatcherConstants.cs**ファイルで、次のプロパティを定義します。

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

構成する必要があります、 **DispatcherConstants.cs**一致するように、Azure Notification Hub の構成と一致します。 値、`SubscriptionTags`プロパティは、クライアント アプリで使用する値と一致する必要があります。 `NotificationHubName`プロパティは、Azure Notification Hub インスタンスの名前。 `FullAccessConnectionString`プロパティは、通知ハブで見つかったアクセス キー**アクセス ポリシー**します。 次のスクリーン ショットの場所、`NotificationHubName`と`FullAccessConnectionString`Azure portal でのプロパティ。

![Azure Notification Hub の名前と FullAccessConnectionString のスクリーン ショット](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure 通知ハブの名前と FullAccessConnectionString")

コンソール アプリケーションをループの各`SubscriptionTags`値し、のインスタンスを使用してサブスクライバーに通知を送信、`NotificationHubClient`クラス。 次のコードは、コンソール アプリケーション`Program`クラス。

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

サンプルのコンソール アプリケーションを実行すると、ときにメッセージを送信する、space キーを押すことができます。 正しく構成されている番号付きの通知、アプリケーションが受信する必要があります、クライアントを実行しているデバイスが用意されています。

## <a name="related-links"></a>関連リンク

* [プッシュ通知テンプレート](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)します。
* [デバイスの登録管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)します。
* [ルーティングとタグ式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)します。
* [Xamarin.Android の Azure Notification Hubs チュートリアル](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)します。
* [Xamarin.iOS の Azure Notification Hubs チュートリアル](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)します。
