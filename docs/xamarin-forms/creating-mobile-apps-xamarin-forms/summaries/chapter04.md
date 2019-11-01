---
title: 第4章の概要。 スタックのスクロール
description: Xamarin を使用した Mobile Apps の作成:第4章の概要。 スタックのスクロール
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bda9d5cb323524981bed9c3bb55998513dd69aab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032873"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>第4章の概要。 スタックのスクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

この章では、主に*レイアウト*の概念を紹介します。これは、ページ上の複数のビューのビジュアル表示を整理するために Xamarin が使用するクラスと手法の全体的な用語です。

レイアウトには、 [`Layout`](xref:Xamarin.Forms.Layout)と[`Layout<T>`](xref:Xamarin.Forms.Layout`1)から派生した複数のクラスが含まれます。 この章では、 [`StackLayout`](xref:Xamarin.Forms.StackLayout)について説明します。

> [!NOTE]
> Xamarin で導入された[`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md)は、`StackLayout` に似ていますが柔軟性の高い方法で使用できます3.0。

また、この章では、 [`ScrollView`](xref:Xamarin.Forms.ScrollView)、 [`Frame`](xref:Xamarin.Forms.Frame)、および[`BoxView`](xref:Xamarin.Forms.BoxView)の各クラスについても説明しました。

## <a name="stacks-of-views"></a>ビューのスタック

[`StackLayout`](xref:Xamarin.Forms.StackLayout)は `Layout<View>` から派生し、型 `IList<View>`の[`Children`](xref:Xamarin.Forms.Layout`1)プロパティを継承します。 複数のビューアイテムをこのコレクションに追加すると、`StackLayout` は水平または垂直のスタックに表示されます。

`StackLayout` の[`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)プロパティを[`StackOrientation`](xref:Xamarin.Forms.StackOrientation)列挙体のメンバー ( [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical)または[`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)のいずれかに設定します。 既定値は、`Vertical` です。

`StackLayout` の[`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)プロパティを `double` 値に設定して、子の間隔を指定します。 既定値は6です。

コードでは、 [**colorloop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop)サンプルで示すように `for` または `foreach` ループ内の `StackLayout` の `Children` コレクションに項目を追加できます。または、「colorloop」で説明されているように、個々のビューの一覧を使用して、`Children` コレクションを初期化できます。 [**ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). 子は `View` から派生する必要がありますが、他の `StackLayout` オブジェクトを含めることができます。

## <a name="scrolling-content"></a>スクロール (コンテンツを)

`StackLayout` に含まれる子が多すぎてページに表示できない場合は、`StackLayout` を[`ScrollView`](xref:Xamarin.Forms.ScrollView)に配置してスクロールできるようにすることができます。

`ScrollView` の [ [`Content`](xref:Xamarin.Forms.ScrollView.Content) ] プロパティに、スクロールするビューを設定します。 これは多くの場合 `StackLayout`ですが、どのようなビューでもかまいません。

`ScrollView` の[`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation)プロパティを[`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation)プロパティ、 [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical)、 [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal)、または[`Both`](xref:Xamarin.Forms.ScrollOrientation.Both)のメンバーに設定します。 既定値は、`Vertical` です。 `ScrollView` の内容が `StackLayout`の場合は、2つの方向が一致している必要があります。

[**ReflectedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors)サンプルでは、使用可能な色を表示するための `ScrollView` と `StackLayout` の使用方法を示します。 このサンプルでは、.NET リフレクションを使用して、明示的に一覧表示せずに `Color` 構造体のすべてのパブリックな静的プロパティとフィールドを取得する方法も示します。

## <a name="the-expands-option"></a>展開オプション

`StackLayout` によってその子がスタックされると、各子は、子のサイズとその `HorizontalOptions` および `VerticalOptions` プロパティの設定に応じて、`StackLayout` の合計の高さ内の特定のスロットを占有します。 これらのプロパティには[`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)型の値が割り当てられます。

`LayoutOptions` 構造体は、次の2つのプロパティを定義します。

- 4つのメンバー、 [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)、 [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center)、 [`End`](xref:Xamarin.Forms.LayoutAlignment.End)、および[`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)を持つ列挙型の[`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment)の[`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment)
- `bool` 型の [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)

利便性を高めるために、`LayoutOptions` 構造体では、次の2つのインスタンスのプロパティのすべての組み合わせを含む、`LayoutOptions` 型の8つの静的な読み取り専用フィールドも定義します。

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

次の説明では、既定の垂直方向の `StackLayout` について説明します。 水平 `StackLayout` は似ています。

垂直 `StackLayout`の場合、`HorizontalOptions` の設定によって、`StackLayout`の幅内での子の水平方向の配置方法が決まります。 `Start`、`Center`、または `End` の `Alignment` 設定によって、子は水平方向に制約されません。 子は独自の幅を決定し、`StackLayout`の左、中央、または右に配置されます。 `Fill` オプションを使用すると、子は水平方向に制限され、`StackLayout`の幅になります。

垂直 `StackLayout`の場合、各子は垂直方向に制約されません。また、子の高さに応じて垂直方向のスロットを取得します。この場合、`VerticalOptions` の設定は関係ありません。

垂直 `StackLayout`、その `VerticalOptions` 設定が `Start`、`Center`、または `End`である場合に、垂直方向の&mdash;に制約がない場合、`StackLayout` の高さはその子の合計の高さになります。

ただし、垂直 `StackLayout` が垂直方向に制限されている場合は&mdash;`VerticalOptions` 設定が&mdash;`Fill`場合は、`StackLayout` の高さがそのコンテナーの高さになります。これは、その子の高さの合計よりも大きくなる可能性があります。 その場合、少なくとも1つの子が `Expands` フラグが `true`の `VerticalOptions` 設定を持っている場合、`StackLayout` 内の余分な領域は `Expands` フラグが `true`のすべての子に均等に割り当てられます。 その後、子の高さの合計が `StackLayout`の高さと等しくなり、`VerticalOptions` 設定の `Alignment` の部分によって、そのスロットにおける子の垂直方向の位置が決定されます。

これについては、「[**垂直オプションのデモ**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo)サンプル」で説明しています。

## <a name="frame-and-boxview"></a>フレームと BoxView

これら2つの四角形のビューは、プレゼンテーション目的でよく使用されます。

[`Frame`](xref:Xamarin.Forms.Frame)ビューには、別のビューの周りに四角形の枠が表示されます。これは、`StackLayout`などのレイアウトにすることができます。 `Frame` は、`Frame`内に表示されるビューに設定した[`ContentView`](xref:Xamarin.Forms.ContentView)から[`Content`](xref:Xamarin.Forms.ContentView.Content)プロパティを継承します。 既定では、`Frame` は透過的です。 フレームの外観をカスタマイズするには、次の3つのプロパティを設定します。

- [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor)プロパティは、表示されるようにします。 基になる配色がわからない場合は、`OutlineColor` を `Color.Accent` に設定するのが一般的です。
- [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)プロパティを `true` に設定すると、iOS デバイスに黒い影が表示されます。
- [ [`Padding`](xref:Xamarin.Forms.Layout.Padding) ] プロパティを `Thickness` の値に設定して、フレームとフレームのコンテンツの間にスペースを残します。 既定値は、すべての辺で20単位です。

`Frame` には `LayoutOptions.Fill`の既定の `HorizontalOptions` と `VerticalOptions` 値があります。これは、`Frame` によってコンテナーがいっぱいになることを意味します。 その他の設定では、`Frame` のサイズはそのコンテンツのサイズに基づいています。

`Frame` は、[**フレーム**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText)のサンプルに示されています。

[`BoxView`](xref:Xamarin.Forms.BoxView)には、 [`Color`](xref:Xamarin.Forms.BoxView.Color)プロパティによって指定された色の四角形の領域が表示されます。

`BoxView` が制約されている場合 (`HorizontalOptions` と `VerticalOptions` プロパティの既定の設定は `LayoutOptions.Fill`)、`BoxView` で使用可能な領域がいっぱいになります。 `BoxView` が制約なし (`Start`、`Center`、または `End`の `HorizontalOptions` と `LayoutOptions` の設定で) の場合、既定のディメンションは40単位の四角形になります。 `BoxView` は、1つのディメンションで制約でき、もう一方の次元では制約を受けることはできません。

多くの場合、`BoxView` の[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)と[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)のプロパティを設定して、特定のサイズを指定します。 これは、 [**Sizedboxview**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView)サンプルで示されています。

`StackLayout` の複数のインスタンスを使用して、`Frame` 内の `BoxView` と複数の `Label` インスタンスを組み合わせて特定の色を表示し、これらの各ビューを `StackLayout` の `ScrollView` に配置して、魅力的な色のリストを作成することができます。[**Colorblocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks)サンプルに次のように表示されます。

[![カラーブロックのトリプルスクリーンショット](images/ch04fg11-small.png "色の一覧")](images/ch04fg11-large.png#lightbox "色の一覧")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout の ScrollView ですか。

`ScrollView` に `StackLayout` を配置することは一般的ですが、`ScrollView` を `StackLayout` に配置することも便利です。 理論的には、垂直 `StackLayout` の子には垂直方向の制約がないため、これを行うことはできません。 ただし、`ScrollView` は垂直方向に制限されている必要があります。 スクロール用の子のサイズを決定できるように、特定の高さを指定する必要があります。

秘訣は、`StackLayout` の `ScrollView` 子に `FillAndExpand`の `VerticalOptions` 設定を与えることです。 これは、 [**Blackcat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat)サンプルに示されています。

**Blackcat**サンプルは、共有ライブラリに埋め込まれているプログラムリソースを定義してアクセスする方法も示しています。 これは、共有アセットプロジェクト (Sap) でも実現できますが、 [**Blackcatsap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap)サンプルで示すように、プロセスは少し複雑になります。

## <a name="related-links"></a>関連リンク

- [第4章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Chapter 4 のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Chapter 4 F#のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
