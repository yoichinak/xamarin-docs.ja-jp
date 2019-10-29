---
title: watchOS プラットフォームの機能
description: このドキュメントでは、Apple Pay、通知、複雑さ、プロアクティブな提案、トレーニングアプリなど、watchOS プラットフォームの機能について説明するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: e571132b5f1e30bececb8302f2dacfcd908ad42e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028290"
---
# <a name="watchos-platform-features"></a>watchOS プラットフォームの機能

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[watchOS 5 の概要](introduction-to-watchos5/index.md)

このドキュメントでは、Xamarin を使用して watchOS アプリをビルドするときに使用できる watchOS 5 の新機能と更新された機能の概要を示します。

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 の概要](introduction-to-watchos4.md)

このドキュメントでは、watchOS 4 で追加および更新された機能の概要について説明します。

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 の概要](introduction-to-watchos3/index.md)

この記事では、watchOS 3 の新しい Api と更新された Api について説明します。

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay の機能強化](~/ios/watchos/platform/apple-pay.md)

WatchOS 3 では、Pass Kit フレームワークが拡張され、Apple Watch で実行されているアプリのセキュリティで保護されたアプリ内支払い (物理的な商品とサービスの両方) をサポートできるようになりました。

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[バックグラウンドタスク](~/ios/watchos/platform/background-tasks.md)

watchOS 3 では、アプリで情報を更新するために使用できるバックグラウンドタスクがいくつか導入されています。このタスクを開く前に、ユーザーが必要とするコンテンツがあることを確認します。

## <a name="notificationsnotificationsmd"></a>[通知](notifications.md)

Watch アプリでカスタム通知処理を提供する方法について説明します。

## <a name="complicationscomplicationsmd"></a>[問題点](complications.md)

より複雑なサポートを追加して、ウォッチ式に最新のデータを表示します。

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[プロアクティブな提案](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 を使用すると、アプリは特定のコンテキスト内でユーザーに情報を事前に提示できます。 この機能をサポートするために、 [Nsuseractivity](https://developer.apple.com/reference/foundation/nsuseractivity)に `MapItem` プロパティが含まれるようになりました。これにより、アプリは、他のアプリで後で使用するための場所情報を提供できるようになります。

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[クイック操作の手法](~/ios/watchos/platform/quick-interaction-techniques.md)

ユーザーとの対話を簡単に行うには、説得力のある Apple Watch アプリや複雑さを作成することが不可欠です。 WatchOS 3 の新機能である Apple では、ジェスチャレコグナイザー、Digital Crown へのアクセス、および新しいユーザー通知およびナビゲーション手法のサポートが追加されました。 これに加えて、SceneKit と SpriteKit の両方のサポートが追加されたため、開発者は迅速かつ迅速に対応できる豊富なインターフェイスを簡単に作成できます。

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[トレーニングアプリの機能強化](~/ios/watchos/platform/workout-apps.md)

WatchOS 3 の新機能であるトレーニング関連のアプリは、Apple Watch のバックグラウンドで実行することができます。 この機能を有効にする (および HealthKit データへのアクセス権を取得する) には、アプリで `Info.plist` ファイルに `workout-processing`という値の `WKBackgroundModes` キーを含める必要があります。
