---
title: Android SDK の場所はどこで設定できますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 11/16/2017
ms.openlocfilehash: c004fb7717f78750e7ac1e8dc1856a32ba808638
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227687"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Android SDK の場所はどこで設定できますか

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio に移動します。**ツール > オプション > Xamarin > Android 設定**表示し、Android SDK の場所を設定します。

[![例の基本設定の場所 タブ](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Java Development Kit の場所: 

    **C:\\プログラム ファイル\\Java\\jdk1.8.0_131**

- Android SDK の場所: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK の場所: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK のバージョン番号が異なる場合がありますに注意してください。 例については、代わりに**r13b-android の ndk**、以前のバージョンをなどある可能性があります**r10e-android の ndk**します。

Android SDK の場所を設定するに Android SDK ディレクトリの完全なパスを入力、 **Android SDK の場所**ボックス。 ファイル エクスプ ローラーで Android SDK の場所に移動します、アドレス バーから、パスをコピーおよびにこのパスを貼り付けて、 **Android SDK の場所**ボックス。
たとえば、Android SDK の場所がある場合**c:\\ユーザー\\username\\AppData\\ローカル\\Android\\Sdk**、で古いパスをクリア**Android SDK の場所**ボックス、このパスの貼り付け をクリックして**OK**します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac に移動します。**設定 > プロジェクト > SDK の場所 > Android**します。 **Android**  ページで、をクリックして、**場所**表示し、SDK の場所の設定 タブ。

[![例の基本設定の場所 タブ](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Android SDK の場所: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK の場所: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) の場所: 

    **/usr**

NDK のバージョン番号が異なる場合がありますに注意してください。 例については、代わりに**r14b-android の ndk**、以前のバージョンをなどある可能性があります**r10e-android の ndk**します。

Android SDK の場所を設定するに Android SDK ディレクトリの完全なパスを入力、 **Android SDK の場所**ボックス。 Finder で、キーを押して、Android SDK フォルダーを選択できる**CTRL +&#8984;+ I**フォルダー情報を表示する をクリックし、パスの右側にドラッグ**場所:** をコピーし、それを、 **Android SDK場所**ボックスに、**場所**タブ。たとえば、Android SDK の場所がある場合 **~/Library/Developer/Android/Sdk**で古いパスをクリア、 **Android SDK の場所**ボックス、このパスの貼り付け をクリックして**OK**.

-----
