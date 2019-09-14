---
title: '摩耗 OS のインストールとセットアップ '
description: この記事では、Android の磨耗開発のためにコンピューターとデバイスを準備するために必要なインストール手順と構成の詳細について説明します。 この記事を終了すると、Visual Studio for Mac や Microsoft Visual Studio に統合された、作業中の Xamarin. Android のインストールが可能になります。また、最初の Xamarin. Android 磨耗アプリケーションの構築を開始する準備が整います。
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 190e38e525288dbc2b7f6bd9e785c723c80c6271
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762232"
---
# <a name="setup-and-installation"></a>セットアップとインストール

_この記事では、Android の磨耗開発のためにコンピューターとデバイスを準備するために必要なインストール手順と構成の詳細について説明します。この記事を終了すると、Visual Studio for Mac や Microsoft Visual Studio に統合された、作業中の Xamarin. Android のインストールが可能になります。また、最初の Xamarin. Android 磨耗アプリケーションの構築を開始する準備が整います。_

## <a name="requirements"></a>必要条件

Xamarin ベースの Android Wear アプリを作成するには、次のものが必要です。

- **Visual Studio または Visual Studio for Mac**&ndash; Visual Studio 2017 Community 以降が必要です。

- **Xamarin android** &ndash; 4.17 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。

- **Android SDK** Android SDK 5.0.1 (API 21) 以降を Android SDK Manager を使用してインストールする必要があります。

- **Java Developer Kit**API レベル24以上を開発している場合、Xamarin Android 開発では[jdk 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)が必要です (jdk 1.8 では、24より前の api レベルもサポートされています)。 &ndash;

特に API レベル23以前を開発している場合は、 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)を使用し続けることができます。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

## <a name="installation"></a>インストール

Xamarin Android をインストールしたら、次の手順を実行して、Android Wear アプリをビルドしてテストする準備ができていることを確認します。 

1. 必要な Android SDK とツールをインストールします。
2. テストデバイスを構成します。
3. 初めての Android Wear アプリを作成します。

これらの手順については、次のセクションで説明します。

### <a name="install-android-sdk-and-tools"></a>Android SDK とツールのインストール 

**Android SDK マネージャー**を起動します。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio で Android SDK マネージャーを起動する方法](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Visual Studio for Mac で Android SDK マネージャーを起動する方法](installation-images/xs/sdk-menu.png)

-----

次の Android SDK とツールがインストールされていることを確認します。

- Android SDK Tools v 24.0.0 以上、
- Android 4.4 W (API20)、または
- Android 5.0.1 (API21) 以降。

最新の sdk とツールがインストールされていない場合は、必要な sdk ツール*と*api ビットをダウンロードします (次に示すように&ndash; 、api の選択が表示されていることを確認するために少しスクロールする必要があります)。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android 5.0.1 コンポーネントの有効化に関する SDK Manager のスクリーンショットの例](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android 4.4 および5.0.1 コンポーネントを有効にする例の SDK Manager のスクリーンショット](installation-images/xs/sdk-select.png)

-----

## <a name="configuration"></a>構成

アプリのテストを使用する前に、Android Wear エミュレーターまたは実際の Android Wear デバイスを構成する必要があります。 

### <a name="android-wear-emulator"></a>Android Wear エミュレーター

Android Wear エミュレーターを使用するには、 **Google Emulator Manager**を使用して Android Wear Android 仮想デバイス (avd) を構成する必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio から Android Emulator マネージャーを起動する方法](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Visual Studio for Mac から Android Emulator マネージャーを起動する方法](installation-images/xs/emulator-menu.png)

-----

Android Wear エミュレーターの設定の詳細については、「[エミュレーターでの Android Wear のデバッグ](~/android/wear/deploy-test/debug-on-emulator.md)」を参照してください。

### <a name="android-wear-device"></a>Android Wear デバイス

Android Wear Smartwatch などの Android Wear デバイスがある場合は、エミュレーターを使用する代わりに、このデバイスでアプリをデバッグすることができます。 Wear デバイスを使用した開発の詳細については、「[Wear デバイスでのデバッグ](~/android/wear/deploy-test/debug-on-device.md)」を参照してください。

## <a name="create-your-first-android-wear-app"></a>初めての Android Wear アプリを作成する

[Hello, Wear](~/android/wear/get-started/hello-wear.md)の指示に従って、初めての watch アプリを作成します。

## <a name="packaging-your-app"></a>アプリのパッケージ化

Android Wear アプリケーションは、常にコンパニオン Android phone アプリと共に配布されます。 

Android Wear アプリケーションをメインの Android アプリケーションへの参照として追加すると、自動的に Android Wear プロジェクトと見なされ、必要なすべての XML とメタデータが自動的に生成されます。 また、パッケージとバージョン番号が一致していることを確認して、アプリを Google Play に簡単に配布できます。 

Wear アプリのパッケージ化の詳細については、「[パッケージングの](~/android/wear/deploy-test/packaging.md)使用」を参照してください。

## <a name="related-links"></a>関連リンク

- [SkeletonWear (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)
