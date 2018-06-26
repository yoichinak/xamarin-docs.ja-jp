---
title: Android エミュレーターに接続することから実行している Mac で Windows 仮想マシンですか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: f94c0966dd67940fc201df84a311db422d77b542
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935202"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Android エミュレーターに接続することから実行している Mac で Windows 仮想マシンですか。

Windows 仮想マシンから、Mac で実行されている Android エミュレーターに接続するには、次の手順を使用します。

1.  Mac でエミュレーターを起動します。

2.  Kill、 `adb` Mac 上のサーバー。

    ```bash
    adb kill-server
    ```

3.  エミュレーターがループバック ネットワーク インターフェイス上の 2 つの TCP ポートでリッスンしていることに注意してください。

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    奇数ポートへの接続に使用されるの`adb`します。 関連項目[ http://developer.android.com/tools/devices/emulator.html#emulatornetworking](http://developer.android.com/tools/devices/emulator.html#emulatornetworking)です。

4.  _オプション 1_: 使用[ `nc` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)受信 TCP パケットの転送を外部ポートで受信した 5555 (またはその他の任意のポート) に奇数ポートには、ループバック インターフェイス (**127.0.0.1 5555**この例では)、送信パケットを転送するには、バックアップの他の方法とします。

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    限り、`nc`コマンドは、ターミナル ウィンドウで実行されているまま、期待どおりのパケットが転送されます。 コントロール C を入力するには、ターミナル ウィンドウを終了する場合に、`nc`コマンドを完了したら、エミュレーターを使用します。

    (オプション 1 が、通常より簡単オプション 2 の場合に特に**システム環境設定 > セキュリティとプライバシー > ファイアウォール**オンになっています)。 

    _オプション 2_: 使用[ `pfctl` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)ポートからの TCP パケットにリダイレクトする`5555`(またはかその他の任意のポート) で、[共有ネットワーク](http://kb.parallels.com/en/4948)奇数ポートにインターフェイスをループバック インターフェイス (`127.0.0.1:5555`この例では)。

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    このコマンドは、ポートを使用して転送を設定、`pf packet filter`システム サービスです。 改行が重要です。 そのままそれらとコピー、貼り付けをしてください。 インターフェイスの名前を調整する必要がありますも*vmnet8* Parallels を使用している場合。 `vmnet8` 特別な名前を指定*NAT デバイス*の*共有ネットワーク*VMWare Fusion のモード。 Parallels 内にある適切なネットワーク インターフェイスは、可能性の高い[vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)です。

5.  Windows コンピューターからのエミュレーターに接続します。

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "Ip アドレス-の-、-mac"、Mac の IP アドレスを持つようなによって一覧表示を置き換える`ifconfig vmnet8 | grep 'inet '`です。 必要な場合は置き換えます`5555`手順 4 からと同様に、その他のポートに\. (注: をコマンドラインからアクセスする方法の 1 つ`adb`経由[**ツール > Android > Android Adb コマンド プロンプト**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) Visual Studio でします)。

### <a name="alternate-technique-using-ssh"></a>別の手法を使用します。 `ssh`

有効にした場合_リモート ログイン_mac で実行して`ssh`ポート エミュレーターへの接続に転送します。

1.  Windows 上の SSH クライアントをインストールします。 1 つのオプションのインストールを[Git for Windows](https://git-for-windows.github.io/)です。 `ssh`コマンドで使用できる、 **Git Bash**コマンド プロンプトです。

2.  手順は、するには、エミュレーターを起動、kill 上記の 1 ~ 3、 `adb` Mac 上のサーバーと、エミュレーターのポートを特定します。

3.  実行`ssh`on Windows のローカル ポートの間で双方向ポート フォワーディングをセットアップする Windows (`localhost:15555`この例では) および Mac のループバック インターフェイス上のエミュレーターの奇数ポート (`127.0.0.1:5555`この例では)。

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    置き換える`mac-username`Mac のユーザー名で一覧表示される`whoami`です。 置き換える`ip-address-of-the-mac`mac の IP アドレスを持つ

4.  Windows 上のローカル ポートを使用してエミュレーターに接続します。

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (注: する簡単な方法にアクセスするコマンドラインのいずれか`adb`経由[**ツール > Android > Android Adb コマンド プロンプト**Visual Studio で](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat))。

ちょっとした注意点: ポートを使用する場合`5555`、ローカル ポートに`adb`エミュレーターは Windows 上にローカルで実行されていることでしょう。 Visual Studio で、問題を引き起こすこれが、起動後すぐに終了するアプリを Visual Studio for Mac になります。

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>別の手法を使用して`adb -H`はまだサポートされていません

理論上は、別の方法としては使用すること`adb`の組み込み機能への接続に、`adb`をリモート コンピューターで実行しているサーバー (例を参照してください[ http://stackoverflow.com/a/18551325 ](http://stackoverflow.com/a/18551325))。
Xamarin.Android IDE の拡張機能はそのオプションを構成する方法提供されていません。

## <a name="contact-information"></a>連絡先情報

このドキュメントには、2016 年 3 月の時点で現在の動作について説明します。 このドキュメントで説明した手法は、Xamarin の安定したテスト スイートの一部ではないが、今後分割ため。

手法が動作しなくなった、発生した場合、またはドキュメント内の他の誤りがあることを確認する場合は、自由に、次のフォーラム スレッドに関する説明を追加: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm)です。
ありがとうございます。

