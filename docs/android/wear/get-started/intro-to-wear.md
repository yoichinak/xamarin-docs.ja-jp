---
title: Android Wear の概要
description: Google の Android 磨耗が導入されたことで、優れた Android アプリの開発に関しては、スマートフォンとタブレットのみに制限されなくなりました。 Android で Android の磨耗がサポートされるため、手首でコードをC#実行できるようになります。 この概要では、Android の磨耗の基本的な概要と主な機能について説明し、Android 磨耗2.0 で使用できる機能の概要を示します。 この記事では、より一般的な Android 用の磨耗デバイスのいくつかを紹介し、さらに参考になる、Google Android の重要なドキュメントへのリンクを示します。
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a57273005df45c2f8035563efe9562c27cdfa732
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648410"
---
# <a name="introduction-to-android-wear"></a>Android Wear の概要

_Google の Android 磨耗が導入されたことで、優れた Android アプリの開発に関しては、スマートフォンとタブレットのみに制限されなくなりました。Android で Android の磨耗がサポートされるため、手首でコードをC#実行できるようになります。この概要では、Android の磨耗の基本的な概要と主な機能について説明し、Android 磨耗2.0 で使用できる機能の概要を示します。この記事では、より一般的な Android 用の磨耗デバイスのいくつかを紹介し、さらに参考になる、Google Android の重要なドキュメントへのリンクを示します。_


## <a name="overview"></a>概要

Android の磨耗は、Motorola 360、LG の G watch、Samsung 歯車など、さまざまなデバイスで実行されます。 Sony の SmartWatch 3 を含む2つ目の世代も、組み込みの GPS やオフラインの音楽再生などの追加機能と共にリリースされました。 Android の摩耗2.0 では、Google は LG Watch のスポーツと LG Watch のスタイルの2つの新しいウォッチを LG と協力しました。

![Android 磨耗2.0 デバイス](intro-to-wear-images/hero-image.png "Android の磨耗2.0 デバイスの例")

Xamarin android 5.0 以降では、android 4.4 W (API 20) のサポートと、磨耗に特化した UI コントロールを追加する NuGet パッケージを通じて、Android の磨耗がサポートされます。 Xamarin Android 5.0 以降には、磨耗アプリをパッケージ化するための機能も含まれています。 このガイドで後述するように、NuGet パッケージは Android 磨耗2.0 でも利用できます。


## <a name="android-wear-basics"></a>Android の磨耗の基本

Android の磨耗には、Android ハンドヘルドアプリとは異なるユーザーインターフェイスパラダイムがあります。 磨耗アプリの最初の波は、何らかの方法でコンパニオンハンドヘルドアプリを拡張するように設計されていましたが、Android 磨耗2.0 以降、磨耗アプリをスタンドアロンで使用できます。 アプリを展開すると、付属のハンドヘルドアプリでパッケージ化されます。 ほとんどの摩耗アプリはハンドヘルドコンパニオンアプリに依存しているため、ハンドヘルドアプリと通信するには何らかの方法が必要です。 以下のセクションでは、これらの使用シナリオについて説明し、基本的な Android の磨耗機能について概説します。 



### <a name="usage-scenarios"></a>使用シナリオ

Android 劣化の最初のバージョンは、主に、拡張された通知を使用した現在のハンドヘルドアプリケーションの拡張と、ハンドヘルドアプリとウェアラブルアプリとの間でのデータの同期に重点を置いていました。 そのため、これらのシナリオは比較的簡単に実装できます。


#### <a name="wearable-notifications"></a>ウェアラブル通知

Android の磨耗をサポートする最も簡単な方法は、ハンドヘルドデバイスとウェアラブルデバイス間の通知の共有性質を利用することです。 サポート v4 notification API と`WearableExtender`クラス ( [Xamarin Android サポートライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)で利用可能) を使用することにより、受信トレイスタイルカードや音声入力など、プラットフォームのネイティブ機能を活用できます。 [RecipeAssistant](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant)サンプルには、Android の磨耗デバイスに通知のリストを送信する方法を示すコード例が用意されています。 



#### <a name="companion-applications"></a>コンパニオンアプリケーション

もう1つの方法は、ウェアラブルデバイスでネイティブに実行される完全なアプリケーションと、付属のハンドヘルドアプリを作成することです。 このアプローチの好例として、[クイズ](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-quiz)サンプルアプリがあります。これは、ハンドヘルドデバイスで実行されるクイズを作成し、ウェアラブルデバイスでクイズの質問をする方法を示しています。 



### <a name="user-interface"></a>ユーザー インターフェイス

磨耗の主要なナビゲーションパターンは、垂直方向に並べられた一連のカードです。 これらの各カードには、同じ行に重なっているアクションを関連付けることができます。 クラス`GridViewPager`は、この機能を提供します。と`ListView`同じアダプターの概念に従います。 通常、 `GridViewPager`を`FragmentGridPagerAdaptor` (または`GridPagerAdaptor`) に関連付けて、各行と列のセルを`Fragment`として表すことができます。 

[![磨耗のナビゲーション](intro-to-wear-images/2d-picker-sml.png "磨耗のナビゲーション")](intro-to-wear-images/2d-picker.png#lightbox)

また、磨耗では、上の図に示すように、小さな説明のテキストを含む大きな色の円で構成されるアクションボタンも使用します。  [GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)サンプルは、磨耗アプリで`GridViewPager`および`GridPagerAdapter`を使用する方法を示しています。

Android 磨耗2.0 は、ナビゲーションドロワー、アクションドロワー、およびインラインアクションボタンを磨耗ユーザーインターフェイスに追加します。 Android の摩耗2.0 ユーザーインターフェイス要素の詳細については、Android の[構造](https://www.google.com/design/spec-wear/system-overview/anatomy.html)に関するトピックを参照してください。 



### <a name="communications"></a>Communications

Android の磨耗には、ウェアラブルアプリとコンパニオンハンドヘルドアプリ間の通信を容易にする2つの異なる通信 Api が用意されています。 

**データ API**&ndash;この API は、ウェアラブルデバイスとハンドヘルドデバイス間の同期されたデータストアに似ています。 Android では、ウェアラブルとハンドヘルドの間の変更を、最適な方法で反映することができます。 ウェアラブルが範囲外の場合は、後で同期をキューに置いています。 この API のメインエントリポイントは`WearableClass.DataApi`です。 この API の詳細については、「Android の[データ項目の同期](https://developer.android.com/training/wearables/data-layer/data-items.html)」を参照してください。 

**メッセージ API**&ndash;この API により、低レベルの通信パスを使用できるようになります。小さなペイロードは、ハンドヘルドアプリとウェアラブルアプリの同期なしで一方向に送信されます。
この API のメインエントリポイントは`WearableClass.MessageApi`です。
この API の詳細については、「Android での[メッセージの送受信](https://developer.android.com/training/wearables/data-layer/messages.html)」を参照してください。

これらのメッセージを受信するためのコールバックを API リスナーの各インターフェイスで登録するか、またはから`WearableListenerService`派生するアプリにサービスを実装するかを選択できます。
このサービスは、Android の磨耗によって自動的にインスタンス化されます。
[FindMyPhone](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-findmyphonesample)サンプルは、を`WearableListenerService`実装する方法を示しています。



### <a name="deployment"></a>展開

各ウェアラブルアプリは、メインアプリケーション APK 内に埋め込まれた独自の APK ファイルを使用してデプロイされます。 このパッケージは、Xamarin. Android 5.0 以降で自動的に処理されますが、バージョン5.0 より前の Xamarin. Android のバージョンでは、手動で実行する必要があります。 
[パッケージ化を](~/android/wear/deploy-test/packaging.md)使用すると、展開の詳細が説明されます。 



## <a name="going-further"></a>理解を深める 

Android の磨耗に慣れるための最善の方法は、最初のアプリをビルドしてテストすることです。 次の一覧は、迅速に利用できるようにするために推奨される読み取り順序を示しています。

1.  [セットアップ & のインストール](~/android/wear/get-started/installation.md)では、Xamarin Android のアプリをビルドするための開発環境をインストールして構成するための詳細な手順を提供します。 

2.  必要なパッケージをインストールし、エミュレーターまたはデバイスを構成したら、「 [Hello, 磨耗](~/android/wear/get-started/hello-wear.md)」を参照して、ボタンのクリックを処理し、磨耗に click カウンターを表示する小さな Android 用の磨耗プロジェクトを作成する方法を説明した手順を実行します。ドライブ. 

3.  [展開 & テスト](~/android/wear/deploy-test/index.md)では、エミュレーターとデバイスの構成と展開に関する詳細情報を提供します。これには、Bluetooth 経由でアプリを摩耗デバイスに展開する方法についての説明が含まれます。

4.  [画面のサイズ](~/android/wear/screen-sizes.md)を使用すると、磨耗デバイスで使用できるさまざまな画面サイズに合わせてユーザーインターフェイスをプレビューおよび最適化する方法について説明します。 

5.  [パッケージの操作](~/android/wear/deploy-test/packaging.md)Google Play で配布するために、磨耗アプリを手動でパッケージ化する手順について説明します。

最初の摩耗アプリを作成したら、Android 用のカスタムウォッチ式を作成することをお勧めします。 
[ウォッチ式を作成](~/android/wear/platform/creating-a-watchface.md)すると、デジタルウォッチフェイスサービスを削除するための手順とコード例が示されます。その後、追加の機能を使用してアナログスタイルのウォッチ式に拡張するコードを記述します。 



## <a name="android-wear-20"></a>Android の磨耗2.0

Android の磨耗2.0 では、*複雑さ*、曲線レイアウト、ナビゲーションとアクションの引き出し、拡張された通知など、さまざまな新機能が導入されています。 また、磨耗2.0 を使用すると、ハンドヘルドアプリから独立して動作するスタンドアロンアプリを作成できます。 新しい*手首ジェスチャ*機能によって、アプリとの1対1の対話が可能になります。 以下のセクションでは、これらの機能について説明し、アプリでの使用を開始する際に役立つリンクを示します。



### <a name="install-wear-20-packages"></a>摩耗2.0 パッケージのインストール

Xamarin Android での摩耗2.0 アプリをビルドするには、プロジェクトに**xamarin app-v 2.0**パッケージを追加する必要があります ([**参照] タブ**をクリックします)。

[![Xamarin. Android. app-v 2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin app-v 2.0 NuGet をインストールする")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

この NuGet パッケージには、Android Support ウェアラブルと摩耗互換性ライブラリの両方のバインドが含まれています。

GooglePlayServices をインストールすることをお勧めします。これに加え**て、次**のように**xamarin**をインストールすることをお勧めします。 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Install the Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>磨耗2.0 の主な機能

Android の磨耗2.0 は、2014での初期起動以来、Android の磨耗の最大の更新プログラムです。 以下のセクションでは、Android の磨耗2.0 の主な機能について説明し、アプリでこれらの新機能を使用する際に役立つリンクを示します。 


#### <a name="complications"></a>問題点

*複雑*な点としては、ウォッチ盤をスワイプしなくても一目で見ることができる小さな watch face ウィジェットがあります。 複雑さはデスクトップスタイルのダッシュボードウィジェットに似ています。これらの情報には、天気、バッテリ寿命、カレンダーイベント、適合性アプリの統計情報が表示されます。 

![複雑さの例](intro-to-wear-images/complications.png "複雑さの例")

複雑さの詳細については、「Android [Watch Face の複雑さ](https://developer.android.com/wear/preview/features/complications.html)」を参照してください。 



#### <a name="navigation-and-action-drawers"></a>ナビゲーションとアクションの引き出し 

2つの新しい引き出しが、磨耗2.0 に含まれています。 画面の上部に表示される*ナビゲーションドロワー*を使用すると、ユーザーはアプリのビュー間を移動できます (次の左側の図を参照)。 *操作ドロアー*は、画面の下部 (右側に表示) に表示され、ユーザーはアクションの一覧から選択できます。 

![ナビゲーションとアクションの引き出し](intro-to-wear-images/drawers.png "ナビゲーションとアクションの引き出し")

これら2つの新しい対話型引き出しの詳細については、「Android の[磨耗のナビゲーションと操作](https://developer.android.com/wear/preview/features/ui-nav-actions.html)」を参照してください。 



#### <a name="curved-layouts"></a>曲線レイアウト 

磨耗2.0 では、丸い磨耗デバイスで曲線レイアウトを表示するための新機能が導入されています。 具体的には、 `WearableRecyclerView`新しいクラスは、丸い表示に垂直方向の項目の一覧を表示するように最適化されています。 

![曲線レイアウトの例](intro-to-wear-images/curved-layout.png "曲線レイアウトの例")

`WearableRecyclerView`クラスを`RecyclerView`拡張して、曲線レイアウトと円形スクロールジェスチャをサポートします。 詳細については、Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API のドキュメントを参照してください。 



#### <a name="standalone-apps"></a>スタンドアロンアプリ 

Android の摩耗2.0 アプリは、ハンドヘルドアプリから独立して動作できます。 つまり、たとえば、スマートウォッチは、付属のハンドヘルドデバイスの電源が入っている場合や、ウェアラブルデバイスから離れた場所にいる場合でも、完全な機能を引き続き提供できます。 この機能の詳細については、「Android[スタンドアロンアプリ](https://developer.android.com/wear/preview/features/standalone-apps.html)」を参照してください。



#### <a name="wrist-gestures"></a>手首ジェスチャ 

手首ジェスチャを使用すると、ユーザーはタッチスクリーン&ndash;を使用せずにアプリと対話できるようになります。ユーザーはアプリにワンハンドで応答できます。 2つの手首ジェスチャがサポートされています。 

-   手首をフリック
-   手首のフリック

詳細については、Android の[手首のジェスチャ](https://developer.android.com/wear/preview/features/gestures.html)に関するトピックを参照してください。 


これには、インラインアクション、スマート応答、リモート入力、拡張された通知、通知の新しいブリッジングモードなど、さまざまな摩耗2.0 機能があります。 新しい摩耗2.0 機能の詳細については、「Android [API の概要](https://developer.android.com/wear/preview/api-overview.html)」を参照してください。 



## <a name="devices"></a>[デバイス]

Android の磨耗を実行できるデバイスの例をいくつか次に示します。

* [Motorola 360](https://moto360.motorola.com/)
* [LG G ウォッチ](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G Watch R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung 歯車ライブ](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>関連項目

Google の Android の磨耗ドキュメントを確認してください。

* [Android の磨耗について](http://www.android.com/wear/)
* [Android の摩耗アプリの設計](https://developer.android.com/design/wear/index.html)
* [android. サポート. ウェアラブルライブラリ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android の磨耗2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>まとめ

この概要では、Android の磨耗の概要について説明しました。 Android の磨耗の基本的な機能について概説し、Android 磨耗2.0 で導入された機能の概要を説明しました。 この記事では、開発者が Xamarin. Android の磨耗開発を始めるのに役立つ重要な情報へのリンクを提供し、現在市場にある一部の Android 劣化デバイスの例を示しました。


## <a name="related-links"></a>関連リンク

- [インストールとセットアップ](~/android/wear/get-started/installation.md)
- [はじめに](~/android/wear/get-started/index.md)
