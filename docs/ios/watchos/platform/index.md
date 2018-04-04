---
title: プラットフォーム機能
description: WatchOS アプリに含めるには、固有の機能を Apple Watch です。
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 9b90c799f2635221a2c19bda426c501737600f88
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="platform-features"></a>プラットフォーム機能

_WatchOS アプリに含めるには、固有の機能を Apple Watch です。_

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple 給与の機能強化](~/ios/watchos/platform/apple-pay.md)

WatchOS 3、PassKit フレームワークを Apple Watch で実行されているアプリの (物理的な商品およびサービスの両方) のセキュリティで保護された、アプリ内の支払いをサポートするために展開されています。

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[バックグラウンド タスク](~/ios/watchos/platform/background-tasks.md)

watchOS 3 には、これを開く前に、ユーザーが必要なコンテンツがあることを確認情報を更新するアプリが使用できるいくつかのバック グラウンド タスクが導入されています。

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 の概要](introduction-to-watchos4.md)

WatchOS 4 の新機能です。

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 の概要](introduction-to-watchos3/index.md)

ここでは、Xamarin の開発者向けのすべての watchOS 3 で使用できる新しいまたは変更された Api を紹介します。

##  <a name="notificationsnotificationsmd"></a>[通知](notifications.md)

カスタムの通知、watch アプリでの処理を提供する方法を説明します。

##  <a name="complicationscomplicationsmd"></a>[問題点](complications.md)

ウォッチの文字盤に最新のデータを表示するコンプリケーションのサポートを追加します。


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[プロアクティブな候補](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 内でのユーザーに情報を表示する事前にアプリを許可するコンテキストを指定します。 この機能をサポートするために、 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity)が含まれています、`MapItem`アプリに他のアプリで後で使用できる場所の情報を提供するプロパティです。

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[クイック操作のテクニック](~/ios/watchos/platform/quick-interaction-techniques.md)

提供する簡易ユーザー操作は、説得力のある Apple Watch アプリや複雑さの一部を作成するのに不可欠です。 新しい watchOS 3 に、Apple のジェスチャ レコグナイザー、デジタル クラウンおよび新しいユーザーに通知してナビゲーション手法へのアクセスのサポートが追加されました。 これには、SceneKit と SpriteKit の両方のサポートを追加および開発者ができるようにクイックと応答性の両方である、豊富なドリルインのインターフェイスを簡単に作成します。

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[トレーニング アプリの拡張機能](~/ios/watchos/platform/workout-apps.md)

新しい watchOS 3、トレーニング関連するアプリには、Apple Watch でバック グラウンドで実行する機能が設定されています。 この機能を有効にする (および HealthKit データにアクセスできる)、アプリを含める必要があります、`WKBackgroundModes`キー、 `Info.plist` 、値を持つファイル`workout-processing`です。
