---
title: バック グラウンドで実行する Xamarin.iOS アプリケーションの登録
description: このドキュメントでは、バック グラウンドで実行する Xamarin.iOS アプリケーションを登録する方法について説明します。 オーディオ アプリ、VoIP アプリ、外部アクセサリと bluetooth、および詳細を説明します。
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a0a66571d0249ef6fd65ff382f14c38f48a8af37
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105169"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>バック グラウンドで実行する Xamarin.iOS アプリケーションの登録

一部のアプリケーションでは、GPS を使用してユーザーの方向を取得するなど、重要な実行時間の長いタスクを実行すると、アプリケーションが常に呼び出された場合の動作はバック グラウンド特権 works に対する個々 のタスクを登録しますか。 など、これらのアプリケーションは、既知のバック グラウンドに必要なアプリケーションとして登録する代わりにする必要があります。

アプリを登録する通知を iOS アプリケーションがバック グラウンドでタスクを実行するために必要な特別な特権で指定します。

## <a name="application-registration-categories"></a>アプリケーション登録のカテゴリ

登録済みのアプリは、いくつかのカテゴリに分類されることができます。

-  **オーディオ**-ミュージック プレーヤーやオーディオ コンテンツを操作する他のアプリケーションを引き続き登録される可能性も、アプリが不要になった、フォア グラウンドでオーディオを再生します。 このカテゴリでのアプリでは、オーディオを再生またはバック グラウンドでダウンロード以外は何をしようとすると、iOS によって終了されます。
-  **VoIP** -バック グラウンドでのオーディオの処理を保持するオーディオのアプリケーションに与えられている同じ特権の音声経由でインターネット プロトコルです (VoIP) アプリケーションを取得します。 VoIP サービス ノードの電源をその接続を維持するのに必要に応じて応答もできます。
-  **外部アクセサリと Bluetooth** -Bluetooth デバイスとその他の外部のハードウェア アクセサリと通信する必要があるアプリケーション用に予約されて、これらのカテゴリに登録は、ハードウェアに接続したままアプリを許可します。
-  **Newsstand** -A Newsstand アプリケーション コンテンツをバック グラウンドの同期を続行できます。
-  **場所**アプリケーションを使用して、GPS の - またはネットワークの場所データが送信し、バック グラウンドで位置情報の更新を受信します。
-  **フェッチ (iOS 7 以降)** - アプリケーションの登録は、バック グラウンド フェッチ特権は、アプリケーションに戻るときに更新されたコンテンツを持つユーザーを表示する一定の間隔で新しいコンテンツのプロバイダーを確認できます。
-  **リモート通知 (iOS 7 以降)** -アプリケーションが、プロバイダーから通知を受け取るための登録し、ユーザーがアプリケーションを開く前に、更新プログラムを開始するために、通知を使用します。 通知では、プッシュ通知の形式で取得したり、アプリケーションを自動的にスリープ解除することを選択することができます。


設定してアプリケーションを登録することができます、**バック グラウンド モードのために必要な**アプリケーションのプロパティ*Info.plist*します。 アプリケーションは、必要な数のカテゴリに登録できます。

 [![](registering-applications-to-run-in-background-images/bgmodes.png "バック グラウンド モードを設定")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

場所の更新プログラムをバック グラウンド アプリケーションを登録するステップ バイ ステップ ガイドを参照してください、[バック グラウンド場所チュートリアル](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md)します。

## <a name="application-does-not-run-in-background-property"></a>アプリケーションは、Background プロパティでは実行されません。

別のプロパティで設定できる*Info.plist*は、*アプリケーションがバック グラウンドで実行されない*、または`UIApplicationExitsOnSuspend`プロパティ。

 [![](registering-applications-to-run-in-background-images/plist.png "バック グラウンド実行を無効にします。")](registering-applications-to-run-in-background-images/plist.png#lightbox)

これをオフ iOS 7 以降では、開発者側でのみ変更できる点を除いてバック グラウンド アプリの更新設定を設定するとまったく同じ効果が、iOS 4 以降で使用可能です。 アプリケーションでは、バック グラウンドで入力した直後が中断され、処理を実行することはできません。

アプリケーションが予期しない動作を防ぐことと、バック グラウンド処理を処理するために設計されていない場合は、このプロパティを使用します。

## <a name="background-fetch-and-remote-notifications"></a>バック グラウンド フェッチとリモート通知

バック グラウンド フェッチとリモート通知は、iOS 7 で導入された特別な登録カテゴリです。 これらのカテゴリは、プロバイダーから新しいコンテンツを受信し、バック グラウンドで更新するアプリケーションを許可します。 次のセクションでは、フェッチおよび詳しくは、リモート通知について説明しも iOS 6 でバック グラウンドでアプリケーションを更新するための手段として、位置認識を紹介します。
