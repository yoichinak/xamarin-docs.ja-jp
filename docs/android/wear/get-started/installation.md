---
title: 'インストールとセットアップ着用 OS onXamarin.Android '
description: この記事では、インストール手順と、コンピューターとデバイス開発を Android 着用を準備するために必要な構成の詳細について説明します。 この記事の目的は、最初 Xamarin.Android 消耗アプリケーションのビルドを開始する準備ができるし、Xamarin.Android 消耗インストールが Mac や Microsoft Visual Studio、Visual Studio に統合されて作業を必要があります。
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 14162663c518fdd1324f2b0340592fbae491d112
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436506"
---
# <a name="setup-and-installation"></a>セットアップとインストール

_この記事では、インストール手順と、コンピューターとデバイス開発を Android 着用を準備するために必要な構成の詳細について説明します。この記事の目的は、最初 Xamarin.Android 消耗アプリケーションのビルドを開始する準備ができるし、Xamarin.Android 消耗インストールが Mac や Microsoft Visual Studio、Visual Studio に統合されて作業を必要があります。_

## <a name="requirements"></a>要件

Android 着用して Xamarin ベースのアプリを作成する、次が必要。

-   **Visual Studio または Visual Studio for Mac** &ndash;するか Visual Studio 2015 Professional、Visual Studio を使用している以降が必要です。

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-   **Android SDK** -Android SDK 5.0.1 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin Android 開発が必要です[JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) API レベルの場合 24 開発または大きい値である場合 (JDK 1.8 もサポートしている API レベル 24 より前)。

使用を続行できます[JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API level 23 専用の開発または以前の場合。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

## <a name="installation"></a>インストール

Xamarin.Android をインストールした後は、ビルドし、着用して Android アプリをテストする準備ができるように、次の手順を実行します。 

1.  必要な Android SDK とツールをインストールします。
2.  テスト デバイスを構成します。
3.  最初の Android 着用アプリを作成します。

次の手順については、次のセクションで説明します。


### <a name="install-android-sdk-and-tools"></a>Android SDK とツールをインストールします。 

起動して、 **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio での Android SDK Manager を起動する方法](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Mac 用 Visual Studio での Android SDK Manager を起動する方法](installation-images/xs/sdk-menu.png)

-----


次の Android SDK とツールがインストールされてがあることを確認します。

* Android SDK ツール v 24.0.0 またはそれ以降、および
* Android 4.4W (API20)、または
* Android 5.0.1 (API21) またはそれ以降。

最新の SDK とツールがインストールされていない場合は、必要な SDK ツールをダウンロード*と*API ビット (それらを探してビットをスクロールする必要があります&ndash;API の選択範囲を次に示します)。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android 5.0.1 を有効にする例 SDK Manager スクリーン ショットのコンポーネント](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android 4.4 および 5.0.1 有効にする例 SDK Manager スクリーン ショットのコンポーネント](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>構成

使用するには、アプリをテスト、着用の Android エミュレーターまたは Android を着用実際のデバイスを構成する必要があります。 


### <a name="android-wear-emulator"></a>Android の消耗エミュレーター

Android を着用エミュレーターを使用することができます、前に、Android を着用 Android 仮想デバイス (AVD) を使用して、構成する必要があります、 **Google エミュレーター マネージャー**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio からの Android エミュレーター マネージャーを起動する方法](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Mac 用 Visual Studio からの Android エミュレーター マネージャーを起動する方法](installation-images/xs/emulator-menu.png)

-----

Android を着用エミュレーターの設定に関する詳細については、次を参照してください。[エミュレーターで Android 着用をデバッグ](~/android/wear/deploy-test/debug-on-emulator.md)です。


### <a name="android-wear-device"></a>Android の消耗デバイス

Android の着用 Smartwatch など、Android を着用デバイスがある場合は、エミュレーターを使用する代わりにこのデバイスでアプリをデバッグできます。 消耗デバイスと開発については、次を参照してください。[着用デバイス上でのデバッグ](~/android/wear/deploy-test/debug-on-device.md)です。


## <a name="create-your-first-android-wear-app"></a>最初の消耗の Android アプリを作成します。

以下の[ハロー, 着用](~/android/wear/get-started/hello-wear.md)を初めて watch アプリをビルドする方法です。


## <a name="packaging-your-app"></a>アプリをパッケージ化

Android の消耗アプリケーションは、コンパニオン Android フォン アプリで常に配布されます。 

メインの Android アプリへの参照として Android を着用アプリケーションを追加すると、Android を着用プロジェクトは自動的と見なされますが必要なすべての XML とメタデータを生成します。 さらに、パッケージとバージョン番号は一致するには Google Play アプリを簡単に出荷できるようにすることを確認します。 

消耗アプリをパッケージ化の詳細については、次を参照してください。[パッケージングの使用](~/android/wear/deploy-test/packaging.md)です。


## <a name="related-links"></a>関連リンク

- [SkeletonWear (サンプル)](https://developer.xamarin.com/samples/SkeletonWear/)
