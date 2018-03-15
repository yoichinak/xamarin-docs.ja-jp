---
title: "Android Emulator のセットアップ"
description: "このセクションでは、アプリをテストするために Android SDK エミュレーターを準備する方法について説明します。 パフォーマンスを最大にするためにエミュレーターを高速化する方法について説明し、エミュレーター マネージャーを使用して仮想デバイスを作成およびカスタマイズする方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 55f5cf22718713fdcf11c49e0993f47c2f5a6f1d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="android-emulator-setup"></a>Android Emulator のセットアップ

_このセクションでは、アプリをテストするために Android SDK エミュレーターを準備する方法について説明します。パフォーマンスを最大にするためにエミュレーターを高速化する方法について説明し、エミュレーター マネージャーを使用して仮想デバイスを作成およびカスタマイズする方法を示します。_


## <a name="overview"></a>概要

Google Android SDK エミュレーターは、さまざまなデバイスをシミュレートするために、多様な構成で実行できます。 これらの構成がそれぞれ、_仮想デバイス_として作成されます。 このガイドでは、パフォーマンスを向上させるために Android エミュレーターを高速化する方法と、Xamarin Android Emulator Manager または従来の Google エミュレーター マネージャーを使用して、仮想デバイスを作成する方法を学習します。


> [!NOTE]
> Android SDK Tools バージョン **26.0.1** 以降、Google は、新しい CLI (コマンド ライン インターフェイス) ツールを優先して、既存の AVD/SDK マネージャーのサポートを削除しました。 この非推奨に関する変更のため、Android ツール 26.0.1 以降には、Google SDK/エミュレーター マネージャーの代わりに、Xamarin SDK/デバイス マネージャーが使用されるようになりました  (Xamarin SDK Manager の詳細については、「[Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください)。


## <a name="sections"></a>セクション

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[ハードウェアの高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Android SDK エミュレーターのパフォーマンスを最大にするためにコンピューターを準備する方法。 Android SDK エミュレーターはハードウェアの高速化なしでは非常に低速なため、Android SDK エミュレーターを使用する前に、お使いのコンピューターでハードウェア アクセラレータを有効にすることをお勧めします。

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Xamarin Android Device Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Xamarin Android Device Manager を使って、Android SDK エミュレーターの仮想デバイスを作成およびカスタマイズする方法。 現在プレビュー中の **Xamarin Android Device Manager** は、従来の Google エミュレーター マネージャーを置き換えることを目的としています。 Android Oreo 8.0 以降を対象とする場合は、Google エミュレーター マネージャーではなく、Xamarin Android Device Manager を使用する必要があります。

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Google エミュレーター マネージャー](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

従来の Google エミュレーター マネージャーを使用して、Android SDK エミュレーターの仮想デバイスを作成およびカスタマイズする方法。 Android SDK Tools バージョン 25.2.5 以前に元の Google エミュレーター マネージャーを残して、Google Android エミュレーターを実行し続けることができます。

Android SDK エミュレーターを構成した後は、「[Android SDK エミュレーター](~/android/deploy-test/debugging/android-sdk-emulator/index.md)」でエミュレーターの起動方法と、アプリのテストとデバッグでの使用方法を確認してください。
