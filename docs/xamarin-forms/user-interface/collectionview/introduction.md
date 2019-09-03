---
title: CollectionView の概要
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 14abf2e7eff64d2e3e9656bf1ca76f4cee615408
ms.sourcegitcommit: 5ef92b44f0d10c58013d3c3dd6283509f1499587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/23/2019
ms.locfileid: "69986078"
---
# <a name="xamarinforms-collectionview-introduction"></a>CollectionView の概要

![この API は現在プレリリースされています](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) は、さまざまなレイアウト仕様を使用してデータを一覧表示するためのビューです。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 たとえば、次のスクリーンショットは、 `CollectionView` 2 つの列の垂直グリッドを使用し、複数の選択を可能にするを示しています。

[![iOS および Android における垂直グリッドレイアウトの CollectionView のスクリーンショット](introduction-images/verticalgrid-multipleselection.png " 複数選択のある垂直グリッドレイアウトの CollectionView")](introduction-images/verticalgrid-multipleselection-large.png#lightbox " 複数選択のある垂直グリッドレイアウトの CollectionView")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、Xamarin. Forms 4.0 で使用できます。 ただし、現在試験段階で、`Forms.Init` を呼ぶ前に、Android では `MainActivity` クラス、iOS では `AppDelegate` クラスに以下の1行を加えることによってのみ使用できます:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は iOS と Android で使用できますが、ユニバーサル Windows プラットフォームでのみ[一部利用でき](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14)ます。

## <a name="collectionview-and-listview-differences"></a>CollectionView と ListView の相違点

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Api と[`ListView`](xref:Xamarin.Forms.ListView) api は似ていますが、いくつかの重要な違いがあります。

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、データを垂直方向または水平方向にリストまたはグリッドで表示できる柔軟なレイアウトモデルが用意されています。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)単一および複数の選択をサポートします。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)セルの概念はありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に利用します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)の[`ListView`](xref:Xamarin.Forms.ListView)API サーフェイスを減らします。 のプロパティとイベント[`ListView`](xref:Xamarin.Forms.ListView)の多くは、に`CollectionView`は存在しません。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。

## <a name="move-from-listview-to-collectionview"></a>ListView から CollectionView への移動

[`ListView`](xref:Xamarin.Forms.ListView)既存の Xamarin. フォーム実装の実装は、次[`CollectionView`](xref:Xamarin.Forms.CollectionView)の表を参考にして実装に移行できます。

| 概念 | ListView API | CollectionView |
|---|---|---|
| データ | `ItemsSource` | に[`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 `ItemsSource`プロパティを設定することによってデータが設定されます。 詳細については、「データを使用した[CollectionView の設定](populate-data.md#populate-a-collectionview-with-data)」を参照してください。 |
| 項目の外観 | `ItemTemplate` | の各項目[`CollectionView`](xref:Xamarin.Forms.CollectionView)の外観は、 `ItemTemplate`プロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)をに設定することによって定義できます。 詳細については、「[アイテムの外観を定義](populate-data.md#define-item-appearance)する」を参照してください。 |
| セル | `TextCell`、 `ImageCell`、 `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)セルの概念はありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。 |
| 行区切り記号 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)には、組み込みの区切り記号は含まれません。 これらは、必要に応じて項目テンプレートに指定できます。 |
| 選択ツール | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)単一および複数の選択をサポートします。 詳細については、「 [CollectionView Selection](selection.md)」を参照してください。 |
| 行の高さ | `HasUnevenRows`, `RowHeight` | では`ItemSizingStrategy` 、各項目の行の高さはプロパティによって決定されます。 `CollectionView` 詳細については、「[項目のサイズ](layout.md#item-sizing)設定」を参照してください。|
| キャッシュ | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、基になるネイティブコントロールによって提供される仮想化を自動的に使用します。 |
| ヘッダーとフッター | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、 `Header`、、 `HeaderTemplate`、および`Footer` `FooterTemplate`の各プロパティを使用して、リスト内の項目と共にスクロールするヘッダーとフッターを提供できます。 詳細については、「[ヘッダーとフッター](layout.md#headers-and-footers)」を参照してください。 |
| グループ化 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)`IsGrouped`プロパティをに設定して、適切に`true`グループ化されたデータを表示します。 グループヘッダーとグループフッターは、プロパティ`GroupHeaderTemplate`と`GroupFooterTemplate`プロパティをオブジェクトに[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)設定することによってカスタマイズできます。 詳細については、「 [CollectionView Grouping](grouping.md)」を参照してください。 |
| プルして更新 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | 現在、での`CollectionView`プル更新はサポートされていませんが、今後のリリースで追加される予定です。 |
| コンテキスト アクション | `ContextActions` | コンテキストアクションは現在で`CollectionView`はサポートされていませんが、今後のリリースで追加される予定です。 |
| スクロール | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)項目`ScrollTo`をビューにスクロールするメソッドを定義します。 詳細については、「[スクロール](scrolling.md)」を参照してください。 |

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
