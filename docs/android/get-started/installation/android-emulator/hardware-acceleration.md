---
title: Android Emulator ハードウェアの高速化
description: Google Android Emulator の性能を最大まで上げるための準備をコンピューターに行う方法
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/10/2018
ms.openlocfilehash: b5c20eb9f40bb4c4981d6b60b9fd4bc75fd29336
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Android Emulator ハードウェアの高速化

ハードウェアの高速化なしでは Google Android Emulator は遅すぎて使用がためらわれます。 x86 ハードウェアを対象とする特別なエミュレーター ハードウェア イメージと 2 つある仮想化技術のいずれかを使用することで Google Android Emulator のパフォーマンスを大幅に改善できます。仮想化テクノロジは次のようになります。

1. **Microsoft の Hyper-V と Hypervisor Platform** &ndash; Hyper-V は Windows 10 で利用できる仮想化コンポーネントであり、仮想化したコンピューター システムを物理ホスト上で実行できます。 高速化した Android Emulator イメージにお勧めの仮想化テクノロジです。 Hyper-V の詳細については、[Windows 10 の Hyper-V ガイド](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/)をご覧ください。
2. **Intel の Hardware Accelerated Execution Manager (HAXM)** &ndash; これは Intel CPU を実行しているコンピューターのための仮想化エンジンです。 Hyper-V を使用できない開発者にお勧めの仮想化エンジンです。

Android SDK Manager では、**x86** ベースの仮想デバイスのためのエミュレーター イメージを実行しているとき、自動的にハードウェア高速化が使用されます (詳細は[構成と使用](~/android/deploy-test/debugging/android-sdk-emulator/index.md)に関するページに説明されています)。

## <a name="hyper-v-overview"></a>Hyper-V の概要

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V サポートは現在プレビュー段階です。

Windows 10 (2018 年 4 月更新) を使用している開発者には、Microsoft の Hyper-V の使用を強くお勧めします。 Visual Studio Tools for Xamarin で開発すると、Android デバイスが利用できないか、実効的ない状況でも Xamarin.Android アプリケーションを簡単にテストしたり、デバッグしたりすることができます。

Hyper-V と Google Android Emulator の使用を始めるには:

1. **Windows 10 2018 年 4 月更新 (ビルド 1803) に更新する** &ndash; 実行している Windows のバージョンを確認するには、Cortana 検索バー内をクリックし、「**バージョン情報**」と入力します。 検索結果から **[PC 情報]** を選択します。 **[バージョン情報]** ダイアログ内で下にスクロールし、**[Windows の仕様]** セクションを表示します。 **バージョン**は 1803 以降になります。

    [![Windows の仕様](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Hyper-V と Windows Hypervisor Platform の両方を有効にする** &ndash; Cortana 検索バーに「**Windows の機能の有効化または無効化**」と入力します。
   **[Windows の機能]** ダイアログ内で下にスクロールし、**[Windows Hypervisor Platform]** が有効になっていることを確認します。

    [![Hyper-V と Windows Hypervisor Platform が有効になっています](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    場合によっては、Hyper-V と Windows Hypervisor Platform を有効にしてから、Windows を再起動する必要があります。

3. **[Visual Studio 15.8 プレビュー 1](https://aka.ms/hyperv-emulator-dl) をインストールします**。&ndash; このバージョンの Visual Studio は、Google Android Emulator と共に Hyper-V を使用するための IDE に対応しています。

4. **Google Android エミュレーター パッケージ 27.2.7 以降をインストールします**。&ndash; このパッケージをインストールするには、Visual Studio で **[ツール]、[Android]、[Android SDK Manager]** の順に移動します。 **[ツール]** タブを選択し、Android Emulator コンポーネントがバージョン 27.2.7 以降であることを確認します。

    [![[Android SDK とツール] ダイアログ](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Android Emulator バージョンが 27.3.1 未満の場合、**既知の問題** (次のセクション) で説明する追加の回避策を適用します。


### <a name="known-issues"></a>既知の問題

-   エミュレーターのバージョンが 27.2.7 以上、27.3.1 未満の場合、Hyper-V を使用するには次の回避策が必要になります。
    1.  **C:\\Users\\_username_\\.android** フォルダーに **advancedFeatures.ini** という名前のファイルがなければ、それを作成します。
    2.  次の行を **advancedFeatures.ini** に追加します。
        ```
        WindowsHypervisorPlatform = on
        ```

-   Intel または AMD ベースの一部のプロセッサでは、パフォーマンスが下がることがあります。

-   Android アプリケーションは、展開時、読み込みに膨大な時間を要することがあります。

-   MMIO アクセス エラーが発生し、Android エミュレーターの起動が途切れることがあります。 この問題はエミュレーターを再起動することで解決されます。


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Hyper-V を使用するには Windows 10 が必要です。 詳細については、[Hyper-V の要件](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements)に関するページを参照してください。

-----

## <a name="haxm-overview"></a>HAXM の概要

HAXM は、Intel Virtualization Technology (VT) を使用してホスト コンピューター上の Android アプリのエミュレーションを高速化するハードウェア依存の仮想化エンジン (ハイパーバイザー) です。 Intel が提供する Android x86 エミュレーター イメージと公式の Android SDK Manager を組み合わせることで、HAXM は VT 対応のシステムで Android のエミュレーションを高速化することができます。 

VT 機能を備えている Intel CPU を搭載したコンピューターで開発している場合は、HAXM を利用して Google Android Emulator を大幅に高速化できます (ご使用の CPU が VT をサポートしているかどうかがわからない場合は、「[使用中のプロセッサーはインテル® バーチャライゼーション・テクノロジーに対応していますか?](https://www.intel.com/content/www/us/en/support/processors/000005486.html)」を参照してください)。

> [!NOTE]
> VirtualBox、VMWare、Docker などでホストされる別の VM 内では VM による高速エミュレーターを実行できません。 Google Android エミュレーターは、[システム ハードウェアで直接](https://developer.android.com/studio/run/emulator-acceleration.html#extensions)実行する必要があります。

初めて Google Android Emulator を使用する場合は、事前に HAXM がインストールされ、Google Android Emulator を使用できることを確認することをお勧めします。

### <a name="verifying-haxm-installation"></a>HAXM インストールの確認

エミュレーターの起動時に、**[Starting Android Emulator]\(Android エミュレーターの起動\)** ウィンドウを表示して HAXM が使用可能かどうかを確認することができます。 Google Android Emulator を起動するには、次の操作を行います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **[ツール]、[Android]、[Android エミュレーター マネージャー]** の順にクリックして、Android エミュレーター マネージャーを起動します。

    [![Android エミュレーター マネージャーのメニュー項目の場所](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. 次のような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示される場合は、HAXM がまだインストールされていないか、コンピューター上で正しく構成されていません。

    ![HAXM の準備ができていないことを示す [Performance Warning]\(パフォーマンスに関する警告\) ダイアログ](hardware-acceleration-images/win/11-perf-warn.png)

   このような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示された場合は、「[パフォーマンスに関する警告](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)」で原因を特定し、根本的な問題を解決します。

3. **x86** イメージ (**VisualStudio\_android 23\_x86\_phone** など) を選択し、**[Start]\(開始\)**、**[Launch]\(起動\)** の順にクリックします。

    ![既定の仮想デバイス イメージを使って Google Android Emulator を起動](hardware-acceleration-images/win/02-start-default-avd.png)

4. エミュレーターの起動中は **[Starting Android Emulator]\(Android エミュレーターの起動\)** ダイアログ ウィンドウを監視します。 HAXM がインストールされている場合は、次のスクリーンショットに示されているように、"**HAX is working and emulator runs in fast virt mode**" (HAX は動作しており、エミュレーターは高速仮想モードで実行されています) のメッセージが表示されます。

    ![HAXM が使用可能であることを示す [Starting Android Emulator]\(Android エミュレーターの起動\) ダイアログ](hardware-acceleration-images/win/03-haxm-detected.png)

   このメッセージが表示されない場合は、HAXM がインストールされていない可能性があります。 たとえば、HAXM が使用できない場合には、次のスクリーンショットのようなメッセージが表示される場合があります。

    ![HAXM はこのマシンでは使用できません](hardware-acceleration-images/win/04-haxm-error.png)

   コンピューターで HAXM を使用できない場合は、次のセクションの手順を使用して、HAXM をインストールします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[ツール]、[Google エミュレーター マネージャー]** の順にクリックして、Android エミュレーター マネージャーを起動します。

    [![Android エミュレーター マネージャーのメニュー項目の場所](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. 次のような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示される場合は、HAXM がまだインストールされていないか、コンピューター上で正しく構成されていません。

    ![HAXM の準備ができていないことを示す [Performance Warning]\(パフォーマンスに関する警告\) ダイアログ](hardware-acceleration-images/mac/04-avd-warning.png)

   このような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示された場合は、「[パフォーマンスに関する警告](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)」で原因を特定し、根本的な問題を解決します。

3. **x86** イメージ (**Android\_Accelerated\_x86** など) を選択し、**[Start]\(開始\)**、**[Launch]\(起動\)** の順にクリックします。

    [![既定の仮想デバイス イメージを使って Google Android Emulator を起動](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. エミュレーターの起動中は **[Starting Android Emulator]\(Android エミュレーターの起動\)** ダイアログ ウィンドウを監視します。 HAXM がインストールされている場合は、次のスクリーンショットに示されているように、"**HAX is working and emulator runs in fast virt mode**" (HAX は動作しており、エミュレーターは高速仮想モードで実行されています) のメッセージが表示されます。

    ![HAXM が使用可能であることを示す [Starting Android Emulator]\(Android エミュレーターの起動\) ダイアログ](hardware-acceleration-images/mac/03-haxm-detected.png)

   お使いのコンピューターで HAXM が利用できない場合 (たとえば、"_Please ensure Intel HAXM is propertly installed and usable_" (HAXM が正しくインストールされ使用できることを確認してください) のようなエラー メッセージが表示される場合)、次のセクションの手順を使用して HAXM をインストールします。


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>HAXM のインストール

エミュレーターが起動しない場合は、HAXM を手動でインストールする必要があります。 Windows 版と macOS 版の HAXM インストール パッケージは、どちらも「[Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager)」ページから入手できます。 次の手順に従って、HAXM をダウンロードして手動でインストールします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Intel の Web サイトから Windows 版の最新の [HAXM 仮想化エンジン](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/)のインストーラーをダウンロードします。 Intel の Web サイトから直接 HAXM インストーラーをダウンロードする利点は、確実に最新バージョンを使用できることです。

   または、SDK Manager を使用して HAXM インストーラーをダウンロードすることもできます (SDK Manager で **[Tools] > [Extras] > [Intel x86 Emulator Accelerator (HAXM installer)]** の順にクリックします)。 通常、Android SDK は、次の場所に HAXM インストーラーをダウンロードします。

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   SDK Manager では、HAXM はインストールされないことにご注意ください。上記の場所に HAXM インストーラーがダウンロードされるだけで、起動は手動で行う必要があります。

2. **intelhaxm android.exe** を実行して、HAXM インストーラーを起動します。 インストーラーのダイアログの既定値をそのまま使用します。

   ![Intel Hardware Accelerated Execution Manager のセットアップ ウィンドウ](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>ハードウェアの高速化と AMD の CPU

Google の Android エミュレーターは現在 [Linux でのみ](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies) AMD ハードウェア アクセラレーションをサポートしているため、Windows を実行している AMD ベースのコンピューターではハードウェア アクセラレーションを実行できません。


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Intel の Web サイトから macOS 版の最新の [HAXM 仮想化エンジン](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/)のインストーラーをダウンロードします。

2. HAXM インストーラーを実行します。 インストーラーのダイアログの既定値をそのまま使用します。

   [![Intel Hardware Accelerated Execution Manager のセットアップ ウィンドウ](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>関連リンク

* [Android Emulator でアプリを実行する](https://developer.android.com/studio/run/emulator)
