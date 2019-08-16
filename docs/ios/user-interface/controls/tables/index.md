---
title: Xamarin でのテーブルとセルの操作
description: このドキュメントでは、Xamarin iOS アプリで UITableView コントロールを使用してデータを表示する方法について説明しているさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/06/2016
ms.openlocfilehash: 0dd3830b4903658d0b014eceaad9c57859963c04
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528633"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Xamarin でのテーブルとセルの操作

このセクションでは、テーブルの作成と表示に使用されるクラスについて説明し、Xamarin での使用方法の例を示します。 テーブルの既定の外観を使用する方法、レイアウトをカスタマイズする方法、編集を実装する方法、Xamarin iOS Designer を使用してテーブルを視覚的にデザインする方法について説明します。 場合によっては、表示が明らかに行のリスト (ミュージックアプリなど) である場合や、テーブルコントロールを認識することが困難な場合があります (連絡先アプリでの編集、メッセージアプリのメッセージ交換など)。

Xamarin を使用したクロスプラットフォームアプリケーションで作業している場合、UITableView コントロールは Android の ListView クラスに似ています (また、UITableViewSource クラスは Android のアダプタークラスに似ています)。

これらの記事では、次のようなテーブルの使用について、包括的に説明します。

- **テーブルパーツ**– `UITableView`コントロールのビジュアル要素の概要と説明。 
- テーブル**でのデータの表示**–テーブルを作成および設定する方法を示すと共に、さまざまなテーブルとセルスタイルを使用して、セルオブジェクトを再利用することにより、メモリの問題を回避します。 
- **高度な使用法**–カスタムセルを作成し、UITableView クラスの編集機能を使用します。 
- **テーブルを視覚的に作成**する-Xamarin Designer for iOS を使用して、ストーリーボードを含むテーブルドリブンインターフェイスを作成します。 

## <a name="contents"></a>目次

 [テーブルパーツ&amp;の機能](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [テーブルへのデータの読み込み](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [テーブルの外観のカスタマイズ](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [編集](~/ios/user-interface/controls/tables/editing.md)
 
 [行の操作](~/ios/user-interface/controls/tables/row-action.md)

 [ストーリーボードでのテーブルの作成](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [行の高さの自動サイズ変更](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
- [ストーリーボードのテーブル (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [TableView レシピのストーリーボード](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Monotouch.dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [Github の TableEditing サンプル](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github の TableParts サンプル](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github の TableAndCellStyles サンプル](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView クラスリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell クラスのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
