---
title: Wear デバイスでのデバッグ
description: この記事では、Wear デバイスで Xamarin の Android Wear アプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 965ed4e802c05f8450192c0fec17fe31e464c779
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916625"
---
# <a name="debug-on-a-wear-device"></a>Wear デバイスでのデバッグ

_この記事では、摩耗デバイスで Xamarin の Android の磨耗アプリケーションをデバッグする方法について説明します。_

## <a name="overview"></a>概要

Android Wear Smartwatch などの Android Wear デバイスがある場合は、エミュレーターを使用する代わりに、デバイスでアプリを実行できます。 (Android 用の摩耗アプリの展開と実行のプロセスにまだ慣れていない場合は、「 [Hello, 磨耗](~/android/wear/get-started/hello-wear.md)」を参照してください)。

## <a name="prepare-the-wear-device"></a>Wear デバイスを準備する

Android Wear デバイスでデバッグを有効にするには、次の手順に従います。

1. Android の磨耗デバイスで **[設定]** メニューを開きます。

2. メニューの一番下までスクロールし、 **[バージョン情報]** をタップします。

3. ビルド番号を7回タップします。

4. **[設定]** メニューの **[開発者オプション]** をタップします。

5. **ADB デバッグ**が有効になっていることを確認します。

## <a name="debugging-over-usb"></a>USB 経由でのデバッグ

デバイスに USB ポートがある場合は、コンピューターに磨耗デバイスを接続してデプロイし、Android フォンを使用する場合と同じようにアプリを実行/デバッグすることができます (詳細については、「[デバイスでのデバッグ](~/android/deploy-test/debugging/debug-on-device.md)」を参照してください)。

## <a name="debugging-over-bluetooth"></a>Bluetooth でのデバッグ

使用しているデバイスに USB ポートがない場合は、コンピューターに接続されている Android フォンにアプリのデバッグ出力をルーティングすることによって、Bluetooth 経由でアプリを Wear デバイスに展開することができます。 

### <a name="prepare-your-phone"></a>電話を準備する

次の手順を使用して、デバイスへの Bluetooth 接続を確立するための電話を準備します。 

1. まだインストールしていない場合は、「[開発用にデバイスを設定する](~/android/get-started/installation/set-up-device-for-development.md)」の説明に従って、Xamarin Android 開発用の電話を設定します。

2. Google Play ストアから無料の[Android 劣化](https://play.google.com/store/apps/details?id=com.google.android.wearable.app)アプリをダウンロードしてインストールします。

### <a name="connect-the-device"></a>デバイスを接続する

次の手順に従って、Wear デバイスを電話に接続します。

1. Bluetooth 中継局として機能する電話 (上記で構成) で、Android Wear アプリを起動します。 

2. **[設定]** アイコンをタップします。

3. **Bluetooth でのデバッグ**を有効にします。 Android Wear アプリの画面に次の状態が表示されます。

    ```
    Host: disconnected
    Target: connected
    ```

4. USB 経由で電話をコンピューターに接続します。 コンピューターで、次のコマンドを入力します。

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    ポート4444が使用できない場合は、アクセスできる他の任意のポートを使用できます。 

    > [!NOTE]
    > Visual Studio または Visual Studio for Mac を再起動する場合は、これらのコマンドをもう一度実行して、Wear デバイスへの接続をセットアップする必要があります。

5. 磨耗デバイスからプロンプトが表示されたら、 **ADB デバッグ**を許可していることを確認します。 Android Wear アプリでは、状態が次のように変化します。

    ```
    Host: connected
    Target: connected
    ```

6. 上記の手順を完了すると、`adb devices` を実行すると、電話と Android の両方のデバイスの状態が表示されます。

    ```
    List of devices attached
    127.0.0.1:4444    device
    019ad61df0a69399  device
    ```

この時点で、アプリを Wear デバイスにデプロイできます。

<a name="screenshots" />

### <a name="taking-screenshots"></a>スクリーンショットの撮影

次のコマンドを入力して、Wear デバイスのスクリーンショットを取得できます。 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

次のコマンドを入力して、スクリーンショットをコンピューターにコピーします。

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

次のコマンドを入力して、デバイスのスクリーンショットを削除します。

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```

### <a name="uninstalling-an-app"></a>アプリのアンインストール

次のコマンドを入力して、Wear デバイスからアプリをアンインストールできます。

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

たとえば、`com.xamarin.weartest`パッケージ名を使用してアプリを削除するには、次のコマンドを入力します。

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Bluetooth 経由での Android の磨耗デバイスのデバッグの詳細については、「 [bluetooth 経由](https://developer.android.com/training/wearables/apps/bt-debugging.html)でのデバッグ」を参照してください。

## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>コンパニオン電話アプリでの Wear アプリのデバッグ

Android の摩耗アプリは、Google Play で配布するためのコンパニオン Android phone アプリと共にパッケージ化されます (詳細については、「[パッケージングの](~/android/wear/deploy-test/packaging.md)使用」を参照してください)。 ただし、引き続き、Wear アプリとそのコンパニオンアプリを別々に開発しています。 Google Play ストアを通じてアプリをリリースすると、アプリがコンパニオンアプリと共にパッケージ化され、可能であれば自動的にインストールされます。

コンパニオンアプリを使用して、Wear アプリをデバッグするには: 

1. コンパニオンアプリを構築し、電話にデプロイします。

2. [Wear] プロジェクトを右クリックし、[既定の開始] プロジェクトとして設定します。

3. ウェアラブルデバイスに Wear プロジェクトをデプロイします。

4. デバイスでの Wear アプリの実行とデバッグを行います。

## <a name="summary"></a>まとめ

この記事では、Bluetooth を使用して Visual Studio からの Wear デバッグ用に Android Wear デバイスを構成する方法、およびコンパニオン電話アプリを使用して Wear アプリをデバッグする方法について説明しました。 また、Bluetooth を使用して Wear アプリをデバッグするための一般的なデバッグのヒントも提供しました。
