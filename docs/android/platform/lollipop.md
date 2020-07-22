---
title: Lollipop 機能
description: この記事では、Android 5.0 (Lollipop) で導入された新機能の概要について説明します。 これらの機能には、マテリアル テーマと呼ばれる新しいユーザー インターフェイスのスタイルと、アニメーション、ビューのシャドウ、ドローアブルな色合いなどの新しいサポート機能が含まれています。 Android 5.0 には、強化された通知、2 つの新しい UI ウィジェット、新しいジョブ スケジューラ、およびストレージ、ネットワーク、接続、マルチメディアの機能を向上させるためのいくつかの新しい API が含まれています。
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 297c7806ce8a880d65c38ef0e4672e41fee5acfe
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724443"
---
# <a name="lollipop-features"></a>Lollipop 機能

_この記事では、Android 5.0 (Lollipop) で導入された新機能の概要について説明します。これらの機能には、マテリアル テーマと呼ばれる新しいユーザー インターフェイスのスタイルと、アニメーション、ビューのシャドウ、ドローアブルな色合いなどの新しいサポート機能が含まれています。Android 5.0 には、強化された通知、2 つの新しい UI ウィジェット、新しいジョブ スケジューラ、およびストレージ、ネットワーク、接続、マルチメディアの機能を向上させるためのいくつかの新しい API が含まれています。_

## <a name="lollipop-overview"></a>Lollipop の概要

Android 5.0 (Lollipop) には、新しいデザイン言語である *Material Design* と、アプリをより簡単かつ直感的に使用できるようにする新機能のサポート機能が導入されています。 Material Design では、Android 5.0 は Android フォンのモデルチェンジを行うだけではありません。Android ベースのタブレット、デスクトップ コンピューター、時計、スマート TV 用の新しい設計規則セットも提供します。 これらの設計規則は、ユーザーがインターフェイスをすばやく直感的に理解できるようになじみのある触覚属性 (リアルなサーフェスや端の手がかりなど) を利用しつつ、シンプルさとミニマリズムを強調しています。

*マテリアル テーマ* は、Android でのこれらの UI 設計原則を具現化したものです。 この記事では、まず、マテリアル テーマのサポート機能について説明します。

- **アニメーション** &ndash; "*タッチ フィードバック*" アニメーション、"*アクティビティ遷移*" のアニメーション、"*ビューの状態遷移*" のアニメーション、および "*出現エフェクト*"。

- **ビューのシャドウと高さ** &ndash; ビューが `elevation` プロパティを持つようになりました。`elevation` 値が大きいビューほど、背景に投じられる影が大きくなります。

- **色の特徴** &ndash; "*ドローアブルな色合い*" により、色を変更することでイメージ アセットを再利用できるようになり、"*目立つ色の抽出*" により、イメージ内の色に基づいてアプリのテーマを動的に設定できます。

多くのマテリアル テーマ機能は、既に Android 5.0 UI エクスペリエンスに組み込まれていますが、その他の機能はアプリに明示的に追加する必要があります。 たとえば、一部の標準ビュー (ボタンなど) には既にタッチ フィードバック アニメーションが含まれていますが、アプリではほとんどのビューのシャドウを有効にする必要があります。

Android 5.0 には、マテリアル テーマを通じてもたらされる UI の機能強化に加えて、この記事で説明されている他のいくつかの新機能も含まれています。

- **強化された通知** &ndash; Android 5.0 の通知は、新しい外観、ロック画面の通知のサポート、新しい "*ヘッズアップ*" 通知表示形式で大幅に更新されました。

- **新しい UI ウィジェット** &ndash; 新しい `RecyclerView` ウィジェットを使用すると、アプリで大きなデータ セットや複雑な情報を簡単に伝達できるようになります。また、新しい `CardView` ウィジェットには、テキストやイメージを表示するためのシンプルなカードのような表示形式が用意されています。

- **新しい API** &ndash; Android 5.0 では、複数のネットワーク サポート用に新しい API が追加され、Bluetooth 接続が改善され、ストレージの管理が容易になり、マルチメディア プレーヤーとカメラ デバイスをより柔軟に制御できるようになりました。 スケジュールされた時刻にタスクを非同期に実行するために、新しいジョブ スケジュール機能を使用できます。 この機能を使用すると、たとえば、デバイスが電源に接続されて充電されているときにタスクを実行するようにスケジュールすることで、バッテリの寿命を延ばすことができます。

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリ内で新しい Android 5.0 の機能を使用するには、次のものが必要です。

- **Xamarin.Android** &ndash; Visual Studio または Visual Studio for Mac に Xamarin.Android 4.20 以降をインストールして、構成する必要があります。

- **Android SDK** &ndash; Android SDK マネージャーを使用して Android 5.0 (API 21) 以降をインストールする必要があります。

- **Java Developer Kit** &ndash; API レベル 24 以降向けに開発する場合、Xamarin.Android には [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 以降が必要です (JDK 1.8 では、Lollipop を含め、24 より前の API レベルもサポートされています)。 カスタム コントロールまたは Forms プレビューアーを使用する場合は、64 ビット版の JDK 1.8 が必要です。

API レベル 23 以下専用に開発している場合は、[JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) を引き続き使用することができます。

## <a name="setting-up-an-android-50-project"></a>Android 5.0 プロジェクトの設定

Android 5.0 プロジェクトを作成するには、最新のツールと SDK パッケージをインストールする必要があります。 Android 5.0 を対象とする Xamarin.Android プロジェクトを設定するには、次の手順に従います。

1. Xamarin Android ツールをインストールし、Xamarin ライセンスをアクティブ化します。 Xamarin.Android のインストールの詳細については、「[セットアップとインストール](~/android/get-started/installation/index.md)」を参照してください。

2. Visual Studio for Mac を使用している場合は、最新の Android 5.0 更新プログラムをインストールします。

3. Android SDK マネージャーを起動し (Visual Studio for Mac では **[ツール] &gt; [Android SDK マネージャーを開く]&hellip;** )、Android SDK Tools 23.0.5 以降をインストールします。

    [![Android SDK マネージャーでの Android SDK Tools の選択](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   また、最新の Android 5.0 SDK パッケージ (API 21 以降) をインストールします。

    [![Android SDK マネージャーでの Android 5.0 SDK パッケージのインストール](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK Manager の使用の詳細については、[SDK マネージャー](https://developer.android.com/tools/help/sdk-manager.html)に関するページを参照してください。

4. 新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android の開発を初めて行う場合は、「[Hello, Android](~/android/get-started/hello-android/index.md)」を参照して、Android プロジェクトの作成について学習してください。 Android プロジェクトを作成するときは、Android 5.0 用のバージョン設定を構成してください。
   Visual Studio for Mac で、 **[プロジェクト オプション] &gt; [ビルド] &gt; [全般]** に移動し、 **[ターゲット フレームワーク]** を **[Android 5.0 (Lollipop)]** 以降に設定します。

    ![ターゲット フレームワークを Android 5.0 Lollipop に設定する](lollipop-images/target-framework.png)

   **[プロジェクト オプション] &gt; [ビルド] &gt; [Android アプリケーション]** で、最小とターゲットの Android バージョンを **[自動 - ターゲット フレームワークのバージョンを使用]** に設定します。

    ![最小とターゲットの Android バージョンの設定](lollipop-images/minimum-android-version.png)

5. アプリをテストするためのエミュレーターまたは Android デバイスを構成します。 エミュレーターを使用している場合、Xamarin Studio または Visual Studio で使用するために Android エミュレーターを構成する方法については、「[Android Emulator のセットアップ](~/android/get-started/installation/android-emulator/index.md)」を参照してください。 Android デバイスを使用している場合、Android 5.0 のデバイスを更新する方法については、[プレビュー版 SDK のセットアップ](https://developer.android.com/preview/setup-sdk.html)に関するページを参照してください。 Xamarin.Android アプリケーションの実行とデバッグのために Android デバイスを構成するには、「[開発用のデバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)」を参照してください。

メモ:Android L Preview を対象としていた既存の Android プロジェクトを更新する場合は、 **[ターゲット フレームワーク]** と **[Android バージョン]** を前述の値に更新する必要があります。

## <a name="important-changes"></a>重要な変更点

以前に発行された Android アプリは、Android 5.0 の変更の影響を受ける可能性があります。 特に、Android 5.0 では、新しいランタイムと大幅に変更された通知形式が使用されます。

### <a name="android-runtime"></a>Android ランタイム

Android 5.0 では、Dalvik ではなく新しい Android ランタイム (ART) が既定のランタイムとして使用されます。 ART では、いくつかの主要な新機能が実装されています。

- **Ahead-of-time (AOT) コンパイル** &ndash; AOT では、アプリを初めて起動する前にアプリ コードをコンパイルすることで、アプリのパフォーマンスを向上させることができます。 アプリがインストールされると、ART によって、ターゲット デバイス用のコンパイル済みアプリの実行可能ファイルが生成されます。

- **強化されたガベージ コレクション (GC)** &ndash; ART の GC の強化により、アプリのパフォーマンスも向上します。 ガベージ コレクションでは、使用される GC の一時停止が 2 つではなく 1 つになり、同時実行の GC 操作がよりタイムリーな方法で完了するようになりました。

- **強化されたアプリのデバッグ** &ndash; ART では、例外やクラッシュ レポートの分析に役立つ詳細な診断情報が提供されます。

既存のアプリは変更しなくても ART で動作しますが、以前の Dalvik ランタイムに固有の手法を利用するアプリは例外で、これは ART では動作しません。 これらの変更の詳細については、「[Android ランタイム (ART) でのアプリの動作確認](https://developer.android.com/guide/practices/verifying-apps-art.html)」を参照してください。

### <a name="notification-changes"></a>通知の変更

Android 5.0 では通知が大幅に変更されました。

- **サウンドと振動が異なる方法で処理される** &ndash; 通知のサウンドと振動は、`Ringtone`、`MediaPlayer`、`Vibrator` ではなく `Notification.Builder` によって処理されるようになりました。

- **新しい配色** &ndash; マテリアル テーマに従って、通知は白または非常に明るい背景上に暗いテキストでレンダリングされます。 また、通知アイコン内のアルファ チャネルは、システムの配色と調整するために Android によって変更される場合があります。

- **ロック画面の通知** &ndash; 通知をデバイスのロック画面に表示できるようになりました。

- **ヘッズアップ** &ndash; デバイスのロックが解除され、画面がオンになっているときに、優先度の高い通知が小さなフローティング ウィンドウ (ヘッズアップ通知) に表示されるようになりました。

ほとんどの場合、既存のアプリ通知機能を Android 5.0 に移植するには、次の手順を実行する必要があります。

1. 通知の作成に `Notification.Builder` (または `NotificationsCompat.Builder`) を使用するようにコードを変換します。

2. 新しいマテリアル テーマの配色で既存の通知アセットを表示できることを確認します。

3. 通知をロック画面に表示するときに必要な可視性を決定します。 通知がパブリックでない場合は、どのようなコンテンツをロック画面に表示する必要がありますか?

4. 新しい Android 5.0 の "*応答不可*" モードで正しく処理されるように、通知のカテゴリを設定します。

通知にトランスポート コントロールが表示される場合は、メディアの再生状態を表示するか、`RemoteControlClient` を使用するか、`ActivityManager.GetRecentTasks` を呼び出します。Android 5.0 の通知の更新の詳細については、[重要な動作変更](https://developer.android.com/preview/api-overview.html#Behaviors)に関するページを参照してください。

Android での通知の作成については、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)に関する記事を参照してください。

## <a name="material-theme"></a>マテリアル テーマ

新しい Android 5.0 のマテリアル テーマにより、Android UI のルック アンド フィールが劇的に変化します。 ビジュアル要素では、印刷ベースのデザインの太字のグラフィックス、タイポグラフィ、および明るい色を表示する触覚サーフェスが使用されるようになりました。 マテリアル テーマの例を次のスクリーンショットに示します。

[![マテリアル テーマのホーム画面、アプリ画面、設定画面のスクリーンショット](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 では、最初に左側のホーム画面が表示されます。 中央のスクリーンショットはアプリの一覧の最初の画面で、右側のスクリーンショットは **[設定]** 画面です。 Google の [Material Design](https://material.io/guidelines/material-design/introduction.html) の仕様では、新しいマテリアル テーマの概念の背後にある基礎となる設計規則について説明しています。

マテリアル テーマには、自分のアプリで使用できる 3 つの組み込みフレーバー (`Theme.Material` ダーク テーマ (既定)、`Theme.Material.Light` テーマ、`Theme.Material.Light.DarkActionBar` テーマ) が含まれています。

[![ダーク、Light、DarkActionBar テーマのスクリーンショット](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Xamarin Android アプリでのマテリアル テーマ機能の使用の詳細については、「[マテリアル テーマ](~/android/user-interface/material-theme.md)」を参照してください。

## <a name="animations"></a>Animations

Android 5.0 では、アプリ インターフェイスをより直感的に使用できるように、タッチ フィードバック アニメーション、アクティビティ遷移のアニメーション、ビューの状態遷移のアニメーションが提供されています。 また、Android 5.0 アプリでは、"*出現エフェクト*" アニメーションを使用して、ビューを非表示にしたり表示したりすることができます。 "*曲線モーション*" 設定を使用して、すばやくまたはゆっくりとアニメーションをレンダリングする方法を構成できます。

### <a name="touch-feedback-animations"></a>タッチ フィードバック アニメーション

タッチ フィードバック アニメーションでは、ビューにタッチしたときに視覚的なフィードバックがユーザーに提供されます。 たとえば、ボタンがタッチされたときに波紋の効果が表示されるようになりました。これは Android 5.0 の既定のタッチ フィードバック アニメーションです。 波紋のアニメーションは、新しい `RippleDrawable` クラスによって実装されます。 波紋効果は、ビューの境界で終了するように構成することも、ビューの境界を越えて拡大するように構成することもできます。 たとえば、次の一連のスクリーンショットは、タッチ アニメーションの間のボタンでの波紋効果を示しています。

![ボタン上での波紋のアニメーションをフレームごとに示したスクリーンショット](lollipop-images/touch-animation.png)

ボタンでの最初のタッチ コンタクトは左側の最初のイメージで発生し、残りのシーケンス (左から右) では、波紋効果がボタンの端へと拡大していく様子を示しています。 波紋のアニメーションが終了すると、ビューは元の外観に戻ります。 既定の波紋のアニメーションは一瞬で実行されますが、アニメーションの長さは、期間を長くしたり短くしたりして、カスタマイズできます。

Android 5.0 でのタッチ フィードバック アニメーションの詳細については、「[タッチ フィードバックをカスタマイズする](https://developer.android.com/training/material/animations.html#Touch)」を参照してください。

### <a name="activity-transition-animations"></a>アクティビティ遷移のアニメーション

アクティビティ遷移のアニメーションを利用すると、あるアクティビティが別のアクティビティに遷移するときに、ユーザーは、視覚の継続性の感覚を得ることができます。 アプリでは、次の 3 種類の遷移アニメーションを指定できます。

- **遷移の開始** &ndash; 画面上でアクティビティが開始されるときに使用します。

- **遷移の終了** &ndash; 画面上でアクティビティが終了されるときに使用します。

- **共有要素の遷移** &ndash; 最初のアクティビティが次へと遷移する際、2 つのアクティビティに共通するビューが変化するときに使用します。

たとえば、次の一連のスクリーンショットは、共有要素の遷移を示しています。

[![共有要素の遷移アニメーションをフレームごとに示したスクリーンショット](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

共有要素 (毛虫の写真) は、最初のアクティビティではいくつかのビューのうちの 1 つです。最初のアクティビティから 2 番目へ遷移するときに、2 番目のアクティビティでの唯一のビューになるように拡大されます。

#### <a name="enter-transition-animation-types"></a>遷移の開始のアニメーションの種類

遷移の開始では、Android 5.0 によって次の 3 種類のアニメーションが提供されます。

- **アニメーションの展開** &ndash; シーンの中心からビューを拡大します。

- **アニメーションのスライド** &ndash; シーンのいずれかの端からビューを進入させます。

- **アニメーションのフェード** &ndash; ビューをシーン内へフェードします。

#### <a name="exit-transition-animation-types"></a>遷移の終了のアニメーションの種類

遷移の終了では、Android 5.0 によって次の 3 種類のアニメーションが提供されます。

- **アニメーションの展開** &ndash; シーンの中心に向かってビューを縮小します。

- **アニメーションのスライド** &ndash; シーンのいずれかの端へビューを退出させます。

- **アニメーションのフェード** &ndash; ビューをシーン外へフェードします。

#### <a name="shared-element-transition-animation-types"></a>共有要素の遷移のアニメーションの種類

共有要素の遷移では、次のような複数の種類のアニメーションがサポートされます。

- ビューのレイアウトまたはクリップの境界を変更する。

- ビューのスケールと回転を変更する。

- ビューのサイズとスケールの種類を変更する。

Android 5.0 でのアクティビティ遷移アニメーションの詳細については、「[アクティビティ遷移をカスタマイズする](https://developer.android.com/training/material/animations.html#Transitions)」を参照してください。

### <a name="view-state-transition-animations"></a>ビューの状態遷移のアニメーション

Android 5.0 では、ビューの状態が変化するときにアニメーションを実行することが可能です。 次のいずれかの技術を使用して、ビューの状態遷移をアニメーション化できます。

- 特定のビューに関連付けられている状態の変化をアニメーション化するドローアブルを作成します。 新しい `AnimatedStateListDrawable` クラスを使用すると、ビューの状態変化の間でアニメーションを表示するドローアブルを作成できます。

- ビューの状態が変化するときに実行されるアニメーション機能を定義します。 新しい `StateListAnimator` クラスを使用すると、ビューの状態が変化するときに実行されるアニメーターを定義できます。

Android 5.0 でのビューの状態遷移のアニメーションについて詳しくは、「[ビューの状態遷移にアニメーションを付ける](https://developer.android.com/training/material/animations.html#ViewState)」を参照してください。

### <a name="reveal-effect"></a>出現エフェクト

"*出現エフェクト*" は、半径を変更してビューを表示または非表示にするクリッピングの円です。 クリッピングの円の最初と最後の半径を設定することで、このエフェクトを制御できます。 次の一連のスクリーンショットは、画面の中心からの出現エフェクトのアニメーションを示しています。

[![出現アニメーションのフレームごとのスクリーンショット](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

次のシーケンスは、画面の左下隅から実行される出現エフェクトのアニメーションを示しています。

[![クリッピング アニメーションのフレームごとのスクリーンショット](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

出現アニメーションは逆向きにすることができます。つまり、クリッピングの円によって、ビューを拡大して表示するのではなく、ビューを縮小して非表示にすることができます。

Android 5.0 での出現エフェクトの詳細については、「[出現エフェクトを使用する](https://developer.android.com/training/material/animations.html#Reveal)」を参照してください。

### <a name="curved-motion"></a>曲線モーション

これらのアニメーション機能に加えて、Android 5.0 では、アニメーションの時間とモーションの曲線を指定できる新しい API も提供されています。 Android 5.0 では、これらの曲線を使用して、アニメーション中の一時的および空間的な移動を補間します。 Android 5.0 では、次の 3 つの曲線が定義されています。

- **Fast\_out\_linear\_in** &ndash; すばやく速度を上げ、アニメーションの終わりまで加速を続けます。

- **Fast\_out\_slow\_in** &ndash; すばやく速度を上げ、アニメーションの終わりに向けてゆっくりと減速します。

- **Linear\_out\_slow\_in** &ndash; ピークの速度で開始され、アニメーションの終わりに向けてゆっくりと減速します。

新しい `PathInterpolator` クラスを使用して、モーションの補間が行われる方法を指定できます。 `PathInterpolator` は、指定されたコントロール ポイントとモーション曲線に従ってアニメーションのパスを走査するインターポレーターです。 Android 5.0 で曲線モーションの設定を指定する方法の詳細については、「[曲線モーションを使用する](https://developer.android.com/training/material/animations.html#CurvedMotion)」を参照してください。

## <a name="view-shadows--elevation"></a>ビューのシャドウとエレベーション

Android 5.0 では、新しい `Z` プロパティを設定することで、ビューの "*エレベーション*" を指定できます。 `Z` の値を大きくすると、背景のはるか上方にビューが浮かんでいるように見せて、ビューによって背景により大きなシャドウを投影できます。 レイアウト内に `elevation` 属性を構成することで、ビューの初期のエレベーションを設定することができます。

次の例では、エレベーションの属性がそれぞれ 2dp、4dp、および 6dp に設定されている場合に、空の `TextView` コントロールによって投影されるシャドウを示しています。

[![次第に大きくなるビューのシャドウを示したスクリーンショット](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

ビューのシャドウの設定は、(上記のように) 静的にすることができます。また、アニメーション内で使用して、ビューの背景の上方にビューが一時的に現れるように見せることもできます。 `ViewPropertyAnimator` クラスを使用して、ビューのエレベーションをアニメーション化できます。 ビューのエレベーションとは、そのレイアウトの `elevation` 設定と、`ViewPropertyAnimator` メソッドの呼び出しによって設定できる `translationZ` プロパティの合計値です。

Android 5.0 でのビューのシャドウの詳細については、「[シャドウを作成してビューをクリップする](https://developer.android.com/training/material/shadows-clipping.html)」を参照してください。

## <a name="color-features"></a>色の特徴

Android 5.0 では、アプリでの色を管理するための 2 つの新機能が提供されています。

- "*ドローアブルな色合い*" では、レイアウト属性を変更することによってイメージ アセットの色を変更できます。

- "*目立つ色の抽出*" では、アプリの配色テーマを動的にカスタマイズして、表示されるイメージの色パレットを使って調整することができます。

### <a name="drawable-tinting"></a>ドローアブルな色合い

Android 5.0 のレイアウトでは、ドローアブルな色を設定するために使用できる新しい `tint` 属性が認識され、さまざまな色を表示するためにこれらのアセットの複数のバージョンを作成する必要はありません。 この機能を使用するには、ビットマップをアルファ マスクとして定義し、`tint` 属性を使ってアセットの色を定義します。 これにより、一度アセットを作成すると、テーマに合わせてレイアウトに色付けすることが可能になります。

次の例では、色合いのバリエーションを作成するために、単一のイメージ アセット &ndash;背景が透明な白いロゴ&ndash; が使用されています。

![背景が透明な白い Xamarin のロゴ](lollipop-images/xamarin-logo-white.png)

以下の例に示すように、このロゴは、青い円形の背景の上に表示されます。 左の画像は、`tint` 設定がない場合のロゴの表示方法を示しています。 中央の画像では、ロゴの `tint` 属性が濃い灰色に設定されています。 右の画像では、`tint` が淡い灰色に設定されています。

![上記のロゴの色合い設定が異なる例](lollipop-images/drawable-tinting.png)

Android 5.0 でのドローアブルな色合いの詳細については、[ドローアブルな色合い](https://developer.android.com/training/material/drawables.html#DrawableTint)に関する記事を参照してください。

### <a name="prominent-color-extraction"></a>目立つ色の抽出

新しい Android 5.0 の `Palette` クラスでは、イメージから色を抽出して、カスタムの色パレットに動的に適用することができます。 `Palette` クラスでは、イメージから 6 つの色を抽出して、色の彩度と明るさの相対的なレベルに応じて、これらの色にラベル付けを行います。

- より鮮やか

- 鮮やかな暗さ

- 鮮やかな明るさ

- 落ち着いた

- 落ち着いた暗さ

- 落ち着いた明るさ

たとえば、次のスクリーンショットでは、写真表示アプリによってディスプレイ上のイメージから目立つ色を抽出し、これらの色を使ってイメージに合わせてアプリの配色を調整しています。

[![緑、ピンク、および青の配色の抽出を示したスクリーンショット](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

上に示したスクリーンショットでは、アクション バーは抽出された "鮮やかな明るさ" の色に設定され、背景は "鮮やかな暗さ" の色に設定されています。 上記の例では、イメージから抽出されたパレットの色を示すために、小さな色の正方形の行が含まれています。

Android 5.0 での色抽出に関する詳細については、[イメージからの目立つ色の抽出](https://developer.android.com/training/material/drawables.html#ColorExtract)に関するページを参照してください。

## <a name="new-ui-widgets"></a>新しい UI ウィジェット

Android 5.0 では、2 つの新しい UI ウィジェットが導入されます。

- `RecyclerView` &ndash; スクロール可能な項目の一覧を表示するビュー グループ。

- `CardView` &ndash; 角が丸い基本的なレイアウト。

いずれのウィジェットにも、マテリアル テーマ機能のサポートが組み込まれています。たとえば、`RecyclerView` では、ビューの追加と削除にアニメーションが使用され、`CardView` ではビュー シャドウを使用して、各カードが背景の上にフローティングするように表示されます。 これらの新しいウィジェットの例を、次のスクリーンショットに示します。

[![RecyclerView でビルドされたアプリのスクリーンショット](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

左側のスクリーンショットは、電子メール アプリで使用される `RecyclerView` の例であり、右側のスクリーンショットは旅行予約アプリで使用される `CardView` の例です。

### <a name="recyclerview"></a>RecyclerView

`RecyclerView` は `ListView,` と似ていますが、動的に変化する要素を持つビューやリストの大規模なセットにより適しています。 `ListView,` と同様に、基になるデータ セットにアクセスするためのアダプターを指定します。 ただし、`ListView,` とは異なり、"*レイアウト マネージャー*" を使用して、`RecyclerView` 内に項目を配置します。 レイアウト マネージャーでは、ビューのリサイクルも実行され、ユーザーに表示されなくなった項目ビューの再利用が管理されます。

`RecyclerView` ウィジェットを使用する場合、`LayoutManager` とアダプターを指定する必要があります。 この図に示されているように、`LayoutManager` はアダプターと `RecyclerView` 間の仲介役として機能します。

![LayoutManager、Adapter、Dataset をサポートする RecyclerView の図](lollipop-images/recyclerview-diagram.png)

次のスクリーンショットは、100 項目を含む `RecyclerView` を示しています (各項目は `ImageView` と `TextView` で構成されています)。

[![画像をスクロールする RecyclerView アプリのスクリーンショット](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` ではこの大規模なデータ セットを簡単に処理できます。このサンプル アプリでのリストの先頭から最後までのスクロールには、わずか数秒でしかかかりません。 `RecyclerView` ではアニメーションもサポートされています。実際、項目を追加および削除するためのアニメーションは既定で有効になっています。 `RecyclerView` に追加されると、次の一連のスクリーンショットに示されているように項目はフェードインします。

[![フェードインするフォト項目のフレームごとのスクリーンショット](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

`RecyclerView` の詳細については、「[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)」を参照してください。

### <a name="cardview"></a>CardView

`CardView` は、角が丸いフローティング カードをシミュレートする単純なビューです。 `CardView` には組み込みのビュー シャドウがあるため、アプリに視覚的な奥行きを簡単に加えることができます。 次のスクリーンショットは、`CardView` の 3 つのテキスト指向の例を示しています。

[![CardView ベースの項目で RecyclerView を使用すたアプリのスクリーンショットの例](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

上の例の各カードには `TextView` が含まれています。背景色は、`cardBackgroundColor` 属性によって設定されます。

`CardView` の詳細については、[CardView](~/android/user-interface/controls/card-view.md) に関するページを参照してください。

## <a name="enhanced-notifications"></a>強化された通知

Android 5.0 の通知システムは、新しいビジュアル形式と新機能によって大幅に更新されました。 Android 5.0 での通知の外観が新しくなりました。 たとえば、Android 5.0 の通知では、明るい背景に濃いテキストが使用されるようになりました。

![展開されていない Android 5.0 通知の例](lollipop-images/expanded-notification-contracted.png)

(上の例に示すように) 大きいアイコンが通知に表示されると、Android 5.0 によって小さいアイコンがバッジとして大きいアイコンの上に表示されます。

Android 5.0 では、通知がデバイスのロック画面にも表示される場合があります。
たとえば、次に示すのは、1 つの通知を含むロック画面のスクリーンショットの例です。

[![ロック画面に表示された通知のスクリーンショット](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

ユーザーは、ロック画面で通知をダブルタップしてデバイスのロックを解除し、その通知の発信元のアプリに移動するか、スワイプして通知を破棄することができます。 通知には、ロック画面に表示できるコンテンツの量を決定する新しい "*表示*" 設定があります。 ユーザーは、ロック画面の通知に機微なコンテンツを表示させるかどうかを選択できます。

Android 5.0 では、"*ヘッズアップ*" と呼ばれる、優先度の高い新しい通知表示形式が導入されています。 ヘッズアップ通知は、数秒のうちに画面の上部から下にスライドし、画面の上部にある通知シェードに戻ります。 ヘッズアップ通知を使用すると、システム UI により、現在実行中のアクティビティを中断することなく、ユーザーに重要な情報を提示できます。
次の例は、アプリの上部に表示されるシンプルなヘッズアップ通知を示しています。

[![ヘッズアップ通知の例](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

通常、ヘッズアップ通知は、次のイベントに使用されます。

- 新しい次のメッセージ

- 着信電話

- バッテリ低下の通知

- アラーム

Android 5.0 では、優先順位の設定が高または最大の場合にのみ、ヘッズアップ形式で通知が表示されます。

Android 5.0 では、Android での通知の並べ替えと表示をよりインテリジェントに行うための通知メタデータを指定できます。 Android 5.0 での通知は、優先度、可視性、カテゴリに基づいて整理されます。
通知カテゴリは、デバイスが "*応答不可*" モードの場合に、表示できる通知をフィルター処理するために使用されます。

最新の Android 5.0 機能を使った通知の作成と起動について詳しくは、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)に関する記事を参照してください。

## <a name="new-apis"></a>新しい API

Android 5.0 では、前述の新しいルックアンドフィール機能に加えて、既存のマルチメディア、ストレージ、およびワイヤレス/接続機能の機能を拡張する新しい API が追加されています。 また、Android 5.0 には、新しいジョブ スケジューラ機能へのサポートを提供する新しい API が含まれています。

### <a name="camera"></a>Camera

Android 5.0 では、カメラ機能を強化するための新しい API がいくつか用意されています。 新しい `Android.Hardware.Camera2` 名前空間には、Android デバイスに接続される個々のカメラ デバイスにアクセスするための機能が含まれています。 また、`Android.Hardware.Camera2` により、各カメラ デバイスはパイプラインとしてモデル化されます。これは、キャプチャ要求を受け取り、イメージをキャプチャして、結果を出力します。 このアプローチにより、アプリは複数のキャプチャ要求をカメラ デバイスにキューすることができます。

次の API を使用すると、次の新機能を実現できます。

- `CameraManager.GetCameraIdList` &ndash; カメラ デバイスにプログラムによりアクセスできます。特定のカメラ デバイスに接続するには `CameraManager.OpenCamera` を使用します。

- `CameraCaptureSession` &ndash; カメラ デバイスからイメージをキャプチャまたはストリームします。 新しいイメージ キャプチャ イベントを処理するために `CameraCaptureSession.CaptureListener` インターフェイスを実装してください。

- `CaptureRequest` &ndash; キャプチャ パラメーターを定義します。

- `CaptureResult` &ndash; イメージ キャプチャ操作の結果を提供します。

Android 5.0 の新しいカメラ API の詳細については、「[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)」を参照してください。

### <a name="audio-playback"></a>オーディオ再生

Android 5.0 では、オーディオ再生を改善するために `AudioTrack` クラスが更新されます。

- `ENCODING_PCM_FLOAT` &ndash; 動的範囲、ヘッドルーム、品質 (精度の向上のおかげで) を向上させるために、浮動小数点形式のオーディオ データを受け入れるように `AudioTrack` を構成します。 また、浮動小数点形式を使用すると、オーディオ クリップを回避できます。

- `ByteBuffer` &ndash; オーディオ データを `AudioTrack` に、バイト配列として指定できるようになりました。

- `WRITE_NON_BLOCKING` &ndash; このオプションを使用すると、一部のアプリのバッファーリングとマルチスレッド処理が簡単になります。

Android 5.0 での `AudioTrack` の機能強化の詳細については、「[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)」を参照してください。

### <a name="media-playback-control"></a>メディア再生コントロール

Android 5.0 では、`RemoteControlClient` に代わる新しい `Android.Media.MediaController` クラスが導入されています。 `Android.Media.MediaController` では、簡素化されたトランスポート コントロール API が提供され、UI コンテキストの外部でのスレッドセーフな再生制御が提供されます。 次の新しい API では、トランスポート コントロールが処理されます。

- `Android.Media.Session.MediaSession` &ndash; 複数のコントローラーを処理するメディア コントロール セッション。 `MediaSession.GetSessionToken` を呼び出して、アプリがセッションとの対話に使用するトークンを要求します。

- `MediaController.TransportControls` &ndash; **Play**、**Stop**、**Skip** などのトランスポート コマンドを処理します。

また、新しい `Android.App.Notification.MediaStyle` クラスを使用して、メディア セッションを豊富な通知コンテンツと関連付けることもできます (アルバム アートの抽出や表示など)。

Android 5.0 の新しいメディア再生コントロール機能の詳細については、「[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)」を参照してください。

### <a name="storage"></a>記憶域

Android 5.0 では、アプリケーションがディレクトリとドキュメントを簡単に操作できるように、記憶域アクセス フレームワークが更新されます。

- ディレクトリ サブツリーを選択するには、`Android.Intent.Action.OPEN_DOCUMENT_TREE` インテントをビルドして送信します。 このインテントにより、サブツリーの選択をサポートするすべてのプロバイダー インスタンスが表示されます。次に、ユーザーがディレクトリを参照して選択します。

- 新しいドキュメントまたはディレクトリをサブツリーの下に作成して管理するには、`DocumentsContract` の新しい `CreateDocument`、`RenameDocument`、および `DeleteDocument` メソッドを使用します。

- すべての共有された記憶デバイス上でパスをメディア ディレクトリに取得するには、新しい `Android.Content.Context.GetExternalMediaDirs` メソッドを呼び出します。

Android 5.0 の新しいストレージ API について詳しくは、[ストレージ](https://developer.android.com/preview/api-overview.html#Storage)に関するページを参照してください。

### <a name="wireless--connectivity"></a>ワイヤレスと接続

Android 5.0 では、ワイヤレスおよび接続に対して次の API の機能強化が追加されます。

- 新しい "*マルチネットワーク*" API により、アプリは接続を確立する前に、特定の機能を持つネットワークを検索して選択できるようになります。

- Android 5.0 デバイスが省エネルギー Bluetooth 周辺機器として動作できるようにする Bluetooth ブロードキャスト機能。

- NFC の機能強化により、データを他のデバイスと共有するために近距離通信機能を簡単に使用できるようになりました。

Android 5.0 での新しいワイヤレスおよび接続 API の詳細については、[ワイヤレスと接続](https://developer.android.com/preview/api-overview.html#Wireless)に関するページを参照してください。

### <a name="job-scheduling"></a>ジョブ スケジューリング

Android 5.0 で導入された新しい `JobScheduler` API を使用すると、デバイスが電源に接続されているときのみ実行するように特定のタスクをスケジュールすることで、ユーザーはバッテリ消費を最小限に抑えることができます。 このジョブ スケジューラ機能は、デバイスが従量制課金ネットワークではなく Wi-Fi ネットワーク経由で接続されている場合に大きなファイルをダウンロードするなど、条件がそのタスクにより適している場合に実行するようにタスクをスケジュール設定するのにも使用できます。

Android 5.0 での新しいジョブ スケジューリング API について詳しくは、[ジョブ スケジューリング](https://developer.android.com/preview/api-overview.html#JobScheduler)に関するページを参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Android アプリ開発者向けに Android 5.0 の重要な新機能の概要について説明しました。

- マテリアル テーマ

- Animations

- ビューのシャドウとエレベーション

- 色の特徴 (ドローアブルな色合いや目立つ色の抽出)

- 新しい `RecyclerView` および `CardView` ウィジェット

- 通知の機能強化

- カメラ、オーディオ再生、メディア コントロール、記憶域、ワイヤレス/接続、ジョブ スケジューリングの新しい API

Xamarin Android 開発を初めて使用する場合は、「[セットアップとインストール](~/android/get-started/installation/index.md)」を参照して、Xamarin.Android の使用を開始できます。
「[Hello, Android](~/android/get-started/hello-android/index.md)」は、Android プロジェクトを作成する方法を学習するための優れた入門書です。

## <a name="related-links"></a>関連リンク

- [Android L デベロッパー プレビュー](https://developer.android.com/preview/index.html)
- [Android SDK を入手する](https://developer.android.com/sdk/index.html#Other)
- [マテリアル デザイン](https://developer.android.com/preview/material/index.html)
