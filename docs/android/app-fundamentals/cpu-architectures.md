---
title: "CPU アーキテクチャ"
description: "Xamarin.Android には、32 ビットおよび 64 ビットのデバイスも含め、複数の CPU アーキテクチャがサポートされています。 この記事では、1 つまたは複数の Android でサポートされている CPU アーキテクチャにアプリを対象にする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 6139b5e27e9689da6366a2107acc14a6adcfc928
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="cpu-architectures"></a>CPU アーキテクチャ

_Xamarin.Android には、32 ビットおよび 64 ビットのデバイスも含め、複数の CPU アーキテクチャがサポートされています。この記事では、1 つまたは複数の Android でサポートされている CPU アーキテクチャにアプリを対象にする方法について説明します。_

## <a name="cpu-architectures-overview"></a>CPU アーキテクチャの概要

リリースのアプリを準備するときに、どのプラットフォームの CPU アーキテクチャを指定する必要があります、アプリがサポートします。 1 つの APK に、複数の異なるアーキテクチャをサポートするためのマシン コードを含めることができます。 アーキテクチャ固有のコードの各コレクションに関連付けられている、*アプリケーション バイナリ インターフェイス*(ABI)。 各 ABI は、このマシン語コードを実行時に、Android とやり取りする期待する方法を定義します。
このしくみの詳細については、次を参照してください。[マルチコア デバイス&amp;Xamarin.Android](~/android/deploy-test/multicore-devices.md)です。


## <a name="how-to-specify-supported-architectures"></a>サポートされているアーキテクチャを指定する方法

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

通常は、明示的に選択する、アーキテクチャ (アーキテクチャ) アプリを構成するとき**リリース**です。 アプリを構成するとき**デバッグ**、**共有ランタイムを使用**と**を使用して高速展開**、明示的なアーキテクチャの選択を無効にするオプションが有効です。

Visual Studio で、ダブルクリック**プロパティ**のプロジェクトの下**ソリューション エクスプ ローラー**を選択し、 **Android オプション**ページ。 クリックして、**パッケージ** タブであることを確認し、**共有ランタイムを使用**は無効になります (これをオフにすることができますをサポートするどの ABIs を明示的に選択) します。 をクリックして、 **[詳細設定]** ] タブと [**プロパティの詳細**をサポートするアーキテクチャを確認します。

[ ![Armeabi と armeabi v7a の選択](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

通常は、明示的に選択する、アーキテクチャ (アーキテクチャ) アプリを構成するとき**リリース**です。 アプリを構成するとき**デバッグ**、**共有モノラル ランタイムを使用する**と**アセンブリの展開を高速**オプションが有効に、明示的なアーキテクチャを回避します。選択範囲です。

Mac 用 Visual Studio でのプロジェクトを検索、**ソリューション**の余白を埋める、プロジェクトの横にある歯車アイコンをクリックし、選択**オプション**です。 **プロジェクト オプション**ダイアログ ボックスで、をクリックして**Android ビルド**です。 をクリックして、**全般** タブであることを確認し、**共有モノラル ランタイムを使用する**は無効になります (これをオフにすることができますをサポートするどの ABIs を明示的に選択) します。 をクリックして、 **[詳細設定]** ] タブと [**サポート ABIs**ABIs のアーキテクチャをサポートすることを確認します。

[ ![Armeabi と armeabi v7a の選択](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png)

-----


Xamarin.Android には、次のアーキテクチャがサポートされています。

-   **armeabi** &ndash;少なくとも ARMv5TE 命令セットをサポートする ARM ベースの Cpu。 なお`armeabi`スレッド セーフではないと、マルチ CPU デバイスでは使用できません。

-   **armeabi v7a** &ndash; ARM ベースの Cpu ハードウェア浮動小数点演算と複数の CPU (SMP) デバイスを使用します。 なお`armeabi-v7a`ARMv5 デバイスでのマシン語コードを実行できません。

-   **arm64 v8a** &ndash; Cpu は 64 ビット ARMv8 アーキテクチャに基づいています。

-   **x86** &ndash; x86 (32) をサポートする Cpu 命令セット。 この命令セットは、MMX、SSE、SSE2、および SSE3 命令を含む、Pentium Pro のと同じです。

-   **x86_64** 、64 ビット x86 をサポートする Cpu (とも呼ばれる*x64*と*AMD64*) 命令セット。

Xamarin.Android の既定値は`armeabi-v7a`の**リリース**を構築します。 この設定よりも大幅に優れたパフォーマンスを提供する`armeabi`です。 (Nexus 9) などの 64 ビット ARM プラットフォームを対象とする場合は、選択`arm64-v8a`です。 X86 にアプリを配置するかどうかはデバイスで、`x86`です。 X86 を対象デバイスには、64 ビット CPU アーキテクチャが使用されている場合は、選択`x86_64`です。

## <a name="targeting-multiple-platforms"></a>複数のプラットフォームを対象とします。

複数の CPU アーキテクチャをターゲットにするには、(を犠牲にして大きい APK ファイルのサイズ) の 2 つ以上の ABI を選択できます。 使用することができます、**選択 ABI ごとに 1 つのパッケージ (.apk) の生成**オプション (」に記載[パッケージのプロパティを設定](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) ごとに個別の APK を作成するには、アーキテクチャをサポートします。

選択する必要はありません**arm64 v8a**または**x86_64**を 64 ビットのデバイスを対象とする 64 ビットのサポートは、64 ビット ハードウェアでアプリを実行する必要はありません。 たとえば、64 ビットの ARM デバイス (など、 [Nexus 9](http://www.google.com/nexus/9/)) を構成したアプリを実行できる`armeabi-v7a`です。 64 ビット サポートを有効化の主な利点は、アプリが多くのメモリをできるようにするにです。

> [!NOTE]
> **注:**: 64 ビットのランタイム サポート、現在は実験的な機能です。 64 ビット ランタイムでは*いない*64 ビットのデバイスでアプリを実行するために必要です。 

## <a name="additional-information"></a>追加情報

一部の状況では、アーキテクチャごとに個別の APK を作成する必要があります (、APK のサイズを小さくか、アプリが特定の CPU アーキテクチャに固有のライブラリに共有できたため)。
この方法の詳細については、次を参照してください。 [ABI に固有のビルド APKs](~/android/deploy-test/building-apps/abi-specific-apks.md)です。
