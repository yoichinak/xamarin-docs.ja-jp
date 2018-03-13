---
title: "自分の Android SDK の場所を設定できるのですか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 11/16/2017
ms.openlocfilehash: 0113cc15bf1de5e0e668b05c2b0288a6ead141b5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>自分の Android SDK の場所を設定できるのですか。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio に移動**ツール > オプション > Xamarin > Android 設定**を表示し、Android SDK の場所を設定します。

[![環境設定の例の場所 タブ](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Java Development Kit の場所: 

    **C:\\プログラム ファイル\\Java\\jdk1.8.0_131**

- Android SDK の場所: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK 場所: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK のバージョン番号が異なる場合がありますに注意してください。 たとえばの代わりに**android ndk-r13b**、以前のバージョンをなどある可能性があります**android ndk-r10e**です。

Android SDK の場所を設定するに Android SDK ディレクトリの完全なパスを入力してください、 **Android SDK の場所**ボックス。 ファイル エクスプ ローラーで Android SDK の場所に移動し、アドレス バーからパスをコピーおよびにこのパスを貼り付け、 **Android SDK の場所**ボックス。
例では、Android SDK の場所がある場合、 **c:\\ユーザー\\username\\AppData\\ローカル\\Android\\Sdk**、で古いパスをクリア**Android SDK の場所**ボックス、このパスの貼り付け をクリックして**OK**です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio に移動**設定 > プロジェクト > SDK の場所 > Android**です。 **Android**  ページで、をクリックして、**場所** タブを表示し、SDK の場所を設定します。

[![環境設定の例の場所 タブ](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Android SDK の場所: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK 場所: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) の場所: 

    **/usr**

NDK のバージョン番号が異なる場合がありますに注意してください。 たとえばの代わりに**android ndk-r14b**、以前のバージョンをなどある可能性があります**android ndk-r10e**です。

Android SDK の場所を設定するに Android SDK ディレクトリの完全なパスを入力してください、 **Android SDK の場所**ボックス。 Android SDK フォルダーを選択するにはキーを押して、Finder で**CTRL +&#8984;+ I**フォルダー情報を表示するをクリックし、パスの右側にドラッグ**場所:**、コピー、し、貼り付けます、 **Android SDK場所**ボックスに、**場所**タブです。例では、Android SDK の場所がある場合、 **~/Library/Developer/Android/Sdk**で古いパスをクリア、 **Android SDK の場所**ボックス、このパスの貼り付け をクリックして**OK**.

-----
