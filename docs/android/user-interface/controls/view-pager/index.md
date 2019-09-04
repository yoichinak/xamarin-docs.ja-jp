---
title: ViewPager
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、フラグメントの有無にかかわらず、ViewPager で gestural ナビゲーションを実装する方法について説明します。 また、Pagerタイトルストリップと PagerTabStrip を使用してページインジケーターを追加する方法についても説明します。
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: eca0f82fd967c28bffc8f20bcc9e2ec6bb3ba737
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227704"
---
# <a name="viewpager"></a>ViewPager

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、フラグメントの有無にかかわらず、ViewPager で gestural ナビゲーションを実装する方法について説明します。また、Pagerタイトルストリップと PagerTabStrip を使用してページインジケーターを追加する方法についても説明します。_

 
## <a name="overview"></a>概要

アプリ開発の一般的なシナリオは、兄弟ビュー間の gestural ナビゲーションをユーザーに提供する必要があることです。 この方法では、ユーザーは、(セットアップウィザードやスライドショーなどで) コンテンツのページにアクセスするための左または右にスワイプます。 これらのスワイプビューは、 `ViewPager` [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)で入手できるウィジェットを使用して作成できます。 は`ViewPager` 、複数の子ビューで構成されるレイアウトウィジェットで、各子ビューがレイアウト内のページを構成します。 

[![横方向のスワイプを使用した TreePager アプリのスクリーンショットの例](images/01-intro-sml.png)](images/01-intro.png#lightbox)

通常、 `ViewPager`は[フラグメント](~/android/platform/fragments/index.md)と組み合わせて使用されます。ただし、を複雑`ViewPager` `Fragment`にすることなく、を使用する必要がある状況もあります。

`ViewPager`アダプターパターンを使用して、表示するビューを指定します。 ここで使用するアダプターは、の`PagerAdapter`実装を指定して、 `ViewPager`がユーザーに表示するページを生成する[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash;によって使用されるものと概念的に似ています。 によって`ViewPager` `View`表示されるページは`Fragment`、s または s です。 が`View`表示されると、アダプターは Android の`PagerAdapter`基本クラスをサブクラスにします。 が`Fragment`表示される場合、アダプターは Android の`FragmentPagerAdapter`をサブクラス化します。 Android サポートライブラリには、 `FragmentPagerAdapter`データへの接続`PagerAdapter` `Fragment`の詳細を確認するための (サブクラス) も含まれています。 

このガイドでは、次の両方の方法を示します。 

- [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)。 [treepager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)アプリは、を使用`ViewPager`してツリーカタログのビューを表示する方法を示すために開発されています (広葉樹および evergreen ツリーのイメージギャラリー)。 
    `PagerTabStrip`および`PagerTitleStrip`は、ページナビゲーションに役立つタイトルを表示するために使用されます。

- [Viewpager とフラグメント](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)を使用すると、より複雑な[FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)アプリが作成され、 `ViewPager`を`Fragment`使用して、計算問題をフラッシュカードとして表示し、ユーザー入力に応答するアプリをビルドする方法を示すことができます。 


## <a name="requirements"></a>必要条件

アプリプロジェクト`ViewPager`でを使用するには、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)パッケージをインストールする必要があります。 NuGet パッケージのインストールの詳細について[は、「チュートリアル:プロジェクト](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)に NuGet を含めます。 

 
## <a name="architecture"></a>アーキテクチャ

Gestural ナビゲーションの実装には、次の`ViewPager`3 つのコンポーネントが使用されます。

- ViewPager
- アダプター
- ページャーインジケーター

これらの各コンポーネントの概要を次に示します。



### <a name="viewpager"></a>ViewPager

`ViewPager`は、一度に1つのの`View`コレクションを表示するレイアウトマネージャーです。 そのジョブでは、ユーザーのスワイプジェスチャを検出し、必要に応じて次または前のビューに移動します。 たとえば、次のスクリーンショットは、 `ViewPager`ユーザージェスチャに応じて1つのイメージから次の画像に遷移することを示しています。 

[![ビュー間の切り替えを表示する TreePager アプリのクローズアップ](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>アダプター

`ViewPager`*アダプター*からデータをプルします。 アダプターのジョブは、 `View` `ViewPager`によって表示されるを作成し、必要に応じて提供します。 次の図は、アダプター &ndash;がを作成`View`して設定し、に提供する`ViewPager`この概念を示しています。 は`ViewPager`ユーザーのスワイプジェスチャを検出すると、アダプターに対して、表示する`View`適切なを指定するように求めます。 

[![アダプターが画像と名前を ViewPager に接続する方法を示す図](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

この例では、 `View` `ViewPager`に渡される前に、各がツリーイメージとツリー名から構築されています。 



### <a name="pager-indicator"></a>ページャーインジケーター

`ViewPager`は、大きなデータセットを表示するために使用できます (たとえば、イメージギャラリーには数百のイメージが含まれる場合があります)。 ユーザーが大きなデータセットを移動できるよう`ViewPager`に、には、多くの場合、文字列を表示する*ページャーインジケーター*が付属しています。 文字列には、画像のタイトル、キャプション、または単にデータセット内の現在のビューの位置を指定できます。 

このナビゲーション情報を生成するビューが2つあります。 `PagerTabStrip`と`PagerTitleStrip.`は、それぞれの先頭`ViewPager`に文字列を表示し、それぞれがのアダプターから`ViewPager`データをプルします。これにより、常に、現在表示さ`View`れています。 これらの違い`PagerTabStrip`は、に`PagerTitleStrip`は含まれていません (これらのスクリーンショットに示すように)。 

[![Pagerタイトルストリップと PagerTabStrip を使用した TreePager アプリのスクリーンショット](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

このガイドでは、immplement `ViewPager`、アダプター、およびインジケーターアプリコンポーネントを、gestural ナビゲーションをサポートするように統合する方法について説明します。 



## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
- [FlashCardPager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
