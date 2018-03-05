---
title: "Android の消耗エミュレーターでのデバッグします。"
description: "これらの記事では、エミュレーターで Xamarin.Android 消耗アプリケーションをデバッグする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1ad3c193261bf22b7ee344aa1ccabb226533b907
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="debug-android-wear-on-an-emulator"></a>Android の消耗エミュレーターでのデバッグします。

_これらの記事では、エミュレーターで Xamarin.Android 消耗アプリケーションをデバッグする方法について説明します。_

## <a name="debug-wear-on-emulator-overview"></a>エミュレーターの概要の劣化をデバッグします。

Android 着用アプリケーションを開発するには、物理ハードウェア上でいずれかと、アプリケーションを実行するか、エミュレーターまたはシミュレーターを使用する必要があります。 ハードウェアを使用するのが最適な方法ですが、常に実用的というわけではありません。 多くの場合、簡素化され、コスト効果をシミュレート エミュレート以下に示すように、エミュレーターを使用して Android 着用ハードウェアがあります。 まだ配置および実行の手順に慣れる着用して Android アプリを参照してください場合[ハロー, 着用](~/android/wear/get-started/hello-wear.md)です。

## <a name="configure-the-android-sdk-emulator"></a>Android SDK エミュレーターを構成します。

消耗アプリをエミュレーターで実行するには、Android SDK Android エミュレーターをインストールし、Android 着用用に構成します。 全体的な Android SDK エミュレーターのインストールと構成については、次を参照してください。 [Android SDK エミュレーター](~/android/deploy-test/debugging/android-sdk-emulator/index.md)です。

消耗仮想デバイスを作成するときに、Android を着用デバイス プロファイルを選択 (など**Android 消耗角**)。 パフォーマンス向上のための使用、消耗**x86** CPU/ABI この例のように。

[![消耗仮想デバイス構成の例](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png)


## <a name="launch-the-wear-virtual-device"></a>消耗仮想デバイスを起動します。 

着用して Android 仮想デバイスを作成した後に選択できますが、IDE でデバイスのプルダウン メニューからデバッグを開始する前に。 仮想デバイスがデバイスのドロップダウン リストで使用できない場合は、プロジェクトが Android であることを確認*着用*仮想デバイスとアプリのプロジェクト (、Android アプリ プロジェクトではなく) と同じ API に、ターゲット API レベルが設定されているレベルします。 例:

[ ![Visual Studio デバイス メニューに着け AVD を選択します。](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png)

Android エミュレーターが起動したら、Xamarin.Android は、エミュレーターを消耗アプリを展開します。 エミュレーターは、構成済みの仮想デバイス イメージを使用してアプリを実行します。

これを表示する場合も驚かないでください (または別のスポット画面) 最初にします。 ウォッチ エミュレーターを起動するには時間かかることができます。 

![エミュレーターは 1 分だけを表示を確認してください.](debug-on-emulator-images/please-wait.png)

エミュレーターは実行したままでかまいません。アプリが実行されるたびにシャットダウンして再起動する必要はありません。

 
## <a name="summary"></a>まとめ
 
このガイドでは、消耗開発用の Android SDK エミュレーターを構成して、デバッグ用の仮想デバイスの損傷を起動する方法について説明します。
