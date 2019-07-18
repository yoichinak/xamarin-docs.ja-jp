---
title: Android Wear
description: Android のウェアラブル デバイス用のアプリを構築します。
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: fca72291dd726d4f2a6635d26390baa103ee0d2d
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864901"
---
# <a name="android-wear"></a>Android Wear

Android Wear、スマート ウォッチなど、ウェアラブル デバイス用に設計された Android のバージョンです。 このセクションをインストールして、最初の Wear デバイスと、独自に作成するための Wear アプリを参照できるサンプルの一覧を作成するためのステップ バイ ステップ チュートリアル、Wear の開発に必要なツールを構成する方法の手順を説明します。

## <a name="getting-startedandroidwearget-startedindexmd"></a>[はじめに](~/android/wear/get-started/index.md)

Android Wear が導入されています。 インストールして Wear の開発用にコンピューターを構成する方法について説明します、作成し、エミュレーターまたは Wear デバイスで最初の Android Wear アプリを実行する手順を示します。

## <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[ユーザー インターフェイス](~/android/wear/user-interface/index.md)

Android Wear 固有が制御し、これらのコントロールを使用する方法を示すサンプルへのリンクを提供するについて説明します。

## <a name="platform-featuresandroidwearplatformindexmd"></a>[プラットフォーム機能](~/android/wear/platform/index.md)

このセクションのドキュメントでは、Android Wear に固有の機能について説明します。 ここで、WatchFace を作成する方法を説明するトピックが見つかります。

## <a name="screen-sizesandroidwearscreen-sizesmd"></a>[画面サイズ](~/android/wear/screen-sizes.md)

プレビューし、ユーザー インターフェイスの使用可能な画面サイズを最適化します。

## <a name="deployment--testingandroidweardeploy-testindexmd"></a>[配置とテスト](~/android/wear/deploy-test/index.md)

Android Wear デバイスまたは Wear 用に構成された Android エミュレーターには、Android Wear アプリをデプロイする方法について説明します。 デバッグのヒントと開発用コンピューターと、Android デバイスの Bluetooth 接続を設定する方法についての情報も含まれています。

## <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Wear Api](https://developer.android.com/reference/android/support/wearable)

Android デベロッパー サイトがなど Wear の Api キーの詳細情報を提供します[ウェアラブル アクティビティ](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html)、[インテント](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html)、[認証](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html)、 [コンプリケーション](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html)、[レンダリング コンプリケーション](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html)、[通知](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html)、[ビュー](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)、および[WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html)します。



## <a name="samples"></a>サンプル

数を検索する[サンプル](https://developer.xamarin.com/samples/android/Android%20Wear/)Android Wear を使用して (に直接移動または[github](https://github.com/xamarin/monodroid-samples/tree/master/wear))。

|サンプル|説明|スクリーン ショット|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/monodroid/wear/SkeletonWear/)|GridViewPager や対話型通知など、ウェアラブルのプロジェクトの基本の単純な例です。|![Skeletonwear のスクリーン ショット](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/monodroid/wear/WatchViewStub/)|画面の図形を検出し、適切なレイアウトを自動的に読み込まれます WatchViewStub コントロールの簡単なデモします。 WatchViewStub がでどのように動作するかを参照してください、 **Resources/layout/main_activity.xml**レイアウト。|![WatchViewStub のスクリーン ショット](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/)|Wear 通知ページでは、レシピの手順の形式でのデモです。 通知は、RecipeService.cs に作成されます。|![RecipeAssistant のスクリーン ショット](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/monodroid/wear/ElizaChat/)|「パーソナル アシスタント」との対話の楽しいサンプルには、Wear 対話型通知を使用して定義された応答を使用してメッセージ交換を作成する、Eliza が呼び出されます。|![ElizaChat のスクリーン ショット](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)|GridViewPager は、ユーザーが垂直方向にスワイプ、2D ナビゲーション パターンを実装し、オプションやコンテンツ内を移動するには、水平方向にします。|![GridViewPager のスクリーン ショット](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace は、アナログ スタイルの時間、分、および 2 つ目の手でカスタムのウォッチの文字盤が。 このサンプルでは、現在の時刻を描画するウォッチ face サービスを作成およびハンドル アンビエント モードと可視性は、イベントを変更する方法を示します。 これには、タイム ゾーンの変更をリッスンし、それに応じて、時間が自動的に更新するブロードキャスト レシーバーが含まれます。|![WatchFace のスクリーン ショット](images/gridviewpager.png)|


## <a name="videos"></a>ビデオ

Wear で Xamarin.Android をについて説明するリンクをサポートしてこれらのビデオを確認します。

|説明|スクリーン ショット|
|--- |--- |
|[Android L とはるかに多く](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android L の開発者プレビューはマテリアル デザイン、通知、および新しいアニメーションは、いくつかの名前を含む多くを活用するために開発者向けの新しい Api を導入します。|![プレゼンテーションのビデオのスクリーン ショット](images/video-android-l.png)|
|[C#祝いと目には。Google Glass や Android Wear](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash;将来 (または、Inspector ガジェット エピソード) から何かのように見えるかもしれませんウェアラブル コンピューティングが、多くの人々 が未来を今日増えて既に! C#開発者はこれを知るし、ツールと (進化 2014) から、ウェアラブル デバイスの能力を活用するスキルが既にあります。|![プレゼンテーションのビデオのスクリーン ショット](images/video-eyes-ears.png)|
|[新機能については Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; L を Android、Android Wear、Android TV、Android Auto、マテリアル デザイン、およびアート; これが何 Xamarin の開発者は、平均するでしょうか。 進化 2014 から。|![プレゼンテーションのビデオのスクリーン ショット](Images/video-whats-new.png)|


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
