---
title: Android 9 Pie
description: Xamarin.Android を使用して Android 9 Pie 用アプリの開発を始める方法について説明します。
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 0105b43116df697bc6688becb77298c236dfa601
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019885"
---
# <a name="android-pie-features"></a>Android Pie の機能

_Xamarin.Android を使用して Android 9 Pie 用アプリの開発を始める方法について説明します。_

[Android 9 Pie](https://developer.android.com/about/versions/pie/) を Google から入手できるようになりました。 このリリースでは、多数の新機能と API が提供されており、その多くは最新の Android デバイスで新しいハードウェア機能を使用するために必要です。

![Android Pie ヒーローの画像](pie-images/01-android-p-logo.png)

この記事は、Android Pie 用 Xamarin.Android アプリの開発を始める際に役立つように構成されています。 必要な更新プログラムのインストール方法、SDK の構成方法、テスト用のエミュレーターまたはデバイスの準備方法について説明します。 また、Android Pie の新機能の概要について説明し、Android Pie の主要な機能のいくつかを使用する方法を示すサンプル ソース コードを提供します。

Xamarin.Android 9.0 は、Android Pie のサポートを提供しています。 Xamarin.Android の Android Pie のサポートの詳細については、[Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1) のリリース ノートを参照してください。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで Android Pie の機能を使用するには、次の一覧が必要です。

- **Visual Studio** &ndash; Visual Studio 2019 が推奨されます。
    Visual Studio 2017 を使用している場合、Windows 上では Visual Studio 2017 バージョン 15.8 以降に更新します。 macOS 上では、Visual Studio 2017 for Mac バージョン 7.6 以降に更新します。

- **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 以降を Visual Studio と共にインストールする必要があります (Xamarin.Android は、 **[.NET によるモバイル開発]** ワークロードの一部として自動的にインストールされます)。

- **Java Developer Kit** &ndash; Xamarin Android 9.0 での開発には、[JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) が必要です (または [OpenJDK](~/android/get-started/installation/openjdk.md) の Microsoft によるディストリビューションのプレビューを試すことができます)。 JDK8 は、 **[.NET によるモバイル開発]** ワークロードの一部として自動的にインストールされます。

- **Android SDK** &ndash; Android SDK マネージャーを使用して Android SDK API 28 以降をインストールする必要があります。

## <a name="getting-started"></a>作業の開始

Xamarin.Android を使用して Android Pie アプリの開発を始めるには、最初の Android Pie プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. Visual Studio 2019 をお勧めします。 Visual Studio 2017 を使用している場合は、[Visual Studio 2017 バージョン 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 以降に更新します。 Visual Studio for Mac を使用している場合は、[Visual Studio 2017 for Mac バージョン 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 以降に更新します。

2. SDK マネージャーを使用して **Android Pie (API 28)** パッケージとツールをインストールします。

3. **Android 9.0** をターゲットとする新しい Xamarin.Android プロジェクトを作成します。

4. Android Pie アプリをテストできるようにエミュレーターまたはデバイスを構成します。

これらの各手順を以下のセクションで説明します。

### <a name="update-visual-studio"></a>Visual Studio を更新する

Xamarin を使用して Android Pie アプリをビルドするには、Visual Studio 2019 をお勧めします。

Visual Studio 2017 を使用している場合は、Visual Studio 2017 バージョン 15.8 以降に更新します (手順については、[Visual Studio 2017 を最新リリースに更新する](https://docs.microsoft.com/visualstudio/install/update-visual-studio)方法に関するページを参照してください)。 macOS 上では、Visual Studio 2017 for Mac 7.6 以降に更新します (手順については、[Visual Studio for Mac のセットアップとインストール](https://docs.microsoft.com/visualstudio/mac/installation)に関するページを参照してください)。

### <a name="install-the-android-sdk"></a>Android SDK をインストールする

Xamarin.Android 9.0 でプロジェクトを作成するには、最初に Android SDK マネージャーを使用して **Android Pie (API レベル 28)** 以降の SDK プラットフォームをインストールする必要があります。

1. SDK マネージャーを起動します。 Visual Studio では、 **[ツール] > [Android] > [Android SDK マネージャー]** をクリックします。 Visual Studio for Mac では、 **[ツール] > [SDK マネージャー]** をクリックします。

2. 右下隅の歯車アイコンをクリックして、 **[リポジトリ] > [Google (サポート対象外)]** を選択します。

    [![リポジトリの Google への設定](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. **Android Pie** SDK パッケージをインストールします。これらは、 **[プラットフォーム]** タブで **[Android SDK Platform 28]\(Android SDK プラットフォーム 28\)** という名前で一覧に表示されます (SDK マネージャーの使用方法の詳細については、「[Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください)。

    [![Android Pie パッケージのインストール](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. エミュレーターを使用している場合は、**API レベル 28** をサポートする仮想デバイスを作成します。 仮想デバイスの作成の詳細については、「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照してください。

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始する

新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android の開発を初めて行う場合は、「[Hello, Android](~/android/get-started/hello-android/index.md)」を参照して、Xamarin.Android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、Android 9.0 以降をターゲットとするようにバージョン設定を構成する必要があります。 たとえば、Android Pie をプロジェクトのターゲットとするには、プロジェクトのターゲット Android API レベルを **Android 9.0** (API 28) に構成する必要があります。 また、ターゲット フレームワーク レベルを API 28 以降に設定することをお勧めします。 Android API レベルの構成の詳細については、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください。

### <a name="configure-a-device-or-emulator"></a>デバイスまたはエミュレーターを構成する

Nexus や Pixel などの物理デバイスを使用している場合は、「[Factory Images for Nexus and Pixel Devices (Nexus および Pixel デバイスのファクトリ イメージ)](https://developers.google.com/android/images)」の手順に従って、デバイスを Android Pie に更新できます。

エミュレーターを使用している場合は、API レベル 28 の仮想デバイスを作成し、x86 ベースのイメージを選択します。 Android デバイス マネージャーを使用して仮想デバイスを作成および管理する方法については、「[Android Device Manager による仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照してください。
テストとデバッグに Android エミュレーターを使用する方法の詳細については、「[Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)」を参照してください。

## <a name="new-features"></a>新機能

Android Pie には、さまざまな新機能が導入されています。 これらの新機能には、最新の Android デバイスが提供する新しいハードウェア機能を利用することを目的にしたものと、Android のユーザー エクスペリエンスをさらに強化するために設計されたものがあります。

- **ディスプレイの切り欠きのサポート** &ndash; 新しい Android デバイスの画面上部にある "_切り欠き_" の位置と形状を検出するための API が用意されています。

- **通知の機能強化** &ndash;通知メッセージに画像を表示できるようになりました。会話の参加者を簡略化するために新しい `Person` クラスが使用されます。

- **屋内位置測定** &ndash; プラットフォームが WiFi ラウンドトリップタイム プロトコルをサポートしています。これにより、アプリから屋内設定でのナビゲーションに WiFi デバイスを使用できるようになります。

- **マルチカメラ サポート** &ndash; 複数の物理カメラ (デュアルフロント カメラやデュアルバック カメラなど) からストリームに同時にアクセスする機能が用意されています。

以下のセクションでは、これらの機能について説明し、アプリで使い始める場合に役立つ簡単なコード例を示します。

### <a name="display-cutout-support"></a>ディスプレイの切り欠きのサポート

最先端の画面を備えた新しい Android デバイスの多くは、カメラとスピーカーのためにディスプレイの上部に "*ディスプレイの切り欠き*" (または "ノッチ") があります。
次のスクリーンショットは、切り欠きのエミュレーターの例を示しています。

[![切り欠きをシミュレートする Android エミュレーター](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

ディスプレイの切り欠きがあるデバイスでアプリ ウィンドウにコンテンツを表示する方法を管理できるように、Android Pie には新しい [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) ウィンドウ レイアウト属性が追加されました。 この属性の値は次のいずれかに設定することができます。

- [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; ウィンドウを切り欠き領域と重ねることはできません。

- [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; ウィンドウは切り欠き領域 (ただし、画面の短辺のみ) にまで拡張できます。 

- [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; 切り欠きがシステム バー内に含まれている場合、ウィンドウを切り欠き領域に拡張できます。

たとえば、アプリ ウィンドウが切り欠き領域と重ならないようにするには、レイアウトの切り欠きモードを *never* に設定します。 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

これらの切り欠きモードの例を次に示します。 左側の最初のスクリーンショットは、非全画面表示モードのアプリです。 中央のスクリーンショットでは、`LayoutInDisplayCutoutMode` が `LayoutInDisplayCutoutModeShortEdges` に設定され、アプリは全画面表示に切り替わっています。 アプリの白い背景がディスプレイの切り欠き領域まで広がっていることに注意してください。

[![エミュレーターのディスプレイの切り欠きモードの例](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

最後のスクリーンショット (右上) では、全画面表示に移行する前に `LayoutInDisplayCutoutMode` が `LayoutInDisplayCutoutModeShortNever` に設定されています。
アプリの白い背景は、ディスプレイの切り欠き領域への拡張が許可されていないことに注意してください。

デバイスの切り欠き領域に関する詳細情報が必要な場合は、新しい [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) クラスを使用できます。 `DisplayCutout` は、コンテンツの表示に使用できないディスプレイの領域を表します。 アプリでこの非機能領域にコンテンツを表示しないように、この情報を使用して切り欠きの位置と形状を取得することができます。

Android P の新しい切り欠き機能の詳細については、「[ディスプレイの切り欠きのサポート](https://developer.android.com/about/versions/pie/android-9.0#cutout)」を参照してください。

### <a name="notifications-enhancements"></a>通知の機能強化

Android Pie では、メッセージングのエクスペリエンスを向上させるために以下の機能強化が導入されています。

- 通知チャネル ([Android Oreo](~/android/platform/oreo.md) で導入) は、チャネル グループのブロックをサポートするようになりました。

- 通知システムには、3 つの新しい "応答不可" カテゴリが追加されました (アラーム、システム サウンド、およびメディア ソースの優先順)。 さらに、視覚的な中断 (バッジ、通知ライト、ステータス バーの外観、全画面アクティビティの起動など) を抑制するために使用できる 7 つの新しい "応答不可" モードが追加されました。

- メッセージの送信者を表す新しい [Person](https://developer.android.com/reference/android/app/Person.html) クラスが追加されました。 このクラスを使用すると、会話に関係する人 (アバターや URI を含む) を識別することにより、各通知のレンダリングを最適化できます。

- 通知に画像を表示できるようになりました。 

次の例は、新しい API を使用して、画像を含む通知を生成する方法を示しています。 次のスクリーンショットでは、テキスト通知が投稿され、その後に画像が埋め込まれた通知が続きます。 (右に示すように) 通知が展開されると、最初の通知のテキストが表示され、2 番目の通知に埋め込まれた画像が拡大されます。

[![画像を含む通知の例](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

次の例は、Android Pie の通知に画像を含める方法を示しています。また、新しい `Person` クラスの使用方法を示しています。

1. 送信者を表す `Person` オブジェクトを作成します。 たとえば、送信者の名前とアイコンは `fromPerson` に含まれています。

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. 送信する画像を含む `Notification.MessagingStyle.Message` を作成し、新しい [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) メソッドに画像を渡します。
   次に例を示します。

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. メッセージを `Notification.MessagingStyle` オブジェクトに追加します。 次に例を示します。

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. このスタイルを通知ビルダーに接続します。 次に例を示します。

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. 通知を発行します。 次に例を示します。

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

通知の作成の詳細については、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)に関するページを参照してください。

### <a name="indoor-positioning"></a>屋内位置測定

Android Pie は IEEE 802.11mc ("_WiFi ラウンドトリップタイム_" または _WiFi RTT_ とも呼ばれます) をサポートします。そのため、アプリでは 1 つ以上の Wi-Fi アクセス ポイントまでの距離を検出できます。 この情報を使用すると、1 から 2 メートルの精度で "*屋内位置測定*" をアプリに利用できます。 IEEE 801.11mc のハードウェア サポートを提供する Android デバイスでは、スマート アプライアンスの位置ベースの制御や店舗でのターンバイターン方式の指示などのナビゲーション機能をアプリで提供できます。

[![WiFi RTT を使用した屋内ナビゲーションの例](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

新しい [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) クラスといくつかのヘルパー クラスを使うと、Wi-Fi デバイスまでの距離を測定することができます。 Android P で導入された屋内位置測定 API の詳細については、[Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary) を参照してください。

### <a name="multi-camera-support"></a>マルチカメラのサポート

多くの新しい Android デバイスには、ステレオ ビジョン、視覚効果の向上、ズーム機能の向上などの機能に役立つデュアルフロント カメラやデュアルバック カメラが搭載されています。 Android P には新しい[マルチカメラ](https://developer.android.com/about/versions/pie/android-9.0#camera) API が導入され、複数の物理カメラに裏打ちされた "*論理カメラ*" (または "*論理マルチカメラ*") をアプリで使用できるようになりました。
デバイスで論理マルチカメラがサポートされているかどうかを判断するには、デバイス上の各カメラの機能を調べて、[RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA) をサポートしているかどうかを確認します。

Android Pie には、新しい [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) クラスも追加されました。このクラスを使用すると、初期キャプチャ中の遅延が短縮され、カメラ ストリームを開始する必要がなくなります。

Android P でのマルチカメラのサポートの詳細については、「[マルチカメラのサポートとカメラ機能のアップデート](https://developer.android.com/about/versions/pie/android-9.0#camera)」を参照してください。

### <a name="other-features"></a>その他の機能

さらに、Android Pie では他のいくつかの新機能がサポートされています。

- 新しい [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) クラス。アニメーション画像の描画と表示に使用できます。

- `BitmapFactory` を置き換える新しい [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) クラス。 `ImageDecoder` を使用して `AnimatedImageDrawable` をデコードできます。

- HDR (ハイ ダイナミック レンジ) ビデオおよび HEIF (高効率画像ファイル形式) 画像のサポート。

- [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) が強化され、ネットワーク関連のジョブをよりインテリジェントに処理できるようになりました。 [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) クラスの新しい [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) メソッドからは、特定のジョブのネットワーク要求を実行するために最適なネットワークが返されます。

最新の Android Pie 機能の詳細については、「[Android 9 の機能と API](https://developer.android.com/about/versions/pie/android-9.0)」を参照してください。

## <a name="behavior-changes"></a>動作の変更

ターゲット Android バージョンを API レベル 28 に設定している場合は、上記の新機能を実装していない場合でも、アプリの動作に影響が及ぶ可能性のあるプラットフォームの変更がいくつかあります。 このような変更の簡単な概要を次に示します。

- フォアグラウンド サービスを使用する前に、アプリでフォアグラウンドのアクセス許可を要求する必要があります。

- アプリに複数のプロセスがある場合、1 つの [WebView](xref:Android.Webkit.WebView) データ ディレクトリをプロセス間で共有できません。

- パスで別のアプリのデータ ディレクトリに直接アクセスすることはできなくなりました。

Android P をターゲットとするアプリの動作の変更の詳細については、「[動作の変更点](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps)」を参照してください。

## <a name="sample-code"></a>サンプル コード

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) は、ディスプレイの切り欠きモードの設定方法、新しい `Person` クラスの使用方法、画像を含む通知の送信方法を示す Android Pie 用の Xamarin.Android サンプル アプリです。

## <a name="summary"></a>まとめ

この記事では、Android Pie について紹介し、Android Pie での Xamarin.Android 開発用に最新のツールとパッケージをインストールして構成する方法について説明しました。 Android Pie で使用できる主な機能の概要と、これらの機能のいくつかのソース コードの例を示しました。
Android Pie 用アプリを作り始めるために役立つ API ドキュメントと Android デベロッパー トピックへのリンクが記載されています。 また、既存のアプリに影響する可能性のある最も重要な Android Pie の動作の変更点についても説明しました。

## <a name="related-links"></a>関連リンク

- [Android 9 Pie](https://developer.android.com/about/versions/pie/)
