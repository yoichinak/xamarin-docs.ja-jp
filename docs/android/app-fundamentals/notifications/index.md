---
title: Xamarin.Android での通知
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: ce19a286a288846a67b73e77d31a5da4c77a2462
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455587"
---
# <a name="notifications-in-xamarinandroid"></a>Xamarin.Android での通知

このセクションでは、Xamarin Android で通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。

## <a name="local-notifications-in-android"></a>[Android でのローカル通知](local-notifications.md)

このセクションでは、Xamarin Android でローカル通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する Api について説明します。

## <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>[チュートリアル-Xamarin. Android でのローカル通知の使用](local-notifications-walkthrough.md)  

このチュートリアルでは、Xamarin Android アプリケーションでローカル通知を使用する方法について説明します。 ここでは、通知の作成と公開の基本について説明します。 通知ドロワーで通知をクリックすると、2つ目のアクティビティが開始されます。 

## <a name="further-reading"></a>関連項目

[焼討 Base Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; 焼討 base Cloud Messaging (FCM) は、モバイルアプリとサーバーアプリケーション間のメッセージングを容易にするサービスです。 焼討 base Cloud Messaging を使用して、Xamarin Android アプリケーションでリモート通知 (プッシュ通知とも呼ばれます) を実装できます。

[通知](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; この Android 開発者向けトピックは、Android 通知の最終ガイドです。 このガイドには、Android ユーザーインターフェイスのガイドラインに準拠するように通知をデザインする際に役立つ、設計に関する考慮事項のセクションが含まれています。 アクティビティを開始するときの移動を維持するための背景情報を提供します。また、通知で進行状況を表示し、ロック画面でメディアの再生を制御する方法についても説明します。

[NotificationListenerService](xref:Android.Service.Notification.NotificationListenerService) &ndash; この Android サービスを使用すると、アプリが受信するように登録されている通知だけでなく、Android デバイスにポストされたすべての通知をアプリでリッスン (および操作) できるようになります。
ユーザーは、デバイスで通知をリッスンできるようにするために、アプリに対するアクセス許可を明示的に付与する必要があることに注意してください。

## <a name="related-links"></a>関連リンク

- [ローカル通知 (サンプル)](/samples/xamarin/monodroid-samples/localnotifications)