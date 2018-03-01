---
title: "Android の消耗"
description: "Android ウェアラブル デバイス用のアプリをビルドします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1dad5e859efdf69e7003b45724f718b16faffd62
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="android-wear"></a>Android の消耗

## <a name="android-wear"></a>Android の消耗

Android の劣化は、スマート ウォッチなど、ウェアラブル デバイス用に設計された Android のバージョンです。 このセクションでは、インストールし、ウェア開発では、詳しい手順については、最初の消耗デバイスと、独自に作成するためにアプリを身に参照できるサンプルの一覧を作成するために必要なツールを構成する方法の手順を説明します。

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[はじめに](~/android/wear/get-started/index.md)

Android を着用が導入されていますのインストールし、ウェア開発用コンピューターを構成する方法について説明および作成して、エミュレーターまたはデバイスの損傷で最初の Android 着用アプリを実行するための手順を説明します。

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[ユーザー インターフェイス](~/android/wear/user-interface/index.md)

Android の消耗固有が制御し、これらのコントロールを使用する方法を示すサンプルへのリンクを提供について説明します。

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[プラットフォームの機能](~/android/wear/platform/index.md)

このセクションのドキュメントでは、Android を着用に固有の機能について説明します。 ここで、WatchFace を作成する方法を説明するトピックが見つかります。

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[画面サイズ](~/android/wear/screen-sizes.md)

プレビューし、ユーザー インターフェイスの使用可能な画面サイズを最適化します。

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[展開とテスト](~/android/wear/deploy-test/index.md)

Android 着用デバイスまたは消耗用に構成された Android エミュレーターに着けの Android アプリを展開する方法について説明します。 デバッグのヒントと、開発用コンピューターと、Android デバイスの Bluetooth 接続を設定する方法に関する情報も含まれています。


<a name="Samples" />

## <a name="samples"></a>サンプル

数を検索する[サンプル](https://developer.xamarin.com/samples/android/Android%20Wear/)Android 着用を使用して (に直接移動または[github](https://github.com/xamarin/monodroid-samples/tree/master/wear))。 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>サンプル</strong>
      </th>
      <th>
          <strong>説明</strong>
      </th>
      <th>
          <strong>スクリーン ショット</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
GridViewPager や対話型の通知など、ウェアラブル プロジェクトの基本の単純な例です。
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
画面の図形を検出し、正しいレイアウトを自動的にロードされる WatchViewStub コントロールの簡単なデモを行います。
WatchViewStub がでどのように動作するかを参照してください、 <b>Resources/layout/main_actvity.xml</b>レイアウトです。
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
レシピの手順の形式での消耗通知ページのデモンストレーションです。 通知の作成に<b>RecipeService.cs</b>です。
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
「個人アシスタント」との対話の楽しいサンプルでは、ある eliza が消耗対話型の通知を使用して、用意された応答を使用してメッセージ交換を作成すると呼ばれます。
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager が、ユーザーが垂直方向に読み取ります 2D ナビゲーション パターンを実装し、次のオプションとコンテンツ内を移動するには、水平方向にします。
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b>アナログ スタイルの時間、分、および 2 番目の手でカスタム ウォッチの文字盤がします。 このサンプルでは、現在の時刻を描画するウォッチ フェイス サービスを作成およびハンドル アンビエント モードと可視性は、イベントを変更する方法を示します。 タイム ゾーンの変更をリッスンし、それに応じて、時間が自動的に更新するブロードキャスト受信者が含まれています。
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>ビデオ

消耗と Xamarin.Android をについて説明するリンクをサポートしてこれらのビデオを確認します。

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0"/ /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L およびその他多く</a>
        <br />
Android の L Developer Preview マテリアルのデザイン、通知、および新しいアニメーションは、いくつかの例を含む多種多様な活用するために開発者用の新しい Api が導入されました。</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# の場合は目と my 耳にします Google ガラスおよび Android を着用。</a>
        <br />
将来 (またはインスペクター ガジェット エピソード) から何かのように見えるかもしれませんウェアラブル コンピューティングが、多くの人が既にことを望んで将来今日! C# 開発者を把握このして既にツールとスキル ウェアラブル デバイス (する 2014) の機能を利用します。</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Xamarin.Android の新機能</a>
        <br />
        <i>Android L、Android の消耗、Android テレビ、Android の自動、材料デザイン、およびアートです。どういうことに、Xamarin の開発者ですか。</i>から 2014 の進化します。</td>
    </tr>
</table>


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
