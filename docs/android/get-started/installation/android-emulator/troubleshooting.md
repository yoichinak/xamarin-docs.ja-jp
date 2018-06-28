---
title: エミュレーターのセットアップに関する問題のトラブルシューティング
description: この記事では、Android Device Manager の使用中に発生する可能性のある問題を診断して回避する方法について説明します。
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733652"
---
# <a name="troubleshooting-emulator-setup-problems"></a>エミュレーターのセットアップに関する問題のトラブルシューティング

_この記事では、Android Device Manager を使用して Android Emulator を構成するときに発生する可能性のある問題を診断して回避する方法について説明します。Android Emulator の実行時の問題のトラブルシューティングについては、「[Google Android Emulator のトラブルシューティング](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)」を参照してください。_

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>標準以外の場所にある Android SDK

通常、Android SDK は次の場所にインストールされます。

**C:\\Program Files (x86)\\Android\\android-sdk**

SDK がこの場所にインストールされていない場合、Android Device Manager の起動時に次のエラーが発生する可能性があります。

![Android SDK インスタンスのエラー](troubleshooting-images/win/01-sdk-error.png)

この問題を回避するには、次のようにします。

1. Windows デスクトップから、**C:\\Users\\<*ユーザー名*>\\AppData\\Roaming\\XamarinDeviceManager** に移動します。

    ![Android Device Manager のログ ファイルの場所](troubleshooting-images/win/02-log-files.png)

2. いずれかのログ ファイルをダブルクリックして開き、**構成ファイルのパス**を調べます。 例:

    [![ログ ファイルでの構成ファイルのパス](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. この場所に移動し、**user.config** をダブルクリックして開きます。 

4. **user.config** で **&lt;UserSettings&gt;** 要素を探し、それに **AndroidSdkPath** 属性を追加します。 Android SDK がインストールされているコンピューター上のパスをこの属性に設定し、ファイルを保存します。 たとえば、Android SDK が **C:\\Programs\\Android\\SDK** にインストールされている場合、**&lt;UserSettings&gt;** は次のようになります。
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

**user.config** をこのように変更すると、Android Device Manager を起動できるようになります。

## <a name="snapshot-disables-wifi-on-android-oreo"></a>スナップショットによって Android Oreo の WiFi が無効になる

Android Oreo 用に構成された AVD で Wi-Fi アクセスをシミュレートしている場合、スナップショットの後で AVD を再起動すると、Wi-Fi アクセスが無効になる場合があります。

この問題を回避するには、次のようにします。

1. Android Device Manager で AVD を選びます。

2. 追加のオプション メニューから、**[エクスプローラーで表示します]** をクリックします。

3. **snapshots > default_boot** に移動します。

4. **snapshot.pb** ファイルを削除します。

    ![snapshot.pb ファイルの場所](troubleshooting-images/win/05-delete-snapshot.png)

5. AVD を再起動します。 

これらの変更を行った後は、AVD は Wi-Fi が再び機能する状態で再起動します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>スナップショットによって Android Oreo の WiFi が無効になる

Android Oreo 用に構成された AVD で Wi-Fi アクセスをシミュレートしている場合、スナップショットの後で AVD を再起動すると、Wi-Fi アクセスが無効になる場合があります。

この問題を回避するには、次のようにします。

1. Android Device Manager で AVD を選びます。

2. 追加のオプション メニューから、**[Finder で表示します]** をクリックします。

3. **snapshots > default_boot** に移動します。

4. **snapshot.pb** ファイルを削除します。

    [![snapshot.pb ファイルの場所](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. AVD を再起動します。 

これらの変更を行った後は、AVD は Wi-Fi が再び機能する状態で再起動します。


-----

## <a name="generating-a-bug-report"></a>バグ報告の生成

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

上記のトラブルシューティングのヒントで解決できない問題が Android Device Manager で見つかった場合は、タイトル バーを右クリックし、**[Generate Bug Report]\(バグ報告の生成\)** を選択して、バグ報告を提出してください。

[![バグ報告の提出に関するメニュー項目の場所](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

上記のトラブルシューティングのヒントで解決できない問題が Android Device Manager で見つかった場合は、**[ヘルプ]、[Generate Bug Report]\(バグ報告の生成\)** の順にクリックして、バグ報告を提出してください。

[![バグ報告の提出に関するメニュー項目の場所](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
