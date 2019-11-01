---
title: Xamarin の対話型通知ユーザーインターフェイス
description: IOS 12 では、ローカルおよびリモートの通知用の対話型ユーザーインターフェイスを作成することができます。 このガイドでは、Xamarin iOS でこれらの機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: E3562E1B-E0EF-4C99-9F51-59DE22AFDE46
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: e629cd8f481558991d02c7fb879502ebd54753bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031940"
---
# <a name="interactive-notification-user-interfaces-in-xamarinios"></a>Xamarin の対話型通知ユーザーインターフェイス

IOS 10 で導入された[通知コンテンツ拡張機能](~/ios/platform/user-notifications/advanced-user-notifications.md)により、通知用のカスタムユーザーインターフェイスを作成できるようになります。 IOS 12 以降、通知ユーザーインターフェイスには、ボタンやスライダーなどの対話的な要素を含めることができます。

## <a name="sample-app-redgreennotifications"></a>サンプルアプリ: RedGreenNotifications

[RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)サンプルアプリには、対話ユーザーインターフェイスを備えた notification content 拡張機能が含まれています。

このガイドのコードスニペットは、このサンプルを基にしています。

## <a name="notification-content-extension-infoplist-file"></a>Notification content 拡張機能の情報 plist ファイル

サンプルアプリでは、 **RedGreenNotificationsContentExtension**プロジェクトの**情報の plist**ファイルに次の構成が含まれています。

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

- `UNNotificationExtensionCategory` 配列は、コンテンツ拡張機能が処理する通知カテゴリの種類を指定します。
- 対話形式のコンテンツをサポートするために、notification content 拡張機能は `UNNotificationExtensionUserInteractionEnabled` キーを `true`に設定します。
- `UNNotificationExtensionInitialContentSizeRatio` キーは、コンテンツ拡張機能のインターフェイスの初期の高さと幅の比率を指定します。

## <a name="interactive-interface"></a>対話型インターフェイス

通知コンテンツ拡張機能のインターフェイスを定義する**Maininterface storyboard**は、1つのビューコントローラーを含む標準のストーリーボードです。 このサンプルアプリでは、ビューコントローラーは `NotificationViewController`型で、イメージビュー、3つのボタン、およびスライダーが含まれています。 ストーリーボードは、 **NotificationViewController.cs**で定義されているハンドラーとこれらのコントロールを関連付けます。

- **アプリの起動**ボタンハンドラーは `ExtensionContext`で `PerformNotificationDefaultAction` アクションメソッドを呼び出し、アプリを起動します。

    ```csharp
    partial void HandleLaunchAppButtonTap(UIButton sender)
    {
        ExtensionContext.PerformNotificationDefaultAction();
    }
    ```

    アプリでは、ユーザー通知センターの `Delegate` (サンプルアプリでは `AppDelegate`) が `DidReceiveNotificationResponse` メソッドの対話に応答できます。

    ```csharp
    [Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
    public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
    {
        if (response.IsDefaultAction)
        {
            Console.WriteLine("ACTION: Default");
            // ...
    ```

- **通知**を閉じるボタンハンドラーは `ExtensionContext`で `DismissNotificationContentExtension` を呼び出し、通知を閉じます。

    ```csharp
    partial void HandleDismissNotificationButtonTap(UIButton sender)
    {
        ExtensionContext.DismissNotificationContentExtension();
    }
    ```

- 通知の**削除**ボタンハンドラーによって通知が破棄され、通知センターから削除されます。

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

- [サンプルアプリ– RedGreenNotifications](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Xamarin. iOS のユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)
- [UserNotifications (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [ユーザー通知の新機能 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [ベストプラクティスとユーザー通知の新機能 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [豊富な通知 (WWDC 2017)](https://developer.apple.com/videos/play/wwdc2017/817/)
- [リモート通知の生成 (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
