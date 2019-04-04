---
title: エミュレーターで Android Wear をデバッグします。
description: これらの記事では、エミュレーターで Xamarin.Android Wear アプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 699fb3cc3a5730e8ab2c677feb7cdfbdcf106aeb
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120555"
---
# <a name="debug-android-wear-on-an-emulator"></a>エミュレーターで Android Wear をデバッグします。

_これらの記事では、エミュレーターで Xamarin.Android Wear アプリケーションをデバッグする方法について説明します。_

## <a name="debug-wear-on-emulator-overview"></a>Wear Emulator の概要でのデバッグします。

Android Wear のアプリケーションの開発には、物理ハードウェア上でアプリケーションを実行して、または、エミュレーターまたはシミュレーターを使用する必要があります。 ハードウェアを使用するのが最適な方法ですが、常に実用的というわけではありません。 多くの場合でより簡単かつコスト効率、以下に示すようにエミュレーターを使用して Android Wear のハードウェアをシミュレートまたはエミュレートがあります。 ない場合の展開および実行するプロセスに詳しく Android Wear アプリを参照してください[こんにちは、Wear](~/android/wear/get-started/hello-wear.md)します。

## <a name="configure-the-android-emulator"></a>Android エミュレーターを構成します。

Wear アプリをエミュレーターで実行するには、は、Android SDK Android エミュレーターをインストールし、Android Wear 用に構成する必要があります。 全体的な Android SDK エミュレーターのインストールと構成については、[Android Emulator のセットアップ](~/android/get-started/installation/android-emulator/index.md)を参照してください。

Wear 仮想デバイスを作成するときに、Android Wear デバイス プロファイルを選択 (など**Android Wear の正方形**)。 パフォーマンスを向上させるには、使用、Wear **x86** CPU/ABI この例のように。

[![Wear 仮想デバイスの構成の例](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Wear 仮想デバイスを起動します。 

Android Wear の仮想デバイスを作成した後に選択できますが、IDE で、デバイス プルダウン メニューからデバッグを開始する前に。 仮想デバイスがデバイスのプルダウン メニューで利用できない場合は、プロジェクトが Android であることを確認*Wear*仮想デバイスとレベルのアプリのプロジェクト (Android アプリ プロジェクトされません) と同じ API にそのターゲット API レベルが設定されています。 例えば:

[![Visual Studio メニューでデバイスを着用 AVD を選択します。](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android エミュレーターが開始されると、Xamarin.Android はエミュレーターに Wear アプリを展開します。 エミュレーターは、構成済みの仮想デバイス イメージを使用してアプリを実行します。

これを表示する場合も驚かないでください (または別のスポット画面) 最初。 ウォッチ エミュレーターが起動にかかる可能性があります。 

![エミュレーターは、わずか 1 分を表示を見る.](debug-on-emulator-images/please-wait.png)

エミュレーターは実行したままでかまいません。アプリが実行されるたびにシャットダウンして再起動する必要はありません。

 
## <a name="summary"></a>まとめ
 
このガイドでは、Wear 開発用の Android エミュレーターを構成して、デバッグ用 Wear 仮想デバイスを起動する方法について説明します。
