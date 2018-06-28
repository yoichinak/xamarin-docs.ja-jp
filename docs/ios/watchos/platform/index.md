---
title: watchOS プラットフォーム機能
description: このドキュメントには、Apple Pay、通知など、複雑な問題、プロアクティブなご提案、トレーニング アプリでは、複数の watchOS プラットフォームの機能を記述するさまざまなガイドへのリンクがします。
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 16d10dd69223f404aac7c933302992a1544461e9
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066615"
---
# <a name="watchos-platform-features"></a>watchOS プラットフォーム機能

このドキュメントには、Apple Pay、通知など、複雑な問題、プロアクティブなご提案、トレーニング アプリでは、複数の watchOS プラットフォームの機能を記述するさまざまなガイドへのリンクがします。

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 の概要](introduction-to-watchos4.md)

このドキュメントでは、watchOS 4 で追加され更新機能の大まかな概要を説明します。

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 の概要](introduction-to-watchos3/index.md)

この記事では、新規および更新された watchOS 3 Api について説明します。

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay の機能強化](~/ios/watchos/platform/apple-pay.md)

WatchOS 3、PassKit フレームワークを Apple Watch で実行されているアプリの (物理的な商品およびサービスの両方) のセキュリティで保護された、アプリ内の支払いをサポートするために展開されています。

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[バック グラウンド タスク](~/ios/watchos/platform/background-tasks.md)

watchOS 3 には、これを開く前に、ユーザーが必要なコンテンツがあることを確認情報を更新するアプリが使用できるいくつかのバック グラウンド タスクが導入されています。

## <a name="notificationsnotificationsmd"></a>[通知](notifications.md)

カスタムの通知、watch アプリでの処理を提供する方法を説明します。

## <a name="complicationscomplicationsmd"></a>[問題点](complications.md)

ウォッチの文字盤に最新のデータを表示するコンプリケーションのサポートを追加します。

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[プロアクティブなご提案](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 内でのユーザーに情報を表示する事前にアプリを許可するコンテキストを指定します。 この機能をサポートするために、 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity)が含まれています、`MapItem`アプリに他のアプリで後で使用できる場所の情報を提供するプロパティです。

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[クイック操作方法](~/ios/watchos/platform/quick-interaction-techniques.md)

提供する簡易ユーザー操作は、説得力のある Apple Watch アプリや複雑さの一部を作成するのに不可欠です。 新しい watchOS 3 に、Apple のジェスチャ レコグナイザー、デジタル クラウンおよび新しいユーザーに通知してナビゲーション手法へのアクセスのサポートが追加されました。 これには、SceneKit と SpriteKit の両方のサポートを追加および開発者ができるようにクイックと応答性の両方である、豊富なドリルインのインターフェイスを簡単に作成します。

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[トレーニングのアプリの機能強化](~/ios/watchos/platform/workout-apps.md)

新しい watchOS 3、トレーニング関連するアプリには、Apple Watch でバック グラウンドで実行する機能が設定されています。 この機能を有効にする (および HealthKit データにアクセスできる)、アプリを含める必要があります、`WKBackgroundModes`キー、 `Info.plist` 、値を持つファイル`workout-processing`です。
