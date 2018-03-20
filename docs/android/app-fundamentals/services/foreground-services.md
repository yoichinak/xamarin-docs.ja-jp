---
title: "フォア グラウンド サービス"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: d1267bc4a530deb6dfb6eb2e30bee2facabd8fed
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2018
---
# <a name="foreground-services"></a>フォア グラウンド サービス

フォア グラウンド サービスは、特殊な種類のバインドされているサービスまたは開始されるサービスです。 場合によってはサービスでは、ユーザーがアクティブに認識する必要があるタスクを実行、これらのサービスと呼ばれます_フォア グラウンド サービス_です。 フォア グラウンド サービスの例は、運転または徒歩中に指示をユーザーに提供されているアプリです。 アプリは、バック グラウンドでは、場合でもはサービスが正常に動作するための十分なリソースであると、ユーザーがアプリへのアクセスを迅速かつ便利な方法を持っていることも重要です。 Android アプリで、つまり、フォア グラウンド サービスは、「標準」サービスよりも高い優先順位を受信する必要があります、フォア グラウンド サービスを提供する必要があります、`Notification`サービスが実行されている限り、Android が表示されます。
 
フォア グラウンド サービスを開始するには、アプリは、サービスを開始する Android に通知する目的をディスパッチする必要があります。 その後サービス必要があります登録自体 Android とフォア グラウンド サービスとして。 Android 8.0 で (またはそれ以上) に実行されているアプリを使用する必要があります、`Context.StartForegroundService`古いバージョンの Android デバイスで実行されているアプリが使用中に、サービスを開始するメソッド `Context.StartService`

この c# 拡張メソッドは、フォア グラウンド サービスを開始する方法の例です。 Android 8.0 以降が使用されます、`StartForegroundService`メソッド、それ以外の場合、古い`StartService`メソッドが使用されます。  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
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

## <a name="registering-as-a-foreground-service"></a>フォア グラウンド サービスとして登録します。

フォア グラウンド サービスが開始する必要があります自体が登録 Android を呼び出すことによって、 [ `StartForeground`](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)です。 サービスが開始された場合、`Service.StartForegroundService`メソッド自体が登録されない、Android は、サービスを停止し、応答しないように、アプリのフラグを設定しがします。

`StartForeground` どちらも必須の 2 つのパラメーターを受け取ります。
 
* サービスの識別にアプリケーション内で一意である整数値。
* A`Notification`サービスが実行されている限り、Android がステータス バーに表示されるオブジェクト。

サービスが実行されている限り、android のステータス バーに通知が表示されます。 少なくとも、通知は、サービスが実行されていることをユーザーに視覚上の手掛かりを提供します。 理想的には、通知は、アプリケーションを制御するには、アプリケーションまたは一部のアクション ボタン可能性のあるへのショートカットを使用して、ユーザーを提供する必要があります。 この例は、ミュージック プレーヤー&ndash;表示されている通知は、前の曲に戻るか、次の楽曲にスキップ、音楽の再生/一時停止するためにボタンを必要があります。 

このコード スニペットは、フォア グラウンド サービスとして、サービスを登録する次の例を示します。   

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

以前の通知が次のように、ステータス バー通知が表示されます。

![ステータス バーの通知の画像](foreground-services-images/foreground-services-01.png "ステータス バーの通知の画像")

このスクリーン ショットは、サービスを制御するユーザーに許可する 2 つのアクションがあるトレイで展開された通知を示しています。

![展開された通知の画像](foreground-services-images/foreground-services-02.png "イメージが展開された通知を表示します。")

通知の詳細についてで使用できる、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)のセクションで、 [Android 通知](~/android/app-fundamentals/notifications/index.md)ガイドです。

## <a name="unregistering-as-a-foreground-service"></a>フォア グラウンド サービスとして登録解除しています

サービス除外一覧できます自体、フォア グラウンド サービスとしてメソッドを呼び出して`StopForeground`です。 `StopForeground` サービスは停止されませんが、通知アイコンと必要な場合、このサービスをシャット ダウンすることができます Android の信号が削除されます。

表示されるステータス バーの通知も渡すことによって削除`true`メソッドに。 

```csharp
StopForeground(true);
```

呼び出して、サービスが停止した場合`StopSelf`または`StopService`、ステータス バーの通知が削除される予定です。

## <a name="related-links"></a>関連リンク

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
