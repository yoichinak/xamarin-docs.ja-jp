---
title: デバッガーに必要なプロジェクト設定を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 343f8d37d77726d2cdc06a74c44e476af00dde27
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765159"
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
