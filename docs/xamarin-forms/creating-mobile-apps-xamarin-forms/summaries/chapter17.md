---
title: '第 17 章の概要: グリッドのマスター'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 17 章の概要: グリッドのマスター'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 37b5e2bbafa816de27390771ae6daa33c74f7651
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "70760627"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>第 17 章の概要: グリッドのマスター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

[`Grid`](xref:Xamarin.Forms.Grid) は、その子をセルの行と列に配置する優れたレイアウト メカニズムです。 よく似た HTML の `table` 要素とは異なり、`Grid` はプレゼンテーションではなく、レイアウトのみを目的としています。

## <a name="the-basic-grid"></a>基本のグリッド

`Grid` は、`Grid` によって継承される [`Children`](xref:Xamarin.Forms.Layout`1.Children) プロパティを定義している [`Layout<View>`](xref:Xamarin.Forms.Layout`1) から派生しています。 XAML またはコードで、このコレクションに入力できます。

### <a name="the-grid-in-xaml"></a>XAML でのグリッド

XAML での `Grid` 定義は一般に、[`RowDefinition`](xref:Xamarin.Forms.RowDefinition) および [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) オブジェクトを使って、`Grid` の [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) および [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) コレクションに入力することから始まります。 この方法によって、`Grid` の行および列の数と、それらのプロパティを設定します。

`RowDefinition` には [`Height`](xref:Xamarin.Forms.RowDefinition.Height) プロパティがあり、`ColumnDefinition` には [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) プロパティがあります。どちらも、[`GridLength`](xref:Xamarin.Forms.GridLength) 型の構造体です。

XAML では、[`GridLengthTypeConverter`](xref:Xamarin.Forms.GridLengthTypeConverter) によって単純なテキスト文字列が `GridLength` 値に変換されます。 バックグラウンドでは、[`GridLength` コンストラクター](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType))によって、数値と 3 つのメンバーを持つ列挙体である [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) 型の値に基づいて、`GridLength` 値が作成されます。

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; 幅または高さは、デバイスに依存しない単位で指定されます (XAML では数値)
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; 高さまたは幅は、セルのコンテンツに基づいて自動調整されます (XAML では "Auto")
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; 残りの高さと幅が比例的に割り当てられます (XAML では、"*スター*" と呼ばれる "\*" 付きの数値)

また、`Grid` の各子には、(明示的または暗黙的に) 行と列が割り当てられる必要があります。 行の範囲と列の範囲は、省略可能です。 これらはすべて、アタッチされたバインド可能なプロパティ &mdash;`Grid` によって定義されているが `Grid` の子に対して設定されているプロパティを使用して、指定されます。 `Grid` では、次の 4 つのアタッチされたバインド可能な静的プロパティを定義しています。

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; 0 から始まる行。既定値は 0 です
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; 0 から始まる列。既定値は 0 です
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; 子の範囲とされる行数。既定値は 1 です
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; 子の範囲とされる列数。既定値は 1 です

コードでは、プログラムによって 8 つの静的メソッドを使用して、これらの値を設定および取得できます。

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

XAML では、これらの値を設定するために、次の属性を使用します。

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[**SimpleGridDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) サンプルでは、XAML での `Grid` の作成と初期化を示します。

`Grid` では、`Layout` から [`Padding`](xref:Xamarin.Forms.Layout.Padding) プロパティを継承し、行と列の間の間隔を指定する 2 つの追加のプロパティを定義しています。

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) の既定値は 6 です
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) の既定値は 6 です

`RowDefinitions` および `ColumnDefinitions` コレクションは、厳密には必須ではありません。 省略すると、`Grid` によって `Grid` の子の行と列が作成され、それらすべてに既定の "\*" (スター) の `GridLength` が付与されます。

### <a name="the-grid-in-code"></a>コードでのグリッド

[**GridCodeDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) サンプルでは、コード上で `Grid` を作成して入力する方法を示しています。 [Grid.IGridList<T>](xref:Xamarin.Forms.Grid.IGridList`1) インターフェイスによって定義された [`Add`](xref:Xamarin.Forms.Grid.IGridList`1.Add*) などの追加の `Add` メソッドを呼び出すことで、各子にアタッチされたプロパティを直接または間接的に設定できます。

### <a name="the-grid-bar-chart"></a>グリッドの棒グラフ

[**GridBarChart**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) サンプルは、bulk [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) メソッドを使用して、複数の `BoxView` 要素を `Grid` に追加する方法を示しています。 既定では、これらの `BoxView` 要素の幅は同じです。 各 `BoxView` の高さは、制御して棒グラフのようにすることができます。

**GridBarChart** サンプルにある `Grid` では、`AbsoluteLayout` の親を初期には表示されない `Frame` と共有しています。 また、プログラムでは、`Frame` を使用してタップされた棒に関する情報を表示するために、各 `BoxView` に `TapGestureRecognizer` も設定しています。

### <a name="alignment-in-the-grid"></a>グリッドでの配置

[**GridAlignment**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) サンプルでは、`VerticalOptions` および `HorizontalOptions` プロパティを使用して `Grid` セル内の子を配置する方法を示しています。

[**SpacingButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) サンプルでは、`Grid` セル内で `Button` 要素を均等な間隔で中央に配置しています。

### <a name="cell-dividers-and-borders"></a>セルの区切り線と罫線

`Grid` には、セルの区切り線または罫線を描画する機能は含まれていません。 ただし、独自に作成することができます。

[**GridCellDividers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) では、細い `BoxView` 要素に対して具体的に追加の行と列を定義して、区切り線のように見立てる方法を示しています。

[**GridCellBorders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) プログラムでは、追加のセルは作成しませんが、代わりに各セル内に `BoxView` 要素を配置してセルの境界線のように見立てています。

## <a name="almost-real-life-grid-examples"></a>実際に近いグリッドの例

[**KeypadGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) サンプルでは `Grid` を使用して、以下のようにテンキーを表示します。

[![Keypad Grid のトリプル スクリーンショット](images/ch17fg12-small.png "テンキーのグリッド")](images/ch17fg12-large.png#lightbox "テンキーのグリッド")

### <a name="responding-to-orientation-changes"></a>向きの変化への対応

`Grid` では、向きの変化に対応するためのプログラムを構成できます。 [**GridRgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) サンプルでは、縦向きのスマートフォンの 2 番目の行と横向きのスマートフォンの 2 番目の列間で要素を移動する技法を示しています。

プログラムでは、`Slider` 要素を 0 から 255 の範囲に初期化し、データ バインディングを使用して 16 進数でスライダーの値を表示します。 `Slider` 値は浮動小数点型であり、16 進数用の .NET 書式設定の文字列は整数のみで有効なため、[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリにある [`DoubleToIntConvert`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) クラスが利用されます。

## <a name="related-links"></a>関連リンク

- [第 17 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [第 17 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [グリッド](~/xamarin-forms/user-interface/layouts/grid.md)
