---
title: テーブルのパーツと Xamarin.iOS の機能
description: このドキュメントでは、ios の UITableView のさまざまな部分について説明します。 セクションのヘッダー、セル、ページ フッターのセクションで、インデックス、および編集モードがについて説明します。
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c4d788cce12a9aabdd1170cd1a52915f3b30285f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113951"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>テーブルのパーツと Xamarin.iOS の機能

UITableView は、'グループ化された' または '標準' のスタイルを持つことができ、次の部分で構成されています。

-  [セクション ヘッダー](#Section_Header)
-  [セル](#Cells)(または行を使用する場合)
-  [セクションのフッター](#Section_Footer)
-  [Index](#Index)
-  [編集モード](#Edit_Features)('を削除するにはスワイプ' が含まれています行の順序を変更するハンドルをドラッグします)。 

これらのスクリーン ショットでは、セクションの行、ヘッダー、フッター、エディット コントロール、およびインデックスの表示方法を示します。

 [![](table-parts-and-functionality-images/image1a.png "これらのスクリーン ショットは、セクションの行、ヘッダー、フッター、エディット コントロール、およびインデックスの表示方法を示します")](table-parts-and-functionality-images/image1a.png#lightbox)

これらの部分は以下で詳しく説明します。

<a name="Section_Header" />

## <a name="section-header"></a>セクション ヘッダー

セルことができます必要に応じてするセクションにグループ化、カスタム ヘッダー、ラベルが付いたやフッターとラベル付けします。 文字列値を持つヘッダーを設定することができますかの別のレイアウトやスタイルを許可するカスタム ビューを指定することができます。

<a name="Cells" />

## <a name="cells"></a>セル

セルは、テーブルの主なユーザー インターフェイス要素です。 正しく実装された場合、セルはメモリ効率を高めるため再利用します。 4 つの組み込みのセル スタイルがあるし、ストーリー ボードを使用する場合は、独自のカスタム セル – コードまたはデザイナーを作成することができます。

<a name="Section_Footer"/>

## <a name="section-footer"></a>セクションのフッター

文字列値、省略可能なセクションのフッターを設定できますかの別のレイアウトやスタイルを許可するカスタム ビューを指定することができます。 セクション ヘッダーとフッターが個別に設定がないことができます。

<a name="Index" />

## <a name="index"></a>インデックス

インデックスは、下の表の右端の文字のストリップとして表示されます。
手を加えることや、インデックスにドラッグすることは、テーブルの部分をスクロールを迅速化します。 インデックスは長い一覧の移動を支援することは推奨オプションです。 通常、インデックスは、グループ化されたスタイルは使用されません。

<a name="Edit_Features" />

## <a name="editing-mode"></a>編集モード

使用可能なさまざまな編集機能のいくつかは。

- 個々 のセルを削除するスワイプします。
- 行ごとに delete ボタンを表示する編集モードに入る 
- ハンドルの再注文を表示する編集モードに入ります。 
- (アニメーション) を含む新しいセルを挿入します。

このドキュメントの残りの部分では、Xamarin.iOS でこれらすべての UITableView 機能を実装する方法を示します。


## <a name="classes-overview"></a>クラスの概要

テーブル ビューを表示するために使用する主なクラスを次に示します。

[![](table-parts-and-functionality-images/classdiagram.png "テーブル ビューの表示に使用される主要クラスが次に示します")](table-parts-and-functionality-images/classdiagram.png#lightbox)

各クラスの目的は、次に示します。

- **UITableView** – スクロール コンテナー内のセルのコレクションを含むビュー。 テーブル ビューが画面全体を iPhone アプリで通常使用しますが、可能性があると拡大表示、iPad 上の一部として存在する (または、ポップ オーバーに表示されます)。 
- **UITableViewCell** – テーブル ビューで 1 つのセル (または行) を表すビュー。 次の 4 つの組み込みのセルの種類がありますしでカスタム セル両方を作成することはC#または iOS デザイナー。 
- **UITableViewSource** – Xamarin.iOS 排他テーブル、行の数など、行ごとのセルのビューを返す、行の選択とその他の多くのオプション機能の処理を表示するために必要なすべてのメソッドを提供する抽象クラス。 *する必要があります*サブクラス UITableView 作業を取得します。 
- **NSIndexPath** – テーブル内のセルの位置を一意に識別する値を含む行とセクションのプロパティ。 
- **UITableViewController** – をそのビューとテーブルのプロパティを使用してアクセスできる UITableView ハードコーディングを持つすぐ使用 UIViewController します。 
- **UIViewController** – の表に、そのフレームを使用して任意の UIViewController の UITableView が適切に設定を追加する画面全体を占有しません。 

UITableViewSource には、Xamarin.iOS では使用できますが、通常必要のない次の 2 つクラスが置き換えられます。

- **UITableViewDataSource** : は抽象クラスとして Xamarin.iOS でモデル化されて、OBJECTIVE-C プロトコル。 各セルだけでなくヘッダー、フッター、および行と、テーブル内のセクションの数に関する情報をビューにテーブルを提供するサブクラス化する必要があります。 
- **UITableViewDelegate** – クラスとして Xamarin.iOS でモデル化されて、OBJECTIVE-C プロトコル。 選択した場合、編集機能とその他の省略可能なテーブル機能を処理します。 

このドキュメントでは、例は UITableViewSource を使用して、すべてと、これら 2 つのクラスを無視します。 Objective C 例の Apple のドキュメントを参照できるようになりますがどのように (および Xamarin.iOS の UITableViewSource を代わりに使用できます) を把握するのに役立ちます参照にはためには、ここで説明します。

## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
