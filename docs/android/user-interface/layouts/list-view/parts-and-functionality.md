---
title: ListView のパーツと機能
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 248baa5daceff6db01098a155600ea204547e845
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61319745"
---
# <a name="listview-parts-and-functionality"></a>ListView のパーツと機能


## <a name="overview"></a>概要

A`ListView`次の部分で構成されています。

- **行**&ndash;リスト内のデータの表示形式。

- **アダプター** &ndash;リスト ビューにデータ ソースをバインドする非ビジュアル クラス。

- **高速スクロール**&ndash;ユーザーがリストの長さをスクロールできるハンドル。

- **インデックスをセクション**&ndash;スクロールから浮遊したユーザー インターフェイス要素がリストに現在の行の場所を示す行。

これらのスクリーン ショットを使用して、基本的な`ListView`の高速スクロールとセクションのインデックスがどのように表示されるかを表示するコントロール。

[![スクロール、およびインデックスの高速、プレーンな古い行を使用してアプリのスクリーン ショット](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

構成する要素を`ListView`は以下で詳しく説明します。


## <a name="rows"></a>行

行ごとに独自`View`します。 ビューに定義された組み込みビューのいずれかを指定できます`Android.Resources`、またはカスタムのビュー。 これらが異なる場合がまたは各行は、同じビューのレイアウトを使用できます。 組み込みのレイアウトとカスタム レイアウトを定義する方法を説明する他のユーザーを使用するには、このドキュメントでは、例です。


## <a name="adapter"></a>アダプター

`ListView`コントロールに必要な`Adapter`、書式設定されたを指定する`View`行ごとにします。 Android デバイスには組み込みのアダプターと使用できますが、ビューまたはカスタム クラスを作成できます。


## <a name="fast-scrolling"></a>高速スクロール

ときに、`ListView`多くの行を含むデータの高速スクロールできるようにユーザーが、一覧の任意の部分に移動します。 必要に応じて有効になっている (と API レベル 11 でカスタマイズされた以降)、高速スクロール 'スクロール バー' に指定できます。


## <a name="section-index"></a>セクションのインデックス

長いリストをスクロール中に省略可能なセクションのインデックスはリストのどのような一部表示されている現在のユーザー フィードバックを提供します。 高速スクロールと組み合わせてで通常の長い一覧で適切なだけです。


## <a name="classes-overview"></a>クラスの概要

表示に使用される主要クラス`ListViews`を次に示します。

[![ListView と関連付けられているクラス間のリレーションシップを示す UML 図](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

各クラスの目的は、次に示します。

- **ListView** &ndash;行のスクロール可能なコレクションを表示するユーザー インターフェイス要素です。 電話に通常は画面全体を使用して (この場合は、`ListActivity`クラスを使用できます) かでスマート フォンやタブレット デバイスの大規模なレイアウトの一部である可能性があります。

- **ビュー** &ndash; Android でのビューは、任意のユーザー インターフェイス要素を指定できますがのコンテキストで、`ListView`必要があります、`View`行ごとに指定します。

- **BaseAdapter** &ndash;バインドを実装するアダプターの基本クラスを`ListView`データ ソースにします。

- **ArrayAdapter** &ndash;に文字列の配列をバインドする組み込みのアダプター クラスを`ListView`表示にします。 ジェネリック`ArrayAdapter<T>`他の種類は同じです。

- **CursorAdapter** &ndash;使用`CursorAdapter`または`SimpleCursorAdapter`SQLite クエリに基づいてデータを表示します。

このドキュメントには使用する単純な例が含まれています、`ArrayAdapter`のカスタム実装を必要とするより複雑な例と`BaseAdapter`または`CursorAdapter`します。

