---
title: RecyclerView
description: RecyclerView は、ビュー グループはコレクションを表示します。ListView、GridView などの古いビュー グループ代わりとしてより柔軟に設計されています。  このガイドを使用して Xamarin.Android アプリケーションで RecyclerView をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 1332a7ef5b8e5bb2f1178582bcf058123f1e177c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110148"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView は、ビュー グループはコレクションを表示します。ListView、GridView などの古いビュー グループ代わりとしてより柔軟に設計されています。このガイドを使用して Xamarin.Android アプリケーションで RecyclerView をカスタマイズする方法について説明します。_

## <a name="recyclerview"></a>RecyclerView

多くのアプリが必要 (メッセージ、連絡先、画像、または曲); など、同じ型のコレクションを表示するには多くの場合、このコレクションはコレクション内のすべての項目をスムーズにスクロールできる小さなウィンドウで、コレクションが表示されるように、画面に収まるようには大きすぎます。
`RecyclerView` リストまたはコレクションをスクロールするユーザーを有効にすると、グリッド内の項目のコレクションを表示する Android のウィジェット。 使用するアプリの例のスクリーン ショットを次に`RecyclerView`垂直スクロール リストに電子メールの受信トレイの内容を表示します。

[![受信トレイ メッセージの一覧を RecyclerView を使用したアプリの例](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` 2 つの魅力的な機能を提供します。

-  推奨されるコンポーネントに接続してその動作を変更することができます、柔軟なアーキテクチャがあります。

-  アイテムのビューを再利用しの使用が必要ですので、大きなコレクションが効率的です*ホルダー表示*ビューの参照をキャッシュします。

このガイドを使用する方法について説明`RecyclerView`で Xamarin.Android アプリケーションを追加する方法について説明します、 `RecyclerView` 、Xamarin.Android プロジェクトとパッケージの説明方法`RecyclerView`一般的なアプリケーションでの関数。 統合する方法について説明する実際のコード例が提供されている`RecyclerView`アプリケーションやアイテム ビューをクリックしを実装する方法を更新する方法を`RecyclerView`その基になるデータが変更されたとき。 このガイドでは、Xamarin.Android の開発に精通していることを前提としています。


### <a name="requirements"></a>必要条件

`RecyclerView`は多くの場合、Android 5.0 Lollipop に関連付けられている、サポート ライブラリとして提供されて&ndash;`RecyclerView`そのターゲット API レベル 7 (Android 2.1) をアプリで動作およびそれ以降。 使用する、次が必要な`RecyclerView`Xamarin ベースのアプリケーションで。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  アプリ プロジェクトを含める必要があります、 **Xamarin.Android.Support.v7.RecyclerView**パッケージ。 NuGet パッケージのインストールの詳細については、[チュートリアル: NuGet でプロジェクトを含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)を参照してください。


### <a name="overview"></a>概要

`RecyclerView` 代わりとして考えることができます、`ListView`と`GridView`Android のウィジェット。 などの以前のバージョン`RecyclerView`小さなウィンドウで、大規模なデータ セットを表示するのには設計されていますが、`RecyclerView`他のレイアウト オプションを提供し、大規模なコレクションを表示するために最適化されたより。 慣れて場合`ListView`のいくつかの重要な違いがある`ListView`と`RecyclerView`:

-   `RecyclerView` 使用するは少し複雑です。 使用するコードを記述する必要が`RecyclerView`と比較すると`ListView`。

-   `RecyclerView` 定義済みのアダプターは示しません。データ ソースにアクセスするアダプター コードを実装する必要があります。 ただし、Android で動作するいくつかの定義済みのアダプターが含まれます。`ListView`と`GridView`します。

-   `RecyclerView` 項目をタップすると、項目のクリック イベントを提供していません代わりに、項目クリック イベントは、ヘルパー クラスによって処理されます。 これに対し、`ListView`項目のクリック イベントを提供します。

-   `RecyclerView` ビューを再利用し、不要なレイアウトのリソース ルックアップを排除するビューの所有者のパターンを適用することによってパフォーマンスが向上します。 ビューの所有者のパターンの使用はオプションで`ListView`します。

-   `RecyclerView` カスタマイズしやすくモジュラー デザインに基づいています。 たとえば、大幅なコードを変更せず、別のレイアウト ポリシーでアプリにプラグインできます。
    これに対し、`ListView`構造に比較的モノリシックです。

-   `RecyclerView` 項目の追加し、削除の組み込みのアニメーションが含まれます。 `ListView` アニメーションでは、アプリ開発者の方にいくつか追加の作業が必要です。


### <a name="sections"></a>セクション

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

このトピックで説明する方法、 `Adapter`、`LayoutManager`と`ViewHolder`をサポートするヘルパー クラスとして共同作業`RecyclerView`。
これらのヘルパー クラスのそれぞれの大まかな概要を提供し、アプリでの使用方法について説明します。

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

このトピックで提供される情報に基づいて[RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)方法の実際のコード例を提供することで、さまざまな`RecyclerView`写真ブラウザー アプリを現実世界を構築する要素が実装されます。

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView の例を拡張](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

このトピックで示した例のアプリに追加のコードを追加します[RecyclerView の例の基本的な A](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)項目のクリック イベントを処理し、更新する方法を示す`RecyclerView`基になるデータ ソースの変更。


### <a name="summary"></a>まとめ

このガイドには、Android が導入された`RecyclerView`ウィジェットを追加する方法について説明し、 `RecyclerView` Xamarin.Android プロジェクトにライブラリをどのようにサポート`RecyclerView`効率を高めるため、ビューの所有者のパターンを適用する方法と、ビューを再利用、さまざまなヘルパー クラスを構成する`RecyclerView`コレクションを表示する共同作業を行います。 示すためにコード例が提供されている方法`RecyclerView`が統合されてアプリケーションに合わせてカスタマイズする方法について説明しました`RecyclerView`別のレイアウト マネージャー、およびそのプラグインすることによってレイアウトのポリシーは項目を処理する方法について説明しましたイベントをクリックし、通知`RecyclerView`のデータ ソースの変更。

詳細については`RecyclerView`を参照してください、 [RecyclerView クラス参照](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)します。


## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
