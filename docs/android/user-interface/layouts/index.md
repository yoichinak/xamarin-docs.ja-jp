---
title: レイアウト
description: Xamarin.Android アプリの視覚的な構造を定義します。
ms.prod: xamarin
ms.assetid: 2BA72B0E-230D-4F98-B4D5-4EFB0D479789
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/18/2017
ms.openlocfilehash: e0a9ce52d70079884e7960ccfee9eb7fcbb0f2fb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61311504"
---
# <a name="layouts"></a>レイアウト

レイアウトは、画面 (アクティビティ) などの UI インターフェイスを構成する要素の配置に使用されます。 次のセクションでは、Xamarin.Android アプリで最もよく使われるレイアウトを使用する方法を説明します。

-   [LinearLayout](~/android/user-interface/layouts/linear-layout.md)は垂直方向または水平方向に線形の方向にビューの子要素を表示するビューのグループです。

    ![線形レイアウトの例](images/linear-layout.png)

-   [[相対レイアウト]](~/android/user-interface/layouts/relative-layout.md)は相対位置でビューの子要素を表示するビューのグループです。 ビューの位置は、兄弟要素に対して相対的に指定できます。

    ![相対的なレイアウトの例](images/relative-layout.png)

-   [TableLayout](~/android/user-interface/layouts/table-layout.md)は行と列でビューの子要素を表示するビューのグループです。

    ![テーブル レイアウトの例](images/table-layout.png)

-   [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)がリストまたはコレクションをスクロールするユーザーを有効にすると、グリッド項目のコレクションを表示する UI 要素。

    ![Recycler ビューの例](images/recycler-view.png)

-   [ListView](~/android/user-interface/layouts/list-view/index.md)はスクロール可能な項目のリストを作成するビューのグループです。 リスト項目はリストのアダプターを使用してリストに自動的に挿入されます。 `ListView` Android アプリケーションの重要な UI コンポーネントは、連絡先またはインターネットのお気に入りの長い一覧にメニュー オプションの短いリストからすべての場所で使用されています。 いずれかを組み込みスタイルで書式設定したりできる広範なカスタマイズが行のスクロール リストに表示する簡単な方法を提供します。 ListView のインスタンスでは、行ビューに含まれるデータを渡すためには、アダプターが必要です。

    ![リスト ビューの例](images/list-view.png)

-   [GridView](~/android/user-interface/layouts/grid-view.md)がスクロール可能な 2 次元グリッド内の項目を表示する UI 要素。

    ![グリッド ビューの例](images/grid-view.png)

-   [GridLayout](~/android/user-interface/layouts/grid-layout.md)は、HTML テーブルと同様に、2 次元グリッド ビューのレイアウトをサポートするビューのグループです。

    ![グリッド レイアウトの例](images/grid-layout.png)

-   [レイアウトをタブ付き](~/android/user-interface/layouts/tab-layout/index.md)モバイル アプリケーションの一般的なユーザー インターフェイスのパターンを簡潔さと使いやすさのため、します。 アプリケーションのさまざまな画面間を移動する一貫性のある簡単な方法を提供します。

    ![タブ付きレイアウトの例](images/tabbed-layout.png)
 
