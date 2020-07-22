---
title: Xamarin. iOS のテーブルパーツと機能
description: このドキュメントでは、iOS の UITableView のさまざまな部分について説明します。 ここでは、セクションヘッダー、セル、セクションフッター、インデックス、および編集モードについて説明します。
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: bf023543d3159f5d5baf7f7036a576b8a746cf9e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572131"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>Xamarin. iOS のテーブルパーツと機能

UITableView は、"グループ化" または "plain" スタイルを持つことができ、次の部分で構成されます。

- [セクションヘッダー](#Section_Header)
- [セル](#Cells)(または必要に応じて行)
- [セクションフッター](#Section_Footer)
- [化](#Index)
- [編集モード](#Edit_Features)(行の順序を変更するには ' スワイプする ' とドラッグハンドルを含む) 

これらのスクリーンショットは、セクション行、ヘッダー、フッター、編集コントロール、およびインデックスがどのように表示されるかを示しています。

 [![](table-parts-and-functionality-images/image1a.png "These screenshots show how section rows, headers, footers, edit controls and the index are displayed")](table-parts-and-functionality-images/image1a.png#lightbox)

これらの部分については、以下で詳しく説明します。

<a name="Section_Header"></a>

## <a name="section-header"></a>セクションヘッダー

必要に応じて、セルをセクションにグループ化したり、カスタムヘッダーでラベル付けしたり、フッター付きでラベル付けしたりすることができます。 ヘッダーには文字列値を設定できます。または、別のレイアウトまたはスタイルを使用できるようにカスタムビューを指定することもできます。

<a name="Cells"></a>

## <a name="cells"></a>セル

セルは、テーブルのメインのユーザーインターフェイス要素です。 正しく実装されている場合、セルはメモリ効率のために再利用されます。 組み込みのセルスタイルは4つあります。また、コード内で、またはストーリーボードを使用する場合はデザイナーで独自のカスタムセルを作成することもできます。

<a name="Section_Footer"></a>

## <a name="section-footer"></a>セクションフッター

省略可能なセクションフッターは、文字列値を使用して設定できます。または、別のレイアウトまたはスタイルを使用できるようにカスタムビューを指定することもできます。 セクションのヘッダーとフッターは個別に設定できます。

<a name="Index"></a>

## <a name="index"></a>インデックス

インデックスは、テーブルの右端にある文字のストリップとして表示されます。
インデックス上でタッチまたはドラッグすると、テーブルのその部分へのスクロールが高速化されます。 インデックスは省略可能ですが、長いリストを移動するために推奨されます。 通常、インデックスはグループ化されたスタイルでは使用されません。

<a name="Edit_Features"></a>

## <a name="editing-mode"></a>編集モード

いくつかの異なる編集機能を使用できます。

- スワイプして個々のセルを削除します。
- 編集モードに入り、各行の削除ボタンを表示します 
- 再順序ハンドルを表示するための編集モードに入ります。 
- 新しいセルの挿入 (アニメーションあり)。

このドキュメントの残りの部分では、これらすべての UITableView 機能を Xamarin. iOS で実装する方法について説明します。

## <a name="classes-overview"></a>クラスの概要

テーブルビューの表示に使用される主なクラスを次に示します。

[![](table-parts-and-functionality-images/classdiagram.png "The primary classes used to display table views are shown here")](table-parts-and-functionality-images/classdiagram.png#lightbox)

各クラスの目的は次のとおりです。

- **Uitableview** –スクロールコンテナー内のセルのコレクションを含むビューです。 テーブルビューでは、通常、iPhone アプリで画面全体が使用されますが、iPad 上のより大きなビューの一部として存在する (または segue に表示される) ことがあります。 
- **Uitableviewcell** –テーブルビュー内の1つのセル (または行) を表すビューです。 4つの組み込みのセル型があり、C# または iOS Designer の両方でカスタムセルを作成することができます。 
- **Uitableviewsource** –テーブルを表示するために必要なすべてのメソッド (行数、各行のセルビューの返却、行の選択の処理、およびその他のオプション機能など) を提供する、非排他的抽象クラス。 UITableView を機能させるには、これをサブクラス化する*必要があり*ます。 
- **Nsindexpath** –テーブル内のセルの位置を一意に識別する行とセクションのプロパティが含まれています。 
- **Uitableviewcontroller** –すぐに使用できる uiviewcontroller。 uitableview がビューとしてハードコーディングされ、TableView プロパティを介してアクセスできます。 
- **Uiviewcontroller** –テーブルが画面全体を占有していない場合は、フレームセットが適切に設定された UIViewController に UITableView を追加できます。 

UITableViewSource は、次の2つのクラスを置き換えます。これらのクラスは引き続き Xamarin で使用できますが、通常は必要ありません。

- **Uitableviewdatasource** – Xamarin. iOS で抽象クラスとしてモデル化されている、目標 C プロトコルです。 各セルのビューを含むテーブルと、テーブル内のヘッダー、フッター、および行とセクションの数に関する情報を提供するには、サブクラス化する必要があります。 
- **Uitableviewdelegate** –クラスとして Xamarin. iOS でモデル化された、目標 C プロトコルです。 選択、機能の編集、およびその他のオプションのテーブル機能を処理します。 

このドキュメントでは、すべての例で UITableViewSource を使用し、これら2つのクラスを無視します。 ここでは、Apple のドキュメントに記載されているすべての目的 C の例でそれらを参照するため、ここで説明します。そのため、実行内容を理解しておくと便利です (また、Xamarin の UITableViewSource を使用することもできます)。

## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
