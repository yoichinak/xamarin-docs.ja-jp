---
title: Java Development Kit (JDK) バージョンを更新する方法は?
description: この記事では、Windows とファルダに Java Development Kit (JDK) バージョンを更新する方法を示しています。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: aa04d944f803dded0e9448de27ed7d5ced2efb54
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109186"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Java Development Kit (JDK) バージョンを更新する方法は?

_この記事では、Windows とファルダに Java Development Kit (JDK) バージョンを更新する方法を示しています。_

## <a name="overview"></a>概要

Xamarin.Android では、Java Development Kit (JDK) を使用して、Android アプリを構築して、Android designer を実行するための Android SDK と統合します。 Android SDK (API 24 以降) の最新バージョンでは、JDK 8 (1.8) が必要です。 または、インストールすることができます、 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)します。 Microsoft Mobile OpenJDK は、Xamarin.Android の開発の JDK 8 を最終的に置き換えられます。

Microsoft Mobile openjdk を更新するを参照してください。 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)します。 JDK 8 に更新するには、次の手順を実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  JDK 8 (1.8) をダウンロード、 [Oracle の web サイト](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![JDK のスクリーン ショットは、Oracle web サイトのページをダウンロードします。](update-jdk-images/image1.png)

2.  レンダリングを許可する 64 ビット バージョンを選択[カスタム コントロール](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols)Xamarin Android デザイナーで。

    ![JDK ダウンロードのページからダウンロードする Windows x64 の JDK パッケージの選択](update-jdk-images/image2.png)

3.  .Exe を実行し、インストール、**開発ツール**:

    ![JDK のインストーラーで、開発ツールをインストールします。](update-jdk-images/image3.png)

4.  Visual Studio と更新プログラムを開く、 **Java Development Kit の場所**下で新しい JDK を指す**ツール > オプション > Xamarin > Android 設定 > Java Development Kit の場所**:

    [![[Android 設定] ページで、JDK のパスの設定](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

場所を更新した後、Visual Studio を再起動してください。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  JDK 8 (1.8) をダウンロード、 [Oracle の web サイト](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![JDK のスクリーン ショットは、Oracle web サイトのページをダウンロードします。](update-jdk-images/image1.png)

2.  .Dmg ファイルを開き、.pkg インストーラーを実行します。

    ![MacOS で JDK インストーラーを実行しています。](update-jdk-images/image5.png)

Mac OS は更新することで、既定値として新しい JDK バージョンを自動的に設定されます **/System/Library/Frameworks/JavaVM.framework/Versions/Current**します。 再確認することができますし、 **Java SDK (JDK)** 場所が必要な既定値に設定されている **/usr**  **Visual Studio for Mac > 設定 > プロジェクト > SDK の場所 >Android > の場所 > Java SDK (JDK) の場所**:

[![Android の場所 タブで、JDK の場所を設定します。](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

