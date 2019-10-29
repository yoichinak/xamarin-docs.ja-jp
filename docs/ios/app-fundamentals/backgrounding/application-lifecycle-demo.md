---
title: Xamarin. iOS のアプリケーションライフサイクルデモ
description: このドキュメントでは、iOS アプリケーションでアプリデリゲートによって処理されるさまざまなライフサイクルイベントについて説明し、これらのイベントを処理するタイミングと方法を示します。
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/17/2018
ms.openlocfilehash: 13f34f6287d68736ee509e6fb43e5fc47321b907
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73011201"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin. iOS のアプリケーションライフサイクルデモ

この記事と[サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)では、iOS の4つのアプリケーション状態と、状態が変化したときにアプリケーションに通知するための `AppDelegate` メソッドの役割について説明します。 アプリが状態を変更するたびに、アプリケーションはコンソールに更新を出力します。

[![](application-lifecycle-demo-images/image3-sml.png "The sample app")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "The app will print updates to the console whenever the app changes state")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>チュートリアル

1. **LifecycleDemo**ソリューションで**ライフサイクル**プロジェクトを開きます。
1. `AppDelegate` クラスを開きます。 アプリケーションの状態がいつ変更されたかを示すログがライフサイクルメソッドに追加されました。

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

1. シミュレーターまたはデバイスでアプリケーションを起動します。 `OnActivated` は、アプリの起動時に呼び出されます。 これで、アプリケーションは_アクティブ_状態になります。
1. シミュレーターまたはデバイスの [ホーム] ボタンをクリックして、アプリケーションをバックグラウンドにします。 `OnResignActivation` と `DidEnterBackground` は、アプリが `Active` から `Inactive` および `Backgrounded` 状態に遷移するときに呼び出されます。 バックグラウンドで実行するように設定されたアプリケーションコードがないため、アプリケーションはメモリ内で_中断_されていると見なされます。
1. アプリに戻り、前面に戻ります。 `WillEnterForeground` と `OnActivated` は両方とも呼び出されます。

    ![](application-lifecycle-demo-images/image4.png "State changes printed to the console")

    ビューコントローラーの次のコード行は、アプリケーションが背景からフォアグラウンドに入ったときに実行され、画面に表示されるテキストを変更します。

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. **[ホーム]** ボタンをクリックして、アプリケーションを背景に配置します。 次に、 **[ホーム]** ボタンをダブルタップして、アプリケーションスイッチャーを開きます。 IPhone X で、画面の下部から上方向にスワイプします。

    [![アプリケーションスイッチャー](application-lifecycle-demo-images/app-switcher-sml.png "アプリケーションスイッチャー")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. アプリスイッチャーでアプリケーションを見つけ、上にスワイプして削除します (iOS 11 では、赤いアイコンが隅に表示されるまで長く押します)。

    [![上にスワイプして実行中のアプリを削除する](application-lifecycle-demo-images/app-switcher-swipe-sml.png "上にスワイプして実行中のアプリを削除する")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS によってアプリケーションが終了します。 アプリケーションがバックグラウンドで既に_中断_されているため、`WillTerminate` は呼び出されないことに注意してください。

## <a name="related-links"></a>関連リンク

- [LifecycleDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
