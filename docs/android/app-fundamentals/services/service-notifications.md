---
title: サービスの通知
description: このガイドでは、ユーザーに情報をディスパッチするローカル通知を Android サービスで使用可能性がある方法について説明します。
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d56f67254a9eae334fa8ac3f08d3ef270800c309
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61012357"
---
# <a name="service-notifications"></a>サービスの通知

_このガイドでは、ユーザーに情報をディスパッチするローカル通知を Android サービスで使用可能性がある方法について説明します。_


## <a name="service-notifications-overview"></a>サービス通知の概要

サービスの通知は、Android アプリケーションがフォア グラウンドにない場合でも、ユーザーに情報を表示するアプリを許可します。 通知のアプリケーションからアクティビティを表示するなど、ユーザーの操作を提供することができます。 次のコード サンプルでは、サービスがありますユーザーに通知をディスパッチする方法を示しています。

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

このスクリーン ショットでは、表示される通知の例を示します。

[![ステータス バーに表示される通知アイコン](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

ユーザーのスライドは、通知画面上部から下、完全な通知が表示されます。

![通知トレイに表示されます。](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>通知を更新しています

サービスが同じ通知 ID を使用して通知を再発行通知を更新するには Android を表示または必要に応じて、ステータス バーの通知を更新します。

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

通知の詳細についてで使用できる、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)のセクション、 [Android の通知](~/android/app-fundamentals/notifications/index.md)ガイド。


## <a name="related-links"></a>関連リンク

- [Android でのローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)
