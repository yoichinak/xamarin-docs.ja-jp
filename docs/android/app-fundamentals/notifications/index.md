---
title: "Xamarin.Android での通知"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bc39faa37adae660a7751313d0d573237fadce94
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="notifications-in-xamarinandroid"></a>Xamarin.Android での通知


## <a name="overview"></a>概要

このセクションでは、Xamarin.Android に通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、API について説明します、通知が表示の作成と関係しているのです。


## <a name="sections"></a>セクション

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Android でのローカルの通知](local-notifications.md)

このセクションでは、Xamarin.Android にローカルの通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、API について説明を作成して、メッセージを表示するのに含まれているのです。 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[チュートリアル - Xamarin.Android でローカルの通知の使用](local-notifications-walkthrough.md)  
 
このチュートリアルでは、ローカルの通知を Xamarin.Android アプリケーションで使用する方法について説明します。 作成して、通知の発行の基本を示します。 通知ドロワーの通知で、ユーザーがクリックしたときに、2 番目のアクティビティを開始します。 


## <a name="for-further-reading"></a>関連項目

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase クラウド メッセージング (FCM) は、モバイル アプリとサーバー アプリケーション間のメッセージングを容易にするサービスです。 Firebase Cloud Messaging で指定できます (プッシュ通知とも呼ばれます)、リモートの通知を実装する Xamarin.Android アプリケーションです。

[通知](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; Android 開発者向けのトピックでは、Android の通知をわかりやすく案内します。 Android のユーザー インターフェイスのガイドラインに準拠しているため、通知をデザインする際に役立つに関する考慮事項」の設計が含まれています。 アクティビティの開始時に、preserviing ナビゲーションに関する背景情報を提供し、ロック画面で通知とコントロールのメディア再生で進行状況を表示する方法について説明します。 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash;この Android サービスにより、アプリを聴く (および対話) するためにアプリが登録されている通知だけでなく、Android デバイスにポストされたすべての通知を受信します。 ユーザーができるように、デバイスで通知をリッスンするには、アプリへのアクセス許可を付与する必要があります明示的に注意してください。





## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [リモート通知 (サンプル)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
