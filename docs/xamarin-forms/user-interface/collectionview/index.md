---
title: Xamarin.Forms CollectionView
description: CollectionView は、別のレイアウトの仕様を使用してデータのリストを表示するための柔軟で高パフォーマンスのビューです。
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
---

# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![[プレビュー]](~/media/shared/preview.png)

> [!IMPORTANT]
> `CollectionView`は現在プレビュー段階で、その計画的な機能の一部が不足しています。 さらに、実装が完了すると、API を変更することがあります。

`CollectionView` 別のレイアウトの仕様を使用してデータのリストを表示するためのビュー。 目的より柔軟に提供してパフォーマンスの高い代替に[ `ListView`](xref:Xamarin.Forms.ListView)します。 中に、`CollectionView`と`ListView`Api は似ています、注目すべきいくつか違いがあります。

- `CollectionView` 柔軟なレイアウト モデル、データをリストまたはグリッドの垂直方向または水平方向に表示することが可能があります。
- `CollectionView` セルの概念はありません。 代わりに、データ テンプレートを使用して、一覧のデータの各アイテムの外観を定義します。
- `CollectionView` 基になるネイティブ コントロールによって提供される仮想化を自動的に利用します。
- `CollectionView` API サーフェスを減少[ `ListView`](xref:Xamarin.Forms.ListView)します。 多くのプロパティおよびイベントから`ListView`が存在しない`CollectionView`します。
- `CollectionView` 組み込みの区切り記号は含まれません。

`CollectionView` Xamarin.Forms 4.0 より前のリリースで使用できます。 ただし、現在、試験段階は、し、次のコードの行を追加することでのみ使用できます、`AppDelegate`クラスでは、iOS、または、 `MainActivity` android では、クラスを呼び出す前に`Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` iOS と Android ではできるだけです。

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[データ CollectionView を設定します。](populate-data.md)

A`CollectionView`設定によってデータが読み込まれて、`ItemsSource`プロパティを実装するコレクションを`IEnumerable`します。 設定して、リストの各項目の外観を定義することができます、`ItemTemplate`プロパティを[ `DataTemplate`](xref:Xamarin.Forms.DataTemplate)します。

## <a name="specify-collectionview-layoutlayoutmd"></a>[CollectionView レイアウトを指定します](layout.md)

既定で、`CollectionView`を垂直方向に一覧の項目が表示されます。 ただし、垂直および水平方向のリストとグリッドを指定することができます。

## <a name="set-collectionview-selection-modeselectionmd"></a>[CollectionView 選択モードを設定します。](selection.md)

既定では、`CollectionView`選択が無効になっています。 ただし、1 つまたは複数選択を有効にすることができます。

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[データが利用できない場合に、EmptyView を表示します。](emptyview.md)

`CollectionView`、表示できるデータがない場合、ユーザーにフィードバックを提供する空のビューを指定することができます。 空のビューには、文字列、ビュー、または複数のビューができます。

## <a name="scroll-an-item-into-viewscrollingmd"></a>[項目をビューにスクロールします。](scrolling.md)

ユーザー カードのスクロールを開始するには、ときに項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、 `CollectionView` 2 つ定義する`ScrollTo`メソッドは、プログラムで項目をビューにスクロールします。 ビューに指定したインデックス位置にある項目を指定した項目をスクロール、表示中に、オーバー ロードの 1 つスクロールします。
