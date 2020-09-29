---
title: フォアグラウンド サービス
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 0736e427c5c75b0eebe9b9353322b146e6359603
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455301"
---
# <a name="foreground-services"></a>フォアグラウンド サービス

フォアグラウンドサービスは、バインドされたサービスまたは開始されたサービスの特別な種類です。 場合によっては、ユーザーが積極的に認識する必要があるタスクを実行するサービスもあります。これらのサービスは、 _フォアグラウンドサービス_と呼ばれています。 前景サービスの例としては、運転またはウォーキング中にユーザーに指示を与えるアプリがあります。 アプリがバックグラウンドで実行されている場合でも、正常に動作するのに十分なリソースがサービスにあること、およびユーザーがすばやく簡単にアプリにアクセスできるようにすることが重要です。 Android アプリの場合、フォアグラウンドサービスは "通常の" サービスより高い優先度を受け取る必要があり `Notification` ます。また、サービスが実行されている限り、android が表示するを提供する必要があります。

フォアグラウンドサービスを開始するには、アプリが Android にサービスを開始するように指示するインテントをディスパッチする必要があります。 その後、サービスは Android でフォアグラウンドサービスとして登録する必要があります。 Android 8.0 (またはそれ以降) で実行されているアプリでは、メソッドを使用してサービスを開始する必要がありますが、 `Context.StartForegroundService` 以前のバージョンの Android を使用するデバイスで実行されているアプリでは、 `Context.StartService`

この C# 拡張メソッドは、フォアグラウンドサービスを開始する方法の例です。 Android 8.0 以降では、メソッドが使用され `StartForegroundService` ます。それ以外の場合は、古いメソッドが使用され `StartService` ます。

```csharp
public static void StartForegroundServiceCompat<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>フォアグラウンドサービスとして登録する

フォアグラウンドサービスが開始されたら、を呼び出して、それ自体を Android に登録する必要があり [`StartForeground`](xref:Android.App.Service.StartForeground*) ます。 サービスがメソッドで開始されていても、自身が登録されていない場合 `Service.StartForegroundService` 、Android はサービスを停止し、応答不能としてアプリにフラグを付けます。

`StartForeground` 2つのパラメーターが必要です。どちらも必須です。

- サービスを識別するためにアプリケーション内で一意である整数値。
- サービスが実行されている `Notification` 限り、Android がステータスバーに表示するオブジェクト。

Android では、サービスが実行されている間は、ステータスバーに通知が表示されます。 少なくとも通知は、サービスが実行されていることを示す視覚的な手掛かりを提供します。 理想的には、通知によって、アプリケーションへのショートカットがユーザーに提供されます。また、場合によっては、アプリケーションを制御するためのいくつかのアクションボタンも必要です この例としては、音楽プレーヤーが &ndash; あります。表示される通知には、音楽の一時停止/再生、前の曲への巻き戻し、または次の曲に進むためのボタンがあります。 

次のコードスニペットは、サービスをフォアグラウンドサービスとして登録する例です。   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.

    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

前の通知では、次のようなステータスバーの通知が表示されます。

![ステータスバーに通知が表示されている画像](foreground-services-images/foreground-services-01.png "ステータスバーに通知が表示されている画像")

このスクリーンショットは、通知トレイに展開された通知と、ユーザーがサービスを制御するための2つのアクションを示しています。

![展開された通知を示す画像](foreground-services-images/foreground-services-02.png "展開された通知を示す画像。")

通知の詳細については、「 [Android 通知](~/android/app-fundamentals/notifications/index.md)ガイド」の「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」セクションを参照してください。

## <a name="unregistering-as-a-foreground-service"></a>フォアグラウンドサービスとして登録解除しています

サービスは、メソッドを呼び出すことによって、そのサービス自体をフォアグラウンドサービスとして除外でき `StopForeground` ます。 `StopForeground` はサービスを停止しませんが、通知アイコンを削除し、必要に応じてこのサービスをシャットダウンできることを Android に通知します。

表示されるステータスバーの通知は、メソッドに渡すことによって削除することもでき `true` ます。 

```csharp
StopForeground(true);
```

またはの呼び出しによってサービスが停止した場合 `StopSelf` `StopService` 、ステータスバーの通知は削除されます。

## <a name="related-links"></a>関連リンク

- [Android. App. サービス](xref:Android.App.Service)
- [Android. Service. StartForeground](xref:Android.App.Service.StartForeground*)
- [ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (サンプル)](/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-foregroundservicedemo)