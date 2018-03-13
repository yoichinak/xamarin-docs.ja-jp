---
title: "17 章の概要です。 マスター グリッド"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09f63dd418ea1fb523c028edb02c28c22bfdccd1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>17 章の概要です。 マスター グリッド

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)セルの行と列にその子を整列する強力なレイアウト メカニズムです。 ような HTML とは異なり`table`要素、`Grid`プレゼンテーションではなく、レイアウトの目的でのみです。

## <a name="the-basic-grid"></a>基本のグリッド

`Grid` 派生した[ `Layout<View>`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)を定義する、 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/)プロパティを`Grid`を継承します。 このコレクションで、XAML またはコードを入力できます。

### <a name="the-grid-in-xaml"></a>XAML のグリッド

定義、 `Grid` XAML で一般に始まりますいっぱいになる、 [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/)と[ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/)のコレクション、`Grid`で[ `RowDefinition`。](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/)と[ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)オブジェクト。 これは、行の数との列を確立する方法、 `Grid`、およびそれらのプロパティです。

`RowDefinition` [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/)プロパティおよび`ColumnDefinition`が、 [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/)プロパティの型の両方の[ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/)、構造体。

XAML では、 [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/)に単純なテキスト文字列に変換する`GridLength`値。 背後では、 [ `GridLength`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/)を作成、`GridLength`値が数値と型の値に基づいて[ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/)、3 つのメンバーを持つ列挙します。

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) & #x 2014 です。幅または高さがデバイスに依存しない単位 (XAML の番号) で指定されました。
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) & #x 2014 です。高さまたは幅は、セルの内容 (XAML での"Auto") に基づいて自動的に設定
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) & #x 2014 です。残された高さまたは幅が比例的に割り当てられます (数値に"\*"と呼ばれる、*スター*XAML で)

それぞれの子の`Grid`必要がありますも指定した行と列 (明示的または暗黙的に)。 行にまたがるし、列の範囲は省略可能です。 これらはすべて指定を使用すると、バインド可能なプロパティ & #x 2014; にアタッチプロパティによって定義されている、`Grid`の子で、設定、`Grid`です。 `Grid` 次の 4 つの静的な接続されているバインド可能なプロパティを定義します。

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) & #x 2014 です。0 から始まる行です。既定値は 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) & #x 2014 です。0 から始まる列です。既定値は 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) & #x 2014 です。数の行の子にまたがるです。既定値は 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) & #x 2014 です。数値列の子にまたがるです。既定値は 1

コードでは、プログラムは、設定およびこれらの値を取得する 8 つの静的メソッドを使用できます。

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) そして [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) そして [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) そして [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) そして [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

XAML では、これらの値を設定するため、次の属性を使用します。

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo)サンプルの作成と初期化、 `Grid` XAML でします。

`Grid`継承、 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)プロパティから`Layout`し、行および列間のスペースを提供する 2 つの追加プロパティを定義します。

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) 6 の既定値を持つ
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) 6 の既定値を持つ

`RowDefinitions`と`ColumnDefinitions`コレクションは必須です。 指定しない場合、`Grid`の行と列を作成、`Grid`子はすべて、既定値と`GridLength`の"\*"(アスタリスク) です。

### <a name="the-grid-in-code"></a>コード内のグリッド

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo)サンプルを作成し、設定する方法を示します、`Grid`のコードにします。 添付プロパティを設定するには、それぞれの子、直接または間接的を呼び出して追加`Add`などのメソッド[ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/)によって定義された、 [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/)インターフェイスです。

### <a name="the-grid-bar-chart"></a>グリッドの棒グラフ

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart)サンプルは、複数を追加する方法を示します`BoxView`要素を`Grid`一括[ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/)メソッドです。 既定では、これら`BoxView`要素を持つ幅を等しくします。 それぞれの高さ`BoxView`を横棒グラフのように制御できます。

`Grid`で、 **GridBarChart**サンプルの共有、`AbsoluteLayout`最初に非表示に親`Frame`です。 また、プログラム、設定、`TapGestureRecognizer`各`BoxView`を使用する、`Frame`タップされたバーの情報を表示します。

### <a name="alignment-in-the-grid"></a>グリッド内の配置

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment)サンプルを使用する方法を示します、`VerticalOptions`と`HorizontalOptions`プロパティ内の子に合わせて、`Grid`セル。

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons)スペースを均等にサンプリング`Button`要素中心に`Grid`セル。

### <a name="cell-dividers-and-borders"></a>セルの分割線と境界線

`Grid`セルの区分線または罫線を描画する機能はありません。 ただし、行うことができます独自です。

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers)シン追加の行と専用の列を定義する方法を示します`BoxView`区切り線を模倣する要素。

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders)プログラムの追加のセルでは作成されませんが、代わりに揃えて配置`BoxView`セルの枠線を模倣するためには、各セル内の要素。

## <a name="almost-real-life-grid-examples"></a>ほとんどの現実のグリッドの例

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid)サンプルは、`Grid`キーパッドを表示します。

[![キーパッド グリッドのトリプル スクリーン ショット](images/ch17fg12-small.png "キーパッド グリッド")](images/ch17fg12-large.png#lightbox "キーパッド グリッド")

### <a name="responding-to-orientation-changes"></a>印刷の向きの変更に対応

`Grid`向きの変更に応答するプログラムを構成できます。 [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders)サンプルでは、縦向きでスマート フォンの 2 番目の行と横向きのスマート フォンの 2 番目の列の間で要素を移動する技法。

プログラムを初期化します`Slider`要素には、0 から 255 までおよび 16 進数で、スライダーの値を表示するデータ バインドを使用します。 `Slider`ポイント、および .NET の 16 進数のみ動作するかを整数と文字列の書式設定に値が浮動小数点、 [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリに役立ちます。



## <a name="related-links"></a>関連リンク

- [Chapter 17 フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Chapter 17 サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [グリッド](~/xamarin-forms/user-interface/layouts/grid.md)
