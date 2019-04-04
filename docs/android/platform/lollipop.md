---
title: ロリポップ機能
description: この記事では、Android 5.0 (Lollipop) で導入された新機能の概要を提供します。 これらの機能には、アニメーション、表示の影の描画可能な色合いを付けることなどの新しいサポート機能と同様に、素材のテーマと呼ばれる新しいユーザー インターフェイスのスタイルが含まれます。 Android 5.0 には、強化された通知、2 つの新しい UI ウィジェット、新しいジョブ スケジューラ、および記憶域、ネットワーク、接続、およびマルチ メディア機能を向上させるために、いくつかの新しい Api も含まれています。
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d6173e1886eaf807decd960b07acc022bb17c04d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669077"
---
# <a name="lollipop-features"></a>ロリポップ機能

_この記事では、Android 5.0 (Lollipop) で導入された新機能の概要を提供します。これらの機能には、アニメーション、表示の影の描画可能な色合いを付けることなどの新しいサポート機能と同様に、素材のテーマと呼ばれる新しいユーザー インターフェイスのスタイルが含まれます。Android 5.0 には、強化された通知、2 つの新しい UI ウィジェット、新しいジョブ スケジューラ、および記憶域、ネットワーク、接続、およびマルチ メディア機能を向上させるために、いくつかの新しい Api も含まれています。_

## <a name="lollipop-overview"></a>ロリポップの概要

Android 5.0 (Lollipop) には、新しいデザイン言語が導入されています*マテリアル デザイン*、それにより簡単かつ直感を使用するアプリを構築する新機能のキャストをサポートしているとします。 マテリアル デザインでは、Android 5.0 だけでなくにより、Android フォン手直し;Android ベースのタブレットやデスクトップ コンピューター、ウォッチ、スマート Tv の新しいデザイン ルールのセットも提供します。 これらのデザイン規則強調簡潔さとしながらミニマリズム (現実的なサーフェイスとの端とキュー) などの使い慣れた触る属性を使用してユーザーをすばやくを直感的に、インターフェイスを理解します。

*素材のテーマ*Android の UI の設計原則を具体化したものです。 この記事では、素材のテーマのサポート機能について説明して開始します。

-   **アニメーション** &ndash; *タッチ フィードバック*アニメーション、*アクティビティの遷移*アニメーション、*状態遷移を表示*アニメーション、および、 *表示効果*します。

-   **シャドウと昇格の表示**&ndash;ビューのようになりましたが、`elevation`プロパティです。 ビューで高い`elevation`値は、バック グラウンドで大きい shadows、キャスト。

-   **色の機能** &ndash; *Drawable 色合いを付けること*の色を変更することでイメージの資産の再利用可能と*目立つ色抽出*動的にするのに役立ちますテーマ、アプリは、画像内の色に基づいています。

機能が既に組み込まれている多くの資料テーマ Android 5.0 UI エクスペリエンス、他のユーザー アプリに明示的に追加する必要があります。 たとえば、一部の標準ビュー (ボタンなど) が既に含まれて、タッチ フィードバック アニメーションには中に、アプリには、ほとんどのビューの影が有効にする必要があります。

素材のテーマからもたらさ UI 機能強化、に加えて Android 5.0 はこの記事で取り上げるいくつかその他の新機能も含まれます。

-   **通知の強化**&ndash;の外観が新しく、ロック画面の通知のサポート、および新しい Android 5.0 での通知が大幅に更新されました*ヘッドアップ*通知の表示形式。

-   **新しい UI ウィジェット**&ndash;新しい`RecyclerView`ウィジェットにより、大規模なデータ セットと、複雑な情報と新しいを伝達するためにアプリを簡単に`CardView`ウィジェット テキストを表示するカードのようなプレゼンテーションを簡素化された形式を提供し、イメージ。

-   **新しい Api** &ndash; Android 5.0 は、Bluetooth 接続性、簡単に記憶域の管理、およびマルチ メディア プレーヤーとカメラのデバイスをより柔軟に制御を向上 Api を使用する複数のネットワークのサポートを追加します。 新しいジョブのスケジューリング機能がで非同期的にタスクを実行できるようにスケジュールされた時間。 この機能により、バッテリの寿命を向上させるためになど、デバイスが接続されている場合、課金を実行するタスクをスケジュールできます。


## <a name="requirements"></a>必要条件

Xamarin ベースのアプリでの Android 5.0 の新機能を使用する、次が必要。

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-   **Android SDK** &ndash; Android 5.0 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-   **Java Developer Kit** &ndash; Xamarin.Android 必要[JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)または以降の API レベル 24 開発している場合、または大きい (JDK 1.8 もサポートしている API レベル 24、Lollipop を含むより前)。 カスタム コントロールまたはフォーム プレビューアーを使用している場合、JDK 1.8 の 64 ビット バージョンが必要です。

引き続き使用できます[JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)またはそれ以前の API レベル 23 ののみで開発する場合。


## <a name="setting-up-an-android-50-project"></a>Android 5.0 プロジェクトの設定

Android 5.0 プロジェクトを作成するには、最新のツールおよび SDK パッケージをインストールする必要があります。 対象とする Android 5.0 を Xamarin.Android プロジェクトを設定するのにには、次の手順を使用します。

1. Xamarin.Android ツールをインストールし、Xamarin ライセンスをアクティブ化します。 参照してください[セットアップとインストール](~/android/get-started/installation/index.md)詳細については、Xamarin.Android をインストールします。

2. Visual Studio for Mac を使用している場合は、Android 5.0 の最新の更新プログラムをインストールします。

3. Android SDK マネージャーを起動 (Visual studio for Mac では、次のように使用します。**ツール&gt;Android SDK マネージャーを開く&hellip;**) と Android SDK Tools 23.0.5 をインストールまたはそれ以降。

    [![Android SDK ツール、Android SDK マネージャーを選択](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   また、最新の Android 5.0 SDK パッケージ (API 21 以降) をインストールします。

    [![Android SDK Manager で Android 5.0 SDK パッケージをインストールします。](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   詳細については、Android SDK Manager を使用して、[SDK Manager](https://developer.android.com/tools/help/sdk-manager.html)を参照してください。

4. 新しい Xamarin.Android プロジェクトを作成します。 Xamarin で Android の開発に慣れていない場合は、[Hello, Android](~/android/get-started/hello-android/index.md)を Android プロジェクトを作成する方法について説明を参照してください。 Android プロジェクトを作成するときに、Android 5.0 のバージョンの設定を構成することを確認します。
   Visual studio for Mac に移動します**プロジェクト オプション&gt;ビルド&gt;全般**設定と**ターゲット フレームワーク**に**Android 5.0 (Lollipop)** または。あとで：

    ![ターゲット Framwework を Android 5.0 Lollipop に設定します。](lollipop-images/target-framework.png)

   **プロジェクト オプション&gt;ビルド&gt;Android アプリケーション**、最小値を設定およびターゲットの Android バージョンを**自動 - ターゲット フレームワーク バージョンを使用して**:

    ![最小値とターゲット Android バージョンを自動に設定します。](lollipop-images/minimum-android-version.png)

5. アプリをテストするには、エミュレーターまたは Android デバイスを構成します。 エミュレーターを使用している場合は、[Android Emulator のセットアップ](~/android/get-started/installation/android-emulator/index.md)を Xamarin Studio または Visual Studio を使用するため、Android エミュレーターを構成する方法について説明を参照してください。 Android デバイスを使用している場合は、[the Preview SDK の設定を](https://developer.android.com/preview/setup-sdk.html)を Android 5.0 デバイスを更新する方法について説明を参照してください。 実行していると、Xamarin.Android アプリケーションをデバッグ用に Android デバイスを構成するを参照してください。[開発用デバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)します。

メモ:更新する必要がありますが、Android の L プレビューを対象とする既存の Android プロジェクトを更新する場合、**ターゲット フレームワーク**と**Android バージョン**上記で説明した値にします。

## <a name="important-changes"></a>重要な変更

以前発行された Android アプリは Android 5.0 での変更によって受ける可能性があります。 具体的には、Android 5.0 には、新しいランタイムと大幅に変更された通知の形式が使用されます。

### <a name="android-runtime"></a>Android ランタイム

Android 5.0 は、Dalvik ではなく、既定のランタイムとして新しい Android ランタイム (アート) を使用します。 アートは、いくつかの主要な新機能を実装します。

-   **先行 of time (AOT) コンパイル** &ndash; AOT は、アプリを初めて起動する前に、アプリのコードをコンパイルすることによってアプリのパフォーマンスを向上することができます。 アプリがインストールされているとアートは、ターゲット デバイスの実行可能ファイルのコンパイル済みのアプリを生成します。

-   **ガベージ コレクション (GC) の改善**&ndash;アートの GC 強化アプリのパフォーマンスを改善できます。 ガベージ コレクションではなく、2 つの 1 の GC の一時停止を使用して、タイムリーな方法で同時 GC 操作を完了する.

-   **強化されたアプリのデバッグ**&ndash;アートは、例外の分析に役立つレポートとクラッシュ レポートを詳しく診断します。

アートの下で変更せずに既存のアプリが動作する必要があります&ndash;前 Dalvik 実行時に固有の手法を活用するアプリを除くこれが機能しないアートの下。 これらの変更の詳細については、[アプリの動作を検証する Android ランタイム (アート) で](https://developer.android.com/guide/practices/verifying-apps-art.html)を参照してください。


### <a name="notification-changes"></a>通知の変更

通知が Android 5.0 で大幅に変更されました。

-   **サウンドと振動を異なる方法で処理**&ndash;通知音を鳴らすおよび振動はによって処理される`Notification.Builder`の代わりに`Ringtone`、 `MediaPlayer`、および`Vibrator`します。

-   **新しい配色**&ndash;素材のテーマ、に従って通知がテキストで表示する濃い白または非常に明るい背景上。 また、システムの配色と連携する Android での通知アイコンでアルファ チャネルを変更できます。 

-   **ロック画面通知**&ndash;通知がデバイスのロック画面に表示できるようになりました。

-   **ヘッドアップ**&ndash;小規模のフローティング ウィンドウ (ヘッドアップ通知) に優先度の高い通知が表示されますと、デバイスのロックが解除されていると、画面の電源をオンします。

ほとんどの場合、Android 5.0 に既存のアプリ通知機能の移植にも、次の手順が必要です。

1.  コードを使用して変換`Notification.Builder`(または`NotificationsCompat.Builder`) 通知を作成するためです。 

2.  既存の通知アセットが新しい素材のテーマ色スキームで表示できることを確認します。

3.  通知は、ロック画面に表示するときに必要な可視性を決定します。 通知がパブリックでない場合、コンテンツは、ロック画面に現れる必要がありますか。

4.  新しい Android 5.0 で正しく処理されるため、通知のカテゴリを設定*不可*モード。

場合は、通知には、トランスポート コントロール、ディスプレイのメディア再生状態が存在するを使用して、 `RemoteControlClient`、呼び出したり`ActivityManager.GetRecentTasks`を参照してください[動作の重要な変更](https://developer.android.com/preview/api-overview.html#Behaviors)詳細については、Android の通知を更新しています5.0 です。

Android で通知を作成する方法の詳細については、[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)を参照してください。 [互換性](~/android/app-fundamentals/notifications/local-notifications.md#compatibility)この記事のセクションと互換性のある下向きの通知を作成する方法を説明します。 以前のバージョンの Android です。


## <a name="material-theme"></a>マテリアル テーマ

新しい Android 5.0 素材のテーマでは、Android の UI の外観を大幅に変更が表示されます。 ビジュアル要素は、太字のグラフィックス、文字体裁、および印刷ベースのデザインの明るい色に触るサーフェスを使用するようになりました。 次のスクリーン ショットには、素材のテーマの例を示します。

[![素材のテーマのホーム画面で、アプリの画面と設定画面のスクリーン ショット](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 には、左側のようなホーム画面に送られます。 センターのスクリーン ショットは、アプリの一覧の最初の画面と、右側のスクリーン ショットは、**設定**画面。 Google の[マテリアル デザイン](https://material.io/guidelines/material-design/introduction.html)仕様には、新しい素材のテーマの概念の背後にある基になるデザイン規則がについて説明します。

素材のテーマには、アプリで使用できる組み込み 3 種類の味が含まれています。`Theme.Material`ダーク テーマ (既定)、`Theme.Material.Light`テーマ、および`Theme.Material.Light.DarkActionBar`テーマ。 

[![濃いのスクリーン ショット、光、および DarkActionBar テーマ](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

詳細については、Xamarin.Android アプリの 素材のテーマ機能を使用して、[マテリアル テーマ](~/android/user-interface/material-theme.md)を参照してください。


## <a name="animations"></a>Animations

Android 5.0 は、タッチ フィードバック アニメーション、アクティビティの遷移のアニメーション、およびアプリのインターフェイスをより直感的に使用する表示状態遷移のアニメーションを提供します。 また、Android 5.0 アプリを使用できます*効果を表示*アニメーション ビューを表示または非表示にします。 使用することができます*曲線のモーション*どの程度の速度を構成する設定やアニメーションの緩やかに変化が表示されます。


### <a name="touch-feedback-animations"></a>タッチ フィードバック アニメーション

タッチ フィードバック アニメーションは、ビューがタッチされたときに、視覚的なフィードバックを持つユーザーを提供します。 たとえば、ボタンようになりました表示波及効果が影響を受ける&ndash;Android 5.0 既定タッチ フィードバック アニメーションになります。 Ripple のアニメーションが新しいによって実装される`RippleDrawable`クラス。 波及効果は、ビューの境界で終了またはビューの境界を超えて拡張を構成できます。 たとえば、次の一連のスクリーン ショットでは、タッチのアニメーションの中にボタンに波及効果を示しています。

![ボタン上のリップル アニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/touch-animation.png)

(左右から) から残りのシーケンスに、ボタンの端に波及効果の分散を示しています。 中には、左側で、最初の図で、ボタンとの接続を最初のタッチに発生します。 Ripple アニメーションの終了時に、ビューは、元に返します。 既定のリップル アニメーションには、2 つ目の分数でが行わしますが、時間の長いまたは短い長さのアニメーションの長さをカスタマイズできます。

Android 5.0 でフィードバックのアニメーションをタッチの詳細について参照してください[タッチ フィードバックのカスタマイズ](https://developer.android.com/training/material/animations.html#Touch)します。


### <a name="activity-transition-animations"></a>アクティビティの遷移のアニメーション

アクティビティの遷移のアニメーションは、1 つのアクティビティが遷移するときに、ユーザーのビジュアルの継続性のある意味を提供します。 アプリでは、遷移のアニメーションの次の 3 つの種類を指定できます。

-   **遷移の入力**&ndash;のアクティビティが、シーンに入ったとき。

-   **移行を終了**&ndash;アクティビティが、シーンが終了したときにします。

-   **要素の遷移を共有**&ndash;として、次に最初のアクティビティの遷移は、2 つのアクティビティに共通するビューが変更されたときにします。

たとえば、次の一連のスクリーン ショットは、共有要素の遷移を示しています。

[![共有要素の遷移のアニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

共有要素 (、ヤの写真) は、最初のアクティビティでいくつかのビューの 1 つです。2 番目の最初のアクティビティの遷移として 2 番目のアクティビティで専用のビューにそれを拡大します。

#### <a name="enter-transition-animation-types"></a>遷移のアニメーションの種類を入力します。

入力の遷移に対しては、Android 5.0 は、次の 3 つの種類のアニメーションを提供します。

-   **アニメーションの爆発**&ndash;から、センター シーンのビューを拡大します。

-   **スライド アニメーション** &ndash; 1 つのシーンの端からビューに移動します。

-   **フェード アニメーション**&ndash;をシーンにビューをフェードインします。

#### <a name="exit-transition-animation-types"></a>終了の遷移のアニメーションの種類

終了の切り替えには、Android 5.0 は、次の 3 つの種類のアニメーションを提供します。

-   **アニメーションの爆発**&ndash;センター シーンのビューに縮小します。

-   **スライド アニメーション** &ndash; 1 つのシーンの端にビューを移動します。

-   **フェード アニメーション**&ndash;シーンからビューをフェードインします。

#### <a name="shared-element-transition-animation-types"></a>要素の遷移のアニメーションの種類の共有

共有要素の遷移など複数の種類のアニメーションのサポートします。

-   ビューのレイアウトまたはクリップの境界を変更します。

-   スケールとビューの回転を変更します。

-   ビューのサイズとスケールの種類を変更します。

詳細については、Android 5.0 でアクティビティの遷移アニメーションは、[アクティビティの遷移をカスタマイズ](https://developer.android.com/training/material/animations.html#Transitions)を参照してください。


### <a name="view-state-transition-animations"></a>ビュー状態遷移のアニメーション

Android 5.0 では、アニメーションをビューの状態が変更されたときに実行できます。 次の手法のいずれかを使用してビュー状態遷移をアニメーション化することができます。

-   特定のビューに関連付けられている状態の変化をアニメーション化するドローアブルを作成します。 新しい`AnimatedStateListDrawable`クラスを使用して、ビュー ステートの変更の間でアニメーションを表示するドローアブルを作成できます。

-   ビューの状態が変更されたときに実行されるアニメーション機能を定義します。 新しい`StateListAnimator`クラスを使用して、ビューの状態が変更されたときに実行されるアニメーターを定義できます。

詳細については、Android 5.0 でのビュー状態遷移のアニメーションは、[ビュー ステートの変更をアニメーション化する](https://developer.android.com/training/material/animations.html#ViewState)を参照してください。


### <a name="reveal-effect"></a>効果を表示します。

*表示効果*クリッピング円は、ビューを非表示には、その変更 radius。 クリッピングの円の最初と最後の半径を設定して、この効果を制御できます。 次の一連のスクリーン ショットは、画面の中央から表示効果アニメーションを示しています。

[![表示アニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

次のシーケンスは、画面の左下隅から行われる表示効果アニメーションを示しています。

[![クリッピングのアニメーションのフレームのスクリーン ショットでフレーム](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

表示アニメーションを元に戻すことができます。クリッピング円ことができます、ビューを非表示に圧縮ではなく、ビューを表示すると拡大表示します。

Android 5.0 表示効果の詳細については、[表示効果を使用して、](https://developer.android.com/training/material/animations.html#Reveal)を参照してください。


### <a name="curved-motion"></a>曲線のモーション

Android 5.0 には、これらのアニメーション機能に加えて、アニメーションの時間とアニメーション曲線を指定するための新しい Api も提供します。 Android 5.0 では、これらの曲線を使用して、アニメーションの中に時間と空間的移動を補間します。 Android 5.0 では、3 つの曲線が定義されています。

-   **高速\_アウト\_線形\_で**&ndash;短時間を短縮し、アニメーションの終了まで高速化し続けます。

-   **高速\_アウト\_低速\_で** &ndash; Accelerates 緩やかに変化し、すばやく減速のアニメーションの終了するころにします。

-   **線形\_アウト\_低速\_で**&ndash;ピーク velocity の使用と緩やかに変化値で始まる減速するアニメーションの終了。

新たに使用することができます`PathInterpolator`クラスがアニメーションの補間が行われる方法を指定します。 `PathInterpolator` 指定した制御点とアニメーション曲線に従ってアニメーション パスを通過する補間です。 Android 5.0 の曲線のモーションの設定を指定する方法の詳細については、[使用曲線モーション](https://developer.android.com/training/material/animations.html#CurvedMotion)を参照してください。


## <a name="view-shadows--elevation"></a>昇格 (&)、ビューのシャドウ

Android 5.0 で指定できます、*昇格*をビューで、新しい設定の`Z`プロパティ。 大きい`Z`により、float 型の高い、背景の上に表示されるビューを作成、バック グラウンドで大きいシャドウをキャストするビューが値。 初期ビューの昇格を設定するには、構成をその`elevation`レイアウト内の属性。

次の例は、空でキャスト shadows`TextView`昇格属性が設定されている場合 2dp、4dp、および 6dp、それぞれを制御します。

[![Progessively より大きなビューの影のスクリーン ショット](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

シャドウ設定の表示 (前述のように) 静的またはビュー、ビューの背景の上の一時的に増加し表示するアニメーションで使用できます。 使用することができます、`ViewPropertyAnimator`ビューの昇格をアニメーション化するクラス。 ビューの昇格がそのレイアウトの合計`elevation`設定と`translationZ`プロパティを使用して設定することができます、`ViewPropertyAnimator`メソッドの呼び出し。

詳細については、ビュー shadows Android 5.0 では、[影の定義、クリッピング ビュー](https://developer.android.com/training/material/shadows-clipping.html)を参照してください。


## <a name="color-features"></a>色の機能

Android 5.0 は、アプリの色を管理するための 2 つの新しい機能を提供します。

-   *Drawable 濃淡*レイアウト属性を変更することで、イメージの資産の色を変更することができます。

-   *目立つ色抽出*を動的に表示されるイメージのカラー パレットと協力して、アプリの配色テーマをカスタマイズすることが可能になります。


### <a name="drawable-tinting"></a>Drawable 色合いを付けること

Android 5.0 レイアウト認識、新しい`tint`表示が異なる色にこれらの資産の複数のバージョンを作成することがなくドローアブルの色を設定するために使用できる属性。 この機能を使用するアルファ マスクとして使用して、ビットマップを定義する、`tint`資産の色を定義する属性。 これにより、資産を 1 回作成し、レイアウト、テーマと一致する色にすることが可能にします。

次の例では、1 つのイメージ資産&ndash;透明の背景に白いロゴ&ndash;濃淡のバリエーションを作成するために使用します。

![白の背景が透明な Xamarin ロゴ](lollipop-images/xamarin-logo-white.png)

次の例に示すように、青い円形の背景上にこのロゴが表示されます。 左側のイメージはせず、ロゴを表示する方法、`tint`設定します。 センターの図で、ロゴの`tint`属性が濃い灰色に設定します。 右上のイメージで`tint`薄い灰色に設定されています。

![濃淡の異なる設定で、上記のロゴの例](lollipop-images/drawable-tinting.png)

詳細については、Android 5.0 drawable 色合いを付けることを参照してください。 [Drawable 色合いを付けること](https://developer.android.com/training/material/drawables.html#DrawableTint)します。


### <a name="prominent-color-extraction"></a>目立つ色の抽出

新しい Android 5.0`Palette`クラスでは、カスタムの色パレットに動的に適用できるように、イメージから色を抽出することができます。 `Palette`クラスは、イメージから 6 つの色を抽出し、ラベルの色の彩度と輝度の相対的なレベルに従ってこれらの色。

-   活気のあります。

-   活気のある濃い

-   活気のある光

-   ミュート

-   ミュート濃色

-   ライトをミュート

たとえば、次のスクリーン ショットで、アプリを表示する写真は表示上のイメージから目立つ色を抽出し、これらの色を使用して、イメージと一致するアプリの配色を適応させる。

[![緑、ピンク色、および青のテーマの色の抽出のスクリーン ショット](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

上記のスクリーン ショットでは、操作バーが抽出された「活気のある光」に設定されてを抽出した「活気のある濃い」設定の色と背景色。 各上記の例では、小規模な色の正方形の行を画像から抽出されたパレットの色を示すために含まれています。

詳細については、Android 5.0 での色の抽出は、[イメージからの抽出の目立つ色](https://developer.android.com/training/material/drawables.html#ColorExtract)を参照してください。


## <a name="new-ui-widgets"></a>新しい UI ウィジェット

Android 5.0 には、2 つの新しい UI ウィジェットが導入されています。

-   `RecyclerView` &ndash; スクロール可能な項目の一覧を表示するビューのグループ。

-   `CardView` &ndash; 角の丸みのある基本的なレイアウトです。

両方のウィジェット素材のテーマ機能は組み込みのサポートは含まれませんたとえば、`RecyclerView`ビューを追加および削除のアニメーションを使用してと`CardView`はバック グラウンド上にフローティングする表示される各カードに影を表示します。 これらの新しいウィジェットの例は次のスクリーン ショットに示します。

[![RecyclerView でビルドされたアプリのスクリーン ショット](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

左側のスクリーン ショットの例に示します`RecyclerView`の例は上のスクリーン ショットし、電子メール アプリで使用される`CardView`旅行の予約アプリで使用するためです。


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` ような`ListView,`が、これは、ビューまたは動的に変更する要素を使用したリストのセットが大きい場合に適しています。 ような`ListView,`基になるデータ セットにアクセスするためのアダプターを指定します。 ただしとは異なり`ListView,`を使用する、*レイアウト マネージャー*内のアイテムを配置する`RecyclerView`します。 レイアウト マネージャーも処理のビューがリサイクルされます。ユーザーに表示されなくなった項目ビューの再利用を管理します。

使用すると、`RecyclerView`指定する必要があります、ウィジェット、`LayoutManager`とアダプター。 この図で示すように`LayoutManager`アダプター間の仲介者は、および`RecyclerView`:

![LayoutManager、アダプター、およびデータセットをサポートするいると RecyclerView のダイアグラム](lollipop-images/recyclerview-diagram.png)

次のスクリーン ショットを示しています、 `RecyclerView` 100 項目を格納している (各項目から成る、`ImageView`と`TextView`)。

[![イメージをスクロールして RecyclerView アプリのスクリーン ショット](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` この大規模なデータ セットの処理を簡単に&ndash;を終了するリストの先頭からスクロール アプリでこのサンプルの一覧のいくつか数秒で済みます。 `RecyclerView` アニメーション; もサポートします。実際には、アニメーションを追加して、項目を削除するのには既定で有効にします。 項目を追加するときに、 `RecyclerView`、この一連のスクリーン ショットに示すように消えてしまう。

[![フレームの写真アイテム フェードの枠のスクリーン ショット](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

詳細については`RecyclerView`を参照してください[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)します。


### <a name="cardview"></a>CardView

`CardView` 角が丸い浮動カードをシミュレートする単純なビューです。 `CardView`組み込みビューの影の付いた、ビジュアルの深さをアプリに追加するための簡単な方法を提供します。 次のスクリーン ショットに表示する 3 つの例のテキスト指向`CardView`:

[![RecyclerView を CardView ベースの項目を使用してアプリのスクリーン ショットの例](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

各カード上の例が含まれています、 `TextView`; を使用して背景色を設定、`cardBackgroundColor`属性。

詳細については`CardView`を参照してください[CardView](~/android/user-interface/controls/card-view.md)します。


## <a name="enhanced-notifications"></a>強化された通知

Android 5.0 で通知システムを新しいビジュアルな形式と新機能が大幅に更新されました。 通知では、Android 5.0 で新しい外観があります。 たとえば、Android 5.0 での通知が明るい背景に濃いテキストを使用するようになりました。

![Android 5.0 の通知が展開解除の例](lollipop-images/expanded-notification-contracted.png)

大きいアイコン (、上記の例で示す) の通知が表示されたら、Android 5.0 は、大きなアイコンの上、バッジとして小さいアイコンを表示します。 

Android 5.0 は、デバイス ロック画面に通知できますも表示されます。
たとえば、1 つの通知をロック画面のスクリーン ショットの例を次に示します。

[![ロック画面に表示される通知のスクリーン ショット](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

ユーザーはダブルタップして、デバイスのロック解除して、通知が送信されたアプリにジャンプするロック画面で通知をスワイプして通知を破棄するか。 通知がある新しい*可視性*設定、ロック画面に表示できるコンテンツの量を決定します。 ユーザーは、ロック画面通知に表示される機密性の高いコンテンツを許可するかどうかを選択できます。

Android 5.0 と呼ばれる新しい優先度の高い通知プレゼンテーション形式が導入されています*ヘッドアップ*します。 ヘッドアップ通知は数秒を画面の一番上から下へスライドし、漂い、画面の上部にある通知の網掛けに戻る。 ヘッドアップ通知では、システムの UI を現在実行中のアクティビティを中断することがなく、ユーザーの前に重要な情報を配置できます。 次の例は、アプリの上に表示する単純なヘッドアップ通知を示しています。

[![ヘッドアップ通知の例](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

ヘッドアップ通知は通常、次のイベントに対して使用されます。

-   次の新しいメッセージ

-   着信呼び出し

-   バッテリを示す値

-   アラーム

Android 5.0 では、高または最大の優先度設定がある場合にのみ、ヘッドアップ形式で通知が表示されます。

Android 5.0 は、Android を並べ替えるしよりインテリジェントな通知を表示する通知のメタデータを提供できます。 Android 5.0 は、優先度、可視性、およびカテゴリに従って通知を整理します。
通知カテゴリがフィルター処理に使用されて、デバイスがときに通知を表示する*不可*モード。

作成して、Android 5.0 の最新の機能を使用した通知を起動する方法の詳細についてを参照してください。[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)します。


## <a name="new-apis"></a>新しい API

上記で説明した外観と雰囲気の新機能、に加えては、Android 5.0 は、既存のマルチ メディアの機能、ストレージ、およびワイヤレス接続または機能を拡張する新しい Api を追加します。 また、Android 5.0 には、新しいジョブ スケジューラ機能のサポートを提供する新しい Api が含まれています。

### <a name="camera"></a>カメラ

Android 5.0 は、カメラの拡張機能のいくつかの新しい Api を提供します。 新しい`Android.Hardware.Camera2`名前空間に Android デバイスに接続されたカメラの個々 のデバイスへのアクセス機能が含まれます。 また、`Android.Hardware.Camera2`パイプラインとして各カメラ デバイスをモデル化: キャプチャの要求を受け入れのイメージをキャプチャし、結果を出力します。 このアプローチを使うと向けカメラ デバイスに複数のキャプチャ要求をキューに登録できます。

次の Api は、可能なこれらの新機能を作成します。

-   `CameraManager.GetCameraIdList` &ndash; カメラのデバイスをプログラムでアクセスできます。使用する`CameraManager.OpenCamera`カメラを特定のデバイスに接続します。

-   `CameraCaptureSession` &ndash; キャプチャまたはカメラ デバイスから画像をストリームします。 実装する、`CameraCaptureSession.CaptureListener`新しいイメージのキャプチャのイベントを処理するインターフェイス。

-   `CaptureRequest` &ndash; キャプチャのパラメーターを定義します。

-   `CaptureResult` &ndash; イメージのキャプチャ操作の結果を提供します。

詳細については、新しいカメラ Api を Android 5.0 では、[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)を参照してください。

### <a name="audio-playback"></a>オーディオの再生

Android 5.0 の更新プログラム、`AudioTrack`向上のオーディオ再生のクラス。

-   `ENCODING_PCM_FLOAT` &ndash; 構成`AudioTrack`ダイナミック レンジの向上、大きいヘッドルーム、および (精度が高まること) により高い品質の浮動小数点形式でのオーディオ データを受け入れるようにします。 また、浮動小数点形式では、オーディオ クリップを回避するのに役立ちます。

-   `ByteBuffer` &ndash; オーディオ データは、ここで、`AudioTrack`としてバイト配列。

-   `WRITE_NON_BLOCKING` &ndash; このオプションは、バッファリングを簡略化し、一部のアプリのマルチ スレッドです。

詳細については`AudioTrack`Android 5.0 は、の機能強化を参照してください[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)します。

### <a name="media-playback-control"></a>メディア再生コントロール

Android 5.0 が導入されていますが、新しい`Android.Media.MediaController`クラス、後継`RemoteControlClient`します。 `Android.Media.MediaController` 簡略化された転送コントロール Api を提供し、UI コンテキスト外での再生のスレッド セーフな制御を提供します。 次の新しい Api は、トランスポート コントロールを処理します。

-   `Android.Media.Session.MediaSession` &ndash; メディアは、複数のコント ローラーを処理するセッションを制御します。 呼び出す`MediaSession.GetSessionToken`アプリを使用して、セッションで対話するトークンを要求します。

-   `MediaController.TransportControls` &ndash; などのコマンドでは、トランスポート**再生**、**停止**、および**スキップ**します。

また、使用する新しい`Android.App.Notification.MediaStyle`リッチな通知のコンテンツ (など、抽出およびアルバム アートを表示) とメディア セッションを関連付けるクラス。

詳細については、Android 5.0 で新しいメディア再生コントロール機能は、[メディア](https://developer.android.com/about/versions/android-5.0.html#Media)を参照してください。

### <a name="storage"></a>記憶域

Android 5.0 は、アプリケーション ディレクトリとドキュメント操作を容易に記憶域アクセス フレームワークを更新します。

-   ディレクトリ サブツリーを選択するには、ビルドし、送信、`Android.Intent.Action.OPEN_DOCUMENT_TREE`インテントです。 この目的により、システムは、サブツリーの選択をサポートするすべてのプロバイダー インスタンスを表示するにはユーザーを参照し、ディレクトリを選択します。

-   作成して、新しいドキュメントまたはサブツリーで任意の場所のディレクトリの管理を使用する新しい`CreateDocument`、 `RenameDocument`、および`DeleteDocument`メソッドの`DocumentsContract`します。

-   新しい呼び出しをすべての共有記憶域デバイスでメディアのディレクトリへのパスを取得する`Android.Content.Context.GetExternalMediaDirs`メソッド。

詳細については、新しいストレージ Api を Android 5.0 では、[ストレージ](https://developer.android.com/preview/api-overview.html#Storage)を参照してください。

### <a name="wireless--connectivity"></a>ワイヤレスと接続性

Android 5.0 には、ワイヤレスと接続に関する次の API の機能強化が追加されます。

-   新しい*複数のネットワーク*Api アプリを検索して接続を行う前に特定の機能を持つネットワークを選択できるようにします。

-   Bluetooth ブロードキャスト低電力の Bluetooth 周辺機器として動作する Android 5.0 デバイスをできるようにする機能。

-   NFC の機能強化を容易には、他のデバイスとデータを共有する近距離通信機能を使用します。

詳細については、新しいワイヤレスおよび接続 Api を Android 5.0 では、[ワイヤレスおよび接続](https://developer.android.com/preview/api-overview.html#Wireless)を参照してください。

### <a name="job-scheduling"></a>ジョブのスケジュール設定

Android 5.0 が導入されていますが、新しい`JobScheduler`ユーザーに役立つ API、デバイスが接続されている場合にのみ実行する特定のタスクをスケジュール設定と充電中、バッテリの消耗を最小限に抑えます。 このジョブ スケジューラ機能は、条件は、デバイスが従量制課金接続ではなく、Wi-fi ネットワーク経由で接続されているときに大きなファイルのダウンロードなど、そのタスクをより適切なときに実行するタスクのスケジュールにも使用できます。

詳細については、Android 5.0 での Api のスケジュール設定、新しいジョブは、[ジョブをスケジュール](https://developer.android.com/preview/api-overview.html#JobScheduler)を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Android アプリ開発者のため、Android 5.0 で重要な新機能の概要を提供します。

-   マテリアル テーマ

-   Animations

-   ビューのシャドウと昇格

-   色の機能、drawable 色合いを付けることなど、目立つ色の抽出

-   新しい`RecyclerView`と`CardView`ウィジェット

-   通知の機能強化

-   カメラ、オーディオの再生、メディア コントロール、ストレージ、ワイヤレス/接続、およびジョブのスケジュール設定用の新しい Api

Xamarin Android 開発に慣れていない場合は、読み取る[セットアップとインストール](~/android/get-started/installation/index.md)に Xamarin.Android で作業を開始できます。
[こんにちは, Android](~/android/get-started/hello-android/index.md) Android プロジェクトを作成する方法を学習するための役立つ入門書です。



## <a name="related-links"></a>関連リンク

- [Android の L Developer Preview](https://developer.android.com/preview/index.html)
- [Android SDK を入手します。](https://developer.android.com/sdk/index.html#Other)
- [素材のデザイン](https://developer.android.com/preview/material/index.html)
- [素材のデザインの原則](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
