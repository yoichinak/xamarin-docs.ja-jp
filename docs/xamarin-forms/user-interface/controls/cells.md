---
title: Xamarin.Forms セル
description: Xamarin.Forms セルは、ListViews と TableViews に追加できます。 この記事では、に含まれるセルの一覧を示し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01f625d9ecfb91bc36013b7f6d45fb3d275e8bee
ms.sourcegitcommit: 10c7dd16fe78226053d1d036492b6c9102fc421b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "93370833"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms セル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Forms セルは、ListViews と TableViews に追加できます。_

*セル* は、テーブル内の項目に使用される特殊な要素で、リスト内の各項目をどのように表示するかを記述します。 [`Cell`](xref:Xamarin.Forms.Cell)クラスはから派生 [`Element`](xref:Xamarin.Forms.Element) [`VisualElement`](xref:Xamarin.Forms.Element) します。このからも派生します。 セルはそれ自体がビジュアル要素ではありません。代わりに、ビジュアル要素を作成するためのテンプレートです。

`Cell` は、およびコントロールでのみ使用され [`ListView`](xref:Xamarin.Forms.ListView) [`TableView`](xref:Xamarin.Forms.TableView) ます。 セルを使用およびカスタマイズする方法については、およびのドキュメントを参照してください [`ListView`](~/xamarin-forms/user-interface/listview/index.md) [`TableView`](~/xamarin-forms/user-interface/tableview.md) 。

## <a name="cells"></a>セル

Xamarin.Forms では、次のセルの種類がサポートされています。

| Type | 説明 | 外観 |
| --- | --- | --- |
| `TextCell` | に [`TextCell`](xref:Xamarin.Forms.TextCell) は、1つまたは2つのテキスト文字列が表示されます。 [`Text`](xref:Xamarin.Forms.TextCell.Text)プロパティと、必要に応じて、 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) プロパティをこれらのテキスト文字列に設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TextCell)  / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![TextCell の例](cells-images/TextCell.png "TextCell の例")](cells-images/TextCell-Large.png#lightbox "TextCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
| `ImageCell` | には [`ImageCell`](xref:Xamarin.Forms.ImageCell) と同じ情報が表示され [`TextCell`](xref:Xamarin.Forms.TextCell) ますが、プロパティで設定したビットマップが含まれてい [`Source`](xref:Xamarin.Forms.Image.Source) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ImageCell)  / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell の例](cells-images/ImageCell.png "ImageCell の例")](cells-images/ImageCell-Large.png#lightbox "ImageCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
| `SwitchCell` | には、 [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) プロパティが設定されたテキストセット [`Text`](xref:Xamarin.Forms.SwitchCell.Text) と、ブール型プロパティを使用して初期設定されたオン/オフスイッチが含まれてい [`On`](xref:Xamarin.Forms.SwitchCell.On) ます。 [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged)プロパティが変更されたときに通知されるイベントを処理し `On` ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SwitchCell)  / [ガイド](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell の例](cells-images/SwitchCell.png "SwitchCell の例")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
| `EntryCell` | は、 [`EntryCell`](xref:Xamarin.Forms.EntryCell) [`Label`](xref:Xamarin.Forms.EntryCell.Label) プロパティで、セルおよび編集可能なテキストの単一行を識別するプロパティを定義し [`Text`](xref:Xamarin.Forms.EntryCell.Text) ます。 [`Completed`](xref:Xamarin.Forms.EntryCell.Completed)ユーザーがテキスト入力を完了したときに通知されるイベントを処理します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.EntryCell)  / [ガイド](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell の例](cells-images/EntryCell.png "EntryCell の例")](cells-images/EntryCell-Large.png#lightbox "EntryCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
| | | |

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms フォームギャラリーのサンプル](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms サンプル](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API ドキュメント](/dotnet/api/xamarin.forms?view=xamarin-forms)