---
title: 第 3 章の概要です。 テキストに進む
description: Xamarin を使用した Mobile Apps の作成:第 3 章の概要です。 テキストに進む
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e37175240825c0fed350589649469c99f1bbf69a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771225"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>第 3 章の概要です。 テキストに進む

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

この章の説明、 [ `Label` ](xref:Xamarin.Forms.Label)色、フォント、書式設定など、詳しく表示されます。

## <a name="wrapping-paragraphs"></a>段落の折り返し

ときに、 [ `Text` ](xref:Xamarin.Forms.Label.Text)プロパティの`Label`長いテキストを含む`Label`、自動的に折り返さ、複数の行に示すように、 [ **Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles)サンプル。 '\U2014' em dash、または C# の新しい行に分割するには、'\r' などの文字などの Unicode コードを埋め込むことができます。

ときに、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)のプロパティを`Label`に設定されます`LayoutOptions.Fill`、全体のサイズ、`Label`領域に準拠するものをそのコンテナー使用できるようにします。 `Label`言います*制約付き*します。 サイズ、`Label`はそのコンテナーのサイズです。

ときに、`HorizontalOptions`と`VerticalOptions`プロパティが以外の値に設定されます`LayoutOptions.Fill`、サイズ、`Label`サイズのコンテナーが利用できるようにするまで、テキストのレンダリングに必要な領域に準拠するもの、 `Label`。 `Label`言います*制約のない*し、独自のサイズを決定します。

(メモ: 制約*付き*ビューと*制約の*ない用語は、通常、制約されたビューよりも制約のないビューが小さいので、非常に直感的である可能性があります。 また、これらの用語は使用されません一貫した方法で書籍の前半の章。)

などのビューを`Label`1 つのディメンションでは制限し、制約、それ以外のことができます。 A`Label`水平方向に指定されている場合、複数の行にテキストを折り返すだけされます。

場合、`Label`は、制約付きのテキストのために必要なよりもはるかに多くの領域を占有に可能性があります。 全体的な領域内のテキストを配置することができます、`Label`します。 設定、 [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment)プロパティのメンバーを[ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment)列挙型 ([`Start`](xref:Xamarin.Forms.TextAlignment.Start)、 [ `Center` ](xref:Xamarin.Forms.TextAlignment.Center)、または[ `End` ](xref:Xamarin.Forms.TextAlignment.Center))、段落のすべての行の配置を制御します。 既定値は`Start`テキストを左揃えられます。

設定、 [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment)プロパティのメンバーを`TextAlignment`上部、中央、またはによって占有される領域の下部にあるテキストの位置を列挙、`Label`します。

設定、 [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode)プロパティのメンバーを[ `LineBreakMode` ](xref:Xamarin.Forms.LineBreakMode)列挙型 ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap)、 [ `CharacterWrap` ](xref:Xamarin.Forms.LineBreakMode.CharacterWrap)、 [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap)、 [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode.HeadTruncation)、 [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation)、または[ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) するコントロールの複数の段落改行で行または切り捨てられます。

## <a name="text-and-background-colors"></a>テキストと背景色

設定、 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)と[ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)プロパティの`Label`に[ `Color` ](xref:Xamarin.Forms.Color)テキストと背景の色を制御する値。

`BackgroundColor`によって占有される領域全体の背景に適用されます、`Label`します。 に応じて、`HorizontalOptions`と`VerticalOptions`プロパティ、サイズが、テキストを表示するために必要な領域よりもかなり大きい可能性があります。 色を使用して、実験のさまざまな値を持つ`HorizontalOptions`、 `VerticalOptions`、 `HorizontalExeAlignment`、および`VerticalTextAlignment`の位置とサイズの影響を表示する、 `Label`、内のテキストの位置とサイズ、`Label`します。

## <a name="the-color-structure"></a>Color 構造体

[ `Color` ](xref:Xamarin.Forms.Color)構造では、赤、緑、青 (RGB) の値、または色合い-鮮やかさ-明るさ (HSL) の値、または色の名前を持つ色を指定することができます。 アルファ チャネルをもを透明度を示すために使用できます。

使用して、`Color`コンストラクターを指定します。

- [灰色の網掛け](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [RGB 値](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [透過性の RGB 値](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

引数が`double`0 から 1 までの値。

作成するいくつかの静的メソッドを使用することもできます。`Color`値。

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) `double` RGB 値が 0 から 1 に
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) 0 から 255 までの整数 RGB 値の場合
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) `double`透過性の RGB 値
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) 透明度が整数の RGB 値の場合
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) `double`透過性の HSL 値
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) `uint`値の計算 (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) `string`形式の 16 進数形式で"#AARRGGBB"または"#RRGGBB"または"#ARGB"または"#RGB"をそれぞれの文字に対応して 16 進数、アルファ、赤、緑、および青のチャネル。 このメソッドは、XAML の色の変換で説明したように使用されるプライマリ[第 7 章、コードと XAML](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)します。

作成されたら、`Color`値は変更できません。 色の特性は、次のプロパティから取得できます。

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

これらはすべて`double`0 から 1 までの値。

`Color` また、240 パブリック静的読み取り専用フィールドの一般的な色を定義します。 この書籍の執筆時点で、17 の一般的な色が使用可能です。

別パブリックな静的読み取り専用フィールドは、0 に設定するすべてのカラー チャネルで色を定義します。

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

いくつかのインスタンス メソッドは、新しい色を作成する既存の色を変更することを許可します。

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

最後に、2 つの静的な読み取り専用プロパティは、特別な色の値を定義します。

- [`Color.Default`](xref:Xamarin.Forms.Color.Default)、すべてのチャネルを設定&ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` 目的は、プラットフォームの配色を適用して、さまざまなプラットフォームでさまざまなコンテキストで異なる意味を持ちます。 既定では、プラットフォームの配色がされます。

- IOS明るい背景にある濃いテキスト
- Android暗い背景の明るいテキスト (本内) または明るい背景の暗いテキスト (サンプルコードリポジトリの**マスター**ブランチで AppCompat を使用したマテリアル設計の場合)
- UWP明るい背景にある濃いテキスト

`Color.Accent`暗色または明色の背景に表示されているプラットフォーム固有 (および場合によってユーザーが選択可能な) の色で結果の値します。

## <a name="changing-the-application-color-scheme"></a>アプリケーションの配色の変更

さまざまなプラットフォームでは、上記の一覧で示すように既定の配色パターンがあります。

Android を対象とする場合になっても Android.Manifest.xml ファイルまたはによって light テーマを指定することによって、軽量での dark スキームに切り替え[AppCompat の追加と材料設計](~/xamarin-forms/platform/android/appcompat-material-design.md)です。

追加することが、Windows プラットフォーム用の色のテーマは通常、ユーザーが選択、`RequestedTheme`いずれかに属性が設定`Light`または`Dark`プラットフォームの App.xaml ファイルにします。 既定では、UWP プロジェクトで、App.xaml ファイルを含む、`RequestedTheme`属性に設定`Light`します。

## <a name="font-sizes-and-attributes"></a>フォント サイズと属性

設定、 [ `FontFamily` ](xref:Xamarin.Forms.Label.FontFamily)プロパティの`Label`"Times Roman"のフォント ファミリを選択するなどの文字列にします。 ただし、特定のプラットフォームでサポートされているフォント ファミリを指定する必要があると、プラットフォームの一貫性のあるこの点でないは。

設定、 [ `FontSize` ](xref:Xamarin.Forms.Label.FontSize)プロパティの`Label`を`double`フォントのおおよその高さを指定するためです。 参照してください[第 5 章のサイズを扱う](chapter05.md)、フォント サイズを適切に選択する方法の詳細についてはします。

別の方法としていくつかのプリセットのプラットフォームに依存するフォント サイズのいずれかを取得できます。 静的な[ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type))メソッドと[オーバー ロード](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element))両方を返す、`double`プラットフォームに適切なフォント サイズ値がのメンバーに基づいて、 [ `NamedSize` ](xref:Xamarin.Forms.NamedSize)列挙型 ([`Default`](xref:Xamarin.Forms.NamedSize.Default)、 [ `Micro` ](xref:Xamarin.Forms.NamedSize.Micro)、 [ `Small` ](xref:Xamarin.Forms.NamedSize.Small)、 [ `Medium` ](xref:Xamarin.Forms.NamedSize.Medium)、 [ `Large` ](xref:Xamarin.Forms.NamedSize.Large))。 返される値、`Medium`メンバーが必ずしも同じ`Default`します。 [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)サンプルでは、これらのサイズを名前付きのテキストを表示します。

設定、 [ `FontAttributes` ](xref:Xamarin.Forms.Label.FontAttributes)プロパティの`Label`これらのメンバーに[ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes)列挙型、 [ `Bold` ](xref:Xamarin.Forms.FontAttributes.Bold)、 [ `Italic`](xref:Xamarin.Forms.FontAttributes.Italic)、または[ `None`](xref:Xamarin.Forms.FontAttributes.None)します。 組み合わせることができます、`Bold`と`Italic`C# or 演算子を持つメンバー。

## <a name="formatted-text"></a>書式付きテキスト

によってすべての例は、これまでに、テキスト全体が表示される、`Label`一様にフォーマットされています。 テキスト文字列内の書式設定の変更に設定しないでください、`Text`プロパティの`Label`します。 代わりに、設定、 [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText)プロパティ型のオブジェクトを[ `FormattedString`](xref:Xamarin.Forms.FormattedString)します。

`FormattedString` [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans)プロパティのコレクションである[ `Span` ](xref:Xamarin.Forms.Span)オブジェクト。 各`Span`オブジェクトが、独自[ `Text` ](xref:Xamarin.Forms.Span.Text)、 [ `FontFamily` ](xref:Xamarin.Forms.Span.FontFamily)、 [ `FontSize` ](xref:Xamarin.Forms.Span.FontSize)、 [ `FontAttributes` ](xref:Xamarin.Forms.Span.FontAttributes)、 [ `ForegroundColor` ](xref:Xamarin.Forms.Span.ForegroundColor)、および[ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor)プロパティ。

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText)サンプルの使用例、 `FormattedText` 、テキストの 1 つの行のプロパティと[ **VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara)次に示すように、段落全体の手法を示します。

[![変数の 3 倍になるスクリーン ショットは、段落を書式設定された](images/ch03fg06-small.png "形式のラベル テキストを変数")](images/ch03fg06-large.png#lightbox "変数の書式設定されたラベルのテキスト")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)プログラムは、1 つを使用して`Label`と`FormattedString`すべてのプラットフォームごとに名前付きのフォント サイズを表示するオブジェクト。

## <a name="related-links"></a>関連リンク

- [第 3 章のフル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [第 3 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [第 3 章F#サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [色の扱い](~/xamarin-forms/user-interface/colors.md)
