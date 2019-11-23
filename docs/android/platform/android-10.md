---
title: Android 10 の機能
description: Xamarin を使用して Android 10 用アプリの開発を開始する方法について説明します。
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: c19c9e5bd279824ea2d3e4e9f88857388f786a2c
ms.sourcegitcommit: b11dc46a9ba23483195e923de88cbef173730087
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612277"
---
# <a name="android-10-with-xamarin"></a>Android 10 と Xamarin

_Xamarin を使用して Android 10 用アプリの開発を開始する方法について説明します。_

Android 10 を Google から入手できるようになりました。 このリリースでは、多数の新機能と Api が提供されており、その多くは最新の Android デバイスで新しいハードウェア機能を利用するために必要です。

![Android 10 のロゴ](~/android/platform/android-10-images/android10_black.png)

この記事は、Android 10 用の Xamarin Android アプリの開発を開始する際に役立つように構成されています。 ここでは、必要な更新プログラムをインストールし、SDK を構成し、テスト用のエミュレーターまたはデバイスを準備する方法について説明します。 また、Android 10 の新機能の概要を説明し、いくつかの主要な Android 10 機能の使用方法を示すソースコード例を示します。

Android 10.0 では、Android 10 がサポートされています。 Android 10 向けの Xamarin のサポートの詳細については、「 [Xamarin android 10.0 のリリースノート](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0)」を参照してください。

## <a name="requirements"></a>要件

Xamarin ベースのアプリで Android 10 の機能を使用するには、次の一覧が必要です。

- **Visual studio** -visual studio 2019 をお勧めします。 Windows update On Visual Studio 2019 バージョン16.3 以降。 MacOS で、Visual Studio 2019 for Mac バージョン8.3 以降に更新します。
- **Xamarin.** android 10.0 以降を Visual Studio と共にインストールする必要があります (xamarin android は、Windows 上の .net ワークロード**を使用したモバイル開発**の一部として自動的にインストールされ、 **Visual Studio for Mac インストーラー**の一部としてインストールされます)
- **Java Developer Kit** -Xamarin Android 10.0 の開発には JDK 8 が必要です。 Microsoft の OpenJDK の配布は、Visual Studio の一部として自動的にインストールされます。
- **Android SDK** -Android SDK API 29 は Android SDK Manager を使用してインストールする必要があります。

## <a name="get-started"></a>作業開始

Android 10 アプリの開発を Xamarin Android で開始するには、初めての Android 10 プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. **Visual Studio 2019 をお勧め**します。 Visual Studio 2019 バージョン16.3 以降に更新します。 Visual Studio for Mac 2019 を使用している場合は、Visual Studio 2019 for Mac バージョン8.3 以降に更新してください。
2. SDK Manager を使用して、 **Android 10 (API 29)** パッケージとツールをインストールします。
    - Android 10 (API 29) SDK プラットフォーム
    - Android 10 (API 29) システムイメージ
    - Android SDK ビルドツール 29.0.0 +
    - Android SDK プラットフォーム-ツール 29.0.0 +
    - Android Emulator 29.0.0 +
3. Android 10.0 を対象とする新しい Xamarin Android プロジェクトを作成します。
4. Android 10 アプリをテストするためのエミュレーターまたはデバイスを構成します。

これらの各手順を以下に説明します。

### <a name="update-visual-studio"></a>Visual Studio を更新する

Xamarin を使用して Android 10 アプリをビルドする場合は、Visual Studio 2019 をお勧めします。

Visual studio 2019 を使用している場合は、Visual Studio 2019 バージョン16.3 以降に更新してください (手順については、「 [Visual studio 2019 を最新のリリースに更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio)する」を参照してください)。 MacOS で、Mac 8.3 以降の Visual Studio 2019 に更新します (手順については、「 [Visual studio 2019 For mac を最新のリリースに更新](https://docs.microsoft.com/visualstudio/mac/update)する」を参照してください)。

### <a name="install-the-android-sdk"></a>Android SDK のインストール

Xamarin Android 10.0 を使用してプロジェクトを作成するには、最初に Android SDK マネージャーを使用して Android 10 用の SDK プラットフォーム **(API レベル 29)** をインストールする必要があります。

1. SDK Manager を起動します。 Visual Studio で、 **[Tools > Android > Android SDK Manager]** をクリックします。 Visual Studio for Mac で、 **[ツール > SDK Manager]** をクリックします。
2. 右下隅にある歯車アイコンをクリックし、 **[リポジトリ > Google (サポートされていません)]** を選択します。

    ![Android SDK Manager リポジトリの選択](~/android/platform/android-10-images/sdkrepository.png)

3. **[プラットフォーム]** タブで**Android SDK Platform 29**と表示されている**Android 10 SDK Platform**パッケージをインストールします (sdk Manager の使用方法の詳細については、「 [Android SDK セットアップ](https://docs.microsoft.com/xamarin/android/get-started/installation/android-sdk)」を参照してください)。

    ![Android SDK Manager の [プラットフォーム] タブ](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Xamarin Android プロジェクトを作成する

新しい Xamarin. Android プロジェクトを作成します。 Xamarin を使用した Android 開発を初めて使用する場合は、「 [Hello, android](https://docs.microsoft.com/xamarin/android/get-started/hello-android/index) 」を参照して、xamarin android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、バージョン設定を Android 10.0 以降を対象とするように構成する必要があります。 たとえば、Android 10 のプロジェクトを対象にするには、プロジェクトのターゲットの Android API レベルを**android 10.0 (API 29)** に構成する必要があります。 これには、**ターゲットフレームワークバージョン**と**ターゲット Android SDK バージョン**の両方が API 29 以降に含まれます。 Android API レベルの構成の詳細については、「 [ANDROID Api レベル](https://docs.microsoft.com/xamarin/android/app-fundamentals/android-api-levels)について」を参照してください。

![Xamarin Android のターゲットフレームワーク](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>デバイスまたはエミュレーターを構成する

ピクセルなどの物理デバイスを使用している場合は、電話の設定で `System` > `System update` > `Check for update` に移動して、Android 10 の更新プログラムをダウンロードできます。 デバイスをフラッシュする場合は、デバイスへの[ファクトリイメージ](https://developers.google.com/android/images)または[OTA イメージ](https://developers.google.com/android/ota)のフラッシュに関する手順を参照してください。

エミュレーターを使用している場合は、API レベル29用の仮想デバイスを作成し、x86 ベースのイメージを選択します。 Android Device Manager を使用した仮想デバイスの作成と管理の詳細については、「 [Android Device Manager を使用した仮想デバイスの管理](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager)」を参照してください。 テストとデバッグに Android Emulator を使用する方法の詳細については、「 [Android Emulator でのデバッグ](https://docs.microsoft.com/xamarin/android/deploy-test/debugging/debug-on-emulator)」を参照してください。

## <a name="new-features"></a>新機能

Android 10 では、さまざまな新機能が導入されています。 これらの新機能の一部は、最新の Android デバイスによって提供される新しいハードウェア機能を利用することを目的としていますが、Android ユーザーエクスペリエンスをさらに強化するように設計されています。

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Android 10 の機能と Api を使用してアプリを強化する

次に、Android 10 について説明し、使用可能な [新しい機能と api](https://developer.android.com/preview/api-overview.html)  について説明します。 ここでは、基本的な機能をいくつか紹介します。

すべてのアプリに対して、次の機能をお勧めします。

- **ダークテーマ:**   [ダークテーマ](https://developer.android.com/preview/features/darktheme) を追加するか、 [強制濃色](https://developer.android.com/preview/features/darktheme#force_dark)を有効にすることによって、システム全体のダークテーマを有効にするユーザーに一貫したエクスペリエンスを確保します。

![ダーク テーマ](~/android/platform/android-10-images/darktheme.png)

- エッジツーエッジに移動し、カスタムジェスチャがシステムのナビゲーションジェスチャを補完するようにすることで、**アプリの  [gestural ナビゲーション](https://developer.android.com/preview/features/gesturalnav)をサポート** します。

![ジェスチャのナビゲーション](~/android/platform/android-10-images/gesturenavigation.png)

- **Optimize for foldables:**   [foldables 用に最適化する](https://developer.android.com/preview/features/foldables)ことにより、現在の革新的なデバイス上でシームレスなエッジツーエッジのエクスペリエンスを実現します。

![たたみ込み](~/android/platform/android-10-images/foldable.png)

アプリに関連する場合は、次の機能をお勧めします。

- **より対話的な通知:**  通知にメッセージが含まれている場合は、 [通知の返信とアクション](https://developer.android.com/preview/features#smart-suggestions)を有効にし、ユーザーがすぐにアクションを実行できるように します。
- **生体認証の向上:**  生体認証を使用する場合は、最新のデバイスで指紋認証をサポートするための推奨される方法として、 [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt)に移動します。
- 強化された**記録:** キャプションまたはゲームプレイの記録をサポートする 、 [オーディオ再生のキャプチャ](https://developer.android.com/preview/features/playback-capture)を有効にします。 これは、より多くのユーザーにリーチし、アプリをより使いやすくするための優れた方法です。
- **より優れたコーデック:**  メディアアプリの場合は、ビデオストリーミングには [AV1](https://en.wikipedia.org/wiki/AV1) を、高ダイナミックレンジのビデオでは [HDR10 +](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) を試してください。 Speech および音楽ストリーミングの場合は、 [Opus](http://opus-codec.org/)encoding を使用できます。また、ミュージシャンでは、 [ネイティブ MIDI API](https://developer.android.com/preview/features/midi) が使用可能です。
- **ネットワーク api の向上 :** アプリが wi-fi 経由で IoT デバイスを管理している場合は、構成、ダウンロード、印刷などの機能に対して、新しい [ネットワーク接続 api](https://developer.android.com/preview/features#peer2peer) 試してみてください。

これらは、Android 10 の多くの新機能と Api のほんの一部です。 すべてを表示するには、 [開発者向けの Android 10 サイト](https://developer.android.com/about/versions/10/highlights)にアクセスしてください。

## <a name="behavior-changes"></a>動作の変更

ターゲットの Android バージョンが API レベル29に設定されている場合、上で説明した新機能を実装していない場合でも、アプリの動作に影響を与えるプラットフォームの変更がいくつかあります。 これらの変更の簡単な概要を次に示します。

- Android プラットフォームでは、アプリの[安定性と互換性を確保するために、アプリで android 10 で使用できる非 SDK インターフェイスが制限されるようになりました](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions)。
- [共有メモリが変更されまし](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory)た。
- [Android ランタイム & AOT の正確性](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat)。
- [全画面のインテントのアクセス許可は `USE_FULL_SCREEN_INTENT`を要求する必要があり](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents)ます。
- [Foldables のサポート](https://developer.android.com/about/versions/10/behavior-changes-10#foldables)。

## <a name="summary"></a>概要

この記事では、Android 10 について紹介し、android 10 を使用した Xamarin Android 開発用の最新のツールとパッケージをインストールして構成する方法について説明しました。 Android 10 で使用できる主な機能の概要について説明しました。 Android 10 用のアプリの作成を開始する際に役立つ API ドキュメントと Android 開発者向けのトピックへのリンクが含まれています。 また、既存のアプリに影響する可能性がある、最も重要な Android 10 の動作変更も強調表示されています。

## <a name="related-links"></a>関連リンク

- [Android 10](https://developer.android.com/about/versions/10)
