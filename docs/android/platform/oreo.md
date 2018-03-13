---
title: "Oreo 機能"
description: "Android の最新バージョンのアプリを開発する Xamarin.Android を使用して作業を開始する方法。"
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 03be7b624ffa9dd8774f291b96be27499cccab2b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="oreo-features"></a>Oreo 機能

_Android の最新バージョンのアプリを開発する Xamarin.Android を使用して作業を開始する方法。_

[Android 8.0 Oreo](https://developer.android.com/index.html)は Android の最新バージョンを Google から使用できます。 Android Oreo Xamarin.Android 開発者にとって関心のある多くの新機能を提供します。 これらの機能には、XML、ダウンロード可能なフォント、オートコンプリート、および画像 (PIP) の画像での通知チャネル、通知のバッジ、カスタム フォントが含まれます。 Android Oreo には、これらの新しい機能用の新しい Api が含まれています。 これらの Api は以降 Xamarin.Android アプリ Xamarin.Android 8.0 を使用する場合に利用可能.

[![Android Oreo のヒーローの画像](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

この記事は、Android の 8.0 Oreo 用 Xamarin.Android アプリの開発を開始するために構成されます。 これには、必要な更新プログラムをインストール、SDK を構成およびテスト用エミュレーター (またはデバイス) を作成する方法について説明します。 また、Android Oreo 機能 Xamarin.Android アプリで使用する方法を説明するサンプル アプリへのリンクを Android 8.0 Oreo の新機能の概要を示します。


## <a name="requirements"></a>必要条件

次が機能を使用する Android Oreo Xamarin ベースのアプリに必要です。

-   **Visual Studio** &ndash; 15.5 またはそれ以降の Visual Studio のバージョンが必要な Windows を使用している場合。  Mac を使用している場合は、Visual Studio for Mac 7.2.0 はのバージョンが必要です。

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 or later must be installed and configured with Visual Studio.

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) 以降、Android SDK Manager を使用してをインストールする必要があります。



## <a name="getting-started"></a>作業の開始

Xamarin.Android での Android Oreo の使用開始するには、ダウンロードして、Android Oreo プロジェクトを作成する前に、最新のツールと SDK のパッケージをインストールする必要があります。

1. Visual Studio の最新バージョンに更新します。

2. インストール、 **Android 8.0.0 (API 26)**または以降のパッケージおよびツール、SDK Manager を使用しています。

3. 新しい Xamarin.Android プロジェクトを対象とする Android Oreo (API 26) を作成します。

4. エミュレーターまたは Android Oreo アプリのテスト用のデバイスを構成します。

次のセクションでは、これらの各手順について説明します。



### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio および Xamarin.Android を更新します。

Visual Studio には、Android Oreo サポートを追加するには、次の操作を行います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Visual Studio 2017: 使用する場合 

    1. Visual Studio 2017 15.5 またはそれ以降のバージョンに更新プログラム (を参照してください[更新 Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio))。

    2. SDK Manager を使用して[インストール手順](~/android/get-started/installation/android-sdk.md#installation)Xamarin SDK Manager をインストールします。
       Google にスタンドアロン 26.0 およびそれ以降の API をサポートする GUI SDK manager が指定されていないために、Xamarin SDK Manager をインストールする必要があります。

-   Visual Studio 2015 SDK ツールを 25 にダウン グレードして、使用をお勧めおを使用している場合、古い Google エミュレーター マネージャー GUI します。 SDK ツール 25 では、API 26、27 日以降と共に使用でき、新しいプラットフォームの開発に影響はありません。 こうインターフェイスとの古いバージョンの Android SDK を管理するためです。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   更新して最新の安定したバージョンの Visual Studio 2017 を Mac の説明に従って[Mac 用の Visual Studio の更新](https://docs.microsoft.com/en-us/visualstudio/mac/update)です。

-----

Android Oreo の Xamarin サポートに関する詳細については、次を参照してください。、 [Xamarin.Android 8.0 リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/)です。



### <a name="install-the-android-sdk"></a>Android SDK をインストールします。

Xamarin.Android 8.0 でプロジェクトを作成する必要があります最初を使用して、Xamarin Android SDK Manager のプラットフォーム SDK をインストール**Android 8.0 - Oreo**またはそれ以降。 また、26.0 またはそれ以降、Android SDK ツールをインストールする必要があります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. SDK Manager を開始 (Visual Studio で、[**ツール > Android > Android SDK Manager**)。

2. インストール、 **Android 8.0 - Oreo**パッケージです。 Android SDK エミュレーターを使用している場合を必ず含めて、 **x86**システム イメージを必要があります。

    [![Android SDK Manager で Android 8.0 パッケージを選択します。](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. インストール**Android SDK ツール 26.0.2**またはそれ以降、 **Android SDK プラットフォーム ツール 26.0.0**またはそれ以降、および**Android SDK Build ツール 26.0.0** (またはそれ以降)。

    [![Android SDK Manager で Android SDK ツール 26 を選択します。](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. SDK Manager を開始 (Mac を Visual Studio で [**ツール > SDK Manager**)。

2. インストール、 **Android 8.0 - Oreo** SDK パッケージです。 Android SDK エミュレーターを使用している場合を必ず含めて、 **x86**システム イメージを必要があります。

    [![SDK Manager で Android 8.0 パッケージを選択します。](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. インストール**Android SDK ツール 26.0.2**またはそれ以降、 **Android SDK プラットフォーム ツール 26.0.0**またはそれ以降、および**Android SDK Build ツール 26.0.0** (またはそれ以降)。

    [![SDK Manager で Android SDK ツール 26 を選択します。](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android 開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android プロジェクトの作成について学習します。

Android プロジェクトを作成するときに、Android 8.0 以降をターゲットにバージョン設定を構成する必要があります。 たとえば、Android 8.0 の場合、プロジェクトを対象にする必要がありますレベルを構成するターゲット Android API にプロジェクトの**Android 8.0 (API 26)**です。 API 26 またはそれ以降は、ターゲット フレームワークのレベルを設定することをお勧めします。 Android API レベル レベルの構成に関する詳細は、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。


### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成します。

Android SDK ツール 26.0 をインストールした後既定 Google GUI ベースの AVD マネージャーを起動しようとする場合、後で、次のエラーのダイアログは、コマンド ライン AVD マネージャー ツールを使用するよう指示を get 可能性があります**avdmanager**代わりに:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android エミュレーター マネージャー警告ダイアログ ボックス](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android エミュレーター マネージャー警告ダイアログ ボックス](oreo-images/mac/03-avd-warning.png)

-----

Google がスタンドアロン 26.0 およびそれ以降の API をサポートする GUI AVD マネージャーが用意されていませんので、このメッセージが表示されます。 Android の 8.0 Oreo を Xamarin Android エミュレーター マネージャーまたはコマンドラインを使用する必要があります`avdmanager`Android Oreo の仮想デバイスを作成するツールです。

Xamarin Android デバイス マネージャーを作成し、仮想デバイスの管理を使用するのを参照してください。 [Xamarin Android デバイス マネージャー](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)です。
Xamarin Android エミュレーター マネージャーなしの仮想デバイスを作成するには、次のセクションで、手順を実行します。


#### <a name="creating-virtual-devices-using-avdmanager"></a>仮想デバイスを使用して avdmanager を作成します。

使用する**avdmanager**を新しい仮想デバイスを作成するには、次の手順に従います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  コマンド プロンプト ウィンドウを開き、設定`JAVA_HOME`お使いのコンピューター上の Java SDK の場所にします。 通常 Xamarin のインストールでは、次のコマンドを使用できます。

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Android SDK の場所を追加`bin`フォルダーを`PATH`です。
    通常 Xamarin のインストールでは、次のコマンドを使用できます。

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  コマンド プロンプト ウィンドウを閉じ、新しいコマンド プロンプト ウィンドウを開きます。 使用して新しい仮想デバイスを作成、 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)コマンド。 たとえば、という名前、AVD を作成する**AVD Oreo 8.0** API レベル 26 のシステム イメージ、x86 を使用して、次のコマンドを使用します。

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  メッセージが表示されたら**[いいえ] のカスタムのハードウェア プロファイルを作成する**を入力できます**ありません**し、既定のハードウェア プロファイルをそのまま使用します。 答えた場合**はい**、 **avdmanager**ハードウェア プロファイルをカスタマイズするための質問の一覧が表示されます。

したら**avdmanager**こと、仮想デバイスを作成するデバイスのプルダウン メニューで含めるは。

[![デバイスのプルダウン メニューに追加された新しい AVD](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  開く、**ターミナル**ウィンドウと、mac、Android SDK ツール ディレクトリの場所の変更 通常 Xamarin のインストールでは、次のコマンドを使用できます。

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  使用して新しい仮想デバイスを作成、 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)コマンド。 たとえば、という名前、AVD を作成する**AVD Oreo 8.0** API レベル 26 のシステム イメージ、x86 を使用して、次のコマンドを使用します。

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  メッセージが表示されたら**[いいえ] のカスタムのハードウェア プロファイルを作成する**を入力できます**ありません**し、既定のハードウェア プロファイルをそのまま使用します。 答えた場合**はい**、 **avdmanager**をカスタマイズするための質問の一覧が表示、ハードウェア プロファイル。

使用した後**avdmanager**こと、仮想デバイスを作成するデバイスのプルダウン メニューで含めるは。

[![デバイスのプルダウン メニューに追加された新しい AVD](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

テストとデバッグ用の Android エミュレーターの構成の詳細については、次を参照してください。 [Android SDK エミュレーター](~/android/deploy-test/debugging/android-sdk-emulator/index.md)です。

Nexus やピクセルなどの物理デバイスを使用している場合か、無線 (OTA) 更新プログラムを自動でデバイスを更新またはシステム イメージをダウンロードでき、デバイスを直接フラッシュできます。 Android Oreo にデバイスを手動で更新の詳細については、次を参照してください。 [Nexus およびピクセル デバイスの工場出荷時イメージ](https://developers.google.com/android/images)です。



## <a name="new-features"></a>新機能

Android Oreo には、さまざまな新機能や通知チャネル、通知のバッジ、XML でのカスタムのフォント、ダウンロード可能なフォント、オートコンプリート、およびピクチャ ピクチャなどの機能が導入されています。 次のセクションでは、これらの機能を強調表示し、アプリで使用を開始に役立つリンクを提供します。



### <a name="notification-channels"></a>通知チャネル

*通知チャネル*がアプリ定義のカテゴリを通知します。
通知を送信する必要があるは、各種類の通知チャネルを作成して、アプリのユーザーによって行われた選択肢を反映するように通知チャネルを作成することができます。 新しい通知チャネルの機能によりさまざまな種類の通知より粒度の細かい制御をユーザーにできます。 たとえば、メッセージング アプリケーションを実装している場合は、ユーザーによって作成されるメッセージ交換グループごとに個別の通知チャネルを作成できます。

[通知チャネル](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan)通知チャネルを作成し、ローカルの通知を通知するため使用する方法について説明します。 実際のコード例は、次を参照してください。、 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels)サンプルです。 このサンプル アプリは、2 つのチャネルを管理し、その他の通知オプションを設定します。



### <a name="notification-badges"></a>通知のバッジ

通知のバッジは、このスクリーン ショットに示すようにアプリのアイコンの上に表示される小さな点です。

[![アプリのアイコン上の例通知のバッジ](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

これらのドットは、そのアプリのアイコンに関連付けられているアプリで通知チャネルを 1 つまたは複数の新しい通知があることを示します&ndash;これらは、ユーザーが破棄されていないか、処理されている通知します。 ユーザーできる時間の長い - するキーを押しますアイコンを消してまたはその appeaars に時間の長いキーを押してメニューからの通知で動作して、通知のバッジと関連する通知の概要です。

通知のバッジの詳細については、Android Developer を参照してください。[通知のバッジ](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges)トピックです。



### <a name="custom-fonts-in-xml"></a>XML でのカスタム フォント

Android Oreo 紹介*XML 内のフォント*、リソースとしてカスタム フォントを組み込むことのできるようになります。 OpenType (**.otf**) および TrueType (**.ttf**) フォントの形式がサポートされます。 フォントをリソースとして追加するには、次の操作を行います。

1. 作成、**リソース/フォント**フォルダーです。

2. フォント ファイルをコピー (例では、 **.ttf**と**.otf**ファイル) に**リソース/フォント**です。 

3. 必要に応じて、Android のファイル名前付け規則に従っているため、各フォント ファイルの名前 (使用だけ小文字など*a ~ z*、 *0-9*、およびファイル名にアンダー スコア)。 たとえば、フォント ファイル`Pacifico-Regular.ttf`のように名前を変更する可能性があります`pacifico.ttf`です。

4. 新しいを使用してカスタム フォントを適用する`android:fontFamily`レイアウト XML での属性です。 たとえば、次`TextView`宣言を使用して、追加した**pacifico.ttf**フォント リソース。

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

複数のフォントとスタイルと重み付けの詳細を記述するフォント ファミリ XML ファイルを作成することもできます。 詳細については、Android Developer を参照してください。 [XML 内のフォント](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html)トピックです。


### <a name="downloadable-fonts"></a>ダウンロード可能なフォント

Android Oreo から始まり、アプリは、APK にバンドルするのではなく、プロバイダーからフォントを要求できます。 必要な場合のみ、フォントは、ネットワークからダウンロードされます。 この機能では、電話のメモリおよび携帯データの使用状況の節約、APK サイズを縮小します。 Android のサポート ライブラリ 26 パッケージをインストールして 14 以降、Android API バージョンでこの機能を使用することもできます。

作成する、アプリに必要なフォントが、ときに、 `FontsRequest` (をダウンロードするフォントを指定する) オブジェクトに渡すと、`FontsContract`フォントをダウンロードするメソッド。 次の手順では、フォントのダウンロード プロセスの詳細について説明します。

1.  インスタンスを作成、 [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html)オブジェクト。 

2.  サブクラスをインスタンス化と[FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)です。

3.  実装、 [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29)メソッドで、フォントの要求の完了を処理するために使用します。

4.  実装、 [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29)アプリ フォント要求プロセス中に実行されたすべてのエラーを通知するために使用されるメソッド。

5.  呼び出す、 [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback))フォント プロバイダーからフォントを取得します。 

呼び出すと、`RequestFonts`メソッドに最初を確認するフォントがローカルでキャッシュされたかどうか (前の呼び出しから`RequestFont`)。 キャッシュされていない場合、フォント プロバイダーを呼び出して、非同期処理で、フォントを取得およびアプリケーションに呼び出すことによって、結果を渡します、`OnTypeFaceRetrieved`メソッドです。

[ダウンロード可能なフォント](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts)サンプルは Android Oreo で導入された、ダウンロード可能なフォント機能を使用する方法を示します。 

フォントのダウンロードの詳細については、Android Developer を参照してください。[ダウンロード可能なフォント](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html)トピックです。



### <a name="autofill"></a>オートコンプリートを使用

新しい_Autofill_ Android Oreo のフレームワークでは、ログイン、アカウントの作成、およびクレジット カード トランザクションなどの反復的なタスクを処理するユーザーが簡単にします。 ユーザーには、(入力エラーを招くことができます) がある情報を再入力する時間が短縮します。 アプリは、Autofill フレームワークを使用できます、前に、システムの設定 (ユーザーを有効または無効にオートコンプリートを使用) で autofill サービスを有効にする必要があります。

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) Autofill Framework の使用を示します。 Autofilled、およびクライアントのアクティビティにオートフィル用のデータを提供するサービスにする必要のあるビューを使用するクライアント アクティビティの実装が含まれています。

新しい Autofill 機能および autofill 用のアプリを最適化する方法の詳細については、Android Developer を参照してください。 [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html)トピックです。



### <a name="picture-in-picture-pip"></a>(PIP) の図の画像

Android Oreo できるようになりますピクチャ ピクチャ (PIP) モードで起動するためのアクティビティの別のアクティビティの画面をオーバーレイします。 この機能はビデオの再生を目的としています。

アプリのアクティビティが PIP モードを使用できることを指定するには、Android マニフェストで true に、次のフラグを設定します。

```xml
android:supportsPictureInPicture
```

PIP のモードにあるときに、アクティビティの動作方法を指定するには、使用する新しい[PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html)オブジェクト。 `PictureInPictureParams` 初期化し、PIP モード (たとえば、アクティビティの優先縦横比) 内のアクティビティの更新に使用するパラメーターのセットを表します。 追加された次の新しい PIP メソッド`Activity`Android Oreo で。

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; PIP モード、アクティビティに設定します。 アクティビティは、画面の隅に配置され、画面の残りの部分は、画面には、前の活動で塗りつぶされます。

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash;アクティビティの PIP 構成設定 (たとえば、縦横比の変更) を更新します。

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo で導入されたハンドヘルド デバイスのピクチャ画像 (PiP) モードの基本的な使用法を示します。 サンプルでは、表示モードまたはその他の活動の間で前後の切り替え中に中断なく継続的ビデオを再生します。



### <a name="other-features"></a>その他の機能

Android Oreo には、他の多くの Emoji サポート ライブラリ、場所の API、バック グラウンド制限などの新機能、アプリ、新しいオーディオ コーデック、WebView の機能強化、改善のキーボード ナビゲーションのサポート、および用の新しい AAudio (pro オーディオ) API の wide 域色が含まれています。これらの機能の詳細については、高性能な待機時間の短いオーディオは、Android Developer を参照してください。 [Android Oreo 機能と Api](https://developer.android.com/about/versions/oreo/android-8.0.html)トピックです。



## <a name="behavior-changes"></a>動作の変更

Android Oreo には、さまざまなシステムと既存のアプリの機能に影響する可能性が API の動作の変更が含まれています。 これらの変更は次のとおりです。


### <a name="background-execution-limits"></a>バック グラウンドの実行の制限

アプリが実行できる操作の制限が生じます。 Android Oreo ユーザー エクスペリエンスを向上させるにはバック グラウンドで実行中にします。 など、ユーザーがビデオを視聴したり、ゲームをプレイ、バック グラウンドで実行されているアプリにはフォア グラウンドで実行されているビデオ大量に消費するアプリのパフォーマンスが損なわれる可能性があります。 その結果、Android Oreo は、ユーザーと直接やり取りしていないアプリで、次の制限を配置します。

1.  **サービスの制限事項をバック グラウンド**&ndash;を数分のまだ許可されて作成およびサービスを使用するウィンドウがあるアプリがバック グラウンドで実行されている場合。 そのウィンドウの最後に、Android アプリのバック グラウンド サービスを停止およびとして扱います_アイドル_です。

2.  **制限事項をブロードキャスト** &ndash; Android 7.0 (API 25) が受信するアプリを登録するブロードキャストに制限を配置します。 Android Oreo では、これらの制限を厳格になります。 たとえば、Android Oreo アプリはマニフェストで暗黙ブロードキャスト用放送受信機を登録することができますできません。

新しいバック グラウンドの実行の制限の詳細については、Android Developer を参照してください。[背景実行制限](https://developer.android.com/about/versions/oreo/background.html)トピックです。


### <a name="breaking-changes"></a>互換性に影響する変更点

またはアプリを対象に Android Oreo 以降、該当する場合は、次の変更をサポートするために、アプリを変更する必要があります。

- Android Oreo には、個々 の通知の優先順位を設定する機能が推奨されなくなりました。 代わりに、通知チャネルを作成するときに、推奨される重要度レベルを設定します。 通知にすべてのメッセージを投稿することを通知チャネルに割り当てる重要度レベルが適用されます。

- Android の Oreo を対象とするアプリの`PendingIntent.GetService()`バック グラウンドで開始されたサービスの新しい制限のため機能しません。 Android Oreo を対象とする場合を[PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/)代わりにします。  


## <a name="sample-code"></a>サンプル コード

Xamarin.Android サンプルがいくつかは Android Oreo 機能を活用する方法を利用できます。

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Android Oreo で導入された新しい通知チャネル システムを使用する方法を示します。 このサンプルは、2 つの通知チャネルを管理します。 既定の重要度と重要度が高いと、その他のいずれか。

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo で導入されたハンドヘルド デバイスのピクチャ画像 (PiP) モードの基本的な使用法を示しています。 サンプルでは、表示モードまたはその他の活動の間で前後の切り替え中に中断なく継続的ビデオを再生します。

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) Autofill Framework の使用例を示します。 Autofilled、およびクライアントのアクティビティにオートフィル用のデータを提供するサービスにする必要のあるビューを使用するクライアント アクティビティの実装が含まれています。

-   [ダウンロード可能なフォント](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts)前に説明したフォントのダウンロード可能な機能を使用する方法の例を示します。

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) EmojiCompat サポート ライブラリの使用方法を示します。 このライブラリは、不足している emoji 文字「階層」の文字として表示からアプリを防ぐために使用できます。

-   [場所更新保留中のインテント](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent)デバイスの場所に関する更新プログラムを取得する場所 API の使用方法を使用して、`PendingIntent`です。

-   [場所の更新プログラムの前景色サービス](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService)場所 API を使用して、バインドと起動の前景色サービスを使用して、デバイスの場所に関する更新プログラムを取得する方法を示しています。


## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C# を使用して android の 8.0 Oreo 開発**


## <a name="summary"></a>まとめ

ここでは、Android Oreo を導入し、インストールして、Android Oreo で最新のツールおよび Xamarin.Android 開発用のパッケージを構成する方法を説明します。 いくつかの新機能の例のソース コードへのリンクを Android Oreo で使用できる主な機能の概要を提供します。 API のドキュメントへのリンクが含まれているし、Android 開発者向けのトピックを参考 Oreo Android 用アプリの作成に開始します。 既存のアプリを及ぼす可能性のある最も重要な Android Oreo 動作の変更も強調表示されます。


## <a name="related-links"></a>関連リンク

- [Android 8.0 Oreo](https://developer.android.com/index.html)
