---
title: 7 章の概要です。 XAML コードとの比較
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 97f1ad1f818c74a294421f223c4cea0123b83373
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>7 章の概要です。 XAML コードとの比較

Xamarin.Forms では、拡張アプリケーション マークアップ言語と呼ばれる XML に基づくマークアップ言語または XAML (「ザメル」と発音) をサポートします。 XAML では、代わりに C# の場合、Xamarin.Forms アプリケーションのユーザー インターフェイスのレイアウトの定義とユーザー インターフェイス要素間のバインディングを定義して、データを基になるを提供します。

## <a name="properties-and-attributes"></a>プロパティと属性

Xamarin.Forms クラスと構造体 XAML では、XML 要素なり、これらのクラスと構造体のプロパティの XML 属性になります。 XAML でインスタンス化されるクラスはパブリック パラメーターなしのコンス トラクターを通常が必要です。 XAML で設定されたプロパティはパブリックである必要があります`set`アクセサー。

基本データ型のプロパティの (`string`、 `double`、`bool`など)、XAML パーサーは、標準を使用して`TryParse`属性の設定をこれらの型に変換するメソッド。 XAML パーサーでは列挙型も簡単に処理し、列挙型のフラグが付いた列挙型のメンバーを組み合わせて、`Flags`属性。

複雑な型 (またはそれらの型のプロパティ) を含めることができます、XAML パーサーを支援するために、 [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/)から派生するクラスを識別する[ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/)からの変換をサポートします。これらの型を文字列値です。 たとえば、 [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/)変換の色の名前と"#rrggbb"などの文字列に`Color`値。

## <a name="property-element-syntax"></a>プロパティ要素構文

XAML では、クラスとそこから作成されたオブジェクトが XML 要素として表されます。 これらと呼ばれます*object 要素*です。 これらのオブジェクトのプロパティのほとんどは、XML 属性として表されます。 これらと呼ばれる*プロパティ属性*です。

プロパティは、単純な文字列として表現できないオブジェクトを設定する必要があります。 このような場合は、XAML をサポートと呼ばれるタグ、*プロパティ要素*ピリオドで区切ったプロパティ名とクラスの名前で構成されます。 Object 要素はできます内のプロパティ要素タグが表示されます。

## <a name="adding-a-xaml-page-to-your-project"></a>プロジェクトに XAML ページを追加します。

Xamarin.Forms ポータブル クラス ライブラリは、これが最初に作成、または既存のプロジェクトに XAML ページを追加するときに、XAML ページを含めることができます。 新しい項目を追加するダイアログ ボックスで、XAML ページを参照する項目を選択または`ContentPage`と XAML です。 (されません、 `ContentView`)。

2 つのファイルが作成されます: ファイル名拡張子 .xaml を持つ XAML ファイルと、拡張機能の c# ファイルです。 xaml.cs です。 C# ファイルと呼ばれる多くの場合、*コード ビハインド*XAML ファイルのです。 分離コード ファイルから派生する部分クラス定義は、`ContentPage`です。 ビルド時に、XAML が解析し、同じクラスに対して別の部分クラス定義を生成します。 この生成されたクラスには、という名前のメソッドが含まれています。`InitializeComponent`分離コード ファイルのコンス トラクターから呼び出されます。

最後に、実行時に、`InitializeComponent`を呼び出すと、すべての XAML ファイルの要素がインスタンス化され、c# コードで作成した場合と同様に初期化されました。

XAML ファイルのルート要素は`ContentPage`します。 ルート タグには、少なくとも 2 つの XML 名前空間宣言には、1 つは Xamarin.Forms 要素、その他の定義が含まれています、`x`要素と属性のすべての XAML 実装に固有のプレフィックス。 ルート タグにも含まれています、`x:Class`から派生したクラスの名前と名前空間を示す属性`ContentPage`です。 これには、分離コード ファイルの名前空間とクラス名と一致します。

XAML およびコードの組み合わせがで示される、 [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)サンプルです。

## <a name="the-xaml-compiler"></a>XAML コンパイラ

Xamarin.Forms は XAML コンパイラが、その使用は任意の使用状況に基づいて、 [ `XamlCompilationAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)です。 XAML がコンパイルされていない場合は、ビルド時に、XAML が解析し、XAML ファイルがここではも解析結果は実行時に、PCL に埋め込まれています。 XAML をコンパイルすると、ビルド プロセス バイナリの形式に XAML を変換し、ランタイム処理の方が効率的です。

## <a name="platform-specificity-in-the-xaml-file"></a>XAML ファイルで、プラットフォームの特異性

XAML では、 [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/)プラットフォームに依存するマークアップを選択するクラスを使用できます。 これは、ジェネリック クラスは、インスタンス化する必要があります、`x:TypeArguments`対象の型と一致する属性。 [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/)クラスは似ていますが、使用をそれほど多くの場合は。

使用`OnPlatform`が、ブックを公開した後に変更されました。 組み合わせてという名前のプロパティで使用された最初`iOS`、 `Android`、および`WinPhone`です。 これは、子で使用して今すぐ[ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/)オブジェクト。 設定、 [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/)プロパティをパブリックで一貫性のある文字列に`const`のフィールド、 [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)クラスです。 設定、 [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/)値と一致するプロパティ、`x:TypeArguments`の属性、`OnPlatform`タグ。

`OnPlatform` 示される、 [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList)サンプルについては、ほぼ同一である XAML のブロックが含まれているためです。 この繰り返し実行するマークアップの存在は、技術がサイズを小さくできることを提案します。

## <a name="the-content-property-attributes"></a>コンテンツ プロパティの属性

一部のプロパティ要素がきわめて頻繁に発生するなど、`<ContentPage.Content>`タグのルート要素で、 `ContentPage`、または`<StackLayout.Children>`タグの子を囲む`StackLayout`です。

持つ 1 つのプロパティを識別するすべてのクラスは許可されて、 [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/)クラスです。 このプロパティのプロパティ要素タグは必要ありません。 `ContentPage` そのコンテンツのプロパティが定義されて`Content`、および`Layout<T>`(元となるクラス`StackLayout`派生) としてそのコンテンツのプロパティが定義されて`Children`です。 これらのプロパティ要素タグが必要ではありません。

プロパティ要素`Label`は`Text`します。

## <a name="formatted-text"></a>書式付きテキスト

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations)サンプルには、設定のいくつかの例が含まれています、`Text`と`FormattedText`プロパティの`Label`します。 XAML では、`Span`オブジェクトの子として表示されます、`FormattedString`オブジェクト。

 複数行文字列を設定すると、`Text`プロパティは、行末の文字は、空白文字に変換されますのコンテンツとして複数行文字列が表示されたら、行末の文字は保持されます、`Label`または`Label.Text`タグ。

 [![テキストのバリエーションが共有のトリプル スクリーン ショット](images/ch07fg03-small.png "書式設定テキスト バリエーション")](images/ch07fg03-large.png#lightbox "テキスト バリエーションの書式設定")



## <a name="related-links"></a>関連リンク

- [7 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [サンプルの第 7 章](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [第 7 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
