---
title: Oreo 機能
description: 最新バージョンの Android のアプリを開発する Xamarin.Android の使用を開始する方法。
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: 0c5e048dd3f3496691b83eb10d377d012efedc72
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528768"
---
# <a name="oreo-features"></a>Oreo 機能

_最新バージョンの Android のアプリを開発する Xamarin.Android の使用を開始する方法。_

[Android 8.0 Oreo](https://developer.android.com/index.html)は最新バージョンの Android の Google から使用できます。 Android Oreo では、Xamarin.Android 開発者にとって関心のある多くの新機能を提供します。 これらの機能には、XML、ダウンロード可能なフォント、オートコンプリート、および画像 (PIP) の画像で通知チャネル、通知のバッジ、カスタム フォントが含まれます。 Android Oreo には、これらの新機能の新しい Api が含まれており、これらの Api は、Xamarin.Android 8.0 を使用すると、Xamarin.Android アプリで使用できる以降。

[![Android Oreo ヒーローのイメージ](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

この記事では、Android 8.0 Oreo 用 Xamarin.Android アプリの開発を開始するために構成されます。 これには、必要な更新プログラムをインストール、SDK を構成およびテスト用のエミュレーター (またはデバイス) を作成する方法について説明します。 Android 8.0 Oreo の新機能の概要は、Xamarin.Android アプリで Android Oreo 機能を使用する方法を説明するサンプル アプリへのリンクも提供します。


## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで Android Oreo 機能を使用する、次が必要。

-   **Visual Studio** &ndash; Windows を使用している場合、バージョンの Visual Studio 15.5 以降が必要です。  Mac を使用している場合は、Visual Studio for Mac バージョン 7.2.0 が必要です。

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 以降をインストールして Visual Studio で構成する必要があります。

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) 以降、Android SDK Manager を使用してをインストールする必要があります。



## <a name="getting-started"></a>作業の開始

Xamarin.Android で Android Oreo の使用を開始するには、ダウンロードし、Android Oreo プロジェクトを作成する前に、最新のツールと SDK パッケージをインストールする必要があります。

1. Visual Studio の最新バージョンに更新します。

2. インストール、 **Android 8.0.0 (API 26)** またはそれ以降のパッケージとツール、SDK Manager を使用しています。

3. 新しい Xamarin.Android プロジェクトを対象とする Android Oreo (API 26) を作成します。

4. エミュレーターまたは Android Oreo のアプリのテスト用のデバイスを構成します。

次のセクションでは、これらの各手順について説明します。



### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio を更新および Xamarin.Android

Visual Studio には、Android Oreo のサポートを追加するには、次の操作を行います。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-   : Visual Studio 2017 を使用する場合 

    1. Visual Studio 2017 バージョン 15.7 以降に更新プログラム (を参照してください[Visual Studio 2017 更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio))。

    2. 使用して、 [SDK Manager](~/android/get-started/installation/android-sdk.md) API レベル 26.0 以降をインストールします。

-   Visual Studio 2015 を使用している場合は、25 古い Google エミュレーター マネージャー GUI を使用して SDK Tools の昇格を勧めします。 25 の SDK ツールは、API 26、27 日以降と共に使用でき、新しいプラットフォームの開発に影響しません。 こうインターフェイスとの古いバージョンの Android SDK を管理するためです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

-   For Mac で説明したように Visual Studio 2017 の最新の安定したバージョンに更新[Visual Studio for Mac の更新](https://docs.microsoft.com/visualstudio/mac/update)します。

-----

Android Oreo 用の Xamarin サポートの詳細については、次を参照してください。、 [Xamarin.Android 8.0 リリース ノート](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/)します。



### <a name="install-the-android-sdk"></a>Android SDK をインストールします。

Xamarin.Android 8.0 でプロジェクトを作成する必要があります最初に使用する、Xamarin Android SDK Manager の SDK プラットフォームをインストールする**Android 8.0 の Oreo**またはそれ以降。 また、Android SDK Tools 26.0 以降をインストールする必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. SDK マネージャーを起動 (Visual Studio で、次のようにクリックします。**ツール > Android > Android SDK Manager**)。

2. インストール、 **Android 8.0 の Oreo**パッケージ。 Android SDK エミュレーターを使用している場合に含めるようにしてください、 **x86**システム イメージを必要があります。

    [![Android SDK Manager で Android 8.0 パッケージを選択します。](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. インストール**Android SDK Tools 26.0.2**またはそれ以降、 **Android SDK Platform-tools 26.0.0**またはそれ以降、および**Android SDK ビルド ツール 26.0.0** (またはそれ以降)。

    [![Android SDK Manager で Android SDK Tools 26 を選択します。](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. SDK マネージャーを起動 (Visual studio for Mac では、次のようにクリックします。**ツール > SDK Manager**)。

2. インストール、 **Android 8.0 の Oreo** SDK パッケージ。 Android SDK エミュレーターを使用している場合に含めるようにしてください、 **x86**システム イメージを必要があります。

    [![SDK Manager で Android 8.0 パッケージを選択します。](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. インストール**Android SDK Tools 26.0.2**またはそれ以降、 **Android SDK Platform-tools 26.0.0**またはそれ以降、および**Android SDK ビルド ツール 26.0.0** (またはそれ以降)。

    [![SDK Manager で Android SDK Tools 26 を選択します。](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始します。

新しい Xamarin.Android プロジェクトを作成します。 Xamarin で Android の開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md)を Xamarin.Android プロジェクトを作成する方法について説明します。

Android プロジェクトを作成するときに、Android 8.0 以降をターゲットにバージョン設定を構成する必要があります。 たとえば、Android 8.0 のプロジェクトを対象にする必要がありますを構成するプロジェクトのターゲットの Android API レベル**Android 8.0 (API 26)** します。 API 26 またはそれ以降は、ターゲット フレームワークのレベルを設定することをお勧めします。 詳細レベルの Android API レベルの構成については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。


### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成します。

Android SDK Tools 26.0 をインストールした後、既定の Google GUI ベースの AVD マネージャーを起動しようとした場合、後で、コマンド ライン AVD マネージャー ツールを使用するように指示する、次のエラー ダイアログを get が**avdmanager**代わりに:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android エミュレーター マネージャーの警告ダイアログ](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android エミュレーター マネージャーの警告ダイアログ](oreo-images/mac/03-avd-warning.png)

-----

Google がスタンドアロン 26.0 以降の API をサポートしている GUI AVD マネージャーが用意されていませんので、このメッセージが表示されます。 Android 8.0 Oreo、用の Xamarin Android エミュレーター マネージャーまたはコマンドラインを使用する必要があります`avdmanager`Android Oreo 用の仮想デバイスを作成するためのツール。

Android デバイス マネージャーを作成し、仮想デバイスの管理を使用する、次を参照してください。[仮想デバイスの管理が Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)します。
Android Device Manager なしの仮想デバイスを作成するには、次のセクションの手順に従います。


#### <a name="creating-virtual-devices-using-avdmanager"></a>仮想デバイスを使用して avdmanager を作成します。

使用する**avdmanager**を新しい仮想デバイスを作成するには、次の手順に従います。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  コマンド プロンプト ウィンドウを開き、設定`JAVA_HOME`コンピューター上の Java SDK の場所にします。 一般的な Xamarin のインストールでは、次のコマンドを使用できます。

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Android SDK の場所を追加`bin`フォルダーを`PATH`します。
    一般的な Xamarin のインストールでは、次のコマンドを使用できます。

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  コマンド プロンプト ウィンドウを閉じ、新しいコマンド プロンプト ウィンドウを開きます。 使用して新しい仮想デバイスを作成、 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)コマンド。 たとえば、AVD を作成するという名前**8.0 Oreo の AVD の**API レベル、26 のシステム イメージ、x86 を使用して、次のコマンドを使用します。

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  メッセージが表示されたら **[いいえ] のカスタムのハードウェア プロファイルを作成する**を入力できます**ありません**と既定のハードウェア プロファイルをそのまま使用します。 答えると**はい**、 **avdmanager**ハードウェア プロファイルをカスタマイズするための質問の一覧が表示されます。

したら**avdmanager**こと、仮想デバイスを作成するデバイスのプルダウン メニューに含めるは。

[![デバイスのプルダウン メニューに追加された新しい AVD](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  開く、**ターミナル**ウィンドウと mac の Android SDK tools ディレクトリの場所の変更 一般的な Xamarin のインストールでは、次のコマンドを使用できます。

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  使用して新しい仮想デバイスを作成、 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)コマンド。 たとえば、AVD を作成するという名前**8.0 Oreo の AVD の**API レベル、26 のシステム イメージ、x86 を使用して、次のコマンドを使用します。

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  メッセージが表示されたら **[いいえ] のカスタムのハードウェア プロファイルを作成する**を入力できます**ありません**と既定のハードウェア プロファイルをそのまま使用します。 答えると**はい**、 **avdmanager**ハードウェア プロファイルをカスタマイズするための質問の一覧が表示されます。

使用した後**avdmanager**こと、仮想デバイスを作成するデバイスのプルダウン メニューに含めるは。

[![デバイスのプルダウン メニューに追加された新しい AVD](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

テストとデバッグ用の Android エミュレーターを構成する方法の詳細については、次を参照してください。 [Android エミュレーターでデバッグする](~/android/deploy-test/debugging/debug-on-emulator.md)します。

Nexus やピクセルなどの物理デバイスを使用している場合か、無線 (OTA) 更新プログラムを自動でデバイスを更新またはシステム イメージをダウンロードしてフラッシュ、デバイスを直接。 Android Oreo にデバイスを手動で更新の詳細については、次を参照してください。 [Nexus とピクセル デバイスの工場出荷時イメージ](https://developers.google.com/android/images)します。



## <a name="new-features"></a>新機能

Android Oreo では、さまざまな新機能と、通知チャネル、通知のバッジ、XML でのカスタム フォント、フォントのダウンロード可能なオートコンプリート、およびピクチャ イン ピクチャなどの機能について説明します。 次のセクションでは、これらの機能を強調表示し、アプリでの使用に役立つリンクを開始するを指定します。



### <a name="notification-channels"></a>通知チャネル

*通知チャネル*は通知をアプリで定義されたカテゴリ。
通知を送信する必要があるは、各種類の通知チャネルを作成して、アプリのユーザーによる選択を反映するように、通知チャネルを作成することができます。 新しい通知チャネル機能では、さまざまな種類の通知をきめ細かく制御をユーザーに付与することが可能です。 たとえば、メッセージング アプリを実装する場合は、ユーザーによって作成される各メッセージ交換グループの別の通知チャネルを作成できます。

[通知チャネル](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan)通知チャネルを作成し、ローカル通知をポストするために使用する方法について説明します。 実際のコード例では、次を参照してください。、 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels)サンプルです。 このサンプル アプリは、2 つのチャネルを管理し、追加の通知オプションを設定します。



### <a name="notification-badges"></a>通知のバッジ

通知のバッジは、このスクリーン ショットで示すように、アプリのアイコンの上に表示される小さなドットです。

[![アプリ アイコンに通知バッジの例](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

これらのドットは、そのアプリ アイコンに関連付けられているアプリで通知チャネルを 1 つまたは複数の新しい通知があることを示します&ndash;これらは、通知をユーザーがまだ破棄も処理します。 ユーザーできます時間の長い-キーを押しますアイコンに関連付けられた通知のバッジ、通知を消去またはその appeaars 時間の長い-キーを押してメニューからの通知の動作をします。

通知のバッジの詳細については、Android の開発者を参照してください。[通知のバッジ](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges)トピック。



### <a name="custom-fonts-in-xml"></a>XML でのカスタム フォント

Android Oreo の紹介*XML 内のフォント*、用にカスタム フォントをリソースとして組み込むことができるようにします。 OpenType (**.otf**) と TrueType (**.ttf**) フォント形式がサポートされています。 リソースとしてフォントを追加するには、次の操作を行います。

1. 作成、**リソース/フォント**フォルダー。

2. フォント ファイルをコピー (例では、 **.ttf**と **.otf**ファイル) に**リソース/フォント**します。 

3. 必要に応じて、Android のファイル名前付け規則に準拠できるように各フォント ファイルの名前 (使用のみ小文字つまり*a ~ z*、 *0 ~ 9*、およびファイル名にアンダー スコア)。 たとえば、フォント ファイル`Pacifico-Regular.ttf`のように名前を変更することが`pacifico.ttf`します。

4. 新しいを使用してカスタム フォントを適用`android:fontFamily`レイアウトでは、XML 属性です。 たとえば、次`TextView`宣言を使用して、追加した**pacifico.ttf**フォント リソース。

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

複数のフォントとスタイルと重みの詳細を説明するフォント ファミリ XML ファイルを作成することもできます。 詳細については、Android の開発者を参照してください。 [XML 内のフォント](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html)トピック。


### <a name="downloadable-fonts"></a>ダウンロード可能なフォント

Android Oreo 以降では、アプリは、APK にバンドルしするのではなく、プロバイダーからのフォントを要求できます。 フォントは、必要な場合のみ、ネットワークからダウンロードされます。 この機能では、電話のメモリと携帯電話データの使用法の節約、APK のサイズを縮小します。 Android サポート ライブラリ 26 パッケージをインストールすることで、Android API バージョン 14 以降でこの機能を使用することもできます。

作成するアプリで、フォントが必要なときに、 `FontsRequest` (ダウンロードするフォントを指定する) オブジェクトに渡すと、`FontsContract`フォントをダウンロードするメソッド。 次の手順では、フォントのダウンロード プロセスの詳細について説明します。

1.  インスタンスを作成、 [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html)オブジェクト。 

2.  サブクラスのインスタンスを作成および[FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)します。

3.  実装、 [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29)メソッドで、フォントの要求の完了を処理するために使用します。

4.  実装、 [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29)メソッドで、フォントの要求の処理中に行われるすべてのエラーのアプリに通知するために使用します。

5.  呼び出す、 [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback))フォント プロバイダーからフォントを取得します。 

呼び出すと、`RequestFonts`メソッドでは、まずを確認、フォントがローカルでキャッシュされたかどうか (以前の呼び出しから`RequestFont`)。 キャッシュされていない場合、フォントのプロバイダーを呼び出して、フォントを非同期的に取得およびアプリケーションに呼び出すことによって、結果を渡します、`OnTypeFaceRetrieved`メソッド。

[ダウンロード可能なフォント](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts)サンプルは Android Oreo で導入されたフォントのダウンロード可能な機能を使用する方法を示します。 

フォントをダウンロードする方法の詳細については、Android の開発者を参照してください。[ダウンロード可能なフォント](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html)トピック。



### <a name="autofill"></a>オートコンプリート

新しい_オートフィル_Android Oreo のフレームワークでは、ログイン、アカウントの作成、およびクレジット カード トランザクションなどの反復的なタスクを処理するためにユーザーが簡単にします。 ユーザーは、(入力エラーにつながる) 情報を再入力時間を短縮します。 アプリは、Autofill Framework を使用できます、前に、システム設定 (ユーザーが有効またはオートコンプリートを無効にする)、autofill サービスを有効にする必要があります。

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) Autofill Framework の使用を示します。 Autofilled、およびクライアントの活動に自動入力データを提供できるサービス必要のあるビューを使用するクライアント アクティビティの実装が含まれています。

新しい Autofill 機能とオートコンプリートのアプリを最適化する方法の詳細については、Android の開発者を参照してください。 [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html)トピック。



### <a name="picture-in-picture-pip"></a>Picture in Picture (PIP)

Android Oreo によりピクチャ イン ピクチャ (PIP) モードで起動するアクティビティを別のアクティビティの画面をオーバーレイします。 この機能は、ビデオ再生の対象としています。

アプリのアクティビティが PIP モードを使用できることを指定するには、Android マニフェストで true に、次のフラグを設定します。

```xml
android:supportsPictureInPicture
```

PIP モードにあるときに、アクティビティがどのように動作する必要がありますかを指定するを使用する新しい[PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html)オブジェクト。 `PictureInPictureParams` 初期化し、PIP モード (たとえば、アクティビティの推奨される縦横比) 内のアクティビティを更新するために使用するパラメーターのセットを表します。 追加された次の新しい PIP メソッド`Activity`Android Oreo で。

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; PIP モードでアクティビティを配置します。 アクティビティは、画面の隅にあるに配置され、画面の残りの部分は、画面には、前の活動で塗りつぶされます。

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash;アクティビティの PIP 構成設定 (たとえば、縦横比の変更) を更新します。

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo で導入されたハンドヘルド デバイス用の画像画像 (PiP) モードの基本的な使用方法を示します。 サンプルでは、表示モードまたは他のアクティビティ間の切り替え中に中断なく引き続きビデオを再生します。



### <a name="other-features"></a>その他の機能

Android Oreo には、その他の多くの絵文字サポート ライブラリ、Location API では、バック グラウンドの制限などの新機能、アプリ、新しいオーディオ コーデック、WebView の機能強化、改善のキーボード ナビゲーションのサポート、および用の新しい AAudio (pro オーディオ) API の wide 域色が含まれていますこれらの機能の詳細については、高パフォーマンスの低待機時間オーディオは、Android 開発者を参照してください[Android Oreo 機能と Api](https://developer.android.com/about/versions/oreo/android-8.0.html)トピック。



## <a name="behavior-changes"></a>動作の変更

Android Oreo には、さまざまなシステムと既存のアプリの機能に影響を与える可能性の API 動作の変更が含まれています。 これらの変更は次のとおりです。


### <a name="background-execution-limits"></a>バック グラウンド実行の制限

アプリが実行できる操作の制限が生じます。 Android Oreo ユーザー エクスペリエンスを向上させるにはバック グラウンドで実行中にします。 たとえば、ユーザーがビデオを視聴したり、ゲームをプレイ、バック グラウンドで実行中のアプリは、フォア グラウンドで実行されているビデオを要するアプリケーションのパフォーマンスを低下しました。 その結果、Android Oreo では、ユーザーと直接対話しないアプリで、次の制限を配置します。

1.  **サービスの制限事項をバック グラウンド**&ndash;を数分のことを引き続き許可を作成し、サービスを使用して、ウィンドウがあるアプリがバック グラウンドで実行しているときにします。 そのウィンドウの最後に、Android アプリのバック グラウンド サービスを停止およびものとして扱われます_アイドル_します。

2.  **制限事項をブロードキャスト** &ndash; Android 7.0 (API 25) がブロードキャストを受信するアプリを登録するに制限を配置します。 Android Oreo では、これらの制限が厳しいなります。 たとえば、Android Oreo アプリでは、マニフェストでの暗黙的なブロードキャスト、ブロードキャスト レシーバーは登録できます不要になった。

新しいバック グラウンド実行の制限の詳細については、Android の開発者を参照してください。[バック グラウンド実行制限](https://developer.android.com/about/versions/oreo/background.html)トピック。


### <a name="breaking-changes"></a>互換性に影響する変更点

Android Oreo を対象または以降、該当する場合は、次の変更をサポートするために、アプリを変更する必要がありますアプリ:

- Android Oreo で、個々 の通知の優先順位を設定する機能。 代わりに、通知チャネルを作成するときに、推奨される重要度レベルを設定します。 通知チャネルに割り当てる重要度レベルは、すべての通知へのメッセージの送信先となるに適用されます。

- Android Oreo を対象とするアプリの`PendingIntent.GetService()`バック グラウンドで開始するサービスの新しい制限のため機能しません。 Android Oreo を対象とする場合は使用[PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/)代わりにします。  


## <a name="sample-code"></a>サンプル コード

Android Oreo 機能を活用する方法について説明するいくつかの Xamarin.Android サンプルがあります。

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Android Oreo で導入された新しい通知チャネル システムを使用する方法を示します。 このサンプルは、2 つの通知チャネルを管理します。 既定の重要性および高重要度で、その他の 1 つ。

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo で導入されたハンドヘルド デバイス用の画像画像 (PiP) モードの基本的な使用方法について説明します。 サンプルでは、表示モードまたは他のアクティビティ間の切り替え中に中断なく引き続きビデオを再生します。

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) Autofill Framework の使用方法を示します。 Autofilled、およびクライアントの活動に自動入力データを提供できるサービス必要のあるビューを使用するクライアント アクティビティの実装が含まれています。

-   [ダウンロード可能なフォント](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts)前に説明したダウンロード可能なフォントの機能を使用する方法の例を示します。

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) EmojiCompat サポート ライブラリの使用方法について説明します。 このライブラリを使用すると、アプリから不足している絵文字文字「階層」の文字として表示されないようにします。

-   [場所の更新プログラムの保留中のインテント](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent)デバイスの場所に関する更新プログラムを取得する場所の API の使用法を示しますを使用して、`PendingIntent`します。

-   [場所の更新プログラムのフォア グラウンド サービス](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService)Location API を使用して、バインドと開始のフォア グラウンド サービスを使用して、デバイスの場所に関する更新プログラムを取得する方法を示します。


## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C# での android 8.0 Oreo 開発**


## <a name="summary"></a>まとめ

この記事では、Android Oreo を導入し、インストールし、Android Oreo の最新のツールと Xamarin.Android の開発用のパッケージを構成する方法について説明します。 いくつかの新機能の例のソース コードへのリンクを Android Oreo で使用できる主な機能の概要を提供します。 API のドキュメントへのリンクが含まれているし、Android Oreo 用アプリの作成に役立つ Android 開発者向けのトピックを開始します。 既存のアプリに影響する最も重要な Android Oreo の動作の変更も強調表示されます。


## <a name="related-links"></a>関連リンク

- [Android 8.0 Oreo](https://developer.android.com/index.html)
