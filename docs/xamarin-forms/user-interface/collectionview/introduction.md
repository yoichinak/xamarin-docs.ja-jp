---
title: Xamarin.Forms CollectionView の概要
description: CollectionView は、異なるレイアウト仕様を使ってデータのリストを表示するための柔軟で高パフォーマンスなビューです。
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 889c78ea6849cdd094d34ed0cf74ceebd33ce51d
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005142"
---
# <a name="xamarinforms-collectionview-introduction"></a>Xamarin.Forms CollectionView の概要

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 別のレイアウトの仕様を使用してデータのリストを表示するためのビュー。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 たとえば、次のスクリーン ショットは、表示、 `CollectionView` 2 つの列の垂直グリッドを使用して、複数選択できます。

[![IOS と Android での CollectionView 垂直グリッド レイアウトのスクリーン ショット](introduction-images/verticalgrid-multipleselection.png "複数選択の垂直グリッド レイアウトの CollectionView")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView 垂直グリッド レイアウト複数の選択")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Xamarin.Forms 4.0 で使用できます。 ただし、現在試験段階で、`Forms.Init` を呼ぶ前に、Android では `MainActivity` クラス、iOS では `AppDelegate` クラスに以下の1行を加えることによってのみ使用できます:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) iOS と Android ではできるだけです。

## <a name="collectionview-and-listview-differences"></a>CollectionView と ListView の相違点

中に、 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)と[ `ListView` ](xref:Xamarin.Forms.ListView) Api は似ています、注目すべきいくつか違いがあります。

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 柔軟なレイアウト モデル、データをリストまたはグリッドの垂直方向または水平方向に表示することが可能があります。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) サポートする単一と複数選択します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) セルの概念はありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 基になるネイティブ コントロールによって提供される仮想化を自動的に利用します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) API サーフェスを減少[ `ListView`](xref:Xamarin.Forms.ListView)します。 多くのプロパティおよびイベントから[ `ListView` ](xref:Xamarin.Forms.ListView)が存在しない`CollectionView`します。
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 組み込みの区切り記号は含まれません。

## <a name="move-from-listview-to-collectionview"></a>CollectionView ListView から移動します。

[`ListView`](xref:Xamarin.Forms.ListView) 既存の Xamarin.Forms の実装での実装に移行できる[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)の次の表に、ヘルプと実装。

| 概念 | ListView API | CollectionView |
|---|---|---|
| データ | `ItemsSource` | A [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)設定によってデータが読み込まれて、`ItemsSource`プロパティ。 詳細については、次を参照してください。[設定データの CollectionView](populate-data.md#populate-a-collectionview-with-data)します。 |
| 項目の外観 | `ItemTemplate` | 内の各項目の外観を[ `CollectionView` ](xref:Xamarin.Forms.CollectionView)を設定して定義することができます、`ItemTemplate`プロパティを[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。 詳細については、次を参照してください。[項目の外観を定義](populate-data.md#define-item-appearance)します。 |
| セル | `TextCell`、 `ImageCell`、 `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) セルの概念はありません。 代わりに、データテンプレートを使用して、リストのデータの各アイテムの外観を定義します。 |
| 行区切り記号 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 組み込みの区切り記号は含まれません。 これらは、項目テンプレートで、必要な場合、提供できます。 |
| 選択ツール | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) サポートする単一と複数選択します。 詳細については、次を参照してください。 [Xamarin.Forms CollectionView 選択](selection.md)します。 |
| 行の高さ | `HasUnevenRows`, `RowHeight` | `CollectionView`、各項目の行の高さが続く、`ItemSizingStrategy`プロパティ。 詳細については、次を参照してください。[項目のサイズ変更](layout.md#item-sizing)します。|
| キャッシュ | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 基になるネイティブ コントロールによって提供される仮想化を自動的に使用します。 |
| ヘッダーとフッター | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | ヘッダーとフッターは現時点でサポートされていない`CollectionView`は将来のリリースで追加します。|
| グループ化 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | グループ化がサポートされていない現在`CollectionView`は将来のリリースで追加します。 |
| 引っ張って更新 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | 引っ張って更新がサポートされていない現在`CollectionView`は将来のリリースで追加します。 |
| コンテキスト アクション | `ContextActions` | コンテキスト アクションがで現在サポートされていない`CollectionView`は将来のリリースで追加します。 |
| スクロール | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 定義`ScrollTo`メソッドは、項目をビューにスクロールします。 詳細については、次を参照してください。[スクロール](scrolling.md)します。 |

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
