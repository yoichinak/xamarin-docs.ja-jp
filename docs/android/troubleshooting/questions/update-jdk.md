---
title: Java Development Kit (JDK) のバージョンの更新方法を教えてください
description: この記事では、Windows および Mac で Java Development Kit (JDK) のバージョンを更新する方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: 0f7499551db7d86d7978b9c3e1f562a2f054c202
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019527"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Java Development Kit (JDK) のバージョンの更新方法を教えてください

_この記事では、Windows および Mac で Java Development Kit (JDK) のバージョンを更新する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Android では、Java Development Kit (JDK) を使用して、Android アプリのビルドと Android デザイナーの実行を行う Android SDK を統合します。 最新バージョンの Android SDK (API 24 以降) では JDK 8 (1.8) が必要です。 または、[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) をインストールできます。 Xamarin.Android 開発用の JDK 8 は、最終的には Microsoft Mobile OpenJDK に置き換わります。

Microsoft Mobile OpenJDK に更新するには、[Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) に関する記事を参照してください。 JDK 8 に更新するには、次の手順を実行します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. [Oracle Web サイト](https://www.oracle.com/technetwork/java/javase/downloads/index.html)から JDK 8 (1.8) をダウンロードします。

    ![Oracle Web サイトの JDK ダウンロード ページのスクリーンショット](update-jdk-images/image1.png)

2. Xamarin Android デザイナーで、[カスタム コントロール](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols)を表示できるように、64 ビット バージョンを選択します。

    ![JDK のダウンロード ページからダウンロードする Windows x64 JDK パッケージの選択](update-jdk-images/image2.png)

3. .exe を実行して **Development Tools** をインストールします。

    ![JDK インストーラーでの Development Tools のインストール](update-jdk-images/image3.png)

4. Visual Studio を開き、 **[ツール] > [オプション] > [Xamarin] > [Android の設定] > [Java Development Kit の場所]** にある新しい JDK をポイントするように **[Java Development Kit の場所]** を更新します。

    [![[Android の設定] ページでの JDK のパス設定](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

場所を更新した後、必ず Visual Studio を再起動してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. [Oracle Web サイト](https://www.oracle.com/technetwork/java/javase/downloads/index.html)から JDK 8 (1.8) をダウンロードします。

    ![Oracle Web サイトの JDK ダウンロード ページのスクリーンショット](update-jdk-images/image1.png)

2. .dmg ファイルを開き、.pkg インストーラーを実行します。

    ![macOS での JDK インストーラーの実行](update-jdk-images/image5.png)

Mac OS では、 **/System/Library/Frameworks/JavaVM.framework/Versions/Current** を更新することで、新しい JDK バージョンが既定値として自動的に設定されます。 その後、**Java SDK (JDK)** の場所が、 **[Visual Studio for Mac] > [ユーザー設定] > [プロジェクト] > [SDK の場所] > [Android] > [場所] > [Java SDK (JDK) の場所]** に、期待される既定値である **/usr** に設定されていることを再確認できます。

[![Android の [場所] タブで JDK の場所を設定する](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----
