---
title: Nougat 機能
description: Android Nougat 用アプリを開発する Xamarin.Android を使用して作業を開始する方法。
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 15698767ae71b68a26138169771f7f397bddd95a
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732519"
---
# <a name="nougat-features"></a>Nougat 機能

_Android Nougat 用アプリを開発する Xamarin.Android を使用して作業を開始する方法。_

この記事は Android Nougat で導入された機能の概要が Android Nougat 開発では、Xamarin.Android を準備する方法について説明し、Android Nougat 機能を使用する方法を示すサンプル アプリケーションへのリンクを示しますXamarin.Android アプリ。


## <a name="overview"></a>概要

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) Android 6.0 Marshmallow に Google のフォロー アップがします。 サポートを提供する Xamarin.Android **Android 7.x バインド**Xamarin Android 7.0 以降。 Android Nougat; 以下に示す Nougat 機能の多くの新しい Api を追加します。Xamarin.Android 7.0 を使用する場合は、これらの Api が Xamarin.Android アプリで利用できます。

[![Android タブレットと携帯電話の Android Nougat を実行しているのヒーローの画像](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x Api の詳細については、次を参照してください。[開発者用の Android 7.1](http://developer.android.com/preview/api-overview.html)です。
Xamarin.Android 7.0 の既知の問題の一覧を参照してください、[リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)です。

Android Nougat Xamarin.Android 開発者にとって関心のある多くの新機能を提供します。 これには次の機能があります。

-   **複数のウィンドウのサポート**&ndash;この機能強化により、ユーザーが画面上の 2 つのアプリを一度に開くことです。

-   **通知の機能強化** &ndash; Android Nougat で再設計された通知システムが含まれています、*直接応答*通知から直接、テキスト メッセージを迅速に対処するユーザーに許可する機能UI。 アプリの通知を作成する場合、受信も、メッセージと、新しい*通知をバンドル*機能をバンドルできます通知一緒に 1 つのグループとして複数のメッセージを受信するとします。

-   **データ セーバー** &ndash;この機能は、アプリが携帯データ ネットワークの使用量の削減に役立つ、新しいシステム サービスです。 これにより、ユーザー管理アプリによる携帯データ ネットワークの使用方法です。

さらに、その他の多くの機能強化は、Android Nougat 新しいネットワーク セキュリティ構成機能などのアプリの開発者を対象、外出先で Doze、認証、新しいクイック設定 Api では、複数のロケールのサポート、ICU4J Api では、WebView 機能が拡張されて、Java 8 言語機能にアクセスする、スコープを指定したディレクトリのアクセス、カスタム ポインター API、VR のプラットフォームのサポート、仮想ファイル、およびバック グラウンドの最適化を処理します。

この記事では、新しい機能を試すし、新しい Android Nougat プラットフォームを対象の移行または機能の作業を計画する Android Nougat を使ったアプリの構築を開始する方法について説明します。


## <a name="requirements"></a>必要条件

次が機能を使用する、新しい Android Nougat Xamarin ベースのアプリに必要です。

-   **Visual Studio または Visual Studio for Mac** &ndash; 4.2.0.628 のバージョンの Visual Studio を使用しているかどうか、または Visual Studio Tools for Xamarin のそれ以降が必要です。 使用している Visual Studio for Mac, バージョン 6.1.0 または Visual Studio の後で Mac が必要になるの場合

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 以降をインストールし、Visual Studio または Visual Studio for mac 構成

-   **Android SDK** -Android SDK 7.0 (API 24) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin Android 7.0 開発が必要です[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)または API レベルの場合 24 開発している場合は、以降、または大きい (JDK 8 もサポートしている API レベル 24 より前)。 カスタム コントロールまたはフォーム プレビュー用のプログラムを使用している場合、JDK 8 の 64 ビット バージョンが必要です。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

アプリ必要がありますに再構築する Xamarin C6SR4 以降 Android Nougat で確実に動作に注意してください。 Android Nougat にのみリンクできますので[NDK が提供するネイティブ ライブラリ](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)などのライブラリを使用して既存のアプリ、 **Mono.Data.Sqlite.dll**ない場合は正しく Android Nougat で実行されているときにクラッシュする可能性があります再構築されます。



## <a name="getting-started"></a>作業の開始

Xamarin.Android での Android Nougat の使用開始するには、ダウンロードして、Android Nougat プロジェクトを作成する前に、最新のツールと SDK のパッケージをインストールする必要があります。

1.  Xamarin.Android の最新の更新プログラムを Xamarin をインストールします。

2.  インストール、 **Android 7.0 (API 24)** パッケージおよびツールまたはそれ以降。

3.  対象とする Android Nougat 新しい Xamarin.Android プロジェクトを作成します。

4.  Android Nougat のエミュレーターまたはデバイスを構成します。

次のセクションでは、これらの各手順について説明します。


### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムをインストールします。

Android Nougat の Xamarin のサポートを追加するには、Mac の安定したチャネルを Visual Studio または Visual Studio での更新チャネルに変更し、最新の更新プログラムを適用します。 現在、英数字またはベータ版のチャネルでのみ利用可能な機能も必要がある場合は、(アルファとベータ版のチャネルに対するサポートも提供 Android 7.x)、英数字またはベータ チャネルに切り替えることができます。 更新プログラム (リリース) チャンネルを変更する方法については、次を参照してください。[更新チャネルを変更する](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)です。



### <a name="install-the-android-sdk"></a>Android SDK をインストールします。

Xamarin Android 7.0 でプロジェクトを作成する必要があります最初を使用して、Android SDK Manager をインストール**SDK プラットフォーム Android N (API 24)** またはそれ以降。 最新版をインストールすることも必要があります**Android SDK ツール**:。

1.  Android SDK Manager を開始 (Mac を Visual Studio で使用**ツール > Android SDK Manager を開いている&hellip;**; Visual Studio で、使用**ツール > Android > Android SDK Manager**)。

2.  インストール**Android 7.0 (API 24)** 以降。

    [![Android SDK Manager で Android 7.0 パッケージを選択します。](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  最新の Android SDK ツールをインストールします。

    [![Android SDK Manager での最新の Android SDK ツールを選択します。](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK ツール リビジョン 25.2.2 またはそれ以降、Android SDK プラットフォーム ツール 24.0.3 またはその後、および Android SDK ビルド ツール 24.0.2 をインストールする必要がありますまたはそれ以降。

4.  いることを確認、 **Java Development Kit 場所**JDK 1.8 用に構成されました。

    [![[ツール オプション] で JDK 8 パスの構成](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio でこの設定を表示するクリックして**ツール > オプション > Xamarin > Android 設定**です。 Mac 用 Visual Studio で **設定 > プロジェクト > SDK の場所 > Android**です。



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始します。

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android 開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android プロジェクトの作成について学習します。

Android プロジェクトを作成するときに、Android 7.0 以降をターゲットにバージョン設定を構成する必要があります。 たとえば、Android 7.0 用プロジェクトのターゲット、行う必要がありますにプロジェクトのターゲット Android API レベル**Android 7.0 (API 24 - Nougat)** です。 API 24、またはそれ以降は、ターゲット フレームワークのレベルを設定することをお勧めします。 Android API レベル レベルの構成に関する詳細は、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。


> [!NOTE]
> 現在設定する必要があります、**最低限の Android バージョン**に**Android 7.0 (API 24 - Nougat)** Android Nougat デバイスまたはエミュレーターにアプリを配置します。



### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成します。

エミュレーターを使用している場合は、Android AVD マネージャーを起動し、次の設定を使用して新しいデバイスを作成します。

-   デバイス: Nexus 5、6、9、Nexus 6 P、Nexus Player Nexus またはピクセル C. Nexus X
-   ターゲット: Android 7.0 の API レベル 24
-   ABI: x86、x86\_64

たとえば、この仮想デバイスは、Nexus 6 をエミュレートするために構成されます。

[![Nexus 6 デバイス、Android 7.0 ターゲット、および Intel Atom x86 CPU/ABI を使用して、AVD を構成します。](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

X 5、6、または 9 Nexus などの物理デバイスを使用している場合か、無線 (OTA) 更新プログラムを自動でデバイスを更新またはシステム イメージをダウンロードでき、デバイスを直接フラッシュできます。 Android Nougat にデバイスを手動で更新の詳細については、次を参照してください。 [OTA デバイス用のイメージ Nexus](https://developers.google.com/android/nexus/ota)です。

Nexus 5 デバイスが Android Nougat によってサポートされていないことに注意してください。



## <a name="new-features"></a>新機能

Android Nougat には、さまざまな新機能および複数ウィンドウのサポート、通知機能拡張、およびデータ セーバーなどの機能が導入されています。 次のセクションでは、これらの機能を強調表示し、アプリで使用を開始に役立つリンクを提供します。



### <a name="multi-window-mode"></a>複数のウィンドウ モード

複数のウィンドウ モードによりユーザー完全マルチタスク サポートで一度に 2 つのアプリを開くためにできます。 これらのアプリでは、サイド バイ サイド (横) または 1-上-、-その他 (縦) を分割画面表示モードで実行できます。
ユーザーは、区分線をドラッグしてサイズを変更するには、アプリ間で、切り取り、内容を貼り付けます、アプリ間でします。 マルチ ウィンドウ モードでは、2 個のアプリが表示されたら、選択したアクティビティが引き続きが一時停止しているがまだ表示されて選択されていないアクティビティを実行します。 複数のウィンドウ モードでは、Android のアクティビティのライフ サイクルは変更されません。

[![縦長と横長の両方で、複数のウィンドウ モードで実行されている例のアプリ](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin.Android アプリのアクティビティが複数のウィンドウのモードをサポートする方法を構成することができます。 たとえば、複数のウィンドウ モードの最小サイズと、既定の高さと幅、アプリを設定する属性を構成できます。 新しいを使用する`Activity.IsInMultiWindowMode`プロパティ、アクティビティが複数のウィンドウ モードのかを判断します。 例えば:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/)サンプル アプリには、複数のウィンドウで、アプリのユーザー インターフェイスを利用する方法を示す c# コードが含まれています。

複数のウィンドウ モードの詳細については、次を参照してください。、[マルチ ウィンドウ サポート](https://developer.android.com/guide/topics/ui/multi-window.html)です。



### <a name="enhanced-notifications"></a>強化された通知

Android Nougat には、再設計された通知システムが導入されています。 ユーザー通知の受信のテキスト メッセージ通知 UI 内で直接すばやく応答するできるようにする新しい直接応答機能を備えています。 Android 7.0 では、メッセージまとめて、1 つのグループとして複数のメッセージが受信したときに通知を開始しています。 また、開発者は、ビュー、通知では、システムの文字装飾を活用し、通知を生成するときに、新しい通知テンプレートの利用の通知をカスタマイズできます。


#### <a name="direct-reply"></a>直接応答

ユーザーには、受信メッセージの通知が受信したときに Android Nougat できるようになります通知内でのメッセージに返信 (なく応答を送信するメッセージ アプリを開きます)。
このインライン応答機能により、ユーザーが、SMS またはテキスト メッセージ通知インターフェイス内で直接に迅速に対応します。

[![直接応答フィールドがインラインの通知のスクリーン ショット](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

アプリでこの機能をサポートするために追加する必要があります*インライン応答アクション*を使用してアプリを[RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/)オブジェクトのユーザーがテキストを使用して、通知 UI から直接返信できるようにします。
たとえば、次のコードのビルド、`RemoteInput`テキスト入力を受け取ると、ビルドの応答アクションの保留中のインテントとリモートの入力の有効なアクションを作成します。

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

この操作には、通知が追加されます。

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[メッセージング サービス](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/)サンプル アプリには使用して通知を拡張する方法を示す c# コードが含まれています、`RemoteInput`オブジェクト。 インライン応答の追加の詳細については、アプリを操作 Android 7.0 またはそれ以降を参照してください、Android[通知に応答する](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct)トピックです。


#### <a name="bundled-notifications"></a>バンドルされている通知

Android Nougat は (たとえば、メッセージのトピック) によって通知メッセージをグループ化し、各メッセージではなく、グループを表示できます。
これは、*通知をバンドル*機能により、ユーザーを消去または 1 つのアクションでの通知のグループをアーカイブすることも可能です。 ユーザーは、展開の詳細の各通知の表示への通知のバンドルをダウン スライドできます。

[![バンドルされている通知のスクリーン ショットの例](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

バンドルされている通知をサポートするアプリケーションで使用できる、 [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/)のような通知をバンドルする方法です。 Android N でバンドルされた通知グループの詳細については、Android を参照してください。[バンドル通知](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle)トピックです。


#### <a name="custom-views"></a>カスタム ビュー

Android Nougat によりシステム通知ヘッダーは、アクション、および展開可能なレイアウトを使用してカスタムの通知ビューを作成します。 Android Nougat でカスタムの通知ビューの詳細については、Android を参照してください。[通知の機能強化](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements)トピックです。



### <a name="data-saver"></a>データ セーバー

Android Nougat から始まり、ユーザーが有効にする新しい*データ セーバー*ブロックがデータの使用状況を背景を設定します。 この設定は、可能な限り、フォア グラウンドでの低いデータを使用するアプリにも通知します。 [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/)が Android Nougat で拡張されているアプリは、アプリがデータ セーバーを有効にすると、データ使用量を制限するためにできるように、ユーザーがデータ セーバーを有効かどうかをチェックできるようにします。

新しい Android Nougat データ セーバー機能に関する詳細については、Android を参照してください。[ネットワーク データの使用状況を最適化する](https://developer.android.com/training/basics/network-ops/data-saver.html)トピックです。



### <a name="app-shortcuts"></a>アプリのショートカット

導入された android 7.1、*アプリ ショートカット*すばやく開始一般的なタスクや推奨されるタスクでアプリをユーザーができるようにする機能。
ショートカット メニューをアクティブ化する、ユーザー時間の長い-が、アプリ アイコン秒以上&ndash;クイック振動でメニューが表示されます。
ままにして、メニューをにより、キーを押してを解放します。

[![メッセージング アプリケーションのアプリのショートカット メニューは、画面の例](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

この機能は、使用可能な API レベルのみ 25 以上です。
Android 7.1 の新機能でアプリのショートカットの詳細については、Android を参照してください。[アプリ ショートカット](https://developer.android.com/guide/topics/ui/shortcuts.html)トピックです。


### <a name="sample-code"></a>サンプル コード

Xamarin.Android サンプルがいくつかは Android Nougat 機能を活用する方法を利用できます。

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Android Nougat で使用できる複数のウィンドウの API の使用例を示します。 サンプル アプリは、アプリのライフ サイクルと動作のしくみを表示する複数の windows モードに切り替えることができます。

-   [メッセージング サービス](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/)通知を送信する簡単なサービスを使用して、`NotificationCompatManager`です。 含む通知が拡張されても、 `RemoteInput` Android Nougat デバイス アプリを開かなくても、通知から直接テキストで返信を可能にするオブジェクト。

-   [アクティブな通知](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/)を使用する方法を示します、 `NotificationManager` API をアプリケーションが現在表示通知の数がわかります。

-   [ディレクトリ アクセスのスコープ](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/)スコープを持つディレクトリ アクセス API を使用して、特定のディレクトリを簡単にアクセスする方法を示しています。 これには、代わりに定義することを`READ_EXTERNAL_STORAGE`または`WRITE_EXTERNAL_STORAGE`マニフェストでアクセスを許可します。

-   [ブートの直接](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/)常に使用される、デバイスがブートされる両方前に、と後、ユーザー credentials(PIN/Pattern/Password) が入力中にデバイスで暗号化された記憶域にデータを格納する方法を示しています。


## <a name="summary"></a>まとめ

ここでは、Android Nougat を導入し、インストールして、Android Nougat で最新のツールおよび Xamarin.Android 開発用のパッケージを構成する方法を説明します。 Android Nougat 用アプリの作成を開始するためのソース コードの例へのリンクの Android Nougat で使用できる主な機能の概要についても提供されます。


## <a name="related-links"></a>関連リンク

- [開発者用の android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
