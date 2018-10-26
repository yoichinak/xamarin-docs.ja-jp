---
title: テーブルと Xamarin.iOS でのセルの使用
description: このドキュメントは、Xamarin.iOS アプリに UITableView コントロールでデータを表示する方法を説明するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/06/2016
ms.openlocfilehash: 275c7553e465da67ca0780ea6aa9e986ca33b1f8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121608"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>テーブルと Xamarin.iOS でのセルの使用

このセクションでは、作成し、テーブルの表示に使用されるクラスが導入されていますし、Xamarin.iOS で使用する方法の例を紹介します。 既定の外観を使用して、テーブルの場合、編集、および Xamarin iOS Designer を使用してテーブルを視覚的に設計を実装するレイアウトをカスタマイズするには説明します。 場合があります、表示、明らかに (ミュージック アプリ) などの行とそれ以外の時間 (連絡先アプリの場合、またはメッセージ交換でメッセージ アプリでの編集) などのテーブル コントロールを認識することは困難の一覧です。

Xamarin.Android でのクロス プラットフォーム アプリケーションに取り組んでいる UITableView コントロールは Android では、ListView クラスに似ています (および UITableViewSource クラスは Android のアダプター クラスに似ています)。

これらの記事では、包括的に確認を含め、テーブルの使用方法を実行します。

-   **部品表**– 概要およびのビジュアル要素を説明する、`UITableView`コントロール。 
-   **テーブルにデータを表示する**– を作成し、テーブルを作成する方法を示す、別のテーブルおよびセルのスタイルを使用し、cell オブジェクトをリサイクルしてメモリの問題を回避します。 
-   **使用状況を高度な**: カスタムのセルを構築し、UITableView クラスの編集機能を使用します。 
-   **テーブルの視覚的に作成**– iOS 用の Xamarin デザイナーを使用して、ストーリー ボードをテーブルに基づくインターフェイスを作成します。 

## <a name="contents"></a>目次

 [部品表&amp;機能](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [テーブルへのデータの読み込み](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [テーブルの外観のカスタマイズ](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [編集](~/ios/user-interface/controls/tables/editing.md)
 
 [行アクション](~/ios/user-interface/controls/tables/row-action.md)

 [ストーリー ボードのテーブルの作成](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [行の高さの自動サイズ変更](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [ストーリー ボード (サンプル) 内のテーブル](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [テーブルのレシピをストーリー ボード](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [MonoTouch.Dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [Github 上の TableEditing サンプル](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github 上の TableParts サンプル](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github 上の TableAndCellStyles サンプル](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView クラスのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell クラスのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
