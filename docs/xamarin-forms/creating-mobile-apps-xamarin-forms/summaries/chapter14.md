---
title: 第 14 章の概要です。 絶対レイアウト
description: Xamarin.Forms によるモバイル アプリの作成。第 14 章の概要です。 絶対レイアウト
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 7371f134944d7492e51aa2d02247c0ab48345a47
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334413"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>第 14 章の概要です。 絶対レイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

ような`StackLayout`、 [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout)から派生した`Layout<View>`を継承し、`Children`プロパティ。 `AbsoluteLayout` その子と、必要に応じて、そのサイズの位置を指定するプログラマが必要なレイアウト システムを実装します。 左上隅に対して相対的子の左上隅で、位置が指定された、`AbsoluteLayout`デバイスに依存しない単位。 `AbsoluteLayout` 比例位置とサイズ変更機能を実装します。

`AbsoluteLayout` プログラマは、子のサイズをかけることがときにのみ使用される特殊なレイアウト システムとして見なす必要があります (たとえば、`BoxView`要素) または要素のサイズが影響を及ぼす場合他の子の位置します。 `HorizontalOptions`と`VerticalOptions`の子にプロパティの影響がない、`AbsoluteLayout`します。

この章は、の重要な機能も導入されています。*添付プロパティのバインド可能な*1 つのクラスで定義されたプロパティを使用できる (ここで`AbsoluteLayout`) 別のクラスに接続する (の子、 `AbsoluteLayout`)。

## <a name="absolutelayout-in-code"></a>コードで AbsoluteLayout

子を追加することができます、`Children`のコレクション、`AbsoluteLayout`標準を使用して[ `Add` ](xref:System.Collections.Generic.ICollection`1.Add*)メソッドが`AbsoluteLayout`も拡張を提供します[ `Add` ](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*)メソッドを指定することができます、 [ `Rectangle`](xref:Xamarin.Forms.Rectangle)します。 別[ `Add` ](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*)メソッドにのみが必要です、 [ `Point` ](xref:Xamarin.Forms.Point)、この場合、子は制約がありませんされ自体のサイズを設定します。

作成することができます、`Rectangle`値を[コンス トラクター](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)) 4 つの値が必要な&mdash;親に対する相対的な子の左上隅の位置を示す最初の 2 つとを示す 2 つ目の 2 つ、子のサイズ。 使用することができます、[コンス トラクター](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size))を必要とする、`Point`と[ `Size` ](xref:Xamarin.Forms.Size)値。

これら`Add`でメソッドが示されています[ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo)、どの位置`BoxView`を使用して要素`Rectangle`値、および`Label`だけを使用して要素`Point`値。

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) 32 を使用して`BoxView`野駒パターンを作成する要素。 プログラムでは、 `BoxView` 35 単位正方形のハードコーディングされた要素のサイズ。 `AbsoluteLayout`がその`HorizontalOptions`と`VerticalOptions`に設定`LayoutOptions.Center`、原因となる、 `AbsoluteLayout` 280 単位正方形の合計サイズ。

## <a name="attached-bindable-properties"></a>バインド可能なプロパティのアタッチ

位置と、必要に応じて、子のサイズを設定することも、`AbsoluteLayout`に追加した後、`Children`静的メソッドを使用してコレクション[ `AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle))します。 最初の引数は、子;2 つ目は、`Rectangle`オブジェクト。 子のサイズ自体水平方向または垂直方向を幅と高さの値に設定を指定することができます、 [ `AbsoluteLayout.AutoSize` ](xref:Xamarin.Forms.AbsoluteLayout.AutoSize)定数。

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) put のサンプル、`AbsoluteLayout`で、`ContentView`で、`SizeChanged`に呼び出すハンドラー`AbsoluteLayout.SetLayoutBounds`可能な限り大きくさせることのすべての子にします。  

添付のバインド可能なプロパティを`AbsoluteLayout`定義型の静的な読み取り専用フィールドは、`BindableProperty`という名前の[ `AbsoluteLayout.LayoutBoundsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)します。 静的な`AbsoluteLayout.SetLayoutBounds`メソッドを呼び出すことによって実装`SetValue`を持つ子で、`AbsoluteLayout.LayoutBoundsProperty`します。 子には、接続されているバインド可能なプロパティとその値を格納するディクショナリが含まれています。 レイアウト中に、`AbsoluteLayout`呼び出すことによって、その値を取得できます[ `AbsoluteLayout.GetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject))、と一緒に実装される`GetValue`呼び出します。

## <a name="proportional-sizing-and-positioning"></a>比例サイズと配置

`AbsoluteLayout` 比例サイズ設定と機能の位置を実装します。 クラスは、2 番目の接続されているバインド可能なプロパティを定義します。 [ `LayoutFlagsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)、関連する静的メソッドで[ `AbsoluteLayout.SetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags))と[ `AbsoluteLayout.GetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject))します。

引数に`AbsoluteLayout.SetLayoutFlags`と戻り値の`AbsoluteLayout.GetLayoutFlags`型の値は、 [ `AbsoluteLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayoutFlags)、次のメンバーを持つ列挙体。

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (0 に等しい)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

C# でビットごとの OR 演算子でこれらを組み合わせることができます。

これらのフラグが設定の特定のプロパティと、`Rectangle`の位置し、サイズ、子に使用するレイアウト境界構造がそれに比例して解釈されます。

ときに、`WidthProportional`フラグが設定されて、`Width`値 1 は、子は、幅と同じことを意味、`AbsoluteLayout`します。 高さの同様のアプローチが使用されます。

プロポーショナルの配置では、サイズは考慮されます。 ときに、`XProportional`フラグが設定されて、`X`のプロパティ、`Rectangle`レイアウト境界は比例します。 左端の位置が、値が 0 の場合、子の左エッジのこと、 `AbsoluteLayout`、位置 1 は、子の右端がの右端に配置されていることが、`AbsoluteLayout`の右端を超えないように、 `AbsoluteLayout` expec 場合と同様ですt です。 `X` 0.5 のプロパティが水平方向の子を中央揃え、`AbsoluteLayout`します。

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional)比例サイズと配置の使用を示します。

## <a name="working-with-proportional-coordinates"></a>比例座標の操作

簡単に比例して配置での実装方法とは異なるものと考えることがあります、`AbsoluteLayout`します。 プロポーショナルの座標を使用することができます、 `X` 1 のプロパティの右エッジに対して子の左端 (右端ではなく) を配置、`AbsoluteLayout`します。

この代替配置スキームに「子の小数部の座標です」を呼び出すことができます。 小数部の子の座標から必要なレイアウト境界に変換することができます`AbsoluteLayout`次の数式を使用します。

layoutBounds.X = (fractionalChildCoordinate.X/(1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y/(1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc)のサンプルで例示します。

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout と XAML

使用することができます、 `AbsoluteLayout` XAML での子に接続されているバインド可能なプロパティを設定し、`AbsoluteLayout`の属性値を使用して`AbsoluteLayout.LayoutBounds`と`AbsoluteLayout.LayoutFlags`します。 これは、方法については、 [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo)と[ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml)サンプル。 後者のプログラムが 32 を含む`BoxView`要素が、暗黙的な`Style`を含む、`AbsoluteLayout.LayoutFlags`最小限までマークアップを保持するプロパティ。

クラス名、ドット、およびプロパティ名で構成される XAML の属性は、*常に*添付のバインド可能なプロパティ。

## <a name="overlays"></a>オーバーレイ

使用することができます`AbsoluteLayout`を構築する、*オーバーレイ*から、ページ上の標準コントロールと対話するユーザーを保護する他のコントロールを使用してページをおそらくカバーします。

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay)の例は、この手法を実証し、方法も示しています、 [ `ProgressBar`](xref:Xamarin.Forms.ProgressBar)プログラムが完了するエクステントが表示されますが、タスク。

## <a name="some-fun"></a>楽しむ

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock)サンプルは、シミュレートされた 5 x 7 ドット マトリックス ディスプレイを使用した現在の時刻を表示します。 各ドットは、 `BoxView` (はそれらの 228) のサイズに配置されていると、`AbsoluteLayout`します。

[![ドット マトリックス クロックのスクリーン ショットをトリプル](images/ch14fg08-small.png "ドット マトリックス クロック")](images/ch14fg08-large.png#lightbox "ドット マトリックス クロック")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText)プログラムが 2 つをアニメーション化`Label`画面全体で水平方向および垂直方向にするオブジェクト。



## <a name="related-links"></a>関連リンク

- [第 14 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [第 14 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
