---
title: '第 14 章の概要: 絶対レイアウト'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 14 章の概要: 絶対レイアウト'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 09011647428e2af1bdfcdb2f9def8da64ef84144
ms.sourcegitcommit: bda07768b51e6d24f897ccae16e152dc7a83effa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96039114"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>第 14 章の概要: 絶対レイアウト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

> [!NOTE]
> この本は 2016 年春に発行されて以降、改訂されていません。 多くの情報はまだ価値がありますが、一部の資料は古くなっており、トピックの中にはまったく正しくないものまたは不完全なものもあります。

`StackLayout` と同様に、[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) は `Layout<View>` から派生し、`Children` プロパティを継承します。 `AbsoluteLayout` によって実装されるレイアウト システムでは、プログラマーがその子の位置と、必要に応じてサイズを指定する必要があります。 位置は、デバイスに依存しない単位で、`AbsoluteLayout` の左上隅を基準とする子の左上隅の位置によって指定されます。 `AbsoluteLayout` では、比例の配置とサイズ変更の機能も実装されています。

`AbsoluteLayout` は、プログラマーが子 (たとえば `BoxView` 要素) のサイズを設定できる場合、または要素のサイズが他の子の位置に影響を与えない場合にのみ使用される、特別な目的のレイアウト システムと見なす必要があります。 `HorizontalOptions` プロパティと `VerticalOptions` プロパティは、`AbsoluteLayout` の子には影響しません。

また、この章では、重要な機能である "*アタッチされたバインド可能なプロパティ*" についても説明します。これを使用すると、あるクラス (このケースでは `AbsoluteLayout`) で定義されているプロパティを、別のクラス (`AbsoluteLayout` の子) にアタッチすることができます。

## <a name="absolutelayout-in-code"></a>コードにおける AbsoluteLayout

標準の [`Add`](xref:System.Collections.Generic.ICollection`1.Add*) メソッドを使用して `AbsoluteLayout` の `Children` コレクションに子を追加することもできますが、`AbsoluteLayout` には、[`Rectangle`](xref:Xamarin.Forms.Rectangle) を指定できる拡張 [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) メソッドも用意されています。 もう 1 つの [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) メソッドには、[`Point`](xref:Xamarin.Forms.Point) のみが必要です。この場合、子は制約されず、それ自体でサイズ設定できます。

4 つの値が必要な[コンストラクター](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double))を使用して、`Rectangle` 値を作成できます。最初の 2 つは親を基準にした子の左上隅の位置を示し、次の 2 つは子のサイズを示します。 または、`Point` と [`Size`](xref:Xamarin.Forms.Size) 値を必要とする [コンストラクター](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)) を使用できます。

これらの `Add` メソッドは、[**AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo) で示されています。そこでは、`BoxView` 要素が `Rectangle` 値を使って配置され、`Label` 要素が `Point` 値のみを使って配置されています。

[**ChessboardFixed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) サンプルでは、32 個の `BoxView` 要素を使用してチェスボードのパターンが作成されています。 このプログラムでは、`BoxView` 要素に対して、ハード コーディングされた 35 単位四方のサイズが指定されています。 `AbsoluteLayout` では、その `HorizontalOptions` と `VerticalOptions` が `LayoutOptions.Center` に設定されています。これにより、`AbsoluteLayout` の合計サイズは 280 単位四方になります。

## <a name="attached-bindable-properties"></a>アタッチされたバインド可能なプロパティ

静的メソッド [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)) を使用して、`AbsoluteLayout` の子の位置と、必要に応じてサイズを、`Children` コレクションに追加した後に設定することもできます。 最初の引数は対象の子です。2 つ目は `Rectangle` オブジェクトです。 幅と高さの値を [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) 定数に設定すると、子がそれ自体で水平方向や垂直方向にサイズ設定するように指定することができます。

[**ChessboardDynamic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) サンプルでは、すべての子で `AbsoluteLayout.SetLayoutBounds` を呼び出してそれらを可能な限り拡大する `SizeChanged` ハンドラーを使って、`ContentView` に `AbsoluteLayout` を設定しています。  

`AbsoluteLayout` で定義されているアタッチされたバインド可能なプロパティは、[`AbsoluteLayout.LayoutBoundsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) という名前の `BindableProperty` 型の静的な読み取り専用フィールドです。 静的な `AbsoluteLayout.SetLayoutBounds` メソッドは、`AbsoluteLayout.LayoutBoundsProperty` を持つ子に対して `SetValue` を呼び出すことによって実装されています。 この子には、アタッチされたバインド可能なプロパティとその値が格納されたディクショナリが含まれています。 レイアウト中は、`GetValue` 呼び出しを使用して実装されている [`AbsoluteLayout.GetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)) を呼び出すことによって、`AbsoluteLayout` がその値を取得できます。

## <a name="proportional-sizing-and-positioning"></a>比例のサイズ変更と配置

`AbsoluteLayout` では、比例のサイズ変更と配置の機能も実装されています。 このクラスでは、関連する静的メソッド [`AbsoluteLayout.SetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) および [`AbsoluteLayout.GetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)) を使用して、アタッチできる 2 つ目のバインド可能プロパティ [`LayoutFlagsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) を定義しています。

`AbsoluteLayout.SetLayoutFlags` の引数と `AbsoluteLayout.GetLayoutFlags` の戻り値は、[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) 型の値です。これは次のメンバーを持つ列挙型です。

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (0 と等価)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

これらは、C# のビットごとの OR 演算子で結合することができます。

これらのフラグを設定すると、子の配置やサイズ設定に使用される `Rectangle` レイアウト境界の構造の特定のプロパティが、比例で解釈されます。

`WidthProportional` フラグが設定されている場合、`Width` の値が 1 であることは、子の幅が `AbsoluteLayout` と同じであることを意味します。 高さにも同様の方法が使用されます。

比例の配置では、サイズが考慮されます。 `XProportional` フラグが設定されている場合、`Rectangle` レイアウト境界の `X` プロパティは比例になります。 値が 0 であることは、子の左端が `AbsoluteLayout` の左端に配置されることを意味しますが、位置が 1 であることは、子の右端が、予想されるように `AbsoluteLayout` の右端を超えることはなく、`AbsoluteLayout` の右端に配置されることを意味します。 `X` プロパティが 0.5 である場合、子は水平方向で `AbsoluteLayout` の中央に配置されます。

[**ChessboardProportional**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) サンプルでは、比例のサイズ変更と配置の使用方法が示されています。

## <a name="working-with-proportional-coordinates"></a>比例の座標の使用

ときには、比例の配置を、その `AbsoluteLayout` での実装方法とは異なる方法で考えた方が簡単な場合があります。 `X` のプロパティが 1 である場合に、子の左端 (右端ではなく) が `AbsoluteLayout` の右端に対して配置される比例の座標を使用することをお勧めします。

この代替の配置スキームは、"分数の子の座標" と呼ぶことができます。 次の式を使用して、分数の子の座標から、`AbsoluteLayout` に必要なレイアウト境界に変換することができます。

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[**ProportionalCoordinateCalc**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) サンプルではこれが示されています。

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout と XAML

XAML で `AbsoluteLayout` を使用し、`AbsoluteLayout.LayoutBounds` と `AbsoluteLayout.LayoutFlags` の属性値を使用して、`AbsoluteLayout` の子にアタッチされたバインド可能なプロパティを設定することができます。 これは、[**AbsoluteXamlDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) サンプルおよび [**ChessboardXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) サンプルで示されています。 後者のプログラムには 32 個の `BoxView` 要素が含まれていますが、`AbsoluteLayout.LayoutFlags` プロパティを含む暗黙的な `Style` を使用して、マークアップが最小限に抑えられています。

クラス名、ドット、プロパティ名で構成される XAML の属性は、"*常に*" アタッチされたバインド可能なプロパティです。

## <a name="overlays"></a>オーバーレイ

`AbsoluteLayout` を使用して、ページを他のコントロールで覆う "*オーバーレイ*" を作成することができます。その目的は、おそらく、ページ上の通常のコントロールの操作をユーザーに禁止することです。

[**SimpleOverlay**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) サンプルではこの手法が示されています。また、プログラムによってタスクが完了された程度を表示する [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) も示されています。

## <a name="some-fun"></a>ちょっとしたお楽しみ

[**DotMatrixClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) サンプルでは、シミュレートされた 5x7 のドット マトリックス表示で現在の時刻が表示されます。 各ドットは `BoxView` (228 個含まれています) であり、`AbsoluteLayout` 上でサイズと位置が設定されています。

[![ドット マトリックス時計のトリプル スクリーンショット](images/ch14fg08-small.png "ドット マトリックス時計")](images/ch14fg08-large.png#lightbox "ドット マトリックス時計")

[**BouncingText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) プログラムでは、2 つの `Label` オブジェクトが、画面全体を水平方向および垂直方向にバウンスするようにアニメーション化されます。

## <a name="related-links"></a>関連リンク

- [第 14 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [第 14 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolutelayout.md)
- [関連付けられたプロパティ](~/xamarin-forms/xaml/attached-properties.md)
