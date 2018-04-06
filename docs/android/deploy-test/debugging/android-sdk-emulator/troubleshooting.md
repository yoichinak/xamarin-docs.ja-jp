---
title: Android SDK エミュレーターのトラブルシューティング
description: Android SDK エミュレーターの問題を特定および解決する方法
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b3e55e02d27307dbcef8b6a62b2da368cd0201f3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Android SDK エミュレーターのトラブルシューティング

この記事では、Android SDK エミュレーター (およびそのソリューション) で発生することが多い警告メッセージと問題について説明します。
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>パフォーマンスに関する警告

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2017 バージョン 15.4 以降では、Android SDK エミュレーターにアプリを初めて展開したときにパフォーマンスに関する警告ダイアログが表示される場合があります。 これらの警告ダイアログについて以下に説明します。

### <a name="computer-does-not-contain-an-intel-procesor"></a>コンピューターが Intel プロセッサを備えていません

![コンピューターが Intel プロセッサを備えていません](troubleshooting-images/01-no-intel-processor.png)

このダイアログが表示された場合は、Android SDK エミュレーターのアクセラレータとして必要な Intel プロセッサがコンピューターに搭載されていません。 コンピューターが Intel プロセッサを備えていない場合は、物理的な Android デバイスを使用して開発することをお勧めします。

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V がインストールされているか、アクティブになっています

![Hyper-V がインストールされているか、アクティブになっています](troubleshooting-images/02-hyper-v-active.png)

このダイアログが表示された場合は、Hyper-V がインストールされているか、アクティブになっているので無効にする必要があります。 この問題を解決する方法については、「[Disabling Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv)」 (Hyper-V を無効にする) を参照してください。 

### <a name="haxm-is-not-installed"></a>HAXM がインストールされていません

![HAXM がインストールされていません](troubleshooting-images/03-haxm-not-installed.png)

このダイアログが表示された場合、コンピューターは Intel プロセッサを備えており、Hyper-V が無効になっていますが、HAXM がインストールされていません。
HAXM のインストール手順については、「[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)」 (HAXM をインストールする) を参照してください。

### <a name="haxm-process-not-running"></a>HAXM プロセスが実行されていません

![HAXM プロセスが実行されていません](troubleshooting-images/04-haxm-process-not-running.png)

コンピューターは Intel プロセッサを備えており、Hyper-V は無効になっており、Intel HAXM もインストールされているが、HAXM プロセスが実行されていない場合に、このダイアログが表示されます。 この問題を解決するには、コマンド プロンプト ウィンドウを開き、次のコマンドを入力します。

```cmd
sc query intelhaxm
```

HAXM プロセスが実行されている場合は、次のような出力が表示されます。

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


**[STATE]\(状態)** が **[RUNNING]\(実行中\)** に設定されていない場合は、「[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)」 (Intel Hardware Accelerated Execution Manager の使用方法) を参照して問題を解決してください。


### <a name="other-failures"></a>その他のエラー

![その他のエラー](troubleshooting-images/05-other-failure.png)

コンピューターは Intel プロセッサを備えており、Hyper-V は無効になっており、Intel HAXM はインストールされており、HAXM プロセスも実行中であるが、何か不明な理由によりエミュレーターが起動しない場合に、このダイアログが表示されます。
このエラーを解決するには、「[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)」 (Intel Hardware Accelerated Execution Manager の使用方法) を参照してください。

### <a name="disabling-performance-warnings"></a>パフォーマンスに関する警告を無効にする

パフォーマンスに関する警告を表示したくない場合は、それを無効にすることができます。 Visual Studio で、**[ツール]、[オプション]、[Xamarin]、[Android 設定]** の順にクリックして、**[AVD アクセラレータがサポートされていない場合に警告する (HAXM)]** オプションを無効にします。

[![AVD アクセラレーションに関する警告を無効にする](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac build 7.2 (ビルド 559) では、Android SDK エミュレーターにアプリを最初に展開した場合にパフォーマンスに関する警告ダイアログが表示されます。 これらの警告ダイアログについて以下に説明します。

### <a name="haxm-is-not-installed"></a>HAXM がインストールされていません

![HAXM がインストールされていません](troubleshooting-images/03-haxm-not-installed.png)

このダイアログは、HAXM がインストールされていないことを示します。
HAXM のインストール手順については、「[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)」 (HAXM をインストールする) を参照してください。

### <a name="haxm-process-not-running"></a>HAXM プロセスが実行されていません

![HAXM プロセスが実行されていません](troubleshooting-images/04-haxm-process-not-running.png)

このダイアログは、HAXM プロセスが実行されていない場合に表示されます。 この問題の解決に役立つ詳細については、「[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)」 (Intel Hardware Accelerated Execution Manager の使用方法) を参照してください。

### <a name="other-failures"></a>その他のエラー

![その他のエラー](troubleshooting-images/05-other-failure.png)

このダイアログは、何か不明な理由により、エミュレーターが起動できない場合に表示されます。 このエラーを解決するには、「[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)」 (Intel Hardware Accelerated Execution Manager の使用方法) を参照して問題を解決してください。

-----


## <a name="solutions-to-common-problems"></a>一般的な問題の解決策

Android SDK エミュレーターの一般的な問題の多くは、コンピューターの構成に変更を加えるか、または他のソフトウェアをインストールすることによって解決できます。 次のセクションでは、該当する問題と、解決策を説明します。


### <a name="deployment-issues"></a>展開に関する問題

エミュレーターでの APK のインストールの失敗、または Android Debug Bridge (**adb**) の実行の失敗に関するエラーが表示された場合は、Android SDK がエミュレーターに接続できることを確認してください。 そのためには、次の手順に従います。

1. **Android 仮想デバイス (AVD) マネージャー** からエミュレーターを起動します (仮想デバイスを選択し、**[開始]** をクリックします)。

2. コマンド プロンプトを開き、**adb** がインストールされているフォルダーに移動します。 たとえば、Windows の場合は、**C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe** となります。

3. 次のコマンドを入力します。

   ```shell
   adb devices
   ```

4. Android SDK からエミュレーターにアクセスできる場合は、接続されているデバイスの一覧にエミュレーターが表示されます。 例:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. この一覧にエミュレーターが表示されない場合は、**Android SDK マネージャー**を起動し、すべての更新プログラムを適用し、エミュレーターをもう一度起動してみてください。



### <a name="haxm-issues"></a>HAXM の問題

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android SDK エミュレーターが正常に起動しない場合、通常は HAXM に問題があります。 HAXM の問題の多くは、その他の仮想化テクノロジとの競合、間違った設定、または HAXM ドライバーの期限切れが原因で発生します。

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM 仮想化の競合

HAXM が、仮想化を使用する他のテクノロジ (Hyper-V、Windows Device Guard、ウイルス対策ソフトウェア) と競合している可能性があります。

- **Hyper-V** &ndash;Hyper-V を有効にした状態で Windows を使用している場合は、「[Disabling Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv)」 (Hyper-V を無効にする) の手順を実行してください。

- **Device Guard** &ndash;Device Guard と Credential Guard により、Windows コンピューター上で Hyper-V を無効にできない可能性があります。 Device Guard と Credential Guard を無効にするには、「[Disabling Device Guard](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard)」 (Device Guard を無効にする) を参照してください。

- **ウイルス対策ソフトウェア** &ndash; (Avast など) ハードウェア補助による仮想化を使用するウイルス対策ソフトウェアを実行している場合は、そのソフトウェアを無効にするかアンインストールしてから、Android SDK エミュレーターを再起動してみてください。


#### <a name="incorrect-bios-settings"></a>不適切な BIOS 設定

Windows PC 上で HAXM を使用する場合、BIOS 内で仮想化テクノロジ (Intel VT-x 対応) が有効になっていない限り、HAXM は機能しません。 VT-x が無効になっている場合に、Android SDK エミュレーターの起動を試みると、次のようなエラーが表示されます。

**このコンピューターは HAXM の要件を満たしていますが、Intel 仮想化テクノロジ (VT-x) がオンになっていません。**

このエラーを修正するには、コンピューターを BIOS で起動し、VT-x と SLAT (第 2 レベルのアドレス変換) の両方を有効にし、Windows でコンピューターを再起動します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android SDK エミュレーターが正常に起動しない場合、通常は HAXM に問題があります。 HAXM の問題の多くは、その他の仮想化テクノロジとの競合、間違った設定、または HAXM ドライバーの期限切れが原因で発生します。 「[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)」 (HAXM をインストールする) に詳述された手順に従って、HAXM ドライバーを再インストールしてみてください。

-----
