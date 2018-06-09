---
title: Xamarin.Forms セル
description: Xamarin.Forms のセルは、Listview および TableViews に追加できます。 この記事では、Xamarin.Forms 内に含まれるセルが一覧表示します。
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0947027b43eacd0bac269ebf7a779746e0d22866
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243357"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms セル

_Xamarin.Forms のセルは、Listview および TableViews に追加できます。_

A*セル*テーブル内の項目に使用される特殊な要素があり、一覧内の各項目の表示方法について説明します。 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)クラスから派生[ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/)、元の[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/)も派生します。 セル自体ではなく視覚的要素視覚的要素を作成するためのテンプレートでは代わりにします。

`Cell` 排他的に使用される[ `ListView` ](views.md#listView)と[ `TableView` ](views.md#tableView)コントロール。 使用して、セルをカスタマイズする方法についてを参照してください、 [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md)と[ `TableView` ](~/xamarin-forms/user-interface/tableview.md)ドキュメント。

## <a name="cells"></a>セル

Xamarin.Forms では、次のセルの種類をサポートします。

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) 1 つまたは 2 つのテキスト文字列が表示されます。 設定、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/)プロパティし、必要に応じて、 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/)プロパティをこれらのテキスト文字列。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell 例](cells-images/TextCell.png "TextCell 例")](cells-images/TextCell-Large.png#lightbox "TextCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell)と同じ情報が表示されます[ `TextCell` ](#textCell)で設定したビットマップが含まれていますが、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/)プロパティです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell 例](cells-images/ImageCell.png "ImageCell 例")](cells-images/ImageCell-Large.png#lightbox "ImageCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell)で設定されたテキストを含む、 [ `Text`'](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/)プロパティおよびとオン/オフの切り替え、ブール値に初期設定[ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/)プロパティです。 処理、 [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/)イベント通知を受けるときに、`On`プロパティが変更されました。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [ガイド](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell 例](cells-images/SwitchCell.png "SwitchCell 例")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell)定義、 [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/)セルと編集可能なテキストの 1 行を識別するプロパティ、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/)プロパティです。 処理、 [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/)ユーザーが入力したテキストを完了したときに通知するイベントです。<br /><br />[API のドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [ガイド](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell 例](cells-images/EntryCell.png "EntryCell 例")](cells-images/EntryCell-Large.png#lightbox "EntryCell 例")<br />[このページの c# コード](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery サンプル](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/root/Xamarin.Forms/)
