---
title: ListView のパーツと機能
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 4566ee5d203b5d098133aebe2c32dbaec712e17a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764211"
---
# <a name="xamarinandroid-listview-parts-and-functionality"></a>Xamarin. Android ListView のパーツと機能

は`ListView` 、次の部分で構成されています。

- **行**&ndash;リスト内のデータの可視表現。

- **アダプター**&ndash;データソースをリストビューにバインドする非ビジュアルクラス。

- **高速スクロール**&ndash;ユーザーがリストの長さをスクロールできるようにするハンドル。

- **セクションのインデックス**&ndash;現在の行があるリスト内の場所を示すために、スクロールする行をフローティングするユーザーインターフェイス要素。

これらのスクリーンショットで`ListView`は、基本コントロールを使用して、高速スクロールとセクションインデックスがどのようにレンダリングされるかを示しています。

[![Plain old rows、fast スクロール、section index を使用するアプリのスクリーンショット](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

を構成する要素の詳細`ListView`については、以下を参照してください。

## <a name="rows"></a>行

各行には独自`View`のがあります。 ビューには、で`Android.Resources`定義されている組み込みビュー、またはカスタムビューのいずれかを指定できます。 各行で同じビューレイアウトを使用することも、すべて異なる行を使用することもできます。 このドキュメントには、組み込みのレイアウトを使用する例があります。また、カスタムレイアウトの定義方法について説明しています。

## <a name="adapter"></a>アダプター

コントロール`ListView`では、 `Adapter`行ごとに書式`View`設定されたを指定するためにが必要です。 Android には、使用できる組み込みのアダプターとビュー、またはカスタムクラスを作成することができます。

## <a name="fast-scrolling"></a>高速スクロール

に多数`ListView`のデータ行が含まれている場合は、ユーザーが一覧の任意の部分に移動できるように、高速スクロールを有効にすることができます。 高速スクロール ' スクロールバー ' は、必要に応じて有効にすることができます (API レベル11以降でカスタマイズ)。

## <a name="section-index"></a>セクションのインデックス

長い一覧をスクロールしているときに、オプションのセクションインデックスを使用すると、現在表示しているリストのどの部分についてのフィードバックもユーザーに提供されます。 これは、長いリストにのみ適しており、通常は高速スクロールと組み合わせて使用します。

## <a name="classes-overview"></a>クラスの概要

表示`ListViews`に使用されるプライマリクラスを次に示します。

[![ListView と関連クラスの関係を示す UML 図](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

各クラスの目的は次のとおりです。

- **ListView**&ndash;スクロール可能な行のコレクションを表示するユーザーインターフェイス要素。 スマートフォンでは、通常、画面全体を使用します (この`ListActivity`場合はクラスを使用できます)。また、携帯電話やタブレットデバイスの大規模なレイアウトの一部にすることもできます。

- **ビュー**Android のビューは任意のユーザーインターフェイス要素にすることができますが、 `ListView`のコンテキストで`View`は、各行にを指定する必要があります。 &ndash;

- **Baseadapter**&ndash; を`ListView`データソースにバインドするためのアダプター実装の基本クラス。

- **Arrayadapter**&ndash;表示`ListView`のために文字列の配列をにバインドする組み込みのアダプタークラス。 ジェネリック`ArrayAdapter<T>`は、他の型に対しても同じように動作します。

- **カーソルアダプター**またはを使用`CursorAdapter`して、SQLite クエリに基づいてデータを表示します。 `SimpleCursorAdapter` &ndash;

このドキュメントには、を使用する`ArrayAdapter`簡単な例と、または`CursorAdapter`の`BaseAdapter`カスタム実装を必要とする複雑な例が含まれています。
