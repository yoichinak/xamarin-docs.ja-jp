---
title: フォア グラウンド サービス
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 47e1eda2f701b654f81f664050847677fba8bcc5
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986035"
---
# <a name="foreground-services"></a>フォア グラウンド サービス

フォア グラウンド サービスは、特殊な種類のバインドされているサービスまたは開始されるサービスです。 場合によってはサービスでは、ユーザーがアクティブに意識する必要があるタスクを実行、これらのサービスと呼ばれます_フォア グラウンド サービス_します。 フォア グラウンド サービスの例では、方向が運転または徒歩中にユーザーを提供しているアプリです。 場合でも、アプリは、バック グラウンドでは、サービスに適切に機能するための十分なリソースと、ユーザーがアプリにアクセスする迅速かつ便利な手段を持っていることも重要です。 Android アプリで、つまり、フォア グラウンド サービスは、「通常の」サービスよりも高い優先順位を受信する必要があります、フォア グラウンド サービスを提供する必要があります、`Notification`サービスが実行されている限り、Android が表示されます。
 
フォア グラウンド サービスを開始するには、アプリは Android サービスを開始するかを示す、インテントをディスパッチする必要があります。 サービスする必要があります登録 Android とフォア グラウンド サービスとして。 Android 8.0 (またはそれ以降) で実行されているアプリを使用する必要があります、`Context.StartForegroundService`古いバージョンの Android デバイスで実行されているアプリを使用する必要があります、サービスを開始する方法 `Context.StartService`

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

フォア グラウンド サービスが開始する必要がありますそれ自体が登録 Android を呼び出すことによって、 [ `StartForeground`](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)します。 サービスが開始された場合、`Service.StartForegroundService`メソッドを登録しません自体には、Android は、サービスを停止し、応答しないように、アプリにフラグを設定し、します。

`StartForeground` どちらも必須の 2 つのパラメーターを受け取ります。
 
* 整数値は、サービスを識別するために、アプリケーション内で一意です。
* A`Notification`サービスが実行されている限り、ステータス バーに Android が表示されるオブジェクト。

サービスが実行されている限り、android のステータス バーで、通知が表示されます。 少なくとも、通知は、サービスが実行されていることをユーザーに視覚的な合図を提供します。 理想的には、通知は、アプリケーションを制御するには、アプリケーションまたはいくつかのアクション ボタン可能性がありますのショートカットを持つユーザーを提供する必要があります。 この例は、ミュージック プレーヤー&ndash;通知が表示されますが一時停止]/[play ミュージック、前の曲を振り返ってまたは、次の楽曲にスキップするボタンがあります。 

このコード スニペットでは、フォア グラウンド サービスとしてのサービスを登録する例を示します。   

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

前回の通知は、次のようなあるステータス バーの通知が表示されます。

![ステータス バーの通知を示す画像](foreground-services-images/foreground-services-01.png "ステータス バーの通知を示す画像")

このスクリーン ショットは、サービスを制御するユーザーを許可する 2 つのアクションを含む通知トレイの展開の通知を示しています。

![展開された通知を示す画像](foreground-services-images/foreground-services-02.png "イメージが展開された通知を表示します。")

通知の詳細についてで使用できる、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)のセクション、 [Android の通知](~/android/app-fundamentals/notifications/index.md)ガイド。

## <a name="unregistering-as-a-foreground-service"></a>フォア グラウンド サービスとして登録を解除します。

サービスを除外一覧できます自体フォア グラウンド サービスとしてメソッドを呼び出して`StopForeground`します。 `StopForeground` サービス停止されませんが、通知アイコンとシグナルに応じて、このサービスをシャット ダウンすることが Android が削除されます。

表示されるステータス バーの通知も渡すことによって削除`true`メソッド。 

```csharp
StopForeground(true);
```

呼び出して、サービスが停止した場合`StopSelf`または`StopService`、ステータス バーの通知は削除されます。

## <a name="related-links"></a>関連リンク

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForeground](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
