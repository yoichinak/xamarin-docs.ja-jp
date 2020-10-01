---
title: Marshmallow の機能
description: この記事は、Xamarin.Android を使用して Android 6.0 Marshmallow 用アプリの開発を始める場合に役立ちます。
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 52c44efa335d81004ce2e3dbf0d9160640118bea
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453546"
---
# <a name="marshmallow-features"></a>Marshmallow の機能

_この記事は、Xamarin.Android を使用して Android 6.0 Marshmallow 用アプリの開発を始める場合に役立ちます。_

この記事では、Android 6.0 Marshmallow の新機能の概要、Android Marshmallow での開発用に Xamarin.Android を準備する方法について説明し、Xamarin.Android アプリで新しい Android Marshmallow 機能を使用する方法を示すサンプル アプリケーションへのリンクを提供します。 

## <a name="overview"></a>概要

[Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html) は、Android Lollipop の後継の主要な Android リリースです。
Xamarin.Android では Android Marshmallow がサポートされ、次のものが含まれます。

- **API 23 および Android 6.0 のバインド** &ndash; Android 6.0 では、後述する新機能用の新しい API が多数追加されました。API レベル 23 をターゲットにすると、これらの API を Xamarin.Android アプリで使用できるようになります。 Android 6.0 API の詳細については、[Android 6.0 API](https://developer.android.com/preview/api-overview.html) に関するページを参照してください。 

[![Marshmallow を実行しているタブレットと携帯電話のヒーローの画像](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow リリースは主に "光沢と品質" に焦点を当てていますが、Xamarin.Android 開発者にとって興味深い多くの新機能も提供しています。 これには次の機能があります。 

- **実行時アクセス許可** &ndash; この機能強化により、ユーザーは実行時に状況に応じてセキュリティ アクセス許可を承認できるようになりました。 

- **認証の改善** &ndash; Android Marshmallow 以降、アプリでは指紋センサーを使用してユーザーを認証できるようになりました。また、新しい "*資格情報の確認*" 機能によりパスワードを入力する必要性が最小限に抑えられます。 

- **アプリ リンク** &ndash; この機能を使用して、アプリを Web ドメインに自動的に関連付けることで、**App Chooser** をポップアップ表示する必要がなくなります。 

- **直接共有** &ndash; ユーザーがすばやく直感的に共有できるようになる "*直接共有ターゲット*" を定義できます。ユーザーはこの機能を使って他のアプリとコンテンツを共有できます。 

- **音声操作** &ndash; この新しい API を使用すると、会話音声機能をアプリに組み込むことができます。 

- **4K ディスプレイ モード** &ndash; Android Marshmallow では、アプリから、サポートするハードウェアに対して 4K ディスプレイ解像度を要求できます。 

- **新しいオーディオ機能** &ndash; Marshmallow 以降、Android では MIDI プロトコルがサポートされるようになりました。 また、デジタル オーディオのキャプチャおよび再生オブジェクトを作成するための新しいクラスも用意され、オーディオと入力デバイスを関連付けるための新しい API フックが追加されました。 

- **新しいビデオ機能** &ndash; Marshmallow には、アプリでオーディオ ストリームとビデオ ストリームを同期してレンダリングできるようになる新しいクラスが追加されました。このクラスでは、動的な再生レートもサポートされています。 

- **Android for Work** &ndash; Marshmallow には、企業所有のシングルユーザー デバイス向けに強化されたコントロールが含まれています。 デバイス所有者によるアプリのサイレント インストールとアンインストール、システム更新プログラムの自動受け入れ、証明書管理の向上、データ使用状況の追跡、アクセス許可の管理、および作業状態の通知をサポートしています。 

- **マテリアル デザイン サポート ライブラリ** &ndash; 新しい "*デザイン サポート ライブラリ*" には、マテリアル デザインの外観をアプリに簡単に組み込むことができるデザイン コンポーネントとパターンが用意されています。 

さらに、多くの Android ライブラリのコア更新プログラムが Android M と共にリリースされました。これらの更新プログラムによって Android M と以前のバージョンの Android の両方に新機能が提供されます。

さらに、多くの Android ライブラリのコア更新プログラムが Android Marshmallow と共にリリースされました。これらの更新プログラムによって Android Marshmallow と以前のバージョンの Android の両方に新機能が提供されます。 この記事では、Android Marshmallow を使用してアプリの構築を開始する方法を説明します。また、Android 6.0 の新機能の概要についても説明します。 

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android Marshmallow の機能を使用するには、次のものが必要です。 

- **Xamarin.Android** &ndash; Visual Studio または Xamarin Studio に Xamarin.Android 5.1.7.12 以降をインストールして、構成する必要があります。

- **Visual Studio for Mac** または **Visual Studio** &ndash; Visual Studio for Mac を使用している場合は、バージョン 5.9.7.22 以降が必要です。 Visual Studio を使用している場合は、バージョン 3.11.1537 以降の Visual Studio 用 Xamarin ツールが必要です。 

- **Android SDK** &ndash; Android SDK マネージャーを使用して Android SDK 6.0 (API 23) 以降をインストールする必要があります。

- **Java Developer Kit** &ndash; API レベル 24 以降向けに開発する場合 (JDK 1.8 では、Marshmallow を含め、24 より前の API レベルもサポートされています)、Xamarin.Android には [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 以降が必要です。 カスタム コントロールまたは Forms プレビューアーを使用する場合は、64 ビット版の JDK 1.8 が必要です。

API レベル 23 以下向けにのみ開発している場合は、[JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) を引き続き使用することができます。 

## <a name="getting-started"></a>作業の開始

Android Marshmallow と Xamarin.Android を使い始めるには、Android Marshmallow プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。 

1. **[安定]** チャネルから最新の Xamarin 更新プログラムをインストールします。 

2. Android 6.0 Marshmallow SDK パッケージとツールをインストールします。

3. Android 6.0 Marshmallow (API レベル 23) をターゲットとする新しい Xamarin.Android プロジェクトを作成します。 

4. Android Marshmallow 用のエミュレーターまたはデバイスを構成します。

これらの各手順を以下のセクションで説明します。

### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムをインストールする

Android 6.0 Marshmallow のサポートが含まれるように Xamarin を更新するには、更新チャネルを **[安定]** に変更し、すべての更新プログラムをインストールします。 更新チャネルから更新プログラムをインストールする方法の詳細については、[更新チャネルの変更](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)に関するページを参照してください。 

### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK をインストールする

Android Marshmallow 用の Xamarin.Android プロジェクトを作成するには、まず Android SDK マネージャーを使用して Android 6.0 SDK をインストールする必要があります。

- Android SDK マネージャーを起動し (Visual Studio for Mac では **[ツール] > [SDK マネージャー]** を使用し、Visual Studio では **[ツール] > [Android] > [Android SDK マネージャー]** を使用します)、最新の Android SDK Tools をインストールします。

    [![Android SDK マネージャーでの Android SDK Tools の選択](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

- また、最新の **Android 6.0** SDK パッケージをインストールします。

    [![Android SDK マネージャーでの Android 6.0 SDK パッケージの選択](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools リビジョン 24.3.4 以降をインストールする必要があります。
Android SDK マネージャーを使用した Android 6.0 SDK のインストールの詳細については、[SDK マネージャー](https://developer.android.com/tools/help/sdk-manager.html)に関するページを参照してください。

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始する

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android の開発を初めて行う場合は、「[Hello, Android](~/android/get-started/hello-android/index.md)」を参照して、Android プロジェクトの作成について学習してください。 

Android プロジェクトを作成するときは、Android 6.0 MarshMallow 以降をターゲットとするようにバージョン設定を構成する必要があります。 プロジェクトのターゲットを Marshmallow にするには、プロジェクトを **[API level 23 (Xamarin.Android v6.0 Support)]\(API レベル 23 (Xamarin.Android v6.0 のサポート)\)** に構成する必要があります。 Android API レベルの構成の詳細については、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください。

### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成する

エミュレーターを使用している場合は、Android AVD マネージャーを開始し、次の設定を使用して新しいデバイスを作成します。

- デバイス:Nexus 5、6、または 9。
- ターゲット:Android 6.0 - API レベル 23
- ABI: x86

たとえば、次の仮想デバイスは、Nexus 5 をエミュレートするように構成されています。

[![Nexus 5 デバイス、Android 6.0 ターゲット、Intel Atom (x86) を使用して AVD を構成する](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5、6、または 9 などの物理デバイスを使用している場合は、Android Marshmallow のプレビュー イメージをインストールできます。 デバイスを Android Marshmallow に更新する方法の詳細については、[ハードウェアのシステム イメージ](https://developer.android.com/preview/download.html#images)に関するページを参照してください。

## <a name="new-features"></a>新機能

Android Marshmallow で導入された変更の多くは、Android ユーザー エクスペリエンスの向上、パフォーマンスの向上と、バグの修正に重点を置いています。 一方、Marshmallow では、Android プラットフォームの基礎にいくつかの広範な変更も導入されました。 以下のセクションでは、これらの機能強化について説明します。また、アプリで新しい Android Marshmallow 機能を使い始める際に役立つリンクを提供します。 

### <a name="runtime-permissions"></a>実行時アクセス許可

Android のアクセス許可システムは、Android Lollipop 以降、大幅に最適化され、簡素化されています。 Android Marshmallow では、ユーザーはインストール時ではなく実行時に個別にアクセス許可を付与します。 Android Marshmallow 以降でこの機能をサポートするには、実行時に (アクセス許可が必要な場所のコンテキストで) ユーザーにアクセス許可を求めるようにアプリを設計します。 この変更により、アプリのインストールとアップグレードのプロセスが合理化されるため、ユーザーはすぐにアプリを使い始めることができます。 

Xamarin.Android アプリで実行時アクセス許可を実装する方法の詳細 (コード例を含む) については、[Android Marshmallow での実行時アクセス許可の要求](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/)に関するページを参照してください。
Xamarin には、Android Marshmallow (およびそれ以降) で実行時アクセス許可がどのように機能するかを示すサンプル アプリ [RuntimePermissions](/samples/xamarin/monodroid-samples/android-m-runtimepermissions) も用意されています。

このサンプル アプリでは、次のことを示します。

- 実行時にアクセス許可を確認および要求する方法。
- Android M デバイスのアクセス許可を宣言する方法。

このサンプル アプリを使用するには:

1. **[カメラ]** ボタンまたは **[連絡先]** ボタンをタップして、アクセス許可の要求ダイアログを表示します。
2. [カメラ] または [連絡先] のフラグメントを表示するアクセス許可を付与します。

Android Marshmallow の新しい実行時アクセス許可機能の詳細については、[システム アクセス許可の使用](https://developer.android.com/preview/features/runtime-permissions.html)に関するページを参照してください。

### <a name="authentication-enhancements"></a>認証の機能強化

Android Marshmallow には、パスワードの必要性をなくすために役立つ 2 つの認証拡張機能が用意されています。

- **指紋認証** &ndash; 指紋スキャンを使用してユーザーを認証します。

- **資格情報の確認** &ndash; デバイスのロックが解除された期間に基づいてユーザーを認証します。

次に説明するリンクとサンプル アプリは、これらの新機能を理解するために役立ちます。

#### <a name="fingerprint-authentication"></a>指紋認証

指紋スキャン ハードウェアをサポートするデバイスでは、新しい `FingerPrintManager` クラスを使用してユーザーを認証できます。
Android Marshmallow の指紋認証機能の詳細については、[指紋認証](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)に関するページを参照してください。

Xamarin には、登録された指紋を使用してアプリ内のユーザーを認証する方法を示すサンプル アプリ [FingerprintDialog](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) が用意されています。

このサンプル アプリを使用するには:

1. **[Purchase]\(購入\)** ボタンをタッチして、指紋認証ダイアログを開きます。
2. 登録した指紋をスキャンして認証します。

このサンプル アプリには指紋リーダーが搭載されたデバイスが必要であることに注意してください。
このアプリには、指紋 (またはパスワード) は保存されません。

#### <a name="voice-interactions"></a>音声操作

Android Marshmallow で導入された新しい音声操作機能により、アプリのユーザーは音声を使用してアクションを確認し、オプションの一覧から選択できるようになります。 音声操作の詳細については、「[Overview of the Voice Interaction API (Voice Interaction API の概要)](https://developers.google.com/voice-actions/interaction/)」を参照してください。 

Xamarin.Android アプリに音声操作を実装する方法の詳細 (コード例を含む) については、[音声操作を使用した Android アプリへの会話の追加](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/)に関するページを参照してください。
Xamarin.Android アプリで Voice Interaction API を使用する方法を示すサンプル アプリ [Voice Interactions](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions) が用意されています。

#### <a name="confirm-credential"></a>資格情報の確認

Android Marshmallow の新しい "*資格情報の確認*" 機能を使用すると、デバイスのロックが解除された期間に基づいて認証することにより、ユーザーがアプリ固有のパスワードを記憶して入力する必要がなくなります。
これを行うには、`KeyGenerator` の新しい `SetUserAuthenticationValidityDurationSeconds` メソッドを使用します。 `KeyGuardManager` の `CreateConfirmDeviceCredentialIntent` メソッドを使用して、アプリ内からユーザーを再認証します。 Android Marshmallow のこの新機能の詳細については、[資格情報の確認](https://developer.android.com/preview/api-overview.html#confirm-credential)に関するページを参照してください。

Xamarin には、アプリでデバイスの資格情報 (PIN、パターン、パスワードなど) を使用する方法を示すサンプル アプリ [ConfirmCredential](/samples/xamarin/monodroid-samples/android-m-confirmcredential) が用意されています

このサンプル アプリを使用するには:

1. デバイスでセキュリティで保護されたロック画面を設定します ( **[セキュリティ] > [セキュリティ] > [画面ロック]** )。
2. **[Purchase]\(購入\)** ボタンをタップし、セキュリティで保護されたロック画面の資格情報を確認します。

### <a name="chrome-custom-tabs"></a>Chrome のカスタム タブ

アプリ開発者には、ユーザーが URL をタップしたときに、`WebView` に基づいてアプリでブラウザーを起動するか、アプリ内ブラウザーを使用するか、という選択肢の問題があります。 いずれの方法にも課題があります。ブラウザーの起動は、カスタマイズできない高負荷のコンテキスト スイッチですが、`WebView` は状態をブラウザーと共有しません。 また、`WebView` を使用すると、追加のメンテナンスのオーバーヘッドが生じる可能性があります。 

"*Chrome のカスタム タブ*" を使用すると、ユーザーがアプリを離れることなく、Chrome の機能を使用して Web サイトを簡単かつエレガントに表示できます。 この機能により、アプリでユーザーの Web エクスペリエンスをより詳細に制御できるようになります。`WebView` に頼ることなく、ネイティブ コンテンツと Web コンテンツ間の移行をよりシームレスに実行できます。 また、アプリで以下をカスタマイズして、Chrome の外観に影響を与えることもできます。 

- ツールバーの色

- アニメーションの開始と終了

- Chrome ツールバーとオーバーフロー メニューのカスタム アクション

- Chrome のプリスタートとコンテンツのプリフェッチ (読み込みを高速化するため)

Xamarin.Android アプリでこの機能を使用するには、[Android Support Custom Tabs ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)をダウンロードしてインストールします。
この機能の詳細については、[Chrome のカスタム タブ](https://developer.chrome.com/multidevice/android/customtabs)に関するページを参照してください。

### <a name="material-design-support-library"></a>マテリアル デザイン サポート ライブラリ

Android Lollipop では、Android のエクスペリエンスを更新する新しいデザイン言語として "[マテリアル デザイン](https://www.google.com/design/spec/material-design/introduction.html)" を導入しました (Xamarin.Android アプリでのマテリアル デザインの使用については、「[素材のテーマ](~/android/user-interface/material-theme.md)」を参照してください)。 Google は、Android Marshmallow で "*Android デザイン サポート ライブラリ*" を導入し、アプリ開発者がマテリアル デザインの外観を採用しやすくなりました。 このライブラリには、次のコンポーネントが含まれています。

- **CoordinatorLayout** &ndash; 新しい `CoordinatorLayout` ウィジェットは `FrameLayout` に似ていますが、より強力です。 `CoordinatorLayout` を子ビューのコンテナーまたは最上位のレイアウトとして使用できます。また、他のビューとの相対位置にビューを固定するために使用できる `layout_anchor` 属性が用意されています。

- **折りたたみツールバー** &ndash; 新しい `CollapsingToolbarLayout` は、`Toolbar` のラッパーである折りたたみアプリ バーです ("*アプリ バー*" は、以前は "*アクション バー*" と呼ばれていたものです)。

- **フローティング アクション ボタン** &ndash; アプリのインターフェイスの主要なアクションを示す丸いボタン。

- **テキスト編集用のフローティング ラベル** &ndash; (`EditText` をラップする) 新しい `TextInputLayout` ウィジェット を使用して、ユーザーがテキストを入力するときにヒントが非表示の場合は、フローティング ラベルを表示します。

- **ナビゲーション ビュー** &ndash; 新しい `NavigationView` ウィジェットは、ユーザーが操作しやすい方法でナビゲーション ドロワーを使用するために役立ちます。

- **スナックバー** &ndash; 新しい `SnackBar` ウィジェットは、画面の下部に短いメッセージが表示される軽量のフィードバック メカニズムです (トーストに似ています)。画面上の他のすべての要素の上に表示されます。

- **マテリアルのタブ** &ndash; 新しい `TabLayout` ウィジェットには、アプリで最上位のナビゲーションを実装する方法としてタブを表示するための水平レイアウトが用意されています。

Xamarin.Android アプリで[デザイン サポート ライブラリ](https://developer.android.com/tools/support-library/features.html#design)を活用するには、Xamarin [Xamarin Support Library Design](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet パッケージをダウンロードしてインストールします。

Xamarin.Android アプリでのマテリアル デザイン サポート ライブラリ の使用の詳細 (コード例を含む) については、[Android サポート デザイン ライブラリを使用した美しいマテリアル デザイン](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/)に関するページを参照してください。
Xamarin には、Xamarin.Android &ndash; [Cheesesquare](/samples/xamarin/monodroid-samples/android50-cheesesquare) 上の新しい Android デザイン ライブラリの使用例を示すサンプル アプリが用意されています。
このサンプルは、デザイン ライブラリの次の機能の使用例を示しています。

- 折りたたみツールバー
- フローティング アクション ボタン
- 表示の固定
- NavigationView
- スナックバー

デザイン ライブラリの詳細については、Android 開発者ブログの「[Android Design Support Library (Android デザイン サポート ライブラリ)](https://android-developers.googleblog.com/2015/05/android-design-support-library.html)」を参照してください。

### <a name="additional-library-updates"></a>その他のライブラリの更新

Android Marshmallow に加えて、Google はいくつかのコア Android ライブラリに関連する更新プログラムを発表しました。 Xamarin では、プレビュー リリースのいくつかの NuGet パッケージでこれらの更新プログラムに対する Xamarin.Android のサポートを提供しています。 

- [Google Play 開発者サービス](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; Google Play 開発者サービスの最新バージョンには、新しい *App Invites* 機能が追加され、ユーザーは自分のアプリを友人と共有できます。 この機能の詳細については、[Google の App Invites を使用したアプリのリーチの拡張](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)に関するページを参照してください。 

- [Android サポート ライブラリ](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; これらの NuGet には、ライブラリ API にのみ使用できる機能だけでなく、Android フレームワーク API の下位互換バージョンが用意されています。 

- [Android Wearable ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)&ndash; この NuGet には Google Play 開発者サービスのバインディングが含まれています。 最新バージョンの Wearable ライブラリでは、Android Wear プラットフォームに新機能 (カスタム アプリの操作の簡易化を含む) が追加されました。 

## <a name="summary"></a>まとめ

この記事では、Android Marshmallow について紹介し、Android Marshmallow での Xamarin.Android 開発用に最新のツールとパッケージをインストールして構成する方法について説明しました。 また、Xamarin.Android 開発用の最も魅力的で新しい Android Marshmallow 機能の概要についても説明しました。

## <a name="related-links"></a>関連リンク

- [Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK を入手する](https://developer.android.com/sdk/index.html#Other)
- [機能の概要](https://developer.android.com/preview/api-overview.html)
- [リリース ノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1.99/index.md)
- [RuntimePermissions (サンプル)](/samples/xamarin/monodroid-samples/android-m-runtimepermissions)
- [ConfirmCredential (サンプル)](/samples/xamarin/monodroid-samples/android-m-confirmcredential)
- [FingerprintDialog (サンプル)](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)