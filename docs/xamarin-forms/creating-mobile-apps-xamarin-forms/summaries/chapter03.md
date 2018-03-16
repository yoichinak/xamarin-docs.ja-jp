---
title: "第 3 章の概要です。 テキストに進む"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a3ef515feabfc142f30e7e00a8fed710e733f4dc
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>第 3 章の概要です。 テキストに進む

この章の説明、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)などの色、フォント、および書式設定の詳細の深さで表示します。

## <a name="wrapping-paragraphs"></a>段落の折り返し

ときに、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティの`Label`長いテキストを含む`Label`自動的に折り返されて、複数の行に示すように、 [ **Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles)サンプルです。 Em dash、または C# の場合、新しい行に分割するには、'\r' などの文字 '\u2014' などの Unicode コードを埋め込むことができます。

ときに、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)のプロパティ、`Label`に設定されている`LayoutOptions.Fill`の全体的なサイズ、`Label`スペースによって管理されるをコンテナー使用できるようにします。 `Label`言います*制約*です。 サイズ、`Label`コンテナーのサイズです。

ときに、`HorizontalOptions`と`VerticalOptions`プロパティが以外の値に設定されます`LayoutOptions.Fill`のサイズ、`Label`はコンテナーでの使用可能なサイズまでのテキストを表示するために必要な領域を受ける、`Label`です。 `Label`言います*制約のない*し、独自のサイズを決定します。

(注: 条項*制約*と*制約のない*制約なしのビューは、制約付きビューよりも小さいため、逆があります。 また、これらの用語は使用されません一貫してブックの初期の章で。)

など、ビュー、 `Label` 1 つのディメンション内で制限して、他の制約します。 A`Label`水平方向に指定されている場合、複数の行にテキストを折り返すのみされます。

場合、`Label`は、制約付きテキストに必要なよりもはるかに多くの領域を占有に可能性があります。 全体的な領域内のテキストを配置することができます、`Label`です。 設定、 [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/)プロパティのメンバーを[ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/)列挙 ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/)、 [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)、または[ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/))、段落のすべての行の配置を制御します。 既定値は`Start`し、テキストを左揃えにします。

設定、 [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/)プロパティのメンバーを`TextAlignment`上部、中央、またはによって占有される領域の下部にあるテキストを配置する列挙体、`Label`です。

設定、 [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/)プロパティのメンバーを[ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/)列挙 ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/)、 [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/)、 [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/)、 [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/)、 [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/)、または[ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)) にコントロールの複数の段落区切り内の行または切り捨てられます。

## <a name="text-and-background-colors"></a>テキスト色と背景色

設定、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)と[ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)プロパティの`Label`に[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)テキストと背景の色を制御する値。

`BackgroundColor`によって占有される領域全体の背景に適用されます、`Label`です。 によって、`HorizontalOptions`と`VerticalOptions`プロパティ、サイズが、テキストを表示するために必要な領域よりもかなり大きい可能性があります。 さまざまな値をテストする色を使用することができます`HorizontalOptions`、 `VerticalOptions`、 `HorizontalExeAlignment`、および`VerticalTextAlignment`の位置とサイズの影響を表示する、 `Label`、内のテキストの位置とサイズ、`Label`です。

## <a name="the-color-structure"></a>Color 構造体

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)構造では、赤、緑、青 (RGB) の値、または色合い、鮮やかさ、および明るさ (HSL) 値、または色の名前を持つ色を指定することができます。 アルファ チャネルも透過性を示すために使用できます。

使用して、`Color`コンス トラクターを指定します。

- [灰色の網掛け](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- [RGB 値](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- [透過性の RGB 値](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

引数は`double`0 から 1 までの値。

複数の静的メソッドを使用して作成した`Color`値。

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) `double` 0 ~ 1 の RGB 値
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) 整数 RGB 値の 0 から 255 の範囲
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) `double`透過性の RGB 値
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) 整数 RGB 値の透過性
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) `double`透過性の HSL 値
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) `uint`値の計算 (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) `string`形式の 16 進数字の形式"#AARRGGBB"または"#RRGGBB"または"#ARGB"または"#RGB"が各文字に対応する 16 進数のアルファ、赤、緑、および青のチャネル。 このメソッドで説明したように、XAML の色の変換に使用されるプライマリ[7 章、コードと XAML](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)です。

作成すると、`Color`値は変更できません。 色の特性は、次のプロパティから取得できます。

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

これらはすべて`double`0 から 1 までの値。

`Color` また 240 のパブリック静的読み取り専用フィールドの一般的な色を定義します。 ブックが書き込まれた時間、17 の一般的な色入手できました。

別のパブリック静的な読み取り専用フィールドは、0 に設定するすべてのカラー チャネルで色を定義します。

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

いくつかのインスタンス メソッドでは、新しい色を作成、既存の色の変更を許可します。

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

最後に、2 つの静的な読み取り専用プロパティは、特別な色の値を定義します。

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)、すべてのチャネルを設定&ndash;1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` プラットフォームの配色を適用するためのものでは、さまざまなプラットフォーム上の別のコンテキストで別の意味を持ちます。 既定では、プラットフォームの配色がされます。

- iOS: 明るい背景に濃いテキスト
- Android: 光 (書籍) で、暗い背景上または明るい背景に濃いテキスト (で AppCompat を介してマテリアル設計のため、**マスター**サンプル コード リポジトリのブランチ)
- 明るい背景に濃いテキストを UWP:
- Windows 8.1: が点灯、暗い背景のテキスト
- Windows Phone 8.1: が点灯、暗い背景のテキスト

`Color.Accent`暗色または明色の背景に表示されているプラットフォームに固有の (および場合がありますユーザー選択が可能) の色で結果の値します。

## <a name="changing-the-application-color-scheme"></a>アプリケーションの配色の変更

さまざまなプラットフォームでは、上記の一覧に示すようには、既定の色のスキームがあります。

Android を対象とする場合になっても Android.Manifest.xml ファイルまたはによって light テーマを指定することによって、軽量での dark スキームに切り替え[AppCompat の追加と材料設計](~/xamarin-forms/platform/android/appcompat.md)です。

色のテーマに通常、ユーザーが選択されているプラットフォームについては、Windows、追加することが、`RequestedTheme`いずれかに属性が設定`Light`または`Dark`プラットフォームの App.xaml ファイルにします。 既定では、UWP プロジェクトで、App.xaml ファイルを含む、`RequestedTheme`属性に設定`Light`です。

## <a name="font-sizes-and-attributes"></a>フォント サイズと属性

設定、 [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/)プロパティ`Label`"Times Roman"フォント ファミリを選択するなどの文字列にします。 ただし、特定のプラットフォームでサポートされているフォント ファミリを指定する必要があります、プラットフォーム一貫していないこの点です。

設定、 [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/)プロパティ`Label`を`double`フォントのおおよその高さを指定するためです。 参照してください[第 5 章のサイズを扱う](chapter05.md)、インテリジェントなフォント サイズの選択の詳細についてはします。

代わりにいくつかの事前設定されたプラットフォームに依存するフォント サイズのいずれかを取得できます。 静的な[ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/)メソッドおよび[オーバー ロード](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/)両方を返す、`double`プラットフォームに適切なフォント サイズの値がのメンバーに基づく、 [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)列挙 ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/)、 [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/)、 [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/)、 [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/)、 および[ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/))。 返される値、`Medium`メンバーが必ずしも同じ`Default`です。 [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)サンプルこれら sizes という名前のテキストが表示されます。

設定、 [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/)プロパティ`Label`これらのメンバーに[ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/)列挙型、 [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/)、 [ `Italic`](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/)、または[ `None`](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/)です。 組み合わせることができます、`Bold`と`Italic`c# or 演算子を持つメンバー。

## <a name="formatted-text"></a>書式付きテキスト

によってすべての例では、これまで、テキスト全体が表示される、`Label`一様にフォーマットされています。 テキスト文字列内で書式設定を変更するための設定にされていない、`Text`プロパティ`Label`です。 代わりに、設定、 [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/)プロパティ型のオブジェクトを[ `FormattedString`](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/)です。

`FormattedString` [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/)プロパティのコレクションである[ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/)オブジェクト。 各`Span`オブジェクトには、独自[ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/)、 [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/)、 [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/)、 [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/)、 [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/)、および[ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/)プロパティです。

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText)サンプルの使用例、`FormattedText`プロパティを 1 行のテキスト、および[ **VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara)次に示すように、段落全体の手法を示します。

[![変数の 3 倍のスクリーン ショットの書式設定されたパラグラフ](images/ch03fg06-small.png "形式のラベル テキストを変数")](images/ch03fg06-large.png#lightbox "形式のラベル テキストを変数")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes)プログラムが 1 つを使用して`Label`と`FormattedString`すべてのプラットフォームごとに名前付きのフォント サイズを表示するオブジェクト。



## <a name="related-links"></a>関連リンク

- [第 3 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [第 3 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [第 3 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [色を使用します。](~/xamarin-forms/user-interface/colors.md)
