---
title: ViewPager
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、フラグメントの有無にかかわらず、ViewPager で gestural ナビゲーションを実装する方法について説明します。 また、Pagerタイトルストリップと PagerTabStrip を使用してページインジケーターを追加する方法についても説明します。
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: c7718ef7a02365e9ca09f7491804cbadfa0c9a41
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940858"
---
# <a name="viewpager"></a>ViewPager

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、フラグメントの有無にかかわらず、ViewPager で gestural ナビゲーションを実装する方法について説明します。また、Pagerタイトルストリップと PagerTabStrip を使用してページインジケーターを追加する方法についても説明します。_

## <a name="overview"></a>の概要

アプリ開発の一般的なシナリオは、兄弟ビュー間の gestural ナビゲーションをユーザーに提供する必要があることです。 この方法では、ユーザーは、(セットアップウィザードやスライドショーなどで) コンテンツのページにアクセスするための左または右にスワイプます。 これらのスワイプビューは、 [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)で入手できる `ViewPager` ウィジェットを使用して作成できます。 `ViewPager` は、複数の子ビューで構成されるレイアウトウィジェットで、各子ビューがレイアウト内のページを構成します。 

[TreePager アプリのスクリーンショットを ![横方向のスワイプの例](images/01-intro-sml.png)](images/01-intro.png#lightbox)

通常、`ViewPager` は[フラグメント](~/android/platform/fragments/index.md)と組み合わせて使用されます。ただし、`Fragment`s の複雑さを増すことなく `ViewPager` を使用することが必要になる状況もあります。

`ViewPager` は、アダプターパターンを使用して表示するビューを提供します。 ここで使用するアダプターは、概念的には[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)が使用するアダプターと似ていますが、`PagerAdapter` の実装を指定して、`ViewPager` がユーザーに表示するページを生成 &ndash; ます。 `ViewPager` によって表示されるページは、s または `Fragment``View`にすることができます。 `View`が表示されると、アダプターは Android の `PagerAdapter` 基底クラスをサブクラス化します。 `Fragment`が表示される場合、アダプターは Android の `FragmentPagerAdapter`をサブクラス化します。 Android サポートライブラリには `FragmentPagerAdapter` (`PagerAdapter`のサブクラス) も含まれており、データへの `Fragment`の接続の詳細に役立ちます。 

このガイドでは、次の両方の方法を示します。 

- [Viewpager とビューが](~/android/user-interface/controls/view-pager/viewpager-and-views.md)あるので、 [treepager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)アプリは、`ViewPager` を使用してツリーカタログのビューを表示する方法を示すために開発されています (広葉樹および evergreen ツリーのイメージギャラリー)。 
    `PagerTabStrip` と `PagerTitleStrip` は、ページナビゲーションに役立つタイトルを表示するために使用されます。

- [Viewpager とフラグメント](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)を使用すると、より複雑な[FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)アプリが作成され、`Fragment`s で `ViewPager` を使用して、計算問題をフラッシュカードとして表示し、ユーザー入力に応答するアプリを構築する方法を示すことができます。 

## <a name="requirements"></a>要件

アプリプロジェクトで `ViewPager` を使用するには、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)パッケージをインストールする必要があります。 NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください。 

## <a name="architecture"></a>アーキテクチャ

`ViewPager`を使用した gestural ナビゲーションの実装には、次の3つのコンポーネントが使用されます。

- ViewPager
- アダプタ
- ページャーインジケーター

これらの各コンポーネントの概要を次に示します。

### <a name="viewpager"></a>ViewPager

`ViewPager` は、一度に1つの `View`のコレクションを表示するレイアウトマネージャーです。 そのジョブでは、ユーザーのスワイプジェスチャを検出し、必要に応じて次または前のビューに移動します。 たとえば、次のスクリーンショットは、ユーザージェスチャに応じて、あるイメージから次の画像への切り替えを行う `ViewPager` を示しています。 

[ビュー間の切り替えを表示する TreePager アプリの ![クローズアップ](images/02-transition-sml.png)](images/02-transition.png#lightbox)

### <a name="adapter"></a>アダプタ

`ViewPager` は、*アダプター*からデータをプルします。 アダプターのジョブは、`ViewPager`によって表示される `View`を作成し、必要に応じて提供します。 次の図は、アダプターが `View`s を作成して設定し、`ViewPager`に提供する &ndash; この概念を示しています。 `ViewPager` がユーザーのスワイプジェスチャを検出すると、表示する適切な `View` をアダプターに提供するように求められます。 

[アダプターが画像と名前を ViewPager に接続する方法を示す ![ダイアグラム](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

この例では、各 `View` は、`ViewPager`に渡される前に、ツリーイメージとツリー名から構築されます。 

### <a name="pager-indicator"></a>ページャーインジケーター

`ViewPager` を使用すると、大きなデータセットを表示できます (たとえば、イメージギャラリーには数百のイメージが含まれる場合があります)。 ユーザーが大きなデータセットを移動できるように、多くの場合、`ViewPager` には、文字列を表示する*ページャーインジケーター*が付属しています。 文字列には、画像のタイトル、キャプション、または単にデータセット内の現在のビューの位置を指定できます。 

このナビゲーション情報を生成できるビューは2つあります。 `PagerTabStrip` と `PagerTitleStrip.` はそれぞれ `ViewPager`の上部に文字列を表示し、それぞれが現在表示されている `View`と常に同期されるように、`ViewPager`のアダプターからデータをプルします。 これらの違いは、`PagerTabStrip` には、次のスクリーンショットに示すように、"current" `PagerTitleStrip` 文字列の視覚的なインジケーターが含まれている点です。 

[Pagerタイトルストリップと PagerTabStrip を使用した TreePager アプリのスクリーンショットの ![](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

このガイドでは、`ViewPager`、アダプター、およびインジケーターアプリコンポーネントを immplement し、gestural ナビゲーションをサポートするように統合する方法について説明します。 

## <a name="related-links"></a>関連リンク

- [TreePager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
- [FlashCardPager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
