---
title: Android 9 Pie
description: Xamarin.Android を使用して Android の 9 円用アプリの開発を開始する方法。
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: cd1c374fa68420e1923ef4dee0bb37a4665f3535
ms.sourcegitcommit: 215cad17324ba3fbc23487ce66cd4e1cc74eb879
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710024"
---
# <a name="android-pie-features"></a>Android の円グラフの機能

_Xamarin.Android を使用して Android の 9 円用アプリの開発を開始する方法。_

[Android の 9 円](https://developer.android.com/about/versions/pie/)Google から提供が開始されました。 さまざまな新機能と Api が公開されて、このリリースでは、これらの多くは、最新の Android デバイスで、ハードウェアの新機能を利用するために必要です。

![Android 円ヒーローのイメージ](pie-images/01-android-p-logo.png)

この記事では、Android の円の Xamarin.Android アプリの開発を開始するために構成されます。 これには、必要な更新プログラムをインストール、SDK を構成およびエミュレーターまたはデバイスのテスト用に準備する方法について説明します。 Android の円の新機能の概要を説明し、主な Android の円グラフ機能の一部を使用する方法を示しています。 ソース コードの例を提供します。

Xamarin.Android 9.0 では、Android の円のサポートを提供します。 Android の円の Xamarin.Android のサポートの詳細については、次を参照してください。、 [Android P Developer Preview 3](https://developer.xamarin.com/releases/android/xamarin.android_9/xamarin.android_9.0/#android-p-dp1)リリース ノート。

## <a name="requirements"></a>必要条件

Android の円の機能を Xamarin ベースのアプリで使用する次の一覧が必要です。

-   **Visual Studio** &ndash; Windows を使用している場合は、Visual Studio 2017 15.8 またはそれ以降のバージョンに更新します。 Mac を使用している場合は、Mac 7.6 またはそれ以降のバージョンの Visual Studio 2017 を更新します。

-   **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 以降、Visual Studio をインストールする必要があります (Xamarin.Android がの一部として自動的にインストールされている、 **.NET によるモバイル開発**ワークロード)。

-   **Java Developer Kit** &ndash; Xamarin Android 9.0 開発が必要です[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (または、Microsoft の配布のプレビューを試すことができます、 [OpenJDK](~/android/get-started/installation/openjdk.md))。 JDK8 がの一部として自動的にインストールされている、 **.NET によるモバイル開発**ワークロード。

-   **Android SDK** &ndash; 28 またはそれ以降の Android SDK API が、Android SDK Manager を使用してをインストールする必要があります。

## <a name="getting-started"></a>作業の開始

Xamarin.Android で円グラフの Android アプリを開発して開始するには、ダウンロードし、最初の円を Android プロジェクトを作成する前に、最新のツールと SDK パッケージをインストールする必要があります。

1. 更新する[Visual Studio 2017 バージョン 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes)またはそれ以降。 Visual Studio for Mac を使用している場合に更新[Visual Studio 2017 for Mac バージョン 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes)またはそれ以降。

2. インストール**Android 円 (API 28)** パッケージとツール、SDK Manager を使用しています。

3. 対象とする新しい Xamarin.Android プロジェクトの作成**Android 9.0**します。

4. エミュレーターまたは円の Android アプリのテスト用のデバイスを構成します。

次のセクションでは、これらの各手順について説明します。


### <a name="update-visual-studio"></a>Visual Studio 2017 を更新する

Visual Studio には、Android の円のサポートを追加するには、Visual Studio 2017 15.8 またはそれ以降のバージョンに更新 (手順については、次を参照してください。[を最新のリリースの Visual Studio 2017 の更新](https://docs.microsoft.com/visualstudio/install/update-visual-studio))。

For Mac に Visual Studio を Android 円のサポートを追加するには、Visual Studio 2017 for Mac 7.6 以降に更新 (手順については、次を参照してください。[セットアップとインストールの Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation))。


### <a name="install-the-android-sdk"></a>Android SDK をインストールします。

Xamarin.Android 9.0、プロジェクトを作成する必要があります最初に使用する Android SDK Manager の SDK プラットフォームをインストールする**Android 円 (API レベル 28)** またはそれ以降。

1. SDK マネージャーを起動します。 Visual Studio で、次のようにクリックします。**ツール > Android > Android SDK Manager**します。 Visual studio for Mac では、次のようにクリックします。**ツール > SDK Manager**します。

2. 、右下隅で歯車アイコンをクリックし、選択**リポジトリ > Google (サポートされていません)**:

    [![Google にリポジトリを設定します。](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. インストール、 **Android 円**としてリストされている SDK パッケージ**Android SDK プラットフォーム 28**で、**プラットフォーム** タブ (詳細については、SDK Manager を使用して、参照してください[Android SDK セットアップ](~/android/get-started/installation/android-sdk.md))。

    [![Android の円のパッケージをインストールします。](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. エミュレーターを使用している場合をサポートする仮想デバイスを作成します。 **API レベル 28**します。 仮想デバイスの作成の詳細については、次を参照してください。[仮想デバイスの管理が Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)します。



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android プロジェクトを開始します。

新しい Xamarin.Android プロジェクトを作成します。 Xamarin で Android の開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md)を Xamarin.Android プロジェクトを作成する方法について説明します。

Android プロジェクトを作成するときに、ターゲット Android バージョン 9.0 以降にバージョン設定を構成する必要があります。 たとえば、Android の円のプロジェクトを対象にする必要がありますを構成するにプロジェクトのターゲットの Android API レベル**Android 9.0** (API 28)。 設定することも、ターゲット フレームワーク レベル API 28 以降をお勧めします。 Android API レベルを構成する方法の詳細は、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。


### <a name="configure-a-device-or-emulator"></a>デバイスまたはエミュレーターを構成します。

Nexus やピクセルなどの物理デバイスを使用している場合はデバイスを更新する Android の円を次の手順で[Nexus とピクセル デバイスの工場出荷時イメージ](https://developers.google.com/android/images)します。

エミュレーターを使用している場合は、API レベル 28 仮想デバイスを作成し、x86 ベースのイメージを選択します。 Android Device Manager を使用して作成し、仮想デバイスの管理方法の詳細については、次を参照してください。[仮想デバイスの管理が Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)します。
テストとデバッグ用の Android エミュレーターの使用方法の詳細については、次を参照してください。 [Android エミュレーターでデバッグする](~/android/deploy-test/debugging/debug-on-emulator.md)します。



## <a name="new-features"></a>新機能

Android の円グラフには、さまざまな新しい機能が導入されています。 これらの新機能の一部にさらに、Android ユーザー エクスペリエンスを強化するためにデザインされたものは、最新の Android デバイスによって提供される新しいハードウェアの機能を活用するためのもの。

-   **素材のサポートを表示**&ndash;の形状と場所を検索するは、Api、_素材_以降の Android デバイスの画面の上部にあります。

-   **通知の機能強化**&ndash;イメージ、および新しい通知メッセージを表示できますようになりました`Person`クラスは、メッセージ交換の参加者を簡略化に使用します。

-   **室内配置**&ndash;プラットフォーム プロトコルのサポート、WiFi Round Trip 時、WiFi デバイスに屋内設定ナビゲーションを使用するアプリのことができます。

-   **複数のカメラ サポート** &ndash; (デュアル前面とデュアル背面カメラ) など複数の物理的な cameras から同時にアクセス ストリームに機能を提供します。


次のセクションでは、これらの機能を強調表示しするための簡単なコード例は、アプリでの使用開始を指定します。

### <a name="display-cutout-support"></a>素材のサポートを表示します。

エッジ ツー エッジ画面に多数の新しい Android デバイスが、*素材の表示*(または「ノッチ」) の表示をカメラ、スピーカーの上部にあります。
次のスクリーン ショットでは、素材のエミュレーター例を示します。

[![Android エミュレーターの素材をシミュレートします。](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

表示素材を備えたデバイスでアプリのウィンドウの内容を表示する方法を管理する Android の円は、新しい追加が[LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode)ウィンドウ レイアウトの属性。 この属性は、次の値のいずれかに設定できます。

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash;ウィンドウ素材領域と重複は許可されません。

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash;素材領域には、画面の短いエッジにのみを拡張する、ウィンドウが許可されています。 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash;システム バー内で、素材が含まれている場合は、素材の領域に拡張する、ウィンドウが許可されています。

たとえば、素材の領域に重ならないようにするには、アプリ ウィンドウ レイアウト素材モードを設定*決して*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

次の例では、これらの素材のモードの例を示します。 左側の最初のスクリーン ショットでは、非全画面表示モードでのアプリです。 Center スクリーン ショットは、アプリが全画面表示で`LayoutInDisplayCutoutMode`設定`LayoutInDisplayCutoutModeShortEdges`します。 素材の表示領域に、アプリの背景が白が拡張されていることを確認します。

[![例は、エミュレーターで素材モードを表示します。](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

最終的なスクリーン ショットでは (上、右上)、`LayoutInDisplayCutoutMode`に設定されている`LayoutInDisplayCutoutModeShortNever`全画面表示に移動する前にします。
素材の表示領域に拡張するアプリの背景が白が許可されないことに注意してください。

デバイスで、素材の領域に関する詳細情報が必要な場合、新しい使える[DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html)クラス。 `DisplayCutout` コンテンツを表示するために使用できない表示の領域を表します。 この情報を使用すると、アプリがこの非機能的な領域の内容を表示しないように、素材の形状と場所を取得します。

Android の P で素材の新機能についての詳細については、次を参照してください。[ディスプレイ素材サポート](https://developer.android.com/about/versions/pie/android-9.0#cutout)します。



### <a name="notifications-enhancements"></a>通知の機能強化

Android の円グラフには、メッセージ エクスペリエンスを向上させるために、次の機能強化が導入されています。

-   通知チャネル (で導入された[Android Oreo](~/android/platform/oreo.md)) チャネルのグループのブロックしているようになりました。

-   通知システム 3 カテゴリがあります新しい Do Not Disturb (アラームのシステム サウンドとメディア ソース優先順位を付ける)。 さらは 7 つの新しい Do Not Disturb モード (バッジ、通知のライト、ステータス バーの外観、および全画面表示のアクティビティの起動) などのビジュアルの中断を抑制するために使用できます。

-   新しい[Person](https://developer.android.com/reference/android/app/Person.html)をメッセージの送信者を表すクラスが追加されました。 このクラスの使用は、(自分のアバターと Uri を含む) メッセージ交換にかかわる人を識別することによって、各通知のレンダリングを最適化するために役立ちます。

-   通知は、画像を表示できるようになりました。 

次の例では、新しい Api を使用してイメージを含む通知を生成する方法を示します。 次のスクリーン ショットにテキスト通知が投稿され、を埋め込みイメージでの通知が続きます。 (右側に表示される) と、通知が展開されます、最初の通知のテキストが表示され、イメージに埋め込まれている 2 つ目の通知が拡大します。

[![イメージの例の通知](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

次の例では、Android の円の通知では、画像を含める方法と、新しいの使用方法を示します`Person`クラス。

1. 作成、`Person`送信者を表すオブジェクト。 送信者の名前とアイコンが含まれる`fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. 作成、`Notification.MessagingStyle.Message`を新しいイメージを渡すことを送信するイメージを含む[Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri)メソッド。
   例:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. メッセージを追加、`Notification.MessagingStyle`オブジェクト。 例:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. 通知のビルダーには、このスタイルを接続します。 例:

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

通知の作成の詳細については、次を参照してください。[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)します。


### <a name="indoor-positioning"></a>室内の配置

Android の円は、IEEE 802.11mc のサポートを提供します (とも呼ばれます_WiFi Round-Trip 時_または_WiFi RTT_) との距離をいずれかを検出するためにアプリをできるように、または、複数の Wi-fi アクセス ポイントします。 この情報を使用することが活用するために、アプリの*室内配置*1 ~ 2 のメーターの精度でします。 IEEE 801.11mc のハードウェアのサポートを提供する Android デバイスでアプリがスマート アプライアンスまたはターンで有効にする手順については、店舗の場所ベースのコントロールなどのナビゲーション機能を提供できます。

[![WiFi RTT を使用して、屋内ナビゲーションの例](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

新しい[WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager)クラスといくつかのヘルパー クラスは、Wi-fi デバイスまでの距離を測定する手段を提供します。 Android の P で導入されたに屋内配置 Api に関する詳細については、次を参照してください。 [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary)します。


### <a name="multi-camera-support"></a>複数のカメラのサポート

多くの新しい Android デバイスでは、ステレオ ビジョン、強化された視覚効果、および向上のズーム機能などの機能の有用なデュアル前面またはデュアル背面カメラがあります。 Android P が導入されていますが、新しい[マルチ カメラ](https://developer.android.com/about/versions/pie/android-9.0#camera)API アプリが使用できるようにする、*論理カメラ*(または*論理マルチ カメラ*) は、2 つ以上でサポートします。物理的な cameras します。
デバイスは、論理マルチ カメラをサポートする場合にサポートされているか、デバイスで各カメラの機能を表示できるかを判断する[RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA)します。

Android の円も含まれています、新しい[セッション構成](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html)初期キャプチャ中に遅延を減らすことやを起動し、カメラのストリームを開始する必要性を排除するために使用できるクラス。

複数のカメラの詳細については、Android P でサポートを参照してください[マルチ カメラのサポートとカメラの更新プログラム](https://developer.android.com/about/versions/pie/android-9.0#camera)します。


### <a name="other-features"></a>その他の機能

さらに、Android の円グラフには、その他のいくつかの新機能がサポートされています。

-   新しい[AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html)クラスは、描画、およびアニメーション化されたイメージを表示するために使用できます。

-   新しい[ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html)クラスを置き換える`BitmapFactory`します。 `ImageDecoder` デコードに使用できる、`AnimatedImageDrawable`します。

-   HDR (ハイ ダイナミック レンジ) ビデオおよび HEIF (高効率イメージ ファイルの形式) のイメージをサポートします。

-   [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)はネットワーク関連の仕事をよりインテリジェントに処理するために強化されています。 新しい[GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29)のメソッド、 [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters)クラスは、特定のジョブをすべてのネットワーク要求を実行するための最適なネットワークを返します。

Android の円の最新の機能の詳細については、次を参照してください。 [Android 9 の機能と Api](https://developer.android.com/about/versions/pie/android-9.0)します。


## <a name="behavior-changes"></a>動作の変更

ターゲット Android バージョンは、API レベル 28 に設定されているときに上記で説明した新機能を実装していない場合でも、アプリの動作に影響するいくつかのプラットフォームの変更点があります。 次に、これらの変更の簡単な概要を示します。

-  アプリは、フォア グラウンド サービスを使用する前にフォア グラウンドのアクセス許可を要求する必要があります。

-  1 つを共有できない場合は、アプリは、1 つ以上のプロセスがある、 [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/)プロセス間でのデータ ディレクトリ。

-  直接パスで別のアプリのデータ ディレクトリへのアクセスはもう行えません。

Android の P を対象とするアプリの動作の変更の詳細については、次を参照してください。[動作の変更](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps)します。


## <a name="sample-code"></a>サンプル コード

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo)素材の表示モードを設定する方法については、Android 円の Xamarin.Android サンプル アプリは、新たに使用する方法`Person`クラス、およびイメージを含む通知を送信する方法。


## <a name="summary"></a>まとめ

この記事では、Android の円を導入し、インストールし、Android の円で最新のツールと Xamarin.Android の開発用のパッケージを構成する方法について説明します。 これらの機能のいくつかのソース コードの例での Android の円グラフで使用できる主な機能の概要が提供されています。
API のドキュメントへのリンクが含まれているし、円の Android のアプリの作成に役立つ Android 開発者向けのトピックを開始します。 既存のアプリに影響する最も重要な Android 円動作の変更も強調表示されます。


## <a name="related-links"></a>関連リンク

- [Android の 9 円](https://developer.android.com/about/versions/pie/)
