---
title: デバッガーに必要なプロジェクト設定を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 856c04d129058e8cbac30dcdf619e8b2b5a66cb6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014257"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>デバッガーに必要なプロジェクト設定を教えてください

デバッガーが想定どおりに動作するようにするには (ブレークポイントにヒットし、デバッグログを表示するなど)、開発者のインストルメンテーションとデバッグ情報の表示を両方とも有効にする必要があります。

環境設定を確認するには、次の手順に従ってください。

## <a name="visual-studio"></a>Visual Studio

1. プロジェクトオプションを開く
2. **ビルド > 詳細**に進む...デバッグ情報を**完全**に設定する
3. 各プラットフォームの設定:
   - **Android オプション > デバッグオプション**にアクセスします。 **[開発者インストルメンテーションを有効にする]** チェックボックスをオンにします。
   - **IOS デバッグ > デバッグ & インストルメンテーション**にアクセスします。 **[デバッグを有効にする]** チェックボックスをオンにします。

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. プロジェクトオプションを開く
2. **ビルド > コンパイラ > 全般オプションに関する**ページを参照してください。 デバッグ情報を**完全**に設定する
3. 各プラットフォームの設定:
    - [**ビルド > Android ビルド > デバッグオプション]** にアクセスします。 **[開発者インストルメンテーションを有効にする]** チェックボックスをオンにします。
    - **[ビルド > IOS デバッグ]** にアクセスします。 **[デバッグを有効にする]** チェックボックスをオンにします。
