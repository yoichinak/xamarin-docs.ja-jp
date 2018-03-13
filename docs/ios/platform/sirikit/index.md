---
title: SiriKit
description: "この記事では、Xamarin.iOS アプリで SiriKit を使用して iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 9d7773be1244b0ba4e1a57c8a1efbddf02396138
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="sirikit"></a>SiriKit

_この記事では、Xamarin.iOS アプリで SiriKit を使用して iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する方法を示します。_

新しい SiriKit によりアプリ拡張機能と、新しいを使用して iOS デバイスで Siri とマップのアプリを使用してユーザーがアクセスできるサービスを提供する iOS アプリ、10、iOS に**インテント**と**インテント UI**フレームワークです。

Siri の動作の概念を**ドメイン**のグループが関連するタスクの操作を把握します。 Siri とされているアプリの各相互作用がとおりの既知のサービスのドメインのいずれかに分類する必要があります。

- オーディオまたはビデオ通話です。
- 素敵を予約します。
- ワークアウトを管理します。
- メッセージングです。
- 写真を検索しています。
- 送信または受信支払いを処理します。

ユーザーは、1 つのアプリ拡張機能のサービスに関連する Siri の要求を行う、SiriKit 送信、拡張機能、**インテント**サポート データと共に、ユーザーの要求を記述するオブジェクト。 アプリ拡張機能は、適切な生成**応答**オブジェクトを指定された**インテント**、拡張機能が、要求を処理する方法の詳細を示すです。

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit の概念の理解](~/ios/platform/sirikit/understanding-sirikit.md)

この記事では、Xamarin.iOS アプリで SiriKit を操作するために必要となる重要な概念について説明します。 新しい方法を説明して、意図的およびインテント UI 拡張機能ポイントしくみと、アプリ ユーザー ボキャブラリ Siri のアプリを開きます。

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit の実装](~/ios/platform/sirikit/implementing-sirikit.md)

この記事では、Xamarin.iOS アプリで SiriKit サポートを実装するための手順について説明します。 開発者は、実装を成功させるために必要となるキーについては、概念として、アプリに SiriKit サポートを追加する前に、上記 SiriKit 概念についてのガイドをお読みください。





## <a name="related-links"></a>関連リンク

- [ElizaChat サンプル](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [インテント Framework リファレンス](https://developer.apple.com/reference/intents)
- [目的の UI フレームワークのリファレンス](https://developer.apple.com/reference/intentsui)
