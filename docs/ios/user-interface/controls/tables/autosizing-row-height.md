---
title: Xamarin.iOS での自動サイズ変更行の高さ
description: このドキュメントでは、テーブル ビューの行の高さの変化に基づいてコンテンツを Xamarin.iOS アプリに追加する方法について説明します。 IOS Designer のセルのレイアウトと有効にすると自動サイズ変更の高さがについて説明します。
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e4446abc73817eb0672cd10a69ff6f738de0c1e1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116473"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Xamarin.iOS での自動サイズ変更行の高さ

Apple iOS 8 以降、テーブル ビューを作成する機能を追加 (`UITableView`) を自動的に拡大して自動レイアウトやサイズ クラスの制約を使用してそのコンテンツのサイズに基づいて、特定の行の高さを縮小します。

iOS 11 では、自動的に展開する行の機能を追加しました。 ヘッダー、フッター、およびセル今すぐ自動的にサイズ設定できるコンテンツに基づいています。 をで iOS Designer、Interface Builder では、テーブルが作成された場合、または行の高さを固定にする必要があります手動で有効にする self、セルのサイズ変更このガイドで説明します。

## <a name="cell-layout-in-the-ios-designer"></a>IOS Designer のセルのレイアウト

IOS Designer での行の自動サイズ変更用にするテーブル ビューのストーリー ボード選択セルの開いている*プロトタイプ*とセルのレイアウトを設計します。 例えば:

[![](autosizing-row-height-images/table01.png "セルのプロトタイプのデザイン")](autosizing-row-height-images/table01.png#lightbox)

プロトタイプの各要素に対して、回転または別の iOS デバイスの画面サイズのテーブル ビューのサイズが変更されると、正しい位置に要素を保持する制約を追加します。 たとえば、ピン留め、`Title`上、左のセルの右側と*コンテンツ ビュー*:

[![](autosizing-row-height-images/table02.png "タイトルを上、左および右のセルのコンテンツ ビューをピン留め")](autosizing-row-height-images/table02.png#lightbox)

この例のテーブル、小さい場合`Label`(下、 `Title`) フィールドを行の高さを増減する拡大および縮小できます。 この効果を実現するには、左、右、上、ラベルの下にピン留めする、次の制約を追加します。

[![](autosizing-row-height-images/table03.png "これらの制約、左、右、上、ラベルの下にピン留めするには")](autosizing-row-height-images/table03.png#lightbox)

できたので、セル内の要素を完全に拘束いますが、のどの要素を拡大するかを明確にする必要があります。 これを行うには、次のように設定します。、 **Hugging コンテンツの優先順位**と**コンテンツの圧縮の抵抗の優先順位**で必要に応じて、**レイアウト**Properties Pad のセクション。

[![](autosizing-row-height-images/table03a.png "Properties Pad のレイアウト セクション")](autosizing-row-height-images/table03a.png#lightbox)

拡張する要素の設定、**低い**Hugging の優先順位の値と**低い**圧縮抵抗の優先度の値。

次に、セルのプロトタイプを選択し、一意必要があります**識別子**:

[![](autosizing-row-height-images/table04.png "セルのプロトタイプの一意識別子を提供")](autosizing-row-height-images/table04.png#lightbox)

この例の場合`GrowCell`します。 いますが、テーブルを作成するときに後でこの値を使用します。

> [!IMPORTANT]
> テーブルには、1 つ以上のセルの種類が含まれている場合 (**プロトタイプ**)、各種類には、独自の一意なことを確認する必要がある`Identifier`自動させる行のサイズ変更します。

を、セルのプロトタイプの各要素に割り当てる、**名前**にこれを公開するC#コード。 例えば:

[![](autosizing-row-height-images/table05.png "公開するための名前を割り当てるC#コード")](autosizing-row-height-images/table05.png#lightbox)

カスタム クラスを次に、追加、 `UITableViewController`、`UITableView`と`UITableCell`(プロトタイプ)。 例えば: 

[![](autosizing-row-height-images/table06.png "UITableViewController、UITableView および、UITableCell カスタム クラスの追加")](autosizing-row-height-images/table06.png#lightbox)

最後に、必要なすべてのコンテンツは、ラベルに表示されることを確認する次のように設定します、**行**プロパティを`0`:。

[![](autosizing-row-height-images/table06.png "線のプロパティが 0 に設定")](autosizing-row-height-images/table06a.png#lightbox)

UI が定義されているを使用した自動行の高さのサイズ変更を有効にするコードを追加してみましょう。

## <a name="enabling-auto-resizing-height"></a>高さの自動サイズ変更を有効にします。

いずれかで、テーブル ビューのデータ ソース (`UITableViewDatasource`) またはソース (`UITableViewSource`) を使用する必要があります。 セルをデキューしましたときに、、 `Identifier` 、デザイナーで定義しました。 例えば:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

既定では、テーブル ビューは、行の高さの自動サイズ変更に設定されます。 これは、確実に、`RowHeight`にプロパティを設定する必要があります`UITableView.AutomaticDimension`します。 設定する必要があります、`EstimatedRowHeight`プロパティ、`UITableViewController`します。 例えば:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

この見積もりは、正確ではありません、テーブル ビューの各行の高さの平均値の大まかな推定だけです。

配置でこのコードでは、アプリを実行すると、各行の圧縮をセル プロトタイプで最後のラベルの高さに基づいて、拡張します。 例えば:

[![](autosizing-row-height-images/table07.png "実行のサンプル テーブル")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>関連リンク

- [GrowRowTable (サンプル)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
