---
title: 第 7 章の概要です。 コードと XAML
description: Xamarin.Forms によるモバイル アプリの作成。第 7 章の概要です。 コードと XAML
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: ce4dde3716176daf826678809339afb84c25d84a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334739"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>第 7 章の概要です。 コードと XAML

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

Xamarin.Forms では、Extensible Application Markup Language と呼ばれる XML ベースのマークアップ言語または XAML (「ザムル」) をサポートしています。 XAML では、c# する代わりに、Xamarin.Forms アプリケーションのユーザー インターフェイスのレイアウトの定義とユーザー インターフェイス要素間のバインドを定義して、基になるデータを提供します。

## <a name="properties-and-attributes"></a>プロパティと属性

Xamarin.Forms クラスと構造体に、XAML 内の XML 要素と、これらのクラスと構造のプロパティが XML 属性になります。 XAML でインスタンス化される、クラスは、パブリック コンス トラクターを一般が必要です。 XAML で設定されたプロパティを公開する必要がありますが`set`アクセサー。

基本データ型のプロパティの (`string`、 `double`、`bool`など)、XAML パーサーは、標準を使用して`TryParse`属性の設定をこれらの型に変換するメソッド。 XAML パーサーの列挙型を処理も簡単にし、列挙型のフラグが付いた列挙型メンバーを組み合わせて、`Flags`属性。

複雑な型 (またはそれらの型のプロパティ) を含めることができます、XAML パーサーを支援するために、 [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute)から派生したクラスを識別する[ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter)からの変換をサポートしていますこれらの型を文字列値。 たとえば、 [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter)に変換の色の名前と"#rrggbb"などの文字列に`Color`値。

## <a name="property-element-syntax"></a>プロパティ要素構文

XAML では、クラスとそこから作成されたオブジェクトは、XML 要素として表現されます。 これらと呼ばれます。 *object 要素*します。 これらのオブジェクトのプロパティのほとんどは、XML 属性として表現されます。 これらと呼ばれる*プロパティ属性*します。

場合がありますプロパティは、単純な文字列として表現できないオブジェクトに設定する必要があります。 このような場合は、XAML はというタグをサポートしています、*プロパティ要素*クラス名とピリオドで区切ったプロパティ名で構成されます。 オブジェクト要素は、プロパティ要素タグのペア内で、表示できます。

## <a name="adding-a-xaml-page-to-your-project"></a>プロジェクトに XAML ページを追加します。

Xamarin.Forms ポータブル クラス ライブラリは、これが最初に作成、または既存のプロジェクトに XAML ページを追加するときに、XAML ページを含めることができます。 新しい項目を追加するダイアログ ボックスで、XAML ページを参照する項目を選択または`ContentPage`と XAML。 (いない、 `ContentView`)。

> [!NOTE]
> この章が書き込まれるために、visual Studio のオプションが変わりました。

2 つのファイルが作成されます。 ファイル名拡張子 .xaml、XAML ファイルと、拡張機能の c# ファイル。 xaml.cs。 C# ファイルと呼ばれる多くの場合、*コード ビハインド*XAML ファイルの。 分離コード ファイルは、部分クラス定義から派生した`ContentPage`します。 ビルド時に、XAML が解析され、同じクラスに対して別の部分クラス定義が生成されます。 この生成されたクラスには、という名前のメソッドが含まれています。`InitializeComponent`分離コード ファイルのコンス トラクターから呼び出されます。

最後に、実行時に、 `InitializeComponent` XAML ファイルの要素がインスタンス化され、c# コードで作成した場合と同様に初期化されているすべての呼び出し。

XAML ファイルのルート要素は`ContentPage`します。 ルート タグには、少なくとも 2 つの XML 名前空間宣言は、1 つは Xamarin.Forms 要素、その他の定義が含まれている、`x`要素と属性のすべての XAML 実装に固有のプレフィックス。 ルート タグにも含まれています、`x:Class`から派生したクラスの名前と名前空間を示す属性を`ContentPage`します。 これには、分離コード ファイルの名前空間とクラス名と一致します。

XAML とコードの組み合わせが示されている、 [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)サンプル。

## <a name="the-xaml-compiler"></a>XAML コンパイラ

Xamarin.Forms は XAML コンパイラが、その使用はオプションの使用状況に基づいて、 [ `XamlCompilationAttribute`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)します。 XAML がコンパイルされていない場合は、ビルド時に、XAML が解析され、場所に実行時に解析されても、PCL に XAML ファイルが埋め込まれました。 XAML をコンパイルすると、ビルド プロセスは、バイナリの形式に、XAML を変換し、ランタイム処理が効率的です。

## <a name="platform-specificity-in-the-xaml-file"></a>XAML ファイルでプラットフォーム固有の要素

XAML、 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1)プラットフォームに依存するマークアップを選択するクラスを使用できます。 これは、ジェネリック クラスでありでインスタンス化する必要があります、`x:TypeArguments`ターゲット型と一致する属性。 [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1)クラスは似ていますが、使用される多くの場合、大幅に低下します。

使用`OnPlatform`が、本が出版されて以降に変更します。 組み合わせてという名前のプロパティの使用がもともと`iOS`、 `Android`、および`WinPhone`します。 子と共に使用するようになりました[ `On` ](xref:Xamarin.Forms.On)オブジェクト。 設定、 [ `Platform` ](xref:Xamarin.Forms.On.Platform)プロパティをパブリックで一貫性のある文字列に`const`のフィールド、 [ `Device` ](xref:Xamarin.Forms.Device)クラス。 設定、 [ `Value` ](xref:Xamarin.Forms.On.Value)プロパティで一貫性のある値を`x:TypeArguments`の属性、`OnPlatform`タグ。

`OnPlatform` 方法については、 [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList)サンプルについてと呼ばれるものとほぼ同じ XAML の要素が含まれています。 この繰り返し実行するマークアップの存在は、手法が、サイズを小さくできることをお勧めします。

## <a name="the-content-property-attributes"></a>コンテンツ プロパティの属性

一部のプロパティ要素はかなり頻繁に発生するなど、`<ContentPage.Content>`タグのルート要素を`ContentPage`、または`<StackLayout.Children>`タグの子を囲む`StackLayout`します。

プロパティを 1 つを識別するためにすべてのクラスが許可されている、 [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute)クラス。 このプロパティは、プロパティ要素タグは必要ありません。 `ContentPage` としてこのコンテンツのプロパティを定義します。 `Content`、と`Layout<T>`(元のクラス`StackLayout`派生) としてこのコンテンツのプロパティを定義します。`Children`します。 これらのプロパティ要素タグは必要ありません。

プロパティ要素`Label`は`Text`します。

## <a name="formatted-text"></a>書式付きテキスト

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations)サンプルには、いくつかの設定の例が含まれています、`Text`と`FormattedText`プロパティの`Label`します。 XAML で`Span`オブジェクトの子として表示されます、`FormattedString`オブジェクト。

 複数行文字列に設定した場合、`Text`プロパティは、行末の文字は空白文字に変換されますの内容として、複数行文字列が表示されたら、行末の文字は保持されます、`Label`または`Label.Text`タグ。

 [![共有テキスト バリエーションの 3 倍になるスクリーン ショット](images/ch07fg03-small.png "テキストの書式設定されたバリエーション")](images/ch07fg03-large.png#lightbox "書式設定されたテキストのバリエーション")

## <a name="related-links"></a>関連リンク

- [第 7 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [第 7 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [第 7 章F#サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML の基礎](~/xamarin-forms/xaml/xaml-basics/index.md)
