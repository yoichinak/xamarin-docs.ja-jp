---
title: Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: e81c28f0610a0df2e4fe06349685ef5e0744071a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61159163"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか

GUI が Visual Studio のロック (ハングがフリーズ) したときに収集する診断情報の重要な要素は、Visual Studio プロセスのすべてのスレッドからの呼び出し履歴のセットです。 Visual Studio のハングしたインスタンスのこの情報を保存するには、Visual Studio の 2 番目のインスタンスを使用できます。

1. Visual Studio の 2 番目のインスタンス (新しいウィンドウ) を起動します。

2. Visual Studio の新しいインスタンスで開かれているソリューションを閉じます。

3. **[デバッグ]、[プロセスにアタッチ]** の順に選択します。

  ![](vs-callstack-images/image1.png "デバッグを選択 > プロセスにアタッチします。")

4. ハングした元のインスタンスを選択します。`devenv.exe`の一覧から**選択可能なプロセス**します。

5. 選択**デバッグ > すべて中断**します。

  ![](vs-callstack-images/image2.png "デバッグを選択 > すべて中断")

6. 選択**デバッグ > としてダンプを保存**します。

  ![](vs-callstack-images/image3.png "デバッグを選択 > としてダンプを保存")

7. 変更**型として保存**に**ミニダンプ (\*.dmp)** します。 これよりもはるかに小さいファイルが生成されます**ヒープ付きミニダンプ**ヒープが、通常はフリーズの診断に関連するとします。

  ![](vs-callstack-images/image4.png "ヒープ付きミニダンプよりもはるかにサイズの小さいファイルが生成され、ヒープがフリーズの診断に関連する通常")

8. ダンプ ファイルを保存します。 オンライン ファイルを送信している場合を zip 圧縮サイズを小さくことができます。
