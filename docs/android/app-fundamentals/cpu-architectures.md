---
title: CPU アーキテクチャ
description: Xamarin Android では、32ビットと64ビットのデバイスを含む複数の CPU アーキテクチャがサポートされています。 この記事では、1つまたは複数の Android でサポートされている CPU アーキテクチャにアプリを対象にする方法について説明します。
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2019
ms.openlocfilehash: 40cbc8b18c9f0bd177d2c8829aa0eac72fc6e2b8
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456185"
---
# <a name="cpu-architectures"></a>CPU アーキテクチャ

_Xamarin Android では、32ビットと64ビットのデバイスを含む複数の CPU アーキテクチャがサポートされています。この記事では、1つまたは複数の Android でサポートされている CPU アーキテクチャにアプリを対象にする方法について説明します。_

## <a name="cpu-architectures-overview"></a>CPU アーキテクチャの概要

リリース用にアプリを準備するときは、アプリがサポートするプラットフォームの CPU アーキテクチャを指定する必要があります。 1 つの APK に、複数の異なるアーキテクチャをサポートするためのマシン コードを含めることができます。 アーキテクチャ固有のコードの各コレクションは、 *アプリケーションバイナリインターフェイス* (ABI) に関連付けられています。 各 ABI は、このマシンコードが実行時に Android とどのように対話するかを定義します。
このしくみの詳細については、「 [マルチコアデバイス &amp; Xamarin. Android](~/android/deploy-test/multicore-devices.md)」を参照してください。

## <a name="how-to-specify-supported-architectures"></a>サポートされているアーキテクチャを指定する方法

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

通常、アプリが **リリース**用に構成されている場合は、アーキテクチャ (またはアーキテクチャ) を明示的に選択します。 アプリが **デバッグ**用に構成されている場合は、[ **共有ランタイムを使用** ] と [ **高速配置** オプションを使用する] が有効になっているため、明示的なアーキテクチャの選択が無効になります。

Visual Studio で、 **ソリューションエクスプローラー** の下にあるプロジェクトを右クリックし、[ **プロパティ**] を選択します。 [ **Android オプション** ] ページで、[ **パッケージングプロパティ** ] セクションを確認し、[ **共有ランタイムの使用** ] が無効になっていることを確認します (これをオフにすると、サポートする abis を明示的に選択できます)。 [ **詳細設定** ] ボタンをクリックし、[ **サポートされているアーキテクチャ**] で、サポートするアーキテクチャを確認します。

[![Armeabi と armeabi を選択する-armeabi-v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

通常、アプリが **リリース**用に構成されている場合は、アーキテクチャ (またはアーキテクチャ) を明示的に選択します。 アプリが **デバッグ**用に構成されている場合、[ **共有 Mono ランタイム** ] と [ **高速アセンブリデプロイ** ] オプションが有効になっているため、明示的なアーキテクチャを選択できません。

Visual Studio for Mac で、 **ソリューション** パッドでプロジェクトを見つけ、プロジェクトの横にある歯車アイコンをクリックして、[ **オプション**] を選択します。 [ **プロジェクトオプション** ] ダイアログボックスで、[ **Android ビルド**] をクリックします。 [ **全般** ] タブをクリックし、[ **共有 Mono ランタイムを使用** する] が無効になっていることを確認します (これをオフにすると、サポートする abis を明示的に選択できます)。 [ **詳細設定** ] タブをクリックし、[ **サポートされている abis**] の下で、サポートするアーキテクチャの abis を確認します。

[![Armeabi と armeabi を選択する-armeabi-v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----

Xamarin.Android では、次のアーキテクチャがサポートされています。

- **armeabi** &ndash; 少なくとも ARMv5TE 命令セットをサポートする ARM ベースの Cpu。 `armeabi`はスレッドセーフではなく、マルチ CPU デバイスでは使用できないことに注意してください。

> [!NOTE]
> [Xamarin.Android 9.2](/xamarin/android/release-notes/9/9.2#removal-of-support-for-armeabi-cpu-architecture) 以降、`armeabi` はサポート対象から除外されました。

- **armeabi-armeabi-v7a** &ndash; ハードウェアの浮動小数点演算と複数の CPU (SMP) デバイスを使用した ARM ベースの Cpu。 `armeabi-v7a`マシンコードは ARMv5 デバイスでは実行されないことに注意してください。

- **arm64-arm64-v8a** &ndash; 64ビット ARMv8 アーキテクチャに基づく Cpu。

- **x86** &ndash; X86 (または IA-32) 命令セットをサポートする Cpu。 この命令セットは、MMX、SSE、SSE2、SSE3 命令を含む、Pentium Pro のものと同じです。

- **x86_64** 64ビット x86 (   *x64* および *AMD64*とも呼ばれます) 命令セットをサポートする cpu。

Xamarin. Android では `armeabi-v7a` 、 **リリース** ビルドの既定値はに設定されています。 この設定は、よりもはるかに優れたパフォーマンス `armeabi` を提供します。 64ビットの ARM プラットフォームを対象としている場合 (たとえば、の場合は、)、を選択し `arm64-v8a` ます。 アプリを x86 デバイスに展開する場合は、を選択し `x86` ます。 ターゲットの x86 デバイスで64ビットの CPU アーキテクチャが使用されている場合は、を選択し `x86_64` ます。

## <a name="targeting-multiple-platforms"></a>複数のプラットフォームを対象にする

複数の CPU アーキテクチャをターゲットにするには、複数の ABI を選択できます (APK ファイルのサイズが大きくなります)。 サポートされているアーキテクチャごとに個別の APK を作成するには、[ **選択した ABI ごとに1つのパッケージ (apk) を生成** する] オプションを使用できます (「 [パッケージのプロパティの設定](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)」を参照)。

64ビットのデバイスを対象にするために、 **arm64 arm64-v8a** または **x86_64** を選択する必要はありません。64-64 ビットハードウェアでアプリを実行するために、ビットのサポートは必要ありません。 たとえば、64ビットの ARM デバイス (たとえば、 ["のよう](https://www.google.com/nexus/9/)な ARM デバイス") では、用に構成されたアプリを実行でき `armeabi-v7a` ます。 64ビットのサポートを有効にする主な利点は、アプリでより多くのメモリに対処できるようにすることです。

> [!NOTE]
> 2018 年 8 月から、新しいアプリは API レベル 26 をターゲットにすることが必須となります。また、2019 年 8 月から、アプリは 32 ビット バージョンに加えて [64 ビット バージョンを提供することが必須となります](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)。

## <a name="additional-information"></a>追加情報

場合によっては、アーキテクチャごとに個別の APK を作成する必要があります (APK のサイズを縮小する場合や、アプリに特定の CPU アーキテクチャに固有の共有ライブラリがある場合)。
この方法の詳細については、「 [ABI 固有の APKs のビルド](~/android/deploy-test/building-apps/abi-specific-apks.md)」を参照してください。