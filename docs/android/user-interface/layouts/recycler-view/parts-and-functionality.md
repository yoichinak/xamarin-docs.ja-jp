---
title: RecyclerView のパーツと機能
description: RecyclerView レイアウトマネージャー、アダプター、およびビューホルダーの概要を説明します。
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 89b7f70ae69987edbd465d669f1bac17ddebc7c8
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522445"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView のパーツと機能


`RecyclerView`は、一部のタスクを内部で処理します (たとえば、ビューのスクロールやリサイクルなど)。ただし、基本的には、コレクションを表示するようにヘルパークラスを調整するマネージャーです。 `RecyclerView`次のヘルパークラスにタスクを委任します。

- **`Adapter`** 増え項目のレイアウト (レイアウトファイルの内容をインスタンス化し`RecyclerView`ます) を行い、内に表示されるビューにデータをバインドします。 &ndash; アダプターは、項目クリックイベントも報告します。

- **`LayoutManager`** 内の項目ビューを測定および配置し、ビューのリサイクルのポリシーを管理します。`RecyclerView` &ndash;

- **`ViewHolder`** &ndash;ビュー参照を検索して格納します。 ビューホルダーは、項目ビューのクリックを検出するのにも役立ちます。

- **`ItemDecoration`** &ndash;項目、強調表示、および視覚的なグループ化の境界間の区切り線を描画するために、アプリが特定のビューに特殊な描画とレイアウトのオフセットを追加できるようにします。

- **`ItemAnimator`** &ndash;項目の操作中に実行されるアニメーション、またはアダプターに変更が加えられたときに行われるアニメーションを定義します。

`RecyclerView` 、`LayoutManager`、および`Adapter`クラス間のリレーションシップを次の図に示します。

![アダプターを使用してデータセットにアクセスする LayoutManager を含む RecyclerView の図](parts-and-functionality-images/01-recyclerview-diagram.png)

この図に示すように`LayoutManager` 、は、 `Adapter`と`RecyclerView`の間の仲介役と考えることができます。 は`LayoutManager` `Adapter` 、の`RecyclerView`代わりにメソッドの呼び出しを行います。 たとえば`LayoutManager` 、は、内の`Adapter` `RecyclerView`特定の項目位置に対して新しいビューを作成するときに、メソッドを呼び出します。 増え`Adapter`は、その項目のレイアウトを作成し`ViewHolder` 、その位置にあるビューへの参照をキャッシュするインスタンスを作成します (表示されません)。 がを`LayoutManager` `Adapter`呼び出して特定の項目をデータセットにバインドすると、はその項目のデータを検索し、データセットから取得して、関連付けられている項目ビューにコピーします。 `Adapter`

アプリで`RecyclerView`を使用する場合は、次のクラスの派生型を作成する必要があります。

- **`RecyclerView.Adapter`** アプリのデータセット (アプリに固有のもの) から`RecyclerView`内に表示される項目ビューへのバインディングを提供します。 &ndash; アダプターは、内の各項目ビューの位置を、 `RecyclerView`データソース内の特定の場所に関連付ける方法を認識しています。 さらに、アダプターは個々の項目ビュー内のコンテンツのレイアウトを処理し、各ビューのビューホルダーを作成します。 また、このアダプターは、項目ビューによって検出された項目クリックイベントも報告します。

- **`RecyclerView.ViewHolder`** &ndash;項目レイアウトファイル内のビューへの参照をキャッシュして、リソース参照が不必要に繰り返されないようにします。 また、ビューホルダーは、ユーザーがビューホルダーに関連付けられている項目ビューをタップしたときに、アダプターに転送される項目クリックイベントを配置します。

- **`RecyclerView.LayoutManager`** 内に項目を`RecyclerView`配置します。 &ndash; 定義済みのレイアウトマネージャーの1つを使用することも、独自のカスタムレイアウトマネージャーを実装することもできます。
    `RecyclerView`レイアウトマネージャーにレイアウトポリシーを委任します。これにより、アプリに大きな変更を加えることなく、別のレイアウトマネージャーをプラグインできます。

また、必要に応じて次のクラスを拡張して、アプリ内`RecyclerView`ののルックアンドフィールを変更することもできます。

- **`RecyclerView.ItemDecoration`**
- **`RecyclerView.ItemAnimator`**

`RecyclerView`および`ItemDecoration` を拡張しない場合、では既定の実装が使用されます。`ItemAnimator` このガイドではカスタムを作成する方法については説明しません`ItemDecoration`と`ItemAnimator`クラスですこれらのクラスの詳細については、次を参照してください。 [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html)と[RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)。


<a name="recycling" />

### <a name="how-view-recycling-works"></a>ビューのリサイクルのしくみ

`RecyclerView`は、データソース内のすべての項目に対して項目ビューを割り当てません。 代わりに、画面に表示される項目ビューの数のみを割り当て、ユーザーがスクロールするときにそれらの項目のレイアウトを再利用します。 ビューが最初に表示されなくなると、次の図に示すように、リサイクルプロセスが実行されます。

[![ビューのリサイクルの6つの手順を示す図](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1. ビューが表示されなくなり、表示されなくなった場合は、*スクラップビュー*になります。

2. スクラップビューはプールに配置され、*リサイクルビュー*になります。
    このプールは、同じ種類のデータを表示するビューのキャッシュです。

3. 新しい項目を表示すると、再利用のためにリサイクルプールからビューが取得されます。 このビューは、アダプターによって表示される前に再バインドされる必要があるため、*ダーティビュー*と呼ばれます。

4. ダーティビューがリサイクルされます。アダプターは、次に表示される項目のデータを検索し、このデータをこの項目のビューにコピーします。 これらのビューの参照は、リサイクルビューに関連付けられているビューホルダーから取得されます。

5. 再生ビューは、 `RecyclerView`画面に表示されるの項目の一覧に追加されます。

6. ユーザーがを一覧の次の項目`RecyclerView`にスクロールすると、リサイクルされたビューが画面上に表示されます。 一方、別のビューが表示されなくなり、上記の手順に従ってリサイクルされます。

項目ビューの再利用に加えて`RecyclerView` 、では、もう1つの効率の最適化 (ビューホルダー) も使用します。 *ビューホルダー*は、ビュー参照をキャッシュする単純なクラスです。 アダプターが項目レイアウトファイルを増えするたびに、対応するビューホルダーも作成されます。 ビューホルダーは、 `FindViewById`を使用して、拡大された項目レイアウトファイル内のビューへの参照を取得します。 これらの参照は、新しいデータを表示するためにレイアウトがリサイクルされるたびに、新しいデータをビューに読み込むために使用されます。
 


### <a name="the-layout-manager"></a>レイアウトマネージャー

レイアウトマネージャーは、 `RecyclerView`表示に項目を配置する役割を担います。これは、プレゼンテーションの種類 (リストまたはグリッド)、向き (項目が縦または横に表示されるかどうか)、および表示する方向項目を決定します。(通常の順序で、または逆順に)。 レイアウトマネージャーは、 **RecycleView**表示の各項目のサイズと位置を計算する役割も担います。

レイアウトマネージャーには、追加の目的があります。これは、ユーザーに表示されなくなった項目ビューをいつリサイクルするかのポリシーを決定します。
レイアウトマネージャーは、表示されるビュー (およびではないビュー) を認識しているため、ビューをいつリサイクルできるかを決定するのに最適な位置にあります。 ビューをリサイクルするには、通常、レイアウトマネージャーがアダプターを呼び出して、再生ビューの内容を別のデータに置き換えます。これについては、「[ビューのリサイクルのしくみ](#recycling)」を参照してください。

を拡張`RecyclerView.LayoutManager`して独自のレイアウトマネージャーを作成することも、定義済みのレイアウトマネージャーを使用することもできます。 `RecyclerView`には、次の定義済みのレイアウトマネージャーが用意されています。

- **`LinearLayoutManager`** &ndash;垂直方向にスクロールできる列または水平方向にスクロールできる行で項目を配置します。

- **`GridLayoutManager`** &ndash;項目をグリッドに表示します。

- **`StaggeredGridLayoutManager`** &ndash;項目を交互のグリッドに表示します。一部の項目の高さと幅が異なります。

レイアウトマネージャーを指定するには、選択した`SetLayoutManager`レイアウトマネージャーをインスタンス化し、メソッドに渡します。 既定では 、レイアウトマネージャー &ndash; `RecyclerView`が定義済みのレイアウトマネージャーを選択しないことを指定する必要があることに注意してください。

レイアウトマネージャーの詳細については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)を参照してください。


### <a name="the-view-holder"></a>ビューホルダー

ビューの所有者は、ビュー参照をキャッシュするために定義するクラスです。 アダプターは、これらのビュー参照を使用して、各ビューをそのコンテンツにバインドします。 内のすべての`RecyclerView`項目には、その項目のビュー参照をキャッシュするビューホルダーインスタンスが関連付けられています。 ビューホルダーを作成するには、次の手順を使用して、項目ごとのビューの正確なセットを保持するクラスを定義します。

1. サブ`RecyclerView.ViewHolder`クラスです。
2. ビュー参照を検索して格納するコンストラクターを実装します。
3. アダプターがこれらの参照にアクセスするために使用できるプロパティを実装します。

`ViewHolder`実装の詳細な例については、[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)を参考にしてください。
の詳細`RecyclerView.ViewHolder`については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)を参照してください。


### <a name="the-adapter"></a>アダプター

`RecyclerView`統合コードの大部分は、アダプターで実行されます。 `RecyclerView`では、から`RecyclerView.Adapter`派生したアダプターを使用してデータソースにアクセスし、各項目にデータソースのコンテンツを設定する必要があります。
データソースはアプリに固有であるため、データへのアクセス方法を理解するアダプターの機能を実装する必要があります。 アダプターは、データソースから情報を抽出し、 `RecyclerView`コレクション内の各項目に読み込みます。

次の図は、アダプターがデータソースのコンテンツをビューホルダーを通じて、の`RecyclerView`各行項目内の個々のビューにマップする方法を示しています。

[![データソースを ViewHolders に接続するアダプターを示す図](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

アダプターは、各行`RecyclerView`に特定の行項目のデータを読み込みます。 たとえば、行の位置*p*の場合、アダプターはデータソース内の位置*p*にある関連データを検索し、このデータを`RecyclerView`コレクション内の*p*位置の行項目にコピーします。
たとえば、上の図では、アダプターはビューの所有者を使用して、 `ImageView`その位置でと`TextView`の参照を検索します。その`FindViewById`ため、ユーザーがコレクションをスクロールするときに、これらのビューに対してを繰り返し呼び出す必要はありません。ビューを再利用します。

アダプターを実装する場合は、次`RecyclerView.Adapter`のメソッドをオーバーライドする必要があります。

- **`OnCreateViewHolder`** &ndash;項目レイアウトファイルとビューホルダーをインスタンス化します。

- **`OnBindViewHolder`** &ndash;指定したビューホルダーに参照が格納されているビューに、指定した位置にあるデータを読み込みます。

- **`ItemCount`** &ndash;データソース内の項目の数を返します。

レイアウトマネージャーは、内に項目を配置するときに、 `RecyclerView`これらのメソッドを呼び出します。 



### <a name="notifying-recyclerview-of-data-changes"></a>データ変更の RecyclerView への通知

`RecyclerView`は、データソースの内容が変更されたときに、表示を自動的に更新しません。アダプターは、データ`RecyclerView`セットが変更されたときに通知を受け取る必要があります。 データセットはさまざまな方法で変更できます。たとえば、項目内の内容が変更されたり、データの全体的な構造が変更される可能性があります。
`RecyclerView.Adapter`では、最も効率的な方法でデータ変更に`RecyclerView`応答するために呼び出すことができるメソッドが多数用意されています。

- **`NotifyItemChanged`** &ndash;指定した位置にある項目が変更されたことを通知します。

- **`NotifyItemRangeChanged`** &ndash;指定された位置の範囲内の項目が変更されたことを通知します。

- **`NotifyItemInserted`** &ndash;指定した位置にある項目が新しく挿入されたことを通知します。

- **`NotifyItemRangeInserted`** &ndash;指定された位置の範囲内の項目が新しく挿入されたことを通知します。

- **`NotifyItemRemoved`** &ndash;指定した位置の項目が削除されたことを通知します。

- **`NotifyItemRangeRemoved`** &ndash;指定された位置の範囲内の項目が削除されたことを通知します。

- **`NotifyDataSetChanged`** &ndash;データセットが変更されたことを通知します (完全な更新を強制的に実行します)。

データセットがどのように変更されたかを正確に把握している場合は`RecyclerView` 、上記の適切なメソッドを呼び出して、最も効率的な方法で更新することができます。 データセットがどのように変更されたかわからない場合は、 `NotifyDataSetChanged`を呼び出すことができます`RecyclerView` 。これは、ユーザーに表示されるすべてのビューを更新する必要があるためです。 これらのメソッドの詳細については、「 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)」を参照してください。

次のトピック「[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)」では、上記のパーツと機能の実際のコード例を示すために、サンプルアプリが実装されています。


## <a name="related-links"></a>関連リンク

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
