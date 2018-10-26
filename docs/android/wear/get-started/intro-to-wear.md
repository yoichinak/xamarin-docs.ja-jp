---
title: Android Wear の概要
description: Google の Android Wear の導入に伴いがスマート フォンやタブレットだけに制限優れた Android アプリを開発する際にします。 Android Wear の Xamarin.Android のサポートにより、実行するC#手首上のコードです。 この概要は、Android Wear の基本的な概要を提供します、主な機能について説明し、Android Wear 2.0 で使用できる機能の概要を提供しています。 最も一般的な Android Wear デバイスの一部を一覧表示し、参考資料の重要な Google Android Wear のドキュメントへのリンクを提供します。
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a35cb82f4f6d20e91f45a782c73d3ef811947c3a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117786"
---
# <a name="introduction-to-android-wear"></a>Android Wear の概要

_Google の Android Wear の導入に伴いがスマート フォンやタブレットだけに制限優れた Android アプリを開発する際にします。Android Wear の Xamarin.Android のサポートにより、実行するC#手首上のコードです。この概要は、Android Wear の基本的な概要を提供します、主な機能について説明し、Android Wear 2.0 で使用できる機能の概要を提供しています。最も一般的な Android Wear デバイスの一部を一覧表示し、参考資料の重要な Google Android Wear のドキュメントへのリンクを提供します。_


## <a name="overview"></a>概要

さまざまな第 1 世代の Motorola 360、LG の G watch では、Samsung 歯車 Live などのデバイスで android Wear が実行されます。 Sony のスマートウォッチ 3 を含む、第 2 世代は、組み込みの GPS とオフラインの音楽の再生などの追加機能にもリリースされました。 Android Wear の 2.0 では、Google との協力により LG の 2 つの新しいウォッチ: LG ウォッチ スポーツおよび LG ウォッチ スタイル。

![Android Wear 2.0 デバイス](intro-to-wear-images/hero-image.png "例 Android Wear 2.0 デバイス")

Android Wear Xamarin.Android 5.0 以降では、Android 4.4W (API 20) をサポートし、追加を追加する NuGet パッケージの Wear 固有の UI を制御します。 Xamarin.Android 5.0 以降では、Wear アプリをパッケージ化するための機能も含まれています。 NuGet パッケージでは、Android Wear 2.0 のこのガイドの後半の説明に従って入手できます。


## <a name="android-wear-basics"></a>Android Wear の基本

Android Wear がハンドヘルド Android のアプリの異なるユーザー インターフェイスのパラダイムです。 Wear アプリの最初の波が、付属の拡張に設計されたいくつかの方法が Android Wear 2.0 以降ではハンドヘルド app、Wear アプリは、単体で使用されるをすることができます。 Wear アプリを展開するときに、コンパニオン ハンドヘルド アプリにパッケージされています。 ほとんど Wear アプリは、ハンドヘルド コンパニオン アプリによって異なります、ハンドヘルド アプリと通信する手段を必要な。 次のセクションは、これらの使用状況シナリオについて説明し、Android Wear、essential の新機能の概要します。 



### <a name="usage-scenarios"></a>使用シナリオ

Android Wear の最初のバージョンが強化された通知の使用の現在のハンドヘルド アプリケーションを拡張して、ハンドヘルド アプリとウェアラブルのアプリ間のデータの同期で主に重点を置いています。 そのため、これらのシナリオは比較的簡単に実装できます。


#### <a name="wearable-notifications"></a>ウェアラブル通知

Android Wear をサポートする最も簡単な方法は、ハンドヘルドとウェアラブル デバイス間での通知の共有の性質の活用することです。 サポート v4 通知 API を使用して、`WearableExtender`クラス (で使用できる、 [Xamarin Android サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/))、受信トレイ スタイル カードなど、プラットフォームのネイティブ機能を利用または音声入力することができます。 [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/)サンプルは、Android Wear デバイスに通知の一覧を送信する方法を示すコード例を提供します。 



#### <a name="companion-applications"></a>コンパニオン アプリケーション

別の方法では、ウェアラブル デバイスでネイティブに実行して、コンパニオン ハンドヘルド アプリと構成のペアを完全なアプリケーションを作成します。 この方法の好例が、[クイズ](https://developer.xamarin.com/samples/monodroid/wear/Quiz/)サンプル アプリは、ハンドヘルド デバイスで実行され、ウェアラブル デバイスで、クイズを質問するクイズを作成する方法を示します。 



### <a name="user-interface"></a>ユーザー インターフェイス

Wear の主なナビゲーション パターンは、一連の垂直方向に整列するカードです。 同じ行をレイヤー化されているアクションを関連付けるこれらのカードの各できます。 `GridViewPager`クラスには、この機能が用意されていますと同じアダプターの概念に準拠しています。`ListView`します。 通常関連付ける、`GridViewPager`で、 `FragmentGridPagerAdaptor` (または`GridPagerAdaptor`) を使用して各の行と列のセルとして表すできる、 `Fragment`: 

[![ナビゲーションを着用](intro-to-wear-images/2d-picker-sml.png "Wear ナビゲーション")](intro-to-wear-images/2d-picker.png#lightbox)

大規模で構成されるアクション ボタンの使用 (上記参照) としてその下にある小さな説明テキストを含む円の色は wear もします。  [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)サンプルを使用する方法を示します`GridViewPager`と`GridPagerAdapter`Wear アプリ。

Android Wear 2.0 は、Wear のユーザー インターフェイスにナビゲーション ドロワー、アクションのドロアーおよびインライン アクション ボタンを追加します。 詳細については、Android Wear 2.0 のユーザー インターフェイス要素は、Android を参照してください。[構造](https://www.google.com/design/spec-wear/system-overview/anatomy.html)トピック。 



### <a name="communications"></a>通信

Android Wear では、2 つの異なる通信ウェアラブルのアプリと必携ハンドヘルド アプリ間の通信を容易にするための Api を提供します。 

**データ API** &ndash;この API は、ウェアラブル デバイスとハンドヘルド デバイス間の同期されたデータ ストアに似ています。 これを行うには最適である場合に、ウェアラブルとハンドヘルドの間の変更を反映 android 行われます。 ウェアラブルが範囲外の日時の同期をキューします。 この API のメイン エントリ ポイントは`WearableClass.DataApi`します。 この API の詳細については、Android を参照してください。[データ項目の同期](https://developer.android.com/training/wearables/data-layer/data-items.html)トピック。 

**API メッセージ**&ndash;この API により、下位レベルの通信パスを使用する: 小さなペイロードをハンドヘルドとウェアラブルのアプリ間で同期しなくても一方向に送信されます。
この API のメイン エントリ ポイントは`WearableClass.MessageApi`します。
この API の詳細については、Android を参照してください。[受信メッセージを送信して](https://developer.android.com/training/wearables/data-layer/messages.html)トピック。

各 API のリスナー インターフェイスを使用してこれらのメッセージを受信するためのコールバックを登録または、代わりに、アプリから派生したサービスを実装することもできます`WearableListenerService`します。
このサービスは、Android Wear で自動的にインスタンス化されます。
[FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/)サンプルを実装する方法を示しています、`WearableListenerService`します。



### <a name="deployment"></a>配置

各ウェアラブルのアプリは、APK のメインのアプリケーション内に埋め込まれた独自の APK ファイルと共にデプロイされます。 このパッケージは、Xamarin.Android 5.0 以降では、自動的に処理されますが、バージョン 5.0 より前のバージョンの Xamarin.Android の手動で実行する必要があります。 
[パッケージの操作](~/android/wear/deploy-test/packaging.md)展開について詳しく説明します。 



## <a name="going-further"></a>理解を深める 

Android Wear を理解する最善の方法では、ビルドし、最初のアプリをテストします。 すぐに着手するための推奨される読み取り順序を次に示します。

1.  [セットアップとインストール](~/android/wear/get-started/installation.md)をインストールして、Xamarin.Android Wear アプリを構築するための開発環境の構成の詳細な手順を提供します。 

2.  必要なパッケージをインストールして、エミュレーターまたはデバイスが構成されているを参照してください[Hello、Wear](~/android/wear/get-started/hello-wear.md)ハンドル ボタン Android Wear の小規模なプロジェクトを作成する方法を説明する手順をクリックすると表示、。Wear デバイスでのカウンターをクリックします。 

3.  [配置とテスト](~/android/wear/deploy-test/index.md)を構成して、エミュレーターと Bluetooth 経由で Wear デバイスにアプリをデプロイする方法の手順を含め、デバイスへの展開に関する詳細情報を提供します。

4.  [操作画面サイズ](~/android/wear/screen-sizes.md)をプレビューし、さまざまな使用可能な画面サイズ Wear デバイスでのユーザー インターフェイスを最適化する方法について説明します。 

5.  [パッケージの操作](~/android/wear/deploy-test/packaging.md)Google Play で配布するための Wear アプリを手動でパッケージ化する手順について説明します。

最初の Wear アプリを作成すると、Android Wear の腕時計がカスタムのビルドを試みますしたい場合があります。 
[ウォッチの文字盤を作成する](~/android/wear/platform/creating-a-watchface.md)追加の拡張機能、アナログ スタイル ウォッチの文字盤を拡張するコードの後に、デジタル ウォッチ face サービスを停止、削除を開発するためのコード例および詳細な手順を提供します。 



## <a name="android-wear-20"></a>Android Wear 2.0

Android Wear 2.0 などのさまざまな新機能や機能を紹介*コンプリケーション*、曲線のレイアウト、ナビゲーションとアクションの引き出し、および展開された通知。 また、Wear 2.0 により、ハンドヘルド アプリから独立して動作するスタンドアロン アプリを構築することが可能にします。 新しい*手首ジェスチャ*機能により、アプリの片手やり取りします。 次のセクションでは、これらの機能を強調表示し、アプリでの使用の開始に役立つリンクを指定します。



### <a name="install-wear-20-packages"></a>インストール Wear 2.0 パッケージ

Xamarin.Android で Wear 2.0 アプリを作成するを追加する必要があります、 **Xamarin.Android.Wear v2.0**パッケージをプロジェクト (をクリックして、 **[参照] タブ**)。

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin.Android.Wear v2.0 の NuGet をインストールします。")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

この NuGet パッケージには、Android サポート ウェアラブルと互換性の Wear のライブラリのバインドが含まれています。

ほかに**Xamarin.Android.Wear**をインストールすることをお勧め、 **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Install the Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Wear 2.0 の主な機能

Android Wear 2.0 では、2014 年の最初のリリース以降 Android Wear 最大の更新をされています。 次のセクションでは、Android Wear 2.0 の主な機能を強調表示し、リンクが提供するためのアプリでこれらの新機能の使用を開始します。 


#### <a name="complications"></a>複雑な問題

*コンプリケーション*小さなウォッチをウォッチの文字盤をスワイプすることがなく、ひとめでわかります face ウィジェットは。 複雑な問題はデスクトップ スタイルのダッシュ ボードのウィジェットに似ています天気、バッテリの寿命、予定表イベント、およびアプリケーションへの適合性の統計などの情報を表示します。 

![コンプリケーション例](intro-to-wear-images/complications.png "複雑な問題の例")

詳細については、複雑な問題は、Android を参照してください。[ウォッチ Face コンプリケーション](https://developer.android.com/wear/preview/features/complications.html)トピック。 



#### <a name="navigation-and-action-drawers"></a>ナビゲーションとアクションの引き出し 

Wear 2.0 では、2 つの新しいドロワーが含まれます。 *ナビゲーション ドロワーを*、(ように、左下に) アプリのビュー間を移動するユーザーをできるように、画面の上部に表示されます。 *アクション ドロワー*アクションの一覧から選択できます (右側に表示されます) と、画面の下部に表示されます。 

![ナビゲーションとアクションの引き出し](intro-to-wear-images/drawers.png "ナビゲーションとアクションの引き出し")

これら 2 つの新しいインタラクティブな引き出しの詳細については、Android を参照してください。 [Wear のナビゲーションとアクション](https://developer.android.com/wear/preview/features/ui-nav-actions.html)トピック。 



#### <a name="curved-layouts"></a>曲線のレイアウト 

Wear 2.0 では、round Wear デバイス上の曲線のレイアウトを表示するための新機能について説明します。 具体的には、新しい`WearableRecyclerView`クラスは round のディスプレイで垂直方向の項目の一覧を表示するために最適化されています。 

![曲線のレイアウト例](intro-to-wear-images/curved-layout.png "曲線のレイアウトの例")

`WearableRecyclerView` 拡張、`RecyclerView`曲線のレイアウトと循環のスクロールのジェスチャをサポートするクラス。 詳細については、Android を参照してください。 [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API のドキュメント。 



#### <a name="standalone-apps"></a>スタンドアロン アプリ 

Android Wear 2.0 アプリは、ハンドヘルド アプリから独立して動作できます。 これにより、たとえば、スマート ウォッチ式を続行できますコンパニオン ハンドヘルド デバイスは、オフ、または、ウェアラブル デバイスから遠く離れたが有効な場合でも、すべての機能を提供します。 この機能の詳細については、Android を参照してください。[スタンドアロン アプリ](https://developer.android.com/wear/preview/features/standalone-apps.html)トピック。



#### <a name="wrist-gestures"></a>手首ジェスチャ 

手首ジェスチャを行うユーザーはタッチ スクリーンを使用せず、アプリと対話する&ndash;ユーザーが 1 つの手でアプリに応答できます。 2 つの手首ジェスチャがサポートされています。 

-   フリック手首
-   フリック手首

詳細については、Android を参照してください。[手首ジェスチャ](https://developer.android.com/wear/preview/features/gestures.html)トピック。 


インライン アクション、スマート応答、リモートの入力、展開された通知、および通知の新しいブリッジ モードなどの多くの詳細について Wear 2.0 機能があります。 Wear 2.0 の新機能についての詳細については、Android を参照してください。 [API の概要](https://developer.android.com/wear/preview/api-overview.html)します。 



## <a name="devices"></a>デバイス

Android Wear を実行できるデバイスのいくつかの例を次に示します。

* [Motorola 360](https://moto360.motorola.com/)
* [LG G ウォッチ](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G ウォッチ R](http://www.lg.com/us/smartwatch/g-watch-r)
* [ライブ Samsung 歯車](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony スマートウォッチ 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASU ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>関連項目

Google の Android Wear のドキュメントをご覧ください。

* [Android Wear について](http://www.android.com/wear/)
* [Android Wear アプリの設計](https://developer.android.com/design/wear/index.html)
* [android.support.wearable ライブラリ ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android Wear 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>まとめ

この概要では、Android Wear の概要が提供されています。 Android Wear の基本機能を説明し、Android Wear 2.0 で導入された機能の概要が含まれています。 Wear の Xamarin.Android の開発の概要開発者に役立つ重要な読み取りへのリンクが提供されているし、現在市場で Android Wear デバイスのいくつかの例が表示されています。


## <a name="related-links"></a>関連リンク

- [インストールとセットアップ](~/android/wear/get-started/installation.md)
- [はじめに](~/android/wear/get-started/index.md)
