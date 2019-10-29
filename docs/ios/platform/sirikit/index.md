---
title: Xamarin. iOS の SiriKit
description: この記事では、Xamarin iOS アプリで SiriKit を使用して、iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する方法について説明します。
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 90d3a3b6a1e3a3c20ba8cf2ed1f12f234e3af33b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031481"
---
# <a name="sirikit-in-xamarinios"></a>Xamarin. iOS の SiriKit

_この記事では、Xamarin iOS アプリで SiriKit を使用して、iOS デバイスで Siri を使用してユーザーがアクセスできるサービスを提供する方法について説明します。_

IOS 10 の新機能である SiriKit を使用すると、ios アプリは、アプリ拡張機能と新しい**インテント**および**インテント UI**フレームワークを使用して、Ios デバイスで siri と Maps アプリを使用して、ユーザーがアクセスできるサービスを提供できます。

Siri は、**ドメイン**の概念、関連するタスクの既知のアクションのグループと連携して機能します。 Siri を使用したアプリの相互作用は、次のいずれかの既知のサービスドメインに分類される必要があります。

- オーディオまたはビデオの呼び出し。
- 乗り物を予約します。
- ワークスペースの管理。
- メッセージング.
- 写真を検索しています。
- 支払いの送信または受信。

ユーザーがアプリ拡張機能のいずれかのサービスに関連する Siri の要求を行うと、SiriKit は、ユーザーの要求とサポートデータを記述する**インテント**オブジェクトを拡張機能に送信します。 その後、アプリ拡張機能は、指定された**インテント**に対して適切な**応答**オブジェクトを生成し、拡張機能が要求を処理する方法を詳述します。

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit の概念の理解](~/ios/platform/sirikit/understanding-sirikit.md)

この記事では、Xamarin iOS アプリで SiriKit を操作するために必要な主要な概念について説明します。 この記事では、新しいインテントとインテントの UI 拡張ポイントと、アプリとユーザーのボキャブラリを使用して Siri にアプリを開く方法について説明します。

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit の実装](~/ios/platform/sirikit/implementing-sirikit.md)

この記事では、Xamarin iOS アプリで SiriKit サポートを実装するために必要な手順について説明します。 開発者は、SiriKit のサポートをアプリに追加する前に、前述の「SiriKit の概念を理解する」ガイドを読む必要があります。これは、実装を成功させるために必要な重要な概念について説明しています。

## <a name="related-links"></a>関連リンク

- [サンプルの登録 zach](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit プログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [インテントフレームワークリファレンス](https://developer.apple.com/reference/intents)
- [インテント UI フレームワークリファレンス](https://developer.apple.com/reference/intentsui)
