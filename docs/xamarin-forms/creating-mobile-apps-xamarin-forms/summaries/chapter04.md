---
title: 第 4 章の概要です。 スタックをスクロール
description: Xamarin.Forms によるモバイル アプリの作成。第 4 章の概要です。 スタックをスクロール
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 87846eba71278295ae6f266f6e786c0992aebd34
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334598"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>第 4 章の概要です。 スタックをスクロール

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

この章は主の概念を導入する有用なデータ*レイアウト*、クラスおよび Xamarin.Forms を使用して、ページ上の複数のビューの表示画面を整理する方法の全体的な用語であります。

レイアウトは、いくつかのクラスから派生した[ `Layout` ](xref:Xamarin.Forms.Layout)と[ `Layout<T>`](xref:Xamarin.Forms.Layout`1)します。 この章で[ `StackLayout`](xref:Xamarin.Forms.StackLayout)します。

> [!NOTE]
> [ `FlexLayout` ](~/xamarin-forms/user-interface/layouts/flex-layout.md)で導入された Xamarin.Forms 3.0 に類似した方法で使用できます`StackLayout`がより柔軟です。

この章でも導入されましたが、 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)、 [ `Frame` ](xref:Xamarin.Forms.Frame)、および[ `BoxView` ](xref:Xamarin.Forms.BoxView)クラス。

## <a name="stacks-of-views"></a>ビューのスタック

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 派生した`Layout<View>`を継承し、 [ `Children` ](xref:Xamarin.Forms.Layout`1)型のプロパティ`IList<View>`します。 このコレクションに複数のビュー項目を追加して`StackLayout`水平または垂直方向のスタックに表示されます。

設定、 [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation)プロパティの`StackLayout`のメンバーに、 [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation)列挙型でいずれか[ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical)または[`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). 既定値は `Vertical` です。

設定、 [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing)プロパティの`StackLayout`を`double`子間の間隔を指定する値。 既定値は、6 です。

コードでは、項目を追加することができます、`Children`のコレクション`StackLayout`で、`for`または`foreach`ループで示した、 [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop)またはサンプルについては、初期化、`Children`のとおり、個々 のビューの一覧でコレクション[ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList)します。 子がから派生する必要があります`View`他を含めることができますが、`StackLayout`オブジェクト。

## <a name="scrolling-content"></a>コンテンツをスクロール

場合、 `StackLayout`  ページでは、表示するが多すぎるの子を含む配置することができます、`StackLayout`で、 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)スクロールを許可します。

設定、 [ `Content` ](xref:Xamarin.Forms.ScrollView.Content)プロパティの`ScrollView`をスクロールするビュー。 これは、多くの場合、`StackLayout`が任意のビューができます。

設定、 [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation)プロパティの`ScrollView`のメンバーに、 [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation)プロパティ、 [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical)、 [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal)、または[ `Both`](xref:Xamarin.Forms.ScrollOrientation.Both)します。 既定値は `Vertical` です。 場合のコンテンツを`ScrollView`は、 `StackLayout`、2 つの方向を一貫性のあるにする必要があります。

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors)サンプルの使用を示します`ScrollView`と`StackLayout`使用可能な色を表示します。 .NET リフレクションを使用して、すべてのパブリック静的プロパティおよびのフィールドを取得する方法も示します、`Color`構造を明示的にリストアップする必要はありません。

## <a name="the-expands-option"></a>展開オプション

ときに、`StackLayout`スタック全体の高さ内の特定のスロットを占有するその子では、それぞれの子、`StackLayout`子のサイズの設定に依存するその`HorizontalOptions`と`VerticalOptions`プロパティ。 これらのプロパティには型の値が割り当てられている[ `LayoutOptions`](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/)します。

`LayoutOptions`構造体が 2 つのプロパティを定義します。

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) 列挙型の[ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) 4 つのメンバーを持つ[ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start)、 [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center)、 [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End)と [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 型の `bool`

便宜上、`LayoutOptions`構造体は、型の 8 つ静的読み取り専用のフィールドも定義されています。 `LayoutOptions` 、2 つのインスタンスのプロパティのすべての組み合わせを包含します。

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

次の説明では、`StackLayout`縦方向の既定値。 水平`StackLayout`は類似しています。

垂直`StackLayout`、`HorizontalOptions`の幅内で子を配置する水平方向に設定を決定します、`StackLayout`します。 `Alignment`設定`Start`、 `Center`、または`End`と水平方向に制約する子。 子は、独自の幅を決定し、は、左、中央、またはの右側に配置、`StackLayout`します。 `Fill`オプションによって制約される水平方向に子との幅いっぱいになる、`StackLayout`します。

垂直`StackLayout`、それぞれの子は、垂直方向には制約がなく、取得垂直スロットによって子の高さ、後者の`VerticalOptions`設定は関係ありません。

場合、垂直`StackLayout`自体は制約がありません&mdash;は場合その`VerticalOptions`設定は`Start`、 `Center`、または`End`の高さから、`StackLayout`その子の高さの合計。

ただし場合、垂直`StackLayout`が垂直方向に制約&mdash;場合その`VerticalOptions`設定は`Fill`&mdash;の高さから、`StackLayout`合計を超える可能性のあるコンテナーの高さになりますその子の高さ。 場合は、少なくとも 1 つの子とする場合は、`VerticalOptions`設定で、`Expands`のフラグ`true`で追加の領域では、`StackLayout`でこれらすべての子の間で均等に割り当てられている、`Expands`フラグ`true`。 高さが、子全体の高さに等しく、 `StackLayout`、および`Alignment`の一部、`VerticalOptions`のスロットに子を配置する垂直方向に設定を決定します。

これは、方法については、 [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo)サンプル。

## <a name="frame-and-boxview"></a>フレームと BoxView

これら 2 つの四角形のビューは、プレゼンテーションの目的でよく使用されます。

[ `Frame` ](xref:Xamarin.Forms.Frame)など、レイアウトを指定できますが、別のビューを囲む四角形の枠が表示`StackLayout`します。 `Frame` 継承、 [ `Content` ](xref:Xamarin.Forms.ContentView.Content)プロパティから[ `ContentView` ](xref:Xamarin.Forms.ContentView)内に表示するビューに設定した、`Frame`します。 `Frame`は既定でに対して透過的です。 フレームの外観をカスタマイズする次の 3 つのプロパティを設定します。

- [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor)プロパティを表示します。 設定するが一般的`OutlineColor`に`Color.Accent`基になる配色パターンがわからない場合。
- [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow)にプロパティを設定することができます`true`を iOS デバイスで黒の影を表示します。
- 設定、 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)プロパティを`Thickness`コンテンツのフレームとフレームの間に空白のままにする値。 既定値は、辺すべてにつき 20 単位です。

`Frame`既定値あり`HorizontalOptions`と`VerticalOptions`の値`LayoutOptions.Fill`、です。 つまり、`Frame`コンテナーを入力します。 サイズは、他の設定で、`Frame`コンテンツのサイズに基づきます。

`Frame`方法については、 [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText)サンプル。

[ `BoxView` ](xref:Xamarin.Forms.BoxView)で指定された色の四角形の領域を表示します。 その[ `Color` ](xref:Xamarin.Forms.BoxView.Color)プロパティ。

場合、`BoxView`制限されます (その`HorizontalOptions`と`VerticalOptions`プロパティの既定の設定がある`LayoutOptions.Fill`)、`BoxView`の使用可能な領域を塗りつぶします。 場合、`BoxView`は制約がありません (で`HorizontalOptions`と`LayoutOptions`設定の`Start`、 `Center`、または`End`)、40 単位正方形の既定のディメンションがあります。 A `BoxView` 1 つのディメンションでは制限し、制約、それ以外のことができます。

多くの場合、設定します、 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)と[ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティの`BoxView`を特定のサイズを付けます。 これには、 [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView)サンプル。

複数のインスタンスを使用することができます`StackLayout`を結合する、`BoxView`といくつか`Label`インスタンス、`Frame`に特定の色を表示し、これらの各ビューで、`StackLayout`で、`ScrollView`魅力的なものを作成するには表示される色の一覧、 [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks)サンプル。

[![色のブロックの 3 倍になるスクリーン ショット](images/ch04fg11-small.png "一覧の色")](images/ch04fg11-large.png#lightbox "色の一覧")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout 内で ScrollView でしょうか。

配置すること、`StackLayout`で、`ScrollView`は、一般的な配置、`ScrollView`で、`StackLayout`も便利な場合があります。 理論上は、考えられるはずため、垂直方向の子`StackLayout`垂直方向に制約付きではありません。 `ScrollView`垂直方向に制限する必要があります。 必要があります指定する必要が特定の高さのスクロールの子のサイズを確認ができるようにします。

これには、`ScrollView`の子、 `StackLayout` 、`VerticalOptions`の設定`FillAndExpand`します。 これは、方法については、 [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat)サンプル。

**BlackCat**サンプルも定義して、共有ライブラリに埋め込まれているプログラムのリソースにアクセスする方法を示します。 共有資産プロジェクト (Sap) とこれに行うこともできますが、プロセスは少し複雑で、として、 [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap)サンプルを示します。



## <a name="related-links"></a>関連リンク

- [第 4 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [第 4 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [第 4 章F#サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
