---
title: Xamarin.iOS でグループ化された通知
description: IOS 12、通知センターまたはアプリケーションまたはスレッドのロック画面で通知をグループにことはできます。 このドキュメントでは、スレッドを送信する方法と Xamarin.iOS で連結の通知について説明します。
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 278986b29e629995a202f474242670f5524c45ff
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111602"
---
# <a name="grouped-notifications-in-xamarinios"></a>Xamarin.iOS でグループ化された通知

既定では、iOS 12 は、すべてのアプリの通知をグループに配置します。 ロック画面と通知センターは、このグループを上部に、最新の通知を持つスタックとして表示されます。 ユーザーが含まれており、グループ全体を無視するすべての通知を表示するグループを展開できます。

アプリは、スレッド、検索対象となる特定の情報にアクセスしてユーザーが簡単でグループの通知でこともできます。

## <a name="sample-app-groupednotifications"></a>サンプル アプリ: GroupedNotifications

Xamarin.iOS でグループ化された通知を使用する方法についてを参照してください、 [GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications)サンプル アプリです。

このサンプル アプリでは、各メッセージの通知を送信する、スレッドによってグループ化して、さまざまな友人との会話をシミュレートします。 また、アプリケーション レベルのグループに通知 land を連結する方法を示します。

このガイドでのコード スニペットは、このサンプル アプリから取得されます。

## <a name="request-authorization-and-allow-foreground-notifications"></a>承認を要求し、フォア グラウンドの通知を許可します。

アプリがローカル通知を送信する前に、そのアクセス許可を要求する必要があります。 サンプル アプリの[ `AppDelegate` ](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/)、 [ `FinishedLaunching` ](https://developer.xamarin.com/api/member/UIKit.UIApplicationDelegate.FinishedLaunching/p/UIKit.UIApplication/Foundation.NSDictionary/)メソッドは、このアクセス許可を要求します。

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    center.RequestAuthorization(UNAuthorizationOptions.Alert, (bool success, NSError error) =>
    {
        // Set the Delegate regardless of success; users can modify their notification
        // preferences at any time in the Settings app.
        center.Delegate = this;
    });
    return true;
}
```

[ `Delegate` ](https://developer.xamarin.com/api/property/UserNotifications.UNUserNotificationCenter.Delegate/) (上記) に対して設定、 [ `UNUserNotificationCenter` ](https://developer.xamarin.com/api/type/UserNotifications.UNUserNotificationCenter/)フォア グラウンド アプリがに渡される、完了ハンドラーを呼び出すことによって、受信した通知を表示する必要があるかどうかを決定[`WillPresentNotification`](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification/p/UserNotifications.IUNUserNotificationCenterDelegate/UserNotifications.UNUserNotificationCenter/UserNotifications.UNNotification/System.Action%7BUserNotifications.UNNotificationPresentationOptions%7D/):

```csharp
[Export("userNotificationCenter:willPresentotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

[ `UNNotificationPresentationOptions.Alert` ](https://developer.xamarin.com/api/type/UserNotifications.UNNotificationPresentationOptions/)パラメーターまたはことを示しますアプリする必要がありますアラートを表示しますが、いないサウンドを再生するバッジを更新します。

## <a name="threaded-notifications"></a>スレッドの通知

サンプル アプリのタップ**メッセージを Alice と**ボタンを繰り返し Alice という名前の友人との会話の通知を送信させます。
このメッセージ交換の通知は、同じスレッドの一部であるため、ロック画面と通知センター グループ化します。

異なる friend を使用して、メッセージ交換を開始するには、タップ、**新しい friend を選択して**ボタンをクリックします。 別のグループで、このメッセージ交換の通知が表示されます。

### <a name="threadidentifier"></a>ThreadIdentifier

サンプル アプリは、新しいスレッドを開始、いつでもスレッドの一意の識別子が作成されます。

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

スレッドの通知、サンプル アプリを送信するには。

- アプリに通知を送信するための承認があるかどうかを確認します。
- 作成します。 [`UNMutableNotificationContent`](https://developer.xamarin.com/api/type/UserNotifications.UNMutableNotificationContent/)
オブジェクトの通知のコンテンツと設定、 [`ThreadIdentifier`](https://developer.xamarin.com/api/property/UserNotifications.UNMutableNotificationContent.ThreadIdentifier/)
上記で作成したスレッドの識別子。
- 依頼を作成し、通知をスケジュールします。

```csharp
async partial void ScheduleThreadedNotification(UIButton sender)
{
    var center = UNUserNotificationCenter.Current;

    UNNotificationSettings settings = await center.GetNotificationSettingsAsync();
    if (settings.AuthorizationStatus != UNAuthorizationStatus.Authorized)
    {
        return;
    }

    string author =  // ...
    string message = // ...

    var content = new UNMutableNotificationContent()
    {
        ThreadIdentifier = threadId,
        Title = author,
        Body = message,
        SummaryArgument = author
    };

    var request = UNNotificationRequest.FromIdentifier(
        Guid.NewGuid().ToString(),
        content,
        UNTimeIntervalNotificationTrigger.CreateTrigger(1, false)
    );

    center.AddNotificationRequest(request, null);

    // ...
}
```

同じスレッド識別子を持つ同じアプリからのすべての通知は、同じ通知グループに表示されます。

> [!NOTE]
> リモート通知スレッド識別子を設定するには、追加、`thread-id`通知の JSON ペイロードにキー。 Apple を参照してください。[リモート通知を生成する](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)詳細についてはドキュメントです。

### <a name="summaryargument"></a>SummaryArgument

`SummaryArgument` 通知が、通知が所属する通知先グループの左下隅に表示される概要テキストをどのように影響するかを指定します。 iOS では、全体的な概要の説明を作成する、同じグループ内の通知の概要のテキストを集約します。

サンプル アプリでは、集計の引数として、メッセージの作成者が使用されます。 このアプローチを使用して、alice の 6 つの通知のグループの概要のテキストがある可能性があります**Alice と私からさらに 5 通知**します。

## <a name="unthreaded-notifications"></a>連結の通知

各サンプル アプリのタップ**アラームを設定**ボタンがさまざまな予定の事前通知のいずれかを送信します。 これらの通知がスレッドはないため、ロック画面上のアプリケーション レベルの通知グループには通知センターで表示されます。

通知を送信する連結、サンプル アプリの`ScheduleUnthreadedNotification`メソッドは、上記と同様のコードを使用します。
ただしは設定しません、`ThreadIdentifier`上、`UNMutableNotificationContent`オブジェクト。

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – GroupedNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/GroupedNotifications)
- [Xamarin.iOS でのユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [ユーザーへの通知 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/710/)
- [グループ化された通知 (WWDC 2018) を使用します。](https://developer.apple.com/videos/play/wwdc2018/711/)
- [ユーザーへの通知 (WWDC 2017) の新機能新機能およびベスト プラクティス](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知 (Apple) を生成します。](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
