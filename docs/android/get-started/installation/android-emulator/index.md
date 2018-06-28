---
title: Google Android Emulator のセットアップ
description: Google Android Emulator は、さまざまなデバイスをシミュレートするために、多様な構成で実行できます。 このガイドでは、アプリをテストするために Android Emulator を準備する方法について説明します。
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732532"
---
# <a name="google-android-emulator-setup"></a>Google Android Emulator のセットアップ

_このガイドでは、アプリをテストするために Google Android Emulator を準備する方法について説明します。_


## <a name="overview"></a>概要

Google Android Emulator は、さまざまなデバイスをシミュレートするために、多様な構成で実行できます。 各構成は、_仮想デバイス_と呼ばれます。 エミュレーターでアプリを展開してテストする場合は、Nexus や Pixel Phone などの物理的な Android デバイスをシミュレートする事前構成済みまたはカスタム仮想デバイスを選択します。

以下のセクションでは、Google Android Emulator を高速化してパフォーマンスを最大化する方法、Android Device Manager を使用して仮想デバイスを作成し、カスタマイズする方法、そして仮想デバイスのプロファイル プロパティをカスタマイズする方法について説明します。 さらに、トラブルシューティングのセクションでは、よくあるセットアップの問題とその回避策について説明します。

## <a name="sections"></a>セクション

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[エミュレーター パフォーマンスのためのハードウェア高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Android Emulator のパフォーマンスを最大にするためにコンピューターを準備する方法。
Google Android Emulator はハードウェアの高速化なしでは非常に低速なため、このエミュレーターを使用する前に、お使いのコンピューターでハードウェア アクセラレータを有効にすることをお勧めします。

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)

Android Device Manager を使って仮想デバイスを作成およびカスタマイズする方法。

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)

Android Device Manager を使用して Android 仮想デバイスのプロファイル プロパティを編集する方法。

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[エミュレーターのセットアップに関するトラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md)

Android Emulator のセットアップ時に発生する可能性がある Android Device Manager の問題を診断して修正する方法。


Android Emulator を構成した後は、「[Debugging with the Google Android Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)」(Google Android Emulator でのデバッグ) でエミュレーターの起動方法と、アプリのテストとデバッグでの使用方法を確認してください。


> [!NOTE]
> Android SDK Tools バージョン **26.0.1** 以降、Google は、新しい CLI (コマンド ライン インターフェイス) ツールを優先して、既存の AVD/SDK マネージャーのサポートを削除しました。 この非推奨に関する変更のため、Android Tools 26.0.1 以降には、Google SDK/Device Manager の代わりに、Xamarin SDK/Device Manager が使用されるようになりました。 Xamarin SDK Manager の詳細については、「[Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください。

