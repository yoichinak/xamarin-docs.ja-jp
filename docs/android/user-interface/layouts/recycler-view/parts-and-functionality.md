---
title: RecyclerView のパーツと機能
description: RecyclerView レイアウト マネージャー、アダプター、およびビューの所有者の概要。
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 13678d3b1bca102e6f608ad1c11838db1f14cd08
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105000"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView のパーツと機能


`RecyclerView` ハンドルが、内部的に (など、スクロール ビューの再利用)、一部のタスクは、基本的にコレクションを表示するヘルパー クラスを調整するマネージャーです。 `RecyclerView` 次のヘルパー クラスにタスクを委任するには。

-   **`Adapter`** &ndash; 項目のレイアウトを拡張 (レイアウト ファイルの内容をインスタンス化します) 内に表示されるビューにデータをバインドし、`RecyclerView`します。 アダプターでは、項目クリック イベントをレポートします。

-   **`LayoutManager`** &ndash; 測定し、内のアイテムのビューを配置、`RecyclerView`およびビューのリサイクルのポリシーを管理します。

-   **`ViewHolder`** &ndash; 検索し、ビューの参照を格納します。 Item ビュー数回のクリックを検出すると、ビューの所有者にも役立ちます。

-   **`ItemDecoration`** &ndash; アプリに許可する特定のビュー項目、ハイライト、およびグループ化の表示の境界間の区分線を描画するために特殊な描画とレイアウトのオフセットを追加します。

-   **`ItemAnimator`** &ndash; アイテムの操作時に発生するか、アダプターに加えた変更に応じてアニメーションを定義します。

間のリレーションシップ、 `RecyclerView`、 `LayoutManager`、および`Adapter`クラスは、次の図に示すようにします。

![RecyclerView が LayoutManager を含む、データ セットにアクセスするアダプターを使用してのダイアグラム](parts-and-functionality-images/01-recyclerview-diagram.png)

この図に示すように、`LayoutManager`間の仲介者として考えることができます、 `Adapter` 、`RecyclerView`します。 `LayoutManager`を呼び出す`Adapter`メソッドの代わりに、`RecyclerView`します。 たとえば、`LayoutManager`呼び出し、`Adapter`メソッド内で特定のアイテムの位置の新しいビューの作成に時間がある場合に、`RecyclerView`します。 `Adapter`そのアイテムのレイアウトを拡張し、作成、 `ViewHolder` (非表示) インスタンスでその位置にあるビューに対する参照をキャッシュします。 ときに、`LayoutManager`呼び出し、`Adapter`データ セットに特定のアイテムをバインドする、`Adapter`そのアイテムのデータを検索、データ セットからデータを取得および関連付けられているアイテムのビューにコピーします。

使用する場合`RecyclerView`アプリでは、次のクラスの派生型の作成が必要です。

-   **`RecyclerView.Adapter`** &ndash; (これは、アプリに固有)、アプリのデータ セットから内に表示される項目のビューへのバインドを提供します、`RecyclerView`します。 アダプターで各項目ビューの位置に関連付ける方法を知っている、`RecyclerView`データ ソースの特定の場所にします。 さらに、アダプターは、それぞれ個別のアイテムのビュー内のコンテンツのレイアウトを処理し、それぞれのビューのビューの所有者を作成します。 アダプターでは、item ビューによって検出された項目のクリック イベントをレポートします。

-   **`RecyclerView.ViewHolder`** &ndash; リソースの検索が不必要に繰り返されるしないように、項目のレイアウト ファイルのビューへの参照をキャッシュします。 ビューの所有者は、ビューの所有者の関連付けられているアイテムのビューをタップすると、アダプターに転送される項目のクリック イベント用も配置します。

-   **`RecyclerView.LayoutManager`** &ndash; 内のアイテムを配置、`RecyclerView`します。 いくつかの定義済みのレイアウト マネージャーのいずれかを使用することができますか、独自のカスタム レイアウト マネージャーを実装することができます。
    `RecyclerView` デリゲート レイアウト マネージャー、重要なしなくても、別のレイアウト マネージャーをプラグインできるようにレイアウト ポリシーをアプリに変更します。

外観を変更するのには、次のクラスを必要に応じて拡張することも、`RecyclerView`アプリ。

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

拡張しない場合`ItemDecoration`と`ItemAnimator`、`RecyclerView`既定の実装を使用します。 このガイドではカスタムを作成する方法については説明しません`ItemDecoration`と`ItemAnimator`クラスですこれらのクラスの詳細については、次を参照してください。 [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html)と[RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)。


<a name="recycling" />

### <a name="how-view-recycling-works"></a>リサイクル動作を表示する方法

`RecyclerView` 項目のビューは、データ ソース内のすべての項目を割り当てられません。 代わりに、画面上に収まる項目ビューの数のみを割り当てるし、ユーザーがスクロールそれらの項目のレイアウトを再利用します。 見えないように最初に、ビューをスクロールするときに、次の図のように、リサイクル プロセスをたどります。

[![ビューのリサイクルの 6 つの手順を示す図](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  ビューは、見えないようにまでスクロールし、表示されなくなりますがと、になります、*ビューを破棄*します。

2.  スクラップ ビューには、プールに配置されますなり、*ビューをリサイクル*します。
    このプールは、同じ型のデータを表示するビューのキャッシュです。

3.  表示される新しい項目がある場合は、ビューが再利用するためリサイクル プールから取得されます。 呼び出されたため、アダプターによって表示される前にこのビューを再バインドする必要があります、*ビューをダーティ*します。

4.  ダーティのビューを再利用します。 アダプターがデータを表示するには、次の項目を検索し、この項目のビューにこのデータをコピーします。 これらのビューの参照は、リサイクルのビューに関連付けられたビューの所有者から取得されます。

5.  リサイクル ビュー内の項目の一覧に追加、`RecyclerView`画面上に移動しようとしています。

6.  リサイクルのビューは、ユーザーがスクロール画面に表示されるが、`RecyclerView`の一覧で次の項目にします。 一方で、別のビューは見えないようにスクロールし、上記の手順に従ってリサイクルします。

Item ビューを再利用、に加えて`RecyclerView`も別の効率性の最適化を使用: 所有者を表示します。 A*ビュー所有者*キャッシュ参照を表示する単純なクラスです。 アダプターは、項目のレイアウト ファイルを拡張するたびにも作成対応するビューの所有者。 ビューの所有者を使用して`FindViewById`を高めの項目のレイアウト ファイル内でビューへの参照を取得します。 これらの参照は、レイアウトが新しいデータを表示するリサイクルされるたびに、ビューに新しいデータの読み込みに使用されます。
 


### <a name="the-layout-manager"></a>レイアウト マネージャー

レイアウト マネージャーは、配置内の項目、`RecyclerView`表示; プレゼンテーションの型 (リストまたはグリッド)、(項目が表示されるか垂直方向または水平方向に)、向き、および方向アイテムを表示するかを決定します通常の順序で (逆の順序で)。 内の各項目の位置とサイズを計算するためのレイアウト マネージャーはまた、 **RecycleView**を表示します。

レイアウト マネージャーが、その他の目的: のポリシーをユーザーに表示されなくなった項目のビューをリサイクルするタイミングを決定します。
レイアウト マネージャーは、ビューが表示されます (とサポートされない) の対応であるため、ビューを再利用できる場合を決定する最適な位置になります。 ビューを再利用、レイアウト マネージャー通常にリサイクルされるビューの内容を別のデータに置き換えて、アダプターへの呼び出しで既に説明したよう[ビュー リサイクル動作のしくみ](#recycling)します。

拡張する`RecyclerView.LayoutManager`独自のレイアウトを作成したり、マネージャー、マネージャーを使用できます定義済みのレイアウト。 `RecyclerView` 次の定義済みのレイアウト マネージャーを提供します。

-   **`LinearLayoutManager`** &ndash; 項目を垂直方向にスクロール可能な列または水平方向にスクロール可能な行に配置します。

-   **`GridLayoutManager`** &ndash; グリッド内の項目を表示します。

-   **`StaggeredGridLayoutManager`** &ndash; いくつかの項目が異なる高さと幅がある時間をずらしたグリッドで項目を表示します。

レイアウト マネージャーを指定する、選択したレイアウト マネージャーをインスタンス化に渡すと、`SetLayoutManager`メソッド。 メモした*する必要があります*レイアウト マネージャーを指定&ndash;`RecyclerView`デフォルトで、定義済みのレイアウト マネージャーを選択できません。

レイアウト マネージャーの詳細については、、 [RecyclerView.LayoutManager クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)を参照してください。


### <a name="the-view-holder"></a>ビューの所有者

ビューの所有者は、ビューの参照をキャッシュ用に定義するクラスです。 アダプターでは、これらのビューの参照を使用して、そのコンテンツへそれぞれのビューをバインドします。 すべての項目に、`RecyclerView`そのアイテムのビューの参照をキャッシュする関連付けられているビュー所有者インスタンスがあります。 ビューの所有者を作成するのにには、項目ごとのビューの正確なセットを保持するのにクラスを定義するのに、次の手順を使用します。

1.  サブクラス`RecyclerView.ViewHolder`します。
2.  検索し、ビューの参照を格納するコンス トラクターを実装します。
3.  これらの参照へのアクセスに使用できるアダプターのプロパティを実装します。

詳細な例を`ViewHolder`実装[RecyclerView の例の基本的な A](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)します。
詳細については`RecyclerView.ViewHolder`を参照してください、 [RecyclerView.ViewHolder クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)します。


### <a name="the-adapter"></a>アダプター

「たいへん」なはほとんど、`RecyclerView`統合のコードでは、アダプター内の場所。 `RecyclerView` 派生したアダプターを指定する必要があります`RecyclerView.Adapter`をデータ ソースにアクセスして、データ ソースからコンテンツを持つ各項目を設定します。
データ ソースは、アプリに固有であるため、データにアクセスする方法を理解しているアダプターの機能を実装する必要があります。 アダプターは、データ ソースから情報を抽出し、内の各項目に読み込みます、`RecyclerView`コレクション。

次の図は、アダプターが個々 のビュー内の各の行項目をビューの所有者によってデータ ソース内のコンテンツをマップする方法を示しています、 `RecyclerView`:

[![アダプター ViewHolders へのデータ ソースの接続を示す図](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

アダプターが読み込む各`RecyclerView`特定の行項目のデータを含む行です。 行の位置の*P*、アダプターが関連付けられている位置にあるデータを検索するなど、 *P*位置にある行にこのデータ項目のデータ ソースとコピー内で*P*で、`RecyclerView`コレクション。
上記の図にたとえば、アダプターを使用して、ビューの所有者の参照を検索、`ImageView`と`TextView`を繰り返し呼び出すことがあるないため、その位置にある`FindViewById`ユーザーとしてこれらのビューのコレクションをスクロールし、ビューを再利用します。

アダプターを実装する場合は、次をオーバーライドする必要があります`RecyclerView.Adapter`メソッド。

-   **`OnCreateViewHolder`** &ndash; 項目のレイアウト ファイルとビューの所有者をインスタンス化します。

-   **`OnBindViewHolder`** &ndash; 指定されたビューの所有者の参照が格納されているビューには、指定した位置にあるデータを読み込みます。

-   **`ItemCount`** &ndash; データ ソース内の項目の数を返します。

レイアウト マネージャー内のアイテムの配置中にメソッドが呼び出される、`RecyclerView`します。 



### <a name="notifying-recyclerview-of-data-changes"></a>RecyclerView のデータの変更を通知します。

`RecyclerView` 自動的に更新されない表示内容のデータ ソースの変更;アダプターに通知する必要があります`RecyclerView`データ セット内の変更がある場合。 多くの点で、データ セットを変更することができます。たとえば、項目内の内容を変更できます。 またはデータの全体的な構造を変更することがあります。
`RecyclerView.Adapter` 呼び出すことのできるメソッドを多数提供しますように`RecyclerView`最も効率的な方法でデータの変更に応答します。

-  **`NotifyItemChanged`** &ndash; 指定した位置にある項目が変更されたことを通知します。

-  **`NotifyItemRangeChanged`** &ndash; 位置の指定した範囲内の項目が変更されたことを通知します。

-  **`NotifyItemInserted`** &ndash; 指定された位置に項目が新しく挿入されたことを通知します。

-  **`NotifyItemRangeInserted`** &ndash; 位置の指定した範囲内の項目が新しく挿入されたことを通知します。

-  **`NotifyItemRemoved`** &ndash; 指定された位置に項目が削除されたことを通知します。

-  **`NotifyItemRangeRemoved`** &ndash; 位置の指定した範囲内の項目が削除されていることを通知します。

-  **`NotifyDataSetChanged`** &ndash; データ セットが変更されたことを通知 (完全な更新を強制する)。

データ セットが変更されたか正確にわかっている場合は、更新には、上の適切なメソッドを呼び出すことができます`RecyclerView`最も効率的な方法でします。 データ セットが変更されたか正確にわからない場合は、呼び出す`NotifyDataSetChanged`、あまり効率的であるため、`RecyclerView`をユーザーに表示されているすべてのビューを更新する必要があります。 これらのメソッドの詳細については、[RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)を参照してください。

次のトピックで[A Basic RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)アプリの例は部分と上記で説明した機能の実際のコード例を示すために実装されます。


## <a name="related-links"></a>関連リンク

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
