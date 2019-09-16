---
title: エミュレーターで Android Wear をデバッグする
description: これらの記事では、エミュレーターで Xamarin. Android Wear アプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: f085aaffbedb2965222b98a22cf6a4bb2393642b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764036"
---
# <a name="debug-android-wear-on-an-emulator"></a>エミュレーターで Android Wear をデバッグする

_これらの記事では、エミュレーターで Xamarin. Android Wear アプリケーションをデバッグする方法について説明します。_

## <a name="debug-wear-on-emulator-overview"></a>エミュレーターの Wear のデバッグ概要

Android Wear アプリケーションを開発するには、物理ハードウェア上で、またはエミュレーターまたはシミュレーターを使用して、アプリケーションを実行する必要があります。 ハードウェアを使用するのが最適な方法ですが、常に実用的というわけではありません。 多くの場合、以下に示すエミュレーターを使用して Android Wear ハードウェアをシミュレート/エミュレートすることで、より簡単かつコスト効率が向上します。 Android Wear アプリの展開と実行のプロセスにまだ慣れていない場合は、「 [Hello, Wear](~/android/wear/get-started/hello-wear.md)」を参照してください。

## <a name="configure-the-android-emulator"></a>Android Emulator を構成する

エミュレーターで Wear アプリを実行するには、Android SDK Android Emulator をインストールし、Android 用の Wear 用に構成する必要があります。 Android SDK Emulator のインストールと構成に関する全体的な情報については、「 [Android Emulator セットアップ](~/android/get-started/installation/android-emulator/index.md)」を参照してください。

Wear 仮想デバイスを作成するときに、Android Wear のデバイス プロファイル (たとえば、 **Android Wear Square**) を選択します。 パフォーマンスを向上させるには、次の例に示すように、Wear **x86** CPU/ABI を使用します。

[![Wear 仮想デバイス構成の例](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)

## <a name="launch-the-wear-virtual-device"></a>Wear 仮想デバイスを起動する 

Android Wear 仮想デバイスを作成した後は、デバッグを開始する前に、IDE の [デバイス] プルダウンメニューから選択できます。 仮想デバイスがデバイスのプルダウンで利用できない場合は、プロジェクトが (Android アプリプロジェクトではなく) Android の *Wear* アプリケーションプロジェクトであり、そのターゲット API レベルが仮想デバイスと同じ api レベルに設定されていることを確認します。 例えば:

[![Visual Studio デバイスメニューでの Wear AVD の選択](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android エミュレーターが起動すると、Xamarin Android によって、アプリがエミュレーターに展開されます。 エミュレーターは、構成済みの仮想デバイス イメージを使用してアプリを実行します。

この (または他のスポット画面) が最初に表示された場合は、驚くことはありません。 Watch emulator の起動には時間がかかることがあります。 

![Watch emulator が1分で表示されます...](debug-on-emulator-images/please-wait.png)

エミュレーターは実行したままでかまいません。アプリが実行されるたびにシャットダウンして再起動する必要はありません。

## <a name="summary"></a>まとめ

このガイドでは、Wear 開発用の Android Emulator を構成し、デバッグ用の Wear 仮想デバイスを起動する方法について説明しました。
