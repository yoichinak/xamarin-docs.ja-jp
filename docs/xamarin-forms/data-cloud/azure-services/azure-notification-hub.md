---
title: Azure Notification Hubs と Xamarin. Forms を使用してプッシュ通知を送信および受信する
description: この記事では、Azure Notification Hubs を使用して、Xamarin. フォームアプリケーションにクロスプラットフォームのプッシュ通知を送信する方法について説明します。
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: c4237e9315ccc095abc72fdec24d58ffe1faebdf
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "68739227"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Azure Notification Hubs と Xamarin. Forms を使用してプッシュ通知を送信および受信する

[サンプル ](~/media/shared/download.png)Download ![Download サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

プッシュ通知は、バックエンドシステムからモバイルアプリケーションに情報を配信します。 Apple、Google、およびその他のプラットフォームにはそれぞれ独自のプッシュ通知サービス (PNS) があります。 Azure Notification Hubs を使用すると、複数のプラットフォームにわたる通知を一元化できます。これにより、バックエンドアプリケーションが1つのハブと通信できるようになります。これにより、各プラットフォーム固有の PNS に通知が配信されます。

次の手順に従って、Azure Notification Hubs をモバイルアプリに統合します。

1. [プッシュ Notification Services と Azure Notification Hub を設定](#set-up-push-notification-services-and-azure-notification-hub)します。
1. [テンプレートとタグの使用方法について説明](#register-templates-and-tags-with-the-azure-notification-hub)します。
1. [クロスプラットフォームの Xamarin. フォームアプリケーションを作成](#xamarinforms-application-functionality)します。
1. [ネイティブ Android プロジェクトをプッシュ通知用に構成](#configure-the-android-application-for-notifications)します。
1. [ネイティブ iOS プロジェクトをプッシュ通知用に構成](#configure-ios-for-notifications)します。
1. [Azure Notification Hub を使用して通知をテスト](#test-notifications-in-the-azure-portal)します。
1. [通知を送信するバックエンドアプリケーションを作成](#create-a-notification-dispatcher)します。

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>プッシュ Notification Services と Azure Notification Hub を設定する

Azure Notification Hubs と Xamarin. Forms モバイルアプリを統合することは、Azure Notification Hubs と Xamarin ネイティブアプリケーションを統合することと似ています。 [「Azure Notification Hubs を使用した Xamarin Android へのプッシュ通知」の「Azure のプッシュ通知](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)」に従って、 **fcm アプリケーション**を設定します。 Xamarin のチュートリアルを使用して、次の手順を実行します。

1. サンプルで使用される、`com.xamarin.notifysample` などの Android パッケージ名を定義します。
1. 焼討 Base コンソールから**google-services. json**をダウンロードします。 このファイルは、後の手順で Android アプリケーションに追加します。
1. Azure Notification Hub インスタンスを作成し、名前を付けます。 この記事とサンプルでは、ハブ名として `xdocsnotificationhub` を使用します。
1. FCM**サーバーキー**をコピーし、それを**API キー**として Azure Notification HUB の**Google (GCM/fcm)** に保存します。

次のスクリーンショットは、Azure Notification Hub の Google platform の構成を示しています。

![Azure Notification Hub Google 構成のスクリーンショット](azure-notification-hub-images/fcm-notification-hub-config.png "Azure Notification Hub の Google 構成")

IOS デバイスのセットアップを完了するには、macOS コンピューターが必要です。 [Azure Notification Hubs を使用した Xamarin. iOS へのプッシュ通知](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)に関する最初の手順に従って、APNS を設定します。 Xamarin のチュートリアルを使用して、次の手順を実行します。

1. IOS バンドル識別子を定義します。 この記事とサンプルでは、バンドル識別子として `com.xamarin.notifysample` を使用します。
1. 証明書署名要求 (CSR) ファイルを作成し、それを使用してプッシュ通知証明書を生成します。
1. Azure Notification Hub で、 **Apple (APNS)** の下にプッシュ通知証明書をアップロードします。

次のスクリーンショットは、Azure Notification Hub の Apple プラットフォーム構成を示しています。

![Azure Notification Hub の Apple 構成のスクリーンショット](azure-notification-hub-images/apns-notification-hub-config.png "Azure Notification Hub の Apple の構成")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Azure Notification Hub でテンプレートとタグを登録する

Azure Notification Hub では、モバイルアプリケーションをハブに登録し、テンプレートを定義してタグをサブスクライブする必要があります。 登録によって、プラットフォーム固有の PNS ハンドルが Azure Notification Hub の識別子にリンクされます。 登録の詳細については、「[登録管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)」を参照してください。

テンプレートを使用すると、デバイスでパラメーター化されたメッセージテンプレートを指定できます。 受信メッセージは、タグごとにデバイスごとにカスタマイズできます。 テンプレートの詳細については、「[テンプレート](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)」を参照してください。

タグは、ニュース、スポーツ、天気などのメッセージカテゴリをサブスクライブするために使用できます。 わかりやすくするために、サンプルアプリケーションでは、`messageParam` という1つのパラメーターと `default` という1つのタグを持つ既定のテンプレートを定義しています。 より複雑なシステムでは、ユーザー固有のタグを使用して、カスタマイズされた通知用にデバイス間でユーザーにメッセージを表示することができます。 タグの詳細については、「[ルーティングとタグ式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)」を参照してください。

メッセージを正常に受信するには、各ネイティブアプリケーションで次の手順を実行する必要があります。

1. プラットフォーム PNS から PNS ハンドルまたはトークンを取得します。
1. PNS ハンドルを Azure Notification Hub に登録します。
1. 送信メッセージと同じパラメーターを含むテンプレートを指定してください。
1. 送信メッセージの対象となるタグをサブスクライブします。

これらの手順の詳細については、「[通知用に Android アプリケーションを構成](#configure-the-android-application-for-notifications)する」および「[通知用に iOS を構成](#configure-ios-for-notifications)する」セクションの各プラットフォームについて詳しく説明しています。

## <a name="xamarinforms-application-functionality"></a>Xamarin. フォームアプリケーションの機能

サンプルの Xamarin. フォームアプリケーションでは、プッシュ通知メッセージの一覧が表示されます。 これは、指定されたプッシュ通知メッセージを UI に追加する `AddMessage` メソッドを使用して実現されます。 また、このメソッドは、重複するメッセージが UI に追加されるのを防ぎ、メインスレッドで実行され、任意のスレッドから呼び出すことができるようにします。 `AddMessage` メソッドのコードを次に示します。

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

サンプルアプリケーションには、プラットフォームプロジェクトによって使用されるプロパティを定義する**AppConstants.cs**ファイルが含まれています。 このファイルは、Azure Notification Hub の値を使用してカスタマイズする必要があります。 次のコードは、 **AppConstants.cs**ファイルを示しています。

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

@No__t_0 の次の値をカスタマイズして、サンプルアプリケーションを Azure Notification Hub に接続します。

* `NotificationHubName`: Azure portal で作成した Azure Notification Hub の名前を使用します。
* `ListenConnectionString`: この値は、Azure Notification Hub の **[アクセスポリシー]** にあります。

次のスクリーンショットは、これらの値が Azure portal に配置されている場所を示しています。

![Azure Notification Hub のアクセスポリシーのスクリーンショット](azure-notification-hub-images/notification-hub-access-policy.png "Azure Notification Hub のアクセスポリシー")

## <a name="configure-the-android-application-for-notifications"></a>通知用に Android アプリケーションを構成する

通知を受信して処理するように Android アプリケーションを構成するには、次の手順を実行します。

1. パッケージ名を、焼討 Base コンソールのパッケージ名と一致**するように**構成します。
1. 次の NuGet パッケージをインストールして、Google Play、焼討ベース、Azure Notification Hubs と対話します。
    1. GooglePlayServices。
    1. Xamarin. Messaging。
    1. Xamarin. Azure. NotificationHubs。
1. FCM セットアップ中にダウンロードした `google-services.json` ファイルをプロジェクトにコピーし、[ビルド] アクションを [`GoogleServicesJson`] に設定します。
1. [焼討 base と通信するように AndroidManifest を構成](#configure-android-manifest)します。
1. [@No__t_1 を使用して、アプリケーションを焼討ベースと Azure Notification Hub に登録](#register-using-a-custom-firebaseinstanceidservice)します。
1. [@No__t_1 を使用してメッセージを処理](#process-messages-with-a-firebasemessagingservice)します。
1. [Xamarin. フォーム UI に受信通知を追加](#add-incoming-notifications-to-the-xamarinforms-ui)します。

> [!NOTE]
> **GoogleServicesJson**ビルドアクションは、 **GooglePlayServices** NuGet パッケージの一部です。 Visual Studio 2019 では、起動時に使用可能なビルドアクションが設定されます。 ビルドアクションとして**GoogleServicesJson**が表示されない場合は、NuGet パッケージのインストール後に Visual Studio 2019 を再起動します。

### <a name="configure-android-manifest"></a>Android マニフェストを構成する

@No__t_1 要素内の `receiver` 要素を使用すると、アプリは焼討ベースと通信できます。 @No__t_0 の要素を使用すると、アプリはメッセージを処理し、Azure Notification Hub に登録できます。 完全な**Androidmanifest .xml**は、次の例のようになります。

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

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>カスタム FirebaseInstanceIdService を使用して登録する

焼討 base は、PNS 上のデバイスを一意に識別するトークンを発行します。 トークンの有効期間は長くなりますが、頻繁に更新されます。 トークンが発行または更新された場合、アプリケーションは新しいトークンを Azure Notification Hub に登録する必要があります。 登録は、`FirebaseInstanceIdService` から派生したクラスのインスタンスによって処理されます。

サンプルアプリケーションでは、`FirebaseRegistrationService` クラスは `FirebaseInstanceIdService` から継承されます。 このクラスには、`com.google.firebase.INSTANCE_ID_EVENT` を含む `IntentFilter` があります。これにより、Android OS は、焼討ベースによってトークンが発行されたときに `OnTokenRefresh` を自動的に呼び出すことができます。

次のコードは、サンプルアプリケーションのカスタム `FirebaseInstanceIdService` を示しています。

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

@No__t_1 の `SendRegistrationToServer` メソッドは、デバイスを Azure Notification Hub に登録し、テンプレートを使用してタグをサブスクライブします。 このサンプルアプリケーションでは、`default` という1つのタグと、 **AppConstants.cs**ファイルで `messageParam` という1つのパラメーターを持つテンプレートを定義しています。 登録、タグ、テンプレートの詳細については、「 [Azure Notification Hub でのテンプレートとタグの登録](#register-templates-and-tags-with-the-azure-notification-hub)」を参照してください。

### <a name="process-messages-with-a-firebasemessagingservice"></a>焼討 Basemessagingservice を使用してメッセージを処理する

受信メッセージは `FirebaseMessagingService` インスタンスにルーティングされ、ローカル通知に変換できます。 サンプルアプリケーションの Android プロジェクトには、`FirebaseMessagingService` から継承する `FirebaseService` というクラスが含まれています。 このクラスには `com.google.firebase.MESSAGING_EVENT` を含む `IntentFilter` があり、Android OS はプッシュ通知メッセージを受信したときに `OnMessageReceived` を自動的に呼び出すことができます。

次の例は、サンプルアプリケーションの `FirebaseService` を示しています。

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

受信メッセージは、`SendLocalNotification` メソッドを使用してローカル通知に変換されます。 このメソッドは、新しい `Intent` を作成し、メッセージの内容を `string` `Extra` として `Intent` に配置します。 ユーザーがローカル通知をタップすると、アプリがフォアグラウンドとバックグラウンドのどちらにあるかにかかわらず、`MainActivity` が起動され、`Intent` オブジェクトを介してメッセージの内容にアクセスできるようになります。

ローカル通知と `Intent` の例では、ユーザーは通知をタップする操作を行う必要があります。 これは、アプリケーションの状態が変化する前にユーザーがアクションを実行する必要がある場合に適しています。 ただし、場合によっては、ユーザーの操作を必要とせずに、メッセージデータにアクセスすることもできます。 また、前の例では、`SendMessageToMainPage` メソッドを使用して、現在の `MainPage` インスタンスにメッセージを直接送信します。 運用環境では、両方のメソッドを1つのメッセージ型に対して実装すると、ユーザーが通知をタップしたときに、`MainPage` オブジェクトによって重複するメッセージが表示されます。

> [!NOTE]
> Android アプリケーションは、バックグラウンドまたはフォアグラウンドで実行されている場合にのみプッシュ通知を受信します。 メイン `Activity` が実行されていないときにプッシュ通知を受信するには、サービスを実装する必要があります。これは、このサンプルの範囲を超えています。 詳細については、「 [Android サービスの作成](/xamarin/android/app-fundamentals/services/)」を参照してください。

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Xamarin. フォーム UI に受信通知を追加する

@No__t_0 クラスは、通知を処理し、受信メッセージデータを管理するためのアクセス許可を取得する必要があります。 次のコードは、`MainActivity` の完全な実装を示しています。

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

@No__t_0 属性は、アプリケーションの `LaunchMode` を `SingleTop` に設定します。 この起動モードは、このアクティビティの1つのインスタンスのみを許可するように Android OS に指示します。 この起動モードでは、受信 `Intent` データは `OnNewIntent` メソッドにルーティングされます。このメソッドは、メッセージデータを抽出し、`AddMessage` メソッドを使用して `MainPage` インスタンスに送信します。 アプリケーションで別の起動モードを使用する場合は、`Intent` データを異なる方法で処理する必要があります。

@No__t_0 メソッドでは、`IsPlayServiceAvailable` と呼ばれるヘルパーメソッドを使用して、デバイスが Google Play サービスをサポートしていることを確認します。 Google Play サービスをサポートしないエミュレーターまたはデバイスは、消火ベースからプッシュ通知を受信できません。

## <a name="configure-ios-for-notifications"></a>通知用に iOS を構成する

通知を受信するように iOS アプリケーションを構成するプロセスは次のとおりです。

1. プロビジョニングプロファイルで使用されている値と一致するように、**情報を plist**ファイルで**バンドル id**を構成します。
1. **権利の plist**ファイルに [**プッシュ通知を有効に**する] オプションを追加します。
1. プロジェクトに**Xamarin. Azure. NotificationHubs. iOS** NuGet パッケージを追加します。
1. [APNS で通知を登録](#register-for-notifications-with-apns)します。
1. [アプリケーションを Azure Notification Hub に登録し、タグをサブスクライブ](#register-with-azure-notification-hub-and-subscribe-to-tags)します。
1. [APNS の通知を Xamarin. FORMS UI に追加](#add-apns-notifications-to-xamarinforms-ui)します。

次のスクリーンショットは、Visual Studio 内の権利の**plist**ファイルで選択されている **[プッシュ通知を有効にする]** オプションを示しています。

![プッシュ通知の権利のスクリーンショット](azure-notification-hub-images/push-notification-entitlement.png "プッシュ通知の権利")

### <a name="register-for-notifications-with-apns"></a>APNS で通知を登録する

リモート通知に登録するには、 **AppDelegate.cs**ファイルの `FinishedLaunching` メソッドをオーバーライドする必要があります。 登録は、デバイスで使用されている iOS のバージョンによって異なります。 サンプルアプリケーションの iOS プロジェクトは、次の例に示すように、`FinishedLaunching` メソッドをオーバーライドして `RegisterForRemoteNotifications` を呼び出します。

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

@No__t_0 方法でデバイスがリモート通知に対して正常に登録されると、iOS は `RegisteredForRemoteNotifications` メソッドを呼び出します。 次のアクションを実行するには、このメソッドをオーバーライドする必要があります。

1. @No__t_0 をインスタンス化します。
1. 既存の登録の登録を解除します。
1. デバイスを通知ハブに登録します。
1. テンプレートを使用して特定のタグをサブスクライブします。

デバイス、テンプレート、タグの登録の詳細については、「 [Azure Notification Hub でのテンプレートとタグの登録](#register-templates-and-tags-with-the-azure-notification-hub)」を参照してください。 次のコードは、デバイスとテンプレートの登録を示しています。

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
> ネットワーク接続がないなどの状況では、リモート通知の登録に失敗することがあります。 @No__t_0 メソッドをオーバーライドして、登録の失敗を処理するように選択できます。

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>APNS の通知を Xamarin. フォーム UI に追加する

デバイスがリモート通知を受信すると、iOS は `ReceivedRemoteNotification` メソッドを呼び出します。 受信メッセージ JSON は `NSDictionary` オブジェクトに変換され、`ProcessNotification` メソッドはディクショナリから値を抽出し、それらを Xamarin. Forms `MainPage` インスタンスに送信します。 @No__t_0 メソッドは、次のコードに示すように、`ProcessNotification` を呼び出すようにオーバーライドされます。

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

## <a name="test-notifications-in-the-azure-portal"></a>Azure portal でのテスト通知

Azure Notification Hubs を使用すると、アプリケーションがテストメッセージを受信できるかどうかを確認できます。 通知ハブの **[テスト送信]** セクションでは、ターゲットプラットフォームを選択し、メッセージを送信することができます。 [**タグの送信] 式**を `default` に設定すると、`default` タグのテンプレートを登録したアプリケーションにメッセージが送信されます。 **[送信]** ボタンをクリックすると、メッセージに到達したデバイスの数を含むレポートが生成されます。 次のスクリーンショットは、Azure portal での Android の通知テストを示しています。

![Azure Notification Hub のテストメッセージのスクリーンショット](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure Notification Hub のテストメッセージ")

### <a name="testing-tips"></a>テストのヒント

1. アプリケーションがプッシュ通知を受信できることをテストする場合は、物理デバイスを使用する必要があります。 Android および iOS の仮想デバイスは、プッシュ通知を受信するように正しく構成されていない可能性があります。
1. サンプル Android アプリケーションは、焼討 Base トークンが発行されたときにトークンとテンプレートを登録します。 テスト中に、新しいトークンを要求して、Azure Notification Hub に再登録することが必要になる場合があります。 これを強制する最良の方法は、プロジェクトをクリーンアップし、`bin` と `obj` のフォルダーを削除し、再構築と配置の前にデバイスからアプリケーションをアンインストールすることです。
1. プッシュ通知フローの多くの部分は、非同期的に実行されます。 これにより、ブレークポイントがヒットしたり、予期しない順序でヒットしたりする可能性があります。 アプリケーションフローを中断せずに、デバイスまたはデバッグログを使用して実行をトレースします。 @No__t_1 で指定された `DebugTag` を使用して、Android デバイスのログをフィルター処理します。

## <a name="create-a-notification-dispatcher"></a>通知ディスパッチャーを作成する

Azure Notification Hubs は、バックエンドアプリケーションがプラットフォーム間でデバイスに通知をディスパッチできるようにします。 このサンプルでは、 **Notificationdispatcher**コンソールアプリケーションを使用した通知ディスパッチを示します。 このアプリケーションには、次のプロパティを定義する**DispatcherConstants.cs**ファイルが含まれています。

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

**DispatcherConstants.cs**は、Azure Notification Hub の構成と一致するように構成する必要があります。 @No__t_0 プロパティの値は、クライアントアプリで使用される値と一致している必要があります。 @No__t_0 プロパティは、Azure Notification Hub インスタンスの名前です。 @No__t_0 プロパティは、notification hub の**アクセスポリシー**で見つかったアクセスキーです。 次のスクリーンショットは、Azure portal 内の `NotificationHubName` と `FullAccessConnectionString` プロパティの場所を示しています。

![Azure Notification Hub の名前と FullAccessConnectionString のスクリーンショット](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure Notification Hub の名前と FullAccessConnectionString")

コンソールアプリケーションは、各 `SubscriptionTags` 値をループし、`NotificationHubClient` クラスのインスタンスを使用してサブスクライバーに通知を送信します。 次のコードは、コンソールアプリケーション `Program` クラスを示しています。

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

サンプルコンソールアプリケーションを実行すると、space キーを押してメッセージを送信できます。 クライアントアプリケーションを実行しているデバイスは、適切に構成されていれば、番号付きの通知を受信する必要があります。

## <a name="related-links"></a>関連リンク

* [プッシュ通知テンプレート](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)。
* [デバイス登録管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)。
* [ルーティングとタグ式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)。
* [Xamarin Android Azure Notification Hubs チュートリアル](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)。
* [Xamarin. IOS Azure Notification Hubs チュートリアル](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)。
