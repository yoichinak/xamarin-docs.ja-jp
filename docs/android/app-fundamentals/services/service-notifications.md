---
title: "サービスの通知"
description: "このガイドでは、Android のサービスが、ユーザーに情報をディスパッチするローカルの通知を使用可能性がある方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 3c06fca9c6d8c3cd91889007bd1879149771622b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="service-notifications"></a>サービスの通知

_このガイドでは、Android のサービスが、ユーザーに情報をディスパッチするローカルの通知を使用可能性がある方法について説明します。_


## <a name="service-notifications-overview"></a>サービスの通知の概要

サービス通知は、Android アプリケーションがフォア グラウンドでない場合でも、ユーザーに情報を表示するアプリを許可します。 通知のアプリケーションからアクティビティを表示するなど、ユーザーのアクションを提供することができます。 次のコード サンプルでは、サービスがありますユーザーに通知をディスパッチする方法を示しています。

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

このスクリーン ショットは、表示される通知の例を示します。

[![ステータス バーに表示されている通知アイコン](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

ユーザーのスライド上、通知の画面上部から下、全体の通知が表示されます。

![通知のトレイに表示されます。](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>通知の更新

更新するには、通知サービスが同じ通知 ID を使用して通知を再発行します。 Android を表示または必要に応じて、ステータス バーの通知を更新します。

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

通知の詳細についてで使用できる、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)のセクションで、 [Android 通知](~/android/app-fundamentals/notifications/index.md)ガイドです。


## <a name="related-links"></a>関連リンク

- [Android でのローカルの通知](~/android/app-fundamentals/notifications/local-notifications.md)
