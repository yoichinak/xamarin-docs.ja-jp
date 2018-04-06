---
title: Android SDK エミュレーターの実行
description: Android SDK エミュレーターでアプリをデバッグする方法
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 630520f88dd23d3860b5f42fbb9bc4eb35ca2c4b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="running-the-android-sdk-emulator"></a>Android SDK エミュレーターの実行

このガイドでは、アプリをデバッグしてテストするために Android SDK Emulator で仮想デバイスを起動する方法について説明します。

## <a name="using-a-pre-configured-virtual-device"></a>構成済み仮想デバイスの使用

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio には、デバイスのドロップダウン メニューに表示される構成済みの仮想デバイスが含まれます。 たとえば、次の Visual Studio 2017 のスクリーンショットでは、いくつかの構成済み仮想デバイスが使用可能です。

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![仮想デバイス](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

通常、電話アプリをテストしてデバッグする場合は、**VisualStudio\_android-23\_x86\_phone** 仮想デバイスを選択します。 これらの構成済み仮想デバイスのいずれかが要件を満たしている (つまり、アプリのターゲット API レベルと一致している) 場合は、「[エミュレーターの起動](#launching)」に進み、エミュレーターでのアプリの実行を開始します  (Android API レベルについてまだよく理解していない場合は、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」 (Android API レベルについて) を参照してください)。

Xamarin.Android プロジェクトで、使用可能な仮想マシンと互換性のないターゲット フレームワーク レベルを使用している場合は、ドロップダウン メニューの **[サポートされていないデバイス]** に使用不可の仮想デバイスがリストされます。 たとえば、以下のプロジェクトでは、ターゲット フレームワークが、既定で提供される **Android 6.0** 仮想デバイスと互換性のない **Android 7.1 Nougat (API 25)** に設定されています。

[![互換性のない仮想デバイス](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

使用可能な仮想デバイスの API レベルと一致するように、**[最小 Android ターゲットの変更]** をクリックして、プロジェクトの最小 Android バージョンを変更することができます。 また、**Android エミュレーター マネージャー**を使用して、後述の「[仮想デバイスの構成](#virtualdevice)」で説明するターゲット API レベルをサポートする新しい仮想デバイスを作成することもできます。 新しい API レベル用に仮想デバイスを構成するには、まず、その API レベルに対応するシステム イメージをインストールする必要があります。これについては、次のセクションで説明します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac には、デバイスのドロップダウン メニューに表示される構成済みの仮想デバイスが含まれます。 たとえば、次のスクリーンショットでは、2 つの構成済み仮想デバイスを使用できます。

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![仮想デバイス](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

通常、電話アプリをテストしてデバッグする場合は、**Android\_Accelerated\_x86** 仮想デバイスを選択します。 この構成済み仮想デバイスが要件を満たしている (つまり、アプリのターゲット API レベルと一致している) 場合は、「[エミュレーターの起動](#launching)」に進み、エミュレーターでのアプリの実行を開始します  (Android API レベルについてまだよく理解していない場合は、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」 (Android API レベルについて) を参照してください)。

-----

## <a name="creating-custom-virtual-devices"></a>カスタム仮想デバイスの作成

カスタム仮想デバイスを作成するには、Xamarin Android Device Manager または Android SDK の一部である従来の Google エミュレーター マネージャーを使用する必要があります。 仮想デバイスの作成とカスタマイズの詳細については、「[Xamarin Android Device Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)」を参照してください。
従来の Google エミュレーター マネージャーを使用する場合は、「[Google エミュレーター マネージャー](~/android/get-started/installation/android-emulator/google-emulator-manager.md)」を参照してください。

Android 8.0 Oreo 向けに開発している場合は、Xamarin Android Device Manager を使用する必要があることに注意してください。

<a name="launching" />

## <a name="launching-the-emulator"></a>エミュレーターの起動

IDE の上部近くにドロップダウン メニューがあります。このメニューを使用して、**デバッグ** モードまたは**リリース** モードを選択できます。 **[デバッグ]** を選択すると、エミュレーター内で実行されているアプリケーション プロセスにデバッガーがアタッチされます。 

デバイスのドロップダウン メニューから仮想デバイスを選択した後、**[デバッグ]** または **[リリース]** のいずれかのモードを選択し、**[再生]** ボタンをクリックしてアプリケーションを実行します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![[デバッグ] および [リリース] モードと [再生] ボタン](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![[デバッグ] および [リリース] モードと [再生] ボタン](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

Android エミュレーターが起動すると、Xamarin.Android はエミュレーターにアプリを展開します。 エミュレーターは、構成済みの仮想デバイス イメージを使用してアプリを実行します。 Android SDK エミュレーターの例のスクリーンショットを以下に示します (エミュレーターは **MyApp** という空のアプリを実行しています)。

![空のアプリを実行するエミュレーター](running-the-emulator-images/emulator-running.png)

エミュレーターは実行したままでかまいません。アプリが実行されるたびにシャットダウンして再起動する必要はありません。 Xamarin.Android アプリがエミュレーターで初めて実行されるときに、ターゲットとなる API レベルの Xamarin.Android 共有ランタイムがインストールされてからアプリケーションがインストールされます。 ランタイムのインストールには少し時間がかかる場合があります。完了するまでしばらくお待ちください。 ランタイムのインストールが行われるのは、Xamarin.Android アプリが初めてエミュレーターに展開される場合のみです。以降の展開では、エミュレーターにアプリがコピーされるだけなので、初回より時間はかかりません。

Android SDK エミュレーターの使用の詳細については、以下の Android 開発者向けトピックを参照してください。

-   [画面上で操作する](https://developer.android.com/studio/run/emulator.html#navigate)

-   [エミュレータ上で基本的なタスクを実行する](https://developer.android.com/studio/run/emulator.html#tasks)

-   [拡張コントロール、設定、ヘルプを利用する](https://developer.android.com/studio/run/emulator.html#extended)

