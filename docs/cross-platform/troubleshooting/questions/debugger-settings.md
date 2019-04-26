---
title: デバッガーに必要なプロジェクト設定を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 9a18c97ba227615ae42529424b5c22b5e144f5e5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357863"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>デバッガーに必要なプロジェクト設定を教えてください

デバッガー (ブレークポイントのヒット、デバッグ ログの表示など) が期待どおりに動作するためには、開発者のインストルメンテーションとデバッグ情報の表示する必要がありますどちらも有効にします。

環境設定を確認する次の手順に従ってください。

## <a name="visual-studio"></a>Visual Studio
1. プロジェクトのオプションを開く
2. 移動して**ビルド > 高度な.** デバッグ情報を設定**完全**
3. 各プラットフォームの設定:
   - 移動して**Android オプション > デバッグ オプション**します。 ティック、**開発者のインストルメンテーションを有効にする**ボックス。
   - 移動して**iOS デバッグ > デバッグとインストルメンテーション**します。 ティック、**デバッグを有効にする**ボックス。

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. プロジェクトのオプションを開く
2. 移動して**ビルド > コンパイラ > 全般オプション**します。 デバッグ情報を設定**完全**
3. 各プラットフォームの設定:
  - 移動して**ビルド > Android のビルド > デバッグ オプション**します。 ティック、**開発者のインストルメンテーションを有効にする**ボックス。
  - 移動して**ビルド > iOS デバッグ**します。 ティック、**デバッグを有効にする**ボックス。

