---
title: ロリポップ機能
description: この記事では、Android 5.0 (ロリポップ) で導入された新機能の高レベルの概要を説明します。 これらの機能には、ユーザー インターフェイスと呼ばれる新型マテリアル テーマとアニメーション、表示シャドウ、およびドロウアブル着色などの新しいサポート機能が含まれます。 Android 5.0 には、強化された通知、2 つの新しい UI ウィジェット、新しいジョブのスケジューラでは、および記憶域、ネットワーク、接続、およびマルチ メディア機能を向上させるために、いくつかの新しい Api も含まれています。
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: cdef611525abbe4f066959c0ac56380b1c617747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="lollipop-features"></a>ロリポップ機能

_この記事では、Android 5.0 (ロリポップ) で導入された新機能の高レベルの概要を説明します。これらの機能には、ユーザー インターフェイスと呼ばれる新型マテリアル テーマとアニメーション、表示シャドウ、およびドロウアブル着色などの新しいサポート機能が含まれます。Android 5.0 には、強化された通知、2 つの新しい UI ウィジェット、新しいジョブのスケジューラでは、および記憶域、ネットワーク、接続、およびマルチ メディア機能を向上させるために、いくつかの新しい Api も含まれています。_

## <a name="lollipop-overview"></a>ロリポップの概要

Android 5.0 (ロリポップ) には、新しいデザインの言語が導入されています*マテリアル デザイン*、それにより簡単かつ直感的に使用してアプリを作成する新しい機能のキャストをサポートしているとします。 マテリアルの設計と Android 5.0 だけでなくは、Android フォン手直しです。Android ベース タブレット、デスクトップ コンピューター、監視、およびスマート テレビ用の新しいデザイン規則のセットも提供します。 これらのデザイン規則強調簡潔さと同時に、その minimalism 直感的に、インターフェイスを理解しているユーザーをすばやく (現実的な画面の端視覚および) などの使い慣れた触る属性の使用します。

*材料テーマ*は、Android の UI のデザイン原則を具体化したものです。 この記事は、まずマテリアル テーマの補足の機能をカバーします。

-   **アニメーション** &ndash; *フィードバックをタッチ*アニメーション、*アクティビティの遷移*アニメーション、*状態遷移を表示*アニメーション、および、 *表示効果*です。

-   **シャドウと昇格の表示**&ndash;ビューがあるようになりました、`elevation`プロパティ; ビューで高い`elevation`値のキャスト バック グラウンドでより大きなシャドウします。

-   **色の機能** &ndash; *ドロウアブル着色*の色を変更することでのイメージ アセットを再利用できるようになり、*目立つ色抽出*動的に役立つテーマ、アプリは、イメージの色に基づいています。

機能が既に組み込まれている多くの素材テーマ Android 5.0 UI が発生する、他のユーザーのアプリに明示的に追加する必要があります。 いくつか標準のビュー (ボタンなど) などがあらかじめ含まれてタッチ フィードバック アニメーションでは、アプリにほとんどのビューの影が有効にする必要があります。

素材テーマからもたらさ UI の機能強化、に加えて Android 5.0 はこの記事で対象となっているいくつか他の新機能も含まれます。

-   **通知を強化** &ndash; Android 5.0 での通知が大幅に更新されましたが、新しい外観、ロック画面通知のサポートと、新しい*ヘッドアップ*通知のプレゼンテーションの形式です。

-   **新しい UI ウィジェット**&ndash;新しい`RecyclerView`ウィジェットでは、大きなデータ セットと複雑な情報、および新しいを伝えるためにアプリを簡単に`CardView`ウィジェットがテキストを表示するために簡略化されたカードのような表示形式を提供し、イメージ。

-   **新しい Api** &ndash; Android 5.0 は、Bluetooth 接続、容易に記憶域の管理、およびマルチ メディア プレーヤーやカメラ デバイスのより柔軟に制御を向上 Api を使用する複数のネットワーク サポートを追加します。 新しいジョブのスケジュール機能は非同期でタスクを実行できるようにスケジュールされた時刻。 この機能を使用する、バッテリの寿命を向上させるためになど、デバイスが接続されている場合と充電中のタスクをスケジュールします。


## <a name="requirements"></a>要件

Xamarin ベースのアプリでの Android 5.0 の新機能を使用する、次が必要。

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-   **Android SDK** &ndash; Android 5.0 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin.Android 必要[JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)または API レベルの場合 24 開発している場合は、以降、または大きい (JDK 1.8 もサポートしている API レベル 24、ロリポップを含むより前)。 カスタム コントロールまたはフォーム プレビュー用のプログラムを使用している場合、JDK 1.8 の 64 ビット バージョンが必要です。

使用を続行できます[JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API level 23 専用の開発または以前の場合。


## <a name="setting-up-an-android-50-project"></a>Android 5.0 プロジェクトの設定

Android 5.0 プロジェクトを作成するには、最新のツールおよび SDK パッケージをインストールする必要があります。 対象とする Android 5.0 Xamarin.Android プロジェクトを設定するのにには、次の手順を使用します。

1. Xamarin.Android ツールをインストールして、Xamarin ライセンスをアクティブ化します。 参照してください[セットアップとインストール](~/android/get-started/installation/index.md)Xamarin.Android のインストールに関する詳細。

2. Visual Studio for Mac を使用している場合は、最新の Android 5.0 更新プログラムをインストールします。

3. Android SDK Manager を開始 (Mac を Visual Studio で使用**ツール&gt;Android SDK Manager を開いている&hellip;**) し、Android SDK ツール 23.0.5 をインストールまたはそれ以降。

    [![Android SDK Manager で Android SDK ツールを選択します。](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   また、最新の Android 5.0 SDK パッケージ (API 21 またはそれ以降) をインストールします。

    [![Android SDK Manager で Android 5.0 SDK パッケージをインストールします。](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   詳細については、Android SDK Manager を使用して、次を参照してください。 [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html)です。

4. 新しい Xamarin.Android プロジェクトを作成します。 Xamarin を使用した Android 開発に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/index.md) Android プロジェクトの作成について学習します。 Android プロジェクトを作成する場合は、Android 5.0 バージョン設定を構成することを確認します。
   Mac 用 Visual Studio に移動**プロジェクト オプション&gt;ビルド&gt;全般**設定と**ターゲット フレームワーク**に**Android 5.0 (ロリポップ)**またはあとで：

    ![ターゲット Framwework を Android 5.0 ロリポップに設定します。](lollipop-images/target-framework.png)

   **プロジェクト オプション&gt;ビルド&gt;Android アプリケーション**最小設定、および Android バージョンをターゲットと**自動: ターゲット フレームワークのバージョンを使用して**:

    ![最小値とターゲットの Android バージョンを自動に設定します。](lollipop-images/minimum-android-version.png)

5. アプリをテストするには、エミュレーターまたは Android デバイスを構成します。 エミュレーターを使用している場合は、次を参照してください。 [Android エミュレーターのセットアップ](~/android/get-started/installation/android-emulator/index.md)Xamarin Studio または Visual Studio を使用する Android エミュレーターを構成する方法を学習します。 Android デバイスを使用している場合は、次を参照してください。[が Preview SDK の設定を](https://developer.android.com/preview/setup-sdk.html)Android 5.0 デバイスを更新する方法を学習します。 Xamarin.Android アプリケーションのデバッグを実行および Android デバイスを構成するのを参照してください。[開発用デバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)です。

メモ: 場合 Android L プレビューが対象とする既存の Android プロジェクトを更新する必要がありますを更新する、**ターゲット フレームワーク**と**Android バージョン**上で説明した値にします。

## <a name="important-changes"></a>重要な変更点

以前 Android 5.0 での変更の影響で、発行済みの Android アプリが悪影響を受ける可能性があります。 具体的には、Android 5.0 は、新しいランタイムと大幅に変更された通知の形式を使用します。

### <a name="android-runtime"></a>Android のランタイム

Android 5.0 は、Dalvik の代わりに既定のランタイムとして新しい Android ランタイム (アート) を使用します。 アートでは、いくつかの主要な新機能を実装します。

-   **先行-(AOT) コンパイル** &ndash; AOT は、アプリを初めて起動する前に、アプリのコードをコンパイルすることによってアプリのパフォーマンスを向上させることができます。 アプリがインストールされているときに、アートは、コンパイルされたアプリをターゲット デバイスに対して実行可能ファイルを生成します。

-   **ガベージ コレクション (GC) を強化**&ndash;アートの GC の機能強化ではアプリのパフォーマンスも向上します。 ここで、ガベージ コレクションが、2 つの代わりに 1 GC の一時停止を使用し、同時 GC 操作がより適切なタイミングで完了します。

-   **改良されたアプリのデバッグ**&ndash;アートは、例外の分析でヘルプし、クラッシュ レポートを詳しく診断します。

アートの下にある変更せずに既存のアプリが動作する必要があります&ndash;を以前の Dalvik 実行時に固有の手法を利用するアプリを除くこれが機能しないアートの下。 これらの変更の詳細については、次を参照してください。[アプリの動作を検証する Android ランタイム (アート) で](http://developer.android.com/guide/practices/verifying-apps-art.html)です。


### <a name="notification-changes"></a>通知の変更

通知が Android 5.0 で大幅に変更されました。

-   **サウンドと振動が異なる方法で処理される**&ndash;通知音を鳴らすし、振動がによって処理されるようになりました`Notification.Builder`の代わりに`Ringtone`、 `MediaPlayer`、および`Vibrator`です。

-   **新しい画面の配色**&ndash;マテリアル テーマに従って通知、テキストで表示暗い背景色白などに非常に簡単にします。 また、システムの配色と連携する Android して通知アイコンでアルファ チャネルを変更する可能性があります。 

-   **ロック画面通知**&ndash;通知がデバイス ロック画面に表示できるようになりました。

-   **ヘッドアップ**&ndash;小さいフローティング ウィンドウ (ヘッドアップ通知) に優先度の高い通知が表示されます、デバイスのロックが解除されていると、画面がオンになっています。

ほとんどの場合、Android 5.0 に既存のアプリの通知機能を移植にも、次の手順が必要です。

1.  コードを使用して変換`Notification.Builder`(または`NotificationsCompat.Builder`) 通知を作成するためです。 

2.  既存の通知資産が新しいマテリアルのテーマの配色パターンで表示できることを確認します。

3.  通知は、ロック画面に表示するときにどのような可視性を決定します。 通知がパブリックでない場合、ロック画面で、コンテンツ表示する必要がありますか。

4.  新しい Android 5.0 で正しく処理されるようにするために、通知のカテゴリを設定*不可*モード。

使用する場合は、通知が存在する、トランスポート コントロールの表示のメディア再生状態、 `RemoteControlClient`、呼び出しまたは`ActivityManager.GetRecentTasks`を参照してください[動作の重要な変更](http://developer.android.com/preview/api-overview.html#Behaviors)for Android の通知の更新の詳細については5.0 です。

Android での通知の作成方法の詳細については、次を参照してください。[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)です。 [互換性](~/android/app-fundamentals/notifications/local-notifications.md#compatibility)この記事の下降傾向と互換性のあるは、通知を作成する方法について説明する Android の以前のバージョンとします。


## <a name="material-theme"></a>材料テーマ

新しい Android 5.0 マテリアル テーマでは、Android の UI のルック アンド フィールに大きな変更が表示されます。 ビジュアル要素は、太字のグラフィックス、文字体裁、および印刷ベースのデザインの明るい色で実行できる触るサーフェスを使用するようになりました。 次のスクリーン ショットには、マテリアル テーマの例を示します。

[![マテリアルのテーマのホーム画面、アプリ画面、および設定画面のスクリーン ショット](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 には、左側に表示されるホーム画面に送られます。 Center スクリーン ショットは、アプリの一覧の最初の画面で、右側のスクリーン ショット、**設定**画面。 Google の[マテリアル デザイン](https://material.io/guidelines/material-design/introduction.html)仕様新しいマテリアル テーマ概念の背後にある基になるデザイン規則について説明します。

素材のテーマには、アプリで使用できる 3 つの組み込みのフレーバーが含まれています。`Theme.Material`ダーク テーマ (既定)、`Theme.Material.Light`テーマ、および`Theme.Material.Light.DarkActionBar`テーマ。 

[![暗いのスクリーン ショット、ライト、および DarkActionBar テーマ](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

詳細 Xamarin.Android アプリで資料テーマ機能の使用については、次を参照してください。[マテリアル テーマ](~/android/user-interface/material-theme.md)です。


## <a name="animations"></a>Animations

Android 5.0 では、タッチ フィードバック アニメーション、アクティビティの遷移アニメーション、およびアプリのインターフェイスをより直観的に使用する表示状態遷移アニメーションを提供します。 また、Android 5.0 アプリで使用できる*効果を表示*アニメーション ビューを表示または非表示にします。 使用することができます*曲線の動き*どの程度の速度を構成する設定やアニメーションの緩やかに変化が表示されます。


### <a name="touch-feedback-animations"></a>タッチ フィードバック アニメーション

タッチ フィードバック アニメーションは、ビューの影響を受けるときに、視覚的なフィードバックを持つユーザーを提供します。 たとえば、ボタンようになりました表示波及効果に接している&ndash;これは、Android 5.0 で既定のタッチ フィードバック アニメーション。 Ripple のアニメーションが新しいによって実装される`RippleDrawable`クラスです。 波及効果は、ビューの境界で終了またはビューの範囲を超えて拡張を構成できます。 たとえば、次のスクリーン ショットのシーケンスでは、タッチ アニメーションの中に、ボタンに波及効果を示しています。

![ボタンに ripple アニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/touch-animation.png)

初期のタッチ スクリーンへのボタンでは、(左から右へ)、残りのシーケンスは、 ボタンの端に波及効果の拡散を示しています。 中に、左側の最初のイメージで発生します。 Ripple アニメーションの終了時に、このビューは、元に返します。 既定の ripple アニメーションは、1 秒あたりの割合で行われますが、時間の長いまたは短い長さのアニメーションの長さをカスタマイズすることができます。

アニメーション Android 5.0 ではフィードバックをタッチの詳細についてを参照してください[タッチからのフィードバックのカスタマイズ](http://developer.android.com/training/material/animations.html#Touch)です。


### <a name="activity-transition-animations"></a>アクティビティの遷移のアニメーション

アクティビティの遷移のアニメーションでは、1 つのアクティビティが遷移するとき、ユーザーがビジュアルの継続性の意味に付与します。 アプリでは、遷移のアニメーションの 3 つの種類を指定できます。

-   **遷移の入力**&ndash;アクティビティが、シーンに入ったときにします。

-   **遷移の終了**&ndash;アクティビティが、シーンが終了したときにします。

-   **共有要素する遷移の**&ndash;として、次に最初のアクティビティの遷移は、2 つの活動に共通するビューが変更されたときにします。

たとえば、次の一連のスクリーン ショットは、共有要素の遷移を示しています。

[![共有要素遷移のアニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

共有要素 (、ヤの写真) は、最初のアクティビティでいくつかのビューの 1 つこれを最初の 2 番目にアクティビティの遷移と 2 つ目のアクティビティで唯一のビューに拡大します。

#### <a name="enter-transition-animation-types"></a>移行アニメーションの種類を入力してください。

Enter 遷移に対しては、Android 5.0 は、次の 3 つの種類のアニメーションを提供します。

-   **アニメーションを分解**&ndash;シーンの中心からビューを拡大します。

-   **スライド アニメーション** &ndash; 1 つのシーンの端からビューに移動します。

-   **フェード アニメーション**&ndash;シーンにビューをフェードインします。

#### <a name="exit-transition-animation-types"></a>終了時の遷移のアニメーションの種類

終了時の遷移に対しては、Android 5.0 は、次の 3 つの種類のアニメーションを提供します。

-   **アニメーションを分解**&ndash;シーンの中央にビューを縮小します。

-   **スライド アニメーション** &ndash; 1 つのシーンの端にビューを移動します。

-   **フェード アニメーション**&ndash;シーンからビューをフェードインします。

#### <a name="shared-element-transition-animation-types"></a>要素の遷移のアニメーションの種類の共有

共有要素遷移などのアニメーションでは、複数の種類をサポートします。

-   ビューのレイアウトまたはクリップの境界を変更します。

-   拡大縮小およびビューの回転を変更します。

-   ビューのサイズとスケールの種類を変更します。

詳細については、アクティビティの遷移アニメーション Android 5.0 では、次を参照してください。[カスタマイズ アクティビティの遷移](http://developer.android.com/training/material/animations.html#Transitions)です。


### <a name="view-state-transition-animations"></a>ビュー状態遷移のアニメーション

Android 5.0 によりアニメーションをビューの状態が変更されたときに実行します。 ビュー状態遷移をアニメーション化するには、次の手法のいずれかを使用します。

-   特定のビューに関連付けられている状態の変更をアニメーション化ドロウアブルを作成します。 新しい`AnimatedStateListDrawable`クラスを使用して、ビュー ステートの変化にアニメーションを表示するドロウアブルを作成できます。

-   ビューの状態が変化したときに実行するためのアニメーション機能を定義します。 新しい`StateListAnimator`クラスを使用して、ビューの状態が変化したときに実行されるアニメーターを定義できます。

詳細については、ビュー状態遷移アニメーション Android 5.0 では、次を参照してください。[ビュー ステートの変更をアニメーション化](http://developer.android.com/training/material/animations.html#ViewState)です。


### <a name="reveal-effect"></a>効果を表示します。

*効果を表示*クリッピング円を表示またはビューを非表示にするには、その変更 radius がします。 最初と最後の円の半径、クリッピングを設定して、この特殊効果を制御することができます。 次の一連のスクリーン ショットは、画面の中央から表示効果アニメーションを示しています。

[![表示のアニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

次のシーケンスは、画面の左下隅から行われる表示効果アニメーションを示しています。

[![フレームの領域のアニメーションのフレームのスクリーン ショット](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

明らかにアニメーションを元に戻すことができます。クリッピング円ことができます、ビューを非表示に圧縮ではなく、ビューを表示するために拡大します。

Android 5.0 表示影響の詳細については、次を参照してください。[表示効果を使用して](http://developer.android.com/training/material/animations.html#Reveal)です。


### <a name="curved-motion"></a>曲線の動作

これらのアニメーション機能に加えて、Android 5.0 は、アニメーションの時間とモーション曲線を指定するための新しい Api を提供します。 Android 5.0 では、これらの曲線を使用して、アニメーションの中にテンポラルと空間的移動を補間します。 Android 5.0 では、3 つの曲線が定義されています。

-   **高速\_アウト\_線形\_で**&ndash;短時間を短縮し、アニメーションの終了までを高速化し続けます。

-   **高速\_アウト\_低速\_で** &ndash; Accelerates 迅速かつ緩やかに変化したり減速アニメーションの終了するころにします。

-   **線形\_アウト\_低速\_で**&ndash;開始ピーク ベロシティ緩やかに変化し、アニメーションの最後に減速します。

新しいを使用する`PathInterpolator`クラスが動き補間が行われる方法を指定します。 `PathInterpolator` 指定した管理ポイントとモーション曲線に従ってアニメーション パスを通過するインターポレータがします。 Android 5.0 では曲線運動設定を指定する方法の詳細については、次を参照してください。[使用曲線モーション](http://developer.android.com/training/material/animations.html#CurvedMotion)です。


## <a name="view-shadows--elevation"></a>ビュー Shadows & が昇格されます。

Android 5.0 でを指定できます、*昇格*するには、新しいビューの`Z`プロパティです。 値が高い`Z`float 型の高い、背景の上に表示されるビューを作成、バック グラウンドでより大きなシャドウをキャストするビューを指定するとします。 ビューの初期の昇格を設定するには、構成をその`elevation`レイアウト内の属性です。

次の例は、空でキャスト shadows`TextView`昇格属性が設定されている場合を 2dp、4dp、および 6dp、それぞれを制御します。

[![Progessively より大きなビュー影のスクリーン ショット](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

シャドウ設定を表示 (上記のように) 静的でもかまいません一時的に、ビューの背景を超えるように表示されるビューをするアニメーションで使用できます。 使用することができます、`ViewPropertyAnimator`ビューの昇格をアニメーション化するクラス。 ビューの昇格は、レイアウトの合計`elevation`設定と`translationZ`から設定できるプロパティ、`ViewPropertyAnimator`メソッドの呼び出しです。

詳細については、ビュー shadows Android 5.0 では、次を参照してください。[影の定義とビューのクリッピング](http://developer.android.com/training/material/shadows-clipping.html)です。


## <a name="color-features"></a>色の機能

Android 5.0 は、アプリ内の色を管理するための 2 つの新しい機能を提供します。

-   *ドロウアブル着色*レイアウト属性を変更することでのイメージ資産の色を変更することができます。

-   *目立つ色抽出*を動的に表示されるイメージのカラー パレットと連携する、アプリの配色テーマをカスタマイズできるようになります。


### <a name="drawable-tinting"></a>ドロウアブル着色

Android 5.0 レイアウト認識、新しい`tint`これらの資産を別の色を表示する複数のバージョンを作成することがなくドロウアブルの色を設定に使用できる属性です。 この機能を使用するアルファ マスクおよび使用すると、ビットマップを定義する、`tint`資産の色を定義する属性。 これにより、資産を一度だけ作成し、それらの色のテーマを一致するように、レイアウトを可能です。

次の例では、1 つのイメージ資産&ndash;透明な背景が白ロゴ&ndash;濃淡のバリエーションを作成するために使用します。

![透明な背景が白 Xamarin ロゴ](lollipop-images/xamarin-logo-white.png)

次の例に示すように、青い円形の背景上このロゴが表示されます。 左側の画像はせず、ロゴを表示する方法、`tint`設定します。 図では、center、ロゴの`tint`属性が濃い灰色に設定します。 イメージを右に`tint`淡い灰色に設定されています。

![異なる濃淡の設定で上記のロゴの例](lollipop-images/drawable-tinting.png)

Android 5.0 ドロウアブル着色に関する詳細は、次を参照してください。[ドロウアブル着色](http://developer.android.com/training/material/drawables.html#DrawableTint)です。


### <a name="prominent-color-extraction"></a>目立つ色の抽出

新しい Android 5.0`Palette`クラスでは、カスタム カラー パレットに動的に適用できるように、イメージから色を抽出することができます。 `Palette`クラスがイメージから 6 種類の色を抽出し、これらの色の鮮やかさ、明るさの相対的なレベルに従って [ラベル]。

-   生き生きとしました。

-   活気のある濃い色

-   鮮やかなライト

-   ミュート

-   ミュート dark

-   ライト ミュート

たとえば、次のスクリーン ショットでは、アプリを表示する写真はディスプレイ上のイメージから目立つ色を抽出し、これらの色を使用して、イメージと一致するアプリの画面の配色を改変します。

[![緑、ピンク色、および青のテーマの色の抽出のスクリーン ショット](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

抽出された"鮮やかな light"に設定されている操作バー上のスクリーン ショットでの色と背景に設定されている、抽出された「活気のある濃い色」色。 各上記の例では、小規模の色の四角形の行をイメージから抽出されたパレットの色を示します。

Android 5.0 では色の抽出に関する詳細は、次を参照してください。[イメージから目立つ色の抽出](http://developer.android.com/training/material/drawables.html#ColorExtract)です。


## <a name="new-ui-widgets"></a>新しい UI ウィジェット

Android 5.0 には、次の 2 つの新しい UI ウィジェットが導入されています。

-   `RecyclerView` &ndash; スクロール可能な項目の一覧を表示するビューのグループ。

-   `CardView` &ndash; 角が丸い基本的なレイアウトです。

両方のウィジェットには、マテリアル テーマ機能です。 固定のサポートが含まれます。たとえば、`RecyclerView`の追加と削除、ビュー、アニメーションを使用してと`CardView`使用がバック グラウンドの手前に表示される各カードに影を表示します。 次のスクリーン ショットではこれらの新しいウィジェットの例のとおりです。

[![RecyclerView でビルドされたアプリのスクリーン ショット](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

左側のスクリーン ショットの例に示します`RecyclerView`で使用されている電子メール アプリ、およびスクリーン ショットは、上、右の例に示します`CardView`旅行の予約アプリケーションで使用されています。


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` に似ていますが`ListView,`が、これは、ビューまたは動的に変更する要素のリストのセットが大きい場合に適しています。 同様に`ListView,`基になるデータ セットへのアクセスには、アダプターを指定します。 ただしとは異なり`ListView,`を使用する、*レイアウト マネージャー*内のアイテムを配置する`RecyclerView`です。 レイアウト マネージャーも処理のビューがリサイクルされます。ユーザーに表示されなくなった項目のビューの再利用を管理します。

使用すると、`RecyclerView`を指定する必要があります、ウィジェット、`LayoutManager`とアダプター。 この図に示すよう`LayoutManager`アダプター間の仲介者は、および`RecyclerView`:

![LayoutManager、アダプター、およびデータセットのサポートを含む RecyclerView のダイアグラム](lollipop-images/recyclerview-diagram.png)

次のスクリーン ショットを示しています、 `RecyclerView` 100 アイテムを格納している (各アイテムから成る、`ImageView`と`TextView`)。

[![イメージをスクロール RecyclerView アプリのスクリーン ショット](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` この大きなデータ セットを簡単に処理&ndash;を終了するリストの先頭からスクロールこのサンプルの一覧のアプリのみは数秒かかります。 `RecyclerView` アニメーションもサポートしています実際には、既定で追加および削除する項目のアニメーションを有効にします。 アイテムを追加するときに、 `RecyclerView`、この一連のスクリーン ショットのように消えてしまう。

[![レイアウト枠でフォト項目フェードのフレームのスクリーン ショット](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

詳細については`RecyclerView`を参照してください[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)です。


### <a name="cardview"></a>CardView

`CardView` 角が丸い浮動カードをシミュレートする単純なビューです。 `CardView`組み込みビューの影の付いた、ビジュアルの深さをアプリに追加するための簡単な方法を提供します。 次のスクリーン ショットの 3 つのテキスト指向例を示しています`CardView`:

[![CardView ベースの項目を含む RecyclerView を使用してアプリの例のスクリーン ショット](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

各カード上の例が含まれています、 `TextView`; 経由では、背景色を設定してください。、`cardBackgroundColor`属性。

詳細については`CardView`を参照してください[CardView](~/android/user-interface/controls/card-view.md)です。


## <a name="enhanced-notifications"></a>強化された通知

Android 5.0 で通知システムが新しいビジュアル形式と新しい機能が大幅に更新されました。 通知では、Android 5.0 に外観が新しくがあります。 たとえば、Android 5.0 での通知が明るい背景に濃いテキストを使用するようになりました。

![展開されていない Android 5.0 通知の例](lollipop-images/expanded-notification-contracted.png)

大きいアイコンが表示されたら通知 (示すように上記の例) で、Android 5.0 は大きなアイコンの上、バッジとして小さいアイコンを表示します。 

Android 5.0 は、通知もデバイス ロック画面で表示できます。
たとえば、1 つの通知とロック画面の例のスクリーン ショットを次に示します。

[![ロック画面上に表示される通知のスクリーン ショット](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

ユーザー ダブルタップがデバイスのロックを解除し、その通知が送信されたアプリにジャンプするロック画面で通知またはスワイプして通知を無視します。 通知がある新しい*可視性*ロック画面に表示できるコンテンツの量を決定する設定。 ユーザーは、ロック画面通知に表示される機密性の高いコンテンツを許可するかどうかを選択できます。

Android 5.0 と呼ばれる新しい優先度の高い通知のプレゼンテーション形式が導入されています*ヘッドアップ*です。 ヘッドアップ通知では、数秒、画面の上部から下にスライドさせます、し、画面の上部にある通知の網掛けに戻る退却です。 ヘッドアップ通知では、システム UI を現在実行中のアクティビティを中断することがなく、ユーザーの前に重要な情報を格納できます。 次の例では、アプリの上に表示する単純なヘッドアップ通知を示します。

[![ヘッドアップ通知の例](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

通常、ヘッドアップ通知は次のイベントに対して使用されます。

-   次の新しいメッセージ

-   かかってきた電話

-   バッテリを示す値

-   アラーム

Android 5.0 では、高、最大の優先順位の設定がある場合にのみ、ヘッドアップ形式で通知が表示されます。

Android 5.0 では、Android を並べ替えるしより的確に通知を表示する通知のメタデータを提供できます。 Android 5.0 では、優先度、可視性、およびカテゴリに従って通知を整理します。
通知のカテゴリを使用してフィルターが、デバイスの場合、通知を提示する*不可*モード。

作成して、最新の Android 5.0 機能による通知を起動する方法の詳細についてを参照してください。[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)です。


## <a name="new-apis"></a>新しい API

上記で説明した新しい外観と操作性機能、に加えては、Android 5.0 は、既存のマルチ メディアの機能、ストレージ、およびワイヤレス接続または機能を拡張するための新しい Api を追加します。 また、Android 5.0 には、新しいジョブ スケジューラ機能のサポートを提供する新しい Api が含まれます。

### <a name="camera"></a>カメラ

Android 5.0 は、カメラの拡張機能のいくつかの新しい Api を提供します。 新しい`Android.Hardware.Camera2`名前空間には、Android デバイスに接続されている個々 のカメラ デバイスへのアクセスの機能が含まれます。 また、`Android.Hardware.Camera2`パイプラインとして各カメラ デバイスをモデル化: キャプチャの要求を受け入れ、イメージをキャプチャし、結果を出力します。 この方法によりアプリのカメラ デバイスに複数のキャプチャ要求をキューに登録できます。

次の Api を使うこれらの新機能。

-   `CameraManager.GetCameraIdList` &ndash; プログラムでカメラ デバイス; にアクセスすることができます。使用する`CameraManager.OpenCamera`特定カメラ デバイスに接続します。

-   `CameraCaptureSession` &ndash; キャプチャまたはカメラ デバイスからのイメージがストリームされます。 実装する、`CameraCaptureSession.CaptureListener`を新しいイメージのキャプチャのイベントを処理するインターフェイスです。

-   `CaptureRequest` &ndash; キャプチャ パラメーターを定義します。

-   `CaptureResult` &ndash; イメージのキャプチャ操作の結果を提供します。

新しいカメラ Android 5.0 で Api の詳細についてを参照してください。[メディア](http://developer.android.com/about/versions/android-5.0.html#Media)です。

### <a name="audio-playback"></a>オーディオの再生

Android 5.0 更新プログラム、`AudioTrack`オーディオの再生を向上させるためのクラス。

-   `ENCODING_PCM_FLOAT` &ndash; 構成`AudioTrack`向上動的範囲、大きいヘッドルーム、および品質の向上 (より高い精度) ご協力に感謝の浮動小数点形式でのオーディオのデータを受け入れるようにします。 また、浮動小数点の書式は、オーディオ クリップの回避に役立ちます。

-   `ByteBuffer` &ndash; オーディオのデータには、ここで、`AudioTrack`バイト配列として。

-   `WRITE_NON_BLOCKING` &ndash; このオプションは、バッファリングを簡略化し、一部のアプリのマルチ スレッドです。

詳細については`AudioTrack`Android 5.0 では、機能強化を参照してください[メディア](http://developer.android.com/about/versions/android-5.0.html#Media)です。

### <a name="media-playback-control"></a>メディア再生コントロール

Android 5.0 が導入されていますが、新しい`Android.Media.MediaController`クラス、どの置換`RemoteControlClient`です。 `Android.Media.MediaController` 簡略化されたトランスポート コントロールの Api を提供し、再生 UI コンテキスト外でのスレッド セーフであるコントロールを提供します。 次の新しい Api では、トランスポート コントロールを処理します。

-   `Android.Media.Session.MediaSession` &ndash; メディアは、複数のコント ローラーを処理するセッションを制御します。 呼び出す`MediaSession.GetSessionToken`アプリを使用して、セッションと対話するトークンを要求します。

-   `MediaController.TransportControls` &ndash; ハンドルなどのコマンドのトランスポート**再生**、**停止**、および**Skip**です。

また、使用する新しい`Android.App.Notification.MediaStyle`豊富な通知 (などのコンテンツを抽出するアルバム アートを表示) とメディア セッションを関連付けるクラスです。

Android 5.0 の新しいメディア再生コントロールの機能の詳細についてを参照してください。[メディア](http://developer.android.com/about/versions/android-5.0.html#Media)です。

### <a name="storage"></a>記憶域

Android 5.0 では、ディレクトリとドキュメントを操作するアプリケーションを容易にできるようにするのには、記憶域アクセス フレームワークを更新します。

-   ディレクトリ サブツリーを選択するには、構築し、送信することができます、`Android.Intent.Action.OPEN_DOCUMENT_TREE`目的としました。 この目的が原因でシステム サブツリーの選択をサポートするすべてのプロバイダー インスタンスを表示するにはユーザーを参照し、ディレクトリを選択します。

-   作成し、新しいドキュメントまたはサブツリーで任意の場所のディレクトリの管理、使用する新しい`CreateDocument`、 `RenameDocument`、および`DeleteDocument`のメソッド`DocumentsContract`です。

-   すべての共有記憶域デバイスでメディアのディレクトリへのパスを取得する新しい呼び出す`Android.Content.Context.GetExternalMediaDirs`メソッドです。

新しい記憶域の Android 5.0 Api の詳細についてを参照してください。[ストレージ](http://developer.android.com/preview/api-overview.html#Storage)です。

### <a name="wireless--connectivity"></a>ワイヤレスとの接続

Android 5.0 は、次の API 拡張機能のワイヤレスとの接続を追加します。

-   新しい*マルチ ネットワーク*Api アプリを検索し、接続を行う前に特定の機能を持つネットワークを選択するが可能です。

-   Bluetooth ブロードキャストできるようにする機能が Android 5.0 デバイスの低電力 Bluetooth 周辺機器として機能します。

-   近距離無線通信機能を使用して他のデバイスとデータを共有しやすく NFC 拡張機能です。

詳細については、新しいワイヤレスおよび接続 Android 5.0 での Api は、次を参照してください。[ワイヤレスおよび接続](http://developer.android.com/preview/api-overview.html#Wireless)です。

### <a name="job-scheduling"></a>ジョブのスケジュール設定

Android 5.0 が導入されていますが、新しい`JobScheduler`ユーザーに役立つ API は、デバイスが電源に接続されているときのみ実行する特定のタスクをスケジュール設定と充電でバッテリ消費を最小限に抑えます。 このジョブのスケジューラ機能は、条件は、デバイスが従量制課金接続ではなく、Wi-fi ネットワーク経由で接続しているときにサイズの大きなファイルのダウンロードなど、そのタスクに適切なときに実行するタスクのスケジュールにも使用できます。

新しいジョブの Android 5.0 で Api のスケジューリングの詳細についてを参照してください。[ジョブをスケジュール](http://developer.android.com/preview/api-overview.html#JobScheduler)です。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Android アプリ開発者の Android 5.0 の重要な新機能の概要を提供します。

-   材料テーマ

-   Animations

-   ビュー シャドウと昇格

-   カラー機能、ドロウアブル着色など、目立つ色の抽出

-   新しい`RecyclerView`と`CardView`ウィジェット

-   通知の機能強化

-   カメラ、オーディオの再生、メディア コントロール、記憶域、ワイヤレス/接続、およびジョブのスケジュール設定用の新しい Api

Xamarin Android 開発に慣れていない場合は、読み取り[セットアップとインストール](~/android/get-started/installation/index.md)Xamarin.Android で作業を開始するためです。
[こんにちは, Android](~/android/get-started/hello-android/index.md) Android プロジェクトを作成する方法を学習するための優れた入門がします。



## <a name="related-links"></a>関連リンク

- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Android SDK を取得します。](https://developer.android.com/sdk/index.html#Other)
- [素材のデザイン](http://developer.android.com/preview/material/index.html)
- [素材のデザインの原則](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
