---
title: RecyclerView パーツおよび機能
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 44f042359c9363f8ec3008ee49d2f6a874983e12
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView パーツおよび機能


`RecyclerView` ですが、内部的に (など、スクロールとビューのリサイクル)、一部のタスク ハンドルは、基本的にコレクションを表示するヘルパー クラスを調整するマネージャーです。 `RecyclerView` 次のヘルパー クラスにデリゲート タスク:

-   **`Adapter`** &ndash; 項目のレイアウトの拡張 (レイアウト ファイルの内容をインスタンス化) 内に表示されるビューにデータをバインドし、`RecyclerView`です。 アダプターでは、アイテムのクリック イベントも報告されます。

-   **`LayoutManager`** &ndash; 測定し、内のアイテムのビューを配置、`RecyclerView`およびビューのリサイクルのポリシーを管理します。

-   **`ViewHolder`** &ndash; 検索し、ビューの参照を格納します。 Item ビューのクリックを検出すると、ビューの所有者にも役立ちます。

-   **`ItemDecoration`** &ndash; 項目、強調表示、およびビジュアル化の境界間の区分線を描画するための特定のビューに特殊な描画とレイアウトのオフセットを追加することをアプリを許可します。

-   **`ItemAnimator`** &ndash; アイテムの操作中に実行または変更が行われたとき、アダプターにアニメーションを定義します。

間のリレーションシップ、 `RecyclerView`、 `LayoutManager`、および`Adapter`クラスは、次の図に示されています。

![RecyclerView LayoutManager を含む、データ セットにアクセスするアダプターを使用してのダイアグラム](parts-and-functionality-images/01-recyclerview-diagram.png)

次の図に示すように、`LayoutManager`間の仲介者として考えることができます、`Adapter`と`RecyclerView`です。 `LayoutManager`を呼び出す`Adapter`メソッドの代わりに、`RecyclerView`です。 たとえば、`LayoutManager`呼び出し、`Adapter`メソッドは、特定の項目の位置での新しいビューを作成する際に、`RecyclerView`です。 `Adapter`その項目のレイアウトを拡張し、作成、 `ViewHolder` (非表示) のインスタンスをその位置にあるビューへの参照をキャッシュします。 ときに、`LayoutManager`呼び出し、`Adapter`データ セットに特定のアイテムをバインドする、`Adapter`その項目のデータを検索、データ セットから取得し、関連付けられているアイテムのビューにコピーします。

使用する場合`RecyclerView`アプリでは、次のクラスの派生型を作成する必要は。

-   **`RecyclerView.Adapter`** &ndash; (これは、アプリに固有の) アプリのデータ セットから内に表示されるアイテムのビューへのバインドを提供、`RecyclerView`です。 アダプター内の各項目の表示位置に関連付ける方法を知っている、`RecyclerView`データ ソース内の特定の場所にします。 さらに、アダプターは、それぞれ個々 のアイテムのビュー内の内容のレイアウトを処理し、各ビューのビューの所有者を作成します。 アダプターは、item ビューによって検出された項目をクリックしてイベントにも報告されます。

-   **`RecyclerView.ViewHolder`** &ndash; リソースの検索が不必要に繰り返されませんできるように、項目のレイアウト ファイル内のビューへの参照をキャッシュします。 ビューの所有者は、項目クリック イベントは、ユーザーがビューの所有者の関連付けられているアイテムのビューをタップしたときに、アダプターに転送されるのも配置します。

-   **`RecyclerView.LayoutManager`** &ndash; 内のアイテムを配置、`RecyclerView`です。 いくつかの定義済みのレイアウト マネージャーのいずれかを使用することができますか、独自のカスタム レイアウト マネージャーを実装することができます。
    `RecyclerView` デリゲート レイアウト マネージャー、重要なしなくても、別のレイアウト マネージャーにプラグインできるようにレイアウト ポリシーはアプリに変更します。

またの外観を変更するのには、次のクラスを拡張できます必要に応じて`RecyclerView`アプリ。

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

拡張しない場合`ItemDecoration`と`ItemAnimator`、`RecyclerView`既定の実装を使用します。 このガイドではカスタムを作成する方法については説明しません`ItemDecoration`と`ItemAnimator`クラスですこれらのクラスの詳細については、次を参照してください。 [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html)と[RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)。


<a name="recycling" />

### <a name="how-view-recycling-works"></a>リサイクル動作を表示する方法

`RecyclerView` item ビューは、データ ソース内のすべての項目に割り当てられません。 代わりに、画面に収まる項目ビューの数のみを割り当てるし、ユーザーがスクロールそれらの項目のレイアウトを再利用します。 ビューは、まず見えないスクロールするときに、次の図に示したリサイクル プロセスをたどります。

[![ビューのリサイクルの 6 つの手順を示すダイアグラム](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  ビューは見えないまでスクロールし、表示されなくなりますがと、になります、*ビューを破棄*です。

2.  スクラップ ビューには、プールに配置されますなり、*ビューをリサイクル*です。
    このプールは、同じ型のデータを表示するビューのキャッシュです。

3.  新しい項目は、表示されるが、ビューが再利用するためリサイクル プールから取得されます。 このビューは、表示される前にアダプターによって再バインドする必要があります、ために呼び出されます、*ビューをダーティ*です。

4.  ダーティ ビューが再利用します。 アダプターは、次の項目の表示されるデータを検索し、このアイテムのビューにこのデータをコピーします。 これらのビューの参照は、リサイクルのビューに関連付けられているビュー所有者から取得されます。

5.  リサイクル ビュー内の項目の一覧に追加、`RecyclerView`画面上に移動する直前にあります。

6.  リサイクルのビューは、ユーザーがスクロール画面に表示されるが、`RecyclerView`一覧で次の項目にします。 一方で、別のビューは見えないまでスクロールし、上記の手順に従ってリサイクルします。

Item ビューを再利用、に加えて`RecyclerView`別の効率性の最適化を使用して、また: 保持者を表示します。 A*ビュー所有者*キャッシュ参照の表示を単純なクラスです。 アダプターが、項目のレイアウト ファイルを拡張するたびにも作成、対応する表示者です。 ビューの所有者を使用して`FindViewById`を高めの項目のレイアウト ファイル内のビューへの参照を取得します。 これらの参照は、レイアウトが新しいデータを表示するリサイクルされるたびに、ビューに新しいデータの読み込みに使用されます。
 


### <a name="the-layout-manager"></a>レイアウト マネージャー

レイアウト マネージャーは内の項目の位置を`RecyclerView`表示; プレゼンテーションの種類 (リストまたはグリッド)、(項目が表示されるか垂直または水平方向に)、印刷の向き、および方向項目を表示するかを決定通常の順序で (逆の順序で)。 内の各項目の位置とサイズを計算するためのレイアウト マネージャーはまた、 **RecycleView**を表示します。

レイアウト マネージャーが、その他の目的: ユーザーに表示されなくなった項目のビューを再利用する際のポリシーを決定します。
レイアウト マネージャーは、どのビューが表示されます (およびされていない) の対応であるために、ビューを再利用できる場合を決定する最適な位置にあります。 ビューをリサイクルするレイアウト マネージャー通常によりにリサイクルされるビューの内容を別のデータに置き換えて、アダプターへの呼び出しで既に述べたよう[ビュー リサイクル動作のしくみ](#recycling)です。

拡張できます`RecyclerView.LayoutManager`を独自のレイアウトを作成したり、マネージャー、マネージャーを使用できます定義済みのレイアウトです。 `RecyclerView` 次の定義済みのレイアウト マネージャーを提供します。

-   **`LinearLayoutManager`** &ndash; 垂直方向にスクロール可能な列または行方向にスクロール可能な行の項目を並べ替えます。

-   **`GridLayoutManager`** &ndash; グリッド内の項目を表示します。

-   **`StaggeredGridLayoutManager`** &ndash; 一部の項目が別の高さと幅がある、交互グリッド内の項目を表示します。

レイアウト マネージャーを指定する、選択されているレイアウト マネージャーをインスタンス化に渡すと、`SetLayoutManager`メソッドです。 メモした*必要があります*レイアウト マネージャーを指定&ndash;`RecyclerView`既定で、定義済みのレイアウト マネージャーは選択できません。

レイアウト マネージャーの詳細については、次を参照してください。、 [RecyclerView.LayoutManager クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)です。


### <a name="the-view-holder"></a>ビューの所有者

ビューの所有者は、ビューの参照をキャッシュ用に定義するクラスです。 アダプターは、そのコンテンツへの各ビューをバインドするのにこれらのビューの参照を使用します。 すべての項目に、`RecyclerView`そのアイテムのビューの参照をキャッシュする関連付けられているビュー所有者インスタンスが存在します。 ビューの所有者を作成するには、項目ごとのビューの正確なセットを保持するのにクラスを定義するのに、次の手順を使用します。

1.  サブクラス`RecyclerView.ViewHolder`です。
2.  検索し、ビューの参照を格納するコンス トラクターを実装します。
3.  これらの参照にアクセスするアダプターが使用できるプロパティを実装します。

詳細な例、`ViewHolder`で実装が表示される[A の基本的な RecyclerView 例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)です。
詳細については`RecyclerView.ViewHolder`を参照してください、 [RecyclerView.ViewHolder クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)です。


### <a name="the-adapter"></a>アダプター

ほとんどの「たいへん」な`RecyclerView`統合コードは、アダプターで行われます。 `RecyclerView` 派生したアダプターを指定する必要があります`RecyclerView.Adapter`データ ソースへのアクセスおよびデータ ソースからコンテンツを持つ各項目を設定します。
データ ソースは、アプリに固有であるため、データにアクセスする方法を理解しているアダプターの機能を実装する必要があります。 アダプターは、データ ソースから情報を抽出し、内の各項目に読み込みます、`RecyclerView`コレクション。

次の図は、アダプターが、ビューの所有者によってデータ ソース内のコンテンツを内の各の行項目内の個々 のビューにマップする方法を示しています、 `RecyclerView`:

[![アダプター ViewHolders にデータ ソースを接続する方法を示す図](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

アダプターが読み込む各`RecyclerView`特定の行アイテムのデータを含む行です。 行の位置の*P*、アダプターが関連付けられている位置にあるデータを検索するなど、 *P*位置にある行にこのデータ項目のデータ ソースおよびコピー内で*P*で、`RecyclerView`コレクション。
上記の図でなど、アダプターを使用して、ビューの所有者の参照を検索、`ImageView`と`TextView`を繰り返し呼び出すしないように、その位置にある`FindViewById`ユーザーとしてこれらのビューのコレクションをスクロールし、ビューを再利用します。

アダプターを実装する際に、次をオーバーライドする必要があります`RecyclerView.Adapter`メソッド。

-   **`OnCreateViewHolder`** &ndash; 項目のレイアウト ファイルとビューの所有者をインスタンス化します。

-   **`OnBindViewHolder`** &ndash; 指定した位置にあるデータを指定されたビューの所有者での参照が格納されているビューに読み込みます。

-   **`ItemCount`** &ndash; データ ソース内の項目数を返します。

レイアウト マネージャー内のアイテムを配置には、メソッドが呼び出される、`RecyclerView`です。 



### <a name="notifying-recyclerview-of-data-changes"></a>RecyclerView データの変更を通知します。

`RecyclerView` 自動的に更新されないの表示、データのコンテンツ ソースの変更;アダプターに通知する必要があります`RecyclerView`データ セット内で変更がある場合。 多くの点で、データ セットを変更できます。たとえば、項目内の内容を変更することができます。 またはデータの全体的な構造を変更することがあります。
`RecyclerView.Adapter` 多数の呼び出すことのできるメソッドを提供できるように`RecyclerView`最も効率的な方法でデータの変更に応答します。

-  **`NotifyItemChanged`** &ndash; 指定した位置にある項目が変更されたことを通知します。

-  **`NotifyItemRangeChanged`** &ndash; 位置の指定した範囲内の項目が変更されたことを通知します。

-  **`NotifyItemInserted`** &ndash; 指定された位置に項目が新しく挿入されたことを通知します。

-  **`NotifyItemRangeInserted`** &ndash; 位置の指定した範囲内の項目が新しく挿入されたことを通知します。

-  **`NotifyItemRemoved`** &ndash; 指定された位置に項目が削除されたことを通知します。

-  **`NotifyItemRangeRemoved`** &ndash; 位置の指定した範囲内の項目が削除されていることを通知します。

-  **`NotifyDataSetChanged`** &ndash; データ セットが変更されたことを通知 (完全な更新を強制)。

正確にどのように、データ セットが変更された場合は、更新は、上の適切なメソッドを呼び出すことができます`RecyclerView`最も効率的な方法でします。 正確にどのように、データ セットが変更されたかがわからない場合は、呼び出す`NotifyDataSetChanged`、あまり効率的であるため`RecyclerView`をユーザーに表示されているすべてのビューを更新する必要があります。 これらのメソッドの詳細については、次を参照してください。 [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)です。

次のトピックで[A の基本的な RecyclerView 例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)例のアプリは、パーツと上記で説明した機能の実際のコード例を示すために実装されています。


## <a name="related-links"></a>関連リンク

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView の基本的な例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView 例を拡張します。](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
