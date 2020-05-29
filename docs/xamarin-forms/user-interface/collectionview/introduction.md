---
title: Xamarin.FormsCollectionView の概要
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d6a09ead9c3def2f58ad2755de4574f6d6e331e8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136436"
---
# <a name="xamarinforms-collectionview-introduction"></a>Xamarin.FormsCollectionView の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、さまざまなレイアウト仕様を使用してデータの一覧を表示するためのビューです。 これは、より柔軟でパフォーマンスの高い代替手段を提供することを目的として [`ListView`](xref:Xamarin.Forms.ListView) います。 たとえば、次のスクリーンショットは、 `CollectionView` 2 つの列の垂直グリッドを使用し、複数の選択を可能にするを示しています。

[![IOS と Android の CollectionView 垂直グリッドレイアウトのスクリーンショット](introduction-images/verticalgrid-multipleselection.png "複数選択の CollectionView 垂直グリッドレイアウト")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "複数選択の CollectionView 垂直グリッドレイアウト")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は4.3 から使用でき Xamarin.Forms ます。

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は iOS と Android で使用できますが、ユニバーサル Windows プラットフォームでのみ[一部利用でき](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14)ます。

## <a name="collectionview-and-listview-differences"></a>CollectionView と ListView の相違点

[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView) Api と api は似ていますが、いくつかの重要な違いがあります。

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、データを垂直方向または水平方向にリストまたはグリッドで表示できる柔軟なレイアウトモデルが用意されています。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)単一および複数の選択をサポートします。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)セルの概念はありません。 代わりに、データテンプレートを使用して、リスト内の各データ項目の外観を定義します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に利用します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)の API サーフェイスを減らし [`ListView`](xref:Xamarin.Forms.ListView) ます。 のプロパティとイベントの多く [`ListView`](xref:Xamarin.Forms.ListView) は、には存在しません `CollectionView` 。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)UI スレッドからが更新された場合、は例外をスロー [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) します。

## <a name="move-from-listview-to-collectionview"></a>ListView から CollectionView への移動

[`ListView`](xref:Xamarin.Forms.ListView)既存の実装の実装は Xamarin.Forms [`CollectionView`](xref:Xamarin.Forms.CollectionView) 、次の表を参考にして実装に移行できます。

| 概念 | ListView API | CollectionView |
|---|---|---|
| データ | `ItemsSource` | には、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) プロパティを設定することによってデータが設定され `ItemsSource` ます。 詳細については、「データを使用した[CollectionView の設定](populate-data.md#populate-a-collectionview-with-data)」を参照してください。 |
| 項目の外観 | `ItemTemplate` | の各項目の外観は、プロパティをに設定すること [`CollectionView`](xref:Xamarin.Forms.CollectionView) によって定義でき `ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 詳細については、「[アイテムの外観を定義](populate-data.md#define-item-appearance)する」を参照してください。 |
| セル | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)セルの概念がないため、公開インジケーターの概念はありません。 代わりに、データテンプレートを使用して、リスト内の各データ項目の外観を定義します。 |
| 行区切り記号 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。 これらは、必要に応じて項目テンプレートに指定できます。 |
| 選択 | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)単一および複数の選択をサポートします。 詳細については、「 [ Xamarin.Forms CollectionView Selection](selection.md)」を参照してください。 |
| 行の高さ | `HasUnevenRows`, `RowHeight` | では `CollectionView` 、各項目の行の高さはプロパティによって決定され `ItemSizingStrategy` ます。 詳細については、「[項目のサイズ](layout.md#item-sizing)設定」を参照してください。|
| キャッシュ | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に使用します。 |
| ヘッダーとフッター | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、、、、およびの各プロパティを使用して、リスト内の項目と共にスクロールするヘッダーとフッターを提供でき `Header` `Footer` `HeaderTemplate` `FooterTemplate` ます。 詳細については、「[ヘッダーとフッター](layout.md#headers-and-footers)」を参照してください。 |
| グループ化 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)プロパティをに設定して、適切にグループ化されたデータ `IsGrouped` を表示 `true` します。 グループヘッダーとグループフッターは、 `GroupHeaderTemplate` `GroupFooterTemplate` プロパティとプロパティをオブジェクトに設定することによってカスタマイズでき [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 詳細については、「 [ Xamarin.Forms CollectionView Grouping](grouping.md)」を参照してください。 |
| 引っ張って更新 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Pull to refresh 機能は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) の子としてを設定することによってサポートされ `RefreshView` ます。 詳細については、「 [Pull to refresh](populate-data.md#pull-to-refresh)」を参照してください。 |
| コンテキスト メニュー項目 | `ContextActions` | コンテキストメニュー項目をサポートするには、 `SwipeView` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 内の各データ項目の外観を定義するのルートビューとしてを設定し [`CollectionView`](xref:Xamarin.Forms.CollectionView) ます。 詳細については、「[コンテキストメニュー](populate-data.md#context-menus)」を参照してください。 |
| スクロール | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)`ScrollTo`項目をビューにスクロールするメソッドを定義します。 詳細については、「[スクロール](scrolling.md)」を参照してください。 |

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
