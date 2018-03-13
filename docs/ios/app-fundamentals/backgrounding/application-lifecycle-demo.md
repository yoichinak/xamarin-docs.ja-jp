---
title: "アプリケーション ライフ サイクルのデモ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3d68b1e38ecb5b1833b818dd2a9fb7a5c84f9206
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="application-lifecycle-demo"></a>アプリケーション ライフ サイクルのデモ

このセクションで行う、4 つのアプリケーションの状態との役割を示すアプリケーションを調べて、`AppDelegate`メソッドで示される状態が変更の取得をアプリケーションに通知します。 アプリの状態が変わるたびにコンソールに更新プログラムが印刷されます。

 [![](application-lifecycle-demo-images/image3.png "サンプル アプリ")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "アプリは、アプリの状態が変わるたびに、コンソールに更新プログラムは印刷します。")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>チュートリアル


  1. 開く、_ライフ サイクル_プロジェクトで、 _LifecycleDemo_ソリューションです。
  1. 開き、`AppDelegate`クラスです。 ログ状態が変更されたとき、アプリケーションを知らせ、ライフ サイクル メソッドに追加したことに注意してください。

            ```chsarp
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

  1. シミュレーターまたはデバイスにアプリケーションを起動します。 `OnActivated` アプリが起動するときに呼び出されます。 アプリケーション内では現在、 _Active_状態です。
  1. シミュレーターまたはデバイスで、バック グラウンド アプリケーション [ホーム] ボタンをヒットします。 `OnResignActivation` および`DidEnterBackground`からアプリの遷移として呼び出される`Active`に`Inactive`とに、`Backgrounded`状態です。 アプリケーションでは、バック グラウンドで実行するためのコード与えられていないこと、ので、アプリケーションと見なされます_Suspended_メモリにします。
  1. フォア グラウンドに戻すことをアプリに移動します。 `WillEnterForeground` および`OnActivated`がどちらも呼び出されます。

        ![](application-lifecycle-demo-images/image4.png "状態の変更は、コンソールに出力")

    当社ビュー コント ローラーが、アプリケーションがバック グラウンドから前景色を入力したことを通知する次のコード行を追加おことに注意してください。

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. キーを押して、**ホーム**背景にアプリケーションを配置するボタンをクリックします。 その後、ダブルタップ、**ホーム**アプリケーションのスイッチャーを表示するにはボタン。
    
    ![](application-lifecycle-demo-images/app-switcher-.png "アプリケーションのスイッチャー")
  
1. アプリ スイッチャーでアプリケーションを見つけて削除上方向にスワイプします。
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "上方向にスワイプ実行中のアプリを削除するには") 
    
iOS アプリケーションが終了されます。 注意してください`WillTerminate`おられているアプリケーションを終了しているためには呼び出されません_Suspended_バック グラウンドでします。

IOS アプリケーションの状態と遷移を理解したらは、iOS で backgrounding で使用できるさまざまなオプションを見てをみましょう。



## <a name="related-links"></a>関連リンク

- [LifecycleDemo(Part2) (サンプル)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
