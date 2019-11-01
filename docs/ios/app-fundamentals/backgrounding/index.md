---
title: Xamarin. iOS のバックグラウンド処理
description: バックグラウンド処理またはバックグラウンド処理は、別のアプリケーションがフォアグラウンドで実行されている間に、アプリケーションがバックグラウンドでタスクを実行できるようにするプロセスです。 このガイドは、iOS でのバックグラウンド処理の概要として機能します。
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: e51b6c7ffe0b0ae027e527d959979e558afa6325
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73005068"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin. iOS のバックグラウンド処理

_バックグラウンド処理またはバックグラウンド処理は、別のアプリケーションがフォアグラウンドで実行されている間に、アプリケーションがバックグラウンドでタスクを実行できるようにするプロセスです。このガイドは、iOS でのバックグラウンド処理の概要として機能します。_

モバイルアプリケーションのバックグラウンド処理は、デスクトップ上のマルチタスキングの従来の概念とは基本的に異なります。 デスクトップコンピューターには、画面の不動産、電力、メモリなど、アプリケーションで使用できるさまざまなリソースが用意されています。 アプリケーションはサイドバイサイドで実行でき、パフォーマンスと使用可能な状態を維持できます。 モバイルデバイスでは、リソースの制限がはるかに高くなります。 1つの小さな画面に複数のアプリケーションを表示することは困難であり、複数のアプリケーションを完全に実行するとバッテリが消耗します。 バックグラウンド処理は、パフォーマンスを向上させるために必要なバックグラウンドタスクを実行するためにアプリケーションにリソースを提供し、事前に接地されたアプリケーションとデバイスの応答性を維持するために、一定のセキュリティ侵害を受けます。 IOS と Android はどちらもバックグラウンド処理に対してプロビジョニングされていますが、まったく異なる方法で処理されます。

IOS では、バックグラウンド処理はアプリケーションの状態として認識され、アプリとユーザーの動作に応じて、アプリはバックグラウンド状態との間で移動されます。 また、iOS では、アプリをバックグラウンドで実行するためのいくつかのオプションが用意されています。たとえば、重要なタスクを完了するための時間を OS に要求し、既知のバックグラウンドで必要なアプリケーションの種類として操作し、指定された場所でアプリケーションのコンテンツを更新します。不定期.

このガイドと付随するチュートリアルでは、アプリケーションタスクをバックグラウンドで実行する方法について学習します。 ここでは、主要な概念とベストプラクティスについて説明した後、場所の更新をバックグラウンドで受け取る実際のアプリを作成します。

## <a name="contents"></a>コンテンツ

1. [iOS におけるバックグラウンド処理の概要](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1. [アプリケーション ライフサイクルのデモ](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1. [iOS バックグラウンド処理のテクニック](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1. [チュートリアル: iOS でのバックグラウンド処理](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1. [iOS バックグラウンド処理のガイダンス](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>まとめ

このガイドでは、iOS でバックグラウンド処理を行うさまざまな方法を紹介しました。 Ios アプリケーションの状態について説明し、iOS アプリケーションのライフサイクルでバックグラウンド処理が果たす役割を確認します。 また、iOS のバックグラウンドで動作するように個々のタスクまたはアプリケーション全体を登録する方法についても説明しました。 最後に、バックグラウンドで更新を実行するアプリケーションを構築することにより、iOS でのバックグラウンド処理に関する理解を深めます。

## <a name="related-links"></a>関連リンク

- [Android でのバックグラウンド処理](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
- [場所 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/location)
- [単純なバックグラウンド転送 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
- [iOS のバックグラウンド実行](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
