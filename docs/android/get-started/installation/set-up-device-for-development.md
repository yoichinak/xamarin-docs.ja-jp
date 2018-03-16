---
title: "開発用のデバイスの設定"
description: "この記事では、Android デバイスを Xamarin.Android アプリケーションの実行とデバッグに使用できるように、デバイスの設定およびコンピューターへの接続方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 64036af82ea49ad4d758a89767ff0da02eef094f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="set-up-device-for-development"></a>開発用のデバイスの設定

_この記事では、Android デバイスを Xamarin.Android アプリケーションの実行とデバッグに使用できるように、デバイスの設定およびコンピューターへの接続方法について説明します。_

今頃は、おそらく Android エミュレーターで優れた新しいアプリケーションを実行しており、そろそろピカピカの Android デバイスで実行したい頃だと思います。 以下の手順は、デバッグのためのコンピューターへのデバイスの接続に関するものです。

1.  **デバイスでのデバッグを有効にする** - 既定では、Android デバイスでアプリケーションをデバッグすることはできません。

2.  **USB ドライバーをインストールする** - この手順は、OS X コンピューターでは必要ありません。 Windows コンピューターでは、USB ドライバーのインストールが必要になる場合があります。

3.  **コンピューターにデバイスを接続する** - 最後の手順は、USB または WiFi を介したコンピューターへのデバイスの接続に関するものです。

各手順については、以下のセクションで詳しく説明します。


## <a name="enable-debugging-on-the-device"></a>デバイスでのデバッグを有効にする

任意の Android デバイスを使用して、Android アプリケーションをテストすることができます。 ただし、デバッグを行う前に、デバイスを正しく構成する必要があります。 関係する手順は、デバイスで実行されている Android のバージョンによって多少異なります。


### <a name="android-40-to-android-41"></a>Android 4.0 から Android 4.1

Android 4.0.x から Android 4.1.x の場合は、以下の手順に従ってデバッグを有効にします。

1.  **[設定]** 画面に移動します。
2.  **[開発者向けオプション]** を選択します。
3.  **[USB デバッグ]** オプションをオフにします。

このスクリーン ショットは、Android 4.0.3 を実行しているデバイスの **[開発者向けオプション]** 画面を示しています。

[![開発者向けオプション](set-up-device-for-development-images/developer-options-sml.png)](set-up-device-for-development-images/developer-options.png#lightbox)


### <a name="android-42-and-higher"></a>Android 4.2 以降

Android 4.2 以降では、**[開発者向けオプション]** は既定で非表示になります。 これを使用可能にするには、**[設定]、[端末情報]** の順に移動し、**[ビルド番号]** 項目を 7 回タップして **[開発者向けオプション]** タブを表示します。

[![ビルド番号項目](set-up-device-for-development-images/about-phone-sml.png)](set-up-device-for-development-images/about-phone.png#lightbox)

**[設定]、[システム]** の **[開発者向けオプション]** タブが使用可能になったら、それを開いて開発者向け設定を表示します。

[![開発者向け設定画面](set-up-device-for-development-images/developer3.png)](set-up-device-for-development-images/developer3.png#lightbox)

ここで、USB デバッグやスリープ モードにしないなどの開発者向けオプションを有効にすることができます。


## <a name="install-usb-drivers"></a>USB ドライバーをインストールする

この手順は OS X では必要ありません。デバイスを USB ケーブルで Mac に接続するだけです。

Windows コンピューターが USB で接続された Android デバイスを認識する前に、いくつかの追加のドライバーをインストールする必要がある場合があります。

> [!NOTE]
> これらは、Google Nexus デバイスの設定手順であり、参照として提供されるものです。 特定のデバイスの手順は異なる場合がありますが、同様のパターンに従います。 何かお困りでしたら、インターネットで使用しているデバイスを検索してください。

**[Android SDK のインストール パス]\tools** ディレクトリの **android.bat** アプリケーションを実行します。 既定では、Xamarin.Android インストーラーは、Windows コンピューター上の次の場所に Android SDK を配置します。

    C:\Users\[username]\AppData\Local\Android\android-sdk


### <a name="download-the-usb-drivers"></a>USB ドライバーをダウンロードする

Google Nexus デバイス (Galaxy Nexus を除く) では、Google USB ドライバーが必要です。 Galaxy Nexus 用のドライバーは [Samsung によって配布](http://www.samsung.com/us/support/downloads/)されます。
他のすべての Android デバイスでは、[それぞれの製造元の USB ドライバー](http://developer.android.com/tools/extras/oem-usb.html#Drivers)を使用する必要があります。

Android SDK マネージャーを起動し、以下のスクリーン ショットに示されているように、**Extras** フォルダーを展開して、**Google USB ドライバー** パッケージをインストールします。

[![選択されている Google USB ドライバー パッケージ](set-up-device-for-development-images/usbdriverpackage.png)](set-up-device-for-development-images/usbdriverpackage.png#lightbox)

**[Google USB Driver]\(Google USB ドライバー\)** ボックスをオンにして、**[インストール]** ボタンをクリックします。
次の場所にドライバー ファイルがダウンロードされます。

    [Android SDK install path]\extras\google\usb\_driver

Xamarin.Android インストールの既定のパスは次のとおりです。

    C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver



### <a name="installing-the-usb-driver"></a>USB ドライバーのインストール

USB ドライバーがダウンロードされたら、それらをインストールする必要があります。
Windows 7 でドライバーをインストールする場合は、次のようにします。

1.  USB ケーブルでデバイスをコンピューターに接続します。

2.  デスクトップまたは Windows エクスプローラーでコンピューターを右クリックして、**[管理]** を選択します。

3.  左側のウィンドウで **[デバイス]** を選択します。

4.  右側のウィンドウで **[その他のデバイス]** を見つけて展開します。

5.  デバイス名を右クリックして、**[ドライバー ソフトウェアの更新]** を選択します。
    これで、ハードウェアの更新ウィザードが起動します。

6.  **[コンピューターを参照してドライバー ソフトウェアを検索します**] を選択して、**[次へ]** をクリックします。

7.  **[参照]** をクリックして、USB ドライバー フォルダーを見つけます (Google USB ドライバーは **[Android SDK のインストール パス]\extras\google\usb_driver** にあります)。

8.  **[次へ]** をクリックして、ドライバーをインストールします。


### <a name="installing-unverified-drivers-in-windows-8"></a>Windows 8 での未確認ドライバーのインストール

Windows で未確認のドライバーをインストールするには、追加の手順が必要になる場合があります。
8. 次の手順では、Galaxy Nexus のドライバーのインストール方法について説明します。

1.  **Windows 8 の詳細ブート オプションにアクセスする** - この手順は、詳細ブート オプションにアクセスするためのコンピューターの再起動に関するものです。 コマンド ライン プロンプトを起動し、次のコマンドを使用して、コンピューターを再起動します。

        shutdown.exe /r /o

2.  **デバイスを接続する** - デバイスをコンピューターに接続します。

3.  **デバイス マネージャーを起動する** - **devmgmt.msc** を実行します。黄色の三角形が付いたデバイスがリストされます。

4.  **デバイス ドライバーをインストールする** - 前述のようにデバイス ドライバーをインストールします。



## <a name="connect-the-device-to-the-computer"></a>デバイスをコンピューターに接続する

最後の手順では、デバイスをコンピューターに接続します。 これには、以下の 2 つの方法があります。

-   **USB ケーブル** - これは最も簡単で一般的な方法です。 USB ケーブルをデバイスに接続してからコンピューターに接続するだけです。

-   **WiFi** - USB ケーブルを使用せずに、WiFi 経由で Android デバイスをコンピューターに接続することができます。 この方法は少し手間がかかりますが、USB ケーブルがない場合や、デバイスが離れた場所にあるため USB ケーブルで接続できない場合に便利です。 WiFi 経由の接続については、次のセクションで説明します。


### <a name="connecting-over-wifi"></a>WiFi 経由の接続

既定では、[Android Debug Bridge](http://developer.android.com/tools/help/adb.html) (*ADB*) は USB 経由で Android デバイスと通信するように構成されます。 USB の代わりに TCP/IP を使用するように再構成することができます。 これを行うには、デバイスとコンピューターの両方が同じ WiFi ネットワーク上にある必要があります。 WiFi 経由でデバッグを行うために環境を設定するには、コマンド ラインを使用して以下の手順を実行します。

1.  Android デバイスの IP アドレスを決定します。 IP アドレスを見つける 1 つの方法は、**[設定]、[Wi-Fi]** で確認し、デバイスが接続されている WiFi ネットワークをタップすることです。 これで、設定画面が表示され、以下のスクリーン ショットのような、ネットワーク接続に関する情報が示されます。

    ![IP アドレス](set-up-device-for-development-images/ip-settings.png)

    一部のバージョンの Android では、IP アドレスはリストされませんが、代わりに **[設定]、[端末情報]、[Status]\(ステータス\)** で確認できます。

2.  USB 経由で、コンピューターに Android デバイスを接続します。

3.  次に、ADB を再起動して、ポート 5555 で TCP を使用するようにします。 コマンド プロンプトで、次のコマンドを入力します。

        adb tcpip 5555

    このコマンドを実行すると、コンピューターは USB 経由で接続されているデバイスをリッスンできなくなります。

4.  デバイスをコンピューターに接続している USB ケーブルを外します。

5.  上記の手順 1 で指定されたポートで Android デバイスに接続するように ADB を構成します。

        adb connect 192.168.1.28:5555

    このコマンドが完了すると、Android デバイスは WiFi 経由でコンピューターに接続されます。

WiFi 経由でのデバッグが完了したら、以下のコマンドを使用して、ADB をリセットして USB モードに戻すことができます。

    adb usb

コンピューターに接続されているデバイスをリストするように ADB に要求できます。 デバイスの接続方法に関係なく、コマンド プロンプトで次のコマンドを実行することで、接続されているデバイスを表示できます。

    adb devices


## <a name="summary"></a>まとめ

この記事では、デバイスでのデバッグを有効にして、開発用に Android デバイスを構成する方法について説明しました。 USB または WiFi を使用して、コンピューターにデバイスを接続する方法についても説明しました。


## <a name="related-links"></a>関連リンク

- [Android Debug Bridge](http://developer.android.com/tools/help/adb.html)
- [ハードウェア デバイスの使用](http://developer.android.com/tools/device.html)
- [Samsung ドライバーのダウンロード](http://www.samsung.com/us/support/downloads/)
- [OEM USB ドライバー](http://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google USB ドライバー](http://developer.android.com/sdk/win-usb.html)
- [XDA 開発者: Windows 8 - ADB/高速ブート ドライバーの問題が解決されました](http://forum.xda-developers.com/showthread.php?t=1583801)
