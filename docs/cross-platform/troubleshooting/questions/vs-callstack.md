---
title: Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: f07bc4437293bdc49e4811c0104fd7245ccf9738
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282955"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか

Visual Studio で GUI がロックアップ (ハング、フリーズ) すると、収集する重要な診断情報は、Visual Studio プロセスのすべてのスレッドからの呼び出し履歴のセットになります。 Visual Studio のハングしたインスタンスに対してこの情報を保存するには、Visual Studio の2番目のインスタンスを使用します。

1. Visual Studio の2番目のインスタンス (新しいウィンドウ) を開始します。

2. Visual Studio の新しいインスタンスで、開いているソリューションをすべて終了します。

3. **[デバッグ]、[プロセスにアタッチ]** の順に選択します。

   ![](vs-callstack-images/image1.png "デバッグ > [プロセスにアタッチ] を選択")

4. **使用可能なプロセス**の一覧`devenv.exe`から、元のハングしたインスタンスを選択します。

5. **[デバッグ > すべて中断]** を選択します。

   ![](vs-callstack-images/image2.png "[デバッグ > すべて中断] を選択します。")

6. **デバッグ > ダンプに名前を付けて保存** を選択します。

   ![](vs-callstack-images/image3.png "デバッグ > [ダンプに名前を付けて保存] を選択します。")

7. **ファイルの種類**を**ミニダンプ (\*.dmp)** に変更します。 これにより、**ヒープ付きミニダンプ**よりもはるかに小さなファイルが生成されます。また、ヒープは通常、フリーズの診断には関係ありません。

   ![](vs-callstack-images/image4.png "これにより、ヒープ付きミニダンプよりもはるかに小さなファイルが生成され、ヒープは通常、フリーズの診断には関係ありません。")

8. ダンプファイルを保存します。 ファイルをオンラインで送信する場合は、zip 形式でサイズを小さくすることができます。
