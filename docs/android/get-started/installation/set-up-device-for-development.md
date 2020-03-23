---
title: 開発用のデバイスの設定
description: この記事では、Android デバイスを Xamarin.Android アプリケーションの実行とデバッグに使用できるように、デバイスの設定およびコンピューターへの接続方法について説明します。
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 72e0a2adc79796b3df7b6fb4eca62448f1a1a7a4
ms.sourcegitcommit: 997f7b6a1a1bc50b98c3ca5bbc75d6875ba2ae9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510732"
---
# <a name="set-up-device-for-development"></a>開発用のデバイスの設定

_この記事では、デバイスが Xamarin.Android アプリケーションの実行とデバッグに使用できるように、Android デバイスの設定およびコンピューターへの接続方法について説明します。_

Android エミュレーターでテストしたら、実際の Android デバイスでアプリを動かし、テストすることをお勧めします。 デバッグを有効にし、デバイスをコンピューターに接続する必要があります。

各手順については、以下のセクションで詳しく説明します。

## <a name="enable-debugging-on-the-device"></a>デバイスでのデバッグを有効にする

Android アプリケーションをテストするために、デバイスのデバッグを有効にする必要があります。 Android の [開発者向けオプション] は、バージョン 4.2 以降、既定では非表示になっており、これを有効にする方法は Android のバージョンによって異なる場合があります。

### <a name="android-90"></a>Android 9.0+

Android 9.0 以上では、以下のステップに従ってデバッグを有効にします。

1. **[設定]** 画面に移動します。
2. **[端末情報]** を選択します。
3. **[ビルド番号]** を 7 回をタップすると、 **[これでデベロッパーになりました!]** と表示されます。

### <a name="android-80-and-android-81"></a>Android 8.0 および Android 8.1

1. **[設定]** 画面に移動します。
2. **[システム]** を選択します。
3. **[端末情報]** を選択します。
4. **[ビルド番号]** を 7 回をタップすると、 **[これでデベロッパーになりました!]** と表示されます。

### <a name="android-71-and-lower"></a>Android 7.1 以前

1. **[設定]** 画面に移動します。
2. **[端末情報]** を選択します。
3. **[ビルド番号]** を 7 回をタップすると、 **[これでデベロッパーになりました!]** と表示されます。

[![Android 9.0 の [開発者向けオプション] 画面](set-up-device-for-development-images/build-version-sml.png)](set-up-device-for-development-images/build-version.png#lightbox)

### <a name="verify-that-usb-debugging-is-enabled"></a>USB デバッグが有効になっていることを確認する

デバイスで開発者モードを有効にしたら、デバイスで USB デバッグが有効になっていることを確認する必要があります。 これも、Android のバージョンによって異なります。

### <a name="android-90"></a>Android 9.0+

**[設定]、[システム]、[詳細設定] > [開発者向けオプション]** に移動して **[USB デバッグ]** を有効にします。

### <a name="android-80-and-android-81"></a>Android 8.0 および Android 8.1

**[設定]、[システム]、[開発者向けオプション]** に移動して **[USB デバッグ]** を有効にします。

### <a name="android-71-and-lower"></a>Android 7.1 以前

**[設定]、[開発者向けオプション]** に移動して **[USB デバッグ]** を有効にします。

**[設定]、[システム]** の **[開発者向けオプション]** タブが使用可能になったら、それを開いて開発者向け設定を表示します。

[![Android 9.0 の [開発者向けオプション] 画面](set-up-device-for-development-images/usb-debugging-sml.png)](set-up-device-for-development-images/usb-debugging.png#lightbox)

ここで、USB デバッグやスリープ モードにしないなどの開発者向けオプションを有効にすることができます。

## <a name="connect-the-device-to-the-computer"></a>デバイスをコンピューターに接続する

最後の手順では、デバイスをコンピューターに接続します。 最も簡単で信頼性の高い方法は、USB 経由で行う方法です。

以前にデバッグに使用していない場合は、デバイス上のコンピューターを信頼するように求めるメッセージが表示されます。 デバイスを接続するたびにこのメッセージが表示されないように、 **[Always allow from this computer]\(このコンピューターを常に許可する\)** をオンにすることもできます。

![](set-up-device-for-development-images/trust-computer-for-usb-debugging.png "Google USB")

## <a name="alternate-connection-via-wifi"></a>Wifi 経由の代替接続

USB ケーブルを使用せずに、WiFi 経由で Android デバイスをコンピューターに接続することができます。 この手法は少しばかり大変ですが、デバイスがコンピューターからかなり離れた位置にあり、ケーブルで常時接続することが難しい場合に便利です。 

### <a name="connecting-over-wifi"></a>WiFi 経由の接続

既定では、[Android Debug Bridge](https://developer.android.com/tools/help/adb.html) (*ADB*) は USB 経由で Android デバイスと通信するように構成されます。 USB の代わりに TCP/IP を使用するように再構成することができます。 これを行うには、デバイスとコンピューターの両方が同じ WiFi ネットワーク上にある必要があります。 WiFi 経由でデバッグを行うために環境を設定するには、コマンド ラインを使用して以下のステップを実行します。

1. Android デバイスの IP アドレスを決定します。 IP アドレスを見つける 1 つの方法は、 **[設定]、[ネットワークとインターネット]、[Wi-Fi]** で確認し、デバイスが接続されている WiFi ネットワークをタップしてから、 **[詳細設定]** をタップすることです。 これで、以下のスクリーン ショットのような、ネットワーク接続に関する情報を示すドロップダウンが開きます。

    [![IP アドレス](set-up-device-for-development-images/ip-settings-sml.png)](set-up-device-for-development-images/ip-settings.png#lightbox)

    一部のバージョンの Android では、IP アドレスはリストされませんが、代わりに **[設定]、[端末情報]、[ステータス]** の順に選択して確認できます。

2. USB 経由で、コンピューターに Android デバイスを接続します。

3. 次に、ADB を再起動して、ポート 5555 で TCP を使用するようにします。 コマンド プロンプトで、次のコマンドを入力します。

    ```command
    adb tcpip 5555
    ```

    このコマンドを実行すると、コンピューターは USB 経由で接続されているデバイスをリッスンできなくなります。

4. デバイスをコンピューターに接続している USB ケーブルを外します。

5. 上記の手順 1 で指定されたポートで Android デバイスに接続するように ADB を構成します。

    ```command
    adb connect 192.168.1.28:5555
    ```

    このコマンドが完了すると、Android デバイスは WiFi 経由でコンピューターに接続されます。

    WiFi 経由でのデバッグが完了したら、以下のコマンドを使用して、ADB をリセットして USB モードに戻すことができます。
    
    ```command
    adb usb
    ```
    
    コンピューターに接続されているデバイスをリストするように ADB に要求できます。 デバイスの接続方法に関係なく、コマンド プロンプトで次のコマンドを実行することで、接続されているデバイスを表示できます。
    
    ```command
    adb devices
    ```

## <a name="troubleshooting"></a>トラブルシューティング

場合によっては、デバイスがコンピューターに接続できないことがあります。 この場合は、USB ドライバーがインストールされていることを確認する必要があります。

## <a name="install-usb-drivers"></a>USB ドライバーをインストールする

このステップは macOS では必要ありません。デバイスを USB ケーブルで Mac に接続するだけです。

Windows コンピューターが USB で接続された Android デバイスを認識する前に、いくつかの追加のドライバーをインストールする必要がある場合があります。

> [!NOTE]
> これらは、Google Nexus デバイスの設定手順であり、参照として提供されるものです。 特定のデバイスの手順は異なる場合がありますが、同様のパターンに従います。 何かお困りでしたら、インターネットで使用しているデバイスを検索してください。

**[Android SDK のインストール パス]\tools** ディレクトリの **android.bat** アプリケーションを実行します。 既定では、Xamarin.Android インストーラーは、Windows コンピューター上の次の場所に Android SDK を配置します。

`C:\Users\[username]\AppData\Local\Android\android-sdk`

### <a name="download-the-usb-drivers"></a>USB ドライバーをダウンロードする

Google Nexus デバイス (Galaxy Nexus を除く) では、Google USB ドライバーが必要です。 Galaxy Nexus 用のドライバーは [Samsung によって配布](https://www.samsung.com/us/support/downloads/)されます。
他のすべての Android デバイスでは、[それぞれの製造元の USB ドライバー](https://developer.android.com/tools/extras/oem-usb.html#Drivers)を使用する必要があります。

Android SDK マネージャーを起動し、以下のスクリーン ショットに示されているように、**Extras** フォルダーを展開して、**Google USB ドライバー** パッケージをインストールします。

![](set-up-device-for-development-images/google-usb-driver.png "Google USB driver selected")

**[Google USB ドライバー]** ボックスをオンにして、 **[Apply Changes]\(変更の適用\)** ボタンをクリックします。
次の場所にドライバー ファイルがダウンロードされます。

`[Android SDK install path]\extras\google\usb\_driver`

Xamarin.Android インストールの既定のパスは次のとおりです。

`C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver`

### <a name="installing-the-usb-driver"></a>USB ドライバーのインストール

USB ドライバーがダウンロードされたら、それらをインストールする必要があります。
Windows 7 でドライバーをインストールする場合は、次のようにします。

1. USB ケーブルでデバイスをコンピューターに接続します。

2. デスクトップまたは Windows エクスプローラーでコンピューターを右クリックして、 **[管理]** を選択します。

3. 左側のウィンドウで **[デバイス]** を選択します。

4. 右側のウィンドウで **[その他のデバイス]** を見つけて展開します。

5. デバイス名を右クリックして、 **[ドライバー ソフトウェアの更新]** を選択します。
    これで、ハードウェアの更新ウィザードが起動します。

6. **[コンピューターを参照してドライバー ソフトウェアを検索します**] を選択して、 **[次へ]** をクリックします。

7. **[参照]** をクリックして、USB ドライバー フォルダーを見つけます (Google USB ドライバーは **[Android SDK のインストール パス]\extras\google\usb_driver** にあります)。

8. **[次へ]** をクリックして、ドライバーをインストールします。

## <a name="summary"></a>まとめ

この記事では、デバイスでのデバッグを有効にして、開発用に Android デバイスを構成する方法について説明しました。 USB または WiFi を使用して、コンピューターにデバイスを接続する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [Android Debug Bridge](https://developer.android.com/tools/help/adb.html)
- [ハードウェア デバイスの使用](https://developer.android.com/tools/device.html)
- [Samsung ドライバーのダウンロード](https://www.samsung.com/us/support/downloads/)
- [OEM USB ドライバー](https://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google USB ドライバー](https://developer.android.com/sdk/win-usb.html)
- [XDA 開発者: Windows 8 - ADB/高速ブート ドライバーの問題が解決されました](https://forum.xda-developers.com/showthread.php?t=1583801)
