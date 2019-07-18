---
title: watchOS プラットフォーム機能
description: このドキュメントは、Apple Pay、通知など、複雑な問題、プロアクティブな候補、トレーニングのアプリの詳細は、watchOS プラットフォーム機能を記述するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 09200ba5968838edf829b30a50a8ad0f4a3ab3aa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224561"
---
# <a name="watchos-platform-features"></a>watchOS プラットフォーム機能

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[watchOS 5 の概要](introduction-to-watchos5/index.md)

このドキュメントでは、Xamarin で watchOS アプリを構築するときに使用するために使用できる 5 watchOS で新規および更新された機能の概要を提供します。

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 の概要](introduction-to-watchos4.md)

このドキュメントでは、機能が追加され、watchOS 4 の更新の概要を提供します。

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 の概要](introduction-to-watchos3/index.md)

この記事では、watchOS 3 で新規および更新された Api について説明します。

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay 機能強化](~/ios/watchos/platform/apple-pay.md)

WatchOS 3、PassKit framework は、Apple Watch で実行されるアプリ (物理的な商品およびサービスの両方) のセキュリティで保護された、アプリ内の支払いのサポートを許可する拡張されました。

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)

watchOS 3 では、コンテンツがあることを確認状を開く前に、ユーザーが必要な情報を更新するアプリが使用できるいくつかのバック グラウンド タスクについて説明します。

## <a name="notificationsnotificationsmd"></a>[通知](notifications.md)

カスタム通知の watch アプリで処理を提供する方法について説明します。

## <a name="complicationscomplicationsmd"></a>[問題点](complications.md)

ウォッチの文字盤で最新のデータを表示するコンプリケーションのサポートを追加します。

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[プロアクティブな候補](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 内のユーザーに積極的に情報を提示するアプリを許可するコンテキストを指定します。 この機能をサポートするために、 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity)が含まれています、`MapItem`アプリに他のアプリで後で使用できる場所の情報を提供するプロパティ。

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[クイック操作の手法](~/ios/watchos/platform/quick-interaction-techniques.md)

提供の簡単なユーザー操作は、説得力のある Apple Watch アプリや複雑さを作成するために不可欠です。 新しい watchos 3、Apple がジェスチャ レコグナイザー、デジタル クラウンと新しいユーザー通知とナビゲーション手法へのアクセスのサポートを追加します。 これ、と共に SceneKit とある SpriteKit の両方のサポートを追加できるように、開発者は高速かつ応答性の高い豊富な glanceable のインターフェイスを簡単に作成します。

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[トレーニング アプリの拡張機能](~/ios/watchos/platform/workout-apps.md)

新しい watchOS 3、トレーニングに関連するアプリを Apple Watch でバック グラウンドで実行する機能があります。 この機能を有効にする (および、HealthKit のデータにアクセスできる) に、アプリを含める必要があります、`WKBackgroundModes`キー、`Info.plist`値を持つファイル`workout-processing`します。
