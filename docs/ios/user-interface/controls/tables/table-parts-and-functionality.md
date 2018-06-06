---
title: テーブルの部品および Xamarin.iOS での機能
description: このドキュメントでは、iOS で UITableView のさまざまな機能について説明します。 これは、セクションのヘッダー、セル、ページ フッターのセクションで、インデックス、および編集モードについて説明します。
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c7c9f810798c3d02078b48e17a2ab951cb36aa12
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789877"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>テーブルの部品および Xamarin.iOS での機能

UITableView は、'グループ化' または '標準' のスタイルを持つことができ、次の部分で構成されています。

-  [セクション ヘッダー](#Section_Header)
-  [セル](#Cells)(またはしたい場合は、行)
-  [セクション ページ フッター](#Section_Footer)
-  [インデックス](#Index)
-  [編集モード](#Edit_Features)('を削除するスワイプ' が含まれていますハンドルをドラッグして行の順序を変更すると)。 

これらのスクリーン ショットは、セクションの行、ヘッダー、フッター、エディット コントロールおよびインデックスの表示方法を示しています。

 [![](table-parts-and-functionality-images/image1a.png "これらのスクリーン ショットは、セクションの行、ヘッダー、フッター、エディット コントロールおよびインデックスの表示方法を表示します。")](table-parts-and-functionality-images/image1a.png#lightbox)

これらの部分で詳しく説明します。

<a name="Section_Header" />

## <a name="section-header"></a>セクション ヘッダー

セルことができます必要に応じてするセクションにグループ化、ラベルが付いた、カスタム ヘッダーおよびフッター ラベルが付いたできます。 文字列値を持つヘッダーを設定することができますか、別のレイアウトやスタイルを許可するカスタム ビューを指定することができます。

<a name="Cells" />

## <a name="cells"></a>セル

セルは、テーブルの主なユーザー インターフェイス要素です。 正しく実装されると、セルはメモリ効率を上げるのため再利用します。 4 つの組み込みのセル スタイルがあるし、ストーリー ボードを使用する場合は、独自のカスタム セル – コードでは、または、デザイナーでを作成することができます。

<a name="Section_Footer"/>

## <a name="section-footer"></a>セクション ページ フッター

文字列値を持つ、省略可能なセクション フッターを設定することができますか、別のレイアウトやスタイルを許可するカスタム ビューを指定することができます。 セクションのヘッダーとフッターが個別に設定していないことができます。

<a name="Index" />

## <a name="index"></a>インデックス

インデックスは、下表の右端の文字のストリップとして表示されます。
接触している、インデックスにドラッグするか、テーブルの部分をスクロールを促進します。 インデックスは省略可能ですが、長い一覧を参照する際に勧めします。 通常、インデックスは、グループ化されたスタイルは使用されません。

<a name="Edit_Features" />

## <a name="editing-mode"></a>編集モード

いくつかの別の編集機能を使用できます。

- 個々 のセルを削除する方向にスワイプします。
- 各行の削除 ボタンを表示するために編集モードに入る 
- 再オーダー ハンドルを表示するために編集モードに入ります。 
- (アニメーション) を含む新しいセルを挿入しています。

このドキュメントの残りの部分では、Xamarin.iOS とこれらすべての UITableView 機能を実装する方法を示します。


## <a name="classes-overview"></a>クラスの概要

テーブルのビューを表示するために使用する主なクラスを次に示します。

[![](table-parts-and-functionality-images/classdiagram.png "テーブルのビューを表示するために使用する主なクラスを挙げています")](table-parts-and-functionality-images/classdiagram.png#lightbox)

各クラスの目的は、次のとおりです。

- **UITableView** – スクロール コンテナー内のセルのコレクションを含むビュー。 テーブル ビューが画面全体を iPhone アプリで通常使用しますが、可能性があります iPad でより大きなビューの一部として存在している (または、重なってに表示されます)。 
- **UITableViewCell** – テーブル ビューで、1 つのセル (または行) を示すビューが表示されます。 4 つの組み込みのセルの種類があり、(C#) または iOS デザイナーの両方にカスタムのセルを作成することはできます。 
- **UITableViewSource** – Xamarin.iOS 排他テーブル、行の数を含む、行ごとのセルの表示を返す、行の選択とその他の多くの省略可能な機能の処理を表示するために必要なすべてのメソッドを提供する抽象クラスです。 *必要があります*サブクラス UITableView を機能させるためにします。 
- **NSIndexPath** – テーブル内のセルの位置を一意に識別する Contains 行およびセクションのプロパティです。 
- **UITableViewController** – すぐに使用できる UIViewController とを持つ UITableView ハードコードされたそのビューとテーブルのプロパティを使用してアクセスできます。 
- **UIViewController** – 場合は、テーブルにそのフレームを使用して、UIViewController UITableView を適切に設定を追加する画面全体を占有しません。 

UITableViewSource には、次の 2 つクラス、Xamarin.iOS では使用できますが、通常必要はありませんが置き換えられます。

- **UITableViewDataSource** – は抽象クラスとして Xamarin.iOS でモデル化されて、OBJECTIVE-C プロトコルです。 各セルだけでなくヘッダー、フッター、および行と、テーブル内のセクションの数に関する情報をビューにテーブルを提供するサブクラス化する必要があります。 
- **UITableViewDelegate** – クラスとして Xamarin.iOS でモデル化されて、OBJECTIVE-C プロトコルです。 選択、編集機能とその他の省略可能なテーブル機能を処理します。 

このドキュメントでの例は、すべて UITableViewSource してこれら 2 つのクラスを無視します。 Objective C 例については、Apple のドキュメントがどのように (および Xamarin.iOS の UITableViewSource を代わりに使用できます) を理解すると便利なよう参照にはためには、ここで説明します。

## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
