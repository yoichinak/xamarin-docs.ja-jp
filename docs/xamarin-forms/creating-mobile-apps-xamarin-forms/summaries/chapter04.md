---
title: '第 4 章の概要: スタックのスクロール'
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 第 4 章の概要: スタックのスクロール'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bda9d5cb323524981bed9c3bb55998513dd69aab
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73032873"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>第 4 章の概要: スタックのスクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

この章では、主に、"*レイアウト*" の概念を紹介します。これは、ページ上の複数のビューの視覚的な表示を整理するために Xamarin.Forms で使用されるクラスと手法に対する全体的な用語です。

レイアウトに含まれる複数のクラスは、[`Layout`](xref:Xamarin.Forms.Layout) と [`Layout<T>`](xref:Xamarin.Forms.Layout`1) から派生します。 この章では、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に焦点を当てます。

> [!NOTE]
> Xamarin.Forms 3.0 で導入された [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md) は、`StackLayout` と似た方法で使用できますが、いっそう高い柔軟性を備えています。

この章では、[`ScrollView`](xref:Xamarin.Forms.ScrollView)、[`Frame`](xref:Xamarin.Forms.Frame)、[`BoxView`](xref:Xamarin.Forms.BoxView) の各クラスについても紹介します。

## <a name="stacks-of-views"></a>ビューのスタック

[`StackLayout`](xref:Xamarin.Forms.StackLayout) は `Layout<View>` から派生し、`IList<View>` 型の [`Children`](xref:Xamarin.Forms.Layout`1) プロパティを継承します。 複数のビュー項目をこのコレクションに追加すると、`StackLayout` では水平または垂直のスタックに表示されます。

`StackLayout` の [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) プロパティを、[`StackOrientation`](xref:Xamarin.Forms.StackOrientation) 列挙型のメンバー ([`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) または [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)) に設定します。 既定値は、`Vertical` です。

子の間隔を指定するには、`StackLayout` の [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) プロパティに `double` 値を設定します。 既定値は 6 です。

コードでは、`for` または `foreach` のループ内で `StackLayout` の `Children` コレクションに項目を追加できます ([**ColorLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) サンプルを参照)。または、個々のビューのリストを使用して `Children` コレクションを初期化することもできます ([**ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList) を参照)。 子は `View` から派生している必要がありますが、他の `StackLayout` オブジェクトを含んでいてもかまいません。

## <a name="scrolling-content"></a>内容のスクロール

`StackLayout` に含まれる子の数が多すぎてページに表示できない場合は、`StackLayout` を [`ScrollView`](xref:Xamarin.Forms.ScrollView) に配置してスクロールできるようにすることができます。

`ScrollView` の [`Content`](xref:Xamarin.Forms.ScrollView.Content) プロパティに、スクロールするビューを設定します。 これは、`StackLayout` のことが多いですが、どのようなビューでもかまいません。

`ScrollView` の [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) プロパティを、[`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) プロパティのメンバー ([`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical)、[`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal)、[`Both`](xref:Xamarin.Forms.ScrollOrientation.Both)) に設定します。 既定値は、`Vertical` です。 `ScrollView` の内容が `StackLayout` の場合は、2 つの方向が一致している必要があります。

[**ReflectedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) サンプルでは、`ScrollView` と `StackLayout` を使用して使用可能な色を表示する方法が示されています。 このサンプルでは、.NET リフレクションを使用して、`Color` 構造体のすべてのパブリックな静的プロパティとフィールドを、明示的に列記する必要なしに、取得する方法も示されています。

## <a name="the-expands-option"></a>展開オプション

`StackLayout` でその子がスタックされると、各子によって、`StackLayout` 全体の高さ内で特定のスロットが占有され、それは子のサイズとその `HorizontalOptions` および `VerticalOptions` プロパティの設定に依存します。 これらのプロパティには、[`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 型の値が割り当てられます。

`LayoutOptions` 構造体では、次の 2 つのプロパティを定義します。

- 4 つのメンバー [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)、[`Center`](xref:Xamarin.Forms.LayoutAlignment.Center)、[`End`](xref:Xamarin.Forms.LayoutAlignment.End)、[`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill) を持つ列挙型 [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) の [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment)
- `bool` 型の [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)

利便性を高めるために、`LayoutOptions` 構造体では、2 つのインスタンス プロパティのすべての組み合わせを含む、`LayoutOptions` 型の次の 8 つの静的な読み取り専用フィールドも定義されています。

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

次の説明は、既定の縦方向の `StackLayout` に関するものです。 横方向の `StackLayout` も同様です。

縦の `StackLayout` では、`HorizontalOptions` の設定によって、`StackLayout` の幅内での子の水平方向の配置方法が決まります。 `Start`、`Center`、または `End` の `Alignment` の設定によって、子は水平方向に制約されなくなります。 子によってそれ自体の幅が決定され、子は `StackLayout` の左、中央、または右に配置されます。 `Fill` オプションを使用すると、子は水平方向に制約され、`StackLayout` の幅になります。

縦の `StackLayout` の場合、各子は縦方向に制約されず、子の高さに応じて縦方向のスロットを取得します。この場合、`VerticalOptions` の設定は関係ありません。

縦の `StackLayout` 自体が制約されていない場合、つまりその `VerticalOptions` の設定が `Start`、`Center`、または `End`である場合は、`StackLayout` の高さはその子の高さの合計になります。

一方、縦の `StackLayout` が縦方向に制約されている場合、つまり `VerticalOptions` の設定が `Fill` の場合は、`StackLayout` の高さはそのコンテナーの高さになります。これは、子の高さの合計より大きくなる可能性があります。 そのような場合で、さらに少なくとも 1 つの子の `VerticalOptions` の設定で `Expands` フラグが `true` に設定されている場合は、`StackLayout` 内の余分な領域は `Expands` フラグが `true` であるすべての子に均等に割り当てられます。 その場合、子の高さの合計は `StackLayout` の高さと等しくなり、`VerticalOptions` の設定の `Alignment` の部分によって、そのスロットにおける子の縦方向の配置が決まります。

これについては、[**VerticalOptionsDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) サンプルで示されています。

## <a name="frame-and-boxview"></a>Frame と BoxView

これら 2 つの四角形のビューは、プレゼンテーション目的でよく使用されます。

[`Frame`](xref:Xamarin.Forms.Frame) ビューでは、別のビューの周囲に四角形の枠が表示されます。これは、`StackLayout` などのレイアウトにすることができます。 `Frame` が [`ContentView`](xref:Xamarin.Forms.ContentView) から継承する [`Content`](xref:Xamarin.Forms.ContentView.Content) プロパティには、`Frame` 内に表示するビューを設定します。 `Frame` は、既定では透明になります。 フレームの外観をカスタマイズするには、次の 3 つのプロパティを設定します。

- フレームが表示されるようにするには、[`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) プロパティを使用します。 基になっている配色がわからない場合は、`OutlineColor` を `Color.Accent` に設定するのが一般的です。
- [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) プロパティを `true` に設定すると、iOS デバイスで黒い影を表示できます。
- フレームとフレームの内容の間にスペースを設けるには、[`Padding`](xref:Xamarin.Forms.Layout.Padding) プロパティに `Thickness` の値を設定します。 既定値では、すべての辺が 20 単位です。

`Frame` の `LayoutOptions.Fill` には既定の `HorizontalOptions` と `VerticalOptions` の値があります。これは、`Frame` によってコンテナーがいっぱいになることを意味します。 その他の設定では、`Frame` のサイズはその内容のサイズが基になります。

`Frame` については、[**FramedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) サンプルを参照してください。

[`BoxView`](xref:Xamarin.Forms.BoxView) では、[`Color`](xref:Xamarin.Forms.BoxView.Color) プロパティによって指定された色の四角形の領域が表示されます。

`BoxView` が制約されている場合 (`HorizontalOptions` および `VerticalOptions` プロパティが既定の設定の `LayoutOptions.Fill`)、`BoxView` は使用可能な領域がいっぱいになります。 `BoxView` が制約されていない場合は (`HorizontalOptions` と `LayoutOptions` の設定が `Start`、`Center`、または `End`)、既定の寸法である 40 単位の正方形になります。 `BoxView` では、一方の寸法を制約ありにし、他の寸法を制約なしにすることができます。

多くの場合、`BoxView` の [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティと [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) プロパティを設定して、特定のサイズを指定します。 これについては、[**SizedBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) サンプルで示されています。

`StackLayout` の複数のインスタンスを使用して、1 つの `BoxView` と複数の `Label` のインスタンスを `Frame` 内で組み合わせて特定の色を表示し、これらの各ビューを `ScrollView` 内の `StackLayout` に配置して、[**ColorBlocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) サンプルで示されているような魅力的な色のリストを作成できます。

[![色のブロックのトリプル スクリーンショット](images/ch04fg11-small.png "色の一覧")](images/ch04fg11-large.png#lightbox "色の一覧")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout 内の ScrollView か

`ScrollView` 内に `StackLayout` を配置するのが一般的ですが、`ScrollView` を `StackLayout` に配置すると便利な場合もあります。 理論的には、縦の `StackLayout` の子は縦方向に制約されないため、これは不可能なはずです。 ただし、`ScrollView` を縦方向に制約する必要があります。 スクロールに対して子のサイズを決定できるように、特定の高さを指定する必要があります。

秘訣は、`StackLayout` の子の `ScrollView` で、`VerticalOptions` を `FillAndExpand` に設定することです。 これについては、[**BlackCat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) サンプルで示されています。

**BlackCat** サンプルでは、共有ライブラリに埋め込まれているプログラム リソースを定義してアクセスする方法も示されています。 これは、共有アセット プロジェクト (SAP) でも実現できますが、[**BlackCatSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) サンプルで示されているように、プロセスは少し複雑になります。

## <a name="related-links"></a>関連リンク

- [第 4 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [第 4 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [第 4 章の F# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
