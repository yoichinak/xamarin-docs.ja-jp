---
title: 消耗デバイス上でのデバッグします。
description: この記事では、損傷デバイス上の Xamarin.Android 消耗アプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3f3143dcda4017bbabfbd34a58a40665beea6f75
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="debug-on-a-wear-device"></a>消耗デバイス上でのデバッグします。

_この記事では、損傷デバイス上の Xamarin.Android 消耗アプリケーションをデバッグする方法について説明します。_


## <a name="overview"></a>概要

Android の着用 Smartwatch など、Android を着用デバイスがある場合は場合、は、エミュレーターを使用する代わりに、デバイスでアプリを実行できます。 (がまだ配置および実行の手順に慣れる着用して Android アプリを参照してください場合[ハロー, 着用](~/android/wear/get-started/hello-wear.md))。

## <a name="prepare-the-wear-device"></a>デバイスを消耗を準備するには。

着用して Android デバイスでデバッグを有効にするのにには、次の手順を使用します。

1.  開く、**設定**着用して Android デバイス上のメニュー。

2.  タップし、メニューの一番下までスクロール**に関する**です。

3.  ビルド番号を 7 回タップします。

4.  **設定**メニューをタップ**開発者オプション**です。

5.  いることを確認**ADB デバッグ**を有効にします。


## <a name="debugging-over-usb"></a>USB 上のデバッグ

消耗デバイスに USB ポートがある場合は、損傷デバイスをコンピューターに接続する、展開、でき、Android フォンを使用するようにアプリを実行/デバッグ (詳細については、次を参照してください。[デバイス上でのデバッグ](~/android/deploy-test/debugging/debug-on-device.md))。


## <a name="debugging-over-bluetooth"></a>Bluetooth 経由でのデバッグ

消耗デバイスがある USB ポート、お使いのコンピューターに接続されている Android フォンにアプリのデバッグ出力をルーティングして Bluetooth 経由で消耗デバイスにアプリを展開できます。 

### <a name="prepare-your-phone"></a>お客様の電話を準備します。

消耗デバイスの Bluetooth 接続を行うため、電話を準備するのにには、次の手順を使用します。 

1.  されていない場合は、セットアップ Xamarin.Android 開発用の電話で説明したよう[開発用デバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)です。

2.  ダウンロードしてインストール、無料[Android 着用](https://play.google.com/store/apps/details?id=com.google.android.wearable.app)Google Play ストアからアプリ。

### <a name="connect-the-device"></a>デバイスを接続します。

お客様の電話に損傷デバイスを接続するのに、次の手順を使用します。

1.  電話で Bluetooth 中間 (上で構成した) として機能、着用して Android アプリが起動されます。 

2.  タップして、**設定**アイコン。

3.  有効にする**Bluetooth 経由でデバッグ**です。 Android を着用アプリの画面に表示される次の状態が表示されます。

        Host: disconnected
        Target: connected

4.  お使いのコンピューターに USB 経由で電話を接続します。 自分のコンピューターでは、次のコマンドを入力します。

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    ポート 4444 が利用できない場合は、アクセス権があるその他の使用可能なポートを使用できます。 

    **注**: for Mac を Visual Studio または Visual Studio を再起動する場合は、損傷デバイスへの接続をセットアップするには、もう一度これらのコマンドを実行する必要があります。

5.  消耗デバイスが表示されたら、ことを許可することを確認**ADB デバッグ**です。 Android 着用アプリで、変更の状態が表示されます。

        Host: connected
        Target: connected

6.  上記の手順を完了するを実行している`adb devices`電話、および Android を着用デバイスの状態を示します。

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

この時点では、損傷、デバイスにアプリを展開できます。

<a name="screenshots" />

### <a name="taking-screenshots"></a>作成のスクリーン ショット

消耗デバイスのスクリーン ショットを次のコマンドを入力して実行できます。 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

次のコマンドを入力して、コンピューターにスクリーン ショットをコピーします。

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

デバイスのスクリーン ショットを削除するには、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>アプリをアンインストールします。

消耗デバイスからアプリをアンインストールするには、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

たとえば、パッケージ名を持つアプリを削除する`com.xamarin.weartest`、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Bluetooth 経由での Android 着用デバイスのデバッグの詳細については、次を参照してください。 [Bluetooth 経由でデバッグ](https://developer.android.com/training/wearables/apps/bt-debugging.html)です。


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>コンパニオン電話アプリの消耗アプリのデバッグ

Android の消耗アプリは Google Play に配布するのコンパニオン Android フォン アプリに同梱されて (詳細については、次を参照してください。[パッケージングの使用](~/android/wear/deploy-test/packaging.md))。 ただし、引き続き開発する消耗アプリとそのコンパニオン アプリとは別にします。 Google Play ストアを使用してアプリを離すと、消耗アプリがコンパニオン アプリと一緒にパッケージ化し、可能な場合は自動的にインストールします。

コンパニオン アプリの消耗アプリをデバッグします。 

1.  構築して、コンパニオン アプリを電話に展開します。

2.  消耗プロジェクトを右クリックし、既定のスタート プロジェクトとして設定します。

3.  消耗プロジェクトをウェアラブル デバイスに配置します。

4.  実行し、デバイスの消耗アプリをデバッグします。

 
## <a name="summary"></a>まとめ

この記事では、コンパニオン電話アプリの消耗アプリをデバッグする方法と、Bluetooth を使用して Visual Studio からの消耗デバッグ用の Android 着用のデバイスを構成する方法について説明します。 Bluetooth を使用しての消耗アプリをデバッグするための一般的なデバッグに関するヒントも提供されます。
