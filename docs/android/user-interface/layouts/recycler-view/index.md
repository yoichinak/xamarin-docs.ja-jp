---
title: RecyclerView
description: RecyclerView は、コレクションを表示するためのビューグループです。ListView や GridView など、以前のビューグループをより柔軟に置き換えることができるように設計されています。  このガイドでは、Xamarin Android アプリケーションで RecyclerView を使用およびカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/03/2018
ms.openlocfilehash: bce89be8bec512ac70ca40015521c7d56f3460d3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028806"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView は、コレクションを表示するためのビューグループです。ListView や GridView など、以前のビューグループをより柔軟に置き換えることができるように設計されています。 このガイドでは、Xamarin Android アプリケーションで RecyclerView を使用およびカスタマイズする方法について説明します。_

## <a name="recyclerview"></a>RecyclerView

多くのアプリでは、同じ種類のコレクション (メッセージ、連絡先、画像、曲など) を表示する必要があります。多くの場合、このコレクションは大きすぎて画面に収まらないため、コレクション内のすべての項目をスムーズにスクロールできる小さなウィンドウにコレクションが表示されます。
`RecyclerView` は、リストまたはグリッド内の項目のコレクションを表示し、ユーザーがコレクションをスクロールできるようにする Android ウィジェットです。 次に示すのは、`RecyclerView` を使用して、電子メールの受信トレイの内容を垂直スクロールリストに表示するサンプルアプリのスクリーンショットです。

[RecyclerView を使用して受信トレイメッセージを一覧表示するアプリの![例](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` には、次の2つの魅力的な機能があります。

- これには柔軟なアーキテクチャがあり、優先するコンポーネントをプラグインすることによって動作を変更できます。

- 大きなコレクションでは、項目ビューを再利用し、ビューの参照をキャッシュするために*ビューホルダー*を使用する必要があるため、効率的です。

このガイドでは、Xamarin Android アプリケーションで `RecyclerView` を使用する方法について説明します。`RecyclerView` パッケージを Xamarin Android プロジェクトに追加する方法について説明し、一般的なアプリケーションでの関数の `RecyclerView` 方法について説明します。 アプリケーションに `RecyclerView` を統合する方法、項目ビューのクリックを実装する方法、基になるデータが変更されたときに `RecyclerView` を更新する方法を示す実際のコード例を示します。 このガイドでは、Xamarin Android の開発について理解していることを前提としています。

### <a name="requirements"></a>［要件］

`RecyclerView` は、多くの場合 Android 5.0 ロリポップに関連付けられていますが、API レベル 7 (Android 2.1) 以降を対象とするアプリで動作 `RecyclerView` &ndash; サポートライブラリとして提供されています。 Xamarin ベースのアプリケーションで `RecyclerView` を使用するには、次のものが必要です。

- Xamarin **android** 4.20 &ndash; Visual Studio または Visual Studio for Mac を使用してインストールおよび構成する必要があります。

- アプリプロジェクトには、 **v7**パッケージが含まれている必要があります。 NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください。

### <a name="overview"></a>概要

`RecyclerView` は、Android の `ListView` ウィジェットと `GridView` ウィジェットの代わりとして考えることができます。 前の例と同様に、`RecyclerView` は小さなウィンドウに大きなデータセットを表示するように設計されていますが、`RecyclerView` はより多くのレイアウトオプションを提供し、大きなコレクションを表示するために最適化されています。 `ListView`に慣れている場合、`ListView` と `RecyclerView`にはいくつかの重要な違いがあります。

- `RecyclerView` の方がやや複雑になります。 `ListView`と比較して `RecyclerView` を使用するには、より多くのコードを記述する必要があります。

- `RecyclerView` は、定義済みのアダプターを提供しません。データソースにアクセスするアダプターコードを実装する必要があります。 ただし、Android には `ListView` と `GridView`で動作する定義済みのアダプターがいくつか用意されています。

- `RecyclerView` は、ユーザーが項目をタップしたときに、項目クリックイベントを提供しません。代わりに、項目クリックイベントはヘルパークラスによって処理されます。 これに対し、`ListView` では、項目クリックイベントが提供されます。

- `RecyclerView` を使用すると、ビューのリサイクルとビューホルダーパターンの適用によってパフォーマンスが向上し、不要なレイアウトリソースの参照がなくなります。 `ListView`では、ビューホルダーパターンの使用は省略可能です。

- `RecyclerView` は、カスタマイズしやすくするモジュール形式の設計に基づいています。 たとえば、アプリにコードを大幅に変更することなく、別のレイアウトポリシーをプラグインできます。
    これに対し、`ListView` は構造において比較的モノリシックです。

- `RecyclerView` には、項目の追加と削除のための組み込みのアニメーションが含まれています。 `ListView` アニメーションでは、アプリ開発者の一部に対して追加の作業が必要になります。

### <a name="sections"></a>セクション

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

このトピックでは、`Adapter`、`LayoutManager`、および `ViewHolder` を、`RecyclerView`をサポートするヘルパークラスとして連携させる方法について説明します。
これらの各ヘルパークラスの概要と、アプリでのそれらの使用方法について説明します。

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

このトピックでは、実際の写真閲覧アプリを構築するためにさまざまな `RecyclerView` 要素がどのように実装されるかについて、実際のコード例を示しながら、 [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)について説明します。

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

このトピックでは、[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)に示されているサンプルアプリにコードを追加して、アイテムクリックイベントを処理する方法と、基になるデータソースが変更された場合に `RecyclerView` を更新する方法を示します。

### <a name="summary"></a>まとめ

このガイドでは、Android `RecyclerView` ウィジェットを導入しました。この例では、`RecyclerView` サポートライブラリを Xamarin Android プロジェクトに追加する方法、ビューを再利用 `RecyclerView` 方法、効率を高めるためにビューホルダーパターンを適用する方法、および `RecyclerView` を構成するさまざまなヘルパークラスを使用してコレクションを表示する方法について説明しました。 `RecyclerView` をアプリケーションに統合する方法を示すコード例を提供し、さまざまなレイアウトマネージャーにプラグインして `RecyclerView`のレイアウトポリシーを調整する方法について説明し、項目クリックイベントを処理してデータの `RecyclerView` に通知する方法について説明しました。ソースの変更。

`RecyclerView`の詳細については、「 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)」を参照してください。

## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
