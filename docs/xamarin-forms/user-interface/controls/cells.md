---
title: Xamarin.Forms のセル
description: Xamarin.Forms セルは、Listview および TableViews に追加できます。 この記事では、Xamarin.Forms に含まれるセルを一覧表示します。
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5ef1aba847954ccdeb230acd82ebbc5015ebd6b7
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55292299"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms のセル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)

_Xamarin.Forms セルは、Listview および TableViews に追加できます。_

A*セル*はテーブル内のアイテムに使用される特殊な要素であり、リストの各項目を表示する方法について説明します。 [ `Cell` ](xref:Xamarin.Forms.Cell)クラスから派生[ `Element` ](xref:Xamarin.Forms.Element)、元の[ `VisualElement` ](xref:Xamarin.Forms.Element)も派生します。 セル自体ではない視覚的要素。視覚的要素を作成するためのテンプレートでは代わりにします。

`Cell` 排他的に使用[ `ListView` ](views.md#listView)と[ `TableView` ](views.md#tableView)コントロール。 使用して、セルのカスタマイズ方法についてを参照してください、 [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md)と[ `TableView` ](~/xamarin-forms/user-interface/tableview.md)ドキュメント。

## <a name="cells"></a>セル

Xamarin.Forms には、次のセルの種類がサポートされています。

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](xref:Xamarin.Forms.TextCell) 1 つまたは 2 つのテキスト文字列が表示されます。 設定、 [ `Text` ](xref:Xamarin.Forms.TextCell.Text)プロパティと、オプションで、 [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail)プロパティをこれらのテキスト文字列。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TextCell) / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell 例](cells-images/TextCell.png "TextCell 例")](cells-images/TextCell-Large.png#lightbox "TextCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](xref:Xamarin.Forms.ImageCell)と同じ情報を表示します。 [ `TextCell` ](#textCell)で設定したビットマップが含まれていますが、 [ `Source` ](xref:Xamarin.Forms.Image.Source)プロパティ。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ImageCell) / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell 例](cells-images/ImageCell.png "ImageCell 例")](cells-images/ImageCell-Large.png#lightbox "ImageCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell)で設定されたテキストが含まれています、 [ `Text`'](xref:Xamarin.Forms.SwitchCell.Text)プロパティとのオン/オフ スイッチと、ブール値の初期設定[ `On` ](xref:Xamarin.Forms.SwitchCell.On)プロパティ。 処理、 [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged)イベント時に通知が、`On`プロパティの変更。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SwitchCell) / [ガイド](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell 例](cells-images/SwitchCell.png "SwitchCell 例")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](xref:Xamarin.Forms.EntryCell)定義、 [ `Label` ](xref:Xamarin.Forms.EntryCell.Label)セルと編集可能なテキストでの 1 行を識別するプロパティ、 [ `Text` ](xref:Xamarin.Forms.EntryCell.Text)プロパティ。 処理、 [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed)ユーザーには、テキスト入力が完了したときに通知するイベントです。<br /><br />[API ドキュメント](xref:Xamarin.Forms.EntryCell) / [ガイド](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell 例](cells-images/EntryCell.png "EntryCell 例")](cells-images/EntryCell-Large.png#lightbox "EntryCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
