---
title: Xamarin.iOS で重大なアラート
description: このドキュメントでは、Xamarin.iOS で重大なアラートを使用する方法について説明します。 IOS 12 で導入された重大なアラートは、サウンドを再生するが応答しないかどうかに関係なく停止の通知または着信音スイッチはオフです。
ms.prod: xamarin
ms.assetid: 75742257-081D-44F4-B49E-FB807DF85262
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 699d19228d2dee92f7a730bba4186a3aa5f21b04
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233238"
---
# <a name="critical-alerts-in-xamarinios"></a>Xamarin.iOS で重大なアラート

12、iOS アプリは重大なアラートを送信できます。 重大なアラートは、応答不可が有効にするかどうかに関係なくサウンドを再生または着信音スイッチはオフです。 これらの通知は、中断を伴うユーザーが早急に対処する必要があるときにだけ使用する必要があります。

## <a name="custom-critical-alert-entitlement"></a>カスタムの重要なアラートの権利

アプリでは、最初に重大なアラートを表示する[重大なアラート通知をカスタム エンタイトルメントを要求](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/)Apple から。

Apple からこの権利を受け取り、それを使用するアプリを構成する方法の詳細についてすべての関連付けられている手順に従って、カスタムの追加後[権利](~/ios/deploy-test/provisioning/entitlements.md)アプリの**Entitlements.plist**ファイルします。 次に、構成、 **iOS バンドル署名**オプションを使用する**Entitlements.plist**シミュレーターとデバイスの両方でアプリに署名するとき。

## <a name="request-authorization"></a>要求の承認

アプリの通知の承認要求は、アプリの通知を許可または拒否するユーザーに求めます。 通知の承認要求が重大なアラートを送信するアクセス許可を求める場合、アプリもユーザーをできるように重大なアラートを選択します。

次のコードは、適切なを渡すことによって、重大なアラートと通知の標準とサウンドの両方を送信するアクセス許可を要求します。 [`UNAuthorizationOptions`](xref:UserNotifications.UNAuthorizationOptions)
値を[ `RequestAuthorization` ](xref:UserNotifications.UNUserNotificationCenter.RequestAuthorization*):

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

ローカルの重大なアラートを送信するには、作成します。 [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
設定とその`Sound`プロパティをいずれか。

- `UNNotificationSound.DefaultCriticalSound`、既定の重要な通知サウンドが使用されます。
- `UNNotificationSound.GetCriticalSound`でのアプリやボリュームにバンドルはカスタム サウンドを指定できます。

次に、作成、`UNNotificationRequest`通知のコンテンツし、通知センターへの追加。

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
> アプリのも有効でない場合、重大なアラートは配信されません。 最初に表示されるプロンプトと時刻の重大なアラートを送信するアプリの要求のアクセス許可、ユーザーの有効またはアプリの重大なアラートを無効にすることができますも**通知**、iOS のセクション**設定**アプリ。

## <a name="remote-critical-alerts"></a>リモートの重大なアラート

リモートの重大なアラートについてを参照してください、[はユーザー通知で新しい](https://developer.apple.com/videos/play/wwdc2018/710/)WWDC 2018 からのセッションと[リモート通知を生成する](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)ドキュメント。

## <a name="related-links"></a>関連リンク

- [Xamarin.iOS でのユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザーへの通知 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ユーザーへの通知 (WWDC 2017) の新機能新機能およびベスト プラクティス](https://developer.apple.com/videos/play/wwdc2017/708/)
- [リモート通知 (Apple) を生成します。](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
