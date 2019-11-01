---
title: ListView のパーツと機能
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2017
ms.openlocfilehash: b8fd44a70f4c7ecdcf7919dec1c81461200b35bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028862"
---
# <a name="xamarinandroid-listview-parts-and-functionality"></a>Xamarin. Android ListView のパーツと機能

`ListView` は、次の部分で構成されています。

- **行**は、リスト内のデータの表示形式 &ndash; ます。

- **アダプター** &ndash;、データソースをリストビューにバインドする非ビジュアルクラスです。

- **高速スクロール**&ndash;、ユーザーがリストの長さをスクロールできるハンドルです。

- **セクションインデックス**&ndash;、現在の行があるリスト内の場所を示すために、スクロールする行をフローティングするユーザーインターフェイス要素です。

これらのスクリーンショットでは、基本的な `ListView` コントロールを使用して、高速スクロールとセクションインデックスがどのようにレンダリングされるかを示しています。

[単純な古い行、高速スクロール、およびセクションインデックスを使用してアプリのスクリーンショットを![](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

`ListView` を構成する要素の詳細については、次を参照してください。

## <a name="rows"></a>行

各行には、独自の `View`があります。 ビューには、`Android.Resources`で定義されている組み込みビュー、またはカスタムビューのいずれかを指定できます。 各行で同じビューレイアウトを使用することも、すべて異なる行を使用することもできます。 このドキュメントには、組み込みのレイアウトを使用する例があります。また、カスタムレイアウトの定義方法について説明しています。

## <a name="adapter"></a>アダプター

`ListView` コントロールには、各行に対して書式設定された `View` を提供するための `Adapter` が必要です。 Android には、使用できる組み込みのアダプターとビュー、またはカスタムクラスを作成することができます。

## <a name="fast-scrolling"></a>高速スクロール

`ListView` に多数のデータ行が含まれている場合、高速スクロールを有効にして、ユーザーがリストの任意の部分に移動できるようにすることができます。 高速スクロール ' スクロールバー ' は、必要に応じて有効にすることができます (API レベル11以降でカスタマイズ)。

## <a name="section-index"></a>セクションのインデックス

長い一覧をスクロールしているときに、オプションのセクションインデックスを使用すると、現在表示しているリストのどの部分についてのフィードバックもユーザーに提供されます。 これは、長いリストにのみ適しており、通常は高速スクロールと組み合わせて使用します。

## <a name="classes-overview"></a>クラスの概要

`ListViews` の表示に使用される主なクラスを次に示します。

[ListView と関連クラス間のリレーションシップを示す UML 図の![](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

各クラスの目的は次のとおりです。

- **ListView** &ndash; 行のスクロール可能なコレクションを表示するユーザーインターフェイス要素です。 スマートフォンでは、通常、画面全体が使用されます (この場合は、`ListActivity` クラスを使用できます)。または、電話やタブレットデバイスの大規模なレイアウトの一部にすることもできます。

- **表示**&ndash; Android では、任意のユーザーインターフェイス要素を使用できますが、`ListView` のコンテキストでは、各行に `View` を指定する必要があります。

- **Baseadapter** &ndash; アダプター実装の基本クラスで、`ListView` をデータソースにバインドします。

- **Arrayadapter** &ndash; 文字列の配列を表示用の `ListView` にバインドする組み込みのアダプタークラスです。 ジェネリック `ArrayAdapter<T>` は、他の型に対しても同じように動作します。

- **カーソル**&ndash; `CursorAdapter` または `SimpleCursorAdapter` を使用して、SQLite クエリに基づいてデータを表示します。

このドキュメントには、`ArrayAdapter` を使用する簡単な例と、`BaseAdapter` または `CursorAdapter`のカスタム実装を必要とする複雑な例が含まれています。
