---
title: "ListView の部分と機能"
ms.topic: article
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 4a7947c40d80c0ff8cb35dab54a11907280335d9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="listview-parts-and-functionality"></a>ListView の部分と機能


## <a name="overview"></a>概要

A`ListView`は次の部分で構成されています。

- **行**&ndash;リスト内のデータの表示形式をします。

- **アダプター** &ndash;リスト ビューにデータ ソースにバインドするクラスを非表示します。

- **高速スクロール**&ndash;ユーザーがリストの長さをスクロールできるハンドル。

- **インデックスをセクション**&ndash;一覧で、現在の行がある場所を示すために行をユーザー インターフェイス要素でスクロールです。

これらのスクリーン ショットは、基本的なを使用して`ListView`を高速スクロールとセクション インデックスがどのように表示されるかを表示するコントロール。

[![スクロール、およびインデックスに高速プレーンな古い行を使用するアプリのスクリーン ショット](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

構成する要素、`ListView`で詳しく説明します。


## <a name="rows"></a>行

行のそれぞれが独自`View`です。 ビューで定義されている組み込みのビューのいずれかを指定できます`Android.Resources`、またはカスタム ビューです。 これらが異なる場合がや各行は、同じビューのレイアウトを使用できます。 組み込みのレイアウトとカスタム レイアウトを定義する方法を説明する他のユーザーを使用するには、このドキュメントでは、例です。


## <a name="adapter"></a>アダプター

`ListView`コントロールに必要な`Adapter`、書式設定を指定する`View`行ごとにします。 Android には、組み込みのアダプターと、使用できるビューまたはカスタム クラスを作成することができます。


## <a name="fast-scrolling"></a>高速スクロール

ときに、`ListView`多数の行を含むデータの fast スクロールできるように、一覧の任意の部分に移動するユーザーを支援します。 Fast-スクロール 'スクロール バー' オプションで有効になっている (と API レベル 11 でカスタマイズ以上できます)。


## <a name="section-index"></a>セクションのインデックス

長い一覧をスクロール中には、省略可能なセクション インデックスは、上のどの部分リストの現在表示しているユーザーのフィードバックを提供します。 高速のスクロールと組み合わせて通常の長い一覧の適切なはのみです。


## <a name="classes-overview"></a>クラスの概要

表示に使用されるプライマリ クラス`ListViews`を挙げています。

[![ListView コントロールと関連付けられているクラス間のリレーションシップを示す UML 図](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

各クラスの目的は、次のとおりです。

- **ListView** &ndash;行のスクロール可能なコレクションを表示するユーザー インターフェイス要素です。 電話に通常画面全体を使用して (この場合は、`ListActivity`クラスを使用することができます) か電話やタブレット デバイス上の大規模なレイアウトの一部である可能性があります。

- **ビュー** &ndash; Android でのビューは、任意のユーザー インターフェイス要素を指定できますのコンテキストでは、`ListView`必要があります、`View`行ごとに渡すことができます。

- **BaseAdapter** &ndash;バインドを実装するアダプターの基本クラス、`ListView`データ ソースにします。

- **ArrayAdapter** &ndash;に文字列の配列をバインドする組み込みのアダプター クラス、`ListView`表示用です。 ジェネリック`ArrayAdapter<T>`他の種類は同じです。

- **CursorAdapter** &ndash;使用`CursorAdapter`または`SimpleCursorAdapter`SQLite クエリに基づいてデータを表示します。

このドキュメントには使用する単純な例が含まれています、`ArrayAdapter`複雑な例についてのカスタム実装を必要とするだけでなく`BaseAdapter`または`CursorAdapter`です。

