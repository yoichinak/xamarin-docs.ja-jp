---
title: RecyclerView のパーツと機能
description: RecyclerView レイアウトマネージャー、アダプター、およびビューホルダーの概要を説明します。
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: 823215b7cc1bac8a8f6bffc6e480400135e88ce8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028818"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView のパーツと機能

`RecyclerView` は、一部のタスクを内部で処理します (たとえば、ビューのスクロールやリサイクル)。ただし、基本的には、コレクションを表示するようにヘルパークラスを調整するマネージャーです。 `RecyclerView` は、次のヘルパークラスにタスクを委任します。

- **`Adapter`** &ndash; 増え項目レイアウト (レイアウトファイルの内容をインスタンス化) し、`RecyclerView`内に表示されるビューにデータをバインドします。 アダプターは、項目クリックイベントも報告します。

- **`LayoutManager`** &ndash; は、`RecyclerView` 内の項目ビューを測定および配置し、ビューのリサイクルのポリシーを管理します。

- **`ViewHolder`** &ndash; はビュー参照を検索して格納します。 ビューホルダーは、項目ビューのクリックを検出するのにも役立ちます。

- **`ItemDecoration`** &ndash; を使用すると、アプリは、項目、強調表示、および視覚的なグループ化の境界の間に区切り線を描画するために、特殊な描画およびレイアウトオフセットを特定のビューに追加できます。

- **`ItemAnimator`** &ndash; は、項目の操作中に実行されるアニメーションや、アダプターに変更が加えられたときに行われるアニメーションを定義します。

`RecyclerView`、`LayoutManager`、および `Adapter` クラス間のリレーションシップを次の図に示します。

![アダプターを使用してデータセットにアクセスする LayoutManager を含む RecyclerView の図](parts-and-functionality-images/01-recyclerview-diagram.png)

この図に示すように、`LayoutManager` は `Adapter` と `RecyclerView`間の仲介者と考えることができます。 `LayoutManager` によって、`RecyclerView`の代わりに `Adapter` メソッドが呼び出されます。 たとえば、`LayoutManager` は、`RecyclerView`内の特定の項目位置に対して新しいビューを作成するときに、`Adapter` メソッドを呼び出します。 `Adapter` は、その項目のレイアウトを増えし、その位置にあるビューへの参照をキャッシュするための `ViewHolder` インスタンス (表示されません) を作成します。 `LayoutManager` が `Adapter` を呼び出して特定の項目をデータセットにバインドする場合、`Adapter` はその項目のデータを検索し、データセットから取得して、関連付けられている項目ビューにコピーします。

アプリで `RecyclerView` を使用する場合は、次のクラスの派生型を作成する必要があります。

- **`RecyclerView.Adapter`** &ndash; は、アプリのデータセット (アプリに固有) から `RecyclerView`内に表示される項目ビューへのバインディングを提供します。 アダプターは、`RecyclerView` 内の各項目ビューの位置を、データソース内の特定の場所に関連付ける方法を認識します。 さらに、アダプターは個々の項目ビュー内のコンテンツのレイアウトを処理し、各ビューのビューホルダーを作成します。 また、このアダプターは、項目ビューによって検出された項目クリックイベントも報告します。

- **`RecyclerView.ViewHolder`** &ndash; は、リソース参照が不必要に繰り返されないように、項目レイアウトファイル内のビューへの参照をキャッシュします。 また、ビューホルダーは、ユーザーがビューホルダーに関連付けられている項目ビューをタップしたときに、アダプターに転送される項目クリックイベントを配置します。

- **`RecyclerView.LayoutManager`** &ndash; `RecyclerView`内の項目を配置します。 定義済みのレイアウトマネージャーの1つを使用することも、独自のカスタムレイアウトマネージャーを実装することもできます。
    レイアウトマネージャーにレイアウトポリシーを委任する `RecyclerView`、アプリに大幅な変更を加えることなく、別のレイアウトマネージャーをプラグインできます。

また、必要に応じて次のクラスを拡張して、アプリでの `RecyclerView` のルックアンドフィールを変更することもできます。

- **`RecyclerView.ItemDecoration`**
- **`RecyclerView.ItemAnimator`**

`ItemDecoration` と `ItemAnimator`を拡張しない場合、`RecyclerView` では既定の実装が使用されます。 このガイドでは、カスタム `ItemDecoration` および `ItemAnimator` クラスを作成する方法については説明しません。これらのクラスの詳細については、「 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) 」および「 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)」を参照してください。

<a name="recycling" />

## <a name="how-view-recycling-works"></a>ビューのリサイクルのしくみ

`RecyclerView` は、データソース内のすべての項目に対して項目ビューを割り当てません。 代わりに、画面に表示される項目ビューの数のみを割り当て、ユーザーがスクロールするときにそれらの項目のレイアウトを再利用します。 ビューが最初に表示されなくなると、次の図に示すように、リサイクルプロセスが実行されます。

[ビューのリサイクルの6つの手順を示す![ダイアグラム](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1. ビューが表示されなくなり、表示されなくなった場合は、*スクラップビュー*になります。

2. スクラップビューはプールに配置され、*リサイクルビュー*になります。
    このプールは、同じ種類のデータを表示するビューのキャッシュです。

3. 新しい項目を表示すると、再利用のためにリサイクルプールからビューが取得されます。 このビューは、アダプターによって表示される前に再バインドされる必要があるため、*ダーティビュー*と呼ばれます。

4. ダーティビューがリサイクルされます。アダプターは、次に表示される項目のデータを検索し、このデータをこの項目のビューにコピーします。 これらのビューの参照は、リサイクルビューに関連付けられているビューホルダーから取得されます。

5. 再生ビューが、画面に表示される `RecyclerView` 内の項目の一覧に追加されます。

6. ユーザーが一覧の次の項目に `RecyclerView` をスクロールすると、リサイクルされたビューが画面上に表示されます。 一方、別のビューが表示されなくなり、上記の手順に従ってリサイクルされます。

項目ビューの再利用に加えて、`RecyclerView` もう1つの効率の最適化: ビューホルダーも使用します。 *ビューホルダー*は、ビュー参照をキャッシュする単純なクラスです。 アダプターが項目レイアウトファイルを増えするたびに、対応するビューホルダーも作成されます。 ビューの所有者は、`FindViewById` を使用して、拡大された項目レイアウトファイル内のビューへの参照を取得します。 これらの参照は、新しいデータを表示するためにレイアウトがリサイクルされるたびに、新しいデータをビューに読み込むために使用されます。

## <a name="the-layout-manager"></a>レイアウトマネージャー

レイアウトマネージャーは、`RecyclerView` 表示に項目を配置する役割を担います。このメソッドは、プレゼンテーションの種類 (リストまたはグリッド)、向き (項目を垂直方向または水平方向に表示するかどうか)、および表示する方向 (通常の順序または逆順) を決定します。 レイアウトマネージャーは、 **RecycleView**表示の各項目のサイズと位置を計算する役割も担います。

レイアウトマネージャーには、追加の目的があります。これは、ユーザーに表示されなくなった項目ビューをいつリサイクルするかのポリシーを決定します。
レイアウトマネージャーは、表示されるビュー (およびではないビュー) を認識しているため、ビューをいつリサイクルできるかを決定するのに最適な位置にあります。 ビューをリサイクルするには、通常、レイアウトマネージャーがアダプターを呼び出して、再生ビューの内容を別のデータに置き換えます。これについては、「[ビューのリサイクルのしくみ](#recycling)」を参照してください。

`RecyclerView.LayoutManager` を拡張して独自のレイアウトマネージャーを作成することも、定義済みのレイアウトマネージャーを使用することもできます。 `RecyclerView` には、次の定義済みのレイアウトマネージャーが用意されています。

- **`LinearLayoutManager`** は、垂直方向にスクロールできる列または水平方向にスクロールできる行で項目を整列 &ndash; ます。

- **`GridLayoutManager`** &ndash; では、グリッドに項目が表示されます。

- **`StaggeredGridLayoutManager`** &ndash; では、項目の高さと幅が異なる場合に項目が交互に表示されます。

レイアウトマネージャーを指定するには、選択したレイアウトマネージャーをインスタンス化し、`SetLayoutManager` メソッドに渡します。 既定では、定義済みのレイアウトマネージャーを選択しない `RecyclerView` レイアウトマネージャー &ndash; を指定する*必要が*あることに注意してください。

レイアウトマネージャーの詳細については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)を参照してください。

## <a name="the-view-holder"></a>ビューホルダー

ビューの所有者は、ビュー参照をキャッシュするために定義するクラスです。 アダプターは、これらのビュー参照を使用して、各ビューをそのコンテンツにバインドします。 `RecyclerView` 内のすべてのアイテムには、そのアイテムのビュー参照をキャッシュするビューホルダーインスタンスが関連付けられています。 ビューホルダーを作成するには、次の手順を使用して、項目ごとのビューの正確なセットを保持するクラスを定義します。

1. サブクラス `RecyclerView.ViewHolder`。
2. ビュー参照を検索して格納するコンストラクターを実装します。
3. アダプターがこれらの参照にアクセスするために使用できるプロパティを実装します。

`ViewHolder` 実装の詳細な例については、[基本的な RecyclerView の例を使用](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)して説明します。
`RecyclerView.ViewHolder`の詳細については、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)を参照してください。

## <a name="the-adapter"></a>アダプター

`RecyclerView` 統合コードの大部分は、アダプターで実行されます。 `RecyclerView` を使用するには、`RecyclerView.Adapter` から派生したアダプターを指定してデータソースにアクセスし、各項目にデータソースのコンテンツを設定する必要があります。
データソースはアプリに固有であるため、データへのアクセス方法を理解するアダプターの機能を実装する必要があります。 アダプターは、データソースから情報を抽出し、`RecyclerView` コレクション内の各項目に読み込みます。

次の図は、アダプターがデータソースのコンテンツをビューホルダーを通じて `RecyclerView`の各行項目内の個々のビューにマップする方法を示しています。

[![データソースを ViewHolders に接続するアダプターを示す図](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

アダプターは、特定の行項目のデータを使用して、各 `RecyclerView` 行を読み込みます。 たとえば、行位置*p*の場合、アダプターはデータソース内の位置*p*で関連データを検索し、このデータを `RecyclerView` コレクションの位置*p*の行項目にコピーします。
たとえば、上の図では、アダプターはビューホルダーを使用して `ImageView` の参照を検索し、その位置で `TextView` します。そのため、ユーザーがコレクションをスクロールしてビューを再利用するときに、これらのビューに対して `FindViewById` を繰り返し呼び出す必要はありません。

アダプターを実装する場合は、次の `RecyclerView.Adapter` メソッドをオーバーライドする必要があります。

- **`OnCreateViewHolder`** &ndash; 項目レイアウトファイルとビューホルダーをインスタンス化します。

- **`OnBindViewHolder`** は、指定された位置にあるデータを、指定されたビューホルダーに格納されている参照を持つビューに読み込み &ndash; ます。

- **`ItemCount`** &ndash; は、データソース内の項目の数を返します。

レイアウトマネージャーは、`RecyclerView`内に項目を配置するときに、これらのメソッドを呼び出します。

## <a name="notifying-recyclerview-of-data-changes"></a>データ変更の RecyclerView への通知

`RecyclerView` は、データソースの内容が変更されたときに、表示を自動的に更新しません。アダプターは、データセットが変更されたときに `RecyclerView` に通知する必要があります。 データセットはさまざまな方法で変更できます。たとえば、項目内の内容が変更されたり、データの全体的な構造が変更される可能性があります。
`RecyclerView.Adapter` には、`RecyclerView` が最も効率的な方法でデータ変更に応答するために呼び出すことができるメソッドがいくつか用意されています。

- **`NotifyItemChanged`** &ndash; は、指定された位置にある項目が変更されたことを通知します。

- **`NotifyItemRangeChanged`** &ndash; は、指定された位置の範囲内の項目が変更されたことを通知します。

- **`NotifyItemInserted`** &ndash; は、指定された位置にある項目が新しく挿入されたことを通知します。

- **`NotifyItemRangeInserted`** &ndash; は、指定された位置の範囲内の項目が新しく挿入されたことを通知します。

- **`NotifyItemRemoved`** &ndash; は、指定された位置にある項目が削除されたことを通知します。

- **`NotifyItemRangeRemoved`** &ndash; は、指定された位置の範囲内の項目が削除されたことを通知します。

- **`NotifyDataSetChanged`** &ndash; は、データセットが変更されたことを通知します (完全な更新を強制的に実行します)。

データセットがどのように変更されたかを正確に把握している場合は、上記の適切なメソッドを呼び出して、最も効率的な方法で `RecyclerView` を更新できます。 データセットがどのように変更されたかわからない場合は、`NotifyDataSetChanged`を呼び出すことができます。これは、ユーザーに表示されるすべてのビューを更新 `RecyclerView` 必要があるためです。 これらのメソッドの詳細については、「 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)」を参照してください。

次のトピック「[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)」では、上記のパーツと機能の実際のコード例を示すために、サンプルアプリが実装されています。

## <a name="related-links"></a>関連リンク

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
