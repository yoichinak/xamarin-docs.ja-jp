---
title: '第 26 章の概要: カスタム レイアウト'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 26 章の概要: カスタム レイアウト'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: deb46d1a70e7c707c998be8669b4af3b8e8d7ead
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136605"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>第 26 章の概要: カスタム レイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

Xamarin.Forms には、[`Layout<View>`](xref:Xamarin.Forms.Layout`1) から派生した複数のクラスが含まれます。

- `StackLayout`、
- `Grid`、
- `AbsoluteLayout`、および
- `RelativeLayout`。

この章では、`Layout<View>` から派生する独自のクラスの作成方法について説明します。

## <a name="an-overview-of-layout"></a>レイアウトの概要

Xamarin.Forms レイアウトを処理する集中管理型システムはありません。 それぞれの要素では、必要な固有のサイズと、特定の領域内に自己をレンダリングする方法を決定する必要があります。

### <a name="parents-and-children"></a>親と子

子を持つ要素はすべて、それらの子を要素自体の中に配置する必要があります。 親内で利用可能なサイズとその子に想定されるサイズに基づいて、最終的に子に必要なサイズを決定するのは、親です。

### <a name="sizing-and-positioning"></a>サイズと位置の決定

レイアウトは、そのページでのビジュアル ツリーの一番上から開始され、すべての分岐を通ります。 レイアウトで最も重要なパブリック メソッドは、`VisualElement` によって定義されている [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) です。 他の要素の親になっている要素はすべて、その子それぞれに対して `Layout` を呼び出して、[`Rectangle`](xref:Xamarin.Forms.Rectangle) 値の形式で子のサイズと位置を相対的に指定します。 これらの `Layout` の呼び出しは、ビジュアル ツリーを通じて伝達されます。

`Layout` の呼び出しは、画面上に要素を表示するために必要であり、呼び出しが行われると、次の読み取り専用プロパティが設定されます。 それらは、メソッドに渡される `Rectangle` と一致します。

- `Rectangle` 型の [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)
- `double` 型の [`X`](xref:Xamarin.Forms.VisualElement.X)
- `double` 型の [`Y`](xref:Xamarin.Forms.VisualElement.Y)
- `double` 型の [`Width`](xref:Xamarin.Forms.VisualElement.Width)
- `double` 型の [`Height`](xref:Xamarin.Forms.VisualElement.Height)

`Layout` の呼び出し前には、`Height` と `Width` には &ndash;1 のモック値が含まれています。

`Layout` を呼び出すと、次の保護されたメソッドの呼び出しもトリガーされます。

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)) (以下を呼び出します)
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double)) (オーバーライドできます)

最後に、次のイベントが発生します。

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`OnSizeAllocated` メソッドは、Xamarin.Forms 内に 2 つだけ存在する、子を持つことができるクラスの `Page` と `Layout` によってオーバーライドされます。 オーバーライドされたメソッドでは、以下を呼び出します

- `Page` の派生に対しては [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout)、`Layout` の派生に対しては [`UpdateChildrenLayout`](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) (以下を呼び出します)
- `Page` の派生に対しては [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double))、`Layout` の派生に対しては [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))

`LayoutChildren` では、要素の子それぞれに対して `Layout` を呼び出します。 少なくとも 1 つの子に新しい `Bounds` 設定がある場合、次のイベントが発生します。

- `Page` の派生に対しては [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged)、`Layout` の派生に対しては [`LayoutChanged`](xref:Xamarin.Forms.Layout.LayoutChanged)

### <a name="constraints-and-size-requests"></a>制約とサイズ要求

`LayoutChildren` では、すべての子に対して `Layout` を賢く呼び出すために、子にとって "*望ましい*" または "*必要な*" サイズを把握している必要があります。 そのため、通常は各子に対する `Layout` 呼び出しの前に、以下を呼び出します

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

本書が公開された後に、`GetSizeRequest` メソッドは非推奨となり、以下に置き換えられました

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure` メソッドは [`Margin`](xref:Xamarin.Forms.View.Margin) プロパティに対応しており、次の 2 つのメンバーを持つ [`MeasureFlag`](xref:Xamarin.Forms.MeasureFlags) 型の引数を含みます。

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) (余白を含めないようにするため)

多くの要素では、`GetSizeRequest` または `Measure` によって、そのレンダラーから要素のネイティブ サイズを取得します。 どちらのメソッドにも、幅と高さの "*制約*" に対応するパラメーターがあります。 たとえば、`Label` では、幅の制約を使用して、複数行のテキストを折り返す方法を決定します。

`GetSizeRequest` と `Measure` では両方とも、[`SizeRequest`](xref:Xamarin.Forms.SizeRequest) 型の値を返します。次の 2 つのプロパティがあります。

- `Size` 型の [`Request`](xref:Xamarin.Forms.SizeRequest.Request)
- `Size` 型の [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum)

多くの場合、これら 2 つの値は同一になり、通常は `Minimum` 値は無視できます。

また、`VisualElement` には、`GetSizeRequest` から呼び出される `GetSizeRequest` と同様の、以下の保護されたメソッドも定義されています。

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) (`SizeRequest` 値を返します)

該当のメソッドは現在では非推奨となり、以下に置き換えられました。

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

`Layout` または `Layout<T>` から派生するすべてのクラスでは、`OnSizeRequest` または `OnMeasure` をオーバーライドする必要があります。 ここでは、レイアウト クラスによって固有のサイズが決定されます。通常は、子に対して `GetSizeRequest` または `Measure` を呼び出すことで取得した子のサイズに基づきます。 `OnSizeRequest` または `OnMeasure` を呼び出す前後に、`GetSizeRequest` または `Measure` では、次のプロパティに基づいて調整を行います。

- `double` 型の [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)。`SizeRequest` の `Request` プロパティに影響を与えます
- `double` 型の [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)。`SizeRequest` の `Request` プロパティに影響を与えます
- `double` 型の [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)。`SizeRequest` の `Minimum` プロパティに影響を与えます
- `double` 型の [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)。`SizeRequest` の `Minimum` プロパティに影響を与えます

### <a name="infinite-constraints"></a>無限の制約

`GetSizeRequest` (または `Measure`) および `OnSizeRequest` (または `OnMeasure`) に渡される制約の引数は、無限 (つまり、`Double.PositiveInfinity` の値) です。 ただし、これらのメソッドから返される `SizeRequest` には、無限ディメンションを含めることはできません。

無限の制約では、要求されたサイズには要素の自然なサイズが反映される必要があることを示します。 垂直の `StackLayout` では、無制限の高さ制約を使用して、その子に対して `GetSizeRequest` (または `Measure`) を呼び出します。 水平のスタック レイアウトでは、無制限の幅制約を使用して、その子に対して `GetSizeRequest` (または `Measure`) を呼び出します。 `AbsoluteLayout` では、無制限の幅および高さ制約を使用して、その子に対して `GetSizeRequest` (または `Measure`) を呼び出します。

### <a name="peeking-inside-the-process"></a>プロセス内のピーク

[**ExploreChildSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) では、シンプルなレイアウトに対する制約とサイズ要求の情報を表示します。

## <a name="deriving-from-layoutview"></a>Layout\<View> からの派生

カスタム レイアウト クラスは、`Layout<View>` から派生します。 次の 2 つの役割があります。

- `OnMeasure` をオーバーライドして、レイアウトのすべての子に対して `Measure` を呼び出します。 レイアウト自体に要求されたサイズを返します
- `LayoutChildren` をオーバーライドして、レイアウトのすべての子に対して `Layout` を呼び出します。

これらのオーバーライドでの `for` または `foreach` ループでは、`IsVisible` プロパティが `false` に設定されているすべての子をスキップする必要があります。

`OnMeasure` の呼び出しは保証されません。 レイアウトの親によってレイアウトのサイズが管理されている場合 (たとえば、1 ページを埋めるレイアウトなど)、`OnMeasure` は呼び出されません。 このため、`LayoutChildren` では、`OnMeasure` の呼び出し時に取得された子のサイズに依存することはできません。 多くの場合、`LayoutChildren` 自体によって、レイアウトの子に対して `Measure` を呼び出す必要があります。または、(後述するように) ご自身で何らかのサイズ キャッシュのロジックを実装することもできます。

### <a name="an-easy-example"></a>簡単な例

[**VerticalStackDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) サンプルには、簡略化された [`VerticalStack`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) クラスとその使用方法のデモンストレーションが含まれています。

### <a name="vertical-and-horizontal-positioning-simplified"></a>簡略化された垂直および水平の位置決定

`VerticalStack` によって実行される必要があるジョブの 1 つは、`LayoutChildren` のオーバーライド中に行われる必要があります。 メソッドでは、子の `HorizontalOptions` プロパティを使用して、`VerticalStack` のスロット内での子の配置方法を決定します。 代わりに、静的メソッド [`Layout.LayoutChildIntoBoundingRect`](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) を呼び出すことができます。 このメソッドでは、子に対して `Measure` を呼び出し、その `HorizontalOptions` と `VerticalOptions` プロパティを使用して、指定された四角形の中にその子を配置します。

### <a name="invalidation"></a>無効化

多くの場合、要素のプロパティを変更すると、レイアウト上でのその要素の表示方法に影響します。 新しいレイアウトをトリガーするには、レイアウトを無効にする必要があります。

`VisualElement` では、保護されたメソッド [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) を定義しています。このメソッドは通常、変更されると要素のサイズに影響を与えるバインド可能なプロパティのプロパティ変更ハンドラーによって呼び出されます。 `InvalidateMeasure` メソッドでは、[`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) イベントを発生させます。

`Layout` クラスでは、同様の [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) という保護されたメソッドを定義しています。このメソッドは、子の位置とサイズの決定方法に影響を及ぼす変更があった場合には、`Layout` の派生によって必ず呼び出されます。

### <a name="some-rules-for-coding-layouts"></a>レイアウトをコーディングするためのいくつかのルール

1. `Layout<T>` の派生によって定義されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。また、そのプロパティ変更ハンドラーでは `InvalidateLayout` を呼び出す必要があります。

2. アタッチされたバインド可能なプロパティを定義する `Layout<T>` の派生では、[`OnAdded`](xref:Xamarin.Forms.Layout`1.OnAdded*) をオーバーライドしてプロパティ変更ハンドラーをその子と [`OnRemoved`](xref:Xamarin.Forms.Layout`1.OnRemoved*) に追加して、そのハンドラーを削除する必要があります。 ハンドラーでは、これらのアタッチされたバインド可能なプロパティでの変更を確認し、`InvalidateLayout` を呼び出して応答します。

3. 子サイズのキャッシュを実装する `Layout<T>` の派生では、`InvalidateLayout` および [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) をオーバーライドして、これらのメソッドが呼び出されたときにキャッシュをクリアします。

### <a name="a-layout-with-properties"></a>プロパティが設定されたレイアウト

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) にある [`WrapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) クラスでは、すべての子が同じサイズであることを前提とし、1 つの行 (または列) から次へと子を折り返します。 `StackLayout` のような `Orientation` プロパティと、`Grid` のような `ColumnSpacing` および `RowSpacing` プロパティを定義し、子のサイズをキャッシュします。

[**PhotoWrap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) サンプルでは、ストックされた写真を表示するために、`ScrollView` 内に `WrapLayout` を配置します。

### <a name="no-unconstrained-dimensions-allowed"></a>制約のないディメンションは許可されない

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリにある [`UniformGridLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) は、その中にあるすべての子を表示することを目的としています。 したがって、制約のないディメンションを処理することはできず、その状況に遭遇した場合には、例外が発生します。

[**PhotoGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) サンプルでは、`UniformGridLayout` のデモを示しています。

[![Photo Grid のトリプル スクリーンショット](images/ch26fg08-small.png "グリッド レイアウトの統一")](images/ch26fg08-large.png#lightbox "グリッド レイアウトの統一")

### <a name="overlapping-children"></a>子の重複

`Layout<T>` の派生では、子が重複する場合があります。 しかし、子は、`Layout` メソッドが呼び出される順序ではなく、`Children` コレクション内の順序でレンダリングされます。

`Layout` クラスでは、コレクション内の子を移動できる 2 つのメソッドを定義しています。

- 子をコレクションの先頭に移動するための [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View))
- 子をコレクションの末尾に移動するための [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View))

子が重複している場合、コレクションの末尾にある子は、コレクションの先頭にある子より優先して、視覚的に表示されます。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリにある [`OverlapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) クラスでは、アタッチされたプロパティを定義してレンダリング順序を指示し、これによって子のうちの 1 つをそれ以外よりも優先して表示できるようにします。 [**StudentCardFile**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) サンプルでは、以下のデモを示しています。

[![Student Card File Grid のトリプル スクリーンショット](images/ch26fg10-small.png "レイアウトの子の重複")](images/ch26fg10-large.png#lightbox "レイアウトの子の重複")

### <a name="more-attached-bindable-properties"></a>その他のアタッチされたバインド可能なプロパティ

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリにある [`CartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) クラスでは、アタッチされたバインド可能なプロパティを定義して 2 つの `Point` 値と太さの値を指定し、`BoxView` 要素を操作して行のようにします。

[**UnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) サンプルでは、それを使用して 3 次元のキューブを描画しています。

### <a name="layout-and-layoutto"></a>Layout と LayoutTo

`Layout<T>` の派生では、`Layout` ではなく `LayoutTo` を呼び出して、レイアウトをアニメーション化することができます。 これは [`AnimatedCartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) クラスによって行われ、[**AnimatedUnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) サンプルではそのデモを示しています。

## <a name="related-links"></a>関連リンク

- [第 26 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [第 26 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [カスタム レイアウトの作成](~/xamarin-forms/user-interface/layouts/custom.md)
