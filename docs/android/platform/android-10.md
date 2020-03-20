---
title: Android 10 の機能
description: Xamarin.Android を使用して Android 10 用アプリの開発を始める方法。
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: c19c9e5bd279824ea2d3e4e9f88857388f786a2c
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73612277"
---
# <a name="android-10-with-xamarin"></a>Xamarin を使用する Android 10

"_Xamarin.Android を使用して Android 10 用アプリの開発を始める方法。_ "

Android 10 を Google から入手できるようになりました。 このリリースでは、多数の新機能と API が提供されており、その多くは最新の Android デバイスで新しいハードウェア機能を利用するために必要です。

![Android 10 のロゴ](~/android/platform/android-10-images/android10_black.png)

この記事は、Android 10 用 Xamarin.Android アプリの開発を始める際に役立つように構成されています。 必要な更新プログラムのインストール方法、SDK の構成方法、テスト用のエミュレーターまたはデバイスの準備方法について説明します。 また、Android 10 の新機能の概要について説明し、Android 10 の主要な機能のいくつかを使用する方法を示すサンプル ソース コードを提供します。

Xamarin.Android 10.0 では、Android 10 のサポートが提供されます。 Xamarin.Android での Android 10 のサポートの詳細については、[Xamarin.Android 10.0 のリリース ノート](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0)を参照してください。

## <a name="requirements"></a>要件

Xamarin ベースのアプリで Android 10 の機能を使用するために必要なリストを以下に示します。

- **Visual Studio** - Visual Studio 2019 をお勧めします。 Windows 上では、Visual Studio 2019 バージョン 16.3 以降に更新します。 macOS 上では、Visual Studio 2019 for Mac バージョン 8.3 以降に更新します。
- **Xamarin.Android** - Xamarin.Android 10.0 以降を Visual Studio と共にインストールする必要があります (Xamarin.Android は、Windows 上で **[.NET によるモバイル開発]** ワークロードの一部として、また、**Visual Studio for Mac インストーラー**の一部として自動的にインストールされます)
- **Java Developer Kit** - Xamarin.Android 10.0 の開発には JDK 8 が必要です。 Microsoft の OpenJDK ディストリビューションは、Visual Studio の一部として自動的にインストールされます。
- **Android SDK** - Android SDK マネージャーを使用して、Android SDK API 29 をインストールする必要があります。

## <a name="get-started"></a>作業開始

Xamarin.Android を使用して Android 10 アプリの開発を始めるには、最初の Android 10 プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. **Visual Studio 2019 をお勧めします**。 Visual Studio 2019 バージョン 16.3 以降に更新します。 Visual Studio for Mac 2019 を使用している場合は、Visual Studio 2019 for Mac バージョン 8.3 以降に更新します。
2. SDK マネージャーを使用して、**Android 10 (API 29)** パッケージとツールをインストールします。
    - Android 10 (API 29) SDK Platform
    - Android 10 (API 29) System Image
    - Android SDK Build-Tools 29.0.0 以降
    - Android SDK Platform-Tools 29.0.0 以降
    - Android Emulator 29.0.0 以降
3. Android 10.0 をターゲットとする新しい Xamarin.Android プロジェクトを作成します。
4. Android 10 アプリをテストするためにエミュレーターまたはデバイスを構成します。

これらの各手順については、以下で説明します。

### <a name="update-visual-studio"></a>Visual Studio を更新する

Xamarin を使用して Android 10 アプリをビルドする場合は、Visual Studio 2019 をお勧めします。

Visual Studio 2019 を使用している場合は、Visual Studio 2019 バージョン 16.3 以降に更新します (手順については、[Visual Studio 2019 の最新リリースへの更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio)に関するページを参照してください)。 macOS 上では、Visual Studio 2019 for Mac 8.3 以降に更新します (手順については、[Visual Studio for Mac 2019 の最新リリースへの更新](https://docs.microsoft.com/visualstudio/mac/update)に関するページを参照してください)。

### <a name="install-the-android-sdk"></a>Android SDK をインストールする

Xamarin.Android 10.0 でプロジェクトを作成するには、最初に Android SDK マネージャーを使用して、**Android 10 (API レベル 29)** 用の SDK プラットフォームをインストールする必要があります。

1. SDK マネージャーを起動します。 Visual Studio では、 **[ツール] > [Android] > [Android SDK マネージャー]** の順にクリックします。 Visual Studio for Mac では、 **[ツール] > [SDK マネージャー]** の順にクリックします。
2. 右下隅の歯車アイコンをクリックして、 **[リポジトリ] > [Google (サポート対象外)]** の順に選択します。

    ![Android SDK マネージャーのリポジトリの選択](~/android/platform/android-10-images/sdkrepository.png)

3. **Android 10 SDK Platform** パッケージをインストールします。これらは、 **[プラットフォーム]** タブで **[Android SDK Platform 29]** としてリストされます (SDK マネージャーの使用の詳細については、[Android SDK セットアップ](https://docs.microsoft.com/xamarin/android/get-started/installation/android-sdk)に関するページを参照してください)。

    ![Android SDK マネージャーの [プラットフォーム] タブ](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを作成する

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android の開発を初めて行う場合は、「[Hello, Android](https://docs.microsoft.com/xamarin/android/get-started/hello-android/index)」を参照して、Xamarin.Android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、Android 10.0 以降をターゲットとするようにバージョン設定を構成する必要があります。 たとえば、Android 10 をプロジェクトのターゲットとするには、プロジェクトのターゲット Android API レベルを **Android 10.0 (API 29)** に構成する必要があります。 これには、**ターゲット フレームワークのバージョン**と**ターゲット Android SDK バージョン**の両方を API 29 以降にすることが含まれます。 Android API レベルの構成の詳細については、「[Android API レベルの理解](https://docs.microsoft.com/xamarin/android/app-fundamentals/android-api-levels)」を参照してください。

![Xamarin Android のターゲット フレームワーク](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>デバイスまたはエミュレーターを構成する

Pixel などの物理デバイスを使用している場合は、携帯電話の設定で [システム] > [システム アップデート] > [アップデートを確認] の順に移動して、Android 10 のアップデートをダウンロードできます。`System``System update``Check for update` デバイスをフラッシュする場合は、デバイスへの[ファクトリ イメージ](https://developers.google.com/android/images)または [OTA イメージ](https://developers.google.com/android/ota)のフラッシュに関する手順を参照してください。

エミュレーターを使用している場合は、API レベル 29 の仮想デバイスを作成し、x86 ベースのイメージを選択します。 Android Device Manager を使用する仮想デバイスの作成および管理の詳細については、「[Android Device Manager による仮想デバイスの管理](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager)」を参照してください。 テストとデバッグでの Android Emulator の使用の詳細については、[Android Emulator でのデバッグ](https://docs.microsoft.com/xamarin/android/deploy-test/debugging/debug-on-emulator)に関するページを参照してください。

## <a name="new-features"></a>新機能

Android 10 には、さまざまな新機能が導入されています。 これらの新機能には、最新の Android デバイスによって提供される新しいハードウェア機能を利用することを目的にしたものと、Android のユーザー エクスペリエンスをさらに強化するために設計されたものがあります。

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Android 10 の機能と API を使用してアプリを強化する

準備ができたら、次は Android 10 について掘り下げ、使用できる [新機能と API](https://developer.android.com/preview/api-overview.html)  について学習します。 ここでは、使用を開始する上位の機能をいくつか示します。

すべてのアプリに対して、これらの機能をお勧めします。

- **ダーク テーマ:**   [[ダーク テーマ]](https://developer.android.com/preview/features/darktheme) を追加するか、 [[フォース ダーク]](https://developer.android.com/preview/features/darktheme#force_dark) を有効にすることで、システム全体でダーク テーマを有効にするユーザーに対して一貫したエクスペリエンスを確保します。

![ダーク テーマ](~/android/platform/android-10-images/darktheme.png)

- エッジツーエッジに移動し、カスタムジェスチャがシステムのナビゲーションジェスチャを補完するようにすることで、**アプリの  [gestural ナビゲーション](https://developer.android.com/preview/features/gesturalnav)をサポート** します。

![ジェスチャ ナビゲーション](~/android/platform/android-10-images/gesturenavigation.png)

- **フォルダブルの最適化:**   [フォルダブルを最適化する](https://developer.android.com/preview/features/foldables)ことで、現在の革新的なデバイスでシームレスなエッジツーエッジのエクスペリエンスを提供します。

![フォルダブル](~/android/platform/android-10-images/foldable.png)

アプリに関連する場合は、これらの機能をお勧めします。

- **より対話的な通知:**  通知にメッセージを含める場合は、ユーザーの注意を引き、ユーザーがすぐにアクションを実行できるように、 [通知内で提案された返信とアクション](https://developer.android.com/preview/features#smart-suggestions) を有効にします。
- **生体認証の向上:**  生体認証を使用する場合は、最新のデバイスで指紋認証をサポートするための推奨される方法である、 [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt) に移動します。
- **強化された記録:**  キャプションまたはゲームプレイの記録をサポートするには、 [オーディオ再生キャプチャ](https://developer.android.com/preview/features/playback-capture)を有効にします。 これは、より多くのユーザーにリーチし、アプリにさらにアクセスしやすくするための優れた方法です。
- **コーデックの向上:**  メディア アプリについては、ビデオ ストリーミングの場合は [AV1](https://en.wikipedia.org/wiki/AV1) 、ハイ ダイナミック レンジ ビデオの場合は [HDR10+](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) をお試しください。 音声および音楽ストリーミングについては、 [Opus](http://opus-codec.org/) エンコードを使用できます。また、ミュージシャンの場合は、 [ネイティブ MIDI API](https://developer.android.com/preview/features/midi) を使用できます。
- **ネットワーク API の向上:**  アプリで IoT デバイスを Wi-Fi 経由で管理している場合は、新しい [ネットワーク接続 API](https://developer.android.com/preview/features#peer2peer) で、構成、ダウンロード、印刷などの機能をお試しください。

これらは、Android 10 での多くの新しい機能と API のほんの一部です。 これらすべてを確認する場合は、 [開発者向けの Android 10 のサイト](https://developer.android.com/about/versions/10/highlights)にアクセスしてください。

## <a name="behavior-changes"></a>動作の変更

ターゲット Android バージョンが API レベル 29 に設定されている場合は、上記の新機能を実装していない場合でも、アプリの動作に影響する可能性のあるプラットフォームの変更がいくつかあります。 これらの変更の簡単な概要を以下にリストします。

- [Android プラットフォームでは、アプリの安定性と互換性を確保するために、アプリで Android 10 で使用できる非 SDK インターフェイスが制限されるようになりました](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions)。
- [共有メモリが変更されました](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory)。
- [Android ランタイムと AOT の正確性](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat)。
- [全画面表示インテントに関する権限を要求する必要がある`USE_FULL_SCREEN_INTENT`](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents)。
- [折りたたみ式のサポート](https://developer.android.com/about/versions/10/behavior-changes-10#foldables)。

## <a name="summary"></a>まとめ

この記事では、Android 10 について紹介し、Android 10 での Xamarin.Android 開発用に最新のツールとパッケージをインストールして構成する方法について説明しました。 Android 10 で使用できる主な機能の概要について説明しました。 Android 10 用アプリの作成を開始するのに役立つ、API ドキュメントと Android 開発者向けトピックへのリンクが含まれていました。 また、既存のアプリに影響する可能性のある、最も重要な Android 10 の動作の変更点について重点的に説明しました。

## <a name="related-links"></a>関連リンク

- [Android 10](https://developer.android.com/about/versions/10)
