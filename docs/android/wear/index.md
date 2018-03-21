---
title: Android Wear
description: "Android ウェアラブル デバイス用のアプリをビルドします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: 31114df0b631aea909e82f3a8b836d5ef922d2c1
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2018
---
# <a name="android-wear"></a>Android Wear

Android の劣化は、スマート ウォッチなど、ウェアラブル デバイス用に設計された Android のバージョンです。 このセクションでは、インストールし、ウェア開発では、詳しい手順については、最初の消耗デバイスと、独自に作成するためにアプリを身に参照できるサンプルの一覧を作成するために必要なツールを構成する方法の手順を説明します。

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[はじめに](~/android/wear/get-started/index.md)

Android を着用が導入されていますのインストールし、ウェア開発用コンピューターを構成する方法について説明および作成して、エミュレーターまたはデバイスの損傷で最初の Android 着用アプリを実行するための手順を説明します。

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[ユーザー インターフェイス](~/android/wear/user-interface/index.md)

Android の消耗固有が制御し、これらのコントロールを使用する方法を示すサンプルへのリンクを提供について説明します。

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[プラットフォーム機能](~/android/wear/platform/index.md)

このセクションのドキュメントでは、Android を着用に固有の機能について説明します。 ここで、WatchFace を作成する方法を説明するトピックが見つかります。

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[画面サイズ](~/android/wear/screen-sizes.md)

プレビューし、ユーザー インターフェイスの使用可能な画面サイズを最適化します。

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[配置とテスト](~/android/wear/deploy-test/index.md)

Android 着用デバイスまたは消耗用に構成された Android エミュレーターに着けの Android アプリを展開する方法について説明します。 デバッグのヒントと、開発用コンピューターと、Android デバイスの Bluetooth 接続を設定する方法に関する情報も含まれています。

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Api を着用します。](https://developer.android.com/reference/android/support/wearable)

Android Developer サイトなどにキーを着用 Api の詳細情報を提供する[ウェアラブル アクティビティ](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html)、[インテント](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html)、[認証](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html)、 [複雑さの一部](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html)、[レンダリング複雑](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html)、[通知](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html)、[ビュー](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)、および[WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html)です。



## <a name="samples"></a>サンプル

数を検索する[サンプル](https://developer.xamarin.com/samples/android/Android%20Wear/)Android 着用を使用して (に直接移動または[github](https://github.com/xamarin/monodroid-samples/tree/master/wear))。 

|サンプル|説明|スクリーン ショット|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|GridViewPager や対話型の通知など、ウェアラブル プロジェクトの基本の単純な例です。|![Skeletonwear のスクリーン ショット](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|画面の図形を検出し、正しいレイアウトを自動的にロードされる WatchViewStub コントロールの簡単なデモを行います。  WatchViewStub がでどのように動作するかを参照してください、 **Resources/layout/main_actvity.xml**レイアウトです。|![WatchViewStub のスクリーン ショット](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|レシピの手順の形式での消耗通知ページのデモンストレーションです。 通知は、RecipeService.cs に作成されます。|![RecipeAssistant のスクリーン ショット](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|「個人アシスタント」との対話の楽しいサンプルでは、ある eliza が消耗対話型の通知を使用して、用意された応答を使用してメッセージ交換を作成すると呼ばれます。|![ElizaChat のスクリーン ショット](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager が、ユーザーが垂直方向に読み取ります 2D ナビゲーション パターンを実装し、次のオプションとコンテンツ内を移動するには、水平方向にします。|![GridViewPager のスクリーン ショット](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace は、アナログ スタイルの時間、分、および 2 番目の手でカスタム ウォッチの文字盤がします。 このサンプルでは、現在の時刻を描画するウォッチ フェイス サービスを作成およびハンドル アンビエント モードと可視性は、イベントを変更する方法を示します。 タイム ゾーンの変更をリッスンし、それに応じて、時間が自動的に更新するブロードキャスト受信者が含まれています。|![WatchFace のスクリーン ショット](images/gridviewpager.png)|


##  <a name="videos"></a>ビデオ

消耗と Xamarin.Android をについて説明するリンクをサポートして以下のビデオを確認します。

|説明|スクリーン ショット|
|--- |--- |
|[Android L、およびよりずっと](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android L Developer Preview 導入された多くを活用するために開発者用の新しい Api の資料デザイン、通知、および新しいアニメーションは、いくつかの例を含むです。|![プレゼンテーションのビデオ スクリーン ショット](images/video-android-l.png)|
|[C# の場合は目と祝いに: Google ガラスおよび Android を着用](https://www.youtube.com/watch?v=80H8tXByZQc)&ndash;将来 (またはインスペクター ガジェット エピソード) から何かのように見えるかもしれませんウェアラブル コンピューティングが、多くの人が既にことを望んで将来今日! C# 開発者を把握このして既にツールとスキル ウェアラブル デバイス (する 2014) の機能を利用します。|![プレゼンテーションのビデオ スクリーン ショット](images/video-eyes-ears.png)|
|[Xamarin.Android の新機能](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L、Android を着用、Android のテレビ、Android の自動、マテリアルの設計、およびアート; これが何 Xamarin 開発者に平均? する 2014年からです。|![プレゼンテーションのビデオ スクリーン ショット](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
