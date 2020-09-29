---
title: ViewPager
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、フラグメントの有無にかかわらず、ViewPager で gestural ナビゲーションを実装する方法について説明します。 また、Pagerタイトルストリップと PagerTabStrip を使用してページインジケーターを追加する方法についても説明します。
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 427ace2043f966b617a258b5f50fa42f943e707e
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457654"
---
# <a name="viewpager"></a>ViewPager

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、フラグメントの有無にかかわらず、ViewPager で gestural ナビゲーションを実装する方法について説明します。また、Pagerタイトルストリップと PagerTabStrip を使用してページインジケーターを追加する方法についても説明します。_

## <a name="overview"></a>概要

アプリ開発の一般的なシナリオは、兄弟ビュー間の gestural ナビゲーションをユーザーに提供する必要があることです。 この方法では、ユーザーは、(セットアップウィザードやスライドショーなどで) コンテンツのページにアクセスするための左または右にスワイプます。 これらのスワイプビューは、 `ViewPager` [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)で入手できるウィジェットを使用して作成できます。 は、 `ViewPager` 複数の子ビューで構成されるレイアウトウィジェットで、各子ビューがレイアウト内のページを構成します。 

[![横方向のスワイプを使用した TreePager アプリのスクリーンショットの例](images/01-intro-sml.png)](images/01-intro.png#lightbox)

通常、 `ViewPager` は [フラグメント](~/android/platform/fragments/index.md)と組み合わせて使用されます。ただし、を複雑にすることなく、を使用する必要がある状況もあり `ViewPager` `Fragment` ます。

`ViewPager` アダプターパターンを使用して、表示するビューを指定します。 ここで使用するアダプターは、 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; の実装を指定して、が `PagerAdapter` ユーザーに表示するページを生成する RecyclerView によって使用されるものと概念的に似てい `ViewPager` ます。 によって表示されるページは、 `ViewPager` `View` s または `Fragment` s です。 `View`が表示されると、アダプターは Android の基本クラスをサブクラスにし `PagerAdapter` ます。 が表示される場合 `Fragment` 、アダプターは Android のをサブクラス化し `FragmentPagerAdapter` ます。 Android サポートライブラリには、 `FragmentPagerAdapter` データへの接続の詳細を確認するための (サブクラス) も含まれてい `PagerAdapter` `Fragment` ます。 

このガイドでは、次の両方の方法を示します。 

- [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)。 [treepager](/samples/xamarin/monodroid-samples/userinterface-treepager)アプリは、を使用してツリーカタログのビューを表示する方法を示すために開発されてい `ViewPager` ます (広葉樹および evergreen ツリーのイメージギャラリー)。 
    `PagerTabStrip`  および `PagerTitleStrip` は、ページナビゲーションに役立つタイトルを表示するために使用されます。

- [Viewpager とフラグメント](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)を使用すると、より複雑な[FlashCardPager](/samples/xamarin/monodroid-samples/userinterface-flashcardpager)アプリが作成され、を使用し `ViewPager` て、 `Fragment` 計算問題をフラッシュカードとして表示し、ユーザー入力に応答するアプリをビルドする方法を示すことができます。 

## <a name="requirements"></a>必要条件

アプリプロジェクトでを使用するには `ViewPager` 、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) パッケージをインストールする必要があります。 NuGet パッケージのインストールの詳細については、「 [チュートリアル: プロジェクトに nuget を含める](/visualstudio/mac/nuget-walkthrough)」を参照してください。 

## <a name="architecture"></a>アーキテクチャ

Gestural ナビゲーションの実装には、次の3つのコンポーネントが使用され `ViewPager` ます。

- ViewPager
- アダプター
- ページャーインジケーター

これらの各コンポーネントの概要を次に示します。

### <a name="viewpager"></a>ViewPager

`ViewPager` は、一度に1つののコレクションを表示するレイアウトマネージャーです `View` 。 そのジョブでは、ユーザーのスワイプジェスチャを検出し、必要に応じて次または前のビューに移動します。 たとえば、次のスクリーンショットは、 `ViewPager` ユーザージェスチャに応じて1つのイメージから次の画像に遷移することを示しています。 

[![ビュー間の切り替えを表示する TreePager アプリのクローズアップ](images/02-transition-sml.png)](images/02-transition.png#lightbox)

### <a name="adapter"></a>アダプター

`ViewPager`*アダプター*からデータをプルします。 アダプターのジョブは `View` 、によって表示されるを作成し `ViewPager` 、必要に応じて提供します。 次の図は、 &ndash; アダプターがを作成して設定し、に提供するこの概念を示してい `View` `ViewPager` ます。 は `ViewPager` ユーザーのスワイプジェスチャを検出すると、アダプターに対して、表示する適切なを指定するように求め `View` ます。 

[![アダプターが画像と名前を ViewPager に接続する方法を示す図](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

この例では `View` 、に渡される前に、各がツリーイメージとツリー名から構築されてい `ViewPager` ます。 

### <a name="pager-indicator"></a>ページャーインジケーター

`ViewPager` は、大きなデータセットを表示するために使用できます (たとえば、イメージギャラリーには数百のイメージが含まれる場合があります)。 ユーザーが大きなデータセットを移動できるように、には、多くの場合、 `ViewPager` 文字列を表示する *ページャーインジケーター* が付属しています。 文字列には、画像のタイトル、キャプション、または単にデータセット内の現在のビューの位置を指定できます。 

このナビゲーション情報を生成するビューが2つあります。 `PagerTabStrip` とは `PagerTitleStrip.` それぞれの先頭に文字列を表示し、それぞれが現在表示されていると `ViewPager` 常に同期さ `ViewPager` れるように、それぞれのアダプターからデータをプルします `View` 。 これらの違いは、に `PagerTabStrip` は含まれて `PagerTitleStrip` いません (これらのスクリーンショットに示すように)。 

[![Pagerタイトルストリップと PagerTabStrip を使用した TreePager アプリのスクリーンショット](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

このガイド `ViewPager` では、immplement、アダプター、およびインジケーターアプリコンポーネントを、gestural ナビゲーションをサポートするように統合する方法について説明します。 

## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](/samples/xamarin/monodroid-samples/userinterface-treepager)
- [FlashCardPager (サンプル)](/samples/xamarin/monodroid-samples/userinterface-flashcardpager)