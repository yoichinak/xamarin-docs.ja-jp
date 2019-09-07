---
title: Xamarin.Forms ListView
description: このガイドでは、対話型リストにデータを表示するために使用できる、Xamarin. Forms ListView を紹介します。
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/04/2019
ms.openlocfilehash: 5d09d76a44a6322285a143230173d244848ba4a6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770226"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)は、データのリストを表示するためのビューです。特に、スクロールが必要な長いリストです。

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、さまざまなレイアウト仕様を使用してデータを一覧表示するためのビューです。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 詳細は、「[Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)」を参照してください。

## <a name="use-cases"></a>使用事例

`ListView`コントロールは、スクロール可能なデータリストが表示されている状況で使用できます。 クラス`ListView`は、コンテキストアクションとデータバインディングをサポートしています。

コントロールと混同しないように[してください。`TableView`](~/xamarin-forms/user-interface/tableview.md) `ListView` オプション`TableView`やデータのバインドされていないリストがある場合は常に、コントロールの方が適しています。これは、XAML で定義済みのオプションを指定できるためです。 たとえば、iOS 設定アプリは、ほとんどの場合、事前に定義されているオプションのセットを使用`TableView` `ListView`することをお勧めします。

クラス`ListView`は、XAML でのリスト項目の`ItemsSource`定義`ItemTemplate`をサポートしていません。リスト内の項目を定義するには、プロパティまたはデータバインディングをで使用する必要があります。

は、1つのデータ型で構成されるコレクションに最適です。`ListView` この要件は、リストの各行に使用できるセルの種類が1つだけであるためです。 コントロール`TableView`は複数のセルの種類をサポートするため、複数のデータ型を表示する必要がある場合は、より適切なオプションになります。

`ListView`インスタンスにデータをバインドする方法の詳細については、「 [ListView データソース](~/xamarin-forms/user-interface/listview/data-and-databinding.md)」を参照してください。

## <a name="components"></a>コンポーネント
`ListView`コントロールには、各プラットフォームのネイティブ機能を実行するために使用できる多数のコンポーネントがあります。 これらのコンポーネントは、次のセクションで定義されています。

### <a name="headers-and-footerscustomizing-list-appearancemdheaders-and-footers"></a>[ヘッダーとフッター](customizing-list-appearance.md#headers-and-footers)

ヘッダーとフッターのコンポーネントは、リストのデータとは別に、リストの先頭と末尾に表示されます。 ヘッダーとフッターは、ListView のデータソースとは別のデータソースにバインドできます。

### <a name="groupscustomizing-list-appearancemdgrouping"></a>[グループ](customizing-list-appearance.md#grouping)

ナビゲーションを容易`ListView`にするために、のデータをグループ化することができます。 グループは通常、データバインドされます。 次のスクリーンショットは`ListView` 、グループ化されたデータを含むを示しています。

(images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "Listview 内のグループ化")されたデータのグループ化されたデータ[ ![]

### <a name="cellscustomizing-cell-appearancemd"></a>[セル](customizing-cell-appearance.md)

内のデータ項目`ListView`は、セルと呼ばれます。 各セルは、データの行に対応します。 選択する組み込みのセルがまたは独自のカスタムのセルを定義することができます。 組み込みとカスタムの両方のセルには、XAML またはコードで使用される定義を指定できます。

- `TextCell`やなど`ImageCell`の[組み込みセル](customizing-cell-appearance.md#built-in-cells)は、ネイティブコントロールに対応しており、特にパフォーマンスに優れています。
  - に[`TextCell`](customizing-cell-appearance.md#textcell)は、テキストの文字列が表示されます。オプションで、詳細テキストを表示できます。 詳細なテキストはアクセントの色とフォント サイズを小さくの 2 行目としてレンダリングされます。
  - に[`ImageCell`](customizing-cell-appearance.md#imagecell)は、画像とテキストが表示されます。 は、 `TextCell`左側にイメージを含むとして表示されます。
- [カスタムセル](customizing-cell-appearance.md#customcells)は、複雑なデータを表示するために使用されます。 たとえば、カスタムセルを使用して、アルバムとアーティストを含む曲の一覧を表示できます。

次のスクリーンショットは`ListView` 、with ImageCell items を示しています。

Listview の["ImageCell items in a listview" ImageCell items ![](images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "")

で`ListView`のセルのカスタマイズの詳細については、「 [ListView セルの外観のカスタマイズ](customizing-cell-appearance.md)」を参照してください。

## <a name="functionality"></a>機能
クラス`ListView`は、さまざまな相互作用スタイルをサポートしています。

- [プルから更新](interactivity.md#pull-to-refresh)を行うことで、 `ListView`ユーザーはコンテンツを最新の状態に更新できます。
- [コンテキストアクション](interactivity.md#context-actions)を使用すると、開発者は個々のリスト項目に対してカスタムアクションを指定できます。 たとえば、ios では、スワイプのアクションを実装したり時間の長い Android でのアクションをタップできます。
- [選択](interactivity.md#selectiontaps)すると、開発者はリスト項目の選択イベントおよび deselection イベントに機能をアタッチできます。

次のスクリーンショットは`ListView` 、コンテキストアクションを含むを示しています。

(images/context-default-cropped.png)](images/context-default.png#lightbox "Listview における") ["listview" コンテキストアクションのコンテキストアクション![]

の対話機能の詳細について`ListView`は、「 [ListView との対話 & 操作](interactivity.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [ListView での操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [2 つの方法 (サンプル) をバインド](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [セルで構築された (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [カスタムのセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [グループ化 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Custom Renderer View (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView の対話機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
