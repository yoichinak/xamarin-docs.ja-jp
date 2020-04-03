---
title: Windows VM から Mac 上で動作する Android エミュレーターに接続できますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 49d1eea60f766f4cb61484a6e441506cf8f046ff
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "76725079"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM から Mac 上で動作する Android エミュレーターに接続できますか

Windows 仮想マシンから Mac で実行されている Android Emulator に接続するには、次のステップに従います。

1. Mac でエミュレーターを起動します。

2. Mac 上の `adb` サーバーを強制終了します。

    ```bash
    adb kill-server
    ```

3. エミュレーターは、ループバック ネットワーク インターフェイスの 2 つの TCP ポートでリッスンしていることにご注意ください。

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    奇数のポートは、`adb` に接続するために使用されます。 [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking) も参照してください。

4. _オプション 1_:`nc` を使用して、ポート 5555 (またはその他の任意のポート) で外部に受信した受信 TCP パケットをループバック インターフェイスの奇数ポート (この例では **127.0.0.1 5555**) に転送し、送信パケットを逆方向に転送します。

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    `nc` コマンドがターミナル ウィンドウで実行されている限り、パケットは想定どおりに転送されます。 ターミナル ウィンドウに「Control-C」と入力すると、エミュレーターの使用が終わったときに `nc` コマンドを終了できます。

    (特に、 **[システム環境設定] > [セキュリティとプライバシー] > [ファイアウォール]** がオンになっている場合、オプション 1 は通常オプション 2 よりも簡単です。)

    _オプション 2_:`pfctl` を使用して、[共有ネットワーク](https://kb.parallels.com/en/4948) インターフェイス上のポート `5555` (またはその他の任意のポート) から、ループバック インターフェイスの奇数ポート (この例では`127.0.0.1:5555`) に TCP パケットをリダイレクトします。

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    このコマンドは、`pf packet filter` システム サービスを使用したポート フォワーディングを設定します。 改行は重要です。 コピーして貼り付ける場合は、必ずそのままにしておいてください。 また、Parallels を使用している場合は、"*vmnet8*" からインターフェイス名を調整する必要があります。 `vmnet8` は、VMWare Fusion の "*共有ネットワーク*" モード用の特別な "*NAT デバイス*" の名前です。 Parallels の適切なネットワーク インターフェイスとしては、[vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm) が考えられます。

5. Windows マシンからエミュレーターに接続します。

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "ip-address-of-the-mac" を `ifconfig vmnet8 | grep 'inet '` で表示されるような Mac の IP アドレスに置き換えます。 必要に応じて、`5555` をステップ 4 の任意のポートに置き換えます。 (注: `adb` へのコマンドライン アクセスを取得する方法の 1 つとして、Visual Studio の [ **[ツール] > [Android] > [Android Adb コマンド プロンプト]** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) を使用する方法があります。)

### <a name="alternate-technique-using-ssh"></a>`ssh` を使用した別の手法

Mac で "_リモート ログイン_" を有効にしている場合は、`ssh` ポート フォワーディングを使用してエミュレーターに接続できます。

1. Windows に SSH クライアントをインストールします。 1 つのオプションが [Git for Windows](https://git-for-windows.github.io/) をインストールするものです。 `ssh` コマンドが、**Git Bash** コマンド プロンプトで使用できるようになります。

2. 上記のステップ 1-3 に従ってエミュレーターを起動し、Mac で `adb` サーバーを強制終了して、エミュレーターのポートを特定します。

3. Windows で `ssh` を実行して、Windows 上のローカル ポート (この例では `localhost:15555`) と、Mac のループバック インターフェイスの奇数のエミュレーター ポート (この例では `127.0.0.1:5555`) との間に双方向のポート フォワーディングを設定します。

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    `mac-username` を、`whoami` で一覧表示されている Mac ユーザー名に置き換えます。 `ip-address-of-the-mac` を Mac の IP アドレスに置き換えます。

4. Windows のローカル ポートを使用してエミュレーターに接続します。

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (注: `adb` へのコマンドライン アクセスを取得する簡単な方法として、Visual Studio の [ **[ツール] > [Android] > [Android Adb コマンド プロンプト]** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) があります。)

注意: ローカル ポートにポート `5555` を使用すると、`adb` はエミュレーターが Windows でローカルに実行されていると認識します。 これにより、Visual Studio では問題は発生しませんが Visual Studio for Mac では、アプリが起動直後に終了します。

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>`adb -H` を使用した別の手法はまだサポートされていません

理論上は、別の方法として、`adb` の組み込み機能を使用してリモート マシン上で実行されている `adb` サーバーに接続する方法があります (例については、[https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325) を参照してください)。
ただし、現在、Xamarin.Android の IDE 拡張機能には、そのオプションを構成する方法が用意されていません。

## <a name="contact-information"></a>連絡先情報

このドキュメントでは、2016 年 3 月時点での現在の動作について説明されています。 このドキュメントで説明されている手法は、Xamarin の安定したテスト スイートの一部ではないため、将来は使用できない可能性があります。

手法が動作しない、またはドキュメントに誤りがあることに気づいた場合は、フォーラムのスレッド [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm) にディスカッションを追加してください。
ありがとうございます。
