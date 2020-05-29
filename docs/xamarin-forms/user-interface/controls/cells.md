---
title: Xamarin.Formsセル
description: Xamarin.Formsセルは、ListViews と TableViews に追加できます。 この記事では、に含まれるセルの一覧を示し Xamarin.Forms ます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bd1a2398787fe39c0b4cbd08ccd5c5793775d5cf
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137281"
---
# <a name="xamarinforms-cells"></a>Xamarin.Formsセル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin. フォームセルは、ListViews と TableViews に追加できます。_

*セル*は、テーブル内の項目に使用される特殊な要素で、リスト内の各項目をどのように表示するかを記述します。 [`Cell`](xref:Xamarin.Forms.Cell)クラスはから派生 [`Element`](xref:Xamarin.Forms.Element) [`VisualElement`](xref:Xamarin.Forms.Element) します。このからも派生します。 セルはそれ自体がビジュアル要素ではありません。代わりに、ビジュアル要素を作成するためのテンプレートです。

`Cell`は、およびコントロールでのみ使用され [`ListView`](views.md#listview) [`TableView`](views.md#tableview) ます。 セルを使用およびカスタマイズする方法については、およびのドキュメントを参照してください [`ListView`](~/xamarin-forms/user-interface/listview/index.md) [`TableView`](~/xamarin-forms/user-interface/tableview.md) 。

## <a name="cells"></a>セル

Xamarin.Formsでは、次のセルの種類がサポートされています。

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| に [`TextCell`](xref:Xamarin.Forms.TextCell) は、1つまたは2つのテキスト文字列が表示されます。 [`Text`](xref:Xamarin.Forms.TextCell.Text)プロパティと、必要に応じて、 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) プロパティをこれらのテキスト文字列に設定します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.TextCell)  / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![TextCell の例](cells-images/TextCell.png "TextCell の例")](cells-images/TextCell-Large.png#lightbox "TextCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| には [`ImageCell`](xref:Xamarin.Forms.ImageCell) と同じ情報が表示され [`TextCell`](#textCell) ますが、プロパティで設定したビットマップが含まれてい [`Source`](xref:Xamarin.Forms.Image.Source) ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.ImageCell)  / [ガイド](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell の例](cells-images/ImageCell.png "ImageCell の例")](cells-images/ImageCell-Large.png#lightbox "ImageCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| には、 [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) プロパティが設定されたテキストセット [`Text`](xref:Xamarin.Forms.SwitchCell.Text) と、ブール型プロパティを使用して初期設定されたオン/オフスイッチが含まれてい [`On`](xref:Xamarin.Forms.SwitchCell.On) ます。 [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged)プロパティが変更されたときに通知されるイベントを処理し `On` ます。<br /><br />[API ドキュメント](xref:Xamarin.Forms.SwitchCell)  / [ガイド](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell の例](cells-images/SwitchCell.png "SwitchCell の例")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| は、 [`EntryCell`](xref:Xamarin.Forms.EntryCell) [`Label`](xref:Xamarin.Forms.EntryCell.Label) プロパティで、セルおよび編集可能なテキストの単一行を識別するプロパティを定義し [`Text`](xref:Xamarin.Forms.EntryCell.Text) ます。 [`Completed`](xref:Xamarin.Forms.EntryCell.Completed)ユーザーがテキスト入力を完了したときに通知されるイベントを処理します。<br /><br />[API ドキュメント](xref:Xamarin.Forms.EntryCell)  / [ガイド](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell の例](cells-images/EntryCell.png "EntryCell の例")](cells-images/EntryCell-Large.png#lightbox "EntryCell の例")<br />[このページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  の C# コード[XAML ページ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>関連リンク

- [Xamarin.Formsフォームギャラリーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsSamples](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
