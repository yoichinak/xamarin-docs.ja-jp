---
title: Windows VM から Mac 上で動作する Android エミュレーターに接続できますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 35bfdb92ccfffe54f0ca10dc001d8919703a5bd8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153361"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM から Mac 上で動作する Android エミュレーターに接続できますか

Windows 仮想マシンから、Mac で実行されている Android エミュレーターに接続するには、次の手順を使用します。

1.  Mac でエミュレーターを起動します。

2.  強制終了、 `adb` Mac 上のサーバー。

    ```bash
    adb kill-server
    ```

3.  エミュレーターがネットワークのループバック インターフェイスに 2 つの TCP ポートでリッスンしていることに注意してください。

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    奇数のポートに接続するために使用する 1 つは、`adb`します。 参照してください[ https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking)します。

4.  _オプション 1_:使用 [`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    受信 TCP パケットを転送する受信した外部ポート 5555 で (またはなどの他の任意のポート) のループバック インターフェイス上のポートを奇数に (**127.0.0.1 5555**この例では)、送信パケットを転送するには、再びの他の方法。

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    限り、`nc`コマンドが常に実行されているターミナル ウィンドウで、期待どおりにパケットが転送されます。 終了するターミナル ウィンドウでコントロール C と入力することができます、`nc`コマンドを完了したら、エミュレーターを使用します。

    (オプション 1 が、通常は、オプション 2 の場合より簡単場合に特に**システム環境設定 > プライバシーおよびセキュリティ > ファイアウォール**がオンにします)。 

    _オプション 2_:使用 [`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    ポートから TCP パケットをリダイレクトする`5555`(またはたいその他の任意のポート) で、[共有ネットワーク](http://kb.parallels.com/en/4948)のループバック インターフェイスに奇数ポートへのインターフェイス (`127.0.0.1:5555`この例では)。

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    このコマンドはポートを使用して転送を設定、`pf packet filter`システム サービスです。 改行が重要です。 コピー、貼り付け時にそのまま保持することを確認します。 インターフェイス名を調整する必要がありますも*vmnet8* Parallels を使用している場合。 `vmnet8` 特殊な名前を指定します*NAT デバイス*の*共有ネットワーク*VMWare Fusion のモード。 Parallels で適切なネットワーク インターフェイスは可能性の高い[vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)します。

5.  Windows マシンからエミュレーターに接続します。

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "Ip アドレス-の-、-mac"、Mac の IP アドレスを持つとしてによって一覧表示などを置き換える`ifconfig vmnet8 | grep 'inet '`します。 必要な場合、置換`5555`手順 4 から、その他のポート\. (注: コマンド ライン アクセスする方法の 1 つ`adb`経由では、 [**ツール > Android > Android Adb コマンド プロンプト**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) Visual Studio で)。

### <a name="alternate-technique-using-ssh"></a>別の手法を使用して `ssh`

有効にした場合_リモート ログイン_、Mac 上で使用できます`ssh`ポート エミュレーターへの接続に転送します。

1.  Windows で SSH クライアントをインストールします。 1 つのオプションは、インストールする[Git for Windows](https://git-for-windows.github.io/)します。 `ssh`コマンドで使用できますし、 **Git Bash**コマンド プロンプト。

2.  上記の 1 ~ 3 は、エミュレーターを起動し、強制終了する手順、 `adb` Mac 上のサーバーとエミュレーターのポートを特定します。

3.  実行`ssh`Windows 上のローカル ポート間の双方向のポート フォワーディングをセットアップする Windows の (`localhost:15555`この例では) および Mac のループバック インターフェイス上のエミュレーターの奇数ポート (`127.0.0.1:5555`この例では)。

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    置換`mac-username`で一覧表示される、Mac のユーザー名と`whoami`します。 置換`ip-address-of-the-mac`mac の IP アドレス

4.  Windows 上のローカル ポートを使用してエミュレーターに接続します。

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (注: 簡単な方法にコマンド ライン アクセス`adb`経由では、 [**ツール > Android > Android Adb コマンド プロンプト**Visual Studio で](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat))。

ちょっとした注意点: ポートを使用する場合`5555`ローカル ポートの`adb`Windows でエミュレーターがローカルで実行されていることでしょう。 Visual Studio で、問題を引き起こすこれが、Visual Studio for Mac で、アプリケーションの起動後すぐに終了が発生します。

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>代替手法を使用して`adb -H`はまだサポートされていません

理論上は、使用する別の方法があります`adb`の組み込みの機能への接続に、`adb`リモート コンピューターで実行しているサーバー (例を参照してください[ https://stackoverflow.com/a/18551325 ](https://stackoverflow.com/a/18551325))。
Xamarin.Android の IDE 拡張機能はそのオプションを構成する方法を現在提供していません。

## <a name="contact-information"></a>連絡先情報

このドキュメントでは、2016 年 3 月の時点では、現在の動作について説明します。 このドキュメントで説明した手法は、Xamarin の安定したテスト スイートの一部ではないが、今後中断ため。

この手法は機能しなくなります、ことを確認する場合、またはドキュメントの他の誤りが発生した場合は、次のフォーラム スレッドでのディスカッションに追加してみてください: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm)します。
ありがとうございます。

