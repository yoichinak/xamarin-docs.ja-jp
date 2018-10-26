---
title: Xamarin.iOS でのユーザー インターフェイスの対話型通知
description: IOS 12 は、ローカルとリモート通知の対話型ユーザー インターフェイスを作成できます。 このガイドでは、Xamarin.iOS でこれらの機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 9/4/2018
ms.openlocfilehash: 5a3a737360f898c2638bc5431cb8f8fc4882f3b0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131434"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Xamarin.iOS でのユーザー インターフェイスの対話型通知

[Notification content 拡張機能](~/ios/platform/user-notifications/advanced-user-notifications.md)、iOS 10 で導入された、使用して通知のカスタム ユーザー インターフェイスを作成できます。 IOS 12 以降では、通知のユーザー インターフェイスは、ボタンとスライダーなどの対話型の要素を含めることができます。

## <a name="sample-app-redgreennotifications"></a>サンプル アプリ: RedGreenNotifications

[RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)サンプル アプリには、対話型ユーザー インターフェイスでの notification content 拡張機能が含まれています。

このガイドでのコード スニペットは、このサンプルから取得されます。

## <a name="notification-content-extension-infoplist-file"></a>Notification content 拡張機能の Info.plist ファイル

サンプル アプリで、 **Info.plist**ファイル、 **RedGreenNotificationsContentExtension**プロジェクトには、次の構成が含まれています。

```xml
<!-- ... -->
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>UNNotificationExtensionCategory</key>
        <array>
            <string>red-category</string>
            <string>green-category</string>
        </array>
        <key>UNNotificationExtensionUserInteractionEnabled</key>
        <true/>
        <key>UNNotificationExtensionDefaultContentHidden</key>
        <true/>
        <key>UNNotificationExtensionInitialContentSizeRatio</key>
        <real>0.6</real>
    </dict>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.usernotifications.content-extension</string>
    <key></key>
    <true/>
</dict>
<!-- ... -->
```

次の機能に注意してください。

- `UNNotificationExtensionCategory`配列通知カテゴリの種類を指定するコンテンツの拡張のハンドル。
- 対話型コンテンツをサポートするために notification content 拡張機能の設定、`UNNotificationExtensionUserInteractionEnabled`キー`true`します。
- `UNNotificationExtensionInitialContentSizeRatio`キーは、コンテンツの拡張機能のインターフェイスの初期の高さと幅の比率を指定します。

## <a name="interactive-interface"></a>対話型インターフェイス

**MainInterface.storyboard**、1 つのビュー コント ローラーを含む標準のストーリー ボードは、通知コンテンツの拡張インターフェイスを定義します。 サンプル アプリでビュー コント ローラーは、型の`NotificationViewController`イメージのビュー、3 つのボタンとスライダーが含まれています。 ストーリー ボードで定義されているハンドラーを使用してこれらのコントロールに関連付けます**NotificationViewController.cs**:

- **アプリの起動**ボタン ハンドラーの呼び出し、`PerformNotificationDefaultAction`アクション メソッドで`ExtensionContext`アプリを起動します。

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    アプリでは、ユーザーの通知センターの`Delegate`(サンプル アプリで、 `AppDelegate`) 内での対話に応答できる、`DidReceiveNotificationResponse`メソッド。

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- **通知の閉じる**ハンドラー呼び出しをボタン`DismissNotificationContentExtension`で`ExtensionContext`通知を終了します。

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- **削除通知** ボタンのハンドラーが通知を閉じるし、通知センターから削除されます。

    ```csharp
    partial void HandleRemoveNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
        UNUserNotificationCenter.Current.RemoveDeliveredNotifications(new string[] { notification.Request.Identifier });
    }
    ```

- スライダーの値の変更を処理するメソッドは、通知のインターフェイスに表示されるイメージのアルファを更新します。

    ```csharp
    partial void HandleSliderValueChanged(UISlider sender)
    {
        Xamagon.Alpha = sender.Value;
    }
    ```

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – RedGreenNotifications](https://developer.xamarin.com/samples/monotouch/iOS12/RedGreenNotifications)
- [Xamarin.iOS でのユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザーへの通知 (WWDC 2018) の新機能新機能](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ユーザーへの通知 (WWDC 2017) の新機能新機能およびベスト プラクティス](https://developer.apple.com/videos/play/wwdc2017/708/)
- [豊富な通知 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [リモート通知 (Apple) を生成します。](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
