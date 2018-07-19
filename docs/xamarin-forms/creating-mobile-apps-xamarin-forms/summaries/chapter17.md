---
title: 第 17 章の概要です。 マスター グリッド
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 17 章の概要。 マスター グリッド'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5f5b934b5f828bf6f5e8d4a0f0738c7db633aefb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995841"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>第 17 章の概要です。 マスター グリッド

[ `Grid` ](xref:Xamarin.Forms.Grid)は行とセルの列にその子を配置するレイアウトの強力なメカニズムです。 ような HTML とは異なり`table`要素、`Grid`プレゼンテーションではなく、レイアウトの目的でのみです。

## <a name="the-basic-grid"></a>基本的なグリッド

`Grid` 派生[ `Layout<View>` ](xref:Xamarin.Forms.Layout`1)、定義する、 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children)プロパティを`Grid`を継承します。 XAML またはコードのいずれかで、このコレクションを入力することができます。

### <a name="the-grid-in-xaml"></a>XAML でグリッド

定義、 `Grid` XAML で一般的には、開始いっぱいになると、 [ `RowDefinitions` ](xref:Xamarin.Forms.Grid.RowDefinitions)と[ `ColumnDefinitions` ](xref:Xamarin.Forms.Grid.ColumnDefinitions)のコレクション、`Grid`で[ `RowDefinition`](xref:Xamarin.Forms.RowDefinition)と[ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)オブジェクト。 これは、行の数との列を確立する方法、 `Grid`、およびそれらのプロパティ。

`RowDefinition` [ `Height` ](xref:Xamarin.Forms.RowDefinition.Height)プロパティと`ColumnDefinition`が、 [ `Width` ](xref:Xamarin.Forms.ColumnDefinition.Width)プロパティは、両方の種類の[ `GridLength` ](xref:Xamarin.Forms.GridLength)、構造体。

XAML、 [ `GridLengthTypeConverter` ](xref:Xamarin.Forms.GridLengthTypeConverter)に単純なテキスト文字列に変換する`GridLength`値。 バック グラウンドで、 [ `GridLength`コンス トラクター](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType))を作成、`GridLength`値が、数と型の値に基づく[ `GridUnitType` ](xref:Xamarin.Forms.GridUnitType)、3 つのメンバーを持つ列挙体。

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; 幅または高さがデバイスに依存しない単位 (XAML の番号) で指定されます。
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; 高さまたは幅がセルの内容 (XAML では"Auto") に基づく自動調整です。
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; 残っている高さまたは幅がそれに比例して割り当てられます (数値で"\*"と呼ばれる、*スター*XAML で)

それぞれの子の`Grid`する必要がありますも割り当てる行と列 (明示的または暗黙的に)。 行にまたがるし、列の範囲は省略可能です。 これらはすべて指定添付のバインド可能なプロパティを使用して&mdash;プロパティで定義されている、`Grid`の子の設定が、`Grid`します。 `Grid` 接続されているバインド可能な 4 つの静的プロパティを定義します。

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; 0 から始まる行です。既定値は 0
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; 0 から始まる列です。既定値は 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; 数の行の子にまたがる;既定値は 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; 数値列の子にまたがる;既定値は 1

コードでは、プログラムは、設定およびこれらの値を取得する 8 つの静的メソッドを使用できます。

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) および [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

XAML では、これらの値を設定するため、次の属性を使用します。

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo)サンプルの作成と初期化、 `Grid` XAML でします。

`Grid`継承、 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)プロパティから`Layout`行と列の間隔を提供する 2 つのプロパティを定義します。

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) 6 の既定値を持つ
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) 6 の既定値を持つ

`RowDefinitions`と`ColumnDefinitions`コレクションは厳密に必要ありません。 指定しない場合、`Grid`の行および列を作成、`Grid`子と、既定値は、すべて`GridLength`の"\*"(アスタリスク)。

### <a name="the-grid-in-code"></a>コードのグリッド

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo)サンプルを作成し、`Grid`コード。 添付プロパティを設定するには、それぞれの子、直接または間接的に呼び出して追加`Add`などのメソッド[ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/)によって定義された、 [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/)インターフェイスです。

### <a name="the-grid-bar-chart"></a>グリッドの横棒グラフ

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart)サンプルは、複数追加する方法を示しています。`BoxView`要素を、`Grid`一括[ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/)メソッド。 既定では、これら`BoxView`要素がある等幅。 高さ`BoxView`を横棒グラフのように制御できます。

`Grid`で、 **GridBarChart**サンプル共有、`AbsoluteLayout`最初に非表示に親`Frame`します。 また、プログラム、設定、`TapGestureRecognizer`各`BoxView`を使用する、`Frame`タップされたバーに関する情報を表示します。

### <a name="alignment-in-the-grid"></a>グリッド内の配置

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment)サンプルを使用する方法を示して、`VerticalOptions`と`HorizontalOptions`プロパティ内の子に合わせて、`Grid`セル。

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons)スペースを均等にサンプル`Button`要素中心に`Grid`セル。

### <a name="cell-dividers-and-borders"></a>セルの分割線と境界線

`Grid`セルの区分線または境界線を描画する機能は含まれません。 ただし、行うことができます独自です。

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers)シン追加の行と専用の列を定義する方法を示します`BoxView`区切り線を模倣する要素。

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders)プログラムは、他のセルは作成されませんが、代わりに揃えて配置`BoxView`セルの境界線を模倣するためには、各セル内の要素。

## <a name="almost-real-life-grid-examples"></a>ほとんどの現実 Grid の例

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid)使用して、`Grid`キーパッドを表示します。

[![キーパッド グリッドの 3 倍になるスクリーン ショット](images/ch17fg12-small.png "キーパッド グリッド")](images/ch17fg12-large.png#lightbox "キーパッド グリッド")

### <a name="responding-to-orientation-changes"></a>印刷の向きの変更に応答します。

`Grid`向きの変更に応答するプログラムを構成できます。 [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders)縦向きでスマート フォンの 2 つ目の行と横向きのスマート フォンの 2 番目の列の間の要素を移動する方法を示します。

プログラムを初期化します`Slider`範囲 0 ~ 255 の 16 進数で、スライダーの値を表示するデータ バインドを使用する要素。 `Slider`ポイント、および .NET の 16 進数の整数でのみ動作の文字列の書式設定に値が浮動小数点、 [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs)クラス、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリに役立ちます。



## <a name="related-links"></a>関連リンク

- [第 17 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [第 17 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [グリッド](~/xamarin-forms/user-interface/layouts/grid.md)
