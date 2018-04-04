---
title: RecyclerView
description: RecyclerView はコレクションを表示するため、グループの表示ListView、GridView などの古いグループの表示の代わりに柔軟に設計されています。  このガイドでは、使用および Xamarin.Android アプリケーションで RecyclerView をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: 187339244d53c154cc22672a3d2ceba7e0a75bcf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView はコレクションを表示するため、グループの表示ListView、GridView などの古いグループの表示の代わりに柔軟に設計されています。このガイドでは、使用および Xamarin.Android アプリケーションで RecyclerView をカスタマイズする方法について説明します。_

## <a name="recyclerview"></a>RecyclerView

多くのアプリは、同じ種類 (メッセージ、連絡先、画像、または曲); などのコレクションを表示する必要があります。多くの場合、このコレクションは、コレクションがコレクション内のすべての項目をスムーズにスクロールできる小さいウィンドウに表示されるように、画面に収まるようには大きすぎます。
`RecyclerView` リストまたはユーザー コレクションのスクロールを有効にすると、グリッド項目のコレクションを表示する Android ウィジェットがします。 使用する例のアプリのスクリーン ショットを次に示します`RecyclerView`垂直スクロール リストに電子メールの受信トレイの内容を表示します。

[![受信トレイ メッセージの一覧を RecyclerView を使用した例のアプリ](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` 2 つの魅力的な機能を提供します。

-  推奨されるコンポーネントに接続してその動作を変更できる、柔軟なアーキテクチャを備えています。

-  大規模なコレクションと効率的なアイテムのビューを再利用を使用する必要*保持者を表示*ビューの参照をキャッシュします。

このガイドを使用する方法について説明`RecyclerView`Xamarin.Android アプリケーションに追加する方法について説明します、 `RecyclerView` Xamarin.Android プロジェクトとパッケージの説明方法`RecyclerView`一般的なアプリケーションで機能します。 統合する方法を表示する実際のコード例が提供されて`RecyclerView`、アプリケーション、item ビューをクリックしを実装する方法、および更新する方法に`RecyclerView`その基になるデータが変更されたとき。 このガイドでは、Xamarin.Android 開発に慣れていることを前提としています。


### <a name="requirements"></a>要件

`RecyclerView`は Android 5.0 ロリポップに関連付けられている多くの場合が提供されるサポート ライブラリとして&ndash;`RecyclerView`はアプリと連携するターゲット API レベル (Android 2.1) 7 以降。 次は、使用する必要は`RecyclerView`Xamarin ベースのアプリケーションで。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  アプリ プロジェクトを含める必要があります、 **Xamarin.Android.Support.v7.RecyclerView**パッケージです。 NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)です。


### <a name="overview"></a>概要

`RecyclerView` 代わりとして考えることができます、`ListView`と`GridView`Android でのウィジェット。 などの先行タスク`RecyclerView`小さいウィンドウに大きなデータ セットを表示するよう設計されていますが、`RecyclerView`より多くのレイアウト オプションを提供し、大規模なコレクションを表示するためにより適切に最適化します。 慣れて場合`ListView`、いくつかの重要な違いがある`ListView`と`RecyclerView`:

-   `RecyclerView` 使用するよりも多少複雑: を使用するコードを記述する必要が`RecyclerView`と比較して`ListView`です。

-   `RecyclerView` 定義済みのアダプターは行わないデータ ソースにアクセスするアダプター コードを実装する必要があります。 ただし、Android にを使用するいくつかの定義済みアダプターが含まれます`ListView`と`GridView`です。

-   `RecyclerView` ユーザーが、項目をタップしたときに項目をクリックしてイベントを提供しません代わりに、アイテムのクリック イベントは、ヘルパー クラスによって処理されます。 これに対し、`ListView`項目をクリックしてイベントを提供します。

-   `RecyclerView` ビューを再利用し、不要なレイアウト リソース検索を不要になるビューの所有者のパターンを適用することでパフォーマンスが向上します。 ビューの所有者のパターンの使用は省略可能で`ListView`です。

-   `RecyclerView` モジュールの設計をカスタマイズするが容易に基づいています。 たとえば、アプリに大幅なコード変更しない別のレイアウト ポリシーで接続できます。
    これに対し、`ListView`構造で比較的モノリシックはします。

-   `RecyclerView` 項目の追加し、削除の組み込みのアニメーションが含まれます。 `ListView` アニメーションでは、いくつか追加の労力アプリが必要です。


### <a name="sections"></a>セクション

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView パーツおよび機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

このトピックの内容について説明しますが、どのように`Adapter`、`LayoutManager`と`ViewHolder`をサポートするヘルパー クラスと連携`RecyclerView`です。
これらのヘルパー クラスの概要を提供し、アプリでの使用方法について説明します。

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[RecyclerView の基本的な例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

提供される情報に基づいて、このトピック[RecyclerView パーツおよび機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)方法の実際のコード例を提供することによって、さまざまな`RecyclerView`要素は、実際のフォト参照アプリのビルドに実装されます。

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView 例を拡張します。](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

このトピックに表示される例のアプリに追加のコードを追加する[A の基本的な RecyclerView 例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)項目をクリックしてイベントを処理し、更新する方法を示す`RecyclerView`基になるデータ ソースの変更。


### <a name="summary"></a>まとめ

このガイドには、Android が導入された`RecyclerView`ウィジェットを追加する方法が説明されている、 `RecyclerView` Xamarin.Android プロジェクトにライブラリがどのようにサポート`RecyclerView`効率を高めるため、ビューの所有者のパターンを適用する方法と、ビューがリサイクルされるさまざまな構成するヘルパー クラス`RecyclerView`コレクションを表示する共同作業します。 これを示すためのコード例が用意されて方法`RecyclerView`が統合されてを調整する方法を説明するアプリケーションにその`RecyclerView`異なるレイアウト マネージャー、およびそれにプラグインすることによってレイアウト ポリシー項目を処理する方法を説明イベント をクリックし、を通知`RecyclerView`のデータ ソースの変更。

詳細については`RecyclerView`を参照してください、 [RecyclerView クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)です。


## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
