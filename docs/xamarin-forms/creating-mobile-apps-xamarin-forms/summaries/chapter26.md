---
title: 第 26 章の概要です。 カスタム レイアウト
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 26 章の概要。 カスタム レイアウト'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 18bfb7088d5dcb1476716e881ea54052e62d2504
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563967"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>第 26 章の概要です。 カスタム レイアウト

Xamarin.Forms から派生したいくつかのクラスが含まれています[ `Layout<View>` ](xref:Xamarin.Forms.Layout`1):。

* `StackLayout`、
* `Grid`、
* `AbsoluteLayout`、および
* `RelativeLayout`。

派生する独自のクラスを作成する方法を説明`Layout<View>`します。

## <a name="an-overview-of-layout"></a>レイアウトの概要

Xamarin.Forms のレイアウトを処理する一元的なシステムではありません。 各要素は、どのような独自のサイズは、および特定の領域内で自身をレンダリングする方法を決定する責任を負います。

### <a name="parents-and-children"></a>親と子

子が存在するすべての要素は、それ自体の中には、その子の位置を指定します。 最終的にその子のサイズを決定する親に基づく必要がありますが、サイズ上で使用できると、子のサイズを考えています。

### <a name="sizing-and-positioning"></a>配置してサイズ変更

レイアウトは、ページとビジュアル ツリーの先頭にあるし、すべての分岐に続きます。 レイアウトの最も重要なパブリック メソッドは[ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))によって定義された`VisualElement`します。 その他の要素の呼び出しを親になっているすべての要素`Layout`サイズと位置自体の形式で、子を提供するには、その子の各、 [ `Rectangle` ](xref:Xamarin.Forms.Rectangle)値。 これら`Layout`ビジュアル ツリーからの呼び出しを伝達します。

呼び出し`Layout`の画面に表示する要素が必要ですし、次の読み取り専用プロパティを設定します。 対応、`Rectangle`メソッドに渡されます。

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) 型の `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) 型の `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) 型の `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) 型の `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) 型の `double`

前のバージョン、`Layout`を呼び出すと、`Height`と`Width`のモックの値を持つ&ndash;1。

呼び出し`Layout`も次の保護されたメソッドの呼び出しをトリガーします。

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double))を呼び出す
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double))をオーバーライドできます。

最後に、次のイベントが発生します。

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`OnSizeAllocated`メソッドによってオーバーライドされます`Page`と`Layout`、xamarin.forms の子を持つことができますのみの 2 つのクラスであります。 オーバーライドされたメソッドの呼び出し

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) `Page`導関数と[ `UpdateChildrenLayout` ](xref:Xamarin.Forms.Layout.UpdateChildrenLayout)の`Layout`実行く先の呼び出し
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) `Page`導関数と[ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))の`Layout`派生クラス。

`LayoutChildren` 呼び出して`Layout`の各要素の子。 少なくとも 1 つの子がある新しい場合`Bounds`設定すると、次のイベントが発生し。

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) `Page`導関数と[ `LayoutChanged` ](xref:Xamarin.Forms.Layout.LayoutChanged)の`Layout`派生物

### <a name="constraints-and-size-requests"></a>制約と要求のサイズ

`LayoutChildren`をインテリジェントに呼び出す`Layout`そのすべての子で知る必要があります、*優先*または*目的*子のサイズ。 そのために呼び出し`Layout`のそれぞれの子がへの呼び出しによって前通常

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

この書籍が発行された後、`GetSizeRequest`メソッドが非推奨し、置き換えられます

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure`メソッドに対応、 [ `Margin` ](xref:Xamarin.Forms.View.Margin)プロパティ型の引数が含まれています[ `MeasureFlag` ](xref:Xamarin.Forms.MeasureFlags)、2 つのメンバーを持ちます。

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) 余白を含まない

多くの要素に関する`GetSizeRequest`または`Measure`そのレンダラーからネイティブの要素のサイズを取得します。 どちらの方法は、幅と高さのパラメーターを持つ*制約*します。 たとえば、`Label`幅制約を使用して、複数行のテキストをラップする方法が特定されます。

両方`GetSizeRequest`と`Measure`型の値を返す[ `SizeRequest`](xref:Xamarin.Forms.SizeRequest)を持つ 2 つのプロパティ。

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) 型の `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) 型の `Size`

非常に多くの場合、これら 2 つの値が同じで、`Minimum`通常、値は無視できます。

`VisualElement` ような保護対象のメソッドも定義`GetSizeRequest`から呼び出される`GetSizeRequest`:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) 返します、`SizeRequest`値

そのメソッドに置き換えられます:、非推奨となりました。

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

すべてのクラスから派生した`Layout`または`Layout<T>`オーバーライドする必要があります`OnSizeRequest`または`OnMeasure`します。 これは、レイアウトのクラスがその子には、これを呼び出すことによって取得のサイズに基づく一般的に、独自のサイズを決定`GetSizeRequest`または`Measure`子にします。 呼び出しの前後に`OnSizeRequest`または`OnMeasure`、`GetSizeRequest`または`Measure`次のプロパティに基づく調整。

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)型の`double`、影響を与える、`Request`のプロパティ `SizeRequest`
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 型の`double`、影響を与える、`Request`のプロパティ `SizeRequest`
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) 型の`double`、影響を与える、`Minimum`のプロパティ `SizeRequest`
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) 型の`double`、影響を与える、`Minimum`のプロパティ `SizeRequest`

### <a name="infinite-constraints"></a>無限の制約

渡される制約引数`GetSizeRequest`(または`Measure`) と`OnSizeRequest`(または`OnMeasure`) 有限ことができます (の値、つまり`Double.PositiveInfinity`)。 ただし、`SizeRequest`これらから返されるメソッドは、無限のディメンションを含めることはできません。

無限の制約は、要求されたサイズは、要素の自然なサイズを反映させることを示します。 垂直`StackLayout`呼び出し`GetSizeRequest`(または`Measure`) 無限の高さ制約を使用してその子にします。 水平方向のスタック レイアウトを呼び出す`GetSizeRequest`(または`Measure`) 無限の幅とその子にします。 `AbsoluteLayout`呼び出し`GetSizeRequest`(または`Measure`) 無限の幅と高さの制約とその子にします。

### <a name="peeking-inside-the-process"></a>プロセス内でピークします。

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes)表示制約とサイズは、単純なレイアウトの情報を要求します。

## <a name="deriving-from-layoutview"></a>レイアウトからの派生<View>

カスタム レイアウトのクラスから派生`Layout<View>`します。 2 つの役割があります。

- オーバーライド`OnMeasure`を呼び出す`Measure`レイアウトのすべての子にします。 レイアウトの種類の要求されたサイズを返す
- オーバーライド`LayoutChildren`を呼び出す`Layout`レイアウトのすべての子で

`for`または`foreach`これらのオーバーライドでのループは、すべての子をスキップする必要がありますが`IsVisible`プロパティに設定されて`false`します。

呼び出し`OnMeasure`は保証されません。 `OnMeasure` 呼び出されません、レイアウトの親がレイアウトのサイズ (ページ、レイアウトなど) を制御する場合。 このため、`LayoutChildren`中に取得された子のサイズに依存できない、`OnMeasure`呼び出します。 非常に多くの場合、`LayoutChildren`自体呼び出す必要があります`Measure`レイアウトの子である種のキャッシュ ロジック (後で説明) にサイズを実装することもできます。

### <a name="an-easy-example"></a>簡単な例

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo)サンプルが含まれていますが、簡略化された[ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs)クラスとその使用方法のデモンストレーションします。

### <a name="vertical-and-horizontal-positioning-simplified"></a>垂直および水平方向の配置の簡略化

ジョブのいずれかを`VerticalStack`を実行する必要があります中に発生します、`LayoutChildren`をオーバーライドします。 メソッドは、子の使用`HorizontalOptions`プロパティでは、そのスロット内の子の位置を決定する、`VerticalStack`します。 代わりに静的メソッドを呼び出すことができます[ `Layout.LayoutChildIntoBoundingRect`](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle))します。 このメソッドを呼び出します`Measure`子を使用してその`HorizontalOptions`と`VerticalOptions`プロパティを指定した四角形内の子を配置します。

### <a name="invalidation"></a>無効化

多くの場合、要素のプロパティの変更は、その要素がレイアウトで表示する方法に影響します。 レイアウトは、新しいレイアウトをトリガーする検証されたことがあります。

`VisualElement` 保護されているメソッドを定義します。 [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure)、一般にと呼ばれる任意のバインド可能なプロパティのプロパティ変更ハンドラーによって変更が、要素のサイズに影響を与えます。 `InvalidateMeasure`メソッドの起動、 [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated)イベント。

`Layout`クラスという名前のような保護されたメソッドを定義する[ `InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)を`Layout`方法を配置し、その子のサイズに影響する変更の派生物を呼び出す必要があります。

### <a name="some-rules-for-coding-layouts"></a>レイアウトのコーディングの一部のルール

1. によって定義されたプロパティ`Layout<T>`バインド可能なプロパティは、派生物をバックアップする必要があり、プロパティ変更ハンドラーを呼び出す必要があります`InvalidateLayout`します。

2. A`Layout<T>`接続されているバインド可能なプロパティを定義する派生クラスでオーバーライドする必要があります[ `OnAdded` ](xref:Xamarin.Forms.Layout`1.OnAdded*)プロパティ変更ハンドラーをその子に追加して[ `OnRemoved` ](xref:Xamarin.Forms.Layout`1.OnRemoved*)を削除するにはハンドラー。 ハンドラーは、する必要がありますでこれらの変更には、バインド可能なプロパティがアタッチされているを確認し、呼び出すことによって応答`InvalidateLayout`します。

3. A`Layout<T>`子のサイズのキャッシュを実装する派生クラスでオーバーライドする必要があります`InvalidateLayout`と[ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)し、これらのメソッドが呼び出されたときに、キャッシュをクリアします。

### <a name="a-layout-with-properties"></a>プロパティを持つレイアウト

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)同じサイズのいずれかとに 1 つの行 (または列) から、子をラップしますが、すべての子があると仮定次に、します。 定義、`Orientation`のようにプロパティ`StackLayout`と`ColumnSpacing`と`RowSpacing`などのプロパティ`Grid`、し、子のサイズをキャッシュします。

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) put のサンプルを`WrapLayout`で、`ScrollView`の写真を表示するためです。

### <a name="no-unconstrained-dimensions-allowed"></a>制約のないディメンションが許可されていません。

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリがそれ自体の中のすべての子を表示するためのものです。 したがって、制約のないディメンションを扱うことはできませんし、いずれかが発生した場合に例外を発生させます。

[**フォト グリッド**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid)サンプル`UniformGridLayout`:

[![フォト グリッドの 3 倍になるスクリーン ショット](images/ch26fg08-small.png "統一されたグリッド レイアウト")](images/ch26fg08-large.png#lightbox "統一されたグリッド レイアウト")

### <a name="overlapping-children"></a>重複する子

A`Layout<T>`派生物は、その子を重複ことができます。 ただし、子が内の順序でレンダリング、`Children`コレクション、およびの順序ではない、`Layout`メソッドが呼び出されます。

`Layout`クラスは、コレクション内の子を移動するための 2 つのメソッドを定義します。

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) 子をコレクションの先頭に移動するには
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) 子をコレクションの末尾に移動するには

重複する子、コレクションの末尾にある子はコレクションの先頭の子の上に視覚的に表示されます。

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリは、レンダリング順序を指定し、そのための 1 つできるようにする添付プロパティを定義しますその。その他の上部に表示される子。 [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile)のサンプルで例示します。

[![学生カード ファイル グリッドの 3 倍になるスクリーン ショット](images/ch26fg10-small.png "レイアウトの重複する子")](images/ch26fg10-large.png#lightbox "レイアウトの子の重複")

### <a name="more-attached-bindable-properties"></a>バインド可能なプロパティがアタッチされる方

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ定義の 2 つを指定するバインド可能なプロパティが添付`Point`値と太さの値を操作および`BoxView`要素を行のようにします。

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) 3 D キューブを描画するためにサンプルを使用します。

### <a name="layout-and-layoutto"></a>レイアウトと LayoutTo

A`Layout<T>`派生物を呼び出すことができます`LayoutTo`なく`Layout`レイアウトをアニメーション化します。 [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs)クラスがこれには、実行、および[ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube)サンプルでは、します。



## <a name="related-links"></a>関連リンク

- [第 26 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [第 26 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [カスタム レイアウトを作成します。](~/xamarin-forms/user-interface/layouts/custom.md)
