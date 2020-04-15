---
title: Nougat の機能
description: Xamarin.Android を使用して Android Nougat 用のアプリの開発を始める方法。
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 6274c75abf229268070d495ced662724f5c16627
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027092"
---
# <a name="nougat-features"></a>Nougat の機能

_Xamarin.Android を使用して Android Nougat 用のアプリの開発を始める方法。_

この記事では、Android Nougat で導入された機能の概要と、Android Nougat 開発用に Xamarin.Android を準備する方法について説明し、Xamarin.Android アプリで Android Nougat の機能を使用する方法がわかるサンプル アプリケーションへのリンクを示します。

## <a name="overview"></a>概要

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) は、Google の Android 6.0 Marshmallow の後継です。 Xamarin.Android では、Xamarin Android 7.0 以降での **Android 7.x バインド**がサポートされています。 Android Nougat では、以下で説明する Nougat 機能用に多くの新しい API が追加されています。Xamarin.Android 7.0 を使用すると、これらの API を Xamarin.Android アプリで利用できます。

[![Android Nougat が実行されている Android タブレットと Android フォンのヒーロー画像](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x API の詳細については、[開発者向けの Android 7.1](https://developer.android.com/preview/api-overview.html)に関する記事を参照してください。
Xamarin.Android 7.0 の既知の問題の一覧については、[リリース ノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)を参照してください。

Android Nougat では、Xamarin.Android 開発者にとって関心のある多くの新機能が提供されています。 これには次の機能があります。

- **複数ウィンドウのサポート** &ndash; この機能強化により、ユーザーは 2 つのアプリを一度に開くことができるようになります。

- **通知の機能強化** &ndash; Android Nougat の再設計された通知システムには、ユーザーが通知 UI から直接テキスト メッセージに迅速に応答できる "*ダイレクト リプライ*" 機能が含まれています。 また、受信したメッセージに対する通知をアプリで作成する場合は、新しい "*バンドルされた通知*" 機能を使用して、複数のメッセージを受信したときに、1 つのグループとして通知をまとめることができます。

- **データ セーバー** &ndash; この機能は新しいシステム サービスであり、アプリによる携帯データ ネットワークの使用を減らすのに役立ちます。これにより、ユーザーは、アプリによる携帯データ ネットワークの使用方法を制御できるようになります。

さらに、Android Nougat で導入されたアプリ開発者向けの他の機能強化としては、新しいネットワーク セキュリティ構成機能、Doze on the Go、キーの構成証明、新しいクイック設定 API、マルチロケールのサポート、ICU4J API、WebView の機能強化、Java 8 言語機能へのアクセス、スコープ指定されたディレクトリ アクセス、カスタム ポインター API、プラットフォーム VR のサポート、仮想ファイル、バックグラウンド処理の最適化などがあります。

この記事では、Android Nougat で新機能を試すためのアプリの構築を始める方法と、新しい Android Nougat プラットフォームを対象とする移行や機能の作業を計画する方法を説明します。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android Nougat の機能を使用するには、次のものが必要です。

- **Visual studio または Visual Studio for Mac** &ndash; Visual Studio を使用している場合は、Visual Studio Tools for Xamarin のバージョン4.2.0.628 以降が必要です。 Visual Studio for Mac を使用している場合は、バージョン 6.1.0 以降の Visual Studio for Mac が必要です。

- **Xamarin.Android** &ndash; Visual Studio または Visual Studio for Mac に Xamarin.Android 7.0 以降をインストールして、構成する必要があります。

- **Android SDK** - Android SDK 7.0 (API 24) 以降を Android SDK マネージャーでインストールする必要があります。

- **Java Developer Kit** &ndash; API レベル 24 以降向けに開発する場合 (JDK 8 では 24 より前の API レベルもサポートされています)、Xamarin.Android 7.0 の開発には、[JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 以降が必要です。 カスタム コントロールまたは Forms プレビューアーを使用する場合は、64 ビット版の JDK 8 が必要です。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

Android Nougat で確実に動作させるには、Xamarin C6SR4 以降を使用してアプリをリビルドする必要があることに注意してください。 Android Nougat は、[NDK 提供のネイティブ ライブラリ](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)のみにリンクできるため、**Mono.Data.Sqlite.dll** などのライブラリを使用している既存のアプリは、適切にリビルドされていない場合、Android で実行するとクラッシュすることがあります。

## <a name="getting-started"></a>作業の開始

Android Nougat と Xamarin.Android の使用を始めるには、Android Nougat プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. Xamarin から最新の Xamarin.Android 更新プログラムをインストールします。

2. **Android 7.0 (API 24)** 以降のパッケージとツールをインストールします。

3. Android Nougat を対象とする新しい Xamarin.Android プロジェクトを作成します。

4. Android Nougat 用にエミュレーターまたはデバイスを構成します。

これらの各手順を以下のセクションで説明します。

### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムをインストールする

Android Nougat 用の Xamarin のサポートを追加するには、Visual Studio または Visual Studio for Mac の更新チャネルを安定チャネルに変更し、最新の更新プログラムを適用します。 現在、アルファ チャネルまたはベータ チャネルでのみ使用できる機能が必要な場合は、アルファ チャネルまたはベータ チャネルに切り替えることができます (アルファ チャネルとベータ チャネルでも Android 7.x がサポートされています)。 更新プログラム (リリース) チャネルを変更する方法の詳細については、[更新チャネルの変更](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)に関するページを参照してください。

### <a name="install-the-android-sdk"></a>Android SDK をインストールする

Xamarin.Android 7.0 を使用してプロジェクトを作成するには、最初に Android SDK マネージャーを使用して **SDK Platform Android N (API 24)** 以降をインストールする必要があります。 また、最新の **Android SDK Tools** もインストールする必要があります。

1. Android SDK マネージャーを起動します (Visual Studio for Mac では **ツール > Android SDK マネージャーを開く&hellip;** を使用し、Visual Studio では **ツール > Android > Android SDK マネージャー** を使用)。

2. **Android 7.0 (API 24)** 以降をインストールします。

    [![Android SDK マネージャーでの Android 7.0 パッケージの選択](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3. 最新の Android SDK Tools をインストールします。

    [![Android SDK マネージャーでの最新の Android SDK Tools の選択](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools リビジョン 25.2.2 以降、Android SDK プラットフォーム ツール 24.0.3 以降、Android SDK ビルド ツール 24.0.2 以降をインストールする必要があります。

4. **Java Development Kit の場所**が JDK 1.8 用に構成されていることを確認します。

    [![[ツール] オプションでの JDK 8 のパスの構成](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio でこの設定を表示するには、 **[ツール] > [オプション] > [Xamarin] > [Android 設定]** をクリックします。 Visual Studio for Mac では、 **[ユーザー設定] > [プロジェクト] > [SDK の場所] > [Android]** をクリックします。

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始する

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android の開発を初めて行う場合は、「[Hello, Android](~/android/get-started/hello-android/index.md)」を参照して、Xamarin.Android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、Android 7.0 以降を対象とするようにバージョン設定を構成する必要があります。 たとえば、Android 7.0 をプロジェクトの対象にするには、プロジェクトのターゲット Android API レベルを **Android 7.0 (API 24 - Nougat)** に構成する必要があります。 ターゲット フレームワーク レベルを API 24 以降に設定することをお勧めします。 Android API レベルの構成について詳しくは、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください。

> [!NOTE]
> 現時点では、Android Nougat デバイスまたはエミュレーターにアプリを展開するには、 **[最低限の Android バージョン]** を **[Android 7.0 (API 24 - Nougat)]** に設定する必要があります。

### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成する

エミュレーターを使用している場合は、Android AVD マネージャーを開始し、次の設定を使用して新しいデバイスを作成します。

- デバイス:Nexus 5X、Nexus 6、Nexus 6P、Nexus Player、Nexus 9、または Pixel C。
- ターゲット:Android 7.0 - API レベル 24
- ABI: x86 または x86\_64

たとえば、次の仮想デバイスは、Nexus 6 をエミュレートするように構成されています。

[![Nexus 6 デバイス、Android 7.0 ターゲット、Intel Atom x86 CPU/ABI を使用して AVD を構成する](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Nexus 5X、6、9 などの物理デバイスを使用している場合は、自動無線 (OTA) 更新でデバイスを更新するか、システム イメージをダウンロードしてデバイスを直接フラッシュすることができます。 Android Nougat へのデバイスの手動更新について詳しくは、[Nexus デバイス用の OTA イメージ](https://developers.google.com/android/nexus/ota)に関するページを参照してください。

Android Nougat では Nexus 5 デバイスがサポートされていないことに注意してください。

## <a name="new-features"></a>新機能

Android Nougat では、マルチウィンドウのサポート、通知の強化、データ セーバーなど、さまざまな新機能が導入されています。 以下のセクションでは、これらの機能について説明し、アプリでの使用を開始するのに役立つリンクを示します。

### <a name="multi-window-mode"></a>マルチウィンドウ モード

マルチウィンドウ モードを使用すると、ユーザーは完全なマルチタスクのサポートで、2 つのアプリを同時に開くことができます。 これらのアプリは、分割画面モードでは左右 (横) または上下 (縦) に並べて実行できます。
ユーザーは、アプリの間の区切り線をドラッグしてサイズを変更できます。また、アプリ間でコンテンツを切り取って貼り付けることができます。 2 つのアプリがマルチウィンドウ モードで表示されている場合、選択されたアクティビティが実行し続けている間、選択されていないアクティビティは一時停止しますが、引き続き表示されます。 マルチウィンドウ モードでは、Android アクティビティのライフサイクルは変更されません。

[![縦と横のマルチウィンドウ モードで実行されているアプリの例](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin.Android アプリのアクティビティによるマルチウィンドウ モードのサポート方法を構成できます。 たとえば、マルチウィンドウ モードでのアプリの最小サイズおよび既定の高さと幅を設定する属性を構成できます。 新しい `Activity.IsInMultiWindowMode` プロパティを使用して、アクティビティがマルチウィンドウ モードかどうかを判断できます。 次に例を示します。

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) サンプル アプリには、アプリで複数のウィンドウ ユーザー インターフェイスを利用する方法を示す C# コードが含まれています。

マルチウィンドウ モードについて詳しくは、「[マルチウィンドウのサポート](https://developer.android.com/guide/topics/ui/multi-window.html)」を参照してください。

### <a name="enhanced-notifications"></a>強化された通知

Android Nougat では、再設計された通知システムが導入されています。 ユーザーが通知 UI で直接、受け取ったテキスト メッセージにすばやく返信できる、新しいダイレクト リプライ機能があります。 Android 7.0 以降では、複数のメッセージを受け取ったときに、通知メッセージを 1 つのグループとしてまとめることができます。 また、開発者は、通知ビューをカスタマイズしたり、通知でシステムの装飾を利用したり、通知を生成するときに新しい通知テンプレートを利用したりできます。

#### <a name="direct-reply"></a>ダイレクト リプライ

Android Nougat では、受信メッセージの通知を受け取ったユーザーは、(メッセージング アプリを開いて返信を送信するのではなく) 通知内でメッセージに返信できます。
このインライン リプライ機能により、ユーザーは、通知インターフェイス内で直接 SMS またはテキスト メッセージにすばやく応答できます。

[![インラインの直接返信フィールドが含まれる通知のスクリーンショット](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

アプリでこの機能をサポートするには、[RemoteInput](xref:Android.App.RemoteInput) オブジェクトを使用して "*インライン返信アクション*" をアプリに追加し、ユーザーが通知 UI から直接テキストで返信できるようにする必要があります。
たとえば、次のコードでは、テキスト入力を受け取るための `RemoteInput` を作成し、返信アクションのための保留中意図を作成して、リモート入力を有効にしたアクションを作成しています。

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

[Messaging Service](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) サンプル アプリには、`RemoteInput` オブジェクトを使用して通知を拡張する方法を示す C# コードが含まれています。 Android 7.0 以降向けのアプリにインライン リプライ アクションを追加する方法について詳しくは、[通知への返信](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct)に関するトピックを参照してください。

#### <a name="bundled-notifications"></a>バンドルされた通知

Android Nougat では、通知メッセージをグループ化 (たとえば、メッセージ トピック別) して、個別のメッセージではなくグループを表示することができます。
この "*バンドル化された通知*" 機能を使用すると、ユーザーは 1 回の操作で通知のグループを消去またはアーカイブできます。 ユーザーは、下へスライドして通知のバンドルを展開し、各通知を詳細に表示できます。

[![バンドル通知のスクリーンショットの例](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

アプリでバンドルされた通知をサポートするには、[Builder.SetGroup](xref:Android.App.Notification.Builder.SetGroup*) メソッドを使用して似た通知をバンドルすることができます。 Android N でのバンドルされた通知グループの詳細については、[通知のバンドル](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle)に関するトピックを参照してください。

#### <a name="custom-views"></a>カスタム ビュー

Android Nougat では、システム通知ヘッダー、アクション、展開可能なレイアウトを含むカスタム通知ビューを作成できます。 Android Nougat でのカスタム通知ビューについて詳しくは、Android の「[通知の機能強化](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements)」トピックを参照してください。

### <a name="data-saver"></a>データ セーバー

Android Nougat 以降では、ユーザーは、バックグラウンドでのデータの使用をブロックする新しい "*データ セーバー*" の設定を有効にすることができます。 また、この設定では、可能な限りフォアグラウンドでのデータ使用を少なくするようにアプリに通知されます。 Android Nougat では [ConnectivityManager](xref:Android.Net.ConnectivityManager) が拡張されており、ユーザーがデータ セーバーを有効にしているかどうかをアプリで確認できます。これにより、データ セーバーが有効になっているときに、アプリでデータ使用量を制限できるようになります。

Android Nougat の新しいデータ セーバー機能について詳しくは、Android の「[ネットワーク データ使用量を最適化する](https://developer.android.com/training/basics/network-ops/data-saver.html)」トピックを参照してください。

### <a name="app-shortcuts"></a>アプリのショートカット

Android 7.1 で導入された "*アプリのショートカット*" 機能を使用すると、ユーザーは一般的なタスクや推奨されるタスクをアプリですばやく開始できます。
ショートカットのメニューをアクティブ化するには、アプリ アイコンを 1 秒以上長押しします。細かい振動と共にメニューが表示されます。
押すのを止めても、メニューは表示されたままになります。

[![メッセージング アプリのアプリのショートカット メニューのサンプル画面](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

この機能は、API レベル 25 以上でのみ使用できます。
Android 7.1 での新しいアプリのショートカット機能について詳しくは、Android の[アプリのショートカット](https://developer.android.com/guide/topics/ui/shortcuts.html)に関するトピックを参照してください。

### <a name="sample-code"></a>サンプル コード

Android Nougat の機能を使用する方法がわかる複数の Xamarin.Android サンプルを利用できます。

- [MultiWindowPlayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) では、Android Nougat で使用できるマルチウィンドウ API の使用方法が示されています。 サンプル アプリをマルチウィンドウ モードに切り替えて、アプリのライフサイクルと動作にどのように影響するかを確認できます。

- [Messaging Service](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) は、`NotificationCompatManager` を使用して通知を送信する簡単なサービスです。 また、`RemoteInput` オブジェクトで通知が拡張されており、Android Nougat デバイスでアプリを開くことなく、通知から直接テキストで返信できます。

- [Active Notifications](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications) では、`NotificationManager` API を使用して、アプリケーションで現在表示されている通知の数を知る方法がわかります。

- [Scoped Directory Access](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess) では、特定のディレクトリに簡単にアクセスするためにスコープ付きディレクトリ アクセス API を使用する方法が示されています。 これは、マニフェストで `READ_EXTERNAL_STORAGE` または `WRITE_EXTERNAL_STORAGE` アクセス許可を定義することに対する代替手段として機能します。

- [Direct Boot](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot) では、デバイスで暗号化されたストレージにデータを格納する方法が示されています。これは、ユーザーの資格情報 (PIN、パターン、パスワード) が入力される前と後の両方で、デバイスが起動されている間に常に使用できます。

## <a name="summary"></a>まとめ

この記事では、Android Nougat について紹介し、Android Nougat での Xamarin.Android 開発用に最新のツールとパッケージをインストールして構成する方法について説明しました。 また、Android Nougat で使用できる主な機能の概要と、Android Nougat 用のアプリの作成を開始する際に役立つサンプル ソース コードへのリンクも示しました。

## <a name="related-links"></a>関連リンク

- [開発者向け Android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 リリース ノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
