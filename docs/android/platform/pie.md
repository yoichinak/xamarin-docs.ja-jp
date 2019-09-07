---
title: Android 9 Pie
description: Xamarin を使用して Android 9 の円用アプリの開発を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: 6475cd0f27e41321902b57dd28f59bfb250e0c8f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757459"
---
# <a name="android-pie-features"></a>Android の円機能

_Xamarin を使用して Android 9 の円用アプリの開発を開始する方法について説明します。_

[Android 9 の円](https://developer.android.com/about/versions/pie/)は Google から入手できるようになりました。 このリリースでは、多数の新機能と Api が提供されており、その多くは最新の Android デバイスで新しいハードウェア機能を利用するために必要です。

![Android の円のヒーロー画像](pie-images/01-android-p-logo.png)

この記事は、Android 用の Xamarin Android アプリの開発を始める際に役立つように構成されています。 ここでは、必要な更新プログラムをインストールし、SDK を構成し、テスト用のエミュレーターまたはデバイスを準備する方法について説明します。 また、Android 円の新機能の概要を示し、いくつかの主要な Android 円機能の使用方法を示すサンプルソースコードを提供します。

Android 9.0 では、Android の円グラフがサポートされています。 Android 用の Xamarin のサポートの詳細については、 [Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1)のリリースノートを参照してください。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで Android の円機能を使用するには、次の一覧が必要です。

- **Visual Studio**&ndash; Visual Studio 2019 をお勧めします。
    Visual Studio 2017 を使用している場合は、Windows update で Visual Studio 2017 バージョン15.8 以降に更新します。 MacOS で、Visual Studio 2017 for Mac バージョン7.6 以降に更新します。

- **Xamarin 9.0.0.17**以降を Visual Studio と共にインストールする必要があります (xamarin android は、.net ワークロード**を使用したモバイル開発**の一部として自動的にインストールされます)。 &ndash;

- **Java Developer Kit**Xamarin Android 9.0 開発には[JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)が必要です (または、Microsoft の[openjdk](~/android/get-started/installation/openjdk.md)の配布プレビューを試すことができます)。 &ndash; JDK8 は、.NET ワークロード**を使用したモバイル開発**の一部として自動的にインストールされます。

- **Android SDK**&ndash; Android SDK Manager を使用して Android SDK API 28 以降をインストールする必要があります。

## <a name="getting-started"></a>作業の開始

Xamarin android で Android の円アプリの開発を始めるには、最初の Android 円プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。

1. Visual Studio 2019 をお勧めします。 Visual Studio 2017 を使用している場合は、 [Visual studio 2017 バージョン 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes)以降に更新してください。 Visual Studio for Mac を使用している場合は、 [Visual Studio 2017 For Mac バージョン 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes)以降に更新してください。

2. SDK Manager を使用して、 **Android の円 (API 28)** パッケージとツールをインストールします。

3. **Android 9.0**を対象とする新しい Xamarin android プロジェクトを作成します。

4. Android の円アプリをテストするためのエミュレーターまたはデバイスを構成します。

これらの各手順については、次のセクションで説明します。

### <a name="update-visual-studio"></a>Visual Studio 2017 を更新する

Xamarin を使用して Android の円アプリをビルドする場合は、Visual Studio 2019 をお勧めします。

Visual studio 2017 を使用している場合は、Visual Studio 2017 バージョン15.8 以降に更新してください (手順については、「 [Visual studio 2017 を最新のリリースに更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio)する」を参照してください)。 MacOS で、Mac 7.6 以降の Visual Studio 2017 に更新します (手順については、「 [Visual Studio for Mac のセットアップとインストール](https://docs.microsoft.com/visualstudio/mac/installation)」を参照してください)。

### <a name="install-the-android-sdk"></a>Android SDK のインストール

Xamarin Android 9.0 を使用してプロジェクトを作成するには、最初に Android SDK マネージャーを使用して Android 用 SDK platform **(API レベル 28)** 以降をインストールする必要があります。

1. SDK Manager を起動します。 Visual Studio で、 **[Tools > Android > Android SDK Manager]** をクリックします。 Visual Studio for Mac で、 **[ツール > SDK Manager]** をクリックします。

2. 右下隅にある歯車アイコンをクリックし、 **[リポジトリ > Google (サポートされていません)]** を選択します。

    [![Google にリポジトリを設定しています](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. **[プラットフォーム]** タブで**Android SDK Platform 28**と表示されている**Android の円**sdk パッケージをインストールします (sdk Manager の使用方法の詳細については、「 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください)。

    [![Android の円パッケージのインストール](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. エミュレーターを使用している場合は、 **API レベル 28**をサポートする仮想デバイスを作成します。 仮想デバイスの作成の詳細については、「 [Android Device Manager を使用した仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照してください。

### <a name="start-a-xamarinandroid-project"></a>Xamarin. Android プロジェクトを開始する

新しい Xamarin. Android プロジェクトを作成します。 Xamarin を使用した Android 開発を初めて使用する場合は、「 [Hello, android](~/android/get-started/hello-android/index.md) 」を参照して、xamarin android プロジェクトの作成について学習してください。

Android プロジェクトを作成するときは、バージョン設定を Android 9.0 以降を対象とするように構成する必要があります。 たとえば、Android 用のプロジェクトを対象にするには、プロジェクトのターゲットの Android API レベルを**android 9.0** (API 28) に構成する必要があります。 また、ターゲットフレームワークレベルを API 28 以降に設定することをお勧めします。 Android API レベルの構成の詳細については、「 [ANDROID Api レベルについ](~/android/app-fundamentals/android-api-levels.md)て」を参照してください。

### <a name="configure-a-device-or-emulator"></a>デバイスまたはエミュレーターを構成する

複数のデバイスを使用している場合は、デバイスを Android 円グラフに更新することができます。これを行うには、「デバイス[の出荷先イメージ](https://developers.google.com/android/images)」の指示に従ってください。

エミュレーターを使用している場合は、API レベル28用の仮想デバイスを作成し、x86 ベースのイメージを選択します。 Android Device Manager を使用した仮想デバイスの作成と管理の詳細については、「 [Android Device Manager を使用した仮想デバイスの管理](~/android/get-started/installation/android-emulator/device-manager.md)」を参照してください。
テストとデバッグに Android エミュレーターを使用する方法の詳細については、「 [Android Emulator でのデバッグ](~/android/deploy-test/debugging/debug-on-emulator.md)」を参照してください。

## <a name="new-features"></a>新機能

Android の円グラフには、さまざまな新機能が導入されています。 これらの新機能の一部は、最新の Android デバイスによって提供される新しいハードウェア機能を利用することを目的としていますが、Android ユーザーエクスペリエンスをさらに強化するように設計されています。

- **切り取り線のサポートを表示**する新しい Android デバイス上の画面の上部にある_カットアウト_の位置と形状を検索するための api を提供します。 &ndash;

- **通知の機能強化**通知メッセージに画像を表示できるようになり`Person`ました。また、新しいクラスを使用して、メッセージ交換の参加者を簡略化します。 &ndash;

- **室内ポジショニング**&ndash; Wifi ラウンドトリップタイムプロトコルのプラットフォームサポート。これにより、アプリは、屋内設定のナビゲーションに wifi デバイスを使用できるようになります。

- **マルチカメラのサポート**&ndash;では、複数の物理カメラ (デュアルフロントとデュアルバックカメラなど) からストリームに同時にアクセスする機能が提供されています。

以下のセクションでは、これらの機能について説明し、アプリでの使用を開始するのに役立つ簡単なコード例を示します。

### <a name="display-cutout-support"></a>切り取り線のサポートを表示する

エッジツーエッジ画面を搭載した多くの新しい Android デバイスには、カメラとスピーカーのディスプレイの上部に表示される*カットアウト*("ノッチ") があります。
次のスクリーンショットは、カットアウトのエミュレーターの例を示しています。

[![カットアウトをシミュレートする Android エミュレーター](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

アプリウィンドウで、表示をカットアウトしてデバイスにコンテンツを表示する方法を管理するために、Android の円グラフに新しい[LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode)ウィンドウレイアウト属性が追加されました。 この属性は、次のいずれかの値に設定できます。

- [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER)&ndash;ウィンドウは、カットアウト領域と重複することはできません。

- [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES)&ndash;ウィンドウは、カットアウト領域に拡張できますが、画面の短辺にのみ表示されます。 

- [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT)&ndash;システムバー内にカットアウトが含まれている場合、ウィンドウは切り出領域に拡張できます。

たとえば、アプリウィンドウが切り使用領域と重ならないようにするには、レイアウトのカットアウトモードを [*なし*] に設定します。 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

これらのカットアウトモードの例を次に示します。 左側の最初のスクリーンショットは、非全画面モードのアプリです。 中央のスクリーンショットでは、がに`LayoutInDisplayCutoutMode` `LayoutInDisplayCutoutModeShortEdges`設定された状態でアプリが全画面表示されます。 アプリの白い背景が、表示のカットアウト領域まで拡大していることに注意してください。

[![例エミュレーターでのカットアウトモードの表示](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

最後のスクリーンショット (右側) では、 `LayoutInDisplayCutoutMode`が全画面表示になる前にに`LayoutInDisplayCutoutModeShortNever`設定されています。
アプリの白い背景は、表示のカットアウト領域への拡張が許可されていないことに注意してください。

デバイスの切り抜き領域に関する詳細情報が必要な場合は、新しい[Displaycutout](https://developer.android.com/reference/android/view/DisplayCutout.html)クラスを使用できます。 `DisplayCutout`コンテンツの表示に使用できないディスプレイの領域を表します。 この情報を使用して、アプリケーションが非機能領域のコンテンツを表示しないようにするために、カットアウトの場所と形状を取得できます。

Android P の新しいカットアウト機能の詳細については、「[切り取り線のサポートを表示](https://developer.android.com/about/versions/pie/android-9.0#cutout)する」を参照してください。

### <a name="notifications-enhancements"></a>通知の機能強化

Android の円グラフでは、メッセージングエクスペリエンスを向上させるために次の機能強化が導入されています。

- ( [Android Oreo](~/android/platform/oreo.md)で導入された) 通知チャネルで、チャネルグループのブロックがサポートされるようになりました。

- 通知システムには、3つの新しい [応答不可] カテゴリがあります (アラーム、システムサウンド、およびメディアソースに優先順位を付けます)。 また、視覚的な中断 (バッジ、通知ライト、ステータスバーの外観、全画面表示の活動の開始など) を抑制するために使用できる、新しい "応答不可" モードが7つあります。

- メッセージの送信者を表す新しい[Person](https://developer.android.com/reference/android/app/Person.html)クラスが追加されました。 このクラスを使用すると、メッセージ交換に関係するユーザー (アバターや Uri など) を識別することによって、各通知のレンダリングを最適化できます。

- 通知に画像が表示されるようになりました。 

次の例は、新しい Api を使用して、イメージを含む通知を生成する方法を示しています。 次のスクリーンショットでは、テキスト通知が送信され、その後に埋め込み画像を含む通知が続きます。 (右側に示されているように) 通知が展開されると、最初の通知のテキストが表示され、2番目の通知に埋め込まれている画像が拡大されます。

[![イメージを使用した通知の例](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

次の例は、Android の円通知に画像を含める方法を示しています。また、 `Person`新しいクラスの使用方法を示しています。

1. 送信者`Person`を表すオブジェクトを作成します。 たとえば、送信者の名前とアイコンはに`fromPerson`含まれています。

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. 送信する`Notification.MessagingStyle.Message`イメージを含むを作成し、そのイメージを新しい[Notification. messagingstyle. Message. SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri)メソッドに渡します。
   例:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. メッセージを`Notification.MessagingStyle`オブジェクトに追加します。 例:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. このスタイルを通知ビルダーにプラグインします。 例:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. 通知を発行します。 例:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

通知の作成の詳細については、「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」を参照してください。

### <a name="indoor-positioning"></a>室内ポジショニング

Android の円は、IEEE 802.11 mc ( _WiFi ラウンドトリップ時間_または_wifi RTT_とも呼ばれます) をサポートします。これにより、アプリは1つ以上の wi-fi アクセスポイントへの距離を検出できるようになります。 この情報を使用すると、アプリでは 1 ~ 2 メートルの精度で*室内ポジショニング*を利用することができます。 IEEE 801.11 mc のハードウェアサポートを提供する Android デバイスでは、アプリは、スマートアプライアンスの場所ベースの制御やストアによるターンスルーの指示などのナビゲーション機能を提供できます。

[![WiFi RTT を使用した室内ナビゲーションの例](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

新しい[WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager)クラスといくつかのヘルパークラスは、wi-fi デバイスへの距離を測定するための手段を提供します。 Android P で導入された屋内配置 Api の詳細については、「 [android .net. wi-fi](https://developer.android.com/reference/android/net/wifi/rtt/package-summary)」を参照してください。

### <a name="multi-camera-support"></a>マルチカメラのサポート

多くの新しい Android デバイスには、ステレオビジョン、視覚効果の向上、ズーム機能の向上などの機能に役立つデュアルフロントエンドまたはデュアルバックカメラが搭載されています。 Android P には新しい[マルチカメラ](https://developer.android.com/about/versions/pie/android-9.0#camera)API が導入されています。これにより、アプリで2つ以上の物理カメラによって支えられた*論理カメラ*(または*論理マルチカメラ*) を使用できるようになります。
デバイスで論理マルチカメラがサポートされているかどうかを判断するには、デバイス上の各カメラの機能を見て、 [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA)がサポートされているかどうかを確認します。

また、Android の円グラフには、初期キャプチャ中の遅延を減らし、カメラストリームを開始および開始する必要がなくなるために使用できる新しい[SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html)クラスも含まれています。

Android P でのマルチカメラのサポートの詳細については、「[マルチカメラのサポートとカメラの更新プログラム](https://developer.android.com/about/versions/pie/android-9.0#camera)」を参照してください。

### <a name="other-features"></a>その他の機能

さらに、Android の円グラフでは、他のいくつかの新機能がサポートされています。

- 新しい[AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html)クラス。アニメーション化されたイメージを描画および表示するために使用できます。

- `BitmapFactory` を置き換える新しい [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) クラス。 `ImageDecoder`をデコード`AnimatedImageDrawable`するために使用できます。

- HDR (High Dynamic Range) video および HEIF (高効率のイメージファイル形式) イメージのサポート。

- [Jobscheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)は、ネットワーク関連のジョブをよりインテリジェントに処理するように強化されています。 [Jobparameters](https://developer.android.com/reference/android/app/job/JobParameters)クラスの新しい[getnetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29)メソッドは、特定のジョブに対してネットワーク要求を実行するのに最適なネットワークを返します。

Android の最新の円機能の詳細については、「 [android 9 の機能と api](https://developer.android.com/about/versions/pie/android-9.0)」を参照してください。

## <a name="behavior-changes"></a>動作の変更

ターゲットの Android バージョンが API レベル28に設定されている場合、上で説明した新機能を実装していない場合でも、アプリの動作に影響を与える可能性があるプラットフォームの変更がいくつかあります。 これらの変更の簡単な概要を次に示します。

- フォアグラウンドサービスを使用する前に、アプリでフォアグラウンドアクセス許可を要求できるようになりました。

- アプリに複数のプロセスがある場合、プロセス間で1つの[WebView](xref:Android.Webkit.WebView)データディレクトリを共有することはできません。

- 別のアプリのデータディレクトリにパスで直接アクセスすることはできなくなりました。

Android P を対象とするアプリの動作変更の詳細については、「[動作の変更](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps)」を参照してください。

## <a name="sample-code"></a>サンプル コード

[Androidpminidemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo)は、android 用の Xamarin サンプルアプリです。これは、表示のカットアウトモードを設定する方法、新しい`Person`クラスを使用する方法、およびイメージを含む通知を送信する方法を示しています。

## <a name="summary"></a>まとめ

この記事では、Android の円グラフについて説明しました。 android で Android を使用して開発するための最新のツールとパッケージをインストールして構成する方法について説明します。 Android の円グラフで使用できる主な機能の概要について説明しました。これらの機能のいくつかについては、ソースコード例を参照してください。
Android 用アプリの作成を開始する際に役立つ API ドキュメントおよび Android 開発者向けのトピックへのリンクが含まれています。 また、既存のアプリに影響する可能性がある、最も重要な Android の円の動作変更も強調表示されています。

## <a name="related-links"></a>関連リンク

- [Android 9 円](https://developer.android.com/about/versions/pie/)
