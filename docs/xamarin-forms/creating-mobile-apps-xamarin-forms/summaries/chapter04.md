---
title: "第 4 章の概要です。 スタックをスクロール"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5559f9e6a4baf9d3f82701b5e3f341900ba83bae
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>第 4 章の概要です。 スタックをスクロール

この章のための概念が導入には、主に*レイアウト*、これは、クラスと Xamarin.Forms を使用して、ページ上の複数のビューのビジュアル表示を整理する手法の全体的な用語です。

レイアウトは、いくつかのクラスから派生する[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)と[ `Layout<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)です。 この章で[ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)です。

この章でも導入されましたが、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)、 [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)、および[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)クラスです。

## <a name="stacks-of-views"></a>ビューの履歴

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 派生した`Layout<View>`を継承し、 [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)型のプロパティ`IList<View>`です。 このコレクションに複数のビュー項目を追加して`StackLayout`水平または垂直スタックに表示されます。

設定、 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/)プロパティ`StackLayout`のメンバーに、 [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/)列挙型、どちらか[ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/)または[`Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). 既定値は、`Vertical` です。

設定、 [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/)プロパティ`StackLayout`を`double`子間の間隔を指定する値。 既定値は 6 です。

コードでは、項目を追加することができます、`Children`のコレクション`StackLayout`で、`for`または`foreach`ループで示したように、 [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop)またはサンプルについては、初期化、`Children`リスト ビューのコレクション、個々 に示すよう[ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList)です。 子がから派生する必要があります`View`他を含めることができますが、`StackLayout`オブジェクト。

## <a name="scrolling-content"></a>コンテンツをスクロール

場合、 `StackLayout` 、ページに表示する多数の子を含む配置することができます、`StackLayout`で、 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)にスクロールすることができます。

設定、 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/)プロパティ`ScrollView`をスクロールするビュー。 これは、多くの場合、`StackLayout`が任意のビューを指定できます。

設定、 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/)プロパティ`ScrollView`のメンバーに、 [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)プロパティ、 [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/)、 [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/)、または[ `Both`](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/)です。 既定値は、`Vertical` です。 場合のコンテンツ、`ScrollView`は、 `StackLayout`、2 つの方向が一致する必要があります。

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors)サンプルでの使用`ScrollView`と`StackLayout`を使用できる色を表示します。 .NET リフレクションを使用して、すべてのパブリック静的プロパティおよびのフィールドを取得する方法も示します、`Color`構造を明示的にそれらの一覧を表示する必要はありません。

## <a name="the-expands-option"></a>展開オプション

ときに、`StackLayout`スタックその子では、それぞれの子が占める高さの合計内の特定のスロット、`StackLayout`子のサイズの設定に依存するその`HorizontalOptions`と`VerticalOptions`プロパティです。 これらのプロパティには、型の値が割り当てられる[ `LayoutOptions`](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/)です。

`LayoutOptions`構造体は 2 つのプロパティを定義します。

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) 列挙型の[ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) 4 つのメンバーを持つ[ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/)、 [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/)、 [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/)、および [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) 型の `bool`

必要に応じて、`LayoutOptions`構造体は型の 8 つ静的な読み取り専用のフィールドも定義`LayoutOptions`2 つのインスタンスのプロパティのすべての組み合わせを包含します。

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

次の説明では、`StackLayout`デフォルトの垂直方向。 水平`StackLayout`は類似しています。

垂直方向の`StackLayout`、`HorizontalOptions`子水平方向に配置する方法の幅に収まるかを決定、`StackLayout`です。 `Alignment`設定`Start`、 `Center`、または`End`子供が水平方向に制約付きの原因です。 子が独自の幅を決定し、left、center、またはの右側に配置されます、`StackLayout`です。 `Fill`オプションと、制約が適用される水平方向に子との幅いっぱいになる、`StackLayout`です。

垂直方向の`StackLayout`、それぞれの子は、垂直方向には制約がなく、取得垂直スロットお子様の高さに応じている場合、`VerticalOptions`設定は使用されません。

場合、垂直`StackLayout`自体は制約がありません&mdash;されている場合その`VerticalOptions`設定は`Start`、 `Center`、または`End`の高さ、`StackLayout`その子の高さの合計。

ただし場合、垂直`StackLayout`が垂直方向に制限されて&mdash;場合その`VerticalOptions`設定は`Fill`&mdash;の高さをし、`StackLayout`合計を超える可能性のあるコンテナーの高さになりますその子の高さ。 場合は、少なくとも 1 つの子とする場合は、`VerticalOptions`による設定、`Expands`フラグ`true`で余分なスペースでは、`StackLayout`でこれらすべての子の間で均等に割り当てられている、`Expands`のフラグ`true`です。 子の全体の高さがの高さになりますし、 `StackLayout`、および`Alignment`の一部、`VerticalOptions`のスロットに子を配置する垂直方向に設定を決定します。

これに示されている、 [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo)サンプルです。

## <a name="frame-and-boxview"></a>フレームと BoxView

これら 2 つの四角形のビューは、プレゼンテーションの目的でよく使用されます。

[ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)四角形の枠など、レイアウトは、別のビューが表示`StackLayout`です。 `Frame` 継承、 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティから[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)内で表示するビューに設定した、`Frame`です。 `Frame`は既定では透過的です。 フレームの外観をカスタマイズする次の 3 つのプロパティを設定します。

- [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/)プロパティを表示します。 設定するが一般的`OutlineColor`に`Color.Accent`基になる画面の配色がわからない場合。
- [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)プロパティに設定することができます`true`iOS デバイスを黒の影を表示します。
- 設定、 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)プロパティを`Thickness`コンテンツのフレームとフレームの間に空白のままにする値。 既定値は、すべての側面に 20 の単位です。

`Frame`既定値あり`HorizontalOptions`と`VerticalOptions`の値`LayoutOptions.Fill`、つまり、`Frame`コンテナーを入力します。 他の設定のサイズと、`Frame`コンテンツのサイズに基づきます。

`Frame`に示されているが、 [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText)サンプルです。

[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)によって指定された色の四角形の領域を表示、 [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/)プロパティです。

場合、`BoxView`が制約 (その`HorizontalOptions`と`VerticalOptions`プロパティの既定の設定がある`LayoutOptions.Fill`) では、`BoxView`の使用可能な領域を塗りつぶします。 場合、`BoxView`制約がありません (で`HorizontalOptions`と`LayoutOptions`の設定`Start`、 `Center`、または`End`)、40 単位正方形の既定のディメンションがあります。 A `BoxView` 1 つのディメンション内で制限して、他の制約します。

設定する多くの場合、 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)と[ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)プロパティの`BoxView`特定のサイズを指定します。 によってこの説明は、 [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView)サンプルです。

複数のインスタンスを使用する`StackLayout`を結合する、`BoxView`といくつか`Label`のインスタンスにある、`Frame`を特定の色を表示し、次に、これらの各ビューで、`StackLayout`で、`ScrollView`魅力的なを作成するには表示される色の一覧、 [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks)サンプル。

[![色のブロックのトリプル スクリーン ショット](images/ch04fg11-small.png "一覧の色")](images/ch04fg11-large.png#lightbox "色の一覧")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout ScrollView しますか。

配置すること、`StackLayout`で、`ScrollView`は一般的では、配置、`ScrollView`で、`StackLayout`も便利な場合がします。 理論上は、考えられるはずため垂直方向の子`StackLayout`垂直方向に制約付きではありません。 `ScrollView`垂直方向に制限する必要があります。 これは、必要がある特定の高さスクロールするための子のサイズし、決定できるようにします。

付与することが重要、`ScrollView`の子、 `StackLayout` 、`VerticalOptions`設定`FillAndExpand`です。 これに示されている、 [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat)サンプルです。

**BlackCat**サンプルを定義し、ポータブル クラス ライブラリ (PCL) に埋め込まれているプログラムのリソースにアクセスする方法も示します。 これは共有アセット プロジェクト (Sap) とも実現できますが、プロセスとして、少し複雑な[ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap)サンプルを示します。



## <a name="related-links"></a>関連リンク

- [第 4 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [第 4 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [第 4 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
