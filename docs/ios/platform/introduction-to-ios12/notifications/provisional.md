---
title: Xamarin.iOS での一時的な通知
description: このドキュメントでは、Xamarin.iOS を使用して、一時的な通知と連携する方法について説明します。 IOS 12 で導入された、一時的な通知は、明示的なユーザーのアクセス許可がない通知の停止の通知を送信するアプリケーションを許可します。
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 31b4c6e98945cd7b5dd4cea8be6f5e3857444f78
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131677"
---
# <a name="provisional-notifications-in-xamarinios"></a>Xamarin.iOS での一時的な通知

一時的な通知は、ユーザーの明示的な事前の同意なく通知を配信するアプリを許可します。 これらの通知は、サイレント モードで到着し、ユーザーがそれらの継続的配信の内外でのオプトインの前にプレビューできるように、通知センターにのみを表示します。

通知センターでユーザーことができますかを指定ことアプリ provisional 通知の配信を停止、続行して仮、配信に目立つの配信を開始します。

## <a name="sample-app-redgreennotifications"></a>サンプル アプリ: RedGreenNotifications

見て、 [RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)サンプル アプリは、一時的な通知を送信します。

## <a name="sending-provisional-notifications"></a>一時的な通知を送信します。

一時的な通知を送信する提供`UNAuthorizationOptions.Provisional`にオプションとして、 [`RequestAuthorization`](https://developer.xamarin.com/api/member/UserNotifications.UNUserNotificationCenter.RequestAuthorization/)
メソッドの`UNUserNotificationCenter`:

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

ユーザー、著名な配信を一時的な通知を昇格する場合、`UNAuthorizationOptions`に渡される値`RequestAuthorization`新しい通知の配信設定が決定されます (上記のコードで`UNAuthorizationOptions.Alert`と`UNAuthorizationOptions.Sound`)。

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS でのユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザーへの通知 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ユーザーへの通知 (WWDC 2017) の新機能新機能およびベスト プラクティス](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知 (Apple) を生成します。](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
