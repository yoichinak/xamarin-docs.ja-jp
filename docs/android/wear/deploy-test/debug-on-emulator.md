---
title: エミュレーターで Android の磨耗をデバッグする
description: これらの記事では、エミュレーターで Xamarin. Android の磨耗アプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: ca0a6884c05686bded25a2e515456ab192002a24
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028680"
---
# <a name="debug-android-wear-on-an-emulator"></a>エミュレーターで Android の磨耗をデバッグする

_これらの記事では、エミュレーターで Xamarin. Android の磨耗アプリケーションをデバッグする方法について説明します。_

## <a name="debug-wear-on-emulator-overview"></a>エミュレーターのデバッグ磨耗の概要

Android の磨耗アプリケーションを開発するには、物理ハードウェア上で、またはエミュレーターまたはシミュレーターを使用して、アプリケーションを実行する必要があります。 ハードウェアを使用するのが最適な方法ですが、常に実用的というわけではありません。 多くの場合、以下に示すエミュレーターを使用して Android の磨耗ハードウェアをシミュレート/エミュレートすることで、より簡単かつコスト効率が向上します。 Android の摩耗アプリの展開と実行のプロセスにまだ慣れていない場合は、「 [Hello, 磨耗](~/android/wear/get-started/hello-wear.md)」を参照してください。

## <a name="configure-the-android-emulator"></a>Android Emulator を構成する

エミュレーターで摩耗アプリを実行するには、Android SDK Android Emulator をインストールし、Android 用の磨耗用に構成する必要があります。 Android SDK Emulator のインストールと構成に関する全体的な情報については、「 [Android Emulator セットアップ](~/android/get-started/installation/android-emulator/index.md)」を参照してください。

摩耗仮想デバイスを作成するときに、Android の磨耗装置プロファイル (たとえば、 **android の磨耗四角**) を選択します。 パフォーマンスを向上させるには、次の例に示すように、磨耗**x86** CPU/ABI を使用します。

[![の磨耗仮想デバイス構成の例](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)

## <a name="launch-the-wear-virtual-device"></a>摩耗仮想デバイスを起動する 

Android の磨耗仮想デバイスを作成した後は、デバッグを開始する前に、IDE の [デバイス] プルダウンメニューから選択できます。 仮想デバイスがデバイスのプルダウンで利用できない場合は、プロジェクトが (Android アプリプロジェクトではなく) Android の*磨耗*アプリケーションプロジェクトであり、そのターゲット API レベルが仮想デバイスと同じ api レベルに設定されていることを確認します。 (例:

[Visual Studio デバイスメニューでの磨耗 AVD の選択![](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android エミュレーターが起動すると、Xamarin Android によって、アプリがエミュレーターに展開されます。 エミュレーターは、構成済みの仮想デバイス イメージを使用してアプリを実行します。

この (または他のスポット画面) が最初に表示された場合は、驚くことはありません。 Watch emulator の起動には時間がかかることがあります。 

![Watch emulator が1分で表示されます...](debug-on-emulator-images/please-wait.png)

エミュレーターは実行したままでかまいません。アプリが実行されるたびにシャットダウンして再起動する必要はありません。

## <a name="summary"></a>まとめ

このガイドでは、磨耗開発用の Android Emulator を構成し、デバッグ用の磨耗仮想デバイスを起動する方法について説明しました。
