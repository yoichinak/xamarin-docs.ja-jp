---
title: ViewPager
description: ViewPager は、レイアウト マネージャー ジェスチャー ナビゲーションを実装することができます。 ジェスチャー ナビゲーションでは、左および右のデータ ページの手順をユーザーがスワイプができます。 このガイドには、ViewPager とフラグメント必要に応じて、ジェスチャーのナビゲーションを実装する方法について説明します。 PagerTitleStrip と PagerTabStrip を使用してページ インジケーターを追加する方法も説明します。
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: bb9795eb1e77a48b01556c553ae19613d6ab6de6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61267605"
---
# <a name="viewpager"></a>ViewPager

_ViewPager は、レイアウト マネージャー ジェスチャー ナビゲーションを実装することができます。ジェスチャー ナビゲーションでは、左および右のデータ ページの手順をユーザーがスワイプができます。このガイドには、ViewPager とフラグメント必要に応じて、ジェスチャーのナビゲーションを実装する方法について説明します。PagerTitleStrip と PagerTabStrip を使用してページ インジケーターを追加する方法も説明します。_

 
## <a name="overview"></a>概要

アプリの開発の一般的なシナリオとしてユーザー ジェスチャー ナビゲーション ビューの兄弟間に提供する必要があります。 この方法では、ユーザーは、左または右に (たとえば、セットアップ ウィザードまたはスライド ショー) 内のコンテンツのアクセス ページを読み取ります。 使用してこれらのスワイプ ビューを作成することができます、`ViewPager`で使用できるウィジェット[Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)します。 `ViewPager`の複数の子ビューから成るレイアウト ウィジェットは、各子ビューがレイアウト内のページを構成します。 

[![水平方向のスワイプの例とスクリーン ショットの TreePager アプリ](images/01-intro-sml.png)](images/01-intro.png#lightbox)

通常、`ViewPager`と組み合わせて使用が[フラグメント](https://developer.xamarin.com/guides/android/platform_features/fragments/)。 ただし、使用する場合があります状況によっては、`ViewPager`の複雑さを増すことなく`Fragment`s。

`ViewPager` 表示するのにビューを提供するのにには、アダプターのパターンを使用します。 ここで使用するアダプターは概念的で使用されているような[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash;の実装を提供する`PagerAdapter`、ページの生成にする、`ViewPager`ユーザーに表示します。 によって表示されるページ`ViewPager`できる`View`s または`Fragment`s。 ときに`View`が表示されます、アダプター サブクラス Android の`PagerAdapter`基本クラス。 場合`Fragment`が表示されます、アダプター サブクラス Android の`FragmentPagerAdapter`します。 Android サポート ライブラリも含まれています。 `FragmentPagerAdapter` (のサブクラス`PagerAdapter`) の接続の詳細を扱いやすく`Fragment`データにします。 

このガイドでは、両方の方法を示しています。 

-   [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)、 [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/)を使用する方法を示すためにアプリが開発`ViewPager`ツリー カタログ (各種と付きまとう木のイメージ ギャラリー) のビューを表示します。 
    `PagerTabStrip`  `PagerTitleStrip`ページ ナビゲーションを支援するタイトルを表示するために使用します。

-   [Viewpager とフラグメント](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)、少し複雑な[FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/)を使用する方法を示すためにアプリを開発`ViewPager`で`Fragment`として数学の問題を表示するアプリをビルドします。フラッシュ カードとユーザー入力に応答します。 


## <a name="requirements"></a>必要条件

使用する`ViewPager`、アプリ プロジェクトでインストールする必要があります、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)パッケージ。 NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル。NuGet をプロジェクトに含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)します。 

 
## <a name="architecture"></a>アーキテクチャ

3 つのコンポーネントがジェスチャーを使用したナビゲーションを実装するため`ViewPager`:

-   ViewPager
-   アダプター
-   ポケットベル インジケーター

これらの各コンポーネントは、以下にまとめています。



### <a name="viewpager"></a>ViewPager

`ViewPager` コレクションを表示するレイアウト マネージャー`View`一度に 1 つの s。 そのジョブでは、ユーザーのスワイプ ジェスチャを検出し、必要に応じて、次または前のビューに移動します。 たとえば、次のスクリーン ショットを示しています、`ViewPager`ユーザー ジェスチャへの応答で 1 つのイメージから、[次へ] への移行を行うこと。 

[![ビューの間の遷移を表示する TreePager のクローズ アップ アプリ](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>アダプター

`ViewPager` データを抽出する*アダプター*します。 アダプターのジョブが作成するには、`View`によって表示された、 `ViewPager`、必要に応じてそれらを提供します。 次の図は、この概念を示しています。&ndash;アダプターの作成および設定します`View`s に提供し、`ViewPager`します。 として、`ViewPager`ユーザーのスワイプ ジェスチャを検出して、適切なアダプターの入力を求められたら`View`を表示します。 

[![アダプターが画像と名前を ViewPager に接続する方法を示す図](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

この例では各`View`に渡される前に、イメージをツリーとツリー名から構築、`ViewPager`します。 



### <a name="pager-indicator"></a>ポケットベル インジケーター

`ViewPager` 大規模なデータ セットを表示するために使用可能性があります (など、イメージ ギャラリーは数百の画像とあります)。 大量のデータを移動するユーザーを支援する`ViewPager`が付属して多くの場合、*ポケットベル インジケーター*文字列を表示します。 文字列には、イメージのタイトル、キャプション、またはデータ セット内の現在のビューの位置だけがあります。 

このナビゲーション情報を生成できる 2 つのビュー:`PagerTabStrip`と`PagerTitleStrip.`それぞれの上部にある文字列が表示されます、 `ViewPager`、しから、そのデータをプルごと、`ViewPager`のアダプターことは常に同期を維持するため、現在表示されている`View`します。 これらの相違点は`PagerTabStrip`中に「現在」の文字列の視覚インジケーターが含まれています`PagerTitleStrip`を実行できません (これらのスクリーン ショットに示すように)。 

[![PagerTitleStrip と PagerTabStrip TreePager アプリのスクリーン ショット](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

このガイドでは示します immplement 方法`ViewPager`アダプター、およびインジケーターのアプリ コンポーネントと統合するジェスチャー ナビゲーションをサポートします。 



## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
