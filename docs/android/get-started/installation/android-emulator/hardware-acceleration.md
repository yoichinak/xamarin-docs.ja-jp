---
title: エミュレーターのパフォーマンスのためのハードウェア高速化 (Hyper-V と HAXM)
description: この記事では、お使いのコンピューターのハードウェア高速化機能を利用し、Android Emulator のパフォーマンスを最大化する方法について説明します。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: jondouglas
ms.author: jodou
ms.date: 02/13/2020
ms.openlocfilehash: a776dbb2ecfaf0942d79c2b403c13f98cdc7c2e2
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571975"
---
# <a name="hardware-acceleration-for-emulator-performance-hyper-v--haxm"></a>エミュレーターのパフォーマンスのためのハードウェア高速化 (Hyper-V と HAXM)

_この記事では、お使いのコンピューターのハードウェア高速化機能を利用し、Android Emulator のパフォーマンスを最大化する方法について説明します。_

Visual Studio で開発すると、Android デバイスが利用できないか、実用的でない状況でも、Android エミュレーターを利用することで Xamarin.Android アプリケーションを簡単にテストしたり、デバッグしたりできます。
ただし、Android エミュレーターを実行するコンピューターにハードウェア高速化がない場合、エミュレーターの実行速度があまりにも遅くなります。 特殊な x86 仮想デバイス イメージとコンピューターの仮想化機能を組み合わせることで、Android エミュレーターのパフォーマンスを大幅に向上することができます。

| シナリオ    | HAXM        | WHPX       | Hypervisor.Framework |
| ----------- | ----------- | -----------| ----------- |
| Intel プロセッサを所有している | x | x | x |
| AMD プロセッサを所有している   |   | x |   |
| Hyper-V をサポートする |   | x |   |
| 入れ子になった仮想化をサポートする |   | 制限 |   |
| Docker などのテクノロジを使用する  |   | x | x |

::: zone pivot="windows"

## <a name="accelerating-android-emulators-on-windows"></a>Windows 上での Android エミュレーターの高速化

Android エミュレーターの高速化には、次の仮想化テクノロジを利用できます。

1. **Microsoft の Hyper-V と Windows Hypervisor Platform (WHPX)** 。
   [Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/) は Windows の仮想化技術の 1 つであり、物理的ホスト コンピューター上で、仮想化されたコンピューター システムを実行できます。

2. **Intel の Hardware Accelerated Execution Manager (HAXM)** 。
   HAXM は Intel CPU を実行するコンピューターのための仮想化エンジンです。

Windows で最適なエクスペリエンスを実現するには、WHPX を使用して Android エミュレーターを高速化することをお勧めします。 お使いのコンピューターで WHPX を使用できない場合は、HAXM を使用できます。 Android エミュレーターは、次の条件が満たされている場合、ハードウェア高速化を自動的に利用します。

- 開発コンピューターにハードウェア高速化機能があり、それが有効になっている。

- **x86** ベースの仮想デバイスのために作成されたシステム イメージをエミュレーターが実行している。

> [!IMPORTANT]
> VirtualBox、VMWare、Docker などでホストされる別の VM 内では VM による高速エミュレーターを実行できません。 Android Emulator は、[システム ハードウェアで直接](https://developer.android.com/studio/run/emulator-acceleration.html#extensions)実行する必要があります。

Android エミュレーターの起動とデバッグについては、「[Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)」を参照してください。

<a name="hyper-v-win"></a>

## <a name="accelerating-with-hyper-v"></a>Hyper-V による高速化

Hyper-V を有効にする前に、次のセクションを読み、コンピューターが Hyper-V をサポートしていることを確認してください。

### <a name="verifying-support-for-hyper-v"></a>Hyper-V のサポートを確認する

Hyper-V は Windows ハイパーバイザー プラットフォーム上で動作します。 Hyper-V と共に Android エミュレーターを使用するには、コンピューターが Windows ハイパーバイザー プラットフォームをサポートするために次の条件を満たしている必要があります。

- コンピューター ハードウェアは次の条件を満たしている必要があります。

  - 第 2 レベルのアドレス変換 (SLAT) を備えた 64 ビット Intel または AMD Ryzen CPU。
  - VM モニター モード拡張機能 (Intel CPU 上の VT-c) の CPU サポート。
  - 4 GB 以上のメモリ。

- お使いのコンピューターの BIOS で、次の項目を有効にする必要があります。

  - 仮想化テクノロジ (マザーボードの製造元によって、ラベルが異なる場合があります)。
  - ハードウェアによるデータ実行防止。

- お使いのコンピューターは、Windows 10 April 2018 更新プログラム (ビルド 1803) 以降に更新する必要があります。 Windows のバージョンが最新であることを確認するには、次の手順を実行します。

  1. Windows 検索ボックスに「**バージョン情報**」と入力します。
  2. 検索結果から **[PC 情報]** を選択します。
  3. **[バージョン情報]** ダイアログ内で下にスクロールし、 **[Windows の仕様]** セクションを表示します。
  4. **[バージョン]** が 1803 以降であることを確認します。

      [![Windows の仕様](hardware-acceleration-images/win/01-about-windows-w10-sml.png)](hardware-acceleration-images/win/01-about-windows-w10.png#lightbox)

コンピューターのハードウェアとソフトウェアが Hyper-V と互換性があることを確認するには、コマンド プロンプトを開き、次のコマンドを入力します。

```cmd
systeminfo
```

表示されているすべての Hyper-V 要件の値が **[はい]** の場合、コンピューターは Hyper-V をサポートできます。 次に例を示します。

[![systeminfo の出力例](hardware-acceleration-images/win/02-systeminfo-w158-sml.png)](hardware-acceleration-images/win/02-systeminfo-w158.png#lightbox)

### <a name="enabling-hyper-v-acceleration"></a>Hyper-V の高速化を有効にする

コンピューターが上記の条件を満たしている場合は、次の手順を実行し、Hyper-V を使用して Android エミュレーターを高速化します。

1. Windows 検索ボックスに「**Windows の機能**」を入力し、検索結果で **[Windows の機能の有効化または無効化]** を選択します。 **[Windows の機能]** ダイアログで **[Hyper-V]** と **[Windows ハイパーバイザー プラットフォーム]** を有効にします。

    [![Hyper-V と Windows ハイパーバイザー プラットフォームを有効にする](hardware-acceleration-images/win/03-hyper-v-settings-w158-sml.png)](hardware-acceleration-images/win/03-hyper-v-settings-w158.png#lightbox)

   これらの変更を加えたら、コンピューターを再起動します。
   
> [!IMPORTANT]
>
> Windows 10 の 2018 年 10 月の更新プログラム (RS5) 以降では、Windows ハイパーバイザー プラットフォーム (WHPX) が自動的に使用されるため、Hyper-V のみを有効にする必要があります。

2. **[Visual Studio 15.8 以降](https://visualstudio.microsoft.com/vs/)をインストールします** (このバージョンの Visual Studio には Hyper-V で Android エミュレーターを実行するための IDE が用意されています)。

3. **Android Emulator パッケージ 27.2.7 以降をインストールします**。 このパッケージをインストールするには、Visual Studio で **[ツール]、[Android]、[Android SDK Manager]** の順に移動します。 **[ツール]** タブを選択し、Android エミュレーターのバージョンが確実に 27.2.7 以降であるようにします。 また、Android SDK Tools バージョンが 26.1.1 以降であることを確認します。

    [![[Android SDK とツール] ダイアログ](hardware-acceleration-images/win/04-sdk-manager-w158-sml.png)](hardware-acceleration-images/win/04-sdk-manager-w158.png#lightbox)

仮想デバイスを作成する (「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照) ときに、必ず **x86** ベースのイメージを選択してください。 ARM ベースのシステム イメージを使用すると、仮想デバイスは高速化されず、低速で実行されます。

## <a name="accelerating-with-haxm"></a>HAXM による高速化

お使いのコンピューターが Hyper-V をサポートしていない場合は、HAXM を使用して Android エミュレーターを高速化します。 HAXM を使用する場合は、[Device Guard を無効にする](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#disable-devguard)必要があります。

### <a name="verifying-haxm-support"></a>HAXM のサポートを確認する

お使いのハードウェアが HAXM をサポートしているかどうかを判断するには、「[Does My Processor Support Intel Virtualization Technology?](https://www.intel.com/content/www/us/en/support/processors/000005486.html)」(このプロセッサは Intel 仮想化テクノロジをサポートしていますか?) の手順に従ってください。
ハードウェアが HAXM をサポートしている場合は、次の手順を使用して HAXM が既にインストールされているかどうかを確認できます。

1. コマンド プロンプト ウィンドウを開き、次のコマンドを入力します。

    ```cmd
    sc query intelhaxm
    ```

2. 出力で、HAXM プロセッサが実行されているかどうかを確認します。 この場合、出力に `intelhaxm` の状態は `RUNNING` と表示されます。 次に例を示します。

    ![HAXM を使用できる場合の sc クエリ コマンドの出力](hardware-acceleration-images/win/05-sc_query-w158.png)

   `STATE` が `RUNNING` に設定されていない場合、HAXM はインストールされていません。

HAXM をサポートするコンピューターで、HAXM がインストールされていない場合は、次のセクションの手順に従って HAXM をインストールします。

<a name="install-haxm-win"></a>

### <a name="installing-haxm"></a>HAXM のインストール

Windows 版の HAXM インストール パッケージは、どちらも「[Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager)」ページから入手できます。 次の手順に従って、HAXM をダウンロードしてインストールします。

1. Intel の Web サイトから Windows 版の最新の [HAXM 仮想化エンジン](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/)のインストーラーをダウンロードします。 Intel の Web サイトから直接 HAXM インストーラーをダウンロードする利点は、確実に最新バージョンを使用できることです。

2. **intelhaxm android.exe** を実行して、HAXM インストーラーを起動します。 インストーラーのダイアログの既定値をそのまま使用します。

   ![Intel Hardware Accelerated Execution Manager のセットアップ ウィンドウ](hardware-acceleration-images/win/06-haxm-installer.png)

仮想デバイスを作成する (「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照) ときに、必ず **x86** ベースのイメージを選択してください。 ARM ベースのシステム イメージを使用すると、仮想デバイスは高速化されず、低速で実行されます。

## <a name="troubleshooting"></a>トラブルシューティング

ハードウェア高速化の問題のトラブルシューティングについては、Android エミュレーターの[トラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#accel-issues-win) ガイドを参照してください。

::: zone-end
::: zone pivot="macos"

## <a name="accelerating-android-emulators-on-macos"></a>macOS 上での Android エミュレーターの高速化

Android エミュレーターの高速化には、次の仮想化テクノロジを利用できます。

1. **Apple の Hypervisor フレームワーク**
   [Hypervisor](https://developer.apple.com/documentation/hypervisor) は、Mac 上で仮想マシンを実行できるようにする macOS 10.10 以降の機能です。

2. **Intel の Hardware Accelerated Execution Manager (HAXM)** 。
   [HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm) は Intel CPU を実行するコンピューターのための仮想化エンジンです。

Android エミュレーターを高速化するには、ハイパーバイザー フレームワークを使用することをお勧めします。 お使いの Mac 上で Hypervisor フレームワークを使用できない場合は、HAXM を使用できます。 Android エミュレーターは、次の条件が満たされている場合、ハードウェア高速化を自動的に利用します。

- 開発コンピューターにハードウェア高速化機能があり、それが有効になっている。

- **x86** ベースの仮想デバイスのために作成されたシステム イメージをエミュレーターが実行している。

> [!IMPORTANT]
>
> VirtualBox、VMWare、Docker などでホストされる別の VM 内では VM による高速エミュレーターを実行できません。 Android Emulator は、[システム ハードウェアで直接](https://developer.android.com/studio/run/emulator-acceleration.html#extensions)実行する必要があります。

Android エミュレーターの起動とデバッグについては、「[Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)」を参照してください。

<a name="hypervisor"></a>

## <a name="accelerating-with-the-hypervisor-framework"></a>Hypervisor フレームワークによる高速化

Hypervisor フレームワークで Android エミュレーターを使用するには、Mac が次の条件を満たしている必要があります。

- Mac が macOS 10.10 以降を実行している必要がある。

- Mac の CPU が Hypervisor フレームワークをサポートできる必要がある。

お使いの Mac がこれらの条件を満たしている場合、Android エミュレーターは高速化に Hypervisor フレームワークを自動的に使用します。 Hypervisor フレームワークがお使いの Mac でサポートされているかどうか不明な場合は、[トラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#hypervisor-issues) ガイドで Mac が Hypervisor をサポートしていることを確認する方法を参照してください。

Hypervisor フレームワークがお使いの Mac でサポートされていない場合は、Android エミュレーターの高速化に HAXM を使用できます (次に説明します)。

<a name="haxm-mac"></a>

## <a name="accelerating-with-haxm"></a>HAXM による高速化

お使いの Mac が Hypervisor フレームワークをサポートしていない (または 10.10 より前のバージョンの macOS を使用している) 場合は、**Intel の Hardware Accelerated Execution Manager** ([HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm)) を使用して Android エミュレーターを高速化できます。

初めて HAXM で Android エミュレーターを使用する場合は、HAXM がインストールされ、Android エミュレーターを使用できることを事前に確認することをお勧めします。

### <a name="verifying-haxm-support"></a>HAXM のサポートを確認する

次の手順を使用して HAXM が既にインストールされているかどうかを確認できます。

1. ターミナルを開き、次のコマンドを入力します。

    ```bash
    ~/Library/Developer/Xamarin/android-sdk-macosx/tools/emulator -accel-check
    ```

   このコマンドでは、Android SDK が既定の場所 ( **~/Library/Developer/Xamarin/android-sdk-macosx**) にインストールされていることを前提としています。そうでない場合は、上記のパスをお使いの Mac 上にある Android SDK の場所に変更します。

2. HAXM がインストールされている場合、上記のコマンドで次の結果のようなメッセージが返されます。

    ```bash
    HAXM version 7.2.0 (3) is installed and usable.
    ```

   HAXM がインストールされて*いない*場合、次の出力のようなメッセージが返されます。

    ```bash
    HAXM is not installed on this machine (/dev/HAX is missing).
    ```

HAXM がインストールされていない場合は、次のセクションの手順を使用して、HAXM をインストールします。

<a name="install-haxm-mac"></a>

### <a name="installing-haxm"></a>HAXM のインストール

macOS 版の HAXM インストール パッケージは、どちらも「[Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager)」ページから入手できます。 次の手順に従って、HAXM をダウンロードしてインストールします。

1. Intel の Web サイトから macOS 版の最新の [HAXM 仮想化エンジン](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/)のインストーラーをダウンロードします。

2. HAXM インストーラーを実行します。 インストーラーのダイアログの既定値をそのまま使用します。

   [![Intel Hardware Accelerated Execution Manager のセットアップ ウィンドウ](hardware-acceleration-images/mac/01-haxm-installer-sml.png)](hardware-acceleration-images/mac/01-haxm-installer.png#lightbox)

## <a name="troubleshooting"></a>トラブルシューティング

ハードウェア高速化の問題のトラブルシューティングについては、Android エミュレーターの[トラブルシューティング](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#accel-issues-mac) ガイドを参照してください。

::: zone-end

## <a name="related-links"></a>関連リンク

- [Android Emulator でアプリを実行する](https://developer.android.com/studio/run/emulator)
