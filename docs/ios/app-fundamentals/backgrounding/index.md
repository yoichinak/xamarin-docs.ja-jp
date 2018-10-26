---
title: Xamarin.iOS でバック グラウンド処理
description: バック グラウンド処理または backgrounding は、別のアプリケーションがフォア グラウンドで実行中に、アプリケーションにバック グラウンドでタスクを実行させる処理です。 このガイドは、iOSにおけるバック グラウンド処理の概要を説明します。
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: a4f5112b6e77ab6e00453c19c766d1e905df1144
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122778"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.iOS でバック グラウンド処理

_バック グラウンド処理またはバック グラウンド処理は、アプリケーションが別のアプリケーションがフォア グラウンドで実行中にバック グラウンドでタスクを実行できるようにすることのプロセスです。このガイドは、バック グラウンド処理で iOS の概要として機能します。_

モバイル アプリケーションのバック グラウンド処理は、デスクトップ上のマルチタスクの従来の概念と根本的に異なります。 デスクトップ コンピューターでは、さまざまな実際の画面、能力、メモリなど、アプリケーションで使用可能なリソースがあります。 アプリケーションは、サイド バイ サイドで実行して、パフォーマンスを維持して、使用可能です。 モバイル デバイスでは、リソースがはるかに限られます。 小さな画面では、1 つ以上のアプリケーションを表示することは困難ですし、バッテリーの消耗が最高速度でいくつかのアプリケーションを実行します。 バック グラウンド処理、うまく実行に必要なバック グラウンド タスクを実行するためのリソースをアプリケーションに与えると、応答性の高い foregrounded アプリケーションとデバイスを保持する定数折衷案です。 IOS と Android の両方があるため、バック グラウンド処理の準備が非常にさまざまな方法で処理します。

Ios でバック グラウンド処理は、アプリケーションの状態として認識し、アプリとユーザーの動作によってバック グラウンドの状態との間のアプリを移動します。 iOS には、既知のバック グラウンドに必要なアプリケーションの一種として動作している重要なタスクを完了する時間の OS を依頼したり、バック グラウンドで実行するアプリを関連付けるためのいくつかのオプションも用意されていて、指定のアプリケーションのコンテンツを更新します。間隔。

このガイドとチュートリアルに付属するバック グラウンドでアプリケーションのタスクを実行する方法を説明ましょう。 主要な概念とベスト プラクティス、について説明いたします。 そのし、バック グラウンドで位置情報の更新を受信する現実の世界のアプリの作成を進めます。

## <a name="contents"></a>目次

1.  [iOS におけるバックグラウンド処理の概要](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [アプリケーション ライフサイクルのデモ](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS バックグラウンド処理のテクニック](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [チュートリアル: iOS でのバックグラウンド処理](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS バックグラウンド処理のガイダンス](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>まとめ

このガイドでは、iOS でのバック グラウンド処理を行うさまざまな方法を導入しました。 IOS アプリケーションの状態を説明し、ios アプリケーションのライフ サイクル アプローチをバック グラウンド処理、ロールを確認します。 さらに、どのように個々 のタスクまたは iOS のバック グラウンドで動作する全体のアプリケーション登録でしたわかっています。 最後に、バック グラウンドで更新プログラムを実行するアプリケーションを構築することにより、iOS でバック グラウンド処理の理解を補強します。



## <a name="related-links"></a>関連リンク

- [Android でバック グラウンド処理](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (サンプル)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [場所 (サンプル)](https://developer.xamarin.com/samples/monotouch/Location/)
- [単純なバック グラウンド転送 (サンプル)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS バック グラウンドで実行](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
