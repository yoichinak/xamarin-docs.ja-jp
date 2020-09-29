---
title: Xamarin でのグループ化された通知 (iOS)
description: IOS 12 では、通知センターで通知をグループ化することも、アプリケーションまたはスレッドごとにロック画面をグループ化することもできます。 このドキュメントでは、Xamarin. iOS でスレッド化された通知とスレッドなしの通知を送信する方法について説明します。
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 8c440d61a41fc26aa7537dc169be08c2bf413c68
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435212"
---
# <a name="grouped-notifications-in-xamarinios"></a>Xamarin でのグループ化された通知 (iOS)

既定では、iOS 12 はすべてのアプリの通知をグループに配置します。 ロック画面と通知センターには、このグループが一番上に最新の通知を含むスタックとして表示されます。 ユーザーは、グループを展開して、含まれているすべての通知を表示し、グループ全体を閉じることができます。

アプリでは、スレッド別に通知をグループ化することもできます。これにより、ユーザーは興味のある特定の情報を簡単に見つけて操作できるようになります。

## <a name="sample-app-groupednotifications"></a>サンプルアプリ: GroupedNotifications

Xamarin. iOS でグループ化された通知を使用する方法については、 [Groupednotifications](/samples/xamarin/ios-samples/ios12-groupednotifications) サンプルアプリを参照してください。

このサンプルアプリでは、さまざまな友人とのメッセージ交換をシミュレートし、各メッセージの通知を送信して、スレッド別にグループ化します。 また、スレッド化されていない通知をアプリケーションレベルのグループに配置する方法も示します。

このガイドのコードスニペットは、このサンプルアプリから抜粋したものです。

## <a name="request-authorization-and-allow-foreground-notifications"></a>承認を要求し、フォアグラウンド通知を許可する

アプリでローカル通知を送信するには、そのためのアクセス許可を要求する必要があります。 サンプルアプリのでは [`AppDelegate`](xref:UIKit.UIApplicationDelegate) 、 [`FinishedLaunching`](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary)) メソッドはこのアクセス許可を要求します。

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

の [`Delegate`](xref:UserNotifications.UNUserNotificationCenter.Delegate) (上記の設定) では、 [`UNUserNotificationCenter`](xref:UserNotifications.UNUserNotificationCenter) に渡された完了ハンドラーを呼び出すことにより、フォアグラウンドアプリで受信通知を表示するかどうかを決定し [`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions})) ます。

```csharp
[Export("userNotificationCenter:willPresentNotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

パラメーターは、アプリが警告を表示して [`UNNotificationPresentationOptions.Alert`](xref:UserNotifications.UNNotificationPresentationOptions) も、サウンドを再生したりバッジを更新したりしないことを示します。

## <a name="threaded-notifications"></a>スレッド通知

" **Alice" と** いう名前のサンプルアプリのメッセージを繰り返しタップして、alice という名前の友人とのメッセージ交換について通知を送信するようにします。
このメッセージ交換の通知は同じスレッドの一部であるため、ロック画面と通知センターはそれらをまとめてグループ化します。

別の友人とのメッセージ交換を開始するには、[ **新しいフレンドを選択** します] ボタンをタップします。 このメッセージ交換の通知は、別のグループに表示されます。

### <a name="threadidentifier"></a>ThreadIdentifier

サンプルアプリは、新しいスレッドを開始するたびに、一意のスレッド識別子を作成します。

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

スレッド化された通知を送信するには、サンプルアプリを次のようにします。

- アプリが通知を送信する承認を持っているかどうかを確認します。
- を作成します。 [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
通知の内容を表すオブジェクトです。 [`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier)
前に作成したスレッド識別子にします。
- 要求を作成し、通知をスケジュールします。

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
> リモート通知にスレッド識別子を設定するには、 `thread-id` 通知の JSON ペイロードにキーを追加します。 詳細については、Apple の [リモート通知の生成](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) に関するドキュメントを参照してください。

### <a name="summaryargument"></a>引数の概要

`SummaryArgument` 通知が属する通知グループの左下隅に表示される概要テキストに通知を適用する方法を指定します。 iOS では、同じグループ内の通知から概要テキストを集計して、概要説明全体を作成します。

サンプルアプリでは、メッセージの作成者を summary 引数として使用します。 この方法を使用すると、Alice との6つの通知のグループの概要テキストは、 **alice と Me からの 5**つの通知になることがあります。

## <a name="unthreaded-notifications"></a>未スレッド通知

サンプルアプリの **予定リマインダー** ボタンをタップするたびに、さまざまな予定リマインダー通知が送信されます。 これらのリマインダーはスレッド化されていないため、ロック画面と通知センターのアプリケーションレベルの通知グループに表示されます。

スレッド化されていない通知を送信するために、サンプルアプリの `ScheduleUnthreadedNotification` メソッドは上記と同様のコードを使用します。
ただし、オブジェクトには設定されません `ThreadIdentifier` `UNMutableNotificationContent` 。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– GroupedNotifications](/samples/xamarin/ios-samples/ios12-groupednotifications)
- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [グループ化された通知の使用 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)