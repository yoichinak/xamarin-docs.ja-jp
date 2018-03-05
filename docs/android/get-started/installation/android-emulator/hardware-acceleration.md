---
title: "Android Emulator ハードウェアの高速化"
description: "Android SDK エミュレーターのパフォーマンスを最大にするためにコンピューターを準備する方法"
ms.topic: article
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: 53dc85cab94bdf692e088d7c6eea6916d283ba84
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Android Emulator ハードウェアの高速化

Android SDK エミュレーターはハードウェアの高速化なしでは非常に低速なため、Android SDK エミュレーターのパフォーマンスを劇的に改善する方法として、Intel の HAXM (Hardware Accelerated Execution Manager) が推奨されます。

<a name="haxm-overview" />

## <a name="haxm-overview"></a>HAXM の概要

HAXM は、Intel Virtualization Technology (VT) を使用してホスト コンピューター上の Android アプリのエミュレーションを高速化するハードウェア依存の仮想化エンジン (ハイパーバイザー) です。 Intel が提供する Android x86 エミュレーター イメージと公式の Android SDK Manager を組み合わせることで、HAXM は VT 対応のシステムで Android のエミュレーションを高速化することができます。 VT 機能を備えている Intel CPU を搭載したコンピューターで開発している場合は、HAXM を利用して Android SDK エミュレーターを大幅に高速化できます (ご使用の CPU が VT をサポートしているかどうかがわからない場合は、「[使用中のプロセッサーはインテル® バーチャライゼーション・テクノロジーに対応していますか?](https://www.intel.com/content/www/us/en/support/processors/000005486.html)」を参照してください)。

Android SDK エミュレーターは、HAXM が使用可能であれば、自動的にそれを使用します。 (「[構成および使用](~/android/deploy-test/debugging/android-sdk-emulator/index.md)」で説明されているように) **x86** ベースの仮想デバイスを選択すると、その仮想デバイスはハードウェアの高速化に HAXM を使用します。 初めて Android SDK エミュレーターを使用する場合は、事前に HAXM がインストールされ、Android SDK エミュレーターを使用できることを確認することをお勧めします。

> [!NOTE]
> **注:** HAXM を仮想マシンで実行することはできません。

<a name="verify-haxm" />

## <a name="verifying-haxm-installation"></a>HAXM インストールの確認

エミュレーターの起動時に、**[Starting Android Emulator]\(Android エミュレーターの起動\)** ウィンドウを表示して HAXM が使用可能かどうかを確認することができます。 Android SDK エミュレーターを起動するには、次の操作を行います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **[ツール]、[Android]、[Android エミュレーター マネージャー]** の順にクリックして、Android エミュレーター マネージャーを起動します。

    [![Android エミュレーター マネージャーのメニュー項目の場所](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png)

2. 次のような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示される場合は、HAXM がまだインストールされていないか、コンピューター上で正しく構成されていません。

    ![HAXM の準備ができていないことを示す [Performance Warning]\(パフォーマンスに関する警告\) ダイアログ](hardware-acceleration-images/win/11-perf-warn.png)

   このような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示された場合は、「[パフォーマンスに関する警告](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)」で原因を特定し、根本的な問題を解決します。

3. **x86** イメージ (**VisualStudio\_android 23\_x86\_phone** など) を選択し、**[Start]\(開始\)**、**[Launch]\(起動\)** の順にクリックします。

    ![既定の仮想デバイス イメージを使って Android SDK エミュレーターを起動](hardware-acceleration-images/win/02-start-default-avd.png)

4. エミュレーターの起動中は **[Starting Android Emulator]\(Android エミュレーターの起動\)** ダイアログ ウィンドウを監視します。 HAXM がインストールされている場合は、次のスクリーンショットに示されているように、"**HAX is working and emulator runs in fast virt mode**" (HAX は動作しており、エミュレーターは高速仮想モードで実行されています) のメッセージが表示されます。

    ![HAXM が使用可能であることを示す [Starting Android Emulator]\(Android エミュレーターの起動\) ダイアログ](hardware-acceleration-images/win/03-haxm-detected.png)

   このメッセージが表示されない場合は、HAXM がインストールされていない可能性があります。 たとえば、HAXM が使用できない場合には、次のスクリーンショットのようなメッセージが表示される場合があります。

    ![HAXM はこのマシンでは使用できません](hardware-acceleration-images/win/04-haxm-error.png)

   コンピューターで HAXM を使用できない場合は、次のセクションの手順を使用して、HAXM をインストールします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[ツール]、[Google エミュレーター マネージャー]** の順にクリックして、Android エミュレーター マネージャーを起動します。

    [![Android エミュレーター マネージャーのメニュー項目の場所](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png)

2. 次のような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示される場合は、HAXM がまだインストールされていないか、コンピューター上で正しく構成されていません。

    ![HAXM の準備ができていないことを示す [Performance Warning]\(パフォーマンスに関する警告\) ダイアログ](hardware-acceleration-images/mac/04-avd-warning.png)

   このような **[Performance Warning]\(パフォーマンスに関する警告\)** ダイアログが表示された場合は、「[パフォーマンスに関する警告](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn)」で原因を特定し、根本的な問題を解決します。

3. **x86** イメージ (**Android\_Accelerated\_x86** など) を選択し、**[Start]\(開始\)**、**[Launch]\(起動\)** の順にクリックします。

    [![既定の仮想デバイス イメージを使って Android SDK エミュレーターを起動](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png)

3. エミュレーターの起動中は **[Starting Android Emulator]\(Android エミュレーターの起動\)** ダイアログ ウィンドウを監視します。 HAXM がインストールされている場合は、次のスクリーンショットに示されているように、"**HAX is working and emulator runs in fast virt mode**" (HAX は動作しており、エミュレーターは高速仮想モードで実行されています) のメッセージが表示されます。

    ![HAXM が使用可能であることを示す [Starting Android Emulator]\(Android エミュレーターの起動\) ダイアログ](hardware-acceleration-images/mac/03-haxm-detected.png)

   お使いのコンピューターで HAXM が利用できない場合 (たとえば、"_Please ensure Intel HAXM is propertly installed and usable_" (HAXM が正しくインストールされ使用できることを確認してください) のようなエラー メッセージが表示される場合)、次のセクションの手順を使用して HAXM をインストールします。


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>HAXM のインストール

エミュレーターが起動しない場合は、HAXM を手動でインストールする必要があります。 Windows 版と macOS 版の HAXM インストール パッケージは、どちらも「[Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager)」ページから入手できます。 次の手順に従って、HAXM をダウンロードして手動でインストールします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Intel の Web サイトから Windows 版の最新の [HAXM 仮想化エンジン](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/)のインストーラーをダウンロードします。 Intel の Web サイトから直接 HAXM インストーラーをダウンロードする利点は、確実に最新バージョンを使用できることです。

   または、SDK Manager を使用して HAXM インストーラーをダウンロードすることもできます (SDK Manager で **[Tools] > [Extras] > [Intel x86 Emulator Accelerator (HAXM installer)]** の順にクリックします)。 通常、Android SDK は、次の場所に HAXM インストーラーをダウンロードします。

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   SDK Manager では、HAXM はインストールされないことにご注意ください。上記の場所に HAXM インストーラーがダウンロードされるだけで、起動は手動で行う必要があります。

2. **intelhaxm android.exe** を実行して、HAXM インストーラーを起動します。 インストーラーのダイアログの既定値をそのまま使用します。

   ![Intel Hardware Accelerated Execution Manager のセットアップ ウィンドウ](hardware-acceleration-images/win/05-haxm-installer.png)

次のエラー ダイアログ ("_This computer does not support Intel Virtualization Technology (VT-x) or it is being exclusively used by Hyper-V_" (コンピューターは Intel Virtualization Technology (VT-x) をサポートしていないか、HYPER-V によって排他的に使用されています)) が表示された場合は、HAXM をインストールする前に HYPER-V を無効にする必要があります。

![HYPER-V の競合により HAXM がインストールできない](hardware-acceleration-images/win/06-cant-install-haxm.png)

次のセクションで、HYPER-V を無効にする方法について説明します。

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Hyper-V を無効にする

HYPER-V が有効になっている Windows を使用している場合、HAXM をインストールして使用するには、HYPER-V を無効にしてコンピューターを再起動する必要があります。 次の手順に従って、コントロール パネルから HYPER-V を無効にできます。

1. Windows 検索ボックスに「**プログラムと**」と入力し、検索結果の **[プログラムと機能]** をクリックします。

2. コントロール パネルの **[プログラムと機能]** ダイアログで、**[Windows の機能の有効化または無効化]** をクリックします。

    ![Windows の機能の有効化または無効化](hardware-acceleration-images/win/07-turn-windows-features.png)

3. **[Hyper-V]** のチェック ボックスをオフにして、コンピューターを再起動します。

    ![[Windows の機能] ダイアログで [HYPER-V] を無効にする](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

または、次の PowerShell コマンドレットを使用して HYPER-V を無効にすることもできます。

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM および Microsoft HYPER-V の両方を同時にアクティブ化することはできません。 残念ながら、コンピューターを再起動せずに HYPER-V と HAXM を切り替える方法は今のところありません。 [Visual Studio Emulator for Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (Hyper-V に依存) を使用する場合、再起動しないと Android SDK エミュレーターが使用できません。 HYPER-V と HAXM の両方を使用する 1 つの方法は、「[Creating a no hypervisor boot entry](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/)」 (ハイパーバイザーのないブート エントリを作成する) で説明されているように、複数のブート設定を作成することです。

Device Guard と Credential Guard が有効になっていると、上記の手順で HYPER-V を無効にできない場合があります。 HYPER-V を無効にできない場合 (または無効になっているように見えるが、HAXM のインストールがまだ失敗する場合) は、次のセクションの手順に従って、Device Guard と Credential Guard を無効にします。

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Device Guard の無効化

Device Guard と Credential Guard により、Windows コンピューター上で Hyper-V を無効にできない場合があります。 これは多くの場合、所有している組織によって構成され、制御されている、ドメインに参加しているマシンの問題です。
Windows 10 で、次の手順を使用して、**Device Guard** が実行されているかどうかを確認します。

1. **Windows Search** で、「**システム情報**」と入力し、**システム情報**アプリを起動します。

2. **[システムの概要]** で、**[Device Guard 仮想化ベースのセキュリティ]** があり、**実行**状態になっているかどうかを確認します。

   [![Device Guard があり、実行されている](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png)

Device Guard が有効になっている場合は、次の手順に従って無効にします。

1. 前のセクションで説明したように、(**[Windows の機能の有効化または無効化**] の下で) **HYPER-V** が無効になっていることを確認します。

2. Windows 検索ボックスで、「**gpedit**」を入力し、検索結果で **[グループ ポリシーの編集]** を選択します。 これにより、**ローカル グループ ポリシー エディター**が起動します。

3. **ローカル グループ ポリシー エディター**で、**[コンピューターの構成]、[管理用テンプレート]、[システム]、[Device Guard]** の順に移動します。

   [![ローカル グループ ポリシー エディターでの Device Guard](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png)

4. (上記のように) **[仮想化ベースのセキュリティを有効にする]** を **[無効]** に変更し、**ローカル グループ ポリシー エディター**を終了します。

5. Windows 検索ボックスに「**cmd**」を入力します。 検索結果に **[コマンド プロンプト]** が表示されたら、**[コマンド プロンプト]** を右クリックして **[管理者として実行]** を選択します。

6. 次のコマンドをコピーしてコマンド プロンプト ウィンドウに貼り付けます (ドライブ **Z:** が使用中の場合は、代わりに使用する未使用のドライブ文字を選択してください)。

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. コンピューターを再起動する 起動画面に、次のようなプロンプトが表示されます。

   **Do you want to disable Credential Guard?** (Credential Guard を無効にしますか)

   指示に従って指定されたキーを押して Credential Guard を無効にします。

8. コンピューターが再起動したら、(前の手順で説明したように) HYPER-V が無効になっていることを確認します。

HYPER-V がまだ無効になっていない場合は、ドメインに参加しているコンピューターのポリシーによって Device Guard または Credential Guard を無効にできないようになっている可能性があります。 この場合、Credential Guard を無効化できるように適用除外をドメイン管理者に依頼することができます。 または、HAXM を使用するためにドメインに参加していないコンピューターを使用することができます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Intel の Web サイトから macOS 版の最新の [HAXM 仮想化エンジン](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/)のインストーラーをダウンロードします。

2. HAXM インストーラーを実行します。 インストーラーのダイアログの既定値をそのまま使用します。

   [![Intel Hardware Accelerated Execution Manager のセットアップ ウィンドウ](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png)

-----
