---
title: 'Wear OS onXamarin.Android のインストールとセットアップ '
description: この記事では、インストール手順と、コンピューターとデバイスの Android Wear の開発の準備に必要な構成の詳細について説明します。 この記事の目的は、Mac や Microsoft Visual Studio での Xamarin.Android Wear のインストールが Visual Studio に統合作業する必要があり、最初の Xamarin.Android Wear アプリケーションの構築を開始する準備ができます。
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: af9be54b4509f7202618d9d68210eb534f63ccbf
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671638"
---
# <a name="setup-and-installation"></a>セットアップとインストール

_この記事では、インストール手順と、コンピューターとデバイスの Android Wear の開発の準備に必要な構成の詳細について説明します。この記事の目的は、Mac や Microsoft Visual Studio での Xamarin.Android Wear のインストールが Visual Studio に統合作業する必要があり、最初の Xamarin.Android Wear アプリケーションの構築を開始する準備ができます。_

## <a name="requirements"></a>必要条件

Xamarin 基盤の Android Wear アプリを作成する、次が必要。

-   **Visual Studio または Visual Studio for Mac** &ndash;する Visual Studio 2015 Professional、Visual Studio を使用している場合、または場合以降が必要です。

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-   **Android SDK** -Android SDK 5.0.1 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin Android 開発が必要です[JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)開発 API レベル 24 以上の場合 (JDK 1.8 もサポートしている API レベル 24 より前)。

引き続き使用できます[JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)またはそれ以前の API レベル 23 ののみで開発する場合。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

## <a name="installation"></a>インストール

Xamarin.Android をインストールした後は、次の手順を実行するため、ビルドし、Android Wear アプリをテストする準備ができました。 

1.  必要な Android SDK とツールをインストールします。
2.  テスト デバイスを構成します。
3.  最初の Android Wear アプリを作成します。

次のセクションでは、次の手順を説明します。


### <a name="install-android-sdk-and-tools"></a>Android SDK とツールをインストールします。 

起動、 **Android SDK Manager**: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio での Android SDK マネージャーを起動する方法](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac を Visual Studio での Android SDK Manager を起動する方法](installation-images/xs/sdk-menu.png)

-----


次の Android SDK とツールをインストールできることを確認します。

* Android SDK Tools v 24.0.0 またはそれ以降、および
* Android 4.4W (API20) または
* Android 5.0.1 (API21) またはそれ以降。

最新の SDK とツールがインストールされていない場合は、必要な SDK ツールをダウンロード*と*API ビット (そこには、少しスクロールする必要があります&ndash;API の選択範囲を次に示します)。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android 5.0.1 を有効にする例を SDK Manager のスクリーン ショットのコンポーネント](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android 4.4 および 5.0.1 有効にする例を SDK Manager のスクリーン ショットのコンポーネント](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>構成

使用するには、アプリのテスト、Android Wear エミュレーターまたは実際の Android Wear デバイスを構成する必要があります。 


### <a name="android-wear-emulator"></a>Android Wear エミュレーター

Android Wear エミュレーターを使用するを使用して、Android Wear Android Virtual Device (AVD) を構成する必要があります、 **Google エミュレーター マネージャー**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio から Android エミュレーター マネージャーを起動する方法](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac を Visual Studio から Android エミュレーター マネージャーを起動する方法](installation-images/xs/emulator-menu.png)

-----

Android Wear エミュレーターの設定に関する詳細については、次を参照してください。[エミュレーターで Android Wear をデバッグ](~/android/wear/deploy-test/debug-on-emulator.md)します。


### <a name="android-wear-device"></a>Android Wear デバイス

Android Wear デバイス、Android Wear スマートウォッチなどを使っている場合は、エミュレーターを使用する代わりにこのデバイスでアプリをデバッグできます。 Wear デバイスでの開発方法の詳細については、次を参照してください。 [Wear デバイスでのデバッグ](~/android/wear/deploy-test/debug-on-device.md)します。


## <a name="create-your-first-android-wear-app"></a>最初の Android Wear アプリを作成します。

に従って、[こんにちは、Wear](~/android/wear/get-started/hello-wear.md)最初 watch アプリを構築する手順。


## <a name="packaging-your-app"></a>アプリのパッケージ化

Android wear のアプリケーションは、付属の Android フォン アプリで常に分散されます。 

Android Wear アプリケーションをメインの Android アプリへの参照として追加する場合は自動的に、Android Wear のプロジェクトと見なされます、必要なすべての XML とメタデータを生成します。 さらに、Google Play にアプリを簡単に出荷できるようにパッケージとバージョン番号が一致することを確認します。 

Wear アプリをパッケージ化の詳細については、次を参照してください。[パッケージ化操作](~/android/wear/deploy-test/packaging.md)します。


## <a name="related-links"></a>関連リンク

- [SkeletonWear (サンプル)](https://developer.xamarin.com/samples/SkeletonWear/)
