---
title: Android Wear
description: Android ウェアラブルデバイス用のアプリを構築しています。
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: 5768a8fb402304d399e619ddb111aea24d975daf
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758326"
---
# <a name="android-wear"></a>Android Wear

Android Wear は、スマートウォッチなどのウェアラブルデバイス向けに設計された Android のバージョンです。 このセクションでは、Wear の開発に必要なツールをインストールして構成する方法、最初の Wear デバイスを作成するための手順に関するチュートリアル、および独自の Wear アプリを作成するために参照できるサンプルの一覧について説明します。

## <a name="getting-startedandroidwearget-startedindexmd"></a>[はじめに](~/android/wear/get-started/index.md)

Android Wear について説明します。コンピューターをインストールして、Wear 開発用に構成する方法について説明します。また、最初の Android Wear アプリをエミュレーターまたは Wear デバイスで作成して実行するための手順について説明します。

## <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[ユーザー インターフェイス](~/android/wear/user-interface/index.md)

Android Wear に固有のコントロールについて説明し、これらのコントロールの使用方法を示すサンプルへのリンクを示します。

## <a name="platform-featuresandroidwearplatformindexmd"></a>[プラットフォーム機能](~/android/wear/platform/index.md)

このセクションのドキュメントでは、Android Wear に固有の機能について説明します。 ここでは、WatchFace を作成する方法について説明するトピックを紹介します。

## <a name="screen-sizesandroidwearscreen-sizesmd"></a>[画面サイズ](~/android/wear/screen-sizes.md)

使用可能な画面サイズに合わせてユーザーインターフェイスをプレビューし、最適化します。

## <a name="deployment--testingandroidweardeploy-testindexmd"></a>[配置とテスト](~/android/wear/deploy-test/index.md)

Android Wear デバイス、または Wear 用に構成された Android emulator に Android Wear アプリをデプロイする方法について説明します。 また、開発用コンピューターと Android デバイスとの間に Bluetooth 接続を設定する方法についても、デバッグのヒントと情報が含まれています。

## <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Wear Api](https://developer.android.com/reference/android/support/wearable)

Android 開発者サイトでは、[ウェアラブルアクティビティ](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html)、[インテント](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html)、[認証](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html)、[複雑さ](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html)、[複雑さの表示](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html)、[通知](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html)[など、主要な Wear api に関する詳細情報を提供しています。ビュー](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)と[WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html)。

## <a name="samples"></a>サンプル

Android Wear (または[github](https://github.com/xamarin/monodroid-samples/tree/master/wear)に直接アクセス) を使用して、多数の[サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear)を見つけることができます。

|サンプル|説明|スクリーン ショット|
|--- |--- |--- |
|[SkeletonWear](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)|GridViewPager や対話型通知など、ウェアラブルプロジェクトの基本の簡単な例です。|![Skeletonwear のスクリーンショット](images/skeleton.png)|
|[WatchViewStub](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub)|画面の形状を検出し、正しいレイアウトを自動的に読み込む WatchViewStub コントロールの簡単なデモ。 **Resources/layout/main_activity**レイアウトで WatchViewStub がどのように機能するかを確認します。|![WatchViewStub のスクリーンショット](images/watchview.png)|
|[RecipeAssistant](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant)|レシピの手順の形式での、Wear 通知ページのデモンストレーション。 通知は RecipeService.cs に作成されます。|![RecipeAssistant のスクリーンショット](images/recipeassist.png)|
|[ElizaChat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-elizachat)|Eliza と呼ばれる "パーソナルアシスタント" と対話する楽しいサンプル。Wear の対話型通知を使用して、事前に確立された応答を使用してメッセージ交換を作成します。|![リストの登録のスクリーンショット](images/eliza.png)|
|[GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)|GridViewPager は2D ナビゲーションパターンを実装します。このパターンでは、ユーザーは垂直方向にスワイプし、水平方向に移動して、オプションやコンテンツを移動できます。|![GridViewPager のスクリーンショット](images/gridviewpager.png)|
|[WatchFace](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)|WatchFace は、時間、分、および秒のアナログスタイルを使用したカスタムウォッチフェイスです。 このサンプルでは、現在の時刻を描画し、アンビエントモードと可視性の変更イベントを処理する watch face サービスを作成する方法を示します。 これには、タイムゾーンの変更をリッスンし、それに応じて自動的に時刻を更新するブロードキャストレシーバーが含まれます。|![WatchFace のスクリーンショット](images/gridviewpager.png)|

## <a name="videos"></a>ビデオ

以下のビデオリンクをご確認ください。

|説明|スクリーン ショット|
|--- |--- |
|[Android L など](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/)&ndash; Android L Developer Preview では、多くの開発者向けに大量の新しい api が導入されました。これには、素材のデザイン、通知、新しいアニメーションなどを利用して、いくつかの名前を指定できます。|![プレゼンテーションのビデオスクリーンショット](images/video-android-l.png)|
|[C#耳の中にあります。Google グラスと Android](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash;のウェアラブルコンピューティングは、将来のもの (またはインスペクターガジェットエピソード) のように見えますが、多くの人が既に将来を採用しています。 C#開発者はこれを把握しており、ウェアラブルデバイスの力を活用するためのツールとスキルが既に用意されています (2014 の進化から)。|![プレゼンテーションのビデオスクリーンショット](images/video-eyes-ears.png)|
|[Xamarin Android の新機能](https://www.youtube.com/watch?v=Gpqc2XZIQfU)&ndash; Android L、android Wear、android TV、android Auto、マテリアル設計、および ART は、2014の進化から、Xamarin の開発者として何を意味していますか。|![プレゼンテーションのビデオスクリーンショット](Images/video-whats-new.png)|

<!--

March 18
https://blog.xamarin.com/android-wear/

August 14
https://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
https://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
