---
title: Android SDK の場所はどこで設定できますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/16/2017
ms.openlocfilehash: 8685be4bb1cc45ff04dc8d9f7d8e64e7b1483b60
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027026"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Android SDK の場所はどこで設定できますか

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Android SDK の場所を表示および設定するには、Visual Studio で **[ツール] > [オプション] > [Xamarin] > [Android 設定]** に移動します。

[![[ユーザー設定] の [場所] タブの例](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Java Development Kit の場所: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Android SDK の場所: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK の場所: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK のバージョン番号は異なる場合があることに注意してください。 たとえば、**android-ndk-r13b** ではなく、**android-ndk-r10e** などの以前のバージョンである可能性があります。

Android SDK の場所を設定するには、Android SDK ディレクトリの完全なパスを **[Android SDK の場所]** ボックスに入力します。 エクスプローラーで Android SDK の場所に移動し、アドレス バーからパスをコピーして、 **[Android SDK の場所]** ボックスにこのパスを貼り付けます。
たとえば、Android SDK の場所が **C:\\Users\\username\\AppData\\Local\\Android\\Sdk** である場合、 **[Android SDK の場所]** ボックス内の古いパスをクリアし、このパスを貼り付けて、 **[OK]** をクリックします。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で、 **[ユーザー設定] > [プロジェクト] > [SDK の場所] > [Android]** に移動します。 **[Android]** ページで、 **[場所]** タブをクリックして、SDK の場所を表示および設定します。

[![[ユーザー設定] の [場所] タブの例](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Android SDK の場所: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK の場所: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) の場所: 

    **/usr**

NDK のバージョン番号は異なる場合があることに注意してください。 たとえば、**android-ndk-r14b** ではなく、**android-ndk-r10e** などの以前のバージョンである可能性があります。

Android SDK の場所を設定するには、Android SDK ディレクトリの完全なパスを **[Android SDK の場所]** ボックスに入力します。 ファインダーで Android SDK フォルダーを選択し、**CTRL + &#8984; + I** キーを押してフォルダーの情報を表示し、 **[場所:]** の右側のパスをクリックしてドラッグし、コピーしてから、それを **[場所]** タブの **[Android SDK の場所]** ボックスに貼り付けます。たとえば、Android SDK の場所が **~/Library/Developer/Android/Sdk** である場合、 **[Android SDK の場所]** ボックス内の古いパスをクリアし、このパスを貼り付けて、 **[OK]** をクリックします。

-----
