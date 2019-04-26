---
title: Xamarin.Android での通知
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b98ee5afbd65d5cf32bc6e3151284678e248cf47
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61014252"
---
# <a name="notifications-in-xamarinandroid"></a>Xamarin.Android での通知


## <a name="overview"></a>概要

このセクションでは、Xamarin.Android で通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、API について説明を作成して、通知を表示するのに関係するのです。


## <a name="sections"></a>セクション

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Android でのローカル通知](local-notifications.md)

このセクションでは、Xamarin.Android でローカル通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、API について説明を作成して、通知を表示するのに伴うのです。 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[チュートリアル - Xamarin.Android でローカル通知の使用](local-notifications-walkthrough.md)  
 
このチュートリアルでは、Xamarin.Android アプリケーションでローカル通知を使用する方法について説明します。 作成と公開通知の基本を示します。 通知ドロワーでは、通知、ユーザーがクリックしたときに、2 番目のアクティビティを開始します。 


## <a name="for-further-reading"></a>関連項目

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) はモバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。 Xamarin.Android アプリケーションで、リモート通知が (プッシュ通知とも呼ばれます) を実装するして firebase Cloud Messaging を使用することができます。

[通知](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; Android 開発者向けのトピックでは、Android の通知の完全なガイドです。 Android ユーザー インターフェイスのガイドラインに準拠するよう、通知をデザインするのに役立つに関する考慮事項」の設計が含まれています。 アクティビティを開始するときに、preserviing ナビゲーションに関する背景情報を提供し、ロック画面に通知し、コントロールのメディア再生の進行状況を表示する方法について説明します。 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; Android ではこのサービスにより、アプリをリッスンし (操作) に、アプリが登録されている通知だけでなく、Android デバイスですべての通知が投稿された受信します。 ユーザーが、デバイスへの通知をリッスンできるようにするため、アプリへのアクセス許可を明示的に付ける必要がありますに注意してください。





## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [リモート通知 (サンプル)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
