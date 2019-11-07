---
title: CollectionView の概要
description: CollectionView は、さまざまなレイアウト仕様を使用してデータの一覧を表示するための、柔軟でパフォーマンスの高いビューです。
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: 871d7cad6c57cd34757ae992ce14d5f686935584
ms.sourcegitcommit: 283810340de5310f63ef7c3e4b266fe9dc2ffcaf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73662307"
---
# <a name="xamarinforms-collectionview-introduction"></a>CollectionView の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) は、さまざまなレイアウト仕様を使用してデータを一覧表示するためのビューです。 これは、 [`ListView`](xref:Xamarin.Forms.ListView)に代わる、より柔軟性が高く、パフォーマンスに優れた方法を提供することを目的としています。 たとえば、次のスクリーンショットは、2列の垂直グリッドを使用し、複数の選択を可能にする `CollectionView` を示しています。

[![IOS と Android の CollectionView 垂直グリッドレイアウトのスクリーンショット](introduction-images/verticalgrid-multipleselection.png "複数選択の CollectionView 垂直グリッドレイアウト")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "複数選択の CollectionView 垂直グリッドレイアウト")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、Xamarin. Forms 4.3 から使用できます。

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は IOS と Android で使用できますが、ユニバーサル Windows プラットフォームでのみ[使用でき](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14)ます。

## <a name="collectionview-and-listview-differences"></a>CollectionView と ListView の相違点

[`CollectionView`](xref:Xamarin.Forms.CollectionView) api と[`ListView`](xref:Xamarin.Forms.ListView) api は似ていますが、いくつかの重要な違いがあります。

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、データを垂直方向または水平方向にリストまたはグリッドで表示できる柔軟なレイアウトモデルがあります。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、単一選択と複数選択をサポートします。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、セルの概念がありません。 代わりに、データテンプレートを使用して、リスト内の各データ項目の外観を定義します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に利用します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)により、 [`ListView`](xref:Xamarin.Forms.ListView)の API サーフェイスが減少します。 [`ListView`](xref:Xamarin.Forms.ListView)からの多くのプロパティとイベントは、`CollectionView`には存在しません。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。
- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)が UI スレッドから更新された場合、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)は例外をスローします。

## <a name="move-from-listview-to-collectionview"></a>ListView から CollectionView への移動

既存の Xamarin. フォーム実装の[`ListView`](xref:Xamarin.Forms.ListView)実装は、次の表に示すように、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)の実装に移行できます。

| 概念 | ListView API | CollectionView |
|---|---|---|
| データ | `ItemsSource` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、`ItemsSource` プロパティを設定することによってデータが設定されます。 詳細については、「データを使用した[CollectionView の設定](populate-data.md#populate-a-collectionview-with-data)」を参照してください。 |
| 項目の外観 | `ItemTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)内の各項目の外観は、`ItemTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義できます。 詳細については、「[アイテムの外観を定義](populate-data.md#define-item-appearance)する」を参照してください。 |
| セル | `TextCell`では、 `ImageCell`では、 `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、セルの概念がありません。 代わりに、データテンプレートを使用して、リスト内の各データ項目の外観を定義します。 |
| 行区切り記号 | `SeparatorColor`、 `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。 これらは、必要に応じて項目テンプレートに指定できます。 |
| 選択ツール | `SelectionMode`、 `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、単一選択と複数選択をサポートします。 詳細については、「 [CollectionView Selection](selection.md)」を参照してください。 |
| 行の高さ | `HasUnevenRows`、 `RowHeight` | `CollectionView`では、各項目の行の高さは、`ItemSizingStrategy` プロパティによって決定されます。 詳細については、「[項目のサイズ](layout.md#item-sizing)設定」を参照してください。|
| キャッシュ | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に使用します。 |
| ヘッダーとフッター | `Header`、 `HeaderElement`、 `HeaderTemplate`、 `Footer`、 `FooterElement`、 `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、`Header`、`Footer`、`HeaderTemplate`、および `FooterTemplate` の各プロパティを使用して、リスト内の項目と共にスクロールするヘッダーとフッターを表示できます。 詳細については、「[ヘッダーとフッター](layout.md#headers-and-footers)」を参照してください。 |
| グループ化 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) `IsGrouped` プロパティを `true` に設定して、正しくグループ化されたデータを表示します。 グループヘッダーとグループフッターをカスタマイズするには、`GroupHeaderTemplate` プロパティと `GroupFooterTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定します。 詳細については、「 [CollectionView Grouping](grouping.md)」を参照してください。 |
| プルして更新 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Pull to refresh 機能は、`RefreshView` の子として[`CollectionView`](xref:Xamarin.Forms.CollectionView)を設定することによってサポートされています。 詳細については、「 [Pull to refresh](populate-data.md#pull-to-refresh)」を参照してください。 |
| コンテキスト アクション | `ContextActions` | コンテキストアクションは現在 `CollectionView` ではサポートされていませんが、今後のリリースで追加される予定です。 |
| スクロール | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、項目をビューにスクロールする `ScrollTo` メソッドを定義します。 詳細については、「[スクロール](scrolling.md)」を参照してください。 |

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
