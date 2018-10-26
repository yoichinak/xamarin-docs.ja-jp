---
title: iOS バック グラウンド処理手法
description: このドキュメントは、iOS でのさまざまな backgrounding 手法について説明するガイドにリンクします。 バック グラウンド タスク、バック グラウンド転送サービス、バック グラウンドの取得、およびリモート通知します。
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 00cca0c75cc79858f6edda5d6fb954611d81161b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119748"
---
# <a name="ios-backgrounding-techniques"></a>iOS バック グラウンド処理手法

次のセクションでは、既存の backgrounding オプションと共に、次の iOS 機能について説明します。

-  [バック グラウンド タスクの便宜的](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7)-デバイスが他の処理の起動状態のときに、便宜的のチャンク単位でバック グラウンド タスクを実行してバッテリ寿命を保ちます。
-  [バック グラウンド転送サービス](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers)- 確実にアップロードし、ネットワーク状態やファイルのサイズに関係なくファイルをダウンロードします。
-  [バック グラウンドのフェッチ](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch)-システムで決定された間隔でバック グラウンドからのアプリケーションを更新します。
-  [リモート通知](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications)-ユーザーがユーザーに通知する、またはサイレント モードで更新するオプションで、アプリケーションを開く前に、バック グラウンドでコンテンツの更新をトリガーするプッシュ通知を使用します。
-  **UI の更新をバック グラウンド**- ユーザーのアプリケーションの UI を準備し、バック グラウンドからすべてのアプリケーションのスナップショットを更新します。
