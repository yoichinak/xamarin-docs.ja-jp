---
title: CollectionView の概要
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 89afb0f2bfe93a5f78b0cd162f2a65e585b54b4b
ms.sourcegitcommit: 43423d4018cc0d4b0b8c98a4b3da0704495eb0cf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72303232"
---
# <a name="xamarinforms-collectionview-introduction"></a>CollectionView の概要

![この API は現在プレリリースされています](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) は、さまざまなレイアウト仕様を使用してデータを一覧表示するためのビューです。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 たとえば、次のスクリーンショットは、2列の垂直グリッドを使用し、複数の選択が可能な @no__t 0 を示しています。

[複数選択のある iOS および Android(introduction-images/verticalgrid-multipleselection.png "CollectionView の垂直グリッドレイアウト")![の CollectionView 垂直グリッドレイアウトのスクリーンショット]](introduction-images/verticalgrid-multipleselection-large.png#lightbox "複数選択の CollectionView 垂直グリッドレイアウト")

[`CollectionView` は、](xref:Xamarin.Forms.CollectionView) Xamarin. Forms 4.0 で使用できます。 ただし、現在は実験的であり、次のコード行を iOS 上の `AppDelegate` クラスに追加するか、Android の `MainActivity` クラスに追加するか、または `Forms.Init` を呼び出す前に UWP の `App.xaml.cs` に追加することによってのみ使用できます。

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は IOS と Android で使用できますが、ユニバーサル Windows プラットフォームでのみ[使用でき](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14)ます。

## <a name="collectionview-and-listview-differences"></a>CollectionView と ListView の相違点

[@No__t-1](xref:Xamarin.Forms.CollectionView) api と[@no__t 3](xref:Xamarin.Forms.ListView) api は似ていますが、いくつかの重要な違いがあります。

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には柔軟なレイアウトモデルが用意されています。これにより、データを垂直方向または水平方向にリストまたはグリッドに表示できます。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)では、単一および複数の選択をサポートしています。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)にはセルの概念がありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に利用します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)にすると、 [`ListView`](xref:Xamarin.Forms.ListView)の API サーフェイスが減少します。 [@No__t-1](xref:Xamarin.Forms.ListView)の多くのプロパティとイベントは、`CollectionView` には存在しません。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。

## <a name="move-from-listview-to-collectionview"></a>ListView から CollectionView への移動

既存の Xamarin. フォーム実装での[@no__t](xref:Xamarin.Forms.ListView)の実装は、次の表に示すように、 [@no__t 3](xref:Xamarin.Forms.CollectionView)の実装に移行できます。

| 概念 | ListView API | CollectionView |
|---|---|---|
| data | `ItemsSource` | @No__t の2つのプロパティを設定することにより、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)にデータが入力されます。 詳細については、「データを使用した[CollectionView の設定](populate-data.md#populate-a-collectionview-with-data)」を参照してください。 |
| 項目の外観 | `ItemTemplate` | [@No__t-1](xref:Xamarin.Forms.CollectionView)の各項目の外観は、`ItemTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)に設定することによって定義できます。 詳細については、「[アイテムの外観を定義](populate-data.md#define-item-appearance)する」を参照してください。 |
| セル | `TextCell`、 `ImageCell`、 `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)にはセルの概念がありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。 |
| 行区切り記号 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。 これらは、必要に応じて項目テンプレートに指定できます。 |
| 選択 | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)では、単一および複数の選択をサポートしています。 詳細については、「 [CollectionView Selection](selection.md)」を参照してください。 |
| 行の高さ | `HasUnevenRows`, `RowHeight` | @No__t 0 の場合、各項目の行の高さは、`ItemSizingStrategy` プロパティによって決定されます。 詳細については、「[項目のサイズ](layout.md#item-sizing)設定」を参照してください。|
| キャッシュ | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に使用します。 |
| ヘッダーとフッター | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、`Header`、`Footer`、`HeaderTemplate`、および @no__t 5 の各プロパティを使用して、リスト内の項目をスクロールするヘッダーとフッターを提供できます。 詳細については、「[ヘッダーとフッター](layout.md#headers-and-footers)」を参照してください。 |
| グループ化 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、`IsGrouped` プロパティを `true` に設定して、正しくグループ化されたデータを表示します。 グループヘッダーとグループフッターをカスタマイズするには、`GroupHeaderTemplate` および `GroupFooterTemplate` プロパティを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトに設定します。 詳細については、「 [CollectionView Grouping](grouping.md)」を参照してください。 |
| プルして更新 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | プルへのプルは現在 `CollectionView` ではサポートされていませんが、今後のリリースで追加される予定です。 |
| コンテキスト アクション | `ContextActions` | 現在、コンテキストアクションは @no__t 0 ではサポートされていませんが、今後のリリースで追加される予定です。 |
| スクロール | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、項目をビューにスクロールする、@no__t の2つのメソッドを定義します。 詳細については、「[スクロール](scrolling.md)」を参照してください。 |

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
