---
title: Visual Studio のプロセスの現在の呼び出し履歴を収集する方法は?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: 45ad6cc93d14fc1da077a40abea2c25668fb2269
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio のプロセスの現在の呼び出し履歴を収集する方法は?

GUI が Visual Studio のロック (ハング、フリーズ) したときに収集する診断情報は重要、Visual Studio プロセスのすべてのスレッドから呼び出しスタックのセットです。 Visual Studio のハングしたインスタンスのこの情報を保存するには、Visual Studio の 2 番目のインスタンスを使用できます。

1. Visual Studio の 2 番目のインスタンス (新しいウィンドウ) を開始します。

2. Visual Studio の新しいインスタンスで開かれているソリューションを閉じます。

3. **[デバッグ]、[プロセスにアタッチ]** の順に選択します。

  ![](vs-callstack-images/image1.png "デバッグを選択 > プロセスにアタッチします。")

4. 元のハングしたインスタンスを選択して`devenv.exe`の一覧から**選択可能なプロセス**です。

5. 選択**デバッグ > [すべて中断]**です。

  ![](vs-callstack-images/image2.png "デバッグを選択 > [すべて中断]")

6. 選択**デバッグ > としてダンプを保存**です。

  ![](vs-callstack-images/image3.png "デバッグを選択 > としてダンプを保存")

7. 変更**付けて**に**ミニダンプ (\*.dmp)**です。 これよりも多くより小さなファイルが作成されます**ヒープ付きミニダンプ**ヒープは通常ありませんフリーズを診断するために関連するとします。

  ![](vs-callstack-images/image4.png "ヒープ付きミニダンプよりも多くより小さなファイルが生成され、ヒープは通常ありませんフリーズを診断するために関連します。")

8. ダンプ ファイルを保存します。 オンライン ファイルを送信している場合を zip 圧縮サイズを小さくことができます。
