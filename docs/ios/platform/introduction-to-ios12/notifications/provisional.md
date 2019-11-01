---
title: Xamarin. iOS での仮通知
description: このドキュメントでは、Xamarin を使用して仮の通知を操作する方法について説明します。 IOS 12 で導入された一時的な通知により、アプリケーションは、明示的なユーザーアクセス許可なしに通知を送信できます。
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 7d9fe2a651d8d75d8dd9d8c0dd1225350a58373d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031887"
---
# <a name="provisional-notifications-in-xamarinios"></a>Xamarin. iOS での仮通知

仮通知を使用すると、ユーザーが明示的に事前に同意しなくても、アプリは通知を配信できます。 これらの通知は、通知センターにのみ表示され、ユーザーが継続的な配信をオプトインまたはオプトアウトする前にプレビューすることができます。

通知センターでは、一時的な通知の配信を停止するか、仮の配信を継続するか、またはより目立つように配信を開始するかをユーザーが指定できます。

## <a name="sample-app-redgreennotifications"></a>サンプルアプリ: RedGreenNotifications

仮の通知を送信する[RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)サンプルアプリを見てみましょう。

## <a name="sending-provisional-notifications"></a>仮通知の送信

仮通知を送信するには、 [`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)にオプションとして `UNAuthorizationOptions.Provisional` を指定します
`UNUserNotificationCenter`のメソッド:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

ユーザーが仮の通知を著名な配信に昇格すると、`RequestAuthorization` に渡される `UNAuthorizationOptions` の値によって、新しい通知配信設定が決定されます (上のコード、`UNAuthorizationOptions.Alert` および `UNAuthorizationOptions.Sound`)。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
