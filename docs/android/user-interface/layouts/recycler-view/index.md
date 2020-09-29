---
title: RecyclerView
description: RecyclerView は、コレクションを表示するためのビューグループです。ListView や GridView など、以前のビューグループをより柔軟に置き換えることができるように設計されています。  このガイドでは、Xamarin Android アプリケーションで RecyclerView を使用およびカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/03/2018
ms.openlocfilehash: a972f6686103dc9b3a1317d15985cbf3dc78bd82
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457064"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView は、コレクションを表示するためのビューグループです。ListView や GridView など、以前のビューグループをより柔軟に置き換えることができるように設計されています。 このガイドでは、Xamarin Android アプリケーションで RecyclerView を使用およびカスタマイズする方法について説明します。_

## <a name="recyclerview"></a>RecyclerView

多くのアプリでは、同じ種類のコレクション (メッセージ、連絡先、画像、曲など) を表示する必要があります。多くの場合、このコレクションは大きすぎて画面に収まらないため、コレクション内のすべての項目をスムーズにスクロールできる小さなウィンドウにコレクションが表示されます。
`RecyclerView` は、リストまたはグリッド内の項目のコレクションを表示し、ユーザーがコレクションをスクロールできるようにする Android ウィジェットです。 次に示すのは、を使用して、 `RecyclerView` 電子メールの受信トレイの内容を垂直スクロールリストに表示するサンプルアプリのスクリーンショットです。

[![RecyclerView を使用して受信トレイメッセージを一覧表示するアプリの例](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` は、次の2つの魅力的な機能を提供します。

- これには柔軟なアーキテクチャがあり、優先するコンポーネントをプラグインすることによって動作を変更できます。

- 大きなコレクションでは、項目ビューを再利用し、ビューの参照をキャッシュするために *ビューホルダー* を使用する必要があるため、効率的です。

このガイドでは、Xamarin Android アプリケーションでを使用する方法について説明します。このガイドでは、 `RecyclerView` Xamarin android プロジェクトにパッケージを追加する方法について説明 `RecyclerView` し、 `RecyclerView` 一般的なアプリケーションの機能について説明します。 実際のコード例を使用すると `RecyclerView` 、アプリケーションに統合する方法、項目ビューのクリックを実装する方法、 `RecyclerView` 基になるデータが変更されたときに更新する方法を示すことができます。 このガイドでは、Xamarin Android の開発について理解していることを前提としています。

### <a name="requirements"></a>必要条件

`RecyclerView`は多くの場合 android 5.0 ロリポップに関連付けられていますが、サポートライブラリとして提供されて &ndash; `RecyclerView` おり、API レベル 7 (android 2.1) 以降を対象とするアプリで動作します。 Xamarin ベースのアプリケーションでを使用するには、次のものが必要です `RecyclerView` 。

- **Xamarin android** &ndash; 4.20 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。

- アプリプロジェクトには、 **v7** パッケージが含まれている必要があります。 NuGet パッケージのインストールの詳細については、「 [チュートリアル: プロジェクトに nuget を含める](/visualstudio/mac/nuget-walkthrough)」を参照してください。

### <a name="overview"></a>概要

`RecyclerView` は、 `ListView` Android のウィジェットおよびウィジェットの代わりとして考えることができ `GridView` ます。 前の例と同様に、 `RecyclerView` は小さいウィンドウに大きなデータセットを表示するように設計されていますが、 `RecyclerView` より多くのレイアウトオプションが用意されており、大規模なコレクションを表示するために最適化されています。 に慣れている場合は `ListView` 、との間にいくつかの重要な違いがあり `ListView` `RecyclerView` ます。

- `RecyclerView` は少し複雑になります。を使用するには、より多くのコードを記述する必要があり `RecyclerView` `ListView` ます。

- `RecyclerView` は、定義済みのアダプターを提供しません。データソースにアクセスするアダプターコードを実装する必要があります。 ただし、Android には、およびで動作する定義済みのアダプターがいくつか用意されてい `ListView` `GridView` ます。

- `RecyclerView` ユーザーが項目をタップしても、項目クリックイベントは提供されません。代わりに、項目クリックイベントはヘルパークラスによって処理されます。 これに対し、では、 `ListView` 項目クリックイベントが提供されます。

- `RecyclerView` では、ビューをリサイクルし、ビューホルダーパターンを適用することによってパフォーマンスを向上させます。これにより、不要なレイアウトリソースの参照がなくなります。 では、ビューホルダーパターンの使用は省略可能です `ListView` 。

- `RecyclerView` は、カスタマイズしやすくするモジュール形式の設計に基づいています。 たとえば、アプリにコードを大幅に変更することなく、別のレイアウトポリシーをプラグインできます。
    これに対し、の `ListView` 構造は比較的モノリシックです。

- `RecyclerView` には、項目の追加と削除のための組み込みアニメーションが含まれています。 `ListView` アニメーションでは、アプリ開発者の一部に対して追加の作業が必要になります。

### <a name="sections"></a>セクション

#### <a name="recyclerview-parts-and-functionality"></a>[RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

このトピックでは、、、およびが、を `Adapter` `LayoutManager` `ViewHolder` サポートするヘルパークラスとして連携する方法について説明し `RecyclerView` ます。
これらの各ヘルパークラスの概要と、アプリでのそれらの使用方法について説明します。

#### <a name="a-basic-recyclerview-example"></a>[基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

このトピックでは、実際の写真閲覧アプリを構築するためにさまざまな要素がどのように実装されるかについて実際のコード例を提供することで、 [RecyclerView のパーツと機能](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) について説明し `RecyclerView` ます。

#### <a name="extending-the-recyclerview-example"></a>[RecyclerView の例を拡張する](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

このトピックでは、 [基本的な RecyclerView の例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) に示されているサンプルアプリにコードを追加して、項目クリックイベントを処理し、 `RecyclerView` 基になるデータソースが変更されたときに更新する方法を示します。

### <a name="summary"></a>まとめ

このガイドでは、Android ウィジェットを紹介しました。このガイドでは、 `RecyclerView` `RecyclerView` Xamarin android プロジェクトにサポートライブラリを追加する方法 `RecyclerView` 、ビューを再利用する方法、効率を高めるためにビューホルダーパターンを適用する方法、および `RecyclerView` コレクションを表示するために共同作業を構成するさまざまなヘルパークラスについて説明しました。 ここでは、をアプリケーションに統合する方法を示すコード例を紹介 `RecyclerView` し、さまざまなレイアウトマネージャーにプラグインしてレイアウトポリシーを調整する方法について説明 `RecyclerView` し、項目クリックイベントを処理して `RecyclerView` データソースの変更を通知する方法について説明しました。

の詳細については `RecyclerView` 、 [RecyclerView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)を参照してください。

## <a name="related-links"></a>関連リンク

- [RecyclerViewer (サンプル)](/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)