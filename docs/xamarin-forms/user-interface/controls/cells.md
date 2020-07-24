---
title: :::no-loc(Xamarin.Forms):::セル
description: ':::no-loc(Xamarin.Forms):::セルは、ListViews と TableViews に追加できます。 この記事では、に含まれるセルの一覧を示し :::no-loc(Xamarin.Forms)::: ます。'
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 21fca31ca9e1cf39843e04c3c381bb41ef77335f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997281"
---
# <a name="no-locxamarinforms-cells"></a>:::no-loc(Xamarin.Forms):::セル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_:::no-loc(Xamarin.Forms):::セルは、ListViews と TableViews に追加できます。_

*セル*は、テーブル内の項目に使用される特殊な要素で、リスト内の各項目をどのように表示するかを記述します。 [`Cell`](xref::::no-loc(Xamarin.Forms):::.Cell)クラスはから派生 [`Element`](xref::::no-loc(Xamarin.Forms):::.Element) [`VisualElement`](xref::::no-loc(Xamarin.Forms):::.Element) します。このからも派生します。 セルはそれ自体がビジュアル要素ではありません。代わりに、ビジュアル要素を作成するためのテンプレートです。

`Cell`は、およびコントロールでのみ使用され [`ListView`](xref::::no-loc(Xamarin.Forms):::.ListView) [`TableView`](xref::::no-loc(Xamarin.Forms):::.TableView) ます。 セルを使用およびカスタマイズする方法については、およびのドキュメントを参照してください [`ListView`](~/xamarin-forms/user-interface/listview/index.md) [`TableView`](~/xamarin-forms/user-interface/tableview.md) 。

## <a name="cells"></a>セル

:::no-loc(Xamarin.Forms):::では、次のセルの種類がサポートされています。

| 種類 | 説明 | 外観 |
| --- | --- | --- |
| `TextCell` | に [`TextCell`](xref::::no-loc(Xamarin.Forms):::.TextCell) は、1つまたは2つのテキスト文字列が表示されます。 [`Text`](xref::::no-loc(Xamarin.Forms):::.TextCell.Text)プロパティと、必要に応じて、 [`Detail`](xref::::no-loc(Xamarin.Forms):::.TextCell.Detail) プロパティをこれらのテキスト文字列に設定します。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.TextCell)  / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![TextCell の例](cells-images/TextCell.png "TextCell の例")](cells-images/TextCell-Large.png#lightbox "TextCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
| `ImageCell` | には [`ImageCell`](xref::::no-loc(Xamarin.Forms):::.ImageCell) と同じ情報が表示され [`TextCell`](xref::::no-loc(Xamarin.Forms):::.TextCell) ますが、プロパティで設定したビットマップが含まれてい [`Source`](xref::::no-loc(Xamarin.Forms):::.Image.Source) ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.ImageCell)  / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell の例](cells-images/ImageCell.png "ImageCell の例")](cells-images/ImageCell-Large.png#lightbox "ImageCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
| `SwitchCell` | には、 [`SwitchCell`](xref::::no-loc(Xamarin.Forms):::.SwitchCell) プロパティが設定されたテキストセット [`Text`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.Text) と、ブール型プロパティを使用して初期設定されたオン/オフスイッチが含まれてい [`On`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.On) ます。 [`OnChanged`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.OnChanged)プロパティが変更されたときに通知されるイベントを処理し `On` ます。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.SwitchCell)  / [ガイド](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell の例](cells-images/SwitchCell.png "SwitchCell の例")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
| `EntryCell` | は、 [`EntryCell`](xref::::no-loc(Xamarin.Forms):::.EntryCell) [`Label`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Label) プロパティで、セルおよび編集可能なテキストの単一行を識別するプロパティを定義し [`Text`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Text) ます。 [`Completed`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Completed)ユーザーがテキスト入力を完了したときに通知されるイベントを処理します。<br /><br />[API ドキュメント](xref::::no-loc(Xamarin.Forms):::.EntryCell)  / [ガイド](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell の例](cells-images/EntryCell.png "EntryCell の例")](cells-images/EntryCell-Large.png#lightbox "EntryCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
| | | |

## <a name="related-links"></a>関連リンク

- [:::no-loc(Xamarin.Forms):::フォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms)::: サンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=:::no-loc(Xamarin.Forms):::)
- [:::no-loc(Xamarin.Forms):::API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
