---
title: Xamarin. iOS での仮通知
description: このドキュメントでは、Xamarin を使用して仮の通知を操作する方法について説明します。 IOS 12 で導入された一時的な通知により、アプリケーションは、明示的なユーザーアクセス許可なしに通知を送信できます。
ms.prod: xamarin
ms.assetid: 5DCB36B9-2637-48AE-8FC0-F6124F08AC48
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: d321e8061d3091abeaa3cff6a6af9172c981cb60
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291199"
---
# <a name="provisional-notifications-in-xamarinios"></a>Xamarin. iOS での仮通知

仮通知を使用すると、ユーザーが明示的に事前に同意しなくても、アプリは通知を配信できます。 これらの通知は、通知センターにのみ表示され、ユーザーが継続的な配信をオプトインまたはオプトアウトする前にプレビューすることができます。

通知センターでは、一時的な通知の配信を停止するか、仮の配信を継続するか、またはより目立つように配信を開始するかをユーザーが指定できます。

## <a name="sample-app-redgreennotifications"></a>サンプルアプリ:RedGreenNotifications

仮の通知を送信する[RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)サンプルアプリを見てみましょう。

## <a name="sending-provisional-notifications"></a>仮通知の送信

仮通知を送信するに`UNAuthorizationOptions.Provisional`は、のオプションとしてを指定します。[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)
`UNUserNotificationCenter`方法:

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

ユーザーが仮の通知を目立つ配信に昇格する`UNAuthorizationOptions`と、に`RequestAuthorization`渡される値によって、新しい通知配信設定が決定`UNAuthorizationOptions.Alert`さ`UNAuthorizationOptions.Sound`れます (上のコードでは、と)。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
