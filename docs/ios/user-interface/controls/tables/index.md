---
title: テーブルおよびセルの使用
description: Xamarin.iOS と UITableView を使用してデータを表示します。
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: a1cda3632a75c7e462e763a34fdb5b586237b670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-tables-and-cells"></a>テーブルおよびセルの使用


このセクションでは、作成し、テーブルの表示に使用するクラスが導入されていますし、Xamarin.iOS で使用する方法の例について説明します。 既定の外観を使用して、テーブルの編集、および Xamarin iOS デザイナーを使用してテーブルを視覚的にデザインするを実装する、レイアウトをカスタマイズする場合に適用されます。 場合があります、表示、明らかに (、音楽アプリなど) の行と、テーブルなどのコントロール (連絡先アプリの場合、またはメッセージ アプリにあるメッセージ交換での編集) を認識するが困難である他の時間の一覧です。

Xamarin.Android を使用してクロスプラット フォーム アプリケーションで動作している UITableView コントロール android ListView クラスに似ていますが (と UITableViewSource クラスは Android のアダプター クラスに似ています)。

これらの記事は包括的なを見てを含むテーブルを使用します。

-   **テーブルの部品**– 概要およびのビジュアル要素を説明する、`UITableView`コントロール。 
-   **テーブルにデータを表示する**– を作成し、テーブルを作成する方法を示す、別のテーブルおよびセルのスタイルを使用して、セル オブジェクトをリサイクルしてメモリの問題を回避します。 
-   **使用状況を高度な**– のカスタムのセルのビルドと UITableView クラスの編集機能を使用します。 
-   **テーブルを視覚的に作成する**– iOS 用の Xamarin デザイナーを使用して、ストーリー ボードを使用、テーブルドリブン インターフェイスを作成します。 


## <a name="contents"></a>目次

 [テーブルの部品&amp;機能](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [テーブルへのデータの読み込み](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [テーブルの外観のカスタマイズ](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [編集](~/ios/user-interface/controls/tables/editing.md)
 
 [行の操作](~/ios/user-interface/controls/tables/row-action.md)

 [ストーリー ボードのテーブルの作成](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [行の高さの自動サイズ変更](~/ios/user-interface/controls/tables/autosizing-row-height.md)


## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [ストーリー ボード (サンプル) 内のテーブル](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [ストーリー ボードのレシピのテーブル](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [MonoTouch.Dialog の概要](~/ios/user-interface/monotouch.dialog/index.md)
- [Github に TableEditing サンプル](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github に TableParts サンプル](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github に TableAndCellStyles サンプル](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView クラスのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell クラスのリファレンス](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
