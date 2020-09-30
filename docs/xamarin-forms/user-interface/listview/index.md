---
title: Xamarin.Forms 大きい
description: このガイドでは ListView を紹介します。これを使用すると、 Xamarin.Forms 対話形式でデータを表示できます。
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fa3769d2533a5a6b482c92b832d54506e4954250
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91560144"
---
# <a name="no-locxamarinforms-listview"></a>Xamarin.Forms 大きい

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView) は、データのリストを表示するためのビューです。特に、スクロールが必要な長いリストです。

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、さまざまなレイアウト仕様を使用してデータを一覧表示するためのビューです。 これは、より柔軟でパフォーマンスの高い代替手段を提供することを目的として [`ListView`](xref:Xamarin.Forms.ListView) います。 詳細は、「[Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)」を参照してください。

## <a name="use-cases"></a>ユース ケース

コントロールは、 `ListView` スクロール可能なデータリストが表示されている状況で使用できます。 クラスは、 `ListView` コンテキストアクションとデータバインディングをサポートしています。

コントロール `ListView` と混同しないようにして [`TableView`](~/xamarin-forms/user-interface/tableview.md) ください。 `TableView`オプションやデータのバインドされていないリストがある場合は常に、コントロールの方が適しています。これは、XAML で定義済みのオプションを指定できるためです。 たとえば、iOS 設定アプリは、ほとんどの場合、事前に定義されているオプションのセットを使用することをお勧めし `TableView` `ListView` ます。

クラスは、 `ListView` XAML でのリスト項目の定義をサポートしていません `ItemsSource` `ItemTemplate` 。リスト内の項目を定義するには、プロパティまたはデータバインディングをで使用する必要があります。

は、 `ListView` 1 つのデータ型で構成されるコレクションに最適です。 この要件は、リストの各行に使用できるセルの種類が1つだけであるためです。 `TableView`コントロールは複数のセルの種類をサポートするため、複数のデータ型を表示する必要がある場合は、より適切なオプションになります。

インスタンスにデータをバインドする方法の詳細につい `ListView` ては、「 [ListView データソース](~/xamarin-forms/user-interface/listview/data-and-databinding.md)」を参照してください。

## <a name="components"></a>コンポーネント

`ListView`コントロールには、各プラットフォームのネイティブ機能を実行するために使用できる多数のコンポーネントがあります。 これらのコンポーネントは、次のセクションで定義されています。

### <a name="headers-and-footers"></a>[ヘッダーとフッター](customizing-list-appearance.md#headers-and-footers)

ヘッダーとフッターのコンポーネントは、リストのデータとは別に、リストの先頭と末尾に表示されます。 ヘッダーとフッターは、ListView のデータソースとは別のデータソースにバインドできます。

### <a name="groups"></a>[グループ](customizing-list-appearance.md#grouping)

ナビゲーションを容易にするために、のデータを `ListView` グループ化することができます。 グループは通常、データバインドされます。 次のスクリーンショットは、 `ListView` グループ化されたデータを含むを示しています。

[!["ListView でのグループ化されたデータ"](images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "ListView でのグループ化されたデータ")

### <a name="cells"></a>[セル](customizing-cell-appearance.md)

内のデータ項目 `ListView` は、セルと呼ばれます。 各セルは、データの行に対応します。 選択できる組み込みセルがあります。または、独自のカスタムセルを定義することもできます。 XAML またはコードでは、組み込みセルとカスタムセルの両方を使用または定義できます。

- やなどの[組み込みセル](customizing-cell-appearance.md#built-in-cells)は、 `TextCell` `ImageCell` ネイティブコントロールに対応しており、特にパフォーマンスに優れています。
  - には、 [`TextCell`](customizing-cell-appearance.md#textcell) テキストの文字列が表示されます。オプションで、詳細テキストを表示できます。 詳細テキストは、アクセントカラーの小さいフォントで2行目としてレンダリングされます。
  - には、 [`ImageCell`](customizing-cell-appearance.md#imagecell) 画像とテキストが表示されます。 は、左側にイメージを含むとして表示され `TextCell` ます。
- [カスタムセル](customizing-cell-appearance.md#custom-cells) は、複雑なデータを表示するために使用されます。 たとえば、カスタムセルを使用して、アルバムとアーティストを含む曲の一覧を表示できます。

次のスクリーンショットは、with ImageCell items を示してい `ListView` ます。

[!["ListView 内の項目の ImageCell"](images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "ListView の ImageCell 項目")

でのセルのカスタマイズの詳細につい `ListView` ては、「 [ListView セルの外観のカスタマイズ](customizing-cell-appearance.md)」を参照してください。

## <a name="functionality"></a>機能

クラスは、 `ListView` さまざまな相互作用スタイルをサポートしています。

- [プルから更新](interactivity.md#pull-to-refresh) を行うことで、ユーザーはコンテンツを最新の状態に `ListView` 更新できます。
- [コンテキストアクション](interactivity.md#context-actions) を使用すると、開発者は個々のリスト項目に対してカスタムアクションを指定できます。 たとえば、iOS へのスワイプ操作や、Android での長いタップ操作を実装できます。
- [選択](interactivity.md#selection-and-taps) すると、開発者はリスト項目の選択イベントおよび deselection イベントに機能をアタッチできます。

次のスクリーンショットは、 `ListView` コンテキストアクションを含むを示しています。

[!["ListView のコンテキストアクション"](images/context-default-cropped.png)](images/context-default.png#lightbox "ListView のコンテキストアクション")

の対話機能の詳細については `ListView` 、「 [ListView との対話 & 操作](interactivity.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [ListView の操作 (サンプル)](/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [双方向のバインディング (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [組み込みセル (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [カスタムセル (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [グループ化 (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [カスタムレンダラービュー (サンプル)](/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView のインタラクティビティ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)