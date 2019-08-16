---
title: ロリポップ機能
description: この記事では、Android 5.0 (ロリポップ) で導入された新機能の概要について説明します。 これらの機能には、素材のテーマと呼ばれる新しいユーザーインターフェイスのスタイルと、アニメーション、ビューの影、および描画された色合いなどの新しいサポート機能が含まれています。 Android 5.0 には、強化された通知、2つの新しい UI ウィジェット、新しいジョブスケジューラ、およびストレージ、ネットワーク、接続、およびマルチメディア機能を向上させるためのいくつかの新しい Api が含まれています。
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7cc7eeabebbfbcd4b264202235307638b88842d8
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524213"
---
# <a name="lollipop-features"></a>ロリポップ機能

_この記事では、Android 5.0 (ロリポップ) で導入された新機能の概要について説明します。これらの機能には、素材のテーマと呼ばれる新しいユーザーインターフェイスのスタイルと、アニメーション、ビューの影、および描画された色合いなどの新しいサポート機能が含まれています。Android 5.0 には、強化された通知、2つの新しい UI ウィジェット、新しいジョブスケジューラ、およびストレージ、ネットワーク、接続、およびマルチメディア機能を向上させるためのいくつかの新しい Api が含まれています。_

## <a name="lollipop-overview"></a>ロリポップの概要

Android 5.0 (ロリポップ) では、新しいデザイン言語と*素材設計*が導入されており、これにより、アプリをより使いやすく、直感的に使用できるようにする新機能をサポートすることができます。 マテリアル設計では、android 5.0 では、Android フォンが facを提供するだけではありません。また、Android ベースのタブレット、デスクトップコンピューター、監視、およびスマートテレビ用の設計規則の新しいセットも提供します。 これらの設計規則は、ユーザーがインターフェイスをすばやく理解できるように、使い慣れた tactile 属性 (たとえば、現実的なサーフェイスやエッジキューなど) を使用して、わかりやすさと minimalism を重視しています。

*マテリアルテーマ*は、Android でのこれらの UI デザイン原則の具現化です。 この記事では、まず、素材のテーマのサポート機能について説明します。

- **アニメーション**タッチフィードバックアニメーション、アクティビティ遷移アニメーション、ビューステート遷移アニメーション、および表示効果。 &ndash;

- **影と仰角を表示**するビューにプロパティが`elevation`含まれるようになりました。 &ndash;  値が大きい`elevation`ビューでは、背景に大きな影がキャストされます。

- **色の特徴**描画の濃淡を使用すると、色を変更することで画像資産を再利用できるようになります。また、*色の抽出*を使用すると、画像の色に基づいてアプリのテーマを動的に設定できます。 &ndash;

多くのマテリアルテーマ機能は、既に Android 5.0 UI エクスペリエンスに組み込まれていますが、他の機能はアプリに明示的に追加する必要があります。 たとえば、一部の標準ビュー (ボタンなど) には既にタッチフィードバックアニメーションが含まれていますが、アプリではほとんどのビューシャドウを有効にする必要があります。

Android 5.0 には、マテリアルテーマを通じて行われる UI の機能強化に加えて、この記事で説明されている他のいくつかの新機能も含まれています。

- **強化**された通知Android 5.0 の通知は、新しい外観、ロック画面の通知のサポート、新しい*ヘッドアップ*通知プレゼンテーション形式で大幅に更新されました。 &ndash;

- **新しい UI ウィジェット**新しいウィジェットを使用すると、アプリが大きなデータセットや複雑な情報を簡単に`CardView`伝達できるようになります。新しいウィジェットには、テキストや画像を表示するための簡単なカード形式のプレゼンテーション形式が用意されています。 `RecyclerView` &ndash;

- **新しい api**&ndash; Android 5.0 では、複数のネットワークサポートのための新しい api の追加、Bluetooth 接続の向上、記憶域の管理の簡素化、マルチメディアプレーヤーとカメラデバイスの柔軟な制御が可能になりました。 スケジュールされた時刻にタスクを非同期に実行するために、新しいジョブスケジュール機能を使用できます。 この機能を使用すると、バッテリの寿命を向上させることができます。たとえば、デバイスが電源に接続され、充電されたときに実行されるスケジュールタスクなどです。


## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android 5.0 機能を使用するには、次のものが必要です。

- **Xamarin android** &ndash; 4.20 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。 

- **Android SDK**&ndash; Android SDK Manager を使用して、Android 5.0 (API 21) 以降をインストールする必要があります。

- **Java Developer Kit**API レベル24以上を開発している場合は、Xamarin Android には[jdk 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降が必要です (jdk 1.8 では、ロリポップを含む、24より前の api レベルもサポートされています)。 &ndash; カスタムコントロールまたはフォームプレビューアーを使用する場合は、64ビットバージョンの JDK 1.8 が必要です。

特に API レベル23以前を開発している場合は、 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)を使用し続けることができます。


## <a name="setting-up-an-android-50-project"></a>Android 5.0 プロジェクトの設定

Android 5.0 プロジェクトを作成するには、最新のツールと SDK パッケージをインストールする必要があります。 Android 5.0 を対象とする Xamarin Android プロジェクトを設定するには、次の手順に従います。

1. Xamarin Android ツールをインストールし、Xamarin ライセンスをアクティブ化します。 Xamarin のインストールの詳細については[、「セットアップとインストール](~/android/get-started/installation/index.md)」を参照してください。

2. Visual Studio for Mac を使用している場合は、最新の Android 5.0 更新プログラムをインストールします。

3. Android SDK マネージャーを起動し (Visual Studio for Mac で**ツール&gt; &hellip;** を使用して Android SDK マネージャーを開きます)、Android SDK Tools 23.0.5 以降をインストールします。

    [![Android SDK マネージャーで Android SDK ツールを選択する](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   また、最新の Android 5.0 SDK パッケージ (API 21 以降) をインストールします。

    [![Android SDK Manager での Android 5.0 SDK パッケージのインストール](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK Manager の使用方法の詳細については、「 [SDK manager](https://developer.android.com/tools/help/sdk-manager.html)」を参照してください。

4. 新しい Xamarin. Android プロジェクトを作成します。 Xamarin を使用した Android 開発を初めて使用する場合は、「 [Hello, android](~/android/get-started/hello-android/index.md) 」を参照して、android プロジェクトの作成について学習してください。 Android プロジェクトを作成するときは、必ず Android 5.0 のバージョン設定を構成してください。
   Visual Studio for Mac で、 **[プロジェクトオプション&gt; ] [ &gt;ビルド全般**] に移動し、 **[ターゲットフレームワーク]** を**Android 5.0 (ロリポップ)** 以降に設定します。

    ![ターゲット Framwework を Android 5.0 ロリポップに設定する](lollipop-images/target-framework.png)

   [**プロジェクトオプション&gt; ] &gt;の [android アプリケーションのビルド**] で、[最小] と [ターゲットの android バージョン] を [自動] に設定します。**ターゲットフレームワークのバージョンを使用**します。

    ![最小およびターゲットの Android バージョンを自動に設定する](lollipop-images/minimum-android-version.png)

5. アプリをテストするエミュレーターまたは Android デバイスを構成します。 エミュレーターを使用している場合は、Xamarin Studio または Visual Studio で使用する Android エミュレーターを構成する方法については、「 [Android Emulator セットアップ](~/android/get-started/installation/android-emulator/index.md)」を参照してください。 Android デバイスを使用している場合は、Android 5.0 用にデバイスを更新する方法については、「 [PREVIEW SDK の設定](https://developer.android.com/preview/setup-sdk.html)」を参照してください。 Xamarin Android アプリケーションを実行およびデバッグするように Android デバイスを構成する方法については、「[開発用にデバイスを設定](~/android/get-started/installation/set-up-device-for-development.md)する」を参照してください。

メモ:Android L Preview を対象としていた既存の Android プロジェクトを更新する場合は、**ターゲットフレームワーク**と**android のバージョン**を、前述の値に更新する必要があります。

## <a name="important-changes"></a>重要な変更

以前に発行された Android アプリは、Android 5.0 の変更の影響を受ける可能性があります。 特に、Android 5.0 では、新しいランタイムと大幅に変更された通知形式が使用されます。

### <a name="android-runtime"></a>Android ランタイム

Android 5.0 では、Dalvik ではなく、既定のランタイムとして新しい Android ランタイム (アート) が使用されます。 アートは、いくつかの主要な新機能を実装しています。

- **事前 (AOT) のコンパイル**&ndash; AOT では、アプリを初めて起動する前にアプリコードをコンパイルすることで、アプリのパフォーマンスを向上させることができます。 アプリがインストールされると、アートはターゲットデバイス用のコンパイル済みアプリの実行可能ファイルを生成します。

- 改善された**ガベージコレクション (GC)** &ndash;また、GC の強化により、アプリのパフォーマンスが向上します。 ガベージコレクションでは、2つではなく1つの GC 一時停止が使用されるようになり、同時実行の GC 操作がよりタイムリーな方法で完了しました。

- **強化**されたアプリのデバッグ&ndash;アートは、例外やクラッシュレポートの分析に役立つ詳細な診断情報を提供します。

既存のアプリは、アート下で&ndash;は動作しない可能性がある前の Dalvik ランタイムに固有の手法を利用するアプリを除き、アートの下では変更なしで動作します。 これらの変更の詳細については、「 [Android ランタイムでのアプリの動作の検証 (アート)](https://developer.android.com/guide/practices/verifying-apps-art.html)」を参照してください。


### <a name="notification-changes"></a>通知の変更

Android 5.0 で通知が大幅に変更されました。

- **サウンドと振動の処理方法が異なり**ます。`MediaPlayer` `Notification.Builder` `Ringtone`通知サウンドと vibrations は、、、および`Vibrator`ではなく、によって処理されるようになりました。 &ndash;

- **新しい配色**&ndash;マテリアルテーマに従って、通知は、白い背景または非常に明るい背景の濃いテキストでレンダリングされます。 また、通知アイコン内のアルファチャネルは、Android によってシステムカラースキームと調整される場合があります。 

- **ロック**画面の通知&ndash;デバイスのロック画面に通知が表示されるようになりました。

- **ヘッドアップ**&ndash;優先度の高い通知は、デバイスのロックが解除され、画面がオンになっているときに小さなフローティングウィンドウ (ヘッドアップ通知) に表示されるようになりました。

ほとんどの場合、既存のアプリ通知機能を Android 5.0 に移植するには、次の手順を実行する必要があります。

1. 通知を作成するため`Notification.Builder`に ( `NotificationsCompat.Builder`または) を使用するようにコードを変換します。 

2. 新しいマテリアルテーマの配色で、既存の通知資産が表示されていることを確認します。

3. 通知をロック画面に表示するときに表示する可視性を決定します。 通知がパブリックでない場合は、どのようなコンテンツがロック画面に表示されますか。

4. 新しい Android 5.0 の "*応答なし*" モードで正しく処理されるように、通知のカテゴリを設定します。

通知にトランスポートコントロールがある場合、メディアの再生状態を`RemoteControlClient`表示する場合`ActivityManager.GetRecentTasks`、使用する場合、または呼び出しを行う場合は、Android 5.0 の通知の更新の詳細について、「[重要な動作の変更](https://developer.android.com/preview/api-overview.html#Behaviors)」を参照してください。

Android での通知の作成の詳細については、「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」を参照してください。

## <a name="material-theme"></a>マテリアル テーマ

新しい Android 5.0 のマテリアルテーマにより、Android UI のルックアンドフィールが変化します。 ビジュアル要素は、印刷ベースのデザインの太字のグラフィックス、文字体裁、および明るい色で使用される tactile サーフェイスを使用するようになりました。 素材のテーマの例を次のスクリーンショットに示します。

[![マテリアルテーマのホーム画面、アプリの画面、および設定画面のスクリーンショット](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 では、左側に表示されるホーム画面が greets ます。 中央のスクリーンショットはアプリの一覧の最初の画面で、右側のスクリーンショットは **[設定]** 画面です。 Google の[マテリアル設計](https://material.io/guidelines/material-design/introduction.html)仕様では、新しいマテリアルテーマの概念の背後にある基になる設計規則について説明します。

マテリアルテーマには、 `Theme.Material`アプリで使用できる3つの組み込みのフレーバーが含まれています。ダークテーマ (既定) `Theme.Material.Light` 、テーマ`Theme.Material.Light.DarkActionBar` 、およびテーマです。 

[![ダーク、ライト、DarkActionBar テーマのスクリーンショット](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Xamarin Android アプリでのマテリアルテーマ機能の使用の詳細については、「[マテリアルテーマ](~/android/user-interface/material-theme.md)」を参照してください。


## <a name="animations"></a>Animations

Android 5.0 では、アプリインターフェイスを直感的に使用できるように、タッチフィードバックアニメーション、アクティビティ遷移アニメーション、およびビューステート遷移アニメーションが提供されています。 また、Android 5.0 アプリでは、[*効果*の表示] アニメーションを使用してビューを非表示にしたり表示したりできます。 *曲線のモーション*設定を使用して、すばやくまたは緩やかにアニメーションを表示する方法を構成できます。


### <a name="touch-feedback-animations"></a>タッチフィードバックのアニメーション

タッチフィードバックのアニメーションでは、ビューにタッチしたときに視覚的なフィードバックをユーザーに提供します。 たとえば、ボタンにタッチ&ndash;したときに ripple 効果が表示されるようになりました。これは、Android 5.0 の既定のタッチフィードバックアニメーションです。 Ripple アニメーションは、新しい`RippleDrawable`クラスによって実装されます。 Ripple 効果は、ビューの境界を終了するように構成することも、ビューの境界を越えて拡張することもできます。 たとえば、次の一連のスクリーンショットは、タッチアニメーション中のボタンの ripple 効果を示しています。

![ボタン上の ripple アニメーションのフレームスクリーンショットフレーム](lollipop-images/touch-animation.png)

ボタンを使用した最初のタッチコンタクトは、左側の最初のイメージで発生しますが、残りのシーケンス (左から右) は、ripple 効果がボタンの端にどのように拡散するかを示しています。 Ripple アニメーションが終了すると、ビューは元の外観に戻ります。 既定の ripple アニメーションは、1秒間に何度も実行されますが、アニメーションの長さは、時間を長くしたり短くしたりするようにカスタマイズできます。

Android 5.0 のタッチフィードバックアニメーションの詳細については、「[タッチフィードバックのカスタマイズ](https://developer.android.com/training/material/animations.html#Touch)」を参照してください。


### <a name="activity-transition-animations"></a>アクティビティ遷移のアニメーション

アクティビティ遷移のアニメーションを利用すると、あるアクティビティが別のアクティビティに遷移するときにビジュアルの継続性を把握できます。 アプリでは、次の3種類の移行アニメーションを指定できます。

- **移行の開始**&ndash;アクティビティがシーンに入るタイミング。

- **遷移の終了**&ndash;アクティビティがシーンを終了する場合。

- **共有要素の遷移**&ndash; 2 つのアクティビティに共通するビューが、最初のアクティビティが次に遷移するときに変更される場合は。

たとえば、次の一連のスクリーンショットは、共有要素の遷移を示しています。

[![共有要素の切り替えアニメーションのフレームスクリーンショット別フレーム](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

共有要素 (caterpillar の写真) は、最初のアクティビティのいくつかのビューの1つです。最初のアクティビティが2番目のアクティビティに遷移するときに、2番目のアクティビティの唯一のビューになるように拡大されます。

#### <a name="enter-transition-animation-types"></a>遷移アニメーションの種類を入力

Enter 遷移の場合、Android 5.0 では次の3種類のアニメーションが提供されます。

- **アニメーションの展開**&ndash;シーンの中心からビューを拡大します。

- **スライドアニメーション**&ndash;のビューをシーンのいずれかの端から移動します。

- **フェードアニメーション**&ndash;ビューをシーンにフェードします。

#### <a name="exit-transition-animation-types"></a>遷移のアニメーションの種類を終了する

終了遷移の場合、Android 5.0 では次の3種類のアニメーションが提供されます。

- **アニメーションの展開**&ndash;シーンの中央にビューを縮小します。

- **スライドアニメーション**&ndash;ビューをシーンの端の1つに移動します。

- **フェードアニメーション**&ndash;シーンからビューをフェードアウトします。

#### <a name="shared-element-transition-animation-types"></a>共有要素の遷移のアニメーションの種類

共有要素の遷移では、次のような複数の種類のアニメーションがサポートされるようになります。

- ビューのレイアウトまたはクリップの境界を変更します。

- ビューのスケールと回転を変更する。

- ビューのサイズとスケールの種類を変更します。

Android 5.0 でのアクティビティ遷移のアニメーションの詳細については、「[アクティビティ遷移のカスタマイズ](https://developer.android.com/training/material/animations.html#Transitions)」を参照してください。


### <a name="view-state-transition-animations"></a>状態遷移のアニメーションを表示する

Android 5.0 を使用すると、ビューの状態が変化したときにアニメーションを実行できます。 ビューステートの遷移は、次のいずれかの方法を使用してアニメーション化できます。

- 特定のビューに関連付けられている状態の変更をアニメーション化するために、drawables 作成します。 新しい`AnimatedStateListDrawable`クラスを使用すると、ビューステートの変更の間にアニメーションを表示する drawables 作成できます。

- ビューの状態が変化したときに実行されるアニメーション機能を定義します。 新しい`StateListAnimator`クラスを使用すると、ビューの状態が変化したときに実行されるアニメーターを定義できます。

Android 5.0 でのビューステート遷移のアニメーションの詳細については、「[ビューステートの変更をアニメーション化](https://developer.android.com/training/material/animations.html#ViewState)する」を参照してください。


### <a name="reveal-effect"></a>効果の表示

[*公開] 効果*は、ビューを表示または非表示にするために半径を変更するクリッピング円です。 この効果を制御するには、クリッピング円の最初と最後の半径を設定します。 次の一連のスクリーンショットは、画面の中央から効果を示すアニメーションを示しています。

[![アニメーションを表示するフレームごとのフレームスクリーンショット](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

次のシーケンスは、画面の左下隅から実行される効果アニメーションを示しています。

[![クリッピングアニメーションのフレームごとのフレームショット](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

アニメーションを反転させることができます。つまり、クリッピング円では、ビューを表示するために拡大するのではなく、ビューを非表示にすることができます。

での Android 5.0 の影響の詳細については、「[効果を使用する](https://developer.android.com/training/material/animations.html#Reveal)」を参照してください。


### <a name="curved-motion"></a>曲線の動き

これらのアニメーション機能に加えて、Android 5.0 では、アニメーションの時間とモーション曲線を指定できる新しい Api も提供されています。 Android 5.0 では、これらの曲線を使用して、アニメーション中のテンポラルと空間移動を補間します。 Android 5.0 では、3つの曲線が定義されています。

- **の\_Fastout\_線形\_は迅速**に加速し、アニメーションが終了するまで加速し続けます。&ndash;

- **速度が遅い\_のは、アニメーションの終わりに向かって迅速かつゆっくり減速を加速します。\_\_** &ndash;

- **で\_の\_線形出力の遅延\_** は、ピーク速度で始まり、アニメーションの終わりに減速ます。 &ndash;

新しい`PathInterpolator`クラスを使用して、モーション補間の実行方法を指定できます。 `PathInterpolator`は、指定された制御点とモーション曲線に従ってアニメーションのパスをトラバースする interpolator です。 Android 5.0 で曲線のモーション設定を指定する方法の詳細については、「[曲線モーションの使用](https://developer.android.com/training/material/animations.html#CurvedMotion)」を参照してください。


## <a name="view-shadows--elevation"></a>影 & 仰角を表示する

Android 5.0 では、新しい`Z`プロパティを設定して、ビューの*昇格*を指定できます。 値を`Z`大きくすると、ビューの背景に大きな影が表示され、背景の上の方が手前に表示されます。 ビューの初期昇格を設定するには、レイアウトの`elevation`属性を構成します。

次の例では、昇格属性が2dp、4dp、6dp にそれぞれ設定されている場合に、空`TextView`のコントロールによってキャストされる影を示しています。

[![Progessively の大きなビューの影のスクリーンショット](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

ビューの影の設定は静的にすることができます (上記のように)。または、アニメーションで使用して、ビューの背景の上に一時的に表示されるようにすることもできます。 `ViewPropertyAnimator`クラスを使用して、ビューの昇格をアニメーション化できます。 ビューの昇格とは、そのレイアウト`elevation`設定`translationZ`の合計と、メソッド呼び出しを`ViewPropertyAnimator`使用して設定できるプロパティのことです。

Android 5.0 のビューの影の詳細については、「[影とクリッピングビューの定義](https://developer.android.com/training/material/shadows-clipping.html)」を参照してください。


## <a name="color-features"></a>色の特徴

Android 5.0 では、アプリの色を管理するための2つの新機能が提供されています。

- 描画の*色合い*を使用すると、レイアウト属性を変更することによって、画像アセットの色を変更できます。

- *目立つ色の抽出*により、アプリの配色テーマを動的にカスタマイズして、表示されるイメージのカラーパレットと調整することができます。


### <a name="drawable-tinting"></a>描画の色合い

Android 5.0 のレイアウトでは`tint` 、異なる色を表示するために複数のバージョンのアセットを作成しなくても、drawables 色を設定するために使用できる新しい属性が認識されています。 この機能を使用するには、ビットマップをアルファマスクとして定義`tint`し、属性を使用して資産の色を定義します。 これにより、1回アセットを作成し、テーマに合わせてレイアウトに色を付けることができます。

次の例では、1つの&ndash;イメージアセットに透明な背景&ndash;を持つ白いロゴを使用して、濃淡のバリエーションを作成します。

![背景が透明な白い Xamarin ロゴ](lollipop-images/xamarin-logo-white.png)

このロゴは、次の例に示すように、青い円形の背景の上に表示されます。 左側の画像は、 `tint`設定なしでロゴがどのように表示されるかを示しています。 中央の画像では、ロゴの`tint`属性が濃い灰色に設定されています。 右側の画像では、 `tint`が淡い灰色に設定されています。

![濃淡の設定が異なる上記のロゴの例](lollipop-images/drawable-tinting.png)

Android 5.0 での描画の色合いの詳細については、「描画の[色合い](https://developer.android.com/training/material/drawables.html#DrawableTint)」を参照してください。


### <a name="prominent-color-extraction"></a>目立つ色の抽出

新しい Android 5.0 `Palette`クラスを使用すると、イメージから色を抽出して、カスタムカラーパレットに動的に適用できます。 クラス`Palette`は、イメージから6色を抽出し、色の鮮やかさと明るさの相対的なレベルに応じて、これらの色にラベルを付けます。

- 生き生き

- 濃いダーク

- 鮮やか光

- ミュート

- 色をミュート

- ミュートしたライト

たとえば、次のスクリーンショットでは、写真表示アプリが画面上のイメージから目立つ色を抽出し、これらの色を使用して、イメージに合わせてアプリの配色を調整しています。

[![緑、ピンク、青のテーマカラー抽出のスクリーンショット](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

上のスクリーンショットでは、操作バーは抽出された "鮮やかな光" の色に設定され、背景は "鮮やかなダーク" 色に設定されています。 上の各例では、イメージから抽出されたパレットの色を示すために、小さい色の正方形の行が含まれています。

Android 5.0 での色の抽出の詳細については、「[イメージからの目立つ色の抽出](https://developer.android.com/training/material/drawables.html#ColorExtract)」を参照してください。


## <a name="new-ui-widgets"></a>新しい UI ウィジェット

Android 5.0 では、2つの新しい UI ウィジェットが導入されています。

- `RecyclerView`&ndash;スクロール可能な項目の一覧を表示するビューグループ。

- `CardView`&ndash;角が丸い基本レイアウト。

どちらのウィジェットにも、マテリアルテーマ機能のサポートが組み込まれています。たとえば、で`RecyclerView`は、ビューを追加および削除するため`CardView`にアニメーションを使用し、各カードが背景の上にフローティングするようにビューの影を使用します。 これらの新しいウィジェットの例を次のスクリーンショットに示します。

[![RecyclerView でビルドされたアプリのスクリーンショット](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

左側のスクリーンショットは、電子メールアプリ`RecyclerView`で使用されるの例であり、右側のスクリーンショットは旅行予約アプリ`CardView`で使用されるの例です。


### <a name="recyclerview"></a>RecyclerView

`RecyclerView`はに`ListView,`似ていますが、動的に変更される要素を含む大きなビューやリストのセットに適しています。 基`ListView,`になるデータセットにアクセスするアダプターを指定するのと同様です。 ただし、で`ListView,` `RecyclerView`は、*レイアウトマネージャー*を使用してアイテムを配置します。 レイアウトマネージャーでも、ビューのリサイクルが行われます。ユーザーに表示されなくなった項目ビューの再利用を管理します。

`RecyclerView`ウィジェットを使用する場合は`LayoutManager` 、アダプターとアダプターを指定する必要があります。 この図に示すように`LayoutManager` 、はアダプターとの`RecyclerView`間の仲介です。

![LayoutManager、Adapter、データセットをサポートする RecyclerView の図](lollipop-images/recyclerview-diagram.png)

次のスクリーンショットは`RecyclerView` 、100項目を含むを示しています`ImageView` (各`TextView`項目はとで構成されています)。

[![画像をスクロールする RecyclerView アプリのスクリーンショット](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView`このサンプルアプリでは、リスト&ndash;の先頭からリストの最後まで簡単にスクロールすることで、この大規模なデータセットを処理します。これには数秒しかかかりません。 `RecyclerView`アニメーションもサポートします。実際、項目を追加および削除するためのアニメーションは既定で有効になっています。 に項目が追加されると`RecyclerView`、次の一連のスクリーンショットに示すようにフェードインされます。

[![フレームごとにフェードしているフォト項目のスクリーンショット](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

の詳細に`RecyclerView`ついては、「 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)」を参照してください。


### <a name="cardview"></a>CardView

`CardView`は、角が丸いフリーカードをシミュレートする単純なビューです。 に`CardView`は組み込みのビューの影が付いているため、アプリに視覚的な奥行を簡単に追加することができます。 次のスクリーンショットは、の3つの`CardView`テキスト指向の例を示しています。

[![CardView ベースの項目で RecyclerView を使用するアプリのスクリーンショットの例](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

上の例の各カードには`TextView`が含まれています。背景色は属性に`cardBackgroundColor`よって設定されます。

の詳細に`CardView`ついては、「 [CardView](~/android/user-interface/controls/card-view.md)」を参照してください。


## <a name="enhanced-notifications"></a>強化された通知

Android 5.0 の通知システムは、新しい視覚形式と新機能によって大幅に更新されました。 Android 5.0 では、通知の外観が新しくなりました。 たとえば、Android 5.0 の通知では、明るい背景に濃いテキストが使用されるようになりました。

![展開されていない Android 5.0 通知の例](lollipop-images/expanded-notification-contracted.png)

(上の例で示すように) 大きいアイコンが通知に表示されると、Android 5.0 は小さいアイコンをバッジとして大きいアイコンの上に表示します。 

Android 5.0 では、通知はデバイスのロック画面にも表示されます。
たとえば、次に示すのは、1つの通知を含むロック画面のスクリーンショットの例です。

[![ロック画面に表示される通知のスクリーンショット](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

ユーザーは、ロック画面で通知をダブルタップしてデバイスのロックを解除し、その通知を開始したアプリに移動するか、スワイプして通知を破棄することができます。 通知には、ロック画面に表示できるコンテンツの量を決定する新しい*可視性*の設定があります。 ユーザーは、ロック画面の通知に機微なコンテンツを表示するかどうかを選択できます。

Android 5.0 では、*ヘッドアップ*と呼ばれる優先順位の高い新しい通知プレゼンテーション形式が導入されています。 ヘッドアップ通知は、画面の上部から数秒後にスライドし、画面の上部にある通知網掛けに戻ります。 ヘッドアップ通知を使用すると、システム UI は、現在実行中のアクティビティを中断することなく、ユーザーの前に重要な情報を格納できます。 次の例は、アプリの上部に表示される簡単なヘッドアップ通知を示しています。

[![ヘッドアップ通知の例](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

ヘッドアップ通知は、通常、次のイベントに使用されます。

- 新しい次のメッセージ

- 着信通話

- バッテリ低下の通知

- アラーム

Android 5.0 では、優先順位の設定が高または最大の場合にのみ、ヘッドアップ形式で通知が表示されます。

Android 5.0 では、Android の並べ替えと表示をよりインテリジェントに行うための通知メタデータを提供できます。 Android 5.0 では、優先度、可視性、カテゴリに基づいて通知が整理されています。
通知カテゴリは、デバイスが [*応答不可*] モードのときに表示できる通知をフィルター処理するために使用されます。

最新の Android 5.0 機能を使用した通知の作成と起動の詳細については、「[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)」を参照してください。


## <a name="new-apis"></a>新しい API

Android 5.0 では、前述の新しいルックアンドフィール機能に加えて、既存のマルチメディア、ストレージ、およびワイヤレス/接続機能の機能を拡張する新しい Api が追加されています。 また、Android 5.0 には、新しいジョブスケジューラ機能をサポートする新しい Api が含まれています。

### <a name="camera"></a>カメラ

Android 5.0 では、カメラ機能を強化するための新しい Api がいくつか提供されています。 新しい`Android.Hardware.Camera2`名前空間には、Android デバイスに接続されている個々のカメラデバイスにアクセスするための機能が含まれています。 また、 `Android.Hardware.Camera2`は、各カメラデバイスをパイプラインとしてモデル化します。キャプチャ要求を受け取り、イメージをキャプチャして、結果を出力します。 この方法により、アプリはカメラデバイスに対して複数のキャプチャ要求をキューに取り込むことができます。

次の Api を使用すると、これらの新機能を実現できます。

- `CameraManager.GetCameraIdList`は、カメラデバイスにプログラムでアクセスするの`CameraManager.OpenCamera`に役立ちます。を使用して、特定のカメラデバイスに接続します。 &ndash;

- `CameraCaptureSession`&ndash;カメラデバイスからイメージをキャプチャまたはストリームします。 新しいイメージキャプチャ`CameraCaptureSession.CaptureListener`イベントを処理するためのインターフェイスを実装します。

- `CaptureRequest`&ndash;キャプチャパラメーターを定義します。

- `CaptureResult`&ndash;イメージキャプチャ操作の結果を提供します。

Android 5.0 の新しいカメラ Api の詳細については、「 [Media](https://developer.android.com/about/versions/android-5.0.html#Media)」を参照してください。

### <a name="audio-playback"></a>オーディオ再生

Android 5.0 では`AudioTrack` 、オーディオ再生を改善するためにクラスを更新します。

- `ENCODING_PCM_FLOAT`動的な範囲、ヘッドルーム、高品質を向上させるために、浮動小数点形式のオーディオデータを受け入れるようにを構成`AudioTrack`します (精度の向上による)。 &ndash; また、浮動小数点形式を使用すると、オーディオクリップを回避できます。

- `ByteBuffer`これで、のオーディオデータを`AudioTrack`バイト配列として指定できるようになりました。 &ndash;

- `WRITE_NON_BLOCKING`&ndash;このオプションを使用すると、一部のアプリのバッファリングとマルチスレッド処理が簡単になります。

Android 5.0 の`AudioTrack`機能強化の詳細については、「 [Media](https://developer.android.com/about/versions/android-5.0.html#Media)」を参照してください。

### <a name="media-playback-control"></a>メディア再生コントロール

Android 5.0 では、 `Android.Media.MediaController`に代わる`RemoteControlClient`新しいクラスが導入されています。 `Android.Media.MediaController`は、簡素化されたトランスポートコントロール Api を提供し、UI コンテキストの外部でのスレッドセーフな再生制御を提供します。 次の新しい Api は、トランスポート制御を処理します。

- `Android.Media.Session.MediaSession`&ndash;複数のコントローラーを処理するメディアコントロールセッション。 を呼び出し`MediaSession.GetSessionToken`て、アプリがセッションを操作するために使用するトークンを要求します。

- `MediaController.TransportControls`Play、 **Stop**、 **Skip**などのトランスポートコマンドを処理します。 &ndash;

また、新しい`Android.App.Notification.MediaStyle`クラスを使用して、メディアセッションと豊富な通知コンテンツ (アルバムアートの抽出や表示など) を関連付けることもできます。

Android 5.0 の新しいメディア再生コントロール機能の詳細については、「[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)」を参照してください。

### <a name="storage"></a>記憶域

Android 5.0 では、アプリケーションがディレクトリとドキュメントを簡単に操作できるように、ストレージアクセスフレームワークが更新されます。

- ディレクトリサブツリーを選択するには、 `Android.Intent.Action.OPEN_DOCUMENT_TREE`インテントをビルドして送信します。 この目的により、サブツリーの選択をサポートするすべてのプロバイダーインスタンスがシステムに表示されます。次に、ユーザーがディレクトリを参照して選択します。

- 新しいドキュメントまたはディレクトリをサブツリーの下に作成して管理するに`CreateDocument`は`RenameDocument`、の`DeleteDocument` `DocumentsContract`新しいメソッド、メソッド、およびメソッドを使用します。

- すべての共有記憶装置上のメディアディレクトリへのパスを取得するに`Android.Content.Context.GetExternalMediaDirs`は、新しいメソッドを呼び出します。

Android 5.0 の新しいストレージ Api の詳細については、「 [storage](https://developer.android.com/preview/api-overview.html#Storage)」を参照してください。

### <a name="wireless--connectivity"></a>ワイヤレス & 接続

Android 5.0 では、ワイヤレスおよび接続に対して次の API の機能強化が追加されています。

- 新しい*マルチネットワーク*api。接続を確立する前に、アプリが特定の機能を持つネットワークを検索して選択できるようにします。

- Android 5.0 デバイスが省エネルギー Bluetooth 周辺機器として動作できるようにする bluetooth 放送機能。

- NFC の機能強化により、データを他のデバイスと共有するために近距離通信機能を簡単に使用できるようになりました。

Android 5.0 での新しいワイヤレスおよび接続 Api の詳細については、「[ワイヤレスと接続](https://developer.android.com/preview/api-overview.html#Wireless)」を参照してください。

### <a name="job-scheduling"></a>ジョブのスケジュール設定

Android 5.0 では、 `JobScheduler`デバイスが接続されて充電されている場合にのみ実行するように特定のタスクをスケジュールすることで、ユーザーがバッテリの消耗を最小化するのに役立つ新しい API が導入されています。 このジョブスケジューラ機能は、デバイスが従量制課金ネットワークではなく Wi-fi ネットワーク経由で接続されている場合など、条件がそのタスクに適している場合に実行するタスクのスケジュール設定にも使用できます。

Android 5.0 の新しいジョブスケジュール Api の詳細については、「[ジョブのスケジュール設定](https://developer.android.com/preview/api-overview.html#JobScheduler)」を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin 用 Android 5.0 の重要な新機能の概要について説明しました。 Android アプリ開発者向け:

- マテリアル テーマ

- Animations

- 影と仰角を表示する

- 描画の色合いや目立つ色の抽出などの色の特徴

- `RecyclerView` 新しい`CardView`ウィジェット

- 通知の機能強化

- カメラ、オーディオ再生、メディアコントロール、ストレージ、ワイヤレス/接続、ジョブスケジュール用の新しい Api

Xamarin Android の開発を初めて使用する場合は、「[セットアップとインストール](~/android/get-started/installation/index.md)」を参照して、xamarin android の使用を開始することができます。
Android プロジェクトを作成する方法の詳細については[、「Hello」](~/android/get-started/hello-android/index.md)をご紹介します。



## <a name="related-links"></a>関連リンク

- [Android L Developer Preview](https://developer.android.com/preview/index.html)
- [Android SDK を取得する](https://developer.android.com/sdk/index.html#Other)
- [素材のデザイン](https://developer.android.com/preview/material/index.html)
- [素材デザインの原則](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
