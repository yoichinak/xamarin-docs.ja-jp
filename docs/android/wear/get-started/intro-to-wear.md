---
title: "Android の消耗の概要"
description: "Google の Android 着用の導入に伴い、する制限がなくなりました電話やタブレットは単に優れた Android アプリを開発する際にします。 Android を着用 Xamarin.Android のサポートできるようになります手首に c# コードを実行するためです。 この概要は Android 着用の基本的な概要については、主な機能について説明します、および Android 着用 2.0 で使用できる機能の概要を提供します。 いくつかの人気のある Android 着用デバイスが一覧表示し、さらに詳細不可欠な Google Android 着用ドキュメントへのリンクを提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c334e78793f90b4f349f87e12e6b0093fe5cacf8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-android-wear"></a>Android の消耗の概要

_Google の Android 着用の導入に伴い、する制限がなくなりました電話やタブレットは単に優れた Android アプリを開発する際にします。Android を着用 Xamarin.Android のサポートできるようになります手首に c# コードを実行するためです。この概要は Android 着用の基本的な概要については、主な機能について説明します、および Android 着用 2.0 で使用できる機能の概要を提供します。いくつかの人気のある Android 着用デバイスが一覧表示し、さらに詳細不可欠な Google Android 着用ドキュメントへのリンクを提供します。_

<a name="overview" />

## <a name="overview"></a>概要

Android の損傷は、さまざまなデバイス、第 1 世代 Motorola 360、LG の G ウォッチ、Samsung 歯車 Live などで実行されます。 組み込みの GPS およびオフラインの音楽の再生などの他の機能で、Sony の SmartWatch 3 を含む、第 2 世代もリリースされました。 For Android を着用の 2.0 では、Google と提携して lg その他の 2 つの新しいウォッチ: LG ウォッチ スポーツおよび LG ウォッチ スタイル。

![Android 着用 2.0 デバイス](intro-to-wear-images/hero-image.png "Android 着用 2.0 デバイスの例")

Android 着用 Xamarin.Android 5.0 以降サポート、Android 4.4W (API 20) をサポートし、追加を追加する NuGet パッケージの消耗に固有の UI コントロールします。 Xamarin.Android 5.0 以降では、損傷のアプリをパッケージ化するための機能も含まれます。 NuGet パッケージの管理も Android 着用 2.0 このガイドで後述するように使用できます。


<a name="basics" />

## <a name="android-wear-basics"></a>Android の消耗の基礎

Android の損傷では、ユーザー インターフェイスのパラダイム異なっているハンドヘルドに Android アプリのことがあります。 コンパニオンを拡張するようデザインされている、第一陣消耗アプリのいくつかの方法が Android 消耗 2.0 で始まるハンドヘルド アプリ、アプリの消耗、単体で使用されるを指定できます。 消耗アプリを展開するときに、コンパニオン ハンドヘルド アプリとパッケージされています。 アプリは、ハンドヘルド コンパニオン アプリによって異なります。 ほとんど着用ため、必要ハンドヘルド アプリと通信する方法にします。 次のセクションは、これら使用量シナリオについて説明し、重要な Android 着用機能の概要です。 


<a name="scenarios" />

### <a name="usage-scenarios"></a>使用シナリオ

Android を着用の最初のバージョンが強化された通知で現在のハンドヘルド アプリケーションの拡張とハンドヘルドのアプリとウェアラブル アプリ間でデータを同期する、主に重点を置きます。 したがって、これらのシナリオは比較的簡単に実装します。

<a name="notifications" />

#### <a name="wearable-notifications"></a>ウェアラブル通知

Android を着用をサポートする最も簡単な方法は、ハンドヘルドとウェアラブル デバイス間での通知の共有の性質を活用するためにです。 サポート v4 通知 API を使用して、`WearableExtender`クラス (で利用可能な[Xamarin Android サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/))、受信トレイ スタイルのカードと同様に、プラットフォームのネイティブ機能を活用したり、音声入力します。 [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/)サンプルは Android 着用デバイスに通知の一覧を送信する方法を示すコード例を提供します。 


<a name="companion" />

#### <a name="companion-applications"></a>コンパニオン アプリケーション

別の方法では、ウェアラブル デバイスでネイティブに実行し、必携ハンドヘルド アプリとのペアを完全なアプリケーションを作成します。 このアプローチの好例が、[クイズ](https://developer.xamarin.com/samples/monodroid/wear/Quiz/)サンプル アプリは、ハンドヘルド デバイスで実行され、ウェアラブル デバイスで、クイズを質問するクイズを作成する方法を示します。 


<a name="ui" />

### <a name="user-interface"></a>ユーザー インターフェイス

損傷のプライマリ ナビゲーション パターンは、一連の垂直方向に整列カードです。 これらのカードの各を関連付けることが、同じ行に対して、アウト階層化されているアクション。 `GridViewPager`クラスには、この機能が用意されています; と同じアダプターの概念に従って`ListView`です。 一般に関連付ける、`GridViewPager`で、 `FragmentGridPagerAdaptor` (または`GridPagerAdaptor`) を可能にする各の行と列のセルとして表す、 `Fragment`: 

[ ![ナビゲーションを着用](intro-to-wear-images/2d-picker-sml.png "ナビゲーションを着用")](intro-to-wear-images/2d-picker.png)

大規模なので構成されている動作設定ボタンの使用例のように上) の下にある小さな説明テキストを含む円の色を着用もします。  [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)サンプルを使用する方法を示します`GridViewPager`と`GridPagerAdapter`消耗アプリでします。

Android 着用 2.0 は、損傷のユーザー インターフェイスにナビゲーション ドロアー、アクションのドロアーおよびインライン アクション ボタンを追加します。 Android 着用 2.0 ユーザー インターフェイス要素の詳細を参照してください、Android [Anatomy](https://www.google.com/design/spec-wear/system-overview/anatomy.html)トピックです。 


<a name="comm" />

### <a name="communications"></a>通信

Android の損傷では、次の 2 つの異なる通信ウェアラブル アプリとコンパニオン ハンドヘルド アプリ間の通信を容易にするための Api を提供します。 

**データ API** &ndash;この API は、ウェアラブル デバイスとハンドヘルド デバイス間の同期データ ストアに似ています。 これを行うには最適であるときに、ウェアラブルとハンドヘルドの間で変更を反映する android 行われます。 ウェアラブルが範囲外にある場合は、同期は後のキューに入れます。 この API に対するメイン エントリ ポイントは`WearableClass.DataApi`します。 この API の詳細については、Android を参照してください。[データ項目の同期](https://developer.android.com/training/wearables/data-layer/data-items.html)トピックです。 

**メッセージの API** &ndash;この API により、下位レベルの通信パスを使用する: 小さなペイロードは、ハンドヘルドおよびウェアラブル アプリとの間に同期化せず一方向に送信します。
この API に対するメイン エントリ ポイントは`WearableClass.MessageApi`します。
この API の詳細については、Android を参照してください。[受信メッセージを送信して](https://developer.android.com/training/wearables/data-layer/messages.html)トピックです。

各 API リスナー インターフェイスを使用してそれらのメッセージを受信するためのコールバックを登録または、代わりに、派生元となるアプリでサービスを実装することもできます`WearableListenerService`です。
このサービスは、Android を着用によって自動的にインスタンス化されます。
[FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/)サンプルを実装する方法を示します、`WearableListenerService`です。


<a name="deploy" />

### <a name="deployment"></a>配置

各ウェアラブル アプリは、メイン アプリケーション APK 内に埋め込まれた独自の APK ファイルと共に展開されます。 このパッケージは Xamarin.Android 5.0 以降では、自動的に処理されますが、Xamarin.Android バージョン 5.0 よりも前のバージョンを手動で実行する必要があります。 
[パッケージの操作](~/android/wear/deploy-test/packaging.md)詳細での展開について説明します。 


<a name="further" />

## <a name="going-further"></a>理解を深める 

Android を着用習熟する最善の方法をビルドして最初のアプリをテストします。 速度を迅速に取得するための推奨される読み取り順序を次に示します。

1.  [セットアップとインストール](~/android/wear/get-started/installation.md)インストールおよび Xamarin.Android 消耗アプリを構築するための開発環境を構成する詳細な手順について説明します。 

2.  後に必要なパッケージをインストールして、エミュレーターまたはデバイスを構成するを参照してください[ハロー, 着用](~/android/wear/get-started/hello-wear.md)をそのハンドル ボタン小規模な Android 着用プロジェクトを作成する方法を説明する詳しい手順については、クリックすると表示、。消耗デバイス上のカウンターをクリックします。 

3.  [配置およびテスト](~/android/wear/deploy-test/index.md)を構成して、エミュレーターおよび Bluetooth 経由での消耗デバイスにアプリを展開する方法を手順など、デバイスへの展開に関する詳細情報を提供します。

4.  [画面サイズと作業](~/android/wear/screen-sizes.md)をプレビューし、ユーザー インターフェイスの消耗デバイスでさまざまな使用可能な画面サイズを最適化する方法について説明します。 

5.  [パッケージの操作](~/android/wear/deploy-test/packaging.md)Google Play に配布するの消耗アプリを手動でパッケージ化する手順について説明します。

最初の消耗アプリを作成した後、カスタム ウォッチの文字盤を着用して Android のビルドを再試行してをする可能性があります。 
[ウォッチの文字盤を作成する](~/android/wear/platform/creating-a-watchface.md)アナログ スタイル ウォッチの文字盤追加の拡張機能を拡張するコードを続けて、デジタル ウォッチ フェイス サービスをストリップを開発するための手順とコード例を提供します。 


<a name="wear2" />

## <a name="android-wear-20"></a>Android の消耗 2.0

Android 着用 2.0 が導入されています、さまざまな新機能と、機能など、*複雑*、曲線のレイアウト、ナビゲーションとアクションの引き出し、および展開された通知。 また、着用 2.0 によりハンドヘルド アプリから独立して動作するスタンドアロンのアプリを構築するためのできます。 新しい*手首ジェスチャ*機能により、アプリと共に one-handed 相互作用します。 次のセクションでは、これらの機能を強調表示し、アプリでそれらの使用を開始するのに役立つリンクを提供します。


<a name="install2" />

### <a name="install-wear-20-packages"></a>インストールを着用 2.0 パッケージ

Xamarin.Android を着用 2.0 アプリケーションをビルドするには、追加する必要があります、 **Xamarin.Android.Wear v2.0**をプロジェクトにパッケージ (をクリックして、 **[参照] タブ**)。

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin.Android.Wear v2.0 NuGet のインストール")](intro-to-wear-images/wear-nuget-2.0.png)

この NuGet パッケージには、Android サポート ウェアラブルおよびの両方を着用 Compat ライブラリのバインドが含まれています。

加え**Xamarin.Android.Wear**をインストールすることをお勧め、 **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Install the Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png)

<a name="wear2feat" />

### <a name="key-features-of-wear-20"></a>主な機能を着用 2.0

Android 着用 2.0 では、Android 身に最も大きな更新プログラムは 2014年でその初期起動以後です。 次のセクションでは、Android の着用 2.0 の主な機能を強調表示し、ヘルプを示したリンクが、アプリでこれらの新機能の使用を開始します。 

<a name="compl" />

#### <a name="complications"></a>複雑な問題

*複雑さの一部*小規模のウォッチをウォッチの文字盤をスワイプしなくても、一目でわかりますフェイス ウィジェットはします。 複雑さの一部がデスクトップ スタイルのダッシュ ボードのウィジェットに似ています。天気、バッテリの寿命、予定表イベント、およびアプリ統計情報の適合性などの情報を表示します。 

![複雑さの一部の例](intro-to-wear-images/complications.png "複雑さの一部の例")

詳細については、複雑さの一部を参照してください、Android[ウォッチ フェイス複雑さの一部](https://developer.android.com/wear/preview/features/complications.html)トピックです。 


<a name="drawers" />

#### <a name="navigation-and-action-drawers"></a>ナビゲーションおよびアクションの引き出し 

着用 2.0 では、次の 2 つの新しい引き出しが含まれています。 *ナビゲーション ドロアー*画面の上部に表示されることができます (示すように左下の) アプリのビュー間を移動します。 *アクション ドロアー*アクションの一覧から選択できます (示すように、右側)、画面の下部に表示されます。 

![ナビゲーションおよびアクションの引き出し](intro-to-wear-images/drawers.png "ナビゲーションとアクションの引き出し")

これら 2 つの新しい対話型引き出しの詳細については、Android を参照してください。[着用ナビゲーションとアクション](https://developer.android.com/wear/preview/features/ui-nav-actions.html)トピックです。 


<a name="curved" />

#### <a name="curved-layouts"></a>曲線のレイアウト 

消耗 2.0 では、円形の消耗デバイスの曲線のレイアウトを表示するための新機能について説明します。 具体的には、新しい`WearableRecyclerView`round 表示を垂直方向の項目の一覧を表示するクラスを最適化します。 

![曲線のレイアウトの例](intro-to-wear-images/curved-layout.png "曲線のレイアウトの例")

`WearableRecyclerView` 拡張、`RecyclerView`曲線のレイアウトと循環のスクロール ジェスチャをサポートするクラス。 詳細については、Android を参照してください。 [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API のドキュメントです。 


<a name="standalone" />

#### <a name="standalone-apps"></a>スタンドアロンのアプリ 

Android アプリを着用 2.0 は、ハンドヘルド アプリとは独立して作業できます。 つまり、たとえば、スマート ウォッチ続行できますコンパニオン ハンドヘルド デバイスのオン/オフ遠く離れたウェアラブル デバイスから場合でも、完全な機能を提供します。 この機能の詳細については、Android を参照してください。[スタンドアロン アプリ](https://developer.android.com/wear/preview/features/standalone-apps.html)トピックです。


<a name="wrist" />

#### <a name="wrist-gestures"></a>手首ジェスチャ 

手首ジェスチャを使うと、タッチ スクリーンを使用せずにアプリと対話するユーザーの&ndash;ユーザーが 1 つ手の形を使用してアプリに対応できます。 2 つの手首ジェスチャがサポートされています。 

-   Out フリック手首
-   フリック手首

詳細については、Android を参照してください。[手首ジェスチャ](https://developer.android.com/wear/preview/features/gestures.html)トピックです。 


インライン アクション、スマート応答、リモートの入力、展開された通知、および通知の新しいブリッジ モードなどの多数の着用 2.0 機能があります。 着用 2.0 の新機能に関する詳細については、Android を参照してください。 [API の概要](https://developer.android.com/wear/preview/api-overview.html)です。 


<a name="devices" />

## <a name="devices"></a>デバイス

Android 着用を実行しているデバイスのいくつかの例を次に示します。

* [Motorola 360](https://moto360.motorola.com/)
* [LG G Watch](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G ウォッチ R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung Gear Live](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASU ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)


<a name="reading" />

## <a name="further-reading"></a>関連項目

Google の Android 着用ドキュメントをご覧ください。

* [Android の消耗に関する](http://www.android.com/wear/)
* [Android の消耗アプリのデザイン](https://developer.android.com/design/wear/index.html)
* [android.support.wearable ライブラリ ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android の消耗 2.0](https://developer.android.com/wear/preview/index.html)


<a name="summary" />

## <a name="summary"></a>まとめ

この概要では、Android 着用の概要を説明します。 Android 着用の基本的な機能を説明し、Android 着用 2.0 で導入された機能の概要が含まれています。 Xamarin.Android 消耗開発の概要を開発者に役立つ重要な読み取りへのリンクを提供し、Android を着用現在市場にデバイスの一部の例が表示されています。


## <a name="related-links"></a>関連リンク

- [インストールとセットアップ](~/android/wear/get-started/installation.md)
- [はじめに](~/android/wear/get-started/index.md)
