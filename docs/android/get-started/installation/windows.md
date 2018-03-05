---
title: "Windows のインストール"
description: "このガイドでは、Windows で Visual Studio 用の Xamarin.Android をインストールする手順と、最初の Xamarin.Android アプリケーションをビルドするための Xamarin.Android の構成方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 10cfb790fca30b67faaa8f7720a358ec34c25663
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="windows-installation"></a>Windows のインストール

_このガイドでは、Windows で Visual Studio 用の Xamarin.Android をインストールする手順と、最初の Xamarin.Android アプリケーションをビルドするための Xamarin.Android の構成方法について説明します。_

<a name="overview" />

## <a name="overview"></a>概要

Xamarin は現在、追加料金なしで Visual Studio のすべてのエディションに含まれ、別個のライセンスは必要ないため、Visual Studio インストーラーを使用して、Xamarin.Android ツールをダウンロードしてインストールすることができます 
(以前のバージョンの Xamarin.Android では必要だった手動のインストールとライセンス手順は不要になりました)。このガイドでは、次のことを学習します。

-   Java Development Kit、Android SDK、および Android NDK のカスタムの場所を構成する方法。

-   Android SDK マネージャーを起動して、追加の Android SDK コンポーネントをダウンロードしてインストールする方法。

-   デバッグとテストのために Android デバイスまたはエミュレーターを準備する方法。

-   最初の Xamarin.Android アプリ プロジェクトを作成する方法。

このガイドの終わりには、作業中の Xamarin.Android インストールが Visual Studio に統合され、最初の Xamarin.Android アプリケーションのビルドを開始する準備ができます。

## <a name="installation"></a>インストール

Windows での Visual Studio を使用するための Xamarin のインストールについては、[Windows インストール](~/cross-platform/get-started/installation/windows.md)のガイドをご覧ください。

<a name="configuration" />

## <a name="configuration"></a>構成

Xamarin.Android では Java Development Kit (JDK) と Android SDK を使用して、アプリをビルドします。 インストール中に、Visual Studio インストーラーは既定の場所にこれらのツールを配置し、適切なパス構成で開発環境を構成します。 これらの場所は、**[ツール]、[オプション]、[Xamarin]、[Android 設定]** の順にクリックして表示し、変更することができます。

![Xamarin Android の設定ダイアログのスクリーン ショット](windows-images/07-settings.png)

ほとんどのユーザーは、これらの既定の場所を変更しなくても使用できます。 ただし、これらのツールのカスタムの場所で Visual Studio を構成することもできます (Java JDK、Android SDK、または NDK を別の場所にインストールした場合など)。 変更するパスの横にある **[変更]** をクリックして、新しい場所に移動します。

Xamarin.Android は [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) を使用します。これは、API レベル 24 以上で開発する場合に必要となります (JDK 8 は 23 以下の API レベルもサポートしています)。 API レベル 23 以下のみで開発している場合は、[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) を引き続き使用することができます。

> [!IMPORTANT]
> **注:** Xamarin.Android は JDK 9 をサポートしていません。

<a name="sdk_manager" />

### <a name="android-sdk-manager"></a>Android SDK Manager

Android では複数の Android API レベル設定を使用して、さまざまなバージョンの Android 間のアプリの互換性を確認します (Android API レベルの詳細については、「[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)」 (Android API レベルについて) を参照してください)。
対象となる Android API レベルに応じて、追加の Android SDK コンポーネントをダウンロードしてインストールする必要がある場合があります。 さらに、Android SDK で提供されるオプションのツールとエミュレーター イメージをインストールする必要がある場合があります。 そのためには、**Android SDK マネージャー**を使用します。 **[ツール]、[Android]、[Android SDK マネージャー]** の順にクリックして、**Android SDK マネージャー**を起動できます。

[![Android SDK マネージャーを起動する方法](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png)

既定では、Visual Studio は Google Android SDK マネージャーをインストールします。

![Google Android SDK マネージャーの例を示すスクリーン ショット](windows-images/09-google-sdk-manager.png)

Google Android SDK マネージャーを使用して、バージョン 25.2.3 までの Android SDK Tools パッケージをインストールすることができます。 ただし、最新バージョンの Android SDK Tools パッケージを使用する必要がある場合は、Visual Studio 用の Xamarin Android SDK マネージャー プラグインをインストールする必要があります (Visual Studio Marketplace から入手可能)。 25.2.3 バージョンの Android SDK Tools パッケージでは Google のスタンドアロン SDK マネージャーが推奨されなくなったため、これは必要です。 

Xamarin Android SDK Manager の使用の詳細については、「[Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください。

<a name="emulator" />

### <a name="android-emulator"></a>Android エミュレーター

テストに使用する物理的な Android デバイスがない場合は、Android エミュレーターを使用してアプリをテストできます。 Google Android エミュレーターの詳細については、「[Android SDK エミュレーター](~/android/deploy-test/debugging/android-sdk-emulator/index.md)」を参照してください。

Google Android エミュレーターは、他のエミュレーターで使用される仮想化テクノロジと競合する可能性のある Intel の HAXM (Hardware Accelerated Execution Manager) を使用します。 3 つの主な仮想化テクノロジを以下に示します。

-   **Hyper-V** (Visual Studio Emulator for Android と Windows Phone エミュレーターで使用) 

-   **Virtual Box** (Genymotion で使用)

-   **Intel HAXM** (Google Android SDK エミュレーターで使用) 

開発用コンピューターの CPU では一度にサポートできる仮想化テクノロジは 1 つのみであるため、開発用コンピューター上では 1 つのみを使用することをお勧めします。

<a name="device" />

### <a name="android-device"></a>Android デバイス

テストに使用する物理的な Android デバイスがある場合は、開発用に設定する良いタイミングです。 「[Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)」 (開発用のデバイスの設定) を参照して、開発用に Android デバイスを構成してから、Xamarin.Android アプリケーションの実行とデバッグのためにコンピューターに接続してください。


<a name="create_app" />

## <a name="create-an-application"></a>アプリケーションの作成

Xamarin.Android をインストールしたので、Visual Studio を起動して新しいプロジェクトを作成することができます。 **[ファイル]、[新規]、[プロジェクト]** の順にクリックして、アプリの作成を開始します。

![新しいプロジェクトを作成する方法](windows-images/10-new-project.png)

**[新しいプロジェクト]** ダイアログの **[テンプレート]** で **[Android]** を選択し、右側のウィンドウで **[空のアプリ (Android)]** をクリックします。 アプリの名前 (以下のスクリーン ショットでは、アプリの名前は **MyApp**) を入力してから **[OK]** をクリックします。

[![空の Android アプリを作成している、[新しいプロジェクト] ダイアログのスクリーン ショット](windows-images/11-first-app-sml.png)](windows-images/11-first-app.png)

これで完了です。 これで、Xamarin.Android を使用して Android アプリケーションを作成する準備ができました。


<a name="summary" />

## <a name="summary"></a>まとめ

この記事では、Windows で Xamarin.Android プラットフォームを設定してインストールする方法、(必要に応じて) カスタムの Java JDK と Android SDK インストールの場所で Visual Studio を構成する方法、SDK マネージャーを起動して追加の Android SDK コンポーネントをインストールする方法、Android デバイスまたはエミュレーターを設定する方法、および最初のアプリケーションのビルドを開始する方法を学習しました。

次の手順では、「[Hello, Android](~/android/get-started/hello-android/index.md)」チュートリアルを参照して、作業用 Xamarin.Android アプリを作成する方法を学習します。


## <a name="related-links"></a>関連リンク

- [Visual Studio のダウンロード](https://www.visualstudio.com/vs/)
- [Visual Studio Tools for Xamarin のインストール](~/cross-platform/get-started/installation/windows.md)
- [システム要件](~/cross-platform/get-started/requirements.md)
- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [Android SDK エミュレーター](~/android/get-started/installation/android-emulator/index.md)
- [開発用のデバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)
