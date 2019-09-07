---
title: Oreo の機能
description: Android の最新バージョン用のアプリを開発するために、Xamarin の使用を開始する方法。
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: 0387cd91bd24080417a5e9763410d68b6e688555
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757491"
---
# <a name="oreo-features"></a>Oreo の機能

_Android の最新バージョン用のアプリを開発するために、Xamarin の使用を開始する方法。_

[Android 8.0 Oreo](https://developer.android.com/index.html)は、Google から入手できる最新バージョンの android です。 Android Oreo は、Xamarin Android 開発者にとって関心のある多くの新機能を提供しています。 これらの機能には、通知チャネル、通知バッジ、XML でのカスタムフォント、ダウンロード可能なフォント、オートフィル、画像内画像 (PIP) などがあります。 Android Oreo には、これらの新機能用の新しい Api が含まれています。 Xamarin android 8.0 以降を使用する場合、これらの Api は Xamarin Android アプリで使用できます。

[![Android Oreo ヒーローの画像](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

この記事は、Android 8.0 Oreo 向けの Xamarin Android アプリの開発を始めるのに役立つように構成されています。 ここでは、必要な更新プログラムをインストールし、SDK を構成し、テスト用のエミュレーター (またはデバイス) を作成する方法について説明します。 また、Android 8.0 Oreo の新機能の概要と、Xamarin Android アプリで Android Oreo 機能を使用する方法を示すサンプルアプリへのリンクも示します。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで Android Oreo 機能を使用するには、次のものが必要です。

- **Visual Studio**&ndash; Windows を使用している場合は、Visual Studio のバージョン15.5 以降が必要です。  Mac を使用している場合は、Visual Studio for Mac バージョン7.2.0 が必要です。

- **Xamarin android** &ndash; 8.0 以降がインストールされ、Visual Studio で構成されている必要があります。

- **Android SDK**&ndash; Android SDK Manager を使用して Android SDK 8.0 (API 26) 以降をインストールする必要があります。

## <a name="getting-started"></a>作業の開始

Android Oreo と Xamarin android の使用を開始するには、Android Oreo プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. 最新バージョンの Visual Studio に更新します。

2. SDK Manager を使用して、 **Android 8.0.0 (API 26)** 以降のパッケージとツールをインストールします。

3. Android Oreo (API 26) を対象とする新しい Xamarin Android プロジェクトを作成します。

4. Android Oreo アプリをテストするためのエミュレーターまたはデバイスを構成します。

これらの各手順については、次のセクションで説明します。

### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio と Xamarin Android を更新する

Android Oreo support を Visual Studio に追加するには、次の手順を実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Visual Studio 2019 の場合は、 [SDK Manager](~/android/get-started/installation/android-sdk.md)を使用して API レベル26.0 以降をインストールします。

- Visual Studio 2017 を使用している場合:

    1. Visual Studio 2017 バージョン15.7 以降に更新します (「 [Visual studio 2017 を更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio)する」を参照してください)。

    2. [SDK Manager](~/android/get-started/installation/android-sdk.md)を使用して、API レベル26.0 以降をインストールします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

- 「 [Visual Studio for Mac の更新](https://docs.microsoft.com/visualstudio/mac/update)」で説明されているように、最新の安定したバージョンの Visual Studio for Mac に更新します。

-----

Xamarin support for Android Oreo の詳細については、 [xamarin 8.0 のリリースノート](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/)を参照してください。

### <a name="install-the-android-sdk"></a>Android SDK のインストール

Xamarin Android 8.0 を使用してプロジェクトを作成するには、最初に Xamarin Android SDK Manager を使用して、 **android 8.0-Oreo**以降の SDK プラットフォームをインストールする必要があります。 Android SDK Tools 26.0 以降もインストールする必要があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. SDK Manager を起動します (Visual Studio で、[ **Tools > Android > Android SDK Manager**)] をクリックします。

2. **Android 8.0-Oreo**パッケージをインストールします。 Android SDK emulator を使用している場合は、必要な**x86**システムイメージを必ず含めてください。

    [![Android SDK Manager での Android 8.0 パッケージの選択](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2**以降、 **Android SDK Platform-tools 26.0.0**以降、 **Android SDK ビルドツール 26.0.0** (またはそれ以降) をインストールします。

    [![Android SDK マネージャーで Android SDK Tools 26 を選択する](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. SDK Manager を起動します (Visual Studio for Mac で、 **[ツール > Sdk manager]** をクリックします)。

2. **Android 8.0-Oreo** SDK パッケージをインストールします。 Android SDK emulator を使用している場合は、必要な**x86**システムイメージを必ず含めてください。

    [![SDK Manager での Android 8.0 パッケージの選択](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2**以降、 **Android SDK Platform-tools 26.0.0**以降、 **Android SDK ビルドツール 26.0.0** (またはそれ以降) をインストールします。

    [![SDK Manager での Android SDK Tools 26 の選択](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----

### <a name="start-a-xamarinandroid-project"></a>Xamarin. Android プロジェクトを開始する

新しい Xamarin. Android プロジェクトを作成します。 Xamarin を使用した Android 開発を初めて使用する場合は、「 [Hello, android](~/android/get-started/hello-android/index.md) 」を参照して、xamarin android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、バージョン設定を Android 8.0 以降を対象とするように構成する必要があります。 たとえば、Android 8.0 のプロジェクトを対象にするには、プロジェクトのターゲットの Android API レベルを**android 8.0 (API 26)** に構成する必要があります。 また、ターゲットフレームワークレベルを API 26 以降に設定することをお勧めします。 Android API レベルレベルの構成の詳細については、「 [ANDROID Api レベルについ](~/android/app-fundamentals/android-api-levels.md)て」を参照してください。

### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成する

Android SDK Tools 26.0 以降をインストールした後で既定の Google GUI ベースの AVD Manager を起動しようとすると、次のエラーダイアログが表示されることがあります。これにより、代わりにコマンドライン AVD manager tool **avdmanager**を使用するように指示されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android Emulator Manager の警告ダイアログ](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Android Emulator Manager の警告ダイアログ](oreo-images/mac/03-avd-warning.png)

-----

このメッセージは、Google が API 26.0 以降をサポートするスタンドアロンの GUI AVD マネージャーを提供しなくなったために表示されます。 Android 8.0 Oreo の場合、android Oreo 用の仮想デバイスを作成するには、 `avdmanager` Xamarin Android Emulator Manager またはコマンドラインツールのいずれかを使用する必要があります。

Android Device Manager を使用して仮想デバイスを作成および管理するには、「 [Android Device Manager を使用した仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照してください。
Android Device Manager を使用せずに仮想デバイスを作成するには、次のセクションの手順に従います。

#### <a name="creating-virtual-devices-using-avdmanager"></a>Avdmanager を使用した仮想デバイスの作成

**Avdmanager**を使用して新しい仮想デバイスを作成するには、次の手順を実行します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. コマンドプロンプトウィンドウを開き、コンピューター `JAVA_HOME`上の Java SDK の場所に設定します。 一般的な Xamarin インストールでは、次のコマンドを使用できます。

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2. Android SDK `bin`フォルダーの場所`PATH`をに追加します。
    一般的な Xamarin インストールでは、次のコマンドを使用できます。

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3. コマンドプロンプトウィンドウを閉じて、新しいコマンドプロンプトウィンドウを開きます。 [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)コマンドを使用して、新しい仮想デバイスを作成します。 たとえば、API レベル26の x86 システムイメージを使用して**avd-Oreo-8.0**という名前の avd を作成するには、次のコマンドを使用します。

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4. **[カスタムハードウェアプロファイルを作成**しますか?] というメッセージが表示されたら、「**いいえ**」と入力して既定のハードウェアプロファイルをそのまま使用します。 **「はい」** と入力すると、 **avdmanager**によって、ハードウェアプロファイルをカスタマイズするための質問の一覧が表示されます。

仮想デバイスを作成するために**avdmanager を avし**た後、デバイスのプルダウンメニューには、次のように表示されます。

[![新しい AVD がデバイスのプルダウンメニューに追加されました](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **ターミナル**ウィンドウを開き、Mac 上の Android SDK tools ディレクトリの場所に移動します。 一般的な Xamarin インストールでは、次のコマンドを使用できます。

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2. [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)コマンドを使用して、新しい仮想デバイスを作成します。 たとえば、API レベル26の x86 システムイメージを使用して**avd-Oreo-8.0**という名前の avd を作成するには、次のコマンドを使用します。

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3. **[カスタムハードウェアプロファイルを作成**しますか?] というメッセージが表示されたら、「**いいえ**」と入力して既定のハードウェアプロファイルをそのまま使用します。 **「はい」** と入力すると、 **avdmanager**によって、ハードウェアプロファイルをカスタマイズするための質問の一覧が表示されます。

**Avdmanager**を使用して仮想デバイスを作成すると、そのデバイスはデバイスのプルダウンメニューに表示されます。

[![新しい AVD がデバイスのプルダウンメニューに追加されました](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

テストとデバッグのために Android エミュレーターを構成する方法の詳細については、「 [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)」を参照してください。

このような物理デバイスを使用している場合は、デバイスを無線 (OTA) 更新で自動で更新するか、システムイメージをダウンロードしてデバイスを直接フラッシュすることができます。 デバイスを Android Oreo に手動で更新する方法の詳細については、「デバイス[のデバイスの出荷時のイメージ](https://developers.google.com/android/images)」を参照してください。

## <a name="new-features"></a>新機能

Android Oreo では、通知チャンネル、通知バッジ、XML でのカスタムフォント、ダウンロード可能なフォント、オートフィル、画像の画像など、さまざまな新機能が導入されています。 以下のセクションでは、これらの機能について説明し、アプリでの使用を開始するのに役立つリンクを示します。

### <a name="notification-channels"></a>通知チャネル

*通知チャネル*は、通知用のアプリ定義のカテゴリです。
送信する必要がある通知の種類ごとに通知チャネルを作成し、アプリのユーザーが選択した内容を反映する通知チャネルを作成することができます。 新しい通知チャネル機能を使用すると、ユーザーがさまざまな種類の通知をきめ細かく制御できるようになります。 たとえば、メッセージングアプリを実装する場合は、ユーザーが作成したメッセージ交換グループごとに個別の通知チャネルを作成できます。

[通知](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan)チャネルは、通知チャネルを作成し、それを使用してローカル通知を送信する方法を説明します。 実際のコード例については、 [Notificationchannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels)サンプルを参照してください。このサンプルアプリでは、2つのチャネルを管理し、追加の通知オプションを設定します。

### <a name="notification-badges"></a>通知バッジ

通知バッジは、次のスクリーンショットに示すように、アプリアイコンの上に表示される小さなドットです。

[![アプリアイコンの通知バッジの例](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

これらのドットは、アプリ内の1つ以上の通知チャネルに対して新しい通知があること&ndash;を示しています。アイコンは、ユーザーがまだ終了していないか、操作されていないことを示しています。 ユーザーは、アイコンを使用して、通知バッジに関連付けられている通知をひとめで確認したり、appeaars ている長いプレスメニューからの通知を終了したり、操作したりすることができます。

通知バッジの詳細については、「Android 開発者[通知バッジ](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges)」を参照してください。

### <a name="custom-fonts-in-xml"></a>XML でのカスタムフォント

Android Oreo では、 *XML 形式のフォント*が導入されています。これにより、カスタムフォントをリソースとして組み込むことができるようになります。 OpenType ( **.otf**) および TrueType ( **...** ) フォント形式がサポートされています。 フォントをリソースとして追加するには、次の手順を実行します。

1. **リソース/フォント**フォルダーを作成します。

2. フォントファイル (例、 **.otf**ファイル) を**Resources/font**にコピー**します。** 

3. 必要に応じて、各フォントファイルの名前を Android ファイルの名前付け規則に従って変更します (つまり、ファイル名には小文字*の a-z*、 *0-9*、およびアンダースコアのみを使用します)。 たとえば、フォントファイル`Pacifico-Regular.ttf`の名前をなど`pacifico.ttf`に変更できます。

4. レイアウト XML で新しい`android:fontFamily`属性を使用して、カスタムフォントを適用します。 たとえば、次`TextView`の宣言では、追加された**pacifico**フォントリソースを使用します。

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

また、複数のフォントとスタイルおよび太さの詳細を記述するフォントファミリ XML ファイルを作成することもできます。 詳細については、「 [XML で](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html)の Android Developer フォント」トピックを参照してください。

### <a name="downloadable-fonts"></a>ダウンロード可能なフォント

Android Oreo 以降では、アプリは APK にバンドルするのではなく、プロバイダーのフォントを要求できます。 フォントは、必要な場合にのみネットワークからダウンロードされます。 この機能により、APK のサイズが縮小され、電話のメモリと携帯データの使用量が節約されます。 Android サポートライブラリ26パッケージをインストールすると、Android API バージョン14以上でこの機能を使用することもできます。

アプリにフォントが必要な場合は、( `FontsRequest`ダウンロードするフォントを指定して) オブジェクトを作成し、それ`FontsContract`をメソッドに渡してフォントをダウンロードします。 次の手順では、フォントのダウンロードプロセスの詳細について説明します。

1. [Fontrequest](https://developer.android.com/reference/android/provider/FontRequest.html)オブジェクトをインスタンス化します。 

2. サブクラスを生成し、 [FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html)をインスタンス化します。

3. フォント要求の完了を処理するために使用される[OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29)メソッドを実装します。

4. フォント要求プロセス中に発生するエラーをアプリに通知するために使用される、 [Fontrequestcallback. OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29)メソッドを実装します。

5. フォントプロバイダーからフォントを取得するには、font [contract. RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback))メソッドを呼び出します。 

`RequestFonts`メソッドを呼び出すと、まず、フォントがローカルに`RequestFont`キャッシュされているかどうかを確認します (の前の呼び出しから)。 キャッシュされていない場合は、フォントプロバイダーを呼び出し、そのフォントを非同期に取得してから、 `OnTypeFaceRetrieved`メソッドを呼び出して結果をアプリに渡します。

ダウンロード可能な[フォント](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts)のサンプルは、Android Oreo で導入されたダウンロード可能なフォント機能の使用方法を示しています。 

フォントのダウンロードの詳細については、「Android 開発者向けダウンロード可能な[フォント](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html)」を参照してください。

### <a name="autofill"></a>コンプリート

Android Oreo の新しい_オートフィル_フレームワークにより、ユーザーは、ログイン、アカウントの作成、クレジットカードのトランザクションなど、繰り返し発生するタスクを簡単に処理できるようになります。 ユーザーは、(入力エラーにつながる可能性がある) 情報を再入力する時間が短縮されます。 アプリでオートフィルフレームワークを使用するには、システム設定でオートフィルサービスを有効にする必要があります (ユーザーはオートコンプリートを有効または無効にすることができます)。

[Autofillframework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework)サンプルは、オートフィルフレームワークの使用方法を示しています。 自動入力が必要なビューを含むクライアントアクティビティの実装と、クライアントアクティビティにオートフィルデータを提供できるサービスが含まれています。

オートフィルの新機能と、アプリをオートコンプリート用に最適化する方法の詳細については、「Android 開発者による[オートコンプリートフレームワーク](https://developer.android.com/guide/topics/text/autofill.html)」を参照してください。

### <a name="picture-in-picture-pip"></a>画像の画像 (PIP)

Android Oreo を使用すると、別のアクティビティの画面をオーバーレイする画像 (PIP) モードでアクティビティを起動することができます。 この機能は、ビデオの再生を目的としています。

アプリのアクティビティで PIP モードを使用できるように指定するには、Android マニフェストで次のフラグを true に設定します。

```xml
android:supportsPictureInPicture
```

PIP モードの場合のアクティビティの動作を指定するには、新しい[ピクチャ Inピクチャ params](https://developer.android.com/reference/android/app/PictureInPictureParams.html)オブジェクトを使用します。 `PictureInPictureParams`PIP モードでアクティビティを初期化および更新するために使用するパラメーターのセットを表します (たとえば、アクティビティの優先縦横比など)。 Android Oreo では、次の新しい`Activity` PIP メソッドがに追加されました。

- [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29)&ndash; PIP モードでアクティビティを配置します。 画面の隅にアクティビティが配置され、画面の残りの部分が画面上の前のアクティビティで塗りつぶされます。

- [Setピクチャ Inピクチャ params](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29)&ndash;アクティビティの PIP 構成設定 (縦横比の変更など) を更新します。

Picture [inpicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture)サンプルは、Oreo で導入されたハンドヘルドデバイス用の画像 (PiP) モードの基本的な使用方法を示しています。 このサンプルでは、表示モードまたは他のアクティビティを切り替えながら中断され続けるビデオを再生します。

### <a name="other-features"></a>その他の機能

Android Oreo には、絵文字サポートライブラリ、Location API、バックグラウンドの制限、アプリのための大規模な色の色、新しいオーディオコーデック、WebView の機能強化、キーボードナビゲーションサポートの向上、新しい AAudio (pro オーディオ) API など、他の多くの新機能が含まれています。高パフォーマンスの低待機時間オーディオ。これらの機能の詳細については、Android Developer [Android Oreo の機能と api](https://developer.android.com/about/versions/oreo/android-8.0.html)に関するトピックを参照してください。

## <a name="behavior-changes"></a>動作の変更

Android Oreo には、既存のアプリの機能に影響を与える可能性があるさまざまなシステムおよび API の動作の変更が含まれています。 これらの変更について、次に説明します。

### <a name="background-execution-limits"></a>バックグラウンド実行の制限

Android Oreo では、ユーザーエクスペリエンスを向上させるために、バックグラウンドで実行中のアプリに関する制限を設けています。 たとえば、ユーザーがビデオを見ている場合やゲームをプレイしている場合、バックグラウンドで実行されているアプリは、フォアグラウンドで実行されているビデオを大量に消費するアプリのパフォーマンスに悪影響を与える可能性があります。 その結果、Android Oreo では、ユーザーと直接対話していないアプリに次の制限が課されます。

1. **バックグラウンドサービスの制限事項**&ndash;アプリがバックグラウンドで実行されているときは、数分間のウィンドウがあります。このウィンドウでは、サービスの作成と使用が引き続き許可されます。 このウィンドウの最後で、Android はアプリのバックグラウンドサービスを停止し、_アイドル状態_として扱います。

2. **ブロードキャストの制限事項**&ndash; Android 7.0 (API 25) では、アプリが受信するために登録するブロードキャストに制限が設けられていました。 Android Oreo を使用すると、これらの制限が厳しくなります。 たとえば、Android Oreo アプリでは、暗黙的なブロードキャストのブロードキャストレシーバーをマニフェストに登録できなくなりました。

新しいバックグラウンド実行の制限の詳細については、「Android Developer の[バックグラウンド実行の制限](https://developer.android.com/about/versions/oreo/background.html)」を参照してください。

### <a name="breaking-changes"></a>互換性に影響する変更点

Android Oreo 以降を対象とするアプリでは、次の変更をサポートするようにアプリを変更する必要があります (該当する場合)。

- Android Oreo update-settings は、個々の通知の優先度を設定する機能を備えています。 代わりに、通知チャネルを作成するときに推奨される重要度レベルを設定します。 通知チャネルに割り当てる重要度レベルは、通知メッセージに投稿するすべての通知メッセージに適用されます。

- Android Oreo を対象とする`PendingIntent.GetService()`アプリでは、バックグラウンドで開始されたサービスに新しい制限があるため、は機能しません。 Android Oreo を対象としている場合は、代わりに[Pendingintent](xref:Android.App.PendingIntent.GetBroadcast*)を使用する必要があります。  

## <a name="sample-code"></a>サンプル コード

Android Oreo 機能を活用する方法を示すために、いくつかの Xamarin Android サンプルを利用できます。

- [NotificationsChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels)は、Android Oreo で導入された新しい通知チャネルシステムの使用方法を示しています。 このサンプルでは、2つの通知チャネルを管理します。1つは既定の重要度があり、もう1つは重要度が高くなります。

- 図[Inpicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture)では、Oreo で導入されたハンドヘルドデバイス用の画像 (PiP) モードの基本的な使用方法を示しています。 このサンプルでは、表示モードまたは他のアクティビティを切り替えながら中断され続けるビデオを再生します。

- [Autofillframework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework)は、オートフィルフレームワークの使用方法を示しています。 自動入力が必要なビューを含むクライアントアクティビティの実装と、クライアントアクティビティにオートフィルデータを提供できるサービスが含まれています。

- [ダウンロード](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts)可能なフォントは、前に説明したダウンロード可能なフォント機能の使用方法の例を示しています。

- [EmojiCompat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-emojicompat)は、EmojiCompat サポートライブラリの使用方法を示しています。 このライブラリを使用すると、アプリで見つからない絵文字が "tofu" 文字として表示されないようにすることができます。

- [場所の更新が保留](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdpendintent)になっている場合は、を`PendingIntent`使用してデバイスの場所に関する更新を取得する location API の使用法が示されます。

- [Location Updates フォアグラウンドサービス](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdfgservice)は、location API を使用して、バインドされて開始されたフォアグラウンドサービスを使用してデバイスの場所に関する更新を取得する方法を示します。

## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8.0 Oreo development withC#**

## <a name="summary"></a>Summary

この記事では、Android Oreo について紹介し、android Oreo で Xamarin の開発用の最新のツールとパッケージをインストールして構成する方法について説明しました。 Android Oreo で使用できる主な機能の概要と、いくつかの新機能のソースコード例へのリンクが用意されています。 Android Oreo 用のアプリの作成を開始する際に役立つ API ドキュメントおよび Android 開発者向けのトピックへのリンクが含まれています。 また、既存のアプリに影響する可能性がある、最も重要な Android Oreo 動作の変更も強調表示されています。

## <a name="related-links"></a>関連リンク

- [Android 8.0 Oreo](https://developer.android.com/index.html)
