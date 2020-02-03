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
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725079"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM から Mac 上で動作する Android エミュレーターに接続できますか

Windows 仮想マシンから Mac で実行されている Android Emulator に接続するには、次の手順に従います。

1. Mac でエミュレーターを起動します。

2. Mac 上の `adb` サーバーを強制終了します。

    ```bash
    adb kill-server
    ```

3. エミュレーターは、ループバックネットワークインターフェイスの2つの TCP ポートでリッスンしていることに注意してください。

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    奇数のポートは、`adb`に接続するために使用されるポートです。 「 [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking)」も参照してください。

4. _オプション 1_: `nc` を使用して、ポート 5555 (または任意の他の任意のポート) で外部から受信した受信 TCP パケットをループバックインターフェイスの奇数ポート (この例では**127.0.0.1 5555** ) に転送し、他の方法で送信パケットを転送します。

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    `nc` コマンドがターミナルウィンドウで実行されている限り、パケットは想定どおりに転送されます。 ターミナルウィンドウに「Control-C」と入力すると、エミュレーターの使用が完了したら `nc` コマンドを終了できます。

    (特に、オプション1はオプション2よりも簡単です。特に、**システム環境設定 > セキュリティ & プライバシー > ファイアウォール**がオンになっている場合)。

    _オプション 2_: `pfctl` を使用して、[共有ネットワーク](https://kb.parallels.com/en/4948)インターフェイス上のポート `5555` (またはその他の任意のポート) からの TCP パケットをループバックインターフェイスの奇数ポート (この例では`127.0.0.1:5555`) にリダイレクトします。

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    このコマンドは、`pf packet filter` システムサービスを使用してポートフォワーディングを設定します。 改行は重要です。 コピーして貼り付ける場合は、必ずそのままにしておいてください。 また、 *vmnet8*を使用している場合は、インターフェイス名を調整する必要があります。 `vmnet8` は、VMWare Fusion の*共有ネットワーク*モード用の特殊な*NAT デバイス*の名前です。 多くの場合、 [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)には適切なネットワークインターフェイスがあります。

5. Windows コンピューターからエミュレーターに接続します。

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "Ip アドレス-mac" を Mac の IP アドレスに置き換えます。たとえば、`ifconfig vmnet8 | grep 'inet '`によって表示されます。 必要に応じて、`5555` を手順 4. の他のポートに置き換え\. (注: `adb` へのコマンドラインアクセスを取得する方法の1つは、Visual Studio で[**android > Android Adb コマンドプロンプト > ツール**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)を使用する方法です)。

### <a name="alternate-technique-using-ssh"></a>`ssh` を使用した別の手法

Mac で_リモートログイン_を有効にした場合は、`ssh` ポート転送を使用してエミュレーターに接続できます。

1. Windows に SSH クライアントをインストールします。 1つの方法とし[て、Git For Windows](https://git-for-windows.github.io/)をインストールします。 `ssh` コマンドは、 **Git Bash**コマンドプロンプトで使用できるようになります。

2. 上記の手順1-3 に従ってエミュレーターを起動し、Mac で `adb` サーバーを終了して、エミュレーターのポートを特定します。

3. Windows で `ssh` を実行して、Windows 上のローカルポート (この例では`localhost:15555`) と、Mac のループバックインターフェイス (この例では`127.0.0.1:5555`) の奇数のエミュレーターポートとの間に双方向のポート転送を設定します。

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    `mac-username` を、`whoami`で一覧表示されている Mac ユーザー名に置き換えます。 `ip-address-of-the-mac` を Mac の IP アドレスに置き換えます。

4. Windows のローカルポートを使用してエミュレーターに接続します。

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (注: `adb` へのコマンドラインアクセスを取得する簡単な方法の1つとして、 [Visual Studio で**Android > Android Adb コマンドプロンプト >** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)使用する方法があります)。

注意点: ローカルポートにポート `5555` を使用すると、エミュレーターが Windows でローカルに実行されていることが `adb` になります。 これにより、Visual Studio では問題は発生しませんが Visual Studio for Mac では、起動直後にアプリが終了します。

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>`adb -H` を使用した別の手法はまだサポートされていません

理論的には、別の方法として、`adb`の組み込み機能を使用して、リモートコンピューター上で実行されている `adb` サーバーに接続することもできます (例[https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325))。
ただし、現在、Xamarin の IDE 拡張機能には、そのオプションを構成する方法が用意されていません。

## <a name="contact-information"></a>連絡先情報

このドキュメントでは、2016年3月時点の現在の動作について説明します。 このドキュメントで説明されている手法は、Xamarin の安定したテストスイートの一部ではないため、将来中断する可能性があります。

この手法が動作しなくなった場合、またはドキュメントにその他の誤りがある場合は、フォーラムスレッドの[http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm)に関する説明を自由に追加してください。
ありがとうございます。
