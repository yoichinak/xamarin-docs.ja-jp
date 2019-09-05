---
title: Xamarin. iOS の重大なアラート
description: このドキュメントでは、Xamarin iOS で重大なアラートを使用する方法について説明します。 IOS 12 で導入された重大なアラートは、"応答不可" がオンになっているか、着信音スイッチがオフになっているかどうかに関係なく、音を鳴らす破壊的な通知です。
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/04/2018
ms.openlocfilehash: 54a214215f77b66f6a4b134dcb8d27b26c44fb6c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291287"
---
# <a name="critical-alerts-in-xamarinios"></a>Xamarin. iOS の重大なアラート

IOS 12 では、アプリは重大なアラートを送信できます。 重大なアラートは、"応答不可" が有効になっているかどうか、または着信音スイッチがオフになっているかどうかに関係なく、音を鳴らします。 これらの通知は中断されており、ユーザーが直ちに対処する必要がある場合にのみ使用してください。

## <a name="custom-critical-alert-entitlement"></a>カスタムの重大なアラートの権利

重要なアラートをアプリで表示するには、まず Apple から[カスタムの重大なアラート通知の権利を要求](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/)します。

Apple からこの権利を受け取り、それを使用するようにアプリを構成する方法について、関連する指示に従って、アプリの**権利の plist**ファイルにカスタムの[権利](~/ios/deploy-test/provisioning/entitlements.md)を追加します。 次に、シミュレーターとデバイスの両方でアプリに署名するときに、使用する**IOS バンドル署名**オプションを構成し**ます。**

## <a name="request-authorization"></a>承認を要求する

アプリの通知承認要求によって、アプリの通知を許可または禁止するように求めるメッセージがユーザーに表示されます。 通知承認要求によって重要なアラートを送信するアクセス許可が求められた場合、アプリでは、重要なアラートを選択する機会をユーザーに与えることもできます。

次のコードでは、適切なを渡すことによって、重大なアラートと標準の通知とサウンドの両方を送信するアクセス許可を要求します。[`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
[`RequestAuthorization`](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*)値:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.CriticalAlert;
    center.RequestAuthorization(options, (bool success, NSError error) => {
        // ...
    );
    return true;
}
```

## <a name="local-critical-alerts"></a>ローカルの重大なアラート

ローカルの重大なアラートを送信するには、[`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
次のいずれ`Sound`かのプロパティを設定します。

- `UNNotificationSound.DefaultCriticalSound`。既定の重大な通知音を使用します。
- `UNNotificationSound.GetCriticalSound`。アプリとボリュームにバンドルされているカスタムサウンドを指定できます。

次に、通知`UNNotificationRequest`コンテンツからを作成し、通知センターに追加します。

```csharp
var content = new UNMutableNotificationContent()
{
    Title = "Critical alert title",
    Body = "Text of the critical alert",
    CategoryIdentifier = "my-critical-alert-category",
    // Sound = UNNotificationSound.DefaultCriticalSound
    Sound = UNNotificationSound.GetCriticalSound("my_critical_sound.m4a", 1.0f)
};

var request = UNNotificationRequest.FromIdentifier(
    Guid.NewGuid().ToString(),
    content,
    UNTimeIntervalNotificationTrigger.CreateTrigger(3, false)
);

var center = UNUserNotificationCenter.Current;
center.AddNotificationRequest(request, null);
```

> [!IMPORTANT]
> 重要なアラートは、アプリで有効になっていない場合は配信されません。 重要なアラートを送信するアクセス許可をアプリが初めて要求するときに表示されるプロンプトと共に、ユーザーは iOS**設定**アプリのアプリの**通知**セクションで重要なアラートを有効または無効にすることもできます。

## <a name="remote-critical-alerts"></a>リモートの重大なアラート

リモートの重大なアラートの詳細については、「WWDC 2018 からの[ユーザー通知セッションの新機能](https://developer.apple.com/videos/play/wwdc2018/710/)」および「[リモート通知の生成](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
