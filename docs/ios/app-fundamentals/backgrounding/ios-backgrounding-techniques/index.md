---
title: iOS バックグラウンド処理のテクニック
description: このドキュメントは、iOS でのさまざまな backgrounding 手法について説明するガイドにリンクします。 バック グラウンド タスク、バック グラウンド転送サービス、バック グラウンドの取得、およびリモート通知します。
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 00cca0c75cc79858f6edda5d6fb954611d81161b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169608"
---
# <a name="ios-backgrounding-techniques"></a>iOS バックグラウンド処理のテクニック

次のセクションでは、既存の backgrounding オプションと共に、次の iOS 機能について説明します。

-  [バック グラウンド タスクの便宜的](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7)-デバイスが他の処理の起動状態のときに、便宜的のチャンク単位でバック グラウンド タスクを実行してバッテリ寿命を保ちます。
-  [バック グラウンド転送サービス](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers)- 確実にアップロードし、ネットワーク状態やファイルのサイズに関係なくファイルをダウンロードします。
-  [バック グラウンドのフェッチ](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch)-システムで決定された間隔でバック グラウンドからのアプリケーションを更新します。
-  [リモート通知](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications)-ユーザーがユーザーに通知する、またはサイレント モードで更新するオプションで、アプリケーションを開く前に、バック グラウンドでコンテンツの更新をトリガーするプッシュ通知を使用します。
-  **UI の更新をバック グラウンド**- ユーザーのアプリケーションの UI を準備し、バック グラウンドからすべてのアプリケーションのスナップショットを更新します。
