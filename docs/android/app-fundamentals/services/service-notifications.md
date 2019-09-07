---
title: サービスの通知
description: このガイドでは、Android サービスがローカル通知を使用してユーザーに情報をディスパッチする方法について説明します。
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 5d25604db1f88702f4c24df21b3ebba6c9c2fc95
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754817"
---
# <a name="service-notifications"></a>サービスの通知

_このガイドでは、Android サービスがローカル通知を使用してユーザーに情報をディスパッチする方法について説明します。_

## <a name="service-notifications-overview"></a>サービス通知の概要

サービス通知を使用すると、Android アプリケーションがフォアグラウンドにない場合でも、アプリはユーザーに情報を表示できます。 通知によって、アプリケーションからのアクティビティの表示など、ユーザーに対するアクションが提供される可能性があります。 次のコードサンプルは、サービスがユーザーに通知をディスパッチする方法を示しています。

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

次のスクリーンショットは、表示される通知の例です。

[![ステータスバーに表示される通知アイコン](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

ユーザーが通知画面の上部からスライドダウンすると、完全な通知が表示されます。

![通知トレイに表示される鈴木](service-notifications-images/02-fullnotification.png)

## <a name="updating-a-notification"></a>通知を更新しています

通知を更新するために、サービスは同じ通知 ID を使用して通知を再発行します。 Android では、必要に応じて、ステータスバーに通知が表示または更新されます。

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

通知の詳細については、「 [Android 通知](~/android/app-fundamentals/notifications/index.md)ガイド」の「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」セクションを参照してください。

## <a name="related-links"></a>関連リンク

- [Android でのローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)
