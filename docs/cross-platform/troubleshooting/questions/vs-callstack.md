---
title: Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: aa8ce8791c598fa4891257b3d832478ecc5ee136
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938802"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio のプロセスの現在の呼び出し履歴はどのようにして収集しますか

Visual Studio で GUI がロックアップ (ハング、フリーズ) すると、収集する重要な診断情報は、Visual Studio プロセスのすべてのスレッドからの呼び出し履歴のセットになります。 Visual Studio のハングしたインスタンスに対してこの情報を保存するには、Visual Studio の2番目のインスタンスを使用します。

1. Visual Studio の2番目のインスタンス (新しいウィンドウ) を開始します。

2. Visual Studio の新しいインスタンスで、開いているソリューションをすべて終了します。

3. [**デバッグ > [プロセスにアタッチ**] を選択します。

   ![デバッグ > [プロセスにアタッチ] を選択](vs-callstack-images/image1.png)

4. `devenv.exe`**使用可能なプロセス**の一覧から、元のハングしたインスタンスを選択します。

5. [**デバッグ > すべて中断**] を選択します。

   ![[デバッグ > すべて中断] を選択します。](vs-callstack-images/image2.png)

6. [**デバッグ > [ダンプに名前を付けて保存**] を選択します。

   ![デバッグ > [ダンプに名前を付けて保存] を選択します。](vs-callstack-images/image3.png)

7. **ファイルの種類**を**ミニダンプ ( \* .dmp)** に変更します。 これにより、**ヒープ付きミニダンプ**よりもはるかに小さなファイルが生成されます。また、ヒープは通常、フリーズの診断には関係ありません。

   ![これにより、ヒープ付きミニダンプよりもはるかに小さなファイルが生成され、ヒープは通常、フリーズの診断には関係ありません。](vs-callstack-images/image4.png)

8. ダンプファイルを保存します。 ファイルをオンラインで送信する場合は、zip 形式でサイズを小さくすることができます。
