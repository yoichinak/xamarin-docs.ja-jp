---
title: "26 章の概要です。 カスタム レイアウト"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9447f9fb47a3de0f278a89d45d657158be9b70b9
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-26-custom-layouts"></a>26 章の概要です。 カスタム レイアウト

Xamarin.Forms から派生したクラスがいくつかを含む[ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`、
* `Grid`、
* `AbsoluteLayout`、および
* `RelativeLayout`。

派生する独自のクラスを作成する方法を説明`Layout<View>`です。

## <a name="an-overview-of-layout"></a>レイアウトの概要

Xamarin.Forms のレイアウトを処理する一元的なシステムはありません。 各要素はどのような独自サイズ必要があります、および特定の領域内でそれ自体をレンダリングする方法を決定するために行います。

### <a name="parents-and-children"></a>親と子

子を持つすべての要素は、それ自体には、その子の位置を調整します。 親の子のサイズを新機能を最終的に決定に基づく必要がありますがサイズが使用できると、子のサイズしたいです。

### <a name="sizing-and-positioning"></a>サイズおよび位置

レイアウトでは、ページのビジュアル ツリーの上部にある開始し、すべての分岐を進めします。 レイアウト内で最も重要なパブリック メソッドは[ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)によって定義された`VisualElement`です。 その他の要素の呼び出しの親であるすべての要素`Layout`のそれぞれのサイズと位置自体の形式では、子を提供する子、 [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/)値。 これら`Layout`ビジュアル ツリーを通じて伝達されるまでの呼び出しです。

呼び出し`Layout`は、画面に表示する要素の必須であり、次の読み取り専用プロパティを設定すると、します。 対応する、`Rectangle`メソッドに渡されます。

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) 型の `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) 型の `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) 型の `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) 型の `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) 型の `double`

前のバージョン、`Layout`を呼び出すと、`Height`と`Width`のモック値 & #x 2013; がある 1 です。

呼び出し`Layout`も次の保護されたメソッドの呼び出しをトリガーします。

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/)を呼び出す
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/)、これをオーバーライドすることができます。

最後に、次のイベントが発生します。

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated`メソッドはによってオーバーライド`Page`と`Layout`、これらは子を持つことができる Xamarin.Forms で 2 つのクラスです。 オーバーライドされたメソッドの呼び出し

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) `Page`派生型および[ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/)の`Layout`呼び出しから派生しました。
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) `Page`派生型および[ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)の`Layout`派生します。

`LayoutChildren` 呼び出して`Layout`要素の子の各します。 少なくとも 1 つの子がある新しい場合`Bounds`設定すると、次のイベントが発生し、します。

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) `Page`派生型および[ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/)の`Layout`派生クラス

### <a name="constraints-and-size-requests"></a>制約とサイズの要求

`LayoutChildren`をインテリジェントに呼び出す`Layout`すべての子である必要がありますを認識して、*優先*または*目的*子のサイズ。 そのため、呼び出し`Layout`それぞれの子が一般的に付きますへの呼び出し

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

ブックは、パブリッシュした後、`GetSizeRequest`メソッドが非推奨し、置き換えられます

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure`メソッドに対応、 [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)プロパティ型の引数が含まれていますと[ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/)、2 つのメンバーを持ちます。

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) 余白を含まない

多くの要素、`GetSizeRequest`または`Measure`レンダラーからネイティブの要素のサイズを取得します。 どちらの方法は、幅と高さのパラメーターを持つ*制約*です。 たとえば、`Label`複数行のテキストをラップする方法を決定する、幅を使用します。

両方`GetSizeRequest`と`Measure`型の値を返す[ `SizeRequest`](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/)を持つ 2 つのプロパティ。

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) 型の `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) 型の `Size`

ほとんどの場合、これら 2 つの値が同じで、`Minimum`通常値は無視されます。

`VisualElement` ような保護されたメソッドを定義も`GetSizeRequest`から呼び出される`GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) 返します、`SizeRequest`値

そのメソッドが非推奨に置き換えられますなりました。

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

すべてのクラスから派生した`Layout`または`Layout<T>`オーバーライドする必要があります`OnSizeRequest`または`OnMeasure`です。 これは、レイアウトのクラスが、独自のサイズは、一般的にこれを呼び出すことによって取得、その子のサイズに基づいてが決定される`GetSizeRequest`または`Measure`子にします。 呼び出しの前後に`OnSizeRequest`または`OnMeasure`、`GetSizeRequest`または`Measure`次のプロパティに基づく調整。

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)型の`double`、影響を与える、`Request`のプロパティ `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 型の`double`、影響を与える、`Request`のプロパティ `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) 型の`double`、影響を与える、`Minimum`のプロパティ `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) 型の`double`、影響を与える、`Minimum`のプロパティ `SizeRequest`

### <a name="infinite-constraints"></a>無限の制約

制約の引数に渡されます`GetSizeRequest`(または`Measure`) および`OnSizeRequest`(または`OnMeasure`) 有限ことができます (の値、つまり`Double.PositiveInfinity`)。 ただし、`SizeRequest`これらから返されたメソッドは、無限のディメンションを含めることはできません。

無限の制約は、要求されたサイズが、要素の自然なサイズを反映する必要がありますを指定します。 垂直方向`StackLayout`呼び出し`GetSizeRequest`(または`Measure`) 無限高さ制約を使用してその子にします。 水平方向のスタックのレイアウトを呼び出す`GetSizeRequest`(または`Measure`) 無限幅制約を使用してその子にします。 `AbsoluteLayout`呼び出し`GetSizeRequest`(または`Measure`) 無限の幅と高さの制約とその子にします。

### <a name="peeking-inside-the-process"></a>プロセスの内部のピーク

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes)表示制約とサイズは、単純なレイアウトの情報を要求します。

## <a name="deriving-from-layoutview"></a>レイアウトから派生します。<View>

カスタム レイアウトのクラスから派生`Layout<View>`です。 2 つの役割があります。

- オーバーライド`OnMeasure`を呼び出す`Measure`レイアウトのすべての子にします。 レイアウトの種類の要求されたサイズを返します
- オーバーライド`LayoutChildren`を呼び出す`Layout`レイアウトのすべての子で

`for`または`foreach`これらのオーバーライド内のループは、任意の子をスキップする必要がありますが`IsVisible`プロパティに設定されている`false`です。

呼び出し`OnMeasure`は保証されません。 `OnMeasure` 呼び出されません、レイアウトの親がレイアウトのサイズ (たとえば、ページを格納するレイアウト) を制御する場合。 このため、`LayoutChildren`中に取得された子のサイズに依存できない、`OnMeasure`呼び出します。 非常に多くの場合、`LayoutChildren`自体を呼び出す必要あります`Measure`レイアウトの子供のロジック (後述) のキャッシュ サイズのいくつかの種類を実装することもできます。

### <a name="an-easy-example"></a>簡単な例

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo)サンプルが含まれていますが、簡略化された[ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs)クラスとその使用方法のデモします。

### <a name="vertical-and-horizontal-positioning-simplified"></a>垂直および水平方向の配置の簡略化

ジョブのいずれかを`VerticalStack`実行する必要があります中に発生した、`LayoutChildren`をオーバーライドします。 メソッドは、子の使用`HorizontalOptions`でスロット内の子の位置を決定するプロパティ、`VerticalStack`です。 代わりに静的メソッドを呼び出すことができます[ `Layout.LayoutChildIntoBoundingRect`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/)です。 このメソッドを呼び出す`Measure`使用して子の`HorizontalOptions`と`VerticalOptions`プロパティに指定された四角形内の子を配置します。

### <a name="invalidation"></a>無効化

多くの場合、要素のプロパティの変更は、その要素をレイアウトで表示する方法に影響します。 レイアウトは、新しいレイアウトをトリガーする検証する必要があります。

`VisualElement` 保護されたメソッドを定義[ `InvalidateMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/)要素のサイズに影響を与える、通常によって呼び出される任意のバインド可能なプロパティのプロパティ変更ハンドラーを変更します。 `InvalidateMeasure`メソッドが起動、 [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/)イベント。

`Layout`クラスという名前のような保護されたメソッドを定義する[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)、どの、`Layout`派生物は、任意の変更方法を配置し、その子のサイズに影響を与える呼び出す必要があります。

### <a name="some-rules-for-coding-layouts"></a>レイアウトをコーディングするためのいくつかのルール

1. によって定義されたプロパティ`Layout<T>`バインド可能なプロパティは、派生型をバックアップするか、プロパティ変更ハンドラーを呼び出す必要があります`InvalidateLayout`です。

2. A`Layout<T>`接続されているバインド可能なプロパティを定義する派生物をオーバーライドする必要があります[ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/)自身の子にプロパティ変更ハンドラーを追加して[ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/)を削除するにはハンドラー。 ハンドラーは、これらの変更には、バインド可能なプロパティが接続されているをチェックして呼び出すことによって応答`InvalidateLayout`です。

3. A`Layout<T>`子のサイズのキャッシュを実装する派生物をオーバーライドする必要があります`InvalidateLayout`と[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/)し、これらのメソッドが呼び出されたときに、キャッシュをクリアします。

### <a name="a-layout-with-properties"></a>プロパティを持つレイアウト

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)そのすべての子がサイズ、同一であり、子を 1 つの行 (または列) からに折り返されることを前提としています次に、します。 定義する、`Orientation`のようにプロパティ`StackLayout`、および`ColumnSpacing`と`RowSpacing`のようなプロパティ`Grid`子のサイズがキャッシュとします。

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) puts のサンプル、`WrapLayout`で、`ScrollView`ストックの写真を表示するためです。

### <a name="no-unconstrained-dimensions-allowed"></a>制約のないディメンションが許可されていません。

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリはそれ自体のすべての子を表示するためのものです。 そのため、制約のないディメンションを扱うことはできませんし、いずれかが発生した場合、例外が発生します。

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid)サンプル`UniformGridLayout`:

[![写真のグリッドのトリプル スクリーン ショット](images/ch26fg08-small.png "均一なグリッド レイアウト")](images/ch26fg08-large.png#lightbox "均一なグリッド レイアウト")

### <a name="overlapping-children"></a>重複する子

A`Layout<T>`から派生したその子に重複できます。 ただし、子での順序でレンダリングされます、`Children`コレクション、および順序にない、`Layout`メソッドが呼び出されます。

`Layout`クラス、コレクション内にある子に移動することは 2 つのメソッドを定義します。

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) 子をコレクションの先頭に移動するには
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) 子をコレクションの末尾に移動するには

重複する子は、コレクションの末尾で子を視覚的にコレクションの先頭の子の上に表示されます。

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ レンダリングの順序を指定でき、したがってのいずれかに接続されているプロパティを定義する、その他の上部に表示される子。 [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile)サンプルを示します。

[![学生カード ファイル グリッドのトリプル スクリーン ショット](images/ch26fg10-small.png "レイアウトの子供の重複")](images/ch26fg10-large.png#lightbox "レイアウトの子供の重複")

### <a name="more-attached-bindable-properties"></a>アタッチされる複数のバインド可能なプロパティ

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリは 2 つを指定する接続のバインド可能なプロパティを定義`Point`値と太さ値を操作および`BoxView`行と同じように要素。

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube)サンプルでは、を使用して、3 D キューブを描画します。

### <a name="layout-and-layoutto"></a>レイアウトと LayoutTo

A`Layout<T>`派生物を呼び出すことができます`LayoutTo`なく`Layout`レイアウトをアニメーション化します。 [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs)クラスが、これと[ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube)サンプルでは、ことを示しています。



## <a name="related-links"></a>関連リンク

- [章 26 フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [26 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [カスタム レイアウトを作成します。](~/xamarin-forms/user-interface/layouts/custom.md)
