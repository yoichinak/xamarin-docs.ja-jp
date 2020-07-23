---
title: Xamarin. iOS のアプリケーションライフサイクルデモ
description: このドキュメントでは、iOS アプリケーションでアプリデリゲートによって処理されるさまざまなライフサイクルイベントについて説明し、これらのイベントを処理するタイミングと方法を示します。
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/17/2018
ms.openlocfilehash: bb3fd0623d0361a42c573cf2b2bcb8249d32181c
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933184"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin. iOS のアプリケーションライフサイクルデモ

この記事と[サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)では、iOS の4つのアプリケーション状態と、 `AppDelegate` 状態が変化したときにをアプリケーションに通知するためのメソッドの役割について説明します。 アプリが状態を変更するたびに、アプリケーションはコンソールに更新を出力します。

[![サンプル アプリ](application-lifecycle-demo-images/image3-sml.png)](application-lifecycle-demo-images/image3.png#lightbox)

[![アプリが状態を変更するたびに、アプリによって更新内容がコンソールに出力されます。](application-lifecycle-demo-images/image4.png)](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>チュートリアル

1. **LifecycleDemo**ソリューションで**ライフサイクル**プロジェクトを開きます。
1. クラスを開き `AppDelegate` ます。 アプリケーションの状態がいつ変更されたかを示すログがライフサイクルメソッドに追加されました。

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. シミュレーターまたはデバイスでアプリケーションを起動します。 `OnActivated`アプリが起動すると、が呼び出されます。 これで、アプリケーションは_アクティブ_状態になります。
1. シミュレーターまたはデバイスの [ホーム] ボタンをクリックして、アプリケーションをバックグラウンドにします。 `OnResignActivation`と `DidEnterBackground` は、アプリがからに、状態に遷移するときに呼び出され `Active` `Inactive` `Backgrounded` ます。 バックグラウンドで実行するように設定されたアプリケーションコードがないため、アプリケーションはメモリ内で_中断_されていると見なされます。
1. アプリに戻り、前面に戻ります。 `WillEnterForeground`と `OnActivated` は両方とも呼び出されます。

    ![コンソールに印刷される状態の変更](application-lifecycle-demo-images/image4.png)

    ビューコントローラーの次のコード行は、アプリケーションが背景からフォアグラウンドに入ったときに実行され、画面に表示されるテキストを変更します。

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. [**ホーム**] ボタンをクリックして、アプリケーションを背景に配置します。 次に、[**ホーム**] ボタンをダブルタップして、アプリケーションスイッチャーを開きます。 IPhone X で、画面の下部から上方向にスワイプします。

    [![アプリケーションスイッチャー](application-lifecycle-demo-images/app-switcher-sml.png "アプリケーションスイッチャー")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. アプリスイッチャーでアプリケーションを見つけ、上にスワイプして削除します (iOS 11 では、赤いアイコンが隅に表示されるまで長く押します)。

    [![上にスワイプして実行中のアプリを削除する](application-lifecycle-demo-images/app-switcher-swipe-sml.png "上にスワイプして実行中のアプリを削除する")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS によってアプリケーションが終了します。 `WillTerminate`アプリケーションがバックグラウンドで既に_中断_されているため、が呼び出されないことに注意してください。

## <a name="related-links"></a>関連リンク

- [LifecycleDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
