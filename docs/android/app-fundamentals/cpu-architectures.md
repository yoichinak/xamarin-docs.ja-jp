---
title: CPU アーキテクチャ
description: Xamarin.Android には、32 ビットおよび 64 ビットのデバイスを含め、いくつかの CPU アーキテクチャがサポートされています。 この記事では、1 つまたは複数の Android でサポートされている CPU アーキテクチャにアプリをターゲットにする方法について説明します。
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f2865858552d4445dff95c85767c41849c19cc29
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122271"
---
# <a name="cpu-architectures"></a>CPU アーキテクチャ

_Xamarin.Android には、32 ビットおよび 64 ビットのデバイスを含め、いくつかの CPU アーキテクチャがサポートされています。この記事では、1 つまたは複数の Android でサポートされている CPU アーキテクチャにアプリをターゲットにする方法について説明します。_

## <a name="cpu-architectures-overview"></a>CPU アーキテクチャの概要

どのプラットフォームの CPU アーキテクチャを指定する必要があります、リリースのアプリを準備するときに、アプリケーションがサポートされます。 1 つの APK に、複数の異なるアーキテクチャをサポートするためのマシン コードを含めることができます。 アーキテクチャ固有のコードの各コレクションが関連付けられている、*アプリケーション バイナリ インターフェイス*(ABI)。 各 ABI は、このマシン語コードの実行時に、Android とやり取りが予想される方法を定義します。
このしくみの詳細については、[マルチコア デバイス&amp;Xamarin.Android](~/android/deploy-test/multicore-devices.md)を参照してください。


## <a name="how-to-specify-supported-architectures"></a>サポートされているアーキテクチャを指定する方法

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

通常、明示的に選択したアーキテクチャ (またはアーキテクチャ)、アプリを構成するとき**リリース**します。 アプリを構成するとき**デバッグ**、**共有ランタイムを使用**と**Fast Deployment の使用**、明示的なアーキテクチャの選択を無効にするオプションが有効です。

Visual Studio では、下のプロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択**プロパティ**します。 下、 **Android オプション**チェック ページ、**パッケージング プロパティ**セクションし、いることを確認**共有ランタイムを使用**は無効になります (これをオフにすることができますに明示的にサポートする Abi を選択) します。 をクリックして、 **詳細設定**ボタンをクリックし、**サポートされているアーキテクチャ**、サポートするアーキテクチャを確認してください。

[![Armeabi と armeabi v7a の選択](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

通常、明示的に選択したアーキテクチャ (またはアーキテクチャ)、アプリを構成するとき**リリース**します。 アプリを構成するとき**デバッグ**、**共有 Mono ランタイムを使用する**と**アセンブリの配置を高速**、明示的なアーキテクチャを回避するオプションが有効選択範囲です。

Visual studio for Mac でプロジェクトを見つけ、**ソリューション**パッド、プロジェクトの横にある歯車アイコンをクリックし、選択**オプション**します。 **プロジェクト オプション**ダイアログ ボックスで、をクリックして**Android のビルド**します。 をクリックして、**全般** タブであることを確認し、**共有 Mono ランタイムを使用する**は無効です (これをオフにすることができますをサポートする Abi を明示的に選択)。 をクリックして、 **詳細設定**タブし、 **Abi のサポートされている**、サポートするアーキテクチャの Abi を確認してください。

[![Armeabi と armeabi v7a の選択](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android では、次のアーキテクチャがサポートされています。

-   **armeabi** &ndash;最低限 ARMv5TE 命令のセットをサポートする ARM ベースの Cpu。 なお`armeabi`スレッド セーフでないし、マルチ CPU のデバイスでは使用できません必要があります。

-   **armeabi v7a** &ndash; ARM ベースの Cpu ハードウェアの浮動小数点演算と複数の CPU (SMP) デバイス。 なお`armeabi-v7a`マシン コードは、ARMv5 デバイスでは実行できません。

-   **arm64 v8a** &ndash; Cpu が 64 ビット ARMv8 アーキテクチャに基づいています。

-   **x86** &ndash; x86 (または ia-32) をサポートする Cpu 命令セット。 この命令セットは、Pentium Pro、MMX、SSE、SSE2、および SSE3 の命令を含むのと同じです。

-   **x86_64** 64 ビット x86 をサポートする Cpu (とも呼ばれる*x64*と*AMD64*) 命令セット。

Xamarin.Android の既定値は`armeabi-v7a`の**リリース**をビルドします。 この設定よりパフォーマンスが大幅に向上`armeabi`します。 Nexus 9) などの 64 ビット ARM プラットフォームを対象とする場合は、選択`arm64-v8a`します。 X86 にアプリをデプロイするかどうかはデバイスで、`x86`します。 X86 を対象デバイスには、64 ビットの CPU アーキテクチャが使用されている場合は、選択`x86_64`します。

## <a name="targeting-multiple-platforms"></a>複数のプラットフォームを対象とします。

複数の CPU アーキテクチャを対象とは、1 つ以上の ABI (ただしより大きな APK ファイルのサイズ) を選択できます。 使用することができます、**選択した ABI ごとに 1 つのパッケージ (.apk) を生成**オプション (で説明されている[パッケージング プロパティの設定](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) ごとに個別の APK を作成するには、アーキテクチャをサポートします。

選択する必要はありません**arm64 v8a**または**x86_64** ; 64 ビットのデバイスを対象に 64 ビットのサポートは、64 ビット ハードウェアでアプリを実行する必要はありません。 たとえば、64 ビット ARM デバイス (など、 [Nexus 9](http://www.google.com/nexus/9/)) 用に構成されたアプリを実行できる`armeabi-v7a`します。 64 ビットのサポートを有効にする主な利点は、アプリがより多くのメモリに対処することができるようにするには。

> [!NOTE]
> 64 ビットのランタイム サポートは、現在、実験的な機能です。 64 ビット ランタイムでは*いない*64 ビットのデバイスでアプリを実行するために必要です。 

## <a name="additional-information"></a>追加情報

状況によっては、アーキテクチャごとに個別の APK を作成する必要があります (、APK のサイズを小さくか、アプリが特定の CPU アーキテクチャに固有のライブラリを共有するため)。
この方法の詳細については、[ABI 固有の Apk のビルド](~/android/deploy-test/building-apps/abi-specific-apks.md)を参照してください。
