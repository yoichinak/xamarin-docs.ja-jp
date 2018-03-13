---
title: "14 章の概要です。 絶対レイアウト"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 394e1722c79bac5f034e9ad88eb1fed7e5090f8c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>14 章の概要です。 絶対レイアウト

同様に`StackLayout`、 [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)から派生した`Layout<View>`を継承し、`Children`プロパティです。 `AbsoluteLayout` 子と、必要に応じて、そのサイズの位置を指定するプログラマを必要とするレイアウト システムを実装します。 左上隅を基準とした子の左上隅で、位置が指定された、`AbsoluteLayout`デバイスに依存しない単位。 `AbsoluteLayout` 比例位置とサイズ変更機能を実装します。

`AbsoluteLayout` プログラマが子のサイズを課すことができる場合にのみ使用される特殊なレイアウト システムとして見なす必要があります (たとえば、`BoxView`要素) と、要素のサイズには影響しません他の子の位置またはします。 `HorizontalOptions`と`VerticalOptions`プロパティの子に影響を与えるありません、`AbsoluteLayout`です。

この章では、重要な機能も導入されて*バインド可能なプロパティを添付*1 つのクラスで定義されたプロパティを許可する (ここでは`AbsoluteLayout`) 別のクラスにアタッチされている (の子、 `AbsoluteLayout`)。

## <a name="absolutelayout-in-code"></a>コードで AbsoluteLayout

子を追加することができます、`Children`のコレクション、`AbsoluteLayout`標準を使用して[ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/)メソッドが、`AbsoluteLayout`拡張も提供[ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/)メソッドを指定できる、 [ `Rectangle`](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/)です。 別[ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/)メソッドにのみ必要ですが、 [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/)、その場合は、子は制約がなく、それ自体のサイズを設定します。

作成することができます、`Rectangle`値と、[コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/)を必要とする 4 つの値 & #x 2014 ですその親の子の左上隅の位置を示す最初の 2 つとを示す 2 つ目の 2 つ、。子のサイズ。 使用することができます、[コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/)を必要とする、`Point`と[ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)値。

これら`Add`で方法が示されて[ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo)、どの位置`BoxView`を使用して要素`Rectangle`値、および`Label`だけを使用して要素`Point`値。

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed)サンプルは 32`BoxView`野駒パターンを作成する要素。 プログラムにより、 `BoxView` 35 単位正方形のハードコードされた要素のサイズ。 `AbsoluteLayout`がその`HorizontalOptions`と`VerticalOptions`に設定`LayoutOptions.Center`、これにより、 `AbsoluteLayout` 280 単位正方形の合計サイズ。

## <a name="attached-bindable-properties"></a>接続されているバインド可能なプロパティ

位置と、必要に応じて、子のサイズを設定することも、`AbsoluteLayout`に追加された後、`Children`静的メソッドを使用してコレクション[ `AbsoluteLayout.SetLayoutBounds`](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/)です。 最初の引数は、子です。2 番目は、`Rectangle`オブジェクト。 子のサイズ自体水平方向または垂直方向に幅と高さの値に設定して指定できます、 [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/)定数。

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) puts のサンプル、`AbsoluteLayout`で、`ContentView`で、`SizeChanged`を呼び出してハンドラー`AbsoluteLayout.SetLayoutBounds`可能な限り大きくようにすべての子にします。  

接続されているバインド可能なプロパティを`AbsoluteLayout`定義は、型の静的な読み取り専用フィールド`BindableProperty`という名前[ `AbsoluteLayout.LayoutBoundsProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/)です。 静的な`AbsoluteLayout.SetLayoutBounds`メソッドを呼び出すことによって実装`SetValue`を持つ子で、`AbsoluteLayout.LayoutBoundsProperty`です。 子には、接続されているバインド可能なプロパティとその値を格納するディクショナリが含まれています。 レイアウト中に、`AbsoluteLayout`呼び出すことによって、その値を取得できます[ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/)、と一緒に実装される`GetValue`呼び出します。

## <a name="proportional-sizing-and-positioning"></a>プロポーショナル サイズと配置

`AbsoluteLayout` プロポーショナル サイズと機能を配置を実装します。 クラスには、2 つ目の接続されているバインド可能なプロパティが定義されて[ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/)、関連する静的メソッドとともに[ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/)と[ `AbsoluteLayout.GetLayoutFlags`](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/)です。

引数に`AbsoluteLayout.SetLayoutFlags`され、戻り値の`AbsoluteLayout.GetLayoutFlags`型の値は、 [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/)、次のメンバーを持つ列挙。

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (0 以上)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

C# でビットごとの OR 演算子でこれらを組み合わせることができます。

これらのフラグを設定の特定のプロパティで、`Rectangle`の位置し、子のサイズを使用するレイアウト境界構造は比例して解釈されます。

ときに、`WidthProportional`フラグが設定されて、`Width`値 1 は、子は、幅と同じことを意味、`AbsoluteLayout`です。 高さを同様のアプローチが使用されます。

プロポーショナル配置では、サイズは考慮します。 ときに、`XProportional`フラグが設定されて、`X`のプロパティ、`Rectangle`レイアウト境界は比例します。 左端の位置が 0 の場合は、子の左端の値、 `AbsoluteLayout`、ですが、位置 1 の場合、子の右エッジの右端に配置されていること、`AbsoluteLayout`の右端を超えないように、 `AbsoluteLayout` expec ことt です。 `X` 0.5 のプロパティの中心が水平方向の子、`AbsoluteLayout`です。

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional)比例サイズと配置の使用を示します。

## <a name="working-with-proportional-coordinates"></a>プロポーショナル座標の操作

プロポーショナルでの実装方法とは異なる位置のほうが簡単なことがあります、`AbsoluteLayout`です。 プロポーショナル座標を使用して作業を好む場合があります場所、 `X` 1 のプロパティの右エッジからの (右の端ではなく)、子の左エッジの位置、`AbsoluteLayout`です。

この代替位置指定スキームを呼び出すことができる「小数部の子の座標です」 小数部の子座標からの必要なレイアウト境界に変換できます`AbsoluteLayout`次の数式を使用します。

layoutBounds.X = (fractionalChildCoordinate.X/(1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y/(1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc)サンプルを示します。

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout と XAML

使用することができます、 `AbsoluteLayout` XAML での子に接続されているバインド可能なプロパティを設定し、`AbsoluteLayout`の属性値を使用して`AbsoluteLayout.LayoutBounds`と`AbsoluteLayout.LayoutFlags`です。 これに示されている、 [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo)と[ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml)サンプルです。 後者のプログラムを含む 32`BoxView`要素似ていますが、暗黙的な`Style`が含まれている、`AbsoluteLayout.LayoutFlags`最低までマークアップを格納するプロパティです。

クラス名、ドット、およびプロパティ名で構成される XAML 内の属性は*常に*添付バインド可能なプロパティです。

## <a name="overlays"></a>オーバーレイ

使用することができます`AbsoluteLayout`構築するために、*オーバーレイ*、解説している他のコントロールと、ページなどと、ページ上の標準コントロールとの対話ユーザーを保護します。 

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay)サンプルは、この手法を実証し、を示しています、 [ `ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)プログラムが完了するエクステントが表示されますが、タスク。

## <a name="some-fun"></a>おもしろい

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock)サンプルのシミュレートされた 5 x 7 ドット マトリックスの表示と、現在の時刻が表示されます。 各ドットは、 `BoxView` (がそれらの 228) に配置されているサイズ調整され、`AbsoluteLayout`です。

[![ドット マトリックス クロックのスクリーン ショットをトリプル](images/ch14fg08-small.png "ドット マトリックス クロック")](images/ch14fg08-large.png#lightbox "ドット マトリックス クロック")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText)プログラムは 2 つのアニメーション化`Label`画面全体で水平方向および垂直方向のバウンドするオブジェクト。



## <a name="related-links"></a>関連リンク

- [14 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [14 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
