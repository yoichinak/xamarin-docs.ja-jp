---
title: Android Emulator のセットアップ
description: Android Emulator は、さまざまなデバイスをシミュレートするために、多様な構成で実行できます。 このガイドでは、アプリをテストするために Android Emulator を準備する方法について説明します。
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/27/2018
ms.openlocfilehash: 148afe5354d7995f15dc19c6257ed2a1567162ec
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027952"
---
# <a name="android-emulator-setup"></a>Android Emulator のセットアップ

_このガイドでは、アプリをテストするために Android Emulator を準備する方法について説明します。_

## <a name="overview"></a>概要

Android Emulator は、さまざまなデバイスをシミュレートするために、多様な構成で実行できます。 各構成は、 _仮想デバイス_ と呼ばれます。 エミュレーターでアプリを展開してテストする場合は、Nexus や Pixel Phone などの物理的な Android デバイスをシミュレートする事前構成済みまたはカスタム仮想デバイスを選択します。

以下のセクションでは、Android Emulator を高速化してパフォーマンスを最大化する方法、Android Device Manager を使用して仮想デバイスを作成し、カスタマイズする方法、そして仮想デバイスのプロファイル プロパティをカスタマイズする方法について説明します。 さらに、トラブルシューティングのセクションでは、よくあるエミュレーターの問題とその回避策について説明します。

## <a name="sections"></a>セクション

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[エミュレーター パフォーマンスのためのハードウェア高速化](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Hyper-V または HAXM 仮想化テクノロジを使用して、Android Emulator のパフォーマンスを最大にするためにコンピューターを準備する方法。 Android Emulator はハードウェアの高速化なしでは非常に低速なため、このエミュレーターを使用する前に、お使いのコンピューターでハードウェア アクセラレータを有効にすることをお勧めします。

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)

Android Device Manager を使って仮想デバイスを作成およびカスタマイズする方法。

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Android 仮想デバイス プロパティの編集](~/android/get-started/installation/android-emulator/device-properties.md)

Android Device Manager を使用して仮想デバイスのプロファイル プロパティを編集する方法。

### <a name="android-emulator-troubleshootingandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Android Emulator のトラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md)

この記事では、Android Emulator の実行中に発生する、特に一般的な警告メッセージと問題をその回避策やヒントと共に説明します。

Android Emulator を構成した後は、「[Debugging on the Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)」(Android Emulator でのデバッグ) でエミュレーターの起動方法と、アプリのテストとデバッグでの使用方法を確認してください。

> [!NOTE]
> Android SDK Tools バージョン **26.0.1** 以降、Google は、新しい CLI (コマンド ライン インターフェイス) ツールを優先して、既存の AVD/SDK マネージャーのサポートを削除しました。 この非推奨に関する変更のため、Android Tools 26.0.1 以降には、Google SDK/Device Manager の代わりに、Xamarin SDK/Device Manager が使用されるようになりました。 Xamarin SDK Manager の詳細については、「[Xamarin.Android 向け Android SDK を設定する](~/android/get-started/installation/android-sdk.md)」を参照してください。
