---
title: Android Emulator のトラブルシューティング
description: この記事では、Android Emulator の使用中に発生する可能性のある問題を診断して回避する方法について説明します。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/27/2018
ms.openlocfilehash: 421d51cbb1ae3adb80aef6e4bf3cf1da38d6de8e
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887823"
---
# <a name="android-emulator-troubleshooting"></a>Android Emulator のトラブルシューティング

_この記事では、Android Emulator の構成および実行中に発生する、最も一般的な警告メッセージと問題について説明します。さらに、これらのエラーを解決するための解決策と、エミュレーターの問題の診断に役立つさまざまなトラブルシューティングのヒントについて説明します。_

::: zone pivot="windows"

## <a name="deployment-issues-on-windows"></a>Windows での展開に関する問題

アプリを展開するときに、エミュレーターによっていくつかのエラー メッセージが表示される場合があります。 ここでは、最も一般的なエラーと解決策について説明します。

### <a name="deployment-errors"></a>展開エラー

エミュレーターでの APK のインストールの失敗、または Android Debug Bridge (**adb**) の実行の失敗に関するエラーが表示された場合は、Android SDK がエミュレーターに接続できることを確認してください。 エミュレーターの接続を確認するには、次の手順を実行します。

1. **Android Device Manager** からエミュレーターを起動します (仮想デバイスを選択し、 **[開始]** をクリックします)。

2. コマンド プロンプトを開き、**adb** がインストールされているフォルダーに移動します。 既定の場所に Android SDK がインストールされている場合、**adb** は **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe** にあります。そうでない場合は、このパスをお使いのコンピューター上にある Android SDK の場所に変更します。

3. 次のコマンドを入力します。

   ```shell
   adb devices
   ```

4. Android SDK からエミュレーターにアクセスできる場合は、接続されているデバイスの一覧にエミュレーターが表示されます。 次に例を示します。

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. この一覧にエミュレーターが表示されない場合は、**Android SDK マネージャー**を起動し、すべての更新プログラムを適用し、エミュレーターをもう一度起動してみてください。


### <a name="mmio-access-error"></a>MMIO アクセス エラー

「**MMIO アクセス エラーが発生しました**」というメッセージが表示されたら、エミュレーターを再起動します。


<a name="gps-win" />

## <a name="missing-google-play-services"></a>Google Play 開発者サービスが見つからない

この状態は、エミュレーターで実行している仮想デバイスに Google Play 開発者サービス、または Google Play ストアがインストールされていない場合に、これらのパッケージを含めずに仮想デバイスを作成することでしばしば発生します。 仮想デバイスを作成する (「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照) ときに、次のオプションのいずれかまたは両方を必ず選択してください。

- **Google API** &ndash; 仮想デバイスに Google Play 開発者サービスを含めます。
- **Google Play ストア** &ndash; 仮想デバイスに Google Play ストアを含めます。

たとえば、この仮想デバイスには、Google Play 開発者サービスと Google Play ストアが含まれます。

[![[Google API] と [Google Play ストア] が有効化された AVD の例](troubleshooting-images/win/00-add-gps-w158-sml.png)](troubleshooting-images/win/00-add-gps-w158.png#lightbox)

> [!NOTE]
> Google Play ストアのイメージは、Pixel、Pixel 2、Nexus 5、Nexus 5 X などの一部の基本デバイスの種類でのみ使用できます。


<a name="perf-win" />

## <a name="performance-issues"></a>パフォーマンスの問題

パフォーマンスの問題は、通常、次の問題のいずれかが原因で発生します。

- ハードウェア アクセラレーションなしでエミュレーターを実行している。

- エミュレーターで実行されている仮想デバイスが、x86 ベースのシステム イメージを使用していない。

以降のセクションでは、これらのシナリオについて詳しく説明します。

### <a name="hardware-acceleration-is-not-enabled"></a>ハードウェア アクセラレーションが有効化されていない

ハードウェア アクセラレーションが有効化されていない場合、デバイス マネージャーから仮想デバイスを起動すると、Windows ハイパーバイザー プラットフォーム (WHPX) が正しく構成されていないことを示すエラー メッセージが表示されたダイアログが生成されます。

![デバイス マネージャーの警告の例](troubleshooting-images/win/01-dev-mgr-warning-w158.png)

このエラー メッセージが表示された場合は、ハードウェア アクセラレーションを確認して有効化する手順について、後述の「[ハードウェア アクセラレーションの問題](#accel-issues-win)」を参照してください。


### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>アクセラレーションは有効化されているが、エミュレーターの実行が遅すぎる 

この問題の一般的な原因は、仮想デバイス (AVD) で x86 ベースのイメージが使用されていないことです。 仮想デバイスを作成する (「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照) ときに、必ず x86 ベースのイメージを選択してください。

[![仮想デバイスに x86 ベースのイメージを選択している](troubleshooting-images/win/02-x86-virtual-device-w158-sml.png)](troubleshooting-images/win/02-x86-virtual-device-w158.png#lightbox)


<a name="accel-issues-win" />

## <a name="hardware-acceleration-issues"></a>ハードウェア アクセラレーションの問題

ハードウェア アクセラレーションに Hyper-V または HAXM を使用しているかどうかに関わらず、構成の問題やコンピューター上の他のソフトウェアとの競合が発生する場合があります。 ハードウェア アクセラレーションが有効になっていること (およびどのアクセラレーション メソッドをエミュレーターが使用しているか) を確認するには、コマンド プロンプトを開き、次のコマンドを入力します。

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator-check.exe" accel
```

このコマンドでは、Android SDK が既定の場所 (**C:\\Program Files (x86)\\Android\\android-sdk**) にインストールされていることを前提としています。そうでない場合は、上記のパスをご使用のコンピューター上にある Android SDK の場所に変更します。

### <a name="hardware-acceleration-not-available"></a>ハードウェア アクセラレーションが使用できない

Hyper-V が使用可能な場合、**emulator-check.exe accel** コマンドから次の例のようなメッセージが返されます。

```cmd
HAXM is not installed, but Windows Hypervisor Platform is available.
```

HAXM が使用可能な場合は、次の例のようなメッセージが返されます。

```cmd
HAXM version 6.2.1 (4) is installed and usable.
```

ハードウェア アクセラレーションが使用できない場合は、次の例のようなメッセージが表示されます (Hyper-V が見つからない場合、エミュレーターは HAXM を検索します)。

```cmd
HAXM is not installed on this machine
```

ハードウェア アクセラレーションが使用できない場合は、「[Accelerating with Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vswin#hyper-v-win)」 (Hyper-V による高速化) で、ご使用のコンピューターでハードウェア アクセラレーションを有効化する方法を確認してください。

### <a name="incorrect-bios-settings"></a>不適切な BIOS 設定

BIOS がハードウェア アクセラレーションをサポートするように適切に構成されていないと、**emulator-check.exe accel** コマンドを実行したときに、次の例のようなメッセージが表示されます。

```cmd
VT feature disabled in BIOS/UEFI
```

この問題を修正するには、コンピューターを再起動して BIOS に入り、次のオプションを有効にします。

- 仮想化テクノロジ (マザーボードの製造元によって、ラベルが異なる場合があります)。
- ハードウェアによるデータ実行防止。

ハードウェア アクセラレーションが有効になっていて、BIOS が正しく構成されている場合は、エミュレーターはハードウェア アクセラレーションを使用して正常に実行されるはずです。
ただし、次で説明するように、Hyper-V と HAXM に固有の問題により、問題が発生する可能性はまだあります。


### <a name="hyper-v-issues"></a>Hyper-V の問題

場合によっては、 **[Windows の機能の有効化または無効化]** ダイアログで **[Hyper-V]** と **[Windows Hypervisor Platform]** の両方を有効化すると、Hyper-V が正しく有効化されない場合があります。 Hyper-V が有効になっていることを確認するには、次の手順を実行します。

1. Windows の検索ボックスに「**powershell**」と入力します。

2. 検索結果で **[Windows PowerShell]** を右クリックし、 **[管理者として実行]** を選択します。

3. PowerShell コンソールで、次のコマンドを入力します。

    ```powershell
    Get-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online
    ```

    Hyper-V が有効になっていない場合は、Hyper-V の状態が **Disabled** (無効) であることを示す次の例のようなメッセージが表示されます。

    ```
    FeatureName      : Microsoft-Hyper-V-All
    DisplayName      : Hyper-V
    Description      : Provides services and management tools for creating and running virtual machines and their resources.
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

4. PowerShell コンソールで、次のコマンドを入力します。

    ```powershell
    Get-WindowsOptionalFeature -FeatureName HypervisorPlatform -Online
    ```

    ハイパーバイザーが有効になっていない場合は、HypervisorPlatform の状態が **Disabled** (無効) であることを示す次の例のようなメッセージが表示されます。

    ```
    FeatureName      : HypervisorPlatform
    DisplayName      : Windows Hypervisor Platform
    Description      : Enables virtualization software to run on the Windows hypervisor
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

Hyper-V および HypervisorPlatform のいずれかまたは両方が有効になっていない場合は、次の PowerShell コマンドを使用して有効にします。

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
```

これらのコマンドが完了したら、再起動します。 

Hyper-V を有効化する (Deployment Image Servicing and Management ツールを使用して Hyper-V を有効化する手法を含む) 詳細については、[Hyper-V のインストール](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)に関するページを参照してください。


### <a name="haxm-issues"></a>HAXM の問題

HAXM の問題は、その他の仮想化テクノロジとの競合、間違った設定、または HAXM ドライバーの期限切れが原因で発生することがよくあります。

### <a name="haxm-process-is-not-running"></a>HAXM プロセスが実行されていません

HAXM がインストールされている場合は、コマンド プロンプトを開き、次のコマンドを入力することで、HAXM プロセスが実行されていることを確認できます。

```cmd
sc query intelhaxm
```

HAXM プロセスが実行されている場合は、次の結果と同じような出力が表示されます。

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

`STATE` が `RUNNING` に設定されていない場合は、「[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator)」(Intel Hardware Accelerated Execution Manager の使用方法) を参照して問題を解決してください。


<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM 仮想化の競合

HAXM が、仮想化を使用する他のテクノロジ (Hyper-V、Windows Device Guard、ウイルス対策ソフトウェア) と競合している可能性があります。

- **Hyper-V** &ndash; **Windows 10 April 2018 Update (ビルド 1803)** より前のバージョンの Windows を使用しており、Hyper-V が有効になっている場合は、HAXM を有効化できるように、「[Hyper-V を無効にする](#disable-hyperv)」の手順に従ってください。

- **Device Guard** &ndash;Device Guard と Credential Guard により、Windows コンピューター上で Hyper-V を無効にできない可能性があります。 Device Guard と Credential Guard を無効にするには、「[Disabling Device Guard](#disable-devguard)」 (Device Guard を無効にする) を参照してください。

- **ウイルス対策ソフトウェア** &ndash; (Avast など) ハードウェア補助による仮想化を使用するウイルス対策ソフトウェアを実行している場合は、そのソフトウェアを無効にするかアンインストールしてから、Android Emulator を再起動してみてください。


#### <a name="incorrect-bios-settings"></a>不適切な BIOS 設定

Windows PC 上で HAXM を使用する場合、BIOS 内で仮想化テクノロジ (Intel VT-x 対応) が有効になっていない限り、HAXM は機能しません。 VT-x が無効になっている場合に、Android Emulator の起動を試みると、次のようなエラーが表示されます。

**このコンピューターは HAXM の要件を満たしていますが、Intel 仮想化テクノロジ (VT-x) がオンになっていません。**

このエラーを修正するには、コンピューターを再起動して BIOS に入り、VT-x と SLAT (第 2 レベルのアドレス変換) の両方を有効にしてから、コンピューターを再起動して再度 Windows に入ります。

<a name="disable-hyperv" />

#### <a name="disabling-hyper-v"></a>Hyper-V を無効にする

**Windows 10 April 2018 Update (ビルド 1803)** より前のバージョンの Windows を使用しており、Hyper-V が有効になっている場合、HAXM をインストールして使用するには Hyper-V を無効にしてコンピューターを再起動する必要があります。 **Windows 10 April 2018 Update (ビルド 1803)** 以降を使用している場合は、Android Emulator のバージョン 27.2.7 以降で (HAXM ではなく) Hyper-V を使用してハードウェアを高速化できるため、Hyper-V を無効にする必要はありません。

次の手順に従って、コントロール パネルから HYPER-V を無効にできます。

1. Windows 検索ボックスに「**Windows の機能**」を入力し、検索結果で **[Windows の機能の有効化または無効化]** を選択します。

2. **[Hyper-V]** をオフにします。

    ![[Windows の機能] ダイアログで [HYPER-V] を無効にする](troubleshooting-images/win/03-uncheck-hyper-v.png)

3. コンピューターを再起動します。

または、次の PowerShell コマンドを使用して Hyper-V ハイパーバイザーを無効にすることもできます。

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM および Microsoft HYPER-V の両方を同時にアクティブ化することはできません。 残念ながら、コンピューターを再起動せずに Hyper-V と HAXM を切り替える方法はありません。 

Device Guard と Credential Guard が有効になっていると、上記の手順で HYPER-V を無効にできない場合があります。 HYPER-V を無効にできない場合 (または無効になっているように見えるが、HAXM のインストールがまだ失敗する場合) は、次のセクションの手順に従って、Device Guard と Credential Guard を無効にします。

<a name="disable-devguard" />

#### <a name="disabling-device-guard"></a>Device Guard の無効化

Device Guard と Credential Guard により、Windows コンピューター上で Hyper-V を無効にできない場合があります。 この状況は多くの場合、所有している組織によって構成され、制御されている、ドメインに参加しているマシンの問題です。 Windows 10 で、次の手順を使用して、**Device Guard** が実行されているかどうかを確認します。

1. Windows 検索ボックスに「**システム情報**」と入力し、検索結果で **[システム情報]** を選択します。

2. **[システムの概要]** で、 **[Device Guard 仮想化ベースのセキュリティ]** があり、**実行**状態になっているかどうかを確認します。

   [![Device Guard があり、実行されている](troubleshooting-images/win/04-device-guard-sml.png)](troubleshooting-images/win/04-device-guard.png#lightbox)

Device Guard が有効になっている場合は、次の手順に従って無効にします。

1. 前のセクションで説明したように、( **[Windows の機能の有効化または無効化**] の下で) **HYPER-V** が無効になっていることを確認します。

2. Windows 検索ボックスで、「**gpedit**」を入力し、検索結果で **[グループ ポリシーの編集]** を選択します。 この手順により**ローカル グループ ポリシー エディター**が起動します。

3. **ローカル グループ ポリシー エディター**で、 **[コンピューターの構成]、[管理用テンプレート]、[システム]、[Device Guard]** の順に移動します。

   [![ローカル グループ ポリシー エディターでの Device Guard](troubleshooting-images/win/05-group-policy-editor-sml.png)](troubleshooting-images/win/05-group-policy-editor.png#lightbox)

4. (上記のように) **[仮想化ベースのセキュリティを有効にする]** を **[無効]** に変更し、**ローカル グループ ポリシー エディター**を終了します。

5. Windows 検索ボックスに「**cmd**」を入力します。 検索結果に **[コマンド プロンプト]** が表示されたら、 **[コマンド プロンプト]** を右クリックして **[管理者として実行]** を選択します。

6. 次のコマンドをコピーしてコマンド プロンプト ウィンドウに貼り付けます (ドライブ **Z:** が使用中の場合は、代わりに使用する未使用のドライブ文字を選択してください)。

    ```cmd
    mountvol Z: /s
    copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
    bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
    bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
    mountvol Z: /d
    ```

7. コンピューターを再起動する 起動画面に、次のようなメッセージのプロンプトが表示されます。

   **Do you want to disable Credential Guard?** (Credential Guard を無効にしますか)

   指示に従って指定されたキーを押して Credential Guard を無効にします。

8. コンピューターが再起動したら、(前の手順で説明したように) HYPER-V が無効になっていることを確認します。

HYPER-V がまだ無効になっていない場合は、ドメインに参加しているコンピューターのポリシーによって Device Guard または Credential Guard を無効にできないようになっている可能性があります。 この場合、Credential Guard を無効化できるように適用除外をドメイン管理者に依頼することができます。 または、HAXM を使用する必要がある場合は、ドメインに参加していないコンピューターを使用することができます。


## <a name="additional-troubleshooting-tips"></a>その他のトラブルシューティングのヒント

次の推奨事項は多くの場合、Android Emulator の問題の診断に役立ちます。

### <a name="starting-the-emulator-from-the-command-line"></a>コマンド ラインから Android Emulator を起動する

エミュレーターがまだ実行されていない場合は、コマンド ラインから (Visual Studio 内からではなく) 起動してその出力を表示できます。 通常、Android Emulator AVD イメージは、次の場所に格納されます (*ユーザー名*をご自分の Windows ユーザー名に置き換えます)。

**C:\\Users\\*ユーザー名*\\.android\\avd**

AVD のフォルダー名を渡すことで、この場所から AVD イメージを使用してエミュレーターを起動できます。 たとえば、次のコマンドは **Pixel_API_27** という名前の AVD を起動します。

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator.exe" -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_API_27 -prop monodroid.avdname=Pixel_API_27
```

この例では、Android SDK が既定の場所 (**C:\\Program Files (x86)\\Android\\android-sdk**) にインストールされていることを前提としています。そうでない場合は、上記のパスをご使用のコンピューター上にある Android SDK の場所に変更します。

このコマンドを実行すると、エミュレーターの起動中に多くの行の出力が生成されます。 具体的には、ハードウェア アクセラレーションが有効になっていて正常に機能している場合 (この例では、ハードウェア アクセラレーションに HAXM が使用されています) は、次の例のような行が出力されます。

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: HAXM version 6.2.1 (4) is installed and usable.
```

### <a name="viewing-device-manager-logs"></a>デバイス マネージャー ログを表示する

多くの場合、デバイス マネージャー ログを表示して、エミュレーターの問題を診断できます。 これらのログは、次の場所に書き込まれます。

**C:\\Users\\*ユーザー名*\\AppData\\Roaming\\XamarinDeviceManager**

各 **DeviceManager.log** ファイルは、メモ帳などのテキスト エディターを使用して表示できます。 次のログ エントリの例は、コンピューターで HAXM が見つからなかったことを示しています。

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```



::: zone-end
::: zone pivot="macos"

## <a name="deployment-issues-on-macos"></a>macOS での展開に関する問題

アプリを展開するときに、エミュレーターによっていくつかのエラー メッセージが表示される場合があります。 以下で、最も一般的なエラーと解決策について説明します。

### <a name="deployment-errors"></a>展開エラー

エミュレーターでの APK のインストールの失敗、または Android Debug Bridge (**adb**) の実行の失敗に関するエラーが表示された場合は、Android SDK がエミュレーターに接続できることを確認してください。 接続を確認するには、次の手順を実行します。

1. **Android Device Manager** からエミュレーターを起動します (仮想デバイスを選択し、 **[開始]** をクリックします)。

2. コマンド プロンプトを開き、**adb** がインストールされているフォルダーに移動します。 既定の場所に Android SDK がインストールされている場合、**adb** は **~/Library/Developer/Xamarin/android-sdk-macosx/platform-tools/adb** にあります。そうでない場合は、このパスをご使用のコンピューター上にある Android SDK の場所に変更します。

3. 次のコマンドを入力します。

   ```shell
   adb devices
   ```

4. Android SDK からエミュレーターにアクセスできる場合は、接続されているデバイスの一覧にエミュレーターが表示されます。 次に例を示します。

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. この一覧にエミュレーターが表示されない場合は、**Android SDK マネージャー**を起動し、すべての更新プログラムを適用し、エミュレーターをもう一度起動してみてください。


### <a name="mmio-access-error"></a>MMIO アクセス エラー

「**MMIO アクセス エラーが発生しました**」が表示されたら、エミュレーターを再起動します。

<a name="gps-mac" />

## <a name="missing-google-play-services"></a>Google Play 開発者サービスが見つからない

この状態は通常、エミュレーターで実行している仮想デバイスに Google Play 開発者サービス、または Google Play ストアがインストールされていない場合に、これらのパッケージを含めずに仮想デバイスを作成することで発生します。 仮想デバイスを作成する (「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照) ときに、次のいずれかまたは両方を必ず選択してください。

- **Google API** &ndash; 仮想デバイスに Google Play 開発者サービスを含めます。
- **Google Play ストア** &ndash; 仮想デバイスに Google Play ストアを含めます。

たとえば、この仮想デバイスには、Google Play 開発者サービスと Google Play ストアが含まれます。

[![[Google API] と [Google Play ストア] が有効化された AVD の例](troubleshooting-images/mac/01-google-play-services-m75-sml.png)](troubleshooting-images/mac/01-google-play-services-m75.png#lightbox)

> [!NOTE]
> Google Play ストアのイメージは、Pixel、Pixel 2、Nexus 5、Nexus 5 X などの一部の基本デバイスの種類でのみ使用できます。


<a name="perf-mac" />

## <a name="performance-issues"></a>パフォーマンスの問題

パフォーマンスの問題は、通常、次の問題のいずれかが原因で発生します。

- ハードウェア アクセラレーションなしでエミュレーターを実行している。

- エミュレーターで実行されている仮想デバイスが、x86 ベースのシステム イメージを使用していない。

以降のセクションでは、これらのシナリオについて詳しく説明します。

### <a name="hardware-acceleration-is-not-enabled"></a>ハードウェア アクセラレーションが有効化されていない

ハードウェア アクセラレーションが有効化されていない場合、アプリを Android Emulator に展開するときに、「**デバイスはアクセラレータなしで実行されます**」といったメッセージがポップアップ表示されることがあります。 ご使用のコンピューターでハードウェア アクセラレーションが有効化されているかどうかわからない場合 (またはどのテクノロジがアクセラレーションを提供しているかを確認したい場合) は、ハードウェア アクセラレーションを確認して有効化する手順について、後述の「[ハードウェア アクセラレーションの問題](#accel-issues-mac)」を参照してください。


### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>アクセラレーションは有効化されているが、エミュレーターの実行が遅すぎる 

この問題の一般的な原因は、仮想デバイスで x86 ベースのイメージを使用していないことです。 仮想デバイスを作成する (「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照) ときに、必ず x86 ベースのイメージを選択してください。

[![仮想デバイスに x86 ベースのイメージを選択している](troubleshooting-images/mac/02-x86-virtual-device-m75-sml.png)](troubleshooting-images/mac/02-x86-virtual-device-m75.png#lightbox)

<a name="accel-issues-mac" />

## <a name="hardware-acceleration-issues"></a>ハードウェア アクセラレーションの問題

エミュレーターのハードウェア アクセラレーションに Hypervisor Framework または HAXM を使用しているかどうかに関わらず、インストールの問題または macOS の古いバージョンが原因の問題が発生する場合があります。 以降のセクションが、この問題を解決するのに役立ちます。

<a name="hypervisor-issues" />

### <a name="hypervisor-framework-issues"></a>Hypervisor Framework の問題

新しい Mac で macOS 10.10 以降を使用している場合、Android Emulator は自動的にハードウェア アクセラレーションに Hypervisor Framework を使用します。 ただし、一部の従来の Mac または macOS 10.10 より前のバージョンを実行している Mac では、Hypervisor Framework を提供していない場合があります。

ご使用の Mac が Hypervisor Framework をサポートしているかどうかを確認するには、ターミナルを開き、次のコマンドを入力します。

```bash
sysctl kern.hv_support
```

ご使用の Mac で Hypervisor Framework がサポートされている場合は、上記のコマンドで次の結果が返されます。

```bash
kern.hv_support: 1
```

ご使用の Mac で Hypervisor Framework が使用できない場合は、[HAXM を使用したアクセラレーション](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#haxm-mac)に関するセクションの手順に従って、代わりに HAXM をアクセラレーションに使用することができます。


### <a name="haxm-issues"></a>HAXM の問題

Android Emulator が正常に起動しない場合、その原因の多くは HAXM の問題です。 HAXM の問題は、その他の仮想化テクノロジとの競合、間違った設定、または HAXM ドライバーの期限切れが原因で発生することがよくあります。 「[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#install-haxm-mac)」 (HAXM をインストールする) に詳述された手順に従って、HAXM ドライバーを再インストールしてみてください。


## <a name="additional-troubleshooting-tips"></a>その他のトラブルシューティングのヒント

次の推奨事項は多くの場合、Android Emulator の問題の診断に役立ちます。

### <a name="starting-the-emulator-from-the-command-line"></a>コマンド ラインから Android Emulator を起動する

エミュレーターがまだ実行されていない場合は、コマンドラインから (Visual Studio for Mac 内からではなく) 起動してその出力を表示できます。 通常、Android Emulator AVD イメージは、次の場所に格納されます。

**~/.android/avd**

AVD のフォルダー名を渡すことで、この場所から AVD イメージを使用してエミュレーターを起動できます。 たとえば、次のコマンドは **Pixel_2_API_28** という名前の AVD を起動します。

```cmd
~/Library/Developer/Xamarin/android-sdk-macosx/emulator/emulator -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_2_API_28 -prop monodroid.avdname=Pixel_2_API_28
```

既定の場所に Android SDK がインストールされている場合、エミュレーターは **~/Library/Developer/Xamarin/android-sdk-macosx/emulator** にあります。そうでない場合は、このパスをご使用の Mac 上にある Android SDK の場所に変更します。

このコマンドを実行すると、エミュレーターの起動中に多くの行の出力が生成されます。 具体的には、ハードウェア アクセラレーションが有効になっていて正常に機能している場合 (この例では、ハードウェア アクセラレーションに Hypervisor Framework が使用されています) は、次の例のような行が出力されます。

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: Hypervisor.Framework OS X Version 10.13
```

### <a name="viewing-device-manager-logs"></a>デバイス マネージャー ログを表示する

多くの場合、デバイス マネージャー ログを表示して、エミュレーターの問題を診断できます。 これらのログは、次の場所に書き込まれます。

**~/Library/Logs/XamarinDeviceManager**

各 **Android Devices.log** ファイルを表示するには、コンソール アプリでダブルクリックして開きます。 次のログ エントリの例は、HAXM が見つからなかったことを示しています。

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end
