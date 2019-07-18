---
title: Xamarin.iOS 用のアプリケーション ライフ サイクルのデモ
description: このドキュメントでは、これらのイベントを処理するタイミングと方法を示す、iOS アプリケーション内のアプリ デリゲートによって処理される、さまざまなライフ サイクル イベントを調べます。
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/17/2018
ms.openlocfilehash: 3beb511c03b328ecea824bf89355d056df003f3e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946194"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin.iOS 用のアプリケーション ライフ サイクルのデモ

この記事と[サンプル コード](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)ios で 4 つのアプリケーションの状態の役割を示します、`AppDelegate`メソッドの取得状態の変更されたときのアプリケーションに通知します。 アプリの状態が変わるたびに、コンソールに更新プログラムが印刷されます。

[![](application-lifecycle-demo-images/image3-sml.png "サンプル アプリ")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "アプリ、アプリの状態が変わるたびに、コンソールの更新が出力されます。")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>チュートリアル

1. 開く、**ライフ サイクル**プロジェクト、 **LifecycleDemo**ソリューション。
1. 開き、`AppDelegate`クラス。 ログ記録がアプリケーションに状態が変更されたときを示すライフ サイクル メソッドに追加されています。

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

1. デバイスまたはシミュレーターでアプリケーションを起動します。 `OnActivated` アプリが起動するときに呼び出されます。 アプリケーションがこれで、 _Active_状態。
1. シミュレーターまたはデバイスでアプリケーションをバック グラウンドで [ホーム] ボタンをクリックします。 `OnResignActivation` `DidEnterBackground`からのアプリ移行として呼び出される`Active`に`Inactive`とに、`Backgrounded`状態。 アプリケーションと見なされます、バック グラウンドで実行するアプリケーション コードのセットがないため、_中断_メモリにします。
1. フォア グラウンドに戻すことをアプリに移動します。 `WillEnterForeground` `OnActivated`はどちらも呼び出されます。

    ![](application-lifecycle-demo-images/image4.png "コンソールに出力状態の変更")

    次のビュー コント ローラーでのコード行は、アプリケーションがバック グラウンドから前景色が入力、画面に表示されるテキストを変更したときに実行されます。

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. キーを押して、**ホーム**バック グラウンドにアプリケーションを配置するボタンをクリックします。 ダブルタップし、**ホーム**アプリケーションのスイッチャーを表示するボタンをクリックします。 Iphone X、画面の下部から上へスワイプします。

    [![アプリケーションのスイッチャー](application-lifecycle-demo-images/app-switcher-sml.png "アプリケーションのスイッチャー")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. アプリケーションのスイッチャーので、アプリケーションを見つけて、上方向にスワイプ (iOS 11、長押し上隅にある赤いアイコンが表示されるまで) を削除します。

    [![実行中のアプリの削除までスワイプ](application-lifecycle-demo-images/app-switcher-swipe-sml.png "スワイプまで実行中のアプリの削除")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS では、アプリケーションを終了します。 なお`WillTerminate`アプリケーションが既にあるためには呼び出されません_中断_バック グラウンドでします。

## <a name="related-links"></a>関連リンク

- [LifecycleDemo (サンプル)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
