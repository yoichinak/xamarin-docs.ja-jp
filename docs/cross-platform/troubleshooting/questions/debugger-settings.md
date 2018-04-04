---
title: どのようなプロジェクト設定は、デバッガーに必要なしますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 7dff69e90b5105401da162c52053d884ed9b038b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>どのようなプロジェクト設定は、デバッガーに必要なしますか。

デバッガー (ヒット ブレークポイント、デバッグ ログの表示など) を正常に動作するためには、開発者のインストルメンテーションとデバッグ情報の表示する必要がありますどちらも有効にします。

環境設定を確認して次の手順に従ってください。

## <a name="visual-studio"></a>Visual Studio
1. プロジェクトのオプションを開く
2. 移動して**ビルド > 高度なしています.**デバッグ情報を設定**完全**
3. 各プラットフォームの設定:
   - 移動して**Android オプション > デバッグ オプション**です。 ティック、**開発者インストルメンテーションを有効にする**ボックス。
   - 移動して**iOS ビルド > デバッグ オプション**です。 ティック、**デバッグを有効にする**ボックス。

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. プロジェクトのオプションを開く
2. 移動して**ビルド > コンパイラ > 全般オプション**です。 デバッグ情報を設定**完全**
3. 各プラットフォームの設定:
  - 移動して**ビルド > Android ビルド > デバッグ オプション**です。 ティック、**開発者インストルメンテーションを有効にする**ボックス。
  - 移動して**ビルド > iOS デバッグ**です。 ティック、**デバッグを有効にする**ボックス。

