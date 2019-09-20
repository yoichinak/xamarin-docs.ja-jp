---
title: Windows VM から Mac 上で動作する Android エミュレーターに接続できますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: af32f4af3951eff3b8b5412908e35c4cdef09ae4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757266"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM から Mac 上で動作する Android エミュレーターに接続できますか

Windows 仮想マシンから Mac で実行されている Android Emulator に接続するには、次の手順に従います。

1. Mac でエミュレーターを起動します。

2. Mac 上`adb`のサーバーを強制終了します。

    ```bash
    adb kill-server
    ```

3. エミュレーターは、ループバックネットワークインターフェイスの2つの TCP ポートでリッスンしていることに注意してください。

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    奇数のポートは、に`adb`接続するために使用されるポートです。 「」 [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking)も参照してください。

4. _オプション 1_:使用[`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    ポート 5555 (またはその他の任意のポート) で外部に受信した受信 TCP パケットをループバックインターフェイスの奇数ポート (この例では**127.0.0.1 5555** ) に転送し、送信パケットを逆方向に転送するには、次のようにします。

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    コマンドが`nc`ターミナルウィンドウで実行されている限り、パケットは想定どおりに転送されます。 ターミナルウィンドウに「Control-C」と入力すると、 `nc`エミュレーターの使用が完了したらコマンドを終了できます。

    (特に、オプション1はオプション2よりも簡単です。特に、**システム環境設定 > セキュリティ & プライバシー > ファイアウォール**がオンになっている場合)。 

    _オプション 2_:使用[`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    [共有ネットワーク](http://kb.parallels.com/en/4948)インターフェイス上の`127.0.0.1:5555`ポート`5555` (またはその他の任意のポート) から TCP パケットをループバックインターフェイスの奇数ポート (この例では) にリダイレクトするには、次のようにします。

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    このコマンドは、 `pf packet filter`システムサービスを使用してポートフォワーディングを設定します。 改行は重要です。 コピーして貼り付ける場合は、必ずそのままにしておいてください。 また、 *vmnet8*を使用している場合は、インターフェイス名を調整する必要があります。 `vmnet8`は、VMWare Fusion の*共有ネットワーク*モード用の特殊な*NAT デバイス*の名前です。 多くの場合、 [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)には適切なネットワークインターフェイスがあります。

5. Windows コンピューターからエミュレーターに接続します。

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "Ip アドレス-mac" を Mac の IP アドレスに置き換え`ifconfig vmnet8 | grep 'inet '`ます。たとえば、「」を使用します。 必要に応じて`5555` 、を手順 4. の別のポートに置き換えます\. (注: へ`adb`のコマンドラインアクセスを取得する方法の1つとして、Visual Studio の android > android [**Adb コマンドプロンプト > ツール**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)を使用する方法があります)。

### <a name="alternate-technique-using-ssh"></a>を使用した別の手法`ssh`

Mac で_リモートログイン_を有効にした場合は、ポートフォワーディング`ssh`を使用してエミュレーターに接続できます。

1. Windows に SSH クライアントをインストールします。 1つの方法とし[て、Git For Windows](https://git-for-windows.github.io/)をインストールします。 コマンドは、 **Git Bash**コマンドプロンプトで使用できるようになります。 `ssh`

2. 上記の手順1-3 に従ってエミュレーターを起動し、 `adb` Mac でサーバーを強制終了して、エミュレーターのポートを特定します。

3. Windows `ssh`でを実行して、windows 上のローカルポート (`localhost:15555`この例では) と、Mac のループバックインターフェイスの奇数のエミュレーターポート (`127.0.0.1:5555`この例では) との間に双方向のポート転送を設定します。

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    は`mac-username` 、で`whoami`示されているように、Mac ユーザー名に置き換えます。 を`ip-address-of-the-mac` Mac の IP アドレスに置き換えます。

4. Windows のローカルポートを使用してエミュレーターに接続します。

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (注: へ`adb`のコマンドラインアクセスを取得する簡単な方法の1つとして、 [Visual Studio の android > android **Adb コマンドプロンプト > ツール**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)を使用する方法があります)。

注意点: ローカルポートにポート`5555`を使用すると、 `adb`エミュレーターは Windows でローカルに実行されていると考えられます。 これにより、Visual Studio では問題は発生しませんが Visual Studio for Mac では、起動直後にアプリが終了します。

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>を使用し`adb -H`た別の手法は、まだサポートされていません。

理論的には、の組み込み機能を使用`adb`して、リモートコンピューター上で実行さ`adb`れているサーバーに接続する方法もあり[https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)ます (たとえば、「」を参照してください)。
ただし、現在、Xamarin の IDE 拡張機能には、そのオプションを構成する方法が用意されていません。

## <a name="contact-information"></a>連絡先情報

このドキュメントでは、2016年3月時点の現在の動作について説明します。 このドキュメントで説明されている手法は、Xamarin の安定したテストスイートの一部ではないため、将来中断する可能性があります。

手法が動作しなくなった場合、またはドキュメントに他の誤りがあることがわかった場合は、次のフォーラムスレッド[http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm)のディスカッションに自由に追加してください。
ありがとうございます。
