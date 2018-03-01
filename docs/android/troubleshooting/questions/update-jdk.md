---
title: "Java Development Kit (JDK) バージョンを更新する方法は?"
description: "この記事は、Windows およびファルダで Java Development Kit (JDK) バージョンを更新する方法を示しています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 126d4b2eaf1c2408704c0f2d11c2f88a7c25f406
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Java Development Kit (JDK) バージョンを更新する方法は?

_この記事は、Windows およびファルダで Java Development Kit (JDK) バージョンを更新する方法を示しています。_

## <a name="overview"></a>概要

Xamarin.Android では、Java Development Kit (JDK) を使用して、Android アプリをビルドして、Android デザイナーを実行するための Android SDK を統合します。 Android SDK API (24 以降) の最新バージョンでは、JDK 8 (1.8) が必要です。 まだ更新していない JDK 8 に、この手順をインストールして有効にします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  JDK 8 (1.8) をダウンロード、 [Oracle の web サイト](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![JDK のスクリーン ショットは、Oracle web サイトをダウンロードします。](update-jdk-images/image1.png)

2.  表示を許可する 64 ビット バージョンが取得[カスタム コントロール](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols)Xamarin Android デザイナーで。

    ![JDK ダウンロード ページからダウンロードする、Windows x64 の JDK パッケージを選択します。](update-jdk-images/image2.png)

3.  .Exe を実行し、インストール、**開発ツール**:

    ![JDK インストーラーに、開発ツールをインストールします。](update-jdk-images/image3.png)

4.  Visual Studio と更新プログラムを開く、 **Java Development Kit 場所**下にある新しい JDK を指す**ツール > オプション > Xamarin > Android 設定 > Java Development Kit の場所 > 変更**:

    ![IDE のオプションの Android の設定 ページで、JDK のパスの設定](update-jdk-images/image4.png)

必ず、場所を更新した後に Visual Studio を再起動してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  JDK 8 (1.8) をダウンロード、 [Oracle の web サイト](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![JDK のスクリーン ショットは、Oracle web サイトをダウンロードします。](update-jdk-images/image1.png)

2.  (.Dmg) ファイルを開き、.pkg インストーラーを実行します。

    ![Macos JDK のインストーラーを実行](update-jdk-images/image5.png)

Mac OS は更新することで既定値として新しい JDK のバージョンを自動的に設定が**/System/Library/Frameworks/JavaVM.framework/Versions/Current**です。 再確認することができますし、 **Java SDK (JDK)**場所が必要な既定値に設定されている**/usr**  **Visual Studio for Mac > 設定 > プロジェクト > SDK の場所 >Android > Java SDK (JDK) > 場所**:

![Android SDK の場所 ページで、JDK の場所の設定](update-jdk-images/image6.png)

-----

