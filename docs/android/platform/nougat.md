---
title: Nougat の機能
description: Android Nougat 用のアプリを開発するために Xamarin の使用を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: a28368e0fa4574fbb92a43dbd650a127008f5d06
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643461"
---
# <a name="nougat-features"></a>Nougat の機能

_Android Nougat 用のアプリを開発するために Xamarin の使用を開始する方法について説明します。_

この記事では、Android Nougat で導入された機能の概要を説明し、android Nougat 開発用に Xamarin を準備する方法について説明します。また、で Android Nougat 機能を使用する方法を示すサンプルアプリケーションへのリンクを示します。Xamarin Android アプリ。


## <a name="overview"></a>概要

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html)は、Google が Android 6.0 Marshmallow にフォローアップします。 Xamarin android 7.0 以降では、Android 2.x の**バインド**がサポートされています。 Android Nougat では、以下に説明する Nougat 機能用に多くの新しい Api が追加されています。xamarin android 7.0 を使用すると、これらの Api を Xamarin Android アプリで使用できます。

[![Android タブレットと android Nougat を実行している携帯電話のヒーロー画像](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 2.x Api の詳細については、「[開発者向けの android 7.1](https://developer.android.com/preview/api-overview.html)」を参照してください。
既知の Xamarin. Android 7.0 の問題の一覧については、[リリースノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)を参照してください。

Android Nougat は、Xamarin Android 開発者にとって関心のある多くの新機能を提供します。 これには次の機能があります。

-   **複数ウィンドウのサポート**&ndash;この機能強化により、ユーザーは、画面上で2つのアプリを一度に開くことができるようになります。

-   **通知の機能強化**Android Nougat の再設計された通知システムには、ユーザーが通知 UI から直接テキストメッセージにすばやく応答できるようにする*直接応答*機能が含まれています。 &ndash; また、受信したメッセージに対してアプリが通知を作成した場合、新しいバンドルされた*通知*機能では、複数のメッセージを受信したときに1つのグループとして通知をまとめてまとめることができます。

-   **データセーバー**&ndash;この機能は、アプリによる携帯データネットワークの使用量を削減するために役立つ新しいシステムサービスです。これにより、ユーザーは、アプリが携帯データを使用する方法を制御できるようになります。

さらに、Android Nougat は、新しいネットワークセキュリティ構成機能、外出先での Doze、キーの構成証明、新しいクイック設定 Api、マルチロケールサポート、ICU4J Api、web ビューの機能強化など、アプリ開発者に関心のある他の多くの機能強化をもたらします。Java 8 言語機能、スコープを持つディレクトリアクセス、カスタムポインター API、platform VR サポート、仮想ファイル、およびバックグラウンド処理の最適化にアクセスできます。

この記事では、Android Nougat を使用してアプリの構築を開始する方法について説明します。新しい機能を試すことができます。また、新しい Android Nougat プラットフォームを対象とする移行や機能の作業を計画します。


## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android Nougat 機能を使用するには、次のものが必要です。

-   **Visual Studio または Visual Studio for Mac**&ndash; Visual Studio を使用している場合は、Xamarin 用の Visual Studio Tools バージョン4.2.0.628 以降が必要です。 Visual Studio for Mac を使用している場合は、Visual Studio for Mac のバージョン6.1.0 以降が必要です。

-   **Xamarin android** &ndash; 7.0 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。

-   Android SDK Manager を使用して**Android SDK** -Android SDK 7.0 (API 24) 以降をインストールする必要があります。

-   **Java Developer Kit**API レベル24以上を開発している場合、Xamarin Android 7.0 開発には[jdk 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降が必要です (jdk 8 では、24より前の api レベルもサポートされています)。 &ndash; カスタムコントロールまたはフォームプレビューアーを使用する場合は、64ビットバージョンの JDK 8 が必要です。

> [!IMPORTANT]
> Xamarin.Android は JDK 9 をサポートしていません。

Android Nougat で確実に動作させるには、Xamarin C6SR4 以降を使用してアプリを再構築する必要があることに注意してください。 Android Nougat は、 [NDK によって提供されるネイティブライブラリ](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)にのみリンクできるため、適切に再構築されていない場合、android Nougat で実行すると、 **Mono**などのライブラリを使用する既存のアプリがクラッシュすることがあります。



## <a name="getting-started"></a>作業の開始

Android Nougat と Xamarin android の使用を開始するには、Android Nougat プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1.  Xamarin から最新の Xamarin Android 更新プログラムをインストールします。

2.  **Android 7.0 (API 24)** パッケージとツール以降をインストールします。

3.  Android Nougat を対象とする新しい Xamarin Android プロジェクトを作成します。

4.  Android Nougat 用のエミュレーターまたはデバイスを構成します。

これらの各手順については、次のセクションで説明します。


### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムのインストール

Android Nougat の Xamarin サポートを追加するには、Visual Studio または Visual Studio for Mac の更新チャネルを安定したチャネルに変更し、最新の更新プログラムを適用します。 現在、アルファチャネルまたはベータチャネルでのみ使用できる機能が必要な場合は、アルファチャネルまたはベータチャネルに切り替えることができます (アルファチャネルとベータチャネルは Android 2.x もサポートしています)。 更新プログラム (リリース) チャネルを変更する方法の詳細については、「[更新チャネルの変更](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)」を参照してください。



### <a name="install-the-android-sdk"></a>Android SDK のインストール

Xamarin Android 7.0 を使用してプロジェクトを作成するには、最初に Android SDK マネージャーを使用して**SDK Platform Android N (API 24)** 以降をインストールする必要があります。 また、最新の**Android SDK Tools**もインストールする必要があります。

1.  Android SDK マネージャーを起動します (Visual Studio for Mac で、**ツール > 使用し&hellip;て Android SDK マネージャーを開き**ます。 Visual Studio では、**ツール > Android > Android SDK マネージャー**) を使用します。

2.  **Android 7.0 (API 24)** 以降をインストールします。

    [![Android SDK Manager での Android 7.0 パッケージの選択](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  最新の Android SDK ツールをインストールします。

    [![Android SDK マネージャーで最新の Android SDK ツールを選択する](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools revision 25.2.2 以降、Android SDK Platform tools v24.0.3 以降、Android SDK Build Tools 24.0.2 以降をインストールする必要があります。

4.  **Java Development Kit の場所**が JDK 1.8 用に構成されていることを確認します。

    [![[ツール] オプションの下に JDK 8 パスを構成する](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio でこの設定を表示するには、**ツール > オプション > Xamarin > Android 設定** の順にクリックします。 Visual Studio for Mac で、設定 をクリックして、 **Android > > SDK の場所の > プロジェクト** をクリックします。



### <a name="start-a-xamarinandroid-project"></a>Xamarin. Android プロジェクトを開始する

新しい Xamarin. Android プロジェクトを作成します。 Xamarin を使用した Android 開発を初めて使用する場合は、「 [Hello, android](~/android/get-started/hello-android/index.md) 」を参照して、xamarin android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、バージョン設定を Android 7.0 以降を対象とするように構成する必要があります。 たとえば、Android 7.0 のプロジェクトを対象にするには、プロジェクトのターゲットの Android API レベルを**android 7.0 (API 24-Nougat)** に構成する必要があります。 ターゲットフレームワークレベルを API 24 以降に設定することをお勧めします。 Android API レベルレベルの構成の詳細については、「 [ANDROID Api レベルについ](~/android/app-fundamentals/android-api-levels.md)て」を参照してください。


> [!NOTE]
> 現時点では、android Nougat デバイスまたはエミュレーターにアプリをデプロイするには、android の**最小バージョン**を**ANDROID 7.0 (API 24 Nougat)** に設定する必要があります。



### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成する

エミュレーターを使用している場合は、Android AVD マネージャーを起動し、次の設定を使用して新しいデバイスを作成します。

-   デバイス:"月 ~ 金"、""、""、""、""、""、または "ピクセル C" のようにします。
-   ターゲット:Android 7.0-API レベル24
-   ABI: x86 または\_x86 64

たとえば、この仮想デバイスは、次のようにして、この仮想デバイスを構成します。

[![[の接続6デバイス、Android 7.0 ターゲット、および Intel Atom x86 CPU/ABI を使用した AVD の構成]](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

複数の物理デバイスを使用している場合は、航空会社の無線 (OTA) 更新プログラムで自動でデバイスを更新するか、システムイメージをダウンロードしてデバイスを直接フラッシュすることができます。 デバイスを Android Nougat に手動で更新する方法の詳細については、「[デバイスのデバイスの OTA イメージ](https://developers.google.com/android/nexus/ota)」を参照してください。

Android Nougat では、対応する5つのデバイスがサポートされていないことに注意してください。



## <a name="new-features"></a>新機能

Android Nougat では、複数のウィンドウのサポート、通知の強化、データセーバーなど、さまざまな新機能が導入されています。 以下のセクションでは、これらの機能について説明し、アプリでの使用を開始するのに役立つリンクを示します。



### <a name="multi-window-mode"></a>複数ウィンドウモード

複数ウィンドウモードを使用すると、ユーザーはフルマルチタスキングをサポートすることで、2つのアプリを同時に開くことができます。 これらのアプリは、分割画面モードでサイドバイサイド (横) または1つ上 (縦) で実行できます。
ユーザーは、アプリ間で区分線をドラッグしてサイズを変更できます。また、アプリ間でコンテンツを切り取って貼り付けることができます。 2つのアプリがマルチウィンドウモードで表示されている場合、選択されていないアクティビティは、選択されていないアクティビティが一時停止されていても引き続き表示されます。 マルチウィンドウモードでは、Android アクティビティのライフサイクルは変更されません。

[![縦と横の両方でマルチウィンドウモードで実行されているアプリの例](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin Android アプリのアクティビティが複数のウィンドウモードをサポートする方法を構成できます。 たとえば、アプリの最小サイズと既定の高さおよび幅をマルチウィンドウモードで設定する属性を構成できます。 新しい`Activity.IsInMultiWindowMode`プロパティを使用して、アクティビティが複数ウィンドウモードであるかどうかを判断できます。 例:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[Multiwindowplayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground)サンプルアプリにはC# 、アプリで複数のウィンドウユーザーインターフェイスを活用する方法を示すコードが含まれています。

マルチウィンドウモードの詳細については、[マルチウィンドウのサポート](https://developer.android.com/guide/topics/ui/multi-window.html)に関する説明を参照してください。



### <a name="enhanced-notifications"></a>強化された通知

Android Nougat では、再設計された通知システムが導入されています。 この機能には、ユーザーが通知 UI で直接受信したテキストメッセージの通知にすばやく応答できるようにする新しい直接応答機能があります。 Android 7.0 以降では、複数のメッセージを受信したときに、通知メッセージを1つのグループとしてまとめることができます。 また、通知ビューをカスタマイズしたり、通知のシステム装飾を利用したり、通知の生成時に新しい通知テンプレートを活用したりできます。


#### <a name="direct-reply"></a>直接応答

ユーザーが受信メッセージの通知を受信すると、Android Nougat は、(応答を送信するためにメッセージングアプリを開くのではなく) 通知内のメッセージに返信できるようになります。
このインライン応答機能により、ユーザーは、通知インターフェイス内で直接 SMS またはテキストメッセージにすばやく応答できます。

[![インラインの直接返信フィールドを含む通知のスクリーンショット](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

アプリでこの機能をサポートするには、ユーザーが通知 UI から直接テキストで返信できるように、 [Remoteinput](xref:Android.App.RemoteInput)オブジェクトを使用して*インライン応答アクション*をアプリに追加する必要があります。
たとえば、次のコードは、テキスト`RemoteInput`入力を受信するためのを構築し、応答アクションに対して保留中のインテントを構築し、リモート入力を有効にしたアクションを作成します。

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

[Messaging Service](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice)サンプルアプリにはC# 、 `RemoteInput`オブジェクトを使用して通知を拡張する方法を示すコードが含まれています。 Android 7.0 以降のアプリにインライン応答アクションを追加する方法の詳細については、「Android[への通知へ](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct)の応答」を参照してください。


#### <a name="bundled-notifications"></a>バンドル通知

Android Nougat では、通知メッセージをグループ化する (たとえば、メッセージトピック別) ことができ、個別のメッセージではなくグループを表示できます。
この*バンドル*された通知機能を使用すると、ユーザーは1回の操作で通知のグループを消去またはアーカイブできます。 ユーザーは、下へスライドして通知のバンドルを展開し、各通知を詳細に表示できます。

[![バンドル通知のスクリーンショットの例](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

バンドルされた通知をサポートするために、アプリでは、[ビルダー. SetGroup](xref:Android.App.Notification.Builder.SetGroup*)メソッドを使用して同様の通知をバンドルできます。 Android N でバンドルされている通知グループの詳細については、「Android の[バンドル通知](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle)」を参照してください。


#### <a name="custom-views"></a>カスタム ビュー

Android Nougat を使用すると、システム通知ヘッダー、アクション、および展開可能なレイアウトでカスタム通知ビューを作成できます。 Android Nougat のカスタム通知ビューの詳細については、「Android [notification の拡張機能](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements)」を参照してください。



### <a name="data-saver"></a>データセーバー

Android Nougat 以降では、ユーザーは、バックグラウンドデータの使用をブロックする新しい*データセーバー*設定を有効にすることができます。 また、この設定により、可能な限り、フォアグラウンドで使用するデータが少なくなるようにアプリに通知されます。 [ConnectivityManager](xref:Android.Net.ConnectivityManager)は Android Nougat で拡張されています。これにより、アプリはデータセーバーが有効になっているかどうかを確認できるようになり、データセーバーが有効になっているときにアプリでデータ使用量を制限できるようになりました。

Android Nougat の新しいデータセーバー機能の詳細については、「Android の[ネットワークデータ使用量の最適化](https://developer.android.com/training/basics/network-ops/data-saver.html)」を参照してください。



### <a name="app-shortcuts"></a>アプリのショートカット

Android 7.1 では*アプリのショートカット*機能が導入され、ユーザーはアプリを使用して一般的なタスクや推奨されるタスクをすばやく開始できるようになりました。
ショートカットのメニューをアクティブ化するには、ユーザーが1つ&ndash;以上のメニューのアプリアイコンを長い時間押し、クイック振動で表示します。
押されると、メニューが残ります。

[![メッセージングアプリのアプリショートカットメニューのサンプル画面](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

この機能は、API レベル25以上でのみ使用できます。
Android 7.1 の新しいアプリショートカット機能の詳細については、「Android[アプリのショートカット](https://developer.android.com/guide/topics/ui/shortcuts.html)」を参照してください。


### <a name="sample-code"></a>サンプル コード

Android Nougat 機能を活用する方法を示すために、いくつかの Xamarin Android サンプルを利用できます。

-   [Multiwindowplayground グラウンド](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground)は、Android Nougat で使用できるマルチウィンドウ API の使用方法を示しています。 サンプルアプリをマルチ windows モードに切り替えて、アプリのライフサイクルと動作にどのように影響するかを確認できます。

-   [メッセージングサービス](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice)は、 `NotificationCompatManager`を使用して通知を送信する単純なサービスです。 また、 `RemoteInput`オブジェクトを使用して通知を拡張し、Android Nougat デバイスがアプリを開いていなくても、通知から直接テキストを使用して応答できるようにします。

-   [アクティブな通知](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications)は、API を`NotificationManager`使用して、アプリケーションが現在表示している通知の数を通知する方法を示しています。

-   スコープが指定した[ディレクトリアクセス](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess)スコープが指定されたディレクトリアクセス API を使用して、特定のディレクトリに簡単にアクセスする方法を示します。 これは、マニフェストでまたは`READ_EXTERNAL_STORAGE` `WRITE_EXTERNAL_STORAGE`権限を定義する必要がある代わりに機能します。

-   [直接ブート](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot)デバイスで暗号化されたストレージにデータを格納する方法を説明します。この記憶域は、ユーザーの資格情報 (PIN/パターン/パスワード) が入力される前と後の両方でデバイスが起動されている間は常に使用できます。


## <a name="summary"></a>まとめ

この記事では、Android Nougat について紹介し、android Nougat で Xamarin の開発用の最新のツールとパッケージをインストールして構成する方法について説明しました。 また、android Nougat で使用できる主な機能の概要と、Android Nougat 用アプリの作成を開始する際に役立つサンプルソースコードへのリンクも示しました。


## <a name="related-links"></a>関連リンク

- [開発者向けの Android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 リリースノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
