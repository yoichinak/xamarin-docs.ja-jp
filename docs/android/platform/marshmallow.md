---
title: Marshmallow 機能
description: この記事が Xamarin.Android を使用して、Android 6.0 Marshmallow 用のアプリを開発するを使用して作業を開始します。
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d2150e18a377d61a2e79fabfc845f57cfab8a5c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774949"
---
# <a name="marshmallow-features"></a>Marshmallow 機能

_この記事が Xamarin.Android を使用して、Android 6.0 Marshmallow 用のアプリを開発するを使用して作業を開始します。_

この記事の Android 6.0 Marshmallow の新機能の概要を示します、Android Marshmallow の開発では、Xamarin.Android を準備する方法について説明します、および新しい Android Marshmallow を使用する方法を示すサンプル アプリケーションへのリンクを示しますXamarin.Android アプリで機能します。 


## <a name="overview"></a>概要

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)、次の主要な Android は、Android のロリポップ後にリリースします。
Xamarin.Android は Android Marshmallow をサポートしておりが含まれています。

-   **API 23/Android 6.0 バインド** &ndash; Android 6.0 は、次に示す新機能の多くの新しい Api を追加します。 これらの Api は、API レベル 23 の対象とする Xamarin.Android アプリで利用できます。 Android 6.0 Api の詳細については、次を参照してください。 [Android 6.0 Api](http://developer.android.com/preview/api-overview.html)です。 

[![タブレットと携帯電話 Marshmallow を実行しているのヒーローの画像](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow リリースは主に「ポーランド語と品質」に焦点を当てていますも、Xamarin.Android 開発者にとって関心のある多くの新機能を提供します。 これには次の機能があります。 

-   **実行時アクセス権**&ndash;この機能強化により、実行時に、個別にセキュリティのアクセス許可を承認するユーザー。 

-   **認証の機能強化** &ndash; Android Marshmallow から始めて、アプリ今すぐ指紋センサー認証に使用できるユーザー、および新しい*資格情報を確認*機能には、入力の必要性が最小限に抑えられますパスワード。 

-   **アプリのリンク**&ndash;の必要性を排除するこの機能を使用する、**アプリ選択**ポップアップが自動的にアプリをドメインに関連付けることによってです。 

-   **共有を直接**&ndash;を定義できます*共有ターゲットを直接*する共有を行うため迅速かつ直感的なユーザー以外の場合はこの機能により、他のアプリを使用してコンテンツを共有する uers です。 

-   **相互作用を音声**&ndash;この新しい API では、アプリに会話の音声機能を構築することができます。 

-   **4 K 表示モード** &ndash; Android Marshmallow でアプリを要求できるこれをサポートするハードウェア上のディスプレイ解像度を 4 K です。 

-   **オーディオの新機能** &ndash; Marshmallow 以降、Android ようになりました、MIDI プロトコルです。 デジタル オーディオ キャプチャおよび再生オブジェクトを作成する新しいクラスも用意されていて、およびオーディオ入力デバイスの関連付けの新しい API フックを提供します。 

-   **ビデオの新機能** &ndash; Marshmallow がアプリを支援する新しいクラスがオーディオおよびビデオ ストリームを同期の表示を提供します。 このクラスはも動的再生レートのサポートを提供します。 

-   **作業の android** &ndash; Marshmallow には会社が所有する、シングル ユーザーのデバイスに対する拡張されたコントロールが含まれています。 サイレント インストールをサポートし、アンインストールすると、デバイスの所有者によってアプリ、システムの更新プログラム、強化された証明書の管理、データ使用状況の追跡、権限の管理、および作業の状態通知の自動承認。 

-   **素材のデザインのサポート ライブラリ**&ndash;新しい*デザイン サポート ライブラリ*デザイン コンポーネントとアプリにマテリアル デザイン ルック アンド フィールを構築するためが容易パターンを示します。 

さらに、多くのコア Android ライブラリの更新プログラムは、Android M から、と共にリリースされたし、これらの更新プログラムは、Android M と以前のバージョンの Android の両方の新機能を提供します。

さらに、多くのコア Android ライブラリの更新が Android Marshmallow、と共にリリースされたし、これらの更新プログラムが Android Marshmallow と以前のバージョンの Android の両方の新機能を提供します。 Android Marshmallow でのアプリの構築を開始する方法を説明し、Android 6.0 で強調表示、新しい機能の概要を示します。 

## <a name="requirements"></a>要件

Xamarin ベースのアプリで Android Marshmallow の新機能を使用する、次が必要。 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 or later must be installed and configured with either Visual Studio or Xamarin Studio.

-   **Visual Studio for Mac**または**Visual Studio** &ndash; Mac、5.9.7.22 のバージョンの Visual Studio を使用するか以降が必要です。 バージョン 3.11.1537、Visual Studio を使用している場合は、以降の Visual Studio の Xamarin ツールが必要です。 

-   **Android SDK** &ndash; Android SDK 6.0 (API 23) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin.Android 必要[JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)または API レベルの場合 24 開発している場合は、以降、または大きい (JDK 1.8 もサポートしている API レベル Marshmallow を含め、24 より前)。 カスタム コントロールまたはフォーム プレビュー用のプログラムを使用している場合、JDK 1.8 の 64 ビット バージョンが必要です。

使用を続行できます[JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API level 23 専用の開発または以前の場合。 


## <a name="getting-started"></a>作業の開始

Xamarin.Android Android Marshmallow の使用を開始するには、ダウンロードして、Android Marshmallow プロジェクトを作成する前に、最新のツールと SDK のパッケージをインストールする必要があります。 

1.  最新の Xamarin の更新プログラムをインストール、**安定した**チャネル。 

2.  Android 6.0 Marshmallow SDK パッケージとツールをインストールします。

3.  Android 6.0 Marshmallow (API レベル 23) を対象とする新しい Xamarin.Android プロジェクトを作成します。 

4.  Android Marshmallow のエミュレーターまたはデバイスを構成します。

次のセクションでは、これらの各手順について説明します。


### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムをインストールします。

更新するには Xamarin Android 6.0 Marshmallow のサポートが含まれるように変更の更新チャネルを**安定した**し、すべての更新プログラムをインストールします。 更新プログラムのチャネルから更新プログラムのインストールの詳細については、次を参照してください。[チャンネルを更新変更](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)です。 


### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK をインストールします。

Android Marshmallow の Xamarin.Android プロジェクトを作成するには、Android SDK Manager を使用して、Android 6.0 SDK をインストールする必要があります。

-   Android SDK Manager を開始 (Mac を Visual Studio で使用**ツール > SDK Manager**; Visual Studio で、使用**ツール > Android > Android SDK Manager**) し、最新の Android SDK ツールをインストールします。

    [![Android SDK Manager で Android SDK ツールを選択します。](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   また、最新版をインストール**Android 6.0** SDK パッケージ。

    [![Android SDK Manager で Android 6.0 SDK パッケージを選択します。](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK ツール リビジョン 24.3.4 をインストールする必要がありますまたはそれ以降。
Android SDK Manager を使用して、Android 6.0 SDK をインストールする方法の詳細については、次を参照してください。 [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html)です。



### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android 開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md) Android プロジェクトの作成について学習します。 

Android プロジェクトを作成するときに、Android 6.0 MarshMallow をターゲットにバージョン設定を構成する必要があります。 Marshmallow 用プロジェクトを対象とするには、プロジェクトを構成する必要があります**API level 23 (Xamarin.Android v6.0 サポート)** です。 Android API レベル レベルの構成に関する詳細は、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。



### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成します。

エミュレーターを使用している場合は、Android AVD マネージャーを起動し、次の設定を使用して新しいデバイスを作成します。

-   デバイス: Nexus 5、6、または 9 です。
-   ターゲット: Android 6.0 - API Level 23
-   ABI: x86

たとえば、この仮想デバイスは、Nexus 5 をエミュレートするために構成されます。

[![Nexus 5 デバイス、Android 6.0 ターゲット、および Intel Atom (x86) を使用して、AVD を構成します。](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5 などの物理デバイスを使用している場合は、6、または 9 をインストールできます Android Marshmallow のプレビュー イメージ。 Android Marshmallow に、デバイスの更新の詳細については、次を参照してください。[ハードウェア システムのイメージ](http://developer.android.com/preview/download.html#images)です。



## <a name="new-features"></a>新機能

Android Marshmallow で導入された変更の多くは、Android ユーザー エクスペリエンスを向上させるのパフォーマンスの向上、およびバグを修正して集中しています。 ただし、Marshmallow によって大幅な変更を Android プラットフォームの基本とも説明します。 次のセクションでは、これらの拡張機能を強調表示し、Android Marshmallow の新機能を使用して、アプリ内で開始するのに役立つリンクを提供します。 



### <a name="runtime-permissions"></a>ランタイムのアクセス許可

Android アクセス許可システムは大幅に最適化し、Android ロリポップ以降簡略化されました。 Android Marshmallow は、ユーザーは、インストール時刻をケースバイ ケースではなく、実行時にアクセス許可を付与します。 Android Marshmallow 以降は、この機能をサポートするためには、アプリのアクセス許可 (アクセス許可が必要なのコンテキスト) で実行時にユーザー入力を求めるをデザインします。 この変更により、簡単にアプリの使用を開始することをインストールして、アプリのアップグレードのプロセスを効率化するためにすぐにします。 

参照してください[Android Marshmallow のランタイム アクセス許可の要求](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/)(コード例を含む) に関する詳細 Xamarin.Android アプリでのランタイム アクセス許可を実装します。
Xamarin には、Android Marshmallow で (以降) の実行時の権限のしくみについて説明するサンプル アプリも用意されています: [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)です。

このサンプル アプリを次に示します。

-   確認する方法と実行時に権限を要求します。
-   Android M デバイスのアクセス許可を宣言する方法です。

このサンプル アプリを使用します。

1.  タップして、**カメラ**または**連絡先**アクセス許可を表示するボタンがダイアログを要求します。
2.  カメラまたは連絡先のフラグメントを表示するアクセス許可を付与します。

Android Marshmallow のランタイム アクセス許可の新機能についての詳細については、次を参照してください。[システム権限を持つ作業](https://developer.android.com/preview/features/runtime-permissions.html)です。



### <a name="authentication-enhancements"></a>認証の拡張機能

Android Marshmallow には、パスワードが不要に役立つ 2 つの認証の拡張機能が含まれています。

-   **認証の指紋**&ndash;指紋スキャンを使用してユーザーを認証します。

-   **資格情報を確認**&ndash;期間、デバイスがロック解除されましたに基づいてユーザーを認証します。

リンクとサンプル アプリで次に説明に、これらの新しい機能の理解するのに役立ちます。


#### <a name="fingerprint-authentication"></a>指紋認証

指紋がハードウェアのスキャンをサポートするデバイスを使用して、新しい`FingerPrintManager`ユーザーを認証するクラス。
Android Marshmallow の指紋認証機能の詳細については、次を参照してください。[指紋認証](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)です。

Xamarin は、登録済みの指紋を使用して、アプリでユーザーを認証する方法について説明するサンプル アプリ: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)です。

このサンプル アプリを使用します。

1.  タッチ、**購買**指紋認証ダイアログを開くボタンをクリックします。
2.  登録済みの指紋認証でスキャンします。

このサンプル アプリ必要指紋読み取り装置のデバイスであることに注意してください。
このアプリでは、指紋認証 (パスワード) は保存されません。



#### <a name="voice-interactions"></a>音声の相互作用

Android Marshmallow で導入された新しい音声の対話機能は、アクションを確認し、オプションの一覧から選択する音声を使用して、アプリのユーザーを使用できます。 音声の相互作用の詳細については、次を参照してください。[音声の相互作用 API の概要](https://developers.google.com/voice-actions/interaction/)です。 

参照してください[音声さまざまな相互 Android アプリに追加して、メッセージ交換](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/)(コード例を含む) に関する詳細 Xamarin.Android アプリでの音声の相互作用の実装です。
サンプル アプリを利用できる Xamarin.Android アプリで、音声の相互作用の API を使用する方法を示す:[音声の相互作用](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions)です。



#### <a name="confirm-credential"></a>資格情報を確認します。

使用して、新しい*資格情報を確認*機能 Android Marshmallow からに記憶でき、どのくらいの期間、デバイスがロック解除されましたに基づいてを認証することによってアプリ固有のパスワードの入力を持つユーザーを解放することができます。
これを行うには、使用する新しい`SetUserAuthenticationValidityDurationSeconds`のメソッド、`KeyGenerator`です。 使用して、`KeyGuardManager`の`CreateConfirmDeviceCredentialIntent`再から、アプリ内でのユーザーを認証する方法です。 Android Marshmallow のこの新機能の詳細については、次を参照してください。[資格情報の確認](https://developer.android.com/preview/api-overview.html#confirm-credential)です。

Xamarin アプリでデバイスの資格情報 (暗証番号 (pin)、パターンでは、パスワードなど) を使用する方法について説明するサンプル アプリの提供: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

このサンプル アプリを使用します。

1.  デバイス上のセキュリティで保護されたロック画面の設定 (**Secure > セキュリティ > Screenlock**)。
2.  タップして、**購買**ボタンをクリックし、セキュリティで保護されたロック画面の資格情報を確認します。



### <a name="chrome-custom-tabs"></a>Chrome のカスタム タブ

アプリの開発者、ユーザーが URL をタップしたときに、選択に直面して: アプリのブラウザーを起動またはに基づいてアプリ内のブラウザーを使用できます、`WebView`です。 両方のオプションの課題を&ndash;中に、カスタマイズ可能なない高いコンテキストの切り替えは、ブラウザーを起動`WebView`s はブラウザーを使用して状態を共有できません。 使用も、 `WebView`s は余分なメンテナンスのオーバーヘッドを追加できます。 

*ユーザー設定 タブのクロム*簡単かつ洗練された方法は、Chrome のパワーを備えた web サイトを表示しなくてもアプリのままにして、ユーザーにできるようになります。 この機能は、アプリがユーザーの web エクスペリエンスです。 より詳細に制御ネイティブの間の遷移を行うとシームレスなコンテンツを web に頼ることがなく、`WebView`です。 アプリは、Chrome の検索し、次のカスタマイズで操作にも影響します。 

-   ツールバーの色

-   入力し、アニメーションの終了

-   Chrome のツールバーとオーバーフロー メニューにカスタム アクション

-   Chrome の開始前とコンテンツをプリフェッチ (の高速な読み込み)

Xamarin.Android アプリでこの機能を活用をダウンロードしてインストール、 [Android のサポートのカスタム タブ ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)です。
この機能の詳細については、次を参照してください。 [Chrome カスタム タブ](https://developer.chrome.com/multidevice/android/customtabs)です。



### <a name="material-design-support-library"></a>素材のデザインのサポート ライブラリ

Android のロリポップが導入された[マテリアル デザイン](http://www.google.com/design/spec/material-design/introduction.html)Android エクスペリエンスを更新する新しいデザイン言語として (を参照してください[マテリアル テーマ](~/android/user-interface/material-theme.md)Xamarin.Android アプリ内の素材のデザインの使用方法について)。 Android Marshmallow で Google が導入されました、 *Android のデザインのサポート ライブラリ*マテリアルを採用する開発者はアプリを容易にできるように外観を設計します。 このライブラリには、次のコンポーネントが含まれています。

-   **CoordinatorLayout** &ndash;新しい`CoordinatorLayout`ウィジェットに似ていますより強力な`FrameLayout`します。 使用することができます`CoordinatorLayout`または最上位のレイアウトとして子ビューのコンテナーとしては、`layout_anchor`ビューを他のビューの基準としたアンカー ビューに使用できる属性です。

-   **ツールバーの折りたたみ**&ndash;新しい`CollapsingToolbarLayout`折りたたみアプリ バーのラッパーであるの`Toolbar`します。 (なお、*アプリ バー*新機能が以前と呼ばれますが、*操作バー*)。

-   **アクション ボタンを浮動**&ndash;アプリのインターフェイスでプライマリ アクションを示す丸いボタンをクリックします。

-   **テキストの編集のラベルを浮動**&ndash;では、新しい`TextInputLayout`ウィジェット (ラップ`EditText`) ヒントが非表示になるユーザーのテキストを入力するときに、浮動小数点のラベルを表示します。

-   **ナビゲーション ビュー** &ndash;新しい`NavigationView`ウィジェットを使用して、ユーザーを移動する簡単な方法で、ナビゲーションのドロアーを使用できます。

-   **Snackbar** &ndash;新しい`SnackBar`ウィジェットは軽量のフィードバック メカニズム (トーストに似ています)、画面の他の要素をすべて表示される、画面の下部にある、簡単なメッセージを表示します。

-   **材料タブ**&ndash;新しい`TabLayout`ウィジェットでは、アプリに最上位のナビゲーションを実装する方法としてのタブの表示の水平レイアウトを提供します。

活用するために、[デザイン サポート ライブラリ](http://developer.android.com/tools/support-library/features.html#design)Xamarin.Android アプリをダウンロードして、Xamarin をインストール[Xamarin サポート ライブラリ デザイン](https://www.nuget.org/packages/Xamarin.Android.Support.Design/)NuGet パッケージです。

参照してください[Android サポート デザイン ライブラリと美しいマテリアル デザイン](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/)詳細 (コード例を含む) について Xamarin.Android アプリで資料デザイン サポート ライブラリを使用します。
Xamarin Xamarin.Android 上の新しいデザインの Android ライブラリをデモするサンプル アプリの提供&ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare)です。
このサンプルでは、デザイン ライブラリの次の機能を使用します。


-   ツールバーの折りたたみ
-   浮動小数点動作設定ボタン
-   アンカーのビュー
-   NavigationView
-   Snackbar

デザイン ライブラリの詳細については、次を参照してください。 [Android のデザインのサポート ライブラリ](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html)Android 開発者のブログでします。


### <a name="additional-library-updates"></a>その他のライブラリの更新

Android Marshmallow、に加えて Google は、いくつかの中核となる Android ライブラリに関連する更新プログラムを発表しました。 Xamarin では、いくつかのプレビュー リリースの NuGet パッケージをこれらの更新プログラムを Xamarin.Android サポートを示します。 

-   [Google Play サービス](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; Google Play サービスの最新バージョンが含まれていますが、新しい*アプリへの招待*機能で、ユーザーをアプリを友人と共有できるようになります。 この機能の詳細については、次を参照してください。 [Google のアプリへの招待を展開し、アプリの Reach](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)です。 

-   [Android のサポート ライブラリ](https://www.nuget.org/packages?q=xamarin+support+library)&ndash;これら NuGets のみ提供されるライブラリの Api の旧バージョンと互換性のあるバージョンの Android framework Api を提供しつつ機能が提供されます。 

-   [Android ウェアラブル ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)&ndash;この NuGet に Google Play サービスのバインディングが含まれています。 ウェアラブル ライブラリの最新バージョンでは、Android を着用プラットフォーム (カスタム アプリをより簡単に移動など) の新機能が表示されます。 


## <a name="summary"></a>まとめ

この記事では、Android Marshmallow を導入し、インストールして Marshmallow で最新のツールおよび Xamarin.Android 開発用のパッケージを構成する方法を説明しました。 Xamarin.Android 開発のための最も魅力的な新しい Android Marshmallow 機能の概要についても提供されます。


## <a name="related-links"></a>関連リンク

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK を取得します。](https://developer.android.com/sdk/index.html#Other)
- [機能の概要](https://developer.android.com/preview/api-overview.html)
- [リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (サンプル)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (サンプル)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (サンプル)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
