---
title: 第3章の概要。 テキストの詳細
description: 'Xamarin. Forms での Mobile Apps の作成: 第3章の概要。 テキストの詳細'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 69415b59bbd376330454302981e3216c236a16bb
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980928"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>第3章の概要。 テキストの詳細

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

この章では、 [`Label`](xref:Xamarin.Forms.Label)の表示について、色、フォント、書式設定などの詳細について説明します。

## <a name="wrapping-paragraphs"></a>段落の折り返し

`Label` の[`Text`](xref:Xamarin.Forms.Label.Text)プロパティに長いテキストが含まれている場合は、 [**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles)サンプルで示されているように、`Label` によって複数行に自動的にラップされます。 全角ダッシュには ' \u2014 ' などの Unicode コードを埋め込むことができC#ます。また、' \r ' のような文字を新しい行に分割することもできます。

`Label` の[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)プロパティと[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)プロパティが `LayoutOptions.Fill`に設定されている場合、`Label` の全体的なサイズは、コンテナーによって使用可能になる領域によって決まります。 `Label` は、*制約付き*と呼ばれます。 `Label` のサイズは、コンテナーのサイズです。

`HorizontalOptions` プロパティと `VerticalOptions` プロパティが `LayoutOptions.Fill`以外の値に設定されている場合、`Label` のサイズは、コンテナーが `Label`に使用できるサイズまで、テキストの表示に必要な領域によって決まります。 `Label` は*制約*がなく、それ自体のサイズを決定します。

(注: 制約*付き*の条件*と制約*のない用語は、通常、制約付きビューよりも制約のないビューが小さくなるので、直感的に考えることができます。 また、本書の初期章では、これらの用語は一貫して使用されていません)。

`Label` などのビューは、1つのディメンションで制限でき、もう一方の次元では制約を受けることはできません。 `Label` は、水平方向に制約されている場合にのみ、テキストを複数行にラップします。

`Label` が制限されている場合、テキストに必要な領域よりもかなり多くの領域が消費される可能性があります。 テキストは、`Label`の全体の領域内に配置できます。 [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment)プロパティを[`TextAlignment`](xref:Xamarin.Forms.TextAlignment)列挙型 ([`Start`](xref:Xamarin.Forms.TextAlignment.Start)、 [`Center`](xref:Xamarin.Forms.TextAlignment.Center)、または[`End`](xref:Xamarin.Forms.TextAlignment.Center)) のメンバーに設定して、段落のすべての行の配置を制御します。 既定値は `Start` で、テキストが左揃えになります。

`Label`によって占有されている領域の上部、中央、または下部にテキストを配置するには、 [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment)プロパティを `TextAlignment` 列挙体のメンバーに設定します。

[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)プロパティを[`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)列挙型のメンバー ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap)、 [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap)、 [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap)、 [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation)、 [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation)、または[`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) に設定して、複数の段落区切りまたは切り捨てられた行。

## <a name="text-and-background-colors"></a>テキストと背景の色

`Label` の[`TextColor`](xref:Xamarin.Forms.Label.TextColor)と[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)のプロパティを[`Color`](xref:Xamarin.Forms.Color)の値に設定して、テキストと背景の色を制御します。

`BackgroundColor` は、`Label`によって占有されている領域全体の背景に適用されます。 `HorizontalOptions` と `VerticalOptions` のプロパティによっては、テキストの表示に必要な領域よりもサイズがかなり大きくなることがあります。 色を使用すると、`HorizontalOptions`、`VerticalOptions`、`HorizontalExeAlignment`、および `VerticalTextAlignment` のさまざまな値を試して、`Label`のサイズと位置、および `Label`内のテキストのサイズと位置にどのように影響するかを確認できます。

## <a name="the-color-structure"></a>色の構造

[`Color`](xref:Xamarin.Forms.Color)構造を使用すると、色を赤、緑、青 (RGB) の値、または色合いと鮮やかさの明るさ (HSL) の値、または色の名前で指定できます。 また、透明度を示すためにアルファチャネルを使用することもできます。

`Color` コンストラクターを使用して、次のものを指定します。

- [灰色の網掛け](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [RGB 値](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [透明度の RGB 値](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

引数は、0 ~ 1 の範囲の値 `double` ます。

いくつかの静的メソッドを使用して `Color` の値を作成することもできます。

- `double` の RGB 値を0から1に[`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double))
- 0 ~ 255 の整数の RGB 値の[`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32))
- 透明度が `double` RGB 値の[`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double))
- 透明度のある整数の RGB 値の[`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))
- 透過性を持つ HSL 値の `double` の[`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))
- (B + 256 \* (G + 256 \* (R + 256 \* A)) として計算された `uint` 値の[`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32))
- "#AARRGGBB" または "#RRGGBB"、"#ARGB"、"#RGB" の形式の16進数の `string` 形式を[`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String))します。各文字は、アルファ、赤、緑、および青の各チャネルの16進数の数字に対応します。 このメソッドは[、「7章、xaml、およびコード](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)」で説明されているように、xaml カラー変換に使用されます。

作成された `Color` 値は変更できません。 色の特性は、次のプロパティから取得できます。

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

これらはすべて、0 ~ 1 の範囲の `double` 値です。

`Color` は、一般的な色に対して、240のパブリック静的読み取り専用フィールドも定義します。 本が書かれた時点では、使用できるのは17色の共通色のみでした。

もう1つのパブリック静的読み取り専用フィールドは、すべてのカラーチャネルがゼロに設定される色を定義します。

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

いくつかのインスタンスメソッドでは、既存の色を変更して新しい色を作成することができます。

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

最後に、2つの静的な読み取り専用プロパティは、特殊な色の値を定義します。

- [`Color.Default`](xref:Xamarin.Forms.Color.Default)、すべてのチャネルが &ndash;1 に設定される
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` は、プラットフォームの配色を適用するためのものであり、その結果、プラットフォームごとに異なるコンテキストで異なる意味を持ちます。 既定では、プラットフォームの配色は次のとおりです。

- iOS: 明るい背景にある濃いテキスト
- Android: 濃い背景の明るいテキスト (本内) または明るい背景の濃いテキスト (サンプルコードリポジトリの**マスター**ブランチで AppCompat を使用したマテリアル設計の場合)
- UWP: 明るい背景に濃いテキストを表示する

`Color.Accent` 値により、プラットフォーム固有の (場合によってはユーザーが選択可能な) 色が生成され、暗い背景または明るい背景に表示されます。

## <a name="changing-the-application-color-scheme"></a>アプリケーションの配色の変更

上記の一覧に示すように、さまざまなプラットフォームに既定の配色があります。

Android を対象とする場合は、Android の .Manifest ファイルで明るいテーマを指定するか、 [AppCompat と素材のデザインを追加](~/xamarin-forms/platform/android/appcompat-material-design.md)することによって、ダークオンライトスキームに切り替えることができます。

Windows プラットフォームでは、通常、配色テーマはユーザーによって選択されますが、`RequestedTheme` 属性セットをプラットフォームの app.xaml ファイルの `Light` または `Dark` に追加することができます。 既定では、UWP プロジェクトの app.xaml ファイルには、`Light`に設定された `RequestedTheme` 属性が含まれています。

## <a name="font-sizes-and-attributes"></a>フォントサイズと属性

`Label` の[`FontFamily`](xref:Xamarin.Forms.Label.FontFamily)プロパティを "Times Roman" などの文字列に設定して、フォントファミリを選択します。 ただし、特定のプラットフォームでサポートされているフォントファミリを指定する必要があります。この点については、プラットフォームに整合性がありません。

`Label` の[`FontSize`](xref:Xamarin.Forms.Label.FontSize)プロパティを、フォントのおおよその高さを指定するための `double` に設定します。 フォントサイズをインテリジェントに選択する方法の詳細については[、「サイズを処理](chapter05.md)する」を参照してください。

または、プラットフォームに依存するいくつかの事前設定済みのフォントサイズのいずれかを取得することもできます。 静的[`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type))メソッドと[オーバーロード](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element))はどちらも、 [`NamedSize`](xref:Xamarin.Forms.NamedSize)列挙体のメンバーに基づいてプラットフォームに適した `double` フォントサイズ値を返します ([`Default`](xref:Xamarin.Forms.NamedSize.Default)、 [`Micro`](xref:Xamarin.Forms.NamedSize.Micro)、 [`Small`](xref:Xamarin.Forms.NamedSize.Small)、 [`Medium`](xref:Xamarin.Forms.NamedSize.Medium)、 [`Large`](xref:Xamarin.Forms.NamedSize.Large))。 `Medium` メンバーから返される値は、必ずしも `Default`と同じであるとは限りません。 [**Namedfontsizes サイズ**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)のサンプルでは、これらの名前付きサイズのテキストが表示されます。

`Label` の[`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes)プロパティを、これらの[`FontAttributes`](xref:Xamarin.Forms.FontAttributes)列挙型、 [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold)、 [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic)、または[`None`](xref:Xamarin.Forms.FontAttributes.None)のメンバーに設定します。 `Bold` と `Italic` のメンバーを、 C#ビットごとの or 演算子と組み合わせることができます。

## <a name="formatted-text"></a>書式付きテキスト

これまでのすべての例では、`Label` によって表示されるテキスト全体が一様に書式設定されています。 テキスト文字列内の書式設定を変更するには、`Label`の `Text` プロパティを設定しないでください。 代わりに、 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)プロパティを[`FormattedString`](xref:Xamarin.Forms.FormattedString)型のオブジェクトに設定します。

`FormattedString` には、 [`Span`](xref:Xamarin.Forms.Span)オブジェクトのコレクションである[`Spans`](xref:Xamarin.Forms.FormattedString.Spans)プロパティがあります。 各 `Span` オブジェクトには、独自の[`Text`](xref:Xamarin.Forms.Span.Text)、 [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)、 [`FontSize`](xref:Xamarin.Forms.Span.FontSize)、 [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)、 [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)、および[`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)の各プロパティがあります。

[**VariableFormattedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText)サンプルでは、1行のテキストに対して `FormattedText` プロパティを使用する方法を示しています。 [**VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara)は、次に示すように、段落全体の手法を示しています。

[![変数書式設定された段落のトリプルスクリーンショット](images/ch03fg06-small.png "変数の書式設定されたラベルのテキスト")](images/ch03fg06-large.png#lightbox "変数の書式設定されたラベルのテキスト")

[**Namedfontsizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)プログラムは、1つの `Label` と `FormattedString` オブジェクトを使用して、各プラットフォームのすべての名前付きフォントサイズを表示します。

## <a name="related-links"></a>関連リンク

- [第3章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [第3章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [第 3 F#章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [group1](~/xamarin-forms/user-interface/text/label.md)
- [色の操作](~/xamarin-forms/user-interface/colors.md)
