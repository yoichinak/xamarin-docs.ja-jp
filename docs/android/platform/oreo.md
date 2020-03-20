---
title: Oreo の機能
description: Xamarin.Android を使用して、Android の最新バージョン用のアプリを開発する方法。
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: davidortinau
ms.author: daortin
ms.date: 07/06/2018
ms.openlocfilehash: 56430f8c4988c16a31f9806b0ffb8b6355d6340b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020002"
---
# <a name="oreo-features"></a>Oreo の機能

_Xamarin.Android を使用して、Android の最新バージョン用のアプリを開発する方法。_

[Android 8.0 Oreo](https://developer.android.com/index.html) は Google から入手できる最新バージョンの Android です。 Android Oreo では、Xamarin.Android 開発者にとって興味深い多くの新機能が提供されています。 たとえば、通知チャネル、通知バッジ、XML のカスタム フォント、ダウンロード可能なフォント、自動入力、ピクチャ イン ピクチャ (PIP) などの機能です。 Android Oreo には、これらの新しい機能のための新しい API が追加されました。Xamarin.Android 8.0 以降を使用すると、これらの API を Xamarin.Android アプリで使用できます。

[![Android Oreo ヒーローの画像](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

この記事は、Android 8.0 Oreo 用 Xamarin.Android アプリの開発を始める際に役立つように構成されています。 必要な更新プログラムのインストール方法、SDK の構成方法、テスト用のエミュレーター (またはデバイス) の作成方法について説明します。 また、Android 8.0 Oreo の新機能の概要について説明し、Xamarin.Android アプリで Android Oreo 機能を使用する方法を示すサンプル アプリへのリンクを提供します。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで Android Oreo 機能を使用するには、以下が必要です。

- **Visual Studio** &ndash; Windows を使用している場合、Visual Studio バージョン 15.5 以降が必要です。  Mac を使用している場合は、Visual Studio for Mac バージョン 7.2.0 が必要です。

- **Xamarin.Android** &ndash; Xamarin.Android 8.0 以降をインストールし、Visual Studio で構成する必要があります。

- **Android SDK** &ndash; Android SDK マネージャーを使用して Android SDK 8.0 (API 26) 以降をインストールする必要があります。

## <a name="getting-started"></a>作業の開始

Android Oreo と Xamarin.Android の使用を始めるには、Android Oreo プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. 最新バージョンの Visual Studio に更新します。

2. SDK マネージャーを使用して **Android 8.0.0 (API 26)** 以降のパッケージとツールをインストールします。

3. Android Oreo (API 26) をターゲットとする新しい Xamarin.Android プロジェクトを作成します。

4. Android Oreo アプリをテストできるようにエミュレーターまたはデバイスを構成します。

これらの各手順を以下のセクションで説明します。

### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio と Xamarin.Android を更新する

Visual Studio に Android Oreo のサポートを追加するには、次の手順を実行します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

- Visual Studio 2019 の場合は、[SDK マネージャー](~/android/get-started/installation/android-sdk.md)を使用して API レベル 26.0 以降をインストールします。

- Visual Studio 2017 を使用している場合:

    1. Visual Studio 2017 バージョン 15.7 以降に更新します ([Visual Studio 2017 の更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio)に関するページを参照してください)。

    2. [SDK マネージャー](~/android/get-started/installation/android-sdk.md)を使用して API レベル 26.0 以降をインストールします。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

- 「[Visual Studio for Mac を更新する](https://docs.microsoft.com/visualstudio/mac/update)」で説明されているように、Visual Studio for Mac の最新の安定バージョンに更新します。

-----

Xamarin での Android Oreo のサポートの詳細については、[Xamarin.Android 8.0 のリリース ノート](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/)を参照してください。

### <a name="install-the-android-sdk"></a>Android SDK をインストールする

Xamarin.Android 8.0 でプロジェクトを作成するには、最初に Xamarin Android SDK マネージャーを使用して **Android 8.0 - Oreo** 以降の SDK プラットフォームをインストールする必要があります。 Android SDK Tools 26.0 以降もインストールする必要があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. SDK マネージャーを起動します (Visual Studio で **[ツール] > [Android] > [Android SDK マネージャー]** をクリックします)。

2. **Android 8.0 - Oreo** パッケージをインストールします。 Android SDK エミュレーターを使用している場合は、後で必要になる **x86** システム イメージを必ず含めてください。

    [![Android SDK マネージャーでの Android 8.0 パッケージの選択](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2** 以降、**Android SDK Platform-Tools 26.0.0** 以降、および **Android SDK Build-Tools 26.0.0** (以降) をインストールします。

    [![Android SDK マネージャーでの Android SDK Tools 26 の選択](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. SDK マネージャーを起動します (Visual Studio for Mac で **[ツール] > [SDK マネージャー]** をクリックします)。

2. **Android 8.0 - Oreo** SDK パッケージをインストールします。 Android SDK エミュレーターを使用している場合は、後で必要になる **x86** システム イメージを必ず含めてください。

    [![SDK マネージャーでの Android 8.0 パッケージの選択](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. **Android SDK Tools 26.0.2** 以降、**Android SDK Platform-Tools 26.0.0** 以降、および **Android SDK Build-Tools 26.0.0** (以降) をインストールします。

    [![SDK マネージャーでの Android SDK Tools 26 の選択](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始する

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android の開発を初めて行う場合は、「[Hello, Android](~/android/get-started/hello-android/index.md)」を参照して、Xamarin.Android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、Android 8.0 以降をターゲットとするようにバージョン設定を構成する必要があります。 たとえば、Android 8.0 をプロジェクトのターゲットとするには、プロジェクトのターゲット Android API レベルを **Android 8.0** (API 26) に構成する必要があります。 また、ターゲット フレームワーク レベルを API 26 以降に設定することをお勧めします。 Android API レベルの構成の詳細については、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください。

### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成する

Android SDK Tools 26.0 以降をインストールした後、既定の Google GUI ベースの AVD マネージャーを起動しようとすると、次のエラー ダイアログが表示され、代わりにコマンド ライン AVD マネージャー ツールの **avdmanager** を使用するように指示される場合があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Android エミュレーター マネージャーの警告ダイアログ](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![Android エミュレーター マネージャーの警告ダイアログ](oreo-images/mac/03-avd-warning.png)

-----

このメッセージが表示されるのは、Google から API 26.0 以降をサポートするスタンドアロンの GUI AVD マネージャーを提供されなくなったためです。 Android 8.0 Oreo の場合は、Xamarin Android エミュレーター マネージャーまたはコマンドラインの `avdmanager` ツールを使用して、Android Oreo 用の仮想デバイスを作成する必要があります。

Android デバイス マネージャーを使用して仮想デバイスを作成および管理する方法については、「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照してください。
Android デバイス マネージャーを使用せずに仮想デバイスを作成するには、次のセクションの手順を実行します。

#### <a name="creating-virtual-devices-using-avdmanager"></a>avdmanager を使用して仮想デバイスを作成する

**avdmanager** を使用して新しい仮想デバイスを作成するには、次の手順を実行します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. コマンド プロンプト ウィンドウを開き、`JAVA_HOME` をコンピューター上の Java SDK の場所に設定します。 一般的な Xamarin のインストールでは、次のコマンドを使用できます。

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2. Android SDK の `bin` フォルダーの場所を `PATH` に追加します。
    一般的な Xamarin のインストールでは、次のコマンドを使用できます。

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3. このコマンド プロンプト ウィンドウを閉じて、新しいコマンド プロンプト ウィンドウを開きます。 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) コマンドを使用して、新しい仮想デバイスを作成します。 たとえば、API レベル 26 の x86 システム イメージを使用して **AVD-Oreo-8.0** という名前の AVD を作成するには、次のコマンドを使用します。

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4. **[Do you wish to create a custom hardware profile [no]]\(カスタム ハードウェア プロファイルを作成しますか [いいえ]\)** のプロンプトが表示されたら、「**no**」と入力して既定のハードウェア プロファイルのままにすることができます。 「**yes**」と入力すると、**avdmanager** から、ハードウェア プロファイルをカスタマイズするための質問の一覧に答えるように求められます。

**avdmanager** を使用して仮想デバイスを作成すると、デバイスのプルダウン メニューに追加されます。

[![デバイスのプルダウン メニューに追加された新しい AVD](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. **ターミナル** ウィンドウを開き、Mac 上の Android SDK Tools ディレクトリの場所に移動します。 一般的な Xamarin のインストールでは、次のコマンドを使用できます。

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2. [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) コマンドを使用して、新しい仮想デバイスを作成します。 たとえば、API レベル 26 の x86 システム イメージを使用して **AVD-Oreo-8.0** という名前の AVD を作成するには、次のコマンドを使用します。

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3. **[Do you wish to create a custom hardware profile [no]]\(カスタム ハードウェア プロファイルを作成しますか [いいえ]\)** のプロンプトが表示されたら、「**no**」と入力して既定のハードウェア プロファイルのままにすることができます。 「**yes**」と入力すると、**avdmanager** から、ハードウェア プロファイルをカスタマイズするための質問の一覧に答えるように求められます。

**avdmanager** を使用して仮想デバイスを作成すると、デバイスのプルダウン メニューに追加されます。

[![デバイスのプルダウン メニューに追加された新しい AVD](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

テストおよびデバッグ用の Android Emulator の構成の詳細については、「[Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)」を参照してください。

Nexus、Pixel などの物理デバイスを使用している場合は、自動無線 (OTA) 更新でデバイスを更新するか、システム イメージをダウンロードしてデバイスを直接フラッシュすることができます。 デバイスを Android Oreo に手動で更新する方法の詳細については、「[Factory Images for Nexus and Pixel Devices (Nexus および Pixel デバイスのファクトリ イメージ)](https://developers.google.com/android/images)」を参照してください。

## <a name="new-features"></a>新機能

Android Oreo では、通知チャネル、通知バッジ、XML のカスタム フォント、ダウンロード可能なフォント、自動入力、ピクチャインピクチャなど、さまざまな新機能が導入されました。 以下のセクションでは、これらの機能について説明し、アプリでの使用を開始するのに役立つリンクを示します。

### <a name="notification-channels"></a>通知チャネル

"*通知チャネル*" は、通知のアプリ定義のカテゴリです。
送信する必要がある通知の種類ごとに通知チャネルを作成できます。また、アプリのユーザーが行った選択を反映する通知チャネルを作成できます。 新しい通知チャネル機能を使用すると、さまざまな種類の通知をきめ細かく制御できます。 たとえば、メッセージング アプリを実装している場合、ユーザーが作成する会話グループごとに個別の通知チャネルを作成できます。

「[通知チャネル](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan)」では、通知チャネルを作成し、ローカル通知の投稿に使用する方法について説明しています。 実際のコード例については、[NotificationChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels) サンプルを参照してください。このサンプル アプリでは、2 つのチャネルを管理し、追加の通知オプションを設定しています。

### <a name="notification-badges"></a>通知バッジ

通知バッジは、次のスクリーンショットに示すように、アプリ アイコンの上に表示される小さなドットです。

[![アプリ アイコンの通知バッジの例](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

これらのドットは、そのアプリ アイコンに関連付けられたアプリの 1 つ以上の通知チャネルに、新しい通知があることを示します。これらは、ユーザーがまだ破棄または操作していない通知です。 ユーザーは、アイコンを長押しして通知バッジに関連付けられた通知をひと目で確認することや、表示される長押しメニューから通知を破棄または操作することができます。

通知バッジの詳細については、Android デベロッパーの[通知バッジ](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges)のトピックを参照してください。

### <a name="custom-fonts-in-xml"></a>XML 内のカスタム フォント

Android Oreo では "*XML 内のフォント*" が導入され、カスタム フォントをリソースとして組み込むことができるようになりました。 OpenType ( **.otf**) と TrueType ( **.ttf**) のフォント形式がサポートされています。 フォントをリソースとして追加するには、次の手順を実行します。

1. **Resources/font** フォルダーを作成します。

2. フォント ファイル (たとえば、 **.ttf** ファイルや **.otf** ファイル) を **Resources/font** にコピーします。 

3. 必要に応じて、Android ファイルの名前付け規則に準拠するように各フォント ファイルの名前を変更します (つまり、ファイル名には小文字の *a-z*、*0-9*、アンダースコアのみを使用します)。 たとえば、フォント ファイル `Pacifico-Regular.ttf` は `pacifico.ttf` のような名前に変更できます。

4. レイアウト XML で新しい `android:fontFamily` 属性を使用して、カスタム フォントを適用します。 たとえば、次の `TextView` 宣言では、追加された **pacifico.ttf** フォント リソースが使用されます。

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

また、複数のフォントとスタイル、太さの詳細を記述するフォント ファミリ XML ファイルを作成することもできます。 詳細については、Android デベロッパーの「[Fonts in XML (XML 内のフォント)](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html)」トピックを参照してください。

### <a name="downloadable-fonts"></a>ダウンロード可能なフォント

Android Oreo 以降、アプリは APK にフォントをバンドルするのではなく、プロバイダーにフォントを要求できるようになりました。 フォントは、必要な場合にのみネットワークからダウンロードされます。 この機能により、APK のサイズが小さくなり、携帯電話のメモリと携帯電話データの使用量が節約されます。 Android サポート ライブラリ 26 パッケージをインストールすることで、Android API バージョン 14 以降でこの機能を使用することもできます。

アプリにフォントが必要な場合は、(ダウンロードするフォントを指定して) `FontsRequest` オブジェクト を作成し、それを `FontsContract` メソッドに渡してフォントをダウンロードします。 次の手順では、フォントのダウンロード プロセスについて詳しく説明します。

1. [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) オブジェクトをインスタンス化します。 

2. [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html) をサブクラス化してインスタンス化します。

3. [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) メソッドを実装します。これは、フォント要求の完了を処理するために使用されます。

4. [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) メソッドを実装します。これは、フォント要求プロセス中に発生したエラーをアプリに通知するために使用されます。

5. [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) メソッドを呼び出して、フォント プロバイダーからフォントを取得します。 

`RequestFonts` メソッドを呼び出すと、最初に (前の `RequestFont` の呼び出しから) フォントがローカルにキャッシュされているかどうかが確認されます。 キャッシュされていない場合は、フォント プロバイダーを呼び出し、非同期でフォントを取得し、`OnTypeFaceRetrieved` メソッドを呼び出して結果をアプリに返します。

[DownloadableFonts](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) サンプルは、Android Oreo で導入されたダウンロード可能なフォント機能の使用方法を示しています。 

フォントのダウンロードの詳細については、Android デベロッパーの「[DownloadableFonts](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html)」トピックを参照してください。

### <a name="autofill"></a>自動入力

Android Oreo の新しい "_自動入力_" フレームワークを使用すると、ユーザーはログイン、アカウント作成、クレジット カード トランザクションなどの繰り返し発生するタスクを簡単に処理できるようになります。 ユーザーは、(入力エラーにつながる可能性がある) 情報を再入力する時間が短縮されます。 アプリで自動入力フレームワークを使用するには、システム設定で自動入力 サービスを有効にしておく必要があります (ユーザーは自動入力を有効または無効にできます)。

[AutofillFramework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) サンプルは、自動入力フレームワークの使用方法を示しています。 これには、自動入力が必要なビューがあるクライアント アクティビティと、クライアント アクティビティに自動入力データを提供できるサービスの実装が含まれます。

新しい自動入力機能と、自動入力用にアプリを最適化する方法の詳細については、Android デベロッパーの「[自動入力フレームワーク](https://developer.android.com/guide/topics/text/autofill.html)」トピックを参照してください。

### <a name="picture-in-picture-pip"></a>ピクチャ イン ピクチャ (PIP)

Android Oreo を使用すると、アクティビティを ピクチャインピクチャ (PIP) モードで起動して、別のアクティビティの画面をオーバーレイすることができます。 この機能は、ビデオの再生を目的としています。

アプリのアクティビティで PIP モードを使用できるように指定するには、Android マニフェストで次のフラグを true に設定します。

```xml
android:supportsPictureInPicture
```

PIP モードの場合のアクティビティの動作を指定するには、新しい [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) オブジェクトを使用します。 `PictureInPictureParams` は、PIP モードでアクティビティを初期化および更新するために使用する一連のパラメーターを表します (たとえば、アクティビティの優先縦横比)。 Android Oreo の `Activity` には、次の新しい PIP メソッドが追加されました。

- [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; アクティビティを PIP モードにします。 アクティビティは画面の隅に配置され、画面の残りの部分には画面上にあった前のアクティビティが表示されます。

- [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; アクティビティの PIP 構成設定を更新します (たとえば、縦横比の変更)。

[PictureInPicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) サンプルは、Oreo で導入されたハンドヘルド デバイスでのピクチャインピクチャ (PiP) モードの基本的な使用方法を示しています。 このサンプルでは、表示モードまたは他のアクティビティを切り替えても中断せずに継続してビデオが再生されます。

### <a name="other-features"></a>その他の機能

Android Oreo には、他にも多数の新機能が追加されました。たとえば、絵文字サポート ライブラリ、Location API、背景の制限、アプリの広色域、新しいオーディオ コーデック、WebView の機能強化、キーボード ナビゲーション サポートの向上、ハイパフォーマンス低遅延のオーディオを実現するための新しい AAudio (プロ オーディオ) API などです。これらの機能の詳細については、Android デベロッパーの [Android Oreo の機能と API](https://developer.android.com/about/versions/oreo/android-8.0.html) のトピックを参照してください。

## <a name="behavior-changes"></a>動作の変更

Android Oreo には、既存のアプリの機能に影響を与える可能性があるさまざまなシステムと API の動作の変更が含まれています。 ここでは、これらの変更について説明します。

### <a name="background-execution-limits"></a>バックグラウンド実行の制限

Android Oreo では、ユーザー エクスペリエンスを向上させるために、バックグラウンドで実行中にアプリが実行できる機能に制限を設けています。 たとえば、ユーザーがビデオを見たりゲームをプレイしている場合、バックグラウンドで実行されているアプリによって、フォアグラウンドで実行されているビデオを多様するアプリのパフォーマンスが損なわれる可能性があります。 その結果、Android Oreo では、ユーザーと直接対話していないアプリに次の制限が課されるようになりました。

1. **バックグラウンド サービスの制限** &ndash; バックグラウンドでアプリが実行されている場合、引き続きサービスを作成および使用できる数分間の期間が与えられます。 その期間が終了すると、Android によってアプリのバックグラウンド サービスは停止され、"_アイドル_" として処理されます。

2. **ブロードキャストの制限** &ndash; Android 7.0 (API 25) によって、アプリで受信するために登録するブロードキャストに制限が課せられました。 Android Oreo では、これらの制限がさらに厳しくなりました。 たとえば、Android Oreo アプリでは、マニフェストで暗黙的なブロードキャスト用のブロードキャスト レシーバーを登録できなくなりました。

新しいバックグラウンド実行の制限の詳細については、Android デベロッパーの「[バックグラウンド実行制限](https://developer.android.com/about/versions/oreo/background.html)」トピックを参照してください。

### <a name="breaking-changes"></a>互換性に影響する変更点

Android Oreo 以降をターゲットとするアプリでは、該当する場合、以下の変更点をサポートするようにアプリを変更する必要があります。

- Android Oreo では、個々の通知の優先度を設定する機能が廃止されました。 代わりに、通知チャネルを作成するときに推奨される重要度レベルを設定します。 通知チャネルに割り当てる重要度レベルは、通知チャネルに投稿されるすべての通知メッセージに適用されます。

- Android Oreo をターゲットとするアプリの場合、バックグラウンドで開始されるサービスには新しい制限が課されるため、`PendingIntent.GetService()` は機能しません。 Android Oreo をターゲットにしている場合は、代わりに [PendingIntent.GetBroadcast](xref:Android.App.PendingIntent.GetBroadcast*) を使用する必要があります。  

## <a name="sample-code"></a>サンプル コード

Android Oreo の機能を使用する方法がわかる複数の Xamarin.Android サンプルを利用できます。

- [NotificationsChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels) は、Android Oreo で導入された新しい通知チャネル システムの使用方法を示しています。 このサンプルでは、2 つの通知チャネルを管理しています。1 つは既定の重要度のもの、もう 1 つは重要度が高いものです。

- [PictureInPicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) は、Oreo で導入されたハンドヘルド デバイスでのピクチャインピクチャ (PiP) モードの基本的な使用方法を示しています。 このサンプルでは、表示モードまたは他のアクティビティを切り替えても中断せずに継続してビデオが再生されます。

- [AutofillFramework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) は、自動入力フレームワークの使用方法を示しています。 これには、自動入力が必要なビューがあるクライアント アクティビティと、クライアント アクティビティに自動入力データを提供できるサービスの実装が含まれます。

- [DownloadableFonts](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) は、前述のダウンロード可能なフォント機能の使用方法の例を示しています。

- [EmojiCompat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-emojicompat) は、EmojiCompat サポート ライブラリの使用方法を示しています。 このライブラリを使用すると、アプリに存在しない絵文字を "豆腐" 文字として表示しないようにすることができます。

- [LocationUpdatesPendingIntent](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdpendintent) は、Location API を使用し、`PendingIntent` を使用してデバイスの位置に関する更新情報を取得する方法を示しています。

- [LocationUpdatesForegroundService](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdfgservice) は、Location API を使用し、バインドされ、開始されたフォアグラウンド サービスを使用してデバイスの位置に関する更新情報を取得する方法を示しています。

## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C# を使用した Android 8.0 Oreo での開発**

## <a name="summary"></a>まとめ

この記事では、Android Oreo について紹介し、Android Oreo での Xamarin.Android 開発用に最新のツールとパッケージをインストールして構成する方法について説明しました。 Android Oreo で使用できる主な機能の概要を説明し、いくつかの新機能のソース コードのリンクを示しました。 Android Oreo 用アプリを作り始めるために役立つ API ドキュメントと Android デベロッパー トピックへのリンクが記載されています。 また、既存のアプリに影響する可能性のある最も重要な Android Oreo の動作の変更点についても説明しました。

## <a name="related-links"></a>関連リンク

- [Android 8.0 Oreo](https://developer.android.com/index.html)
