---
title: Java Development Kit (JDK) のバージョンの更新方法を教えてください
description: この記事では、Windows および Mac で Java Development Kit (JDK) のバージョンを更新する方法について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: 02768bfcae9da06d8fd86ada63dd2d463245d254
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510461"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Java Development Kit (JDK) のバージョンの更新方法を教えてください

_この記事では、Windows および Mac で Java Development Kit (JDK) のバージョンを更新する方法について説明します。_

## <a name="overview"></a>概要

Xamarin Android は、Java Development Kit (JDK) を使用して、Android アプリをビルドし、Android designer を実行するための Android SDK と統合します。 最新バージョンの Android SDK (API 24 以降) には JDK 8 (1.8) が必要です。 または、 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)をインストールすることもできます。 Microsoft Mobile OpenJDK は、最終的には、Xamarin Android の開発用に JDK 8 を置き換えます。

Microsoft Mobile OpenJDK に更新するには、「 [Microsoft Mobile Openjdk Preview](~/android/get-started/installation/openjdk.md)」を参照してください。 JDK 8 に更新するには、次の手順を実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  [Oracle web サイト](https://www.oracle.com/technetwork/java/javase/downloads/index.html)から JDK 8 (1.8) をダウンロードします。

    ![Oracle web サイトの JDK ダウンロードページのスクリーンショット](update-jdk-images/image1.png)

2.  Xamarin Android designer での[カスタムコントロール](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols)の表示を許可するには、64ビットバージョンを選択します。

    ![JDK のダウンロードページからダウンロードする Windows x64 JDK パッケージの選択](update-jdk-images/image2.png)

3.  .Exe を実行し、**開発ツール**をインストールします。

    ![JDK インストーラーでの開発ツールのインストール](update-jdk-images/image3.png)

4.  Visual Studio を開き、 **Java Development kit の場所**を更新して、[ツール > オプション] の下の新しい JDK をポイントし **> Xamarin > Android の設定 > java Development kit の場所**に移動します。

    [![[Android の設定] ページの JDK のパス設定](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

場所を更新した後、必ず Visual Studio を再起動してください。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  [Oracle web サイト](https://www.oracle.com/technetwork/java/javase/downloads/index.html)から JDK 8 (1.8) をダウンロードします。

    ![Oracle web サイトの JDK ダウンロードページのスクリーンショット](update-jdk-images/image1.png)

2.  Dmg ファイルを開き、.pkg インストーラーを実行します。

    ![MacOS で JDK インストーラーを実行する](update-jdk-images/image5.png)

**/System/Library/Frameworks/JavaVM.framework/Versions/Current**を更新すると、新しい JDK バージョンが既定値として自動的に設定されます。 Mac OS 次に、 **JAVA sdk (jdk)** の場所が、予想される既定値である **/usr**に設定されていることを再確認できます。 **Visual Studio for Mac > 基本設定 > プロジェクト > SDK 場所 > ANDROID > 場所 > java Sdk (jdk) の場所**:

[![[Android の場所] タブで JDK の場所を設定する](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

