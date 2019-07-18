---
title: Xamarin.iOS で SiriKit
description: この記事では、Xamarin.iOS アプリで SiriKit を使用して iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する方法を示します。
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f7cbb3f7d9e448947ec8163a8660616910e750f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61423463"
---
# <a name="sirikit-in-xamarinios"></a>Xamarin.iOS で SiriKit

_この記事では、Xamarin.iOS アプリで SiriKit を使用して iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する方法を示します。_

新しい SiriKit により、アプリ拡張機能と、新しいを使用して iOS デバイスで Siri とマップ アプリを使用してユーザーがアクセスできるサービスを提供する iOS アプリを iOS 10、**インテント**と**Intents UI**フレームワーク。

Siri の動作の概念を**ドメイン**のグループが関連するタスクの操作を把握します。 Siri をアプリに含まれる各操作は必要がありますには、次のようにその既知のサービスのドメインのいずれかに分類されます。

- オーディオまたはビデオ通話。
- 素敵を予約します。
- トレーニングを管理します。
- メッセージング。
- 写真を検索します。
- 送信または支払いを受信します。

SiriKit 送信、拡張機能で、ユーザーが、アプリ拡張機能のサービスのいずれかに関連する siri 要求を行うと、**インテント**の関連データと共に、ユーザーの要求を記述するオブジェクト。 アプリ拡張機能は、適切な生成**応答**オブジェクトを指定された**インテント**、拡張機能が要求を処理する方法の詳細を示します。

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit の概念の理解](~/ios/platform/sirikit/understanding-sirikit.md)

この記事では、Xamarin.iOS アプリで SiriKit を操作するために必要な主要な概念について説明します。 カバー新しい Intents および Intents UI 拡張機能ポイントと Siri にアプリを開くには、アプリとユーザーのボキャブラリのしくみです。

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit の実装](~/ios/platform/sirikit/implementing-sirikit.md)

この記事では、Xamarin.iOS アプリで SiriKit のサポートを実装するために必要な手順について説明します。 開発者は、アプリに正しく実装するために必要なキーの概念を説明として SiriKit のサポートを追加する前に、SiriKit の概念について、上記のガイドをお読みください。





## <a name="related-links"></a>関連リンク

- [ElizaChat サンプル](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [インテントのフレームワーク参照](https://developer.apple.com/reference/intents)
- [Intents UI フレームワークの参照](https://developer.apple.com/reference/intentsui)
