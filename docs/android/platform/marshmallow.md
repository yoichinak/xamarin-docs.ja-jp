---
title: Marshmallow 機能
description: この記事では、Xamarin.Android を使用して、Android 6.0 Marshmallow 用アプリの開発で使用を開始します。
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a396f4fe59db36b134843d2538bcb470a452a85b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668583"
---
# <a name="marshmallow-features"></a>Marshmallow 機能

_この記事では、Xamarin.Android を使用して、Android 6.0 Marshmallow 用アプリの開発で使用を開始します。_

この記事で Android 6.0 Marshmallow の新機能の概要を示します、Android Marshmallow の開発用の Xamarin.Android を準備する方法について説明し、新しい Android Marshmallow を使用する方法を説明するサンプル アプリケーションへのリンクを提供します。Xamarin.Android アプリで機能します。 


## <a name="overview"></a>概要

[Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)、次の主要な Android は、Android Lollipop の後にリリースします。
Xamarin.Android では、Android Marshmallow をサポートしているしが含まれています。

-   **API 23/Android 6.0 バインディング** &ndash; Android 6.0 以下に示す新機能の多くの新しい Api を追加します。 API レベル 23 が対象とする場合は、これらの Api が Xamarin.Android アプリを利用できます。 Android 6.0 Api の詳細については、次を参照してください。 [Android 6.0 Api](https://developer.android.com/preview/api-overview.html)します。 

[![タブレットと携帯電話 Marshmallow を実行しているのヒーローのイメージ](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow リリースは主に「ポーランド語、および品質」に焦点を当てています、Xamarin.Android 開発者にとって関心のある多くの新機能もあります。 これには次の機能があります。 

-   **ランタイムのアクセス許可**&ndash;この機能強化により、ユーザーは、実行時に、個別にセキュリティのアクセス許可を承認します。 

-   **認証の機能強化** &ndash; Android Marshmallow 以降、アプリ使えるようになりました指紋センサーを認証ユーザー、および新しい*資格情報を確認します*機能には、入力の必要性が最小限に抑えられます。パスワード。 

-   **アプリのリンク**&ndash;ことの必要性を排除するこの機能により、**アプリの選択**自動的にアプリを web ドメインに関連付けることによってポップアップします。 

-   **共有を直接**&ndash;を定義できます*共有ターゲットを直接*を共有迅速かつ直感的なユーザーの; この機能は、他のアプリとコンテンツを共有する uers を使用できます。 

-   **音声の相互作用**&ndash;この新しい API は、会話の音声機能をアプリに組み込むことができます。 

-   **4 K の表示モード** &ndash; Android Marshmallow でアプリのことが要求にこれをサポートするハードウェア上で 4 K のディスプレイ解像度。 

-   **オーディオの新機能** &ndash; Marshmallow 以降、Android ようになりました MIDI プロトコル。 デジタル オーディオ キャプチャと再生のオブジェクトを作成する新しいクラスも用意されていて、オーディオ入力デバイスを関連付けるための新しい API フックを提供します。 

-   **ビデオの新機能** &ndash; Marshmallow がアプリを支援する新しいクラスがオーディオおよびビデオ ストリームを同期のレンダリングを提供します。 このクラスは、動的な再生レートのサポートを提供することもできます。 

-   **Android for Work** &ndash; Marshmallow には会社が所有、シングル ユーザーのデバイス用の拡張コントロールが含まれています。 サイレント インストールをサポートし、アプリ、デバイスの所有者によって、システムの更新プログラム、強化された証明書の管理、データ使用状況の追跡、アクセス許可の管理、および作業状態の通知の自動承認のアンインストールします。 

-   **素材のデザイン サポート ライブラリ**&ndash;新しい*デザイン サポート ライブラリ*デザイン コンポーネントとアプリに組み込むマテリアル デザイン ルック アンド フィールが簡単ですパターンを示します。 

さらに、Android m では、多くの中核となる Android ライブラリの更新プログラムがリリースされたし、これらの更新プログラムは、Android M と以前のバージョンの Android の両方の新機能を提供します。

さらに、多くの中核となる Android ライブラリの更新プログラムが Android Marshmallow、と共にリリースされたし、これらの更新プログラムが Android Marshmallow と以前のバージョンの Android の両方の新機能を提供します。 この記事では、Android Marshmallow は、アプリを構築する方法を説明し、Android 6.0 の新機能の概要が強調表示を提供します。 

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで Android Marshmallow の新機能を使用する、次が必要。 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 or later must be installed and configured with either Visual Studio or Xamarin Studio.

-   **Visual Studio for Mac**または**Visual Studio** &ndash; Visual Studio for Mac バージョン 5.9.7.22 を使用しているか以降が必要です。 直接的またはそれ以降バージョン 3.11.1537、Visual Studio を使用している場合は、Visual Studio 用の Xamarin ツールが必要です。 

-   **Android SDK** &ndash; Android SDK 6.0 (API 23) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin.Android 必要[JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)または以降の API レベル 24 開発している場合、または大きい (JDK 1.8 もサポートしている API レベル 24、Marshmallow などよりも前)。 カスタム コントロールまたはフォーム プレビューアーを使用している場合、JDK 1.8 の 64 ビット バージョンが必要です。

引き続き使用できます[JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)またはそれ以前の API レベル 23 ののみで開発する場合。 


## <a name="getting-started"></a>作業の開始

Xamarin.Android で Android Marshmallow の使用を開始するには、ダウンロードし、Android Marshmallow プロジェクトを作成する前に、最新のツールと SDK パッケージをインストールする必要があります。 

1.  最新の Xamarin の更新プログラムをインストール、**安定した**チャネル。 

2.  Android 6.0 Marshmallow SDK パッケージとツールをインストールします。

3.  新しい Xamarin.Android プロジェクトを対象とする Android 6.0 Marshmallow (API レベル 23) を作成します。 

4.  Android Marshmallow のエミュレーターまたはデバイスを構成します。

次のセクションでは、これらの各手順について説明します。


### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムをインストールします。

Xamarin は、Android 6.0 Marshmallow のサポートを含むように更新するに変更する更新プログラム チャネル**安定した**し、すべての更新プログラムをインストールします。 更新プログラム チャネルから更新プログラムのインストールの詳細については、次を参照してください。[更新チャネルを変更](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)します。 


### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK をインストールします。

Android Marshmallow の Xamarin.Android プロジェクトを作成するには、Android SDK Manager を使用して Android 6.0 SDK をインストールする必要があります。

-   Android SDK マネージャーを起動 (Visual studio for Mac では、次のように使用します。**ツール > SDK Manager**; Visual studio を使用して**ツール > Android > Android SDK Manager**) および最新の Android SDK Tools のインストール。

    [![Android SDK ツール、Android SDK マネージャーを選択](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   また、最新のインストール**Android 6.0** SDK パッケージ。

    [![Android SDK Manager で Android 6.0 SDK パッケージを選択します。](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools のリビジョン 24.3.4 をインストールする必要がありますまたはそれ以降。
Android SDK Manager を使用して、Android 6.0 SDK をインストールする方法の詳細については、次を参照してください。 [SDK Manager](https://developer.android.com/tools/help/sdk-manager.html)します。



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始します。

新しい Xamarin.Android プロジェクトを作成します。 Xamarin で Android の開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md)を Android プロジェクトを作成する方法について説明します。 

Android プロジェクトを作成するときに、ターゲット Android 6.0 MarshMallow にバージョン設定を構成する必要があります。 Marshmallow のプロジェクトを対象とするには、プロジェクトを構成する必要があります**API レベル 23 (Xamarin.Android v6.0 サポート)** します。 詳細レベルの Android API レベルの構成については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。



### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成します。

エミュレーターを使用している場合は、Android AVD マネージャーを起動し、次の設定を使用して新しいデバイスを作成します。

-   デバイス:Nexus 5、6、または 9 です。
-   ターゲット:Android 6.0 - API レベル 23
-   ABI: x86

たとえば、Nexus 5 をエミュレートするためにこの仮想デバイスを構成します。

[![Nexus 5 デバイス、Android 6.0 ターゲット、および Intel Atom (x86) を使用して、AVD を構成します。](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5 などの物理デバイスを使用している場合、6 か月または 9 をインストールできます Android Marshmallow のプレビュー イメージ。 デバイスを Android Marshmallow に更新の詳細については、次を参照してください。[ハードウェア システムのイメージ](https://developer.android.com/preview/download.html#images)します。



## <a name="new-features"></a>新機能

Android Marshmallow で導入された変更の多くは Android ユーザー エクスペリエンスの向上、パフォーマンスを向上およびバグの修正に重点を置いています。 ただし、Marshmallow いくつかの大幅な変更を Android プラットフォームの基礎とも導入します。 次のセクションでは、これらの拡張機能を強調表示し、アプリで Android Marshmallow の新機能の使用を開始するのに役立つリンクを指定します。 



### <a name="runtime-permissions"></a>ランタイムのアクセス許可

Android のアクセス許可システムを大幅に最適化して Android Lollipop 以降簡略化します。 Android Marshmallow では、ユーザーは、インストール時のではなく、実行時に、ケースごとにアクセス許可を付与します。 Android Marshmallow 以降は、この機能をサポートするために、アプリのアクセス許可 (アクセス許可が必要な場所のコンテキスト) で実行時にユーザー入力を求めるを設計します。 この変更により、アプリの使用を開始するユーザーを簡単にインストールして、アプリのアップグレードのプロセスを効率化するためにすぐにします。 

参照してください[Android Marshmallow でのランタイム アクセス許可の要求](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/)Xamarin.Android アプリでランタイムのアクセス許可の実装の詳細について (コード例を含む)。
Xamarin には、Android Marshmallow で (とそれ以降) のランタイムのアクセス許可のしくみを示すサンプル アプリも用意されています。[RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)します。

このサンプル アプリでは、次の項目を示しています。

-   確認し、実行時にアクセス許可を要求する方法。
-   Android M デバイスのアクセス許可を宣言する方法。

このサンプル アプリを使用します。

1.  タップして、**カメラ**または**連絡先**のアクセス許可を表示するボタンがダイアログを要求します。
2.  カメラや連絡先のフラグメントを表示するアクセス許可を付与します。

Android Marshmallow でランタイムのアクセス許可の新機能についての詳細については、次を参照してください。[システム権限を持つ作業](https://developer.android.com/preview/features/runtime-permissions.html)します。



### <a name="authentication-enhancements"></a>認証の拡張

Android Marshmallow には、パスワードの必要性を排除するのに役立つ 2 つの認証の拡張機能が含まれています。

-   **指紋認証**&ndash;指紋のスキャンを使用してユーザーを認証します。

-   **資格情報を確認します。** &ndash;はどのくらいの期間、デバイスがロック解除されたに基づいてユーザーを認証します。

リンクと次に説明されているサンプル アプリに、これらの新しい機能の理解することがのに役立ちます。


#### <a name="fingerprint-authentication"></a>指紋認証

指紋ハードウェアのスキャンをサポートするデバイスの場合で新しい使える`FingerPrintManager`ユーザーを認証するクラス。
Android Marshmallow の指紋認証機能の詳細については、次を参照してください。[指紋認証](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)します。

Xamarin には、登録済みの指紋を使用して、アプリ内のユーザーを認証する方法を示すサンプル アプリが用意されています。[FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)します。

このサンプル アプリを使用します。

1.  タッチ、**購入**指紋認証ダイアログ ボックスを開くボタンをクリックします。
2.  認証に登録されている指紋をスキャンします。

このサンプル アプリが、指紋リーダーを使用したデバイスが必要であるに注意してください。
このアプリでは、指紋 (またはパスワード) は保存されません。



#### <a name="voice-interactions"></a>音声の相互作用

Android Marshmallow で導入された新しい音声の相互作用の機能は、アクションを確認し、オプションの一覧から選択する音声を使用するアプリのユーザーを使用できます。 音声の相互作用の詳細については、次を参照してください。[音声の相互作用 API の概要](https://developers.google.com/voice-actions/interaction/)します。 

参照してください[音声の相互作用使用の Android アプリへのメッセージ交換を追加](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/)Xamarin.Android アプリで音声の相互作用の実装の詳細について (コード例を含む)。
サンプル アプリを利用できるを Xamarin.Android アプリで音声の相互作用の API を使用する方法を示しています。[音声の相互作用](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions)します。



#### <a name="confirm-credential"></a>資格情報を確認します。

新しい*資格情報を確認します。* 機能 Android Marshmallow でのユーザー記憶して、どのくらいの期間、デバイスがロック解除されたに基づいてそれらを認証することによってアプリ固有のパスワードを入力する必要がなくなりますを解放することができます。
これを行うには、使用する新しい`SetUserAuthenticationValidityDurationSeconds`のメソッド、`KeyGenerator`します。 使用して、`KeyGuardManager`の`CreateConfirmDeviceCredentialIntent`をアプリ内からユーザーを再認証する方法。 Android Marshmallow のこの新機能の詳細については、次を参照してください。[資格情報の確認](https://developer.android.com/preview/api-overview.html#confirm-credential)します。

Xamarin では、アプリでデバイスの資格情報 (など、PIN、パターン、またはパスワード) を使用する方法を示すサンプル アプリを提供します。[ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

このサンプル アプリを使用します。

1.  デバイスでのセキュリティで保護されたロック画面のセットアップ (**Secure > セキュリティ > Screenlock**)。
2.  タップして、**購入**ボタンをクリックし、セキュリティで保護されたロック画面の資格情報を確認します。



### <a name="chrome-custom-tabs"></a>Chrome のカスタム タブ

URL をタップすると、アプリ開発者直面選択肢: アプリする、ブラウザーを起動するかに基づいてアプリのブラウザーを使用して、`WebView`します。 両方のオプションの課題を提示する&ndash;いないときに、カスタマイズ可能な負荷の高いコンテキスト スイッチは、ブラウザーを起動`WebView`s は、ブラウザーを使用して状態を共有しないでください。 使用も、 `WebView`s は余分なメンテナンスのオーバーヘッドを追加できます。 

*ユーザー設定 タブの chrome*簡単かつ洗練された方法は、Chrome のパワーを備えた web サイトを表示せず、ユーザーがアプリのままにすることが可能になります。 この機能では、アプリ ユーザーの web エクスペリエンスをさらに制御ネイティブ間の遷移を作成しよりシームレスなコンテンツを web に頼ることがなく、 `WebView`。 アプリは、Chrome の外観し、次のカスタマイズによる感覚にも影響します。 

-   ツールバーの色

-   入力し、アニメーションの終了

-   Chrome のツールバーとオーバーフロー メニューにカスタム アクション

-   Chrome (高速な読み込み) の開始前とコンテンツのプリフェッチ

Xamarin.Android アプリでこの機能を利用するをダウンロードしてインストール、 [Android サポートのカスタム タブ ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)します。
この機能の詳細については、次を参照してください。 [Chrome カスタム タブ](https://developer.chrome.com/multidevice/android/customtabs)します。



### <a name="material-design-support-library"></a>素材のデザイン サポート ライブラリ

導入された android Lollipop[マテリアル デザイン](http://www.google.com/design/spec/material-design/introduction.html)Android エクスペリエンスを更新する新しいデザイン言語として (を参照してください[マテリアル テーマ](~/android/user-interface/material-theme.md)素材のデザインを Xamarin.Android アプリの使用について)。 Google Android Marshmallow での導入、 *Android サポート ライブラリを設計する*マテリアルを採用する開発者はアプリを簡単に外観をデザインします。 このライブラリには、次のコンポーネントが含まれています。

-   **CoordinatorLayout** &ndash;新しい`CoordinatorLayout`ウィジェットに似ていますより強力なは、`FrameLayout`します。 使用することができます`CoordinatorLayout`または最上位のレイアウトとして子ビューのコンテナーとしての提供、`layout_anchor`ビューを他のビューの基準としたアンカー ビューに使用できる属性。

-   **ツールバーの折りたたみ**&ndash;新しい`CollapsingToolbarLayout`のラッパーである折りたたみアプリ バーは、`Toolbar`します。 (なお、*アプリ バー*何が呼ばれていましたが、*操作バー*)。

-   **浮動アクション ボタン**&ndash;アプリのインターフェイスのプライマリ アクションを示す丸いボタン。

-   **テキストの編集のラベルを浮動**&ndash;で使用される新しい`TextInputLayout`ウィジェット (ラップ`EditText`) ユーザーがテキストを入力するときのヒントが非表示の場合、浮動小数点のラベルを表示します。

-   **ナビゲーション ビュー** &ndash;新しい`NavigationView`ウィジェットを使用して、ユーザーが移動するは容易な方法で、ナビゲーション ドロワーを使用できます。

-   **Snackbar** &ndash;新しい`SnackBar`ウィジェットは軽量のフィードバック メカニズム (トーストに似ています)、画面上の他の要素をすべて表示される画面の下部に、簡単なメッセージを表示します。

-   **素材タブ**&ndash;新しい`TabLayout`ウィジェットをアプリに最上位レベルのナビゲーションを実装する方法として、タブを表示するための水平方向のレイアウトを提供します。

活用するために、[デザイン サポート ライブラリ](https://developer.android.com/tools/support-library/features.html#design)で Xamarin.Android アプリをダウンロードして、Xamarin をインストール[Xamarin サポート ライブラリ デザイン](https://www.nuget.org/packages/Xamarin.Android.Support.Design/)NuGet パッケージ。

参照してください[Android サポートのデザイン ライブラリを使用した美しいマテリアル デザイン](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/)詳細 (コード例を含む) について Xamarin.Android アプリでマテリアル デザイン サポート ライブラリを使用します。
Xamarin には、Xamarin.Android で新しい Android のデザイン ライブラリを紹介するサンプル アプリを&ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare)します。
このサンプルでは、デザイン ライブラリの次の機能を使用します。


-   ツールバーの折りたたみ
-   浮動アクション ボタン
-   アンカーのビュー
-   NavigationView
-   Snackbar

デザイン ライブラリの詳細については、次を参照してください。 [Android サポート ライブラリを設計する](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html)Android 開発者のブログにします。


### <a name="additional-library-updates"></a>追加のライブラリの更新プログラム

だけでなく Android Marshmallow、Google はいくつかの中核となる Android ライブラリに関連する更新プログラムを発表しました。 Xamarin では、いくつかのプレビュー リリースの NuGet パッケージでこれらの更新プログラムの Xamarin.Android のサポートを提供します。 

-   [Google play 開発者サービス](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; Google play 開発者サービスの最新バージョンが含まれていますが、新しい*アプリへの招待*機能は、ユーザーを友人と、アプリを共有することができます。 この機能の詳細については、次を参照してください。 [Google のアプリへの招待を展開し、アプリのリーチ](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)します。 

-   [Android サポート ライブラリ](https://www.nuget.org/packages?q=xamarin+support+library)&ndash;は旧バージョンと互換性のあるバージョンの Android framework Api を提供しながら、ライブラリ Api の使用のみこれらの Nuget 機能します。 

-   [Android のウェアラブル ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)&ndash;この NuGet には、Google play 開発者サービスのバインドが含まれています。 ウェアラブル ライブラリの最新バージョンでは、Android Wear のプラットフォーム (カスタム アプリをより容易なナビゲーションを含む) の新機能が表示されます。 


## <a name="summary"></a>まとめ

この記事では、Android Marshmallow を導入し、インストールして Marshmallow で最新のツールと Xamarin.Android の開発用のパッケージを構成する方法について説明しました。 Xamarin.Android の開発のための最も魅力的な新しい Android Marshmallow 機能の概要についても提供します。


## <a name="related-links"></a>関連リンク

- [Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK を入手します。](https://developer.android.com/sdk/index.html#Other)
- [機能の概要](https://developer.android.com/preview/api-overview.html)
- [リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (サンプル)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (サンプル)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (サンプル)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
