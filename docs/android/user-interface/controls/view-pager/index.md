---
title: ViewPager
description: ViewPager は、レイアウト マネージャーで、ジェスチャー ナビゲーションを実装することができます。 ジェスチャー ナビゲーションにより、左と右のデータ ページの手順をスワイプするユーザー。 このガイドでは、およびフラグメントを使用せずにジェスチャー ViewPager を使用したナビゲーションを実装する方法について説明します。 PagerTitleStrip および PagerTabStrip を使用してページ インジケーターを追加する方法についても説明します。
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bd687175048bb6a19dde21e66619667511a76796
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="viewpager"></a>ViewPager

_ViewPager は、レイアウト マネージャーで、ジェスチャー ナビゲーションを実装することができます。ジェスチャー ナビゲーションにより、左と右のデータ ページの手順をスワイプするユーザー。このガイドでは、およびフラグメントを使用せずにジェスチャー ViewPager を使用したナビゲーションを実装する方法について説明します。PagerTitleStrip および PagerTabStrip を使用してページ インジケーターを追加する方法についても説明します。_

 
## <a name="overview"></a>概要

アプリの開発における一般的なシナリオは、兄弟ビュー間のナビゲーションをジェスチャーをユーザーに提供する必要があります。 この方法では、ユーザーは、左または右に (たとえば、セットアップ ウィザードまたはスライド ショー) 内のコンテンツのアクセス ページを読み取ります。 使用してこれらの方向にスワイプ ビューを作成することができます、`ViewPager`ウィジェットで使用できる[Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)です。 `ViewPager`の複数の子ビューから成るレイアウト ウィジェットは、各子ビューがレイアウト内のページを構成します。 

[![水平方向にスワイプ例とスクリーン ショットの TreePager アプリ](images/01-intro-sml.png)](images/01-intro.png#lightbox)

通常、`ViewPager`と組み合わせて使用[フラグメント](https://developer.xamarin.com/guides/android/platform_features/fragments/)。 ただし、状況によっては使用する場合があります`ViewPager`高まる複雑させず`Fragment`s。

`ViewPager` アダプターのパターンを使用して、表示するビューを提供します。 ここで使用するアダプターがによって使用されている概念的に似ています[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash;の実装を指定する`PagerAdapter`、ページを生成する、`ViewPager`をユーザーに表示します。 によって表示されるページ`ViewPager`できます`View`s または`Fragment`s。 ときに`View`s が表示され、アダプター サブクラス Android の`PagerAdapter`基本クラスです。 場合`Fragment`s が表示され、アダプター サブクラス Android の`FragmentPagerAdapter`します。 Android のサポート ライブラリも含まれています。 `FragmentPagerAdapter` (のサブクラス`PagerAdapter`) の接続の詳細に役立てるために`Fragment`データにします。 

このガイドでは、両方の方法を示しています。 

-   [ビュー Viewpager](~/android/user-interface/controls/view-pager/viewpager-and-views.md)、 [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/)アプリを開発して使用する方法を示します`ViewPager`ツリー カタログ (各種と常緑木のイメージ ギャラリー) のビューを表示します。 
    `PagerTabStrip`  および`PagerTitleStrip`ページ ナビゲーションのヘルプのタイトルを表示するために使用します。

-   [フラグメント Viewpager](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)、やや複雑[FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/)アプリを開発して使用する方法を示します`ViewPager`で`Fragment`として計算問題を表示するアプリケーションを作成します。フラッシュ カードとユーザー入力に応答します。 


## <a name="requirements"></a>要件

使用する`ViewPager`インストールする必要があります、アプリ プロジェクトで、 [Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)パッケージです。 NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)です。 

 
## <a name="architecture"></a>アーキテクチャ

によるジェスチャー ナビゲーションを実装するため次の 3 つのコンポーネントを使用する`ViewPager`:

-   ViewPager
-   アダプター
-   ポケットベル インジケーター

これらの各コンポーネントについて、以下にまとめます。



### <a name="viewpager"></a>ViewPager

`ViewPager` コレクションを表示するレイアウト マネージャーは、`View`一度に 1 つの s。 そのジョブを開始ユーザーのスワイプ ジェスチャを検出し、必要に応じて、次または前のビューに移動します。 たとえば、次のスクリーン ショットを示しています、`ViewPager`ユーザー ジェスチャへの応答で 1 つのイメージから、[次へ] への移行を行うこと。 

[![ビューの間の遷移を表示する TreePager のクローズ アップ アプリ](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>アダプター

`ViewPager` データを抽出、*アダプター*です。 アダプターのジョブを作成する、`View`によって表示される、 `ViewPager`、必要に応じてそれらを提供します。 次の図は、この概念を示しています。&ndash;アダプターの作成およびデータ設定`View`s にすると、`ViewPager`です。 として、`ViewPager`ユーザーの方向にスワイプ ジェスチャを検出する場合、適切なアダプターの生成を求めます`View`を表示します。 

[![アダプターに接続する方法の画像と名前、ViewPager を示すダイアグラム](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

この例では各`View`に渡される前に、ツリーのイメージおよびツリー名から構築、`ViewPager`です。 



### <a name="pager-indicator"></a>ポケットベル インジケーター

`ViewPager` 大きなデータ セットの表示に使用することがあります (たとえば、イメージ ギャラリーが格納されているイメージの)。 大量のデータを移動するユーザーを支援する`ViewPager`が付属して多くの場合、*ポケットベル インジケーター*文字列を表示します。 文字列には、イメージのタイトル、キャプション、またはデータ セット内の現在のビューの位置だけがあります。 

このナビゲーション情報を生成できる 2 つのビュー:`PagerTabStrip`と`PagerTitleStrip.`それぞれの上部にある文字列が表示されます、 `ViewPager`、それぞれそのからデータをプルし、`ViewPager`のアダプターが常に同期してため、現在表示されている`View`です。 これらの違いは`PagerTabStrip`中に「現在」の文字列の視覚インジケーターが含まれています`PagerTitleStrip`されません (示すようにこれらのスクリーン ショットでは) には。 

[![PagerTitleStrip と PagerTabStrip TreePager アプリのスクリーン ショット](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

このガイドを示します immplement 方法`ViewPager`アダプター、およびインジケーターのアプリのコンポーネントと統合するジェスチャー ナビゲーションをサポートします。 



## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
