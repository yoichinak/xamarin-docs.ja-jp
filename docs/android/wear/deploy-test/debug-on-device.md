---
title: Wear デバイスでのデバッグします。
description: この記事では、Xamarin.Android Wear Wear デバイスでアプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 232fcd1d369eba1daad170986f2e2c4c913a3649
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112467"
---
# <a name="debug-on-a-wear-device"></a>Wear デバイスでのデバッグします。

_この記事では、Xamarin.Android Wear Wear デバイスでアプリケーションをデバッグする方法について説明します。_


## <a name="overview"></a>概要

Android Wear デバイス、Android Wear スマートウォッチなどを使っている場合は、エミュレーターを使用する代わりに、デバイスでアプリを実行できます。 (ない場合の展開および実行するプロセスに詳しく Android Wear アプリを参照してください[こんにちは、Wear](~/android/wear/get-started/hello-wear.md))。

## <a name="prepare-the-wear-device"></a>Wear デバイスを準備します。

Android Wear デバイスでデバッグを有効にするのにには、次の手順を使用します。

1.  開く、**設定**Android Wear デバイス上のメニュー。

2.  タップし、メニューの一番下までスクロール**について**します。

3.  7 回ビルド番号をタップします。

4.  **設定**メニューをタップします**開発者向けオプション**します。

5.  確認します**ADB デバッグ**を有効にします。


## <a name="debugging-over-usb"></a>USB 経由でのデバッグ

Wear デバイスでは、USB ポートには場合、Wear デバイスをコンピューターに接続を展開し、Android フォンを使用するように、アプリを実行/デバッグ (詳細については、次を参照してください。[デバイス上のデバッグ](~/android/deploy-test/debugging/debug-on-device.md))。


## <a name="debugging-over-bluetooth"></a>Bluetooth 経由でのデバッグ

Wear デバイスでは、USB ポートはありません、お使いのコンピューターに接続されている Android フォンにアプリのデバッグ出力をルーティングして Bluetooth 経由で Wear デバイスにアプリをデプロイできます。 

### <a name="prepare-your-phone"></a>携帯電話を準備します。

Wear デバイスへの Bluetooth 接続を行うため、携帯電話を準備するのにには、次の手順を使用します。 

1.  されていない場合を設定する Xamarin.Android 開発用の電話で説明したよう[開発用デバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)します。

2.  ダウンロードしてインストール、無料[Android Wear](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) Google Play ストアからアプリ。

### <a name="connect-the-device"></a>デバイスを接続します。

Wear デバイスを携帯電話に接続するのにには、次の手順を使用します。

1.  携帯電話の Bluetooth 中間 (上記で構成) として機能、Android Wear アプリが起動されます。 

2.  タップして、**設定**アイコン。

3.  有効にする**Bluetooth 経由でデバッグ**します。 Android Wear アプリの画面に表示される、次の状態が表示されます。

        Host: disconnected
        Target: connected

4.  USB 経由で電話をコンピューターに接続します。 コンピューターには、次のコマンドを入力します。

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    ポート 4444 が使用できない場合は、アクセス権があるその他の使用可能なポートを使用することができます。 

    **注**: Visual Studio または Visual Studio for Mac を再起動する場合は、Wear デバイスへの接続をセットアップするには、もう一度これらのコマンドを実行する必要があります。

5.  Wear デバイスが表示されたら、許可することを確認**ADB デバッグ**します。 Android Wear アプリで、変更の状態が表示されます。

        Host: connected
        Target: connected

6.  上記の手順を完了すると、次のように実行されている`adb devices`電話、および Android Wear デバイスの状態が表示されます。

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

この時点では、Wear デバイスにアプリをデプロイできます。

<a name="screenshots" />

### <a name="taking-screenshots"></a>スクリーン ショットを作成

Wear デバイスのスクリーン ショットは、次のコマンドを入力して実行できます。 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

スクリーン ショットを次のコマンドを入力して、コンピューターにコピーします。

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

デバイスのスクリーン ショットを削除するには、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>アプリをアンインストールします。

Wear デバイスからアプリをアンインストールするには、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

たとえば、パッケージ名を持つアプリを削除する`com.xamarin.weartest`、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Bluetooth 経由で Android Wear デバイスのデバッグの詳細については、[Bluetooth 経由でデバッグ](https://developer.android.com/training/wearables/apps/bt-debugging.html)を参照してください。


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>コンパニオンの phone アプリの Wear アプリのデバッグ

Android Wear アプリが Google Play で配布するためのコンパニオン Android フォン アプリをパッケージ化されます (詳細については、次を参照してください。[パッケージ化操作](~/android/wear/deploy-test/packaging.md))。 ただし、引き続き開発する Wear アプリとそのコンパニオン アプリとは別にします。 Google Play ストアからアプリをリリースするときに、Wear アプリはコンパニオン アプリをパッケージ化し、可能であれば自動的にインストールします。

コンパニオン アプリを使用して、Wear アプリをデバッグします。 

1.  ビルドし、コンパニオン アプリを電話に展開します。

2.  Wear プロジェクトを右クリックし、既定のスタート プロジェクトとして設定します。

3.  Wear プロジェクトをウェアラブル デバイスをデプロイします。

4.  実行し、デバイス上の Wear アプリをデバッグします。

 
## <a name="summary"></a>まとめ

この記事では、付属の phone アプリの Wear アプリをデバッグする方法と、Bluetooth 経由で Visual Studio からデバッグを Wear、Android Wear デバイスを構成する方法について説明します。 Bluetooth 経由で Wear アプリをデバッグするための一般的なデバッグに関するヒントも提供されます。
