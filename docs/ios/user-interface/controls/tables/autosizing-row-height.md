---
title: Xamarin.iOS の自動サイズ変更行の高さ
description: このドキュメントでは、テーブル ビューの行の高さの変化に基づいてコンテンツ Xamarin.iOS アプリに追加する方法について説明します。 これには、iOS デザイナー内のセルのレイアウトと有効にすると自動サイズ変更の高さがについて説明します。
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 3c6beb112947f5423de200fd5c8957ef28dd48f9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789968"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Xamarin.iOS の自動サイズ変更行の高さ

Apple iOS 8 以降、テーブル ビューを作成する機能を追加 (`UITableView`) を自動的に拡大して自動レイアウト、サイズのクラスおよび制約を使用して、コンテンツのサイズに基づいて、特定の行の高さを縮小します。

iOS 11 では、自動的に展開する行の機能を追加しました。 ヘッダー、フッター、およびセル今すぐに自動的にサイズ設定できる内容に基づきます。 ただし、iOS デザイナー、インターフェイスのビルダーで、テーブルが作成された場合、または行の高さを固定にする必要があります手動で有効にする、セルのサイズ変更 self このガイドの説明に従って。

## <a name="cell-layout-in-the-ios-designer"></a>IOS デザイナー内のセルのレイアウト

開いている行の自動サイズ変更用 iOS デザイナーでテーブル ビューのストーリー ボードのセルを選択の*プロトタイプ*セルのレイアウトをデザインします。 例えば:

[![](autosizing-row-height-images/table01.png "セルのプロトタイプのデザイン")](autosizing-row-height-images/table01.png#lightbox)

プロトタイプに各要素に対して回転または別の iOS デバイスの画面サイズのテーブル ビューのサイズが変更されると、正しい位置に要素を保持する制約を追加します。 たとえば、ピン留め、`Title`セルの右と左上に*コンテンツ ビュー*:

[![](autosizing-row-height-images/table02.png "Top、left、セルのコンテンツ ビューの右側にタイトルをピン留め")](autosizing-row-height-images/table02.png#lightbox)

この例のテーブルを小さな場合`Label`(下にある、 `Title`) は、フィールドを行の高さを増減させて拡大および縮小できます。 この効果を実現するには、左、右、上部と下部のラベルをピン留めする次の制約を追加します。

[![](autosizing-row-height-images/table03.png "これらの制約を左、右、上部と下部のラベルをピン留めするには")](autosizing-row-height-images/table03.png#lightbox)

なったので、セル内の要素を完全に拘束おのどの要素を拡大するかを明確にする必要があります。 これを行うには、次のように設定します。、**コンテンツ Hugging の優先度**と**コンテンツ圧縮の耐性を確保する優先順位**で必要に応じて、**レイアウト**パッドのプロパティのセクション。

[![](autosizing-row-height-images/table03a.png "プロパティのパッドのレイアウト セクション")](autosizing-row-height-images/table03a.png#lightbox)

拡張する要素の設定、**低い**Hugging 優先度の値、および**低い**圧縮耐性を確保する優先度の値。

次に、セル プロトタイプを選択し、一意なを指定する必要があります**識別子**:

[![](autosizing-row-height-images/table04.png "セルのプロトタイプの一意の識別子を与える")](autosizing-row-height-images/table04.png#lightbox)

この例の場合`GrowCell`です。 後で、テーブルの作成時にこの値が使用されます。

> [!IMPORTANT]
> テーブルには、1 つ以上のセルの種類が含まれている場合 (**プロトタイプ**)、種類ごとに独自の一意なことを確認する必要があります`Identifier`の自動動作する行がサイズ変更します。

このセルのプロトタイプの各要素に対して割り当てる、**名前**c# コードに公開します。 例えば:

[![](autosizing-row-height-images/table05.png "C# コードに公開する名前を割り当てる")](autosizing-row-height-images/table05.png#lightbox)

カスタム クラスを次に、追加、 `UITableViewController`、`UITableView`と`UITableCell`(プロトタイプ)。 例えば: 

[![](autosizing-row-height-images/table06.png "UITableViewController や、UITableView、UITableCell カスタム クラスを追加します。")](autosizing-row-height-images/table06.png#lightbox)

最後に、必要なすべてのコンテンツのラベルに表示されることを確認、設定、**行**プロパティを`0`:

[![](autosizing-row-height-images/table06.png "行のプロパティが 0 に設定")](autosizing-row-height-images/table06a.png#lightbox)

UI が定義されているを使用した自動行の高さのサイズ変更を有効にするコードを追加してみましょう。

## <a name="enabling-auto-resizing-height"></a>高さの自動サイズ変更を有効にします。

いずれかで、テーブル ビューのデータ ソース (`UITableViewDatasource`) またはソース (`UITableViewSource`) を使用する必要がありますのセルをデキューおときに、`Identifier`デザイナーで選択しました。 例えば:

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

既定では、テーブル ビューを自動サイズ変更行の高さに設定されます。 これには、ことを確認する、`RowHeight`プロパティに設定する必要があります`UITableView.AutomaticDimension`です。 設定する必要があります、`EstimatedRowHeight`プロパティに、`UITableViewController`です。 例えば:

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

この推定値は正確である必要はありませんテーブル ビューの各行の高さの平均値の概算だけです。

場所でこのコードでは、アプリを実行すると、各行は圧縮し、セルのプロトタイプの最後のラベルの高さに基づいて、拡張されます。 例えば:

[![](autosizing-row-height-images/table07.png "実行のサンプル テーブル")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>関連リンク

- [GrowRowTable (サンプル)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
