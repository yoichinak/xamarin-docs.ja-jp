---
title: Xamarin.iOS で backgrounding
description: バック グラウンド処理または backgrounding は、別のアプリケーションがフォア グラウンドで実行中に、バック グラウンドでタスクを実行するアプリケーションのプロセスです。 このガイドは、バック グラウンドで iOS 処理の概要として機能します。
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b22f3ef3276129f7f46c23cc1d06666f151f5ac4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783543"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.iOS で backgrounding

_バック グラウンド処理または backgrounding は、別のアプリケーションがフォア グラウンドで実行中に、バック グラウンドでタスクを実行するアプリケーションのプロセスです。このガイドは、バック グラウンドで iOS 処理の概要として機能します。_

Backgrounding モバイル アプリケーションでは、マルチタスク デスクトップ上の従来の概念と根本的に異なるです。 デスクトップ コンピューターでは、さまざまな実際の画面、能力、メモリなど、アプリケーションで使用できるリソースがあります。 アプリケーションは、サイド バイ サイドの実行し、パフォーマンスの高いを継続することと使い勝手が向上します。 モバイル デバイス、リソースより制限されます。 小さな画面では、複数のアプリケーションを表示するが困難であるし、バッテリが放電が最高速度でいくつかのアプリケーションを実行しています。 Backgrounding も、実行に必要なバック グラウンド タスクを実行するためのリソースをアプリケーションに与えるを保持する方法、foregrounded アプリケーションとデバイスの応答性の高い定数折衷案です。 IOS および Android の両方、backgrounding の準備が非常に異なる方法で処理します。

IOS で backgrounding は、アプリケーションの状態として認識し、アプリは、アプリとユーザーの動作に応じてバック グラウンドの状態との間に移動します。 iOS は、アプリを接続など、既知のバック グラウンドに必要なアプリケーションの種類として動作して、重要なタスクを完了する時間の OS の依頼、バック グラウンドで実行するいくつかのオプションも用意されています指定したアプリケーションのコンテンツで更新しています。間隔です。

このガイドのチュートリアルに付属する、しようとして、バック グラウンドでアプリケーションのタスクを実行する方法を学習します。 カバー主な概念とベスト プラクティス、およびバック グラウンドでの場所の更新プログラムを受信する現実世界のアプリを作成する手順されます。

## <a name="contents"></a>目次

1.  [iOS におけるバックグラウンド処理の概要](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [アプリケーション ライフサイクルのデモ](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS バックグラウンド処理のテクニック](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [チュートリアル: iOS でのバックグラウンド処理](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS バックグラウンド処理のガイダンス](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>まとめ

このガイドでは、iOS でのバック グラウンド処理を行うさまざまな方法が導入されました。 IOS アプリケーションの状態を説明し、backgrounding iOS アプリケーションのライフ サイクル内で果たす役割を確認します。 さらに、個々 のタスクまたは iOS のバック グラウンドで動作するアプリケーションの全体を方法お登録でしたについて学習しました。 最後に、バック グラウンドで更新プログラムを実行するアプリケーションを構築して、iOS で backgrounding の理解を強化します。



## <a name="related-links"></a>関連リンク

- [Android で backgrounding](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (サンプル)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [場所 (サンプル)](https://developer.xamarin.com/samples/monotouch/Location/)
- [単純なバック グラウンド転送 (サンプル)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS バック グラウンドで実行](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
