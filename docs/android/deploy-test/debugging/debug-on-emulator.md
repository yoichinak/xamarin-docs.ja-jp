---
title: Android Emulator でのデバッグ
description: このガイドでは、Visual Studio 内で Android Emulator を使用してアプリを起動およびデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 8ca13b4f9c961b8bb206d065ce3cf641a8662160
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028084"
---
# <a name="debugging-on-the-android-emulator"></a>Android Emulator でのデバッグ

_このガイドでは、アプリをデバッグしてテストするために Android Emulator で仮想デバイスを起動する方法について説明します。_

## <a name="overview"></a>概要

Android Emulator ( **.NET によるモバイル開発**のワークロードの一部としてインストールされている) は各種の Android デバイスをシミュレートするために、さまざまな構成で実行することができます。 これらの構成がそれぞれ、_仮想デバイス_として作成されます。 このガイドでは、Visual Studio からエミュレーターを起動する方法、および仮想デバイスでアプリを実行する方法を説明します。 Android Emulator の構成と、新しい仮想デバイスの作成については、「[Android Emulator のセットアップ](~/android/get-started/installation/android-emulator/index.md)」を参照してください。

## <a name="using-a-pre-configured-virtual-device"></a>構成済み仮想デバイスの使用

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio には、デバイスのドロップダウン メニューに表示される構成済みの仮想デバイスが含まれます。 たとえば、次の Visual Studio 2017 のスクリーンショットでは、いくつかの構成済み仮想デバイスが使用可能です。

- **VisualStudio\_android-23\_arm\_phone**

- **VisualStudio\_android-23\_arm\_tablet**

- **VisualStudio\_android-23\_x86\_phone** 

- **VisualStudio\_android-23\_x86\_tablet** 

[![仮想デバイス](debug-on-emulator-images/win/01-virtual-devices-sml.png)](debug-on-emulator-images/win/01-virtual-devices.png#lightbox)

通常、電話アプリをテストしてデバッグする場合は、**VisualStudio\_android-23\_x86\_phone** 仮想デバイスを選択します。 これらの構成済み仮想デバイスのいずれかが要件を満たしている (つまり、アプリのターゲット API レベルと一致している) 場合は、「[エミュレーターの起動](#launching)」に進み、エミュレーターでのアプリの実行を開始します (Android API レベルについてまだよく理解していない場合は、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」 (Android API レベルについて) を参照してください)。

Xamarin.Android プロジェクトで、使用可能な仮想マシンと互換性のないターゲット フレームワーク レベルを使用している場合は、ドロップダウン メニューの **[サポートされていないデバイス]** に使用不可の仮想デバイスがリストされます。 たとえば、以下のプロジェクトでは、ターゲット フレームワークが、この例にリストされている **Android 6.0** 仮想デバイスと互換性のない **Android 7.1 Nougat (API 25)** に設定されています。

[![互換性のない仮想デバイス](debug-on-emulator-images/win/02-incompatible-level-sml.png)](debug-on-emulator-images/win/02-incompatible-level.png#lightbox)

使用可能な仮想デバイスの API レベルと一致するように、 **[最小 Android ターゲットの変更]** をクリックして、プロジェクトの最小 Android バージョンを変更することができます。 また、[Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) を使用して、ターゲット API レベルをサポートする新しい仮想デバイスを作成することもできます。
新しい API レベル用に仮想デバイスを構成するには、まず、その API レベルに対応するシステム イメージをインストールする必要があります (「[Xamarin.Android 向け Android SDK を設定する](~/android/get-started/installation/android-sdk.md)」を参照)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac には、デバイスのドロップダウン メニューに表示される構成済みの仮想デバイスが含まれます。 たとえば、次のスクリーンショットでは、2 つの構成済み仮想デバイスを使用できます。

- **Android\_Accelerated\_x86**

- **Android\_ARMv7a**

[![仮想デバイス](debug-on-emulator-images/mac/01-virtual-devices-sml.png)](debug-on-emulator-images/mac/01-virtual-devices.png#lightbox)

通常、電話アプリをテストしてデバッグする場合は、**Android\_Accelerated\_x86** 仮想デバイスを選択します。 この構成済み仮想デバイスが要件を満たしている (つまり、アプリのターゲット API レベルと一致している) 場合は、「[エミュレーターの起動](#launching)」に進み、エミュレーターでのアプリの実行を開始します (Android API レベルについてまだよく理解していない場合は、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」 (Android API レベルについて) を参照してください)。

-----

## <a name="editing-virtual-devices"></a>仮想デバイスの編集

仮想デバイスを編集する (または新規に作成する) には、[Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) を使用する必要があります。

<a name="launching" />

## <a name="launching-the-emulator"></a>エミュレーターの起動

VisuaI Studio の上部近くにドロップダウン メニューがあります。このメニューを使用して、**デバッグ** モードまたは**リリース** モードを選択できます。 **[デバッグ]** を選択すると、アプリの起動後にエミュレーター内で実行されているアプリケーション プロセスにデバッガーがアタッチされます。 **[リリース]** モードを選択すると、デバッガーが無効になります (ただし、アプリを実行し、ログ ステートメントを使用してデバッグすることはできます)。 デバイスのドロップダウン メニューから仮想デバイスを選択した後、 **[デバッグ]** または **[リリース]** のいずれかのモードを選択し、[再生] ボタンをクリックしてアプリケーションを実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![[デバッグ] および [リリース] モードと [再生] ボタン](debug-on-emulator-images/win/17-debug-release-sml.png)](debug-on-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![[デバッグ] および [リリース] モードと [再生] ボタン](debug-on-emulator-images/mac/16-debug-release-sml.png)](debug-on-emulator-images/mac/16-debug-release.png#lightbox)

-----

エミュレーターが起動すると、Xamarin.Android はエミュレーターにアプリを展開します。 エミュレーターは、構成済みの仮想デバイス イメージを使用してアプリを実行します。 Android Emulator の例のスクリーン ショットを、下に表示します。 この例では、エミュレーターが **MyApp** という空のアプリを実行しています。

![空のアプリを実行するエミュレーター](debug-on-emulator-images/emulator-running.png)

エミュレーターは実行したままでかまいません。アプリが起動するたびにシャットダウンして再起動を待つ必要はありません。 Xamarin.Android アプリがエミュレーターで初めて実行されるときに、ターゲットとなる API レベルの Xamarin.Android 共有ランタイムがインストールされてからアプリケーションがインストールされます。 ランタイムのインストールには少し時間がかかる場合があります。完了するまでしばらくお待ちください。 ランタイムのインストールが行われるのは、Xamarin.Android アプリが初めてエミュレーターに展開される場合のみです。以降の展開では、エミュレーターにアプリがコピーされるだけなので、初回より時間はかかりません。

<a name="quick-boot" />

## <a name="quick-boot"></a>クイック ブート

新しいバージョンの Android Emulator には、ほんの数秒でエミュレーターを起動する_クイック ブート_と呼ばれる機能が含まれています。 エミュレーターを閉じるときに、仮想デバイスの状態のスナップショットが取得されるため、再起動時にその状態をすばやく復元できます。
この機能にアクセスするには、次が必要になります。

- Android Emulator バージョン 27.0.2 またはそれ以降
- Android SDK Tools バージョン 26.1.1 またはそれ以降

上記のバージョンのエミュレーターと SDK Tools がインストールされていると、クイック ブート機能が既定で有効になります。 

仮想デバイスの最初のコールド ブートではまだスナップショットが作成されていないため、実行速度が改善されません。

![コールド ブートのスクリーンショット](debug-on-emulator-images/cold-boot.png)

エミュレーターを終了すると、クイック ブートによりエミュレーターの状態がスナップショットで保存されます。

![シャットダウン時の状態の保存](debug-on-emulator-images/saving-state.png)

以後の仮想デバイスの起動では、エミュレーターの終了時の状態が単純に復元されるため、はるかに高速になります。

![再起動時の読み込み中の状態](debug-on-emulator-images/loading-state.png)

## <a name="troubleshooting"></a>トラブルシューティング

エミュレーターの一般的な問題のヒントと回避策は、「[エミュレーターのセットアップに関する問題のトラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md)」を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、Xamarin.Android アプリを実行およびテストするために、Android Emulator を構成するプロセスについて説明しました。 あらかじめ構成された仮想デバイスを使用してエミュレーターを起動する手順、および Visual Studio からエミュレーターにアプリケーションを配置するための手順について説明しました。 

Android Emulator の使用の詳細については、以下の Android 開発者向けトピックを参照してください。

- [画面上で操作する](https://developer.android.com/studio/run/emulator.html#navigate)

- [エミュレータ上で基本的なタスクを実行する](https://developer.android.com/studio/run/emulator.html#tasks)

- [拡張コントロール、設定、ヘルプを利用する](https://developer.android.com/studio/run/emulator.html#extended)

- [クイック ブートでエミュレーターを実行する](https://developer.android.com/studio/run/emulator#quickboot)
