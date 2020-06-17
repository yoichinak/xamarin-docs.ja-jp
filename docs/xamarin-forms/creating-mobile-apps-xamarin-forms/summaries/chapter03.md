---
title:"第 3 章の概要: テキストの詳細" の説明: "Xamarin.Forms でモバイル アプリを作成する: 第 3 章の概要。 テキストの詳細" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734 author: davidbritch ms.author: dabritch ms.date:07/18/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-3-deeper-into-text"></a>第 3 章の概要。 テキストの詳細

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

この章では、色、フォント、書式設定など、[`Label`](xref:Xamarin.Forms.Label) ビューの詳細について説明します。

## <a name="wrapping-paragraphs"></a>段落の折り返し

`Label` の [`Text`](xref:Xamarin.Forms.Label.Text) プロパティに長いテキストが含まれている場合、[**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) サンプルで示されているように、`Label` は自動的に複数行に折り返されます。 em ダッシュには '\u2014' などの Unicode コード、改行するには '\r' などの C# 文字を埋め込むことができます。

`Label` の [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) および [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティが `LayoutOptions.Fill` に設定されている場合、`Label` の全体的なサイズは、そのコンテナーに使用できる領域によって決まります。 `Label` は "*制約あり*" と呼ばれます。 `Label` のサイズは、そのコンテナーのサイズです。

`HorizontalOptions` および `VerticalOptions` プロパティが `LayoutOptions.Fill` 以外の値に設定されている場合、`Label` のサイズは、テキストのレンダリングに必要な領域によって決まります。コンテナーで `Label` に使用できるサイズが上限です。 `Label` は "*制約なし*" と呼ばれ、独自のサイズが決定されます

(メモ: 制約なしのビューは制約ありのビューよりも一般に小さいため、"*制約あり*" と "*制約なし*" という用語は直感とは異なる場合があります。 また、これらの用語は、本書の初期の章では一貫した使い方がされていません)。

`Label` などのビューは、あるディメンションで制約ありであり、別のディメンションでは制約なしの場合があります。 `Label` を使うと、横方向に制約がある場合にのみ、テキストが複数行に折り返されます。

`Label` が制約ありの場合、テキストに必要な領域よりもかなり多くの領域が占有される可能性があります。 テキストは、`Label` 全体の領域内に配置できます。 [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) プロパティを [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 列挙型のメンバー ([`Start`](xref:Xamarin.Forms.TextAlignment.Start)、[`Center`](xref:Xamarin.Forms.TextAlignment.Center)、または[`End`](xref:Xamarin.Forms.TextAlignment.Center)) に設定して、段落のすべての行の配置を制御します。 既定値は `Start` であり、テキストは左揃えに配置されます。

[`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) プロパティを `TextAlignment` 列挙体のメンバーに設定して、`Label` が占める領域の上部、中央、または下部にテキストを配置します。

[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) プロパティを [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 列挙型のメンバー ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap)、[`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap)、[`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap)、[`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation)、[`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation)、または[`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) に設定して、段落内の複数の行を改行する方法または切り詰める方法を制御します。

## <a name="text-and-background-colors"></a>テキストと背景の色

`Label` の [`TextColor`](xref:Xamarin.Forms.Label.TextColor) および [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) プロパティを [`Color`](xref:Xamarin.Forms.Color) の値に設定して、テキストと背景の色を制御します。

`BackgroundColor` は、`Label` が占める領域全体の背景に適用されます。 `HorizontalOptions` および `VerticalOptions` プロパティによっては、テキストを表示するために必要な領域よりもかなり大きいサイズになる可能性があります。 color を使って `HorizontalOptions`、`VerticalOptions`、`HorizontalExeAlignment`、および`VerticalTextAlignment` のさまざまな値を実験すると、`Label` のサイズと位置、`Label` 内のテキストのサイズと位置にどのように影響するかを確認できます。

## <a name="the-color-structure"></a>Color の構造

[`Color`](xref:Xamarin.Forms.Color) の構造では、赤緑青 (RGB) 値、色相彩度 (HSL) 値、または色の名前で色を指定できます。 また、アルファ チャネルを使用して透明度を示すこともできます。

`Color` コンストラクターを使用して以下を指定します。

- [灰色の網掛け](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [RGB 値](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [透明度付きの RGB 値](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

引数は、0 から 1 の範囲の `double` 値です。

いくつかの静的メソッドを使用して `Color` 値を作成することもできます。

- 0 から 1 の `double` RGB 値の場合は [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double))
- 0 から 255 の整数の RGB 値の場合は [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32))
- 透明度付きの `double` RGB 値の場合は [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double))
- 透明度付きの整数 RGB 値の場合は [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))
- 透明度付きの `double` HSL 値の場合は [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))
- (B + 256 \* (G + 256 \* (R + 256 \* A))) と計算される `uint` 値の場合は [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32))
- "#AARRGGBB" または "#RRGGBB" または "#ARGB" または "#RGB" 形式の 16 進数の `string` 形式の場合は [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String))。この各文字は、アルファ、赤、緑、青の各チャンネルの 16 進数に対応します。 このメソッドは、[第 7 章 XAML とコード](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)のページで説明されているように、主に XAML の色変換に使用されます。

`Color` 値は作成後に変更できません。 色の特性は、次のプロパティから取得できます。

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

これらは、いずれも 0 から 1 の範囲の `double` 値です。

また、`Color` を使うと、一般的な色の 240 色のパブリックで静的な読み取り専用フィールドも定義できます。 本書の執筆時点では、17 色のみを使用できました。

もう 1 つのパブリックで静的な読み取り専用フィールドを使うと、すべてのカラー チャネルがゼロに設定された色を定義できます。

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

いくつかのインスタンス メソッドでは、既存の色を変更して新しい色を作成できます。

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

最後に、2 つの静的な読み取り専用プロパティを使うと、特殊な色の値を定義できます。

- [`Color.Default`](xref:Xamarin.Forms.Color.Default) (すべてのチャネルが &ndash;1 に設定されます)
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` は、プラットフォームの配色を適用するためのものなので、プラットフォームごとに異なるコンテキストで異なる意味を持ちます。 既定では、プラットフォームの配色は次のとおりです。

- iOS:明るい背景に暗いテキスト
- Android:暗い背景に明るいテキスト (本書)、または明るい背景に暗いテキスト (サンプル コード リポジトリの**マスター** ブランチの AppCompat によるマテリアル デザインの場合)
- UWP:明るい背景に暗いテキスト

`Color.Accent` 値により、暗い背景または明るい背景に表示されるプラットフォーム固有の (場合によってはユーザーが選択可能な) 色になります。

## <a name="changing-the-application-color-scheme"></a>アプリケーションの配色の変更

上記の一覧に示すように、プラットフォームによって既定の配色があります。

Android を対象とする場合、Android.Manifest.xml ファイルで明るいテーマを指定するか、[AppCompat とマテリアル デザインを追加](~/xamarin-forms/platform/android/appcompat-material-design.md)することで、明るい背景に暗いテキストの配色に切り替えることができます。

Windows プラットフォームの場合、通常、ユーザーが配色を選択しますが、プラットフォームの App.xaml ファイルで `Light` または `Dark` に設定した `RequestedTheme` 属性を追加できます。 既定では、UWP プロジェクトの App.xaml ファイルには、`Light` に設定された `RequestedTheme` 属性が含まれています。

## <a name="font-sizes-and-attributes"></a>フォント サイズと属性

フォント ファミリを選択するには、`Label` の [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily) プロパティを "Times Roman" などの文字列に設定します。 ただし、そのプラットフォームでサポートされているフォント ファミリを指定する必要があり、この点については各プラットフォームが一致していません。

フォントのおおよその高さを指定するには、`Label` の [`FontSize`](xref:Xamarin.Forms.Label.FontSize) プロパティを `double` に設定します。 フォント サイズをインテリジェントに選択する方法の詳細については、[第 5 章 サイズの処理](chapter05.md)のページを参照してください。

または、いくつかのプリセットされたプラットフォーム依存のフォント サイズのいずれかを取得できます。 静的な [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) メソッドと [オーバーロード](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) はどちらも、[`NamedSize`](xref:Xamarin.Forms.NamedSize) 列挙型 ([`Default`](xref:Xamarin.Forms.NamedSize.Default)、[`Micro`](xref:Xamarin.Forms.NamedSize.Micro)、[`Small`](xref:Xamarin.Forms.NamedSize.Small)、[`Medium`](xref:Xamarin.Forms.NamedSize.Medium)、および [`Large`](xref:Xamarin.Forms.NamedSize.Large)) のメンバーに基づいて、そのプラットフォームにとって適切な `double` のフォント サイズ値を返します。 `Medium` メンバーから返される値は、必ずしも `Default` と同じにはなりません。 [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) サンプルでは、これら名前付きサイズを使用してテキストが表示されます。

`Label` の [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) プロパティを、これらの [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 列挙対のメンバー ([`Bold`](xref:Xamarin.Forms.FontAttributes.Bold)、[`Italic`](xref:Xamarin.Forms.FontAttributes.Italic)、または [`None`](xref:Xamarin.Forms.FontAttributes.None)) に設定します。 `Bold` および `Italic` のメンバーは、C# のビットごとの OR 演算子で結合することができます。

## <a name="formatted-text"></a>書式付きテキスト

これまでのいずれも例でも、`Label` によって表示されるテキスト全体は同じ書式が設定されていました。 テキスト文字列内の書式設定を変更するには、`Label` の `Text` プロパティを設定しないでください。 代わりに、[`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) プロパティを型 [`FormattedString`](xref:Xamarin.Forms.FormattedString) のオブジェクトに設定します。

`FormattedString` には、[`Span`](xref:Xamarin.Forms.Span) オブジェクトのコレクションである [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) プロパティがあります。 各 `Span` オブジェクトには、独自の [`Text`](xref:Xamarin.Forms.Span.Text)、[`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)、[`FontSize`](xref:Xamarin.Forms.Span.FontSize)、[`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)、[`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)、および [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) プロパティがあります。

[**VariableFormattedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) サンプルは、1 行のテキストに `FormattedText` プロパティを使用する方法を示しています。また、[**VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) は、次のように段落全体の場合の手法を示しています。

[![可変の書式が設定された段落のトリプル スクリーンショット](images/ch03fg06-small.png "可変の書式が設定されたラベルの文字")](images/ch03fg06-large.png#lightbox "可変の書式が設定されたラベルの文字")

[**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) プログラムを実行すると、1 つの `Label` および `FormattedString` オブジェクトを使用して、各プラットフォームのすべての名前付きフォント サイズを表示できます。

## <a name="related-links"></a>関連リンク

- [第 3 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [第 3 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [第 3 章の F# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [色の操作](~/xamarin-forms/user-interface/colors.md)
