---
title: Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 9ed79b2273758b8051a96169d4c9b53870de1fb1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013027"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか

Visual Studio で GUI がロックアップ (ハング、フリーズ) すると、収集する重要な診断情報は、Visual Studio プロセスのすべてのスレッドからの呼び出し履歴のセットになります。 Visual Studio のハングしたインスタンスに対してこの情報を保存するには、Visual Studio の2番目のインスタンスを使用します。

1. Visual Studio の2番目のインスタンス (新しいウィンドウ) を開始します。

2. Visual Studio の新しいインスタンスで、開いているソリューションをすべて終了します。

3. **[デバッグ]、[プロセスにアタッチ]** の順に選択します。

   ![](vs-callstack-images/image1.png "Select Debug > Attach to Process")

4. **使用可能なプロセス**の一覧から、`devenv.exe` の元のハングしたインスタンスを選択します。

5. **[デバッグ > すべて中断]** を選択します。

   ![](vs-callstack-images/image2.png "Select Debug > Break All")

6. **デバッグ > ダンプに名前を付けて保存** を選択します。

   ![](vs-callstack-images/image3.png "Select Debug > Save Dump As")

7. **ファイルの種類**を**ミニダンプ (\*.dmp)** に変更します。 これにより、**ヒープ付きミニダンプ**よりもはるかに小さなファイルが生成されます。また、ヒープは通常、フリーズの診断には関係ありません。

   ![](vs-callstack-images/image4.png "This will produce a much smaller file than Minidump with Heap, and the heap is usually not relevant for diagnosing freezes")

8. ダンプファイルを保存します。 ファイルをオンラインで送信する場合は、zip 形式でサイズを小さくすることができます。
