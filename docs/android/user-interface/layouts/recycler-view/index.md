---
title: RecyclerView
description: RecyclerView は、コレクションを表示するためのビューグループです。ListView や GridView など、以前のビューグループをより柔軟に置き換えることができるように設計されています。  このガイドでは、Xamarin Android アプリケーションで RecyclerView を使用およびカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 7c98686a1aa99e250b3fd1d0fcc6ae64d625a11f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522410"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView は、コレクションを表示するためのビューグループです。ListView や GridView など、以前のビューグループをより柔軟に置き換えることができるように設計されています。このガイドでは、Xamarin Android アプリケーションで RecyclerView を使用およびカスタマイズする方法について説明します。_

## <a name="recyclerview"></a>RecyclerView

多くのアプリでは、同じ種類のコレクション (メッセージ、連絡先、画像、曲など) を表示する必要があります。多くの場合、このコレクションは大きすぎて画面に収まらないため、コレクション内のすべての項目をスムーズにスクロールできる小さなウィンドウにコレクションが表示されます。
`RecyclerView`は、リストまたはグリッド内の項目のコレクションを表示し、ユーザーがコレクションをスクロールできるようにする Android ウィジェットです。 次に示すのは、を使用`RecyclerView`して、電子メールの受信トレイの内容を垂直スクロールリストに表示するサンプルアプリのスクリーンショットです。

[![RecyclerView を使用して受信トレイメッセージを一覧表示するアプリの例](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView`は、次の2つの魅力的な機能を提供します。

- これには柔軟なアーキテクチャがあり、優先するコンポーネントをプラグインすることによって動作を変更できます。

- 大きなコレクションでは、項目ビューを再利用し、ビューの参照をキャッシュするために*ビューホルダー*を使用する必要があるため、効率的です。

このガイドでは、xamarin `RecyclerView` android アプリケーションでを使用する方法について説明し`RecyclerView`ます。このガイドでは、xamarin android プロジェクトにパッケージ`RecyclerView`を追加する方法について説明し、一般的なアプリケーションの機能について説明します。 実際のコード例を使用すると、アプリケーションに`RecyclerView`統合する方法、項目ビューのクリックを実装する方法、基になる`RecyclerView`データが変更されたときに更新する方法を示すことができます。 このガイドでは、Xamarin Android の開発について理解していることを前提としています。


### <a name="requirements"></a>必要条件

は多くの場合 android 5.0 ロリポップに関連付けられていますが&ndash; 、サポートライブラリ`RecyclerView`として提供されており、API レベル 7 (android 2.1) 以降を対象とするアプリで動作します。 `RecyclerView` Xamarin ベースのアプリケーションでを`RecyclerView`使用するには、次のものが必要です。

- **Xamarin android** &ndash; 4.20 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。

- アプリプロジェクトには、 **v7**パッケージが含まれている必要があります。 NuGet パッケージのインストールの詳細について[は、「チュートリアル:プロジェクト](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)に NuGet を含めます。


### <a name="overview"></a>概要

`RecyclerView`は、Android のウィジェット`ListView`および`GridView`ウィジェットの代わりとして考えることができます。 前の例と`RecyclerView`同様に、は小さいウィンドウに大きなデータセットを表示するよう`RecyclerView`に設計されていますが、より多くのレイアウトオプションが用意されており、大規模なコレクションを表示するために最適化されています。 に慣れて`ListView`いる場合は、と`RecyclerView`の間`ListView`にいくつかの重要な違いがあります。

- `RecyclerView`は少し複雑になります。を`RecyclerView` `ListView`使用するには、より多くのコードを記述する必要があります。

- `RecyclerView`は、定義済みのアダプターを提供しません。データソースにアクセスするアダプターコードを実装する必要があります。 ただし、Android には、および`ListView` `GridView`で動作する定義済みのアダプターがいくつか用意されています。

- `RecyclerView`ユーザーが項目をタップしても、項目クリックイベントは提供されません。代わりに、項目クリックイベントはヘルパークラスによって処理されます。 これに対し`ListView` 、では、項目クリックイベントが提供されます。

- `RecyclerView`では、ビューをリサイクルし、ビューホルダーパターンを適用することによってパフォーマンスを向上させます。これにより、不要なレイアウトリソースの参照がなくなります。 で`ListView`は、ビューホルダーパターンの使用は省略可能です。

- `RecyclerView`は、カスタマイズしやすくするモジュール形式の設計に基づいています。 たとえば、アプリにコードを大幅に変更することなく、別のレイアウトポリシーをプラグインできます。
    これに対し`ListView` 、の構造は比較的モノリシックです。

- `RecyclerView`には、項目の追加と削除のための組み込みアニメーションが含まれています。 `ListView`アニメーションでは、アプリ開発者の一部に対して追加の作業が必要になります。


### <a name="sections"></a>セクション

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

このトピックでは、 `Adapter`、 `LayoutManager`、および`ViewHolder`が、をサポート`RecyclerView`するヘルパークラスとして連携する方法について説明します。
これらの各ヘルパークラスの概要と、アプリでのそれらの使用方法について説明します。

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

このトピックでは、実際の写真閲覧アプリを構築するためにさまざまな`RecyclerView`要素がどのように実装されるかについて実際のコード例を提供することで、RecyclerView の[パーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)について説明します。

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

このトピックでは、[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)に示されているサンプルアプリにコードを追加して、項目クリック`RecyclerView`イベントを処理し、基になるデータソースが変更されたときに更新する方法を示します。


### <a name="summary"></a>まとめ

このガイドでは、 `RecyclerView` android ウィジェットについて説明し`RecyclerView`ました。これは、Xamarin android プロジェクト`RecyclerView`にサポートライブラリを追加する方法、ビューを再利用する方法、効率のためにビューホルダーパターンを適用する方法、およびさまざまな方法について説明しました。コレクションを表示するため`RecyclerView`に共同作業を行うヘルパークラス。 ここでは、をアプリケーションに`RecyclerView`統合する方法を示すコード例を紹介し、 `RecyclerView`さまざまなレイアウトマネージャーをプラグインしてレイアウトポリシーを調整する方法について説明し、項目クリックイベントを処理して通知`RecyclerView`する方法について説明しました。データソースの変更。

の詳細`RecyclerView`については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)を参照してください。


## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
