---
title: Nougat 機能
description: Xamarin.Android を使用して Android Nougat 用アプリの開発を開始する方法。
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 2e15de944f05fede802dbf52987d80a46fb890ef
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242154"
---
# <a name="nougat-features"></a>Nougat 機能

_Xamarin.Android を使用して Android Nougat 用アプリの開発を開始する方法。_

この記事では Android Nougat の場合で導入された機能の概要が、Android Nougat 開発用の Xamarin.Android を準備する方法について説明し、Android Nougat 機能を使用する方法を説明するサンプル アプリケーションへのリンクを提供します。Xamarin.Android アプリです。


## <a name="overview"></a>概要

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html)が Android 6.0 Marshmallow に Google のフォロー アップします。 Xamarin.Android のサポートを提供する**Android 7.x バインド**Xamarin Android 7.0 以降。 Android Nougat; 以下に示す Nougat 機能の多くの新しい Api を追加しますXamarin.Android 7.0 を使用する場合は、これらの Api に Xamarin.Android アプリを利用できます。

[![Android タブレットと Android Nougat を実行している携帯電話のヒーローのイメージ](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x Api の詳細については、次を参照してください。[開発者向けの Android 7.1](http://developer.android.com/preview/api-overview.html)します。
Xamarin.Android 7.0 の既知の問題についてを参照してください、[リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)します。

Android Nougat では、Xamarin.Android 開発者にとって関心のある多くの新機能を提供します。 これには次の機能があります。

-   **マルチ ウィンドウ サポート**&ndash;この機能強化により、ユーザーは、画面上の 2 つのアプリを一度に開く。

-   **通知の機能強化** &ndash; Android Nougat で再設計された通知システムが含まれています、*直接応答*通知から直接、テキスト メッセージにすばやく対応できるようにする機能UI。 アプリの通知を作成する場合、受信も、メッセージ、新しい*通知をバンドル*機能できますバンドル通知 1 つのグループとして 1 つ以上のメッセージを受信します。

-   **データのスクリーン セーバー** &ndash;この機能は、アプリによる携帯データ ネットワークの使用量の削減に役立つ新しいシステム サービスは、アプリによる携帯データ ネットワークの使用方法の制御をユーザーに提供します。

Android Nougat は他の多くの機能強化、さらに、新しいネットワーク セキュリティ構成機能などのアプリ開発者の関心のある、外出先で Doze、キーの構成証明、新しいクイック設定 Api では、複数のロケールのサポート、ICU4J Api、web ビューの機能強化Java 8 の言語の機能へのアクセス、スコープを持つディレクトリのアクセス、ポインターのカスタム API、プラットフォームの VR サポート、仮想ファイル、およびバック グラウンドの最適化を処理します。

この記事では、新しい機能を試すし、新しい Android Nougat プラットフォームを対象に移行または機能の作業を計画する Android Nougat でアプリを構築する方法について説明します。


## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android Nougat 機能を使用する、次が必要。

-   **Visual Studio または Visual Studio for Mac** &ndash; 4.2.0.628 のバージョンの Visual Studio を使用しているかどうか、または Visual Studio Tools for Xamarin のそれ以降が必要です。 使用する場合 Visual Studio for Mac バージョン 6.1.0 または以降の Visual Studio Mac が必要です。

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 以降をインストールして Visual Studio または Visual Studio for mac 構成する必要があります。

-   **Android SDK** -Android SDK 7.0 (API 24) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin Android 7.0 の開発が必要です[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)または以降の API レベル 24 開発している場合、または大きい (JDK 8 もサポートしている API レベル 24 より前)。 カスタム コントロールまたはフォーム プレビューアーを使用している場合、JDK 8 の 64 ビット バージョンが必要です。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

アプリする必要がありますリビルドされる Xamarin C6SR4 およびそれ以降を Android Nougat で確実に動作に注意してください。 Android Nougat にのみリンクできますので[NDK が提供するネイティブ ライブラリ](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)などのライブラリを使用して既存のアプリ、 **Mono.Data.Sqlite.dll**がクラッシュする場合、それ以外の場合正しくは、Android Nougat で実行されています。再構築されます。



## <a name="getting-started"></a>作業の開始

Xamarin.Android で Android Nougat を使用して開始するには、ダウンロードし、Android Nougat プロジェクトを作成する前に、最新のツールと SDK パッケージをインストールする必要があります。

1.  Xamarin から最新の Xamarin.Android 更新プログラムをインストールします。

2.  インストール、 **Android 7.0 (API 24)** パッケージとツールまたはそれ以降。

3.  対象とする Android Nougat 新しい Xamarin.Android プロジェクトを作成します。

4.  Android Nougat のエミュレーターまたはデバイスを構成します。

次のセクションでは、これらの各手順について説明します。


### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムをインストールします。

Android Nougat の Xamarin のサポートを追加するには、安定チャネルに for Mac で Visual Studio または Visual Studio 更新プログラム チャネルを変更し、最新の更新プログラムを適用します。 アルファ版またはベータ チャネルのみで現在利用できる機能も必要がある場合は、アルファ チャネルまたはベータ チャネル (アルファ版およびベータ チャネルに対するサポートも提供 Android 7.x まで) に切り替えることができます。 更新プログラム (リリース) チャネルを変更する方法については、次を参照してください。[更新チャネルを変更する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)します。



### <a name="install-the-android-sdk"></a>Android SDK をインストールします。

Xamarin Android 7.0 プロジェクトを作成する必要があります最初に使用する Android SDK Manager をインストールする**SDK プラットフォーム Android N (API 24)** またはそれ以降。 最新版をインストールする必要がありますも**Android SDK Tools**:

1.  Android SDK マネージャーを起動 (Visual studio for Mac では、次のように使用します。**ツール > Android SDK マネージャーを開く&hellip;**; Visual studio を使用して**ツール > Android > Android SDK Manager**)。

2.  インストール**Android 7.0 (API 24)** またはそれ以降。

    [![Android SDK Manager で Android 7.0 のパッケージを選択します。](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  最新の Android SDK ツールをインストールします。

    [![最新の Android SDK ツール、Android SDK マネージャーを選択します。](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools のリビジョン 25.2.2 または以降では、Android SDK プラットフォーム ツール 24.0.3 またはそれ以降、および Android SDK ビルド ツール 24.0.2 をインストールする必要がありますまたはそれ以降。

4.  いることを確認、 **Java Development Kit の場所**JDK 1.8 が構成されています。

    [![[ツール オプション] で JDK 8 パスの構成](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio でこの設定を表示する をクリックして**ツール > オプション > Xamarin > Android 設定**します。 Visual studio for Mac では、次のようにクリックします。**設定 > プロジェクト > SDK の場所 > Android**します。



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始します。

新しい Xamarin.Android プロジェクトを作成します。 Xamarin で Android の開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md)を Xamarin.Android プロジェクトを作成する方法について説明します。

Android プロジェクトを作成するときに、Android 7.0 以降をターゲットにバージョン設定を構成する必要があります。 たとえば、Android 7.0 のプロジェクトを対象にする必要がありますを構成するにプロジェクトのターゲットの Android API レベル**Android 7.0 (API 24 - Nougat)** します。 API 24、またはそれ以降は、ターゲット フレームワークのレベルを設定することをお勧めします。 詳細レベルの Android API レベルの構成については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。


> [!NOTE]
> 現在設定する必要があります、 **Minimum Android version**に**Android 7.0 (API 24 - Nougat)** Android Nougat デバイスまたはエミュレーターにアプリをデプロイします。



### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成します。

エミュレーターを使用している場合は、Android AVD マネージャーを起動し、次の設定を使用して新しいデバイスを作成します。

-   デバイス: Nexus 5 Nexus 6、Nexus 6p、Nexus プレーヤー、Nexus 9、またはピクセル C. X
-   対象: Android 7.0 - API レベル 24
-   ABI x86 または x86\_64。

たとえば、Nexus 6 をエミュレートするためにこの仮想デバイスを構成します。

[![Nexus 6 デバイス、Android 7.0 ターゲット、および Intel Atom x86 CPU/ABI を使用して、AVD を構成します。](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

X 5、6、または 9 の Nexus などの物理デバイスを使用している場合か、無線 (OTA) 更新プログラムを自動でデバイスを更新またはシステム イメージをダウンロードしてフラッシュ、デバイスを直接。 Android Nougat にデバイスを手動で更新の詳細については、次を参照してください。 [Nexus デバイスの OTA イメージ](https://developers.google.com/android/nexus/ota)します。

Nexus 5 デバイスと Android Nougat でサポートされていないことに注意してください。



## <a name="new-features"></a>新機能

Android Nougat では、さまざまな新機能と、複数のウィンドウのサポート、通知の機能強化、およびデータ セーバーなどの機能について説明します。 次のセクションでは、これらの機能を強調表示し、アプリでの使用に役立つリンクを開始するを指定します。



### <a name="multi-window-mode"></a>マルチ ウィンドウ モード

マルチ ウィンドウ モードでは、ユーザーはマルチタス キングに完全なサポートを一度に 2 つのアプリを開く。 これらのアプリでは、分割画面表示モードで並列 (横) または 1-上記-、-その他 (縦) を実行できます。
ユーザーは、区分線をドラッグしてサイズを変更するには、アプリ間で、カット アンド ペーストをことができます、アプリ間で。 マルチ ウィンドウ モードでは、2 つのアプリが表示されたら、選択したアクティビティは、選択されていないアクティビティは一時停止しているがまだ表示されているを実行し続けます。 マルチ ウィンドウ モードでは、Android アクティビティのライフ サイクルは変更されません。

[![縦長と横長の両方でマルチ ウィンドウ モードで実行されているサンプル アプリ](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin.Android アプリのアクティビティがマルチ ウィンドウ モードをサポートする方法を構成することができます。 たとえば、マルチ ウィンドウ モードでの最小サイズと、既定の高さと幅、アプリの設定属性を構成できます。 新たに使用することができます`Activity.IsInMultiWindowMode`プロパティがマルチ ウィンドウ モードで、アクティビティを判断します。 例えば:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/)サンプル アプリに複数のウィンドウで、アプリのユーザー インターフェイスを利用する方法を示す c# コードが含まれています。

マルチ ウィンドウ モードの詳細については、次を参照してください。、[マルチ ウィンドウ サポート](https://developer.android.com/guide/topics/ui/multi-window.html)します。



### <a name="enhanced-notifications"></a>強化された通知

Android Nougat には、再設計された通知システムが導入されています。 ユーザー通知 UI で直接受信のテキスト メッセージ通知に迅速に応答を可能にする新しい直接応答機能が用意されています。 Android 7.0 以降、メッセージまとめて 1 つのグループとして 1 つ以上のメッセージが受信したときに通知します。 また、開発者は、ビュー、通知では、システムの装飾を活用し、通知を生成するときに、新しい通知テンプレートの利用の通知をカスタマイズできます。


#### <a name="direct-reply"></a>直接の返信

ユーザーは、受信メッセージの通知を受信すると Android Nougat できるようになります通知内でメッセージに返信 (ではなく、応答を送信するためのメッセージング アプリを開きます)。
このインライン返信機能により、通知インターフェイス内で直接、SMS またはテキスト メッセージに迅速に対応するユーザー。

[![直接返信フィールドがインラインの通知のスクリーン ショット](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

アプリでこの機能をサポートするために追加する必要があります*インライン応答アクション*経由でアプリを[RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/)通知 UI から直接ユーザーがテキストで返信するためのオブジェクトします。
たとえば、次のコードのビルド、`RemoteInput`テキスト入力を受信するため、応答アクションの保留中のインテントをビルドし、リモートの入力が有効なアクションを作成します。

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

このアクションは、通知に追加されます。

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[メッセージング サービス](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/)サンプル アプリには使用した通知を拡張する方法を示す c# コードが含まれています、`RemoteInput`オブジェクト。 インライン返信を追加する方法についての詳細情報をアプリにアクション Android 7.0 以降を参照してください、Android[通知に返信する](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct)トピック。


#### <a name="bundled-notifications"></a>バンドルされている通知

Android Nougat では、通知メッセージを (たとえば、メッセージのトピック) をグループ化でき、各メッセージではなく、グループを表示することができます。
これは、*通知をバンドル*機能により、ユーザーは無視または 1 つのアクションで、通知のグループをアーカイブします。 ユーザーの詳細にそれぞれの通知を表示する通知のバンドルの展開を下へスライドできます。

[![バンドルされている通知のスクリーン ショットの例](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

バンドルされている通知をサポートするアプリで使用できる、 [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/)のような通知をバンドルする方法。 Android N でバンドルされている通知先グループの詳細については、Android を参照してください。[バンドル通知](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle)トピック。


#### <a name="custom-views"></a>カスタム ビュー

Android Nougat では、システム通知ヘッダー、アクション、および拡張可能なレイアウトでカスタムの通知ビューを作成することが可能です。 Android Nougat でカスタムの通知ビューの詳細については、Android を参照してください。[通知の機能強化](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements)トピック。



### <a name="data-saver"></a>データのスクリーン セーバー

Android Nougat 以降、ユーザーできるように、新しい*データ セーバー*ブロックがデータの使用状況を背景を設定します。 この設定は、可能な場合は、フォア グラウンドでの低いデータを使用するアプリにも通知します。 [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/)が Android Nougat で拡張されて、アプリは、アプリがデータのスクリーン セーバーを有効にすると、そのデータの使用状況を制限する作業できるようにデータのスクリーン セーバーを有効にかどうかをチェックできるようにします。

Android Nougat データ スクリーン セーバーの新しい機能の詳細については、Android を参照してください。[ネットワーク データの使用量を最適化する](https://developer.android.com/training/basics/network-ops/data-saver.html)トピック。



### <a name="app-shortcuts"></a>アプリのショートカット

Android 7.1 が導入された、*アプリのショートカット*すばやく開始一般的なタスクや推奨されるタスクで、アプリにユーザーを可能にする機能。
ショートカット メニューをアクティブ化する、ユーザー時間の長いのアプリ アイコンの 1 秒以上&ndash;クイック振動、メニューが表示されます。
解放、キーを押して、メニューが発生します。

[![メッセージング アプリ、アプリのショートカット メニューのサンプル画面](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

この機能は、唯一利用の API レベル 25 以上です。
Android 7.1 の新機能でアプリのショートカットの詳細については、Android を参照してください。[アプリのショートカット](https://developer.android.com/guide/topics/ui/shortcuts.html)トピック。


### <a name="sample-code"></a>サンプル コード

Android Nougat 機能を活用する方法について説明するいくつかの Xamarin.Android サンプルがあります。

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Android Nougat で使用できるマルチ ウィンドウ API の使用方法を示します。 サンプル アプリは、アプリのライフ サイクルと動作の影響を確認する複数の windows モードに切り替えることができます。

-   [メッセージング サービス](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/)が通知を送信する単純なサービスを使用して、`NotificationCompatManager`します。 通知を拡張し、 `RemoteInput` Android Nougat デバイス アプリを開くことがなく、通知から直接テキストで返信を許可するオブジェクト。

-   [アクティブな通知](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/)を使用する方法を示します、 `NotificationManager` API、アプリケーションが現在表示されている通知の数がわかります。

-   [ディレクトリのアクセスのスコープ](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/)スコープを持つディレクトリ アクセスの API を使用して、特定のディレクトリを簡単にアクセスする方法を示します。 定義することの代替としてこの`READ_EXTERNAL_STORAGE`または`WRITE_EXTERNAL_STORAGE`マニフェストでのアクセスを許可します。

-   [直接ブート](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/)は常に使用される、デバイスがブート両方前に、および任意のユーザー credentials(PIN/Pattern/Password) が入力した後、デバイスで暗号化されたストレージにデータを格納する方法を示しています。


## <a name="summary"></a>まとめ

この記事では、Android Nougat を導入し、インストールし、Android Nougat で最新のツールと Xamarin.Android の開発用のパッケージを構成する方法について説明します。 Android Nougat 用アプリの作成を開始するためのサンプル ソース コードへのリンクの Android Nougat の場合で使用できる主な機能の概要についても提供します。


## <a name="related-links"></a>関連リンク

- [開発者向けの android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
