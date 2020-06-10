---
title: " Xamarin.Forms CollectionView" description: "CollectionView は、さまざまなレイアウト仕様を使用してデータの一覧を表示するための柔軟で高性能なビューです。"
ms. 製品: xamarin ms. assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 07/24/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introduction"></a>[はじめに](introduction.md)

は、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) さまざまなレイアウト仕様を使用してデータの一覧を表示するための、柔軟でパフォーマンスの高いビューです。

## <a name="data"></a>[データ](populate-data.md)

には、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) プロパティをを実装する任意のコレクションに設定することにより、データが設定され `IEnumerable` ます。 リスト内の各項目の外観は、プロパティをに設定することによって定義でき [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

## <a name="layout"></a>[レイアウト](layout.md)

既定では、の [`CollectionView`](xref:Xamarin.Forms.CollectionView) 項目が縦の一覧に表示されます。 ただし、縦と横のリストおよびグリッドを指定することもできます。

## <a name="selection"></a>[選択](selection.md)

既定で [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、選択は無効になっています。 ただし、1つまたは複数の選択を有効にすることができます。

## <a name="empty-views"></a>[空のビュー](emptyview.md)

で [`CollectionView`](xref:Xamarin.Forms.CollectionView) は、表示可能なデータがない場合にユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューを指定できます。

## <a name="scrolling"></a>[スクロール](scrolling.md)

ユーザーがスクロールを開始しようとしたときに、項目が完全に表示されるように、スクロールの終了位置を制御できます。 さらに、では、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 項目をプログラムによってビューにスクロールする2つのメソッドが定義されています。 オーバーロードの1つは、指定されたインデックス位置にある項目をビューにスクロールし、もう一方は指定された項目をビューにスクロールします。

## <a name="grouping"></a>[分類](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)では、プロパティをに設定することによって、正しくグループ化されたデータを表示でき `IsGrouped` `true` ます。
