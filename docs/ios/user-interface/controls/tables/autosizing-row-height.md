---
title: Xamarin の行の高さの自動調整
description: このドキュメントでは、Xamarin に追加する方法について説明します。 iOS アプリテーブルビューでは、コンテンツに基づいて高さが変化します。 IOS Designer のセルレイアウトと、自動サイズ変更の高さを有効にする方法について説明します。
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: eedf76a4ecc566a18f4d4b7d5c4f1b63642b8e25
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437199"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Xamarin の行の高さの自動調整

IOS 8 以降では、テーブルビュー () を作成する機能が追加されました。このビューでは、 `UITableView` 自動レイアウト、サイズのクラス、および制約を使用して、特定の行の高さをコンテンツのサイズに基づいて自動的に拡大縮小することができます。

iOS 11 では、行を自動的に拡張する機能が追加されました。 ヘッダー、フッター、およびセルのサイズは、コンテンツに基づいて自動的に設定できるようになりました。 ただし、テーブルが iOS デザイナーで作成された場合、Interface Builder ている場合、または行の高さが固定されている場合は、このガイドで説明されているように、手動でサイズ変更セルを有効にする必要があります。

## <a name="cell-layout-in-the-ios-designer"></a>IOS デザイナーでのセルレイアウト

IOS Designer で行の自動サイズ変更を行うテーブルビューのストーリーボードを開き、セルの *プロトタイプ* を選択して、セルのレイアウトをデザインします。 次に例を示します。

[![セルのプロトタイプデザイン](autosizing-row-height-images/table01.png)](autosizing-row-height-images/table01.png#lightbox)

プロトタイプ内の各要素に対して制約を追加して、ローテーションまたはさまざまな iOS デバイスの画面サイズに合わせてテーブルビューのサイズが変更されたときに要素を正しい位置に保つようにします。 たとえば、を `Title` セルの *コンテンツビュー*の左上にピン留めすると、次のようになります。

[![セルのコンテンツビューの左上と左右にタイトルをピン留めする](autosizing-row-height-images/table02.png)](autosizing-row-height-images/table02.png#lightbox)

例のテーブルの場合、小 `Label` (の下) は、 `Title` 行の高さを増減するために縮小および拡大できるフィールドです。 この効果を実現するには、ラベルの左、右、上、下にピン留めするために、次の制約を追加します。

[![これらの制約により、ラベルの左、右、上、下をピン留めします。](autosizing-row-height-images/table03.png)](autosizing-row-height-images/table03.png#lightbox)

これで、セル内の要素が完全に制約されたので、どの要素を拡張する必要があるかを明確にする必要があります。 これを行うには、Properties Pad の**レイアウト**セクションで、必要に応じて**content Hugging Priority**および**Content Compression 抵抗の優先度**を設定します。

[![Properties Pad のレイアウトセクション](autosizing-row-height-images/table03a.png)](autosizing-row-height-images/table03a.png#lightbox)

展開する要素を、 **低い** Hugging 優先度値に設定し、圧縮の抵抗優先度の値を **低く** 設定します。

次に、セルプロトタイプを選択し、一意の **識別子**を指定する必要があります。

[![セルプロトタイプに一意の識別子を付ける](autosizing-row-height-images/table04.png)](autosizing-row-height-images/table04.png#lightbox)

この例の場合は、「」のように `GrowCell` なります。 この値は、後でテーブルにデータを設定するときに使用します。

> [!IMPORTANT]
> テーブルに複数のセルの種類 (**プロトタイプ**) が含まれている場合は、 `Identifier` 行の自動サイズ変更が機能するように、各型が独自の一意であることを確認する必要があります。

セルプロトタイプの要素ごとに、 **名前** を割り当てて C# コードに公開します。 次に例を示します。

[![名前を割り当てて C# コードに公開する](autosizing-row-height-images/table05.png)](autosizing-row-height-images/table05.png#lightbox)

次に、、、 `UITableViewController` `UITableView` および (プロトタイプ) のカスタムクラスを追加し `UITableCell` ます。 次に例を示します。

[![UITableViewController、UITableView、および UITableCell のカスタムクラスを追加する](autosizing-row-height-images/table06.png)](autosizing-row-height-images/table06.png#lightbox)

最後に、予想されるすべてのコンテンツがラベルに表示されるようにするには、[ **Lines** ] プロパティをに設定し `0` ます。

[![Lines プロパティが0に設定されています。](autosizing-row-height-images/table06.png)](autosizing-row-height-images/table06a.png#lightbox)

UI が定義されているので、行の高さの自動サイズ変更を有効にするコードを追加してみましょう。

## <a name="enabling-auto-resizing-height"></a>自動サイズ変更の高さを有効にする

テーブルビューの Datasource ( `UITableViewDatasource` ) または Source () のいずれかで `UITableViewSource` 、セルをデキューするときに、デザイナーで定義したを使用する必要があり `Identifier` ます。 次に例を示します。

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

既定では、テーブルビューは行の高さを自動的に変更するように設定されます。 これを実現するには、 `RowHeight` プロパティをに設定する必要があり `UITableView.AutomaticDimension` ます。 また、でプロパティを設定する必要もあり `EstimatedRowHeight` `UITableViewController` ます。 次に例を示します。

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

この見積もりは正確である必要はなく、テーブルビューの各行の平均高さの概算値にすぎません。

このコードを配置すると、アプリを実行すると、セルプロトタイプの最後のラベルの高さに基づいて各行が縮小および拡大されます。 次に例を示します。

[![サンプルテーブルの実行](autosizing-row-height-images/table07.png)](autosizing-row-height-images/table07.png#lightbox)

## <a name="related-links"></a>関連リンク

- [GrowRowTable (サンプル)](/samples/xamarin/ios-samples/growrowtable)