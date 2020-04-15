---
title: '第 7 章の概要: XAML とコードの比較'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 7 章の概要: XAML とコードの比較'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: ce4dde3716176daf826678809339afb84c25d84a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "61334739"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>第 7 章の概要: XAML とコードの比較

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> このページのメモでは、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

Xamarin.Forms では、Extensible Application Markup Language (XAML) ("ザムル" と読みます) と呼ばれる XML ベースのマークアップ言語がサポートされています。 C# の代わりに XAML を使用して、Xamarin.Forms アプリケーションのユーザー インターフェイスのレイアウトを定義したり、ユーザー インターフェイス要素と基になるデータの間のバインディングを定義したりできます。

## <a name="properties-and-attributes"></a>プロパティと属性

Xamarin.Forms のクラスと構造体は XAML の XML 要素になり、これらのクラスと構造体のプロパティは XML 属性になります。 XAML でインスタンス化するには、通常、クラスにパラメーターなしのパブリック コンストラクターが必要です。 XAML で設定されるプロパティには、パブリック `set` アクセサーが必要です。

基本データ型 (`string`、`double`、`bool` など) のプロパティについては、XAML パーサーにより、標準の `TryParse` メソッドを使用して、属性の設定がこれらの型に変換されます。 また、XAML パーサーを使うと、列挙型を簡単に処理でき、列挙型に `Flags` 属性でフラグが設定されている場合は、列挙型のメンバーを組み合わせることができます。

XAML パーサーを補助するため、より複雑な型 (またはそれらの型のプロパティ) には、文字列値からそれらの型への変換をサポートする [`TypeConverter`](xref:Xamarin.Forms.TypeConverter) から派生したクラスを識別する [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute) を含めることができます。 たとえば、[`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) では、色の名前と文字列 ("#rrggbb" など) が `Color` の値に変換されます。

## <a name="property-element-syntax"></a>プロパティ要素の構文

XAML では、クラスおよびクラスから作成されたオブジェクトは、XML 要素として表現されます。 これらは、"*オブジェクト要素*" と呼ばれます。 これらのオブジェクトのほとんどのプロパティは、XML 属性として表現されます。 これらは "*プロパティ属性*" と呼ばれます。

場合によっては、単純な文字列として表現できないオブジェクトにプロパティを設定する必要があります。 そのような場合のために XAML でサポートされている "*プロパティ要素*" と呼ばれるタグは、クラス名とプロパティ名をピリオドで区切ったもので構成されています。 それにより、オブジェクト要素をプロパティ要素タグのペアに含めることができます。

## <a name="adding-a-xaml-page-to-your-project"></a>プロジェクトへの XAML ページの追加

Xamarin.Forms のポータブル クラス ライブラリには、最初に作成するときに XAML ページを含めることができます。または、既存のプロジェクトに XAML ページを追加することもできます。 新しい項目を追加するダイアログで、XAML ページまたは `ContentPage` と XAML を参照する項目を選択します。 (`ContentView` ではありません)。

> [!NOTE]
> この章が書かれた後で、Visual Studio のオプションが変更されています。

2 つのファイル (ファイル名拡張子が .xaml の XAML ファイルと、拡張子が .xaml.cs の C# ファイル) が作成されます。 C# ファイルは、XAML ファイルの "*分離コード*" と呼ばれることがよくあります。 分離コード ファイルは、`ContentPage` から派生する部分クラス定義です。 ビルド時に、XAML が解析されて、同じクラスに対して別の部分クラス定義が生成されます。 この生成されたクラスに含まれる `InitializeComponent` という名前のメソッドは、分離コード ファイルのコンストラクターから呼び出されます。

実行時には、`InitializeComponent` の呼び出しの最後で、C# コードで作成された場合と同じように、XAML ファイルのすべての要素がインスタンス化されて初期化されます。

XAML ファイルのルート要素は `ContentPage` です。 ルート タグには、少なくとも 2 つの XML 名前空間宣言が含まれています。1 つは Xamarin.Forms 要素用で、もう 1 つでは、すべての XAML 実装に組み込まれている要素と属性に対して `x` プレフィックスが定義されます。 ルート タグには、`ContentPage` から派生されたクラスの名前空間と名前を示す `x:Class` 属性も含まれます。 これにより、分離コード ファイル内の名前空間とクラス名がマッチングされます。

XAML とコードの組み合わせは、[**CodePlusXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) サンプルで示されています。

## <a name="the-xaml-compiler"></a>XAML コンパイラ

Xamarin.Forms には XAML コンパイラがありますが、その使用は [`XamlCompilationAttribute`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) の使用に基づいて省略可能です。 XAML がコンパイルされていない場合、XAML はビルド時に解析され、XAML ファイルは PCL に埋め込まれます。このファイルも実行時に解析されます。 XAML がコンパイルされている場合は、ビルド プロセスによって XAML がバイナリ形式に変換され、ランタイムの処理の効率が向上します。

## <a name="platform-specificity-in-the-xaml-file"></a>XAML ファイルでのプラットフォームの特異性

XAML では、[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) クラスを使用して、プラットフォームに依存するマークアップを選択できます。 これはジェネリック クラスであり、対象の型と一致する `x:TypeArguments` 属性を使用してインスタンス化する必要があります。 [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) クラスは似ていますが、それほど頻繁には使用されません。

`OnPlatform` の使用方法は、本書が発行されてから変更されました。 もともとは、`iOS`、`Android`、`WinPhone` という名前のプロパティと共に使用されていました。 現在は、子の [`On`](xref:Xamarin.Forms.On) オブジェクト共に使用されます。 [`Platform`](xref:Xamarin.Forms.On.Platform) プロパティを、[`Device`](xref:Xamarin.Forms.Device) クラスのパブリック `const` フィールドと一致する文字列に設定します。 [`Value`](xref:Xamarin.Forms.On.Value) プロパティを、`OnPlatform` タグの `x:TypeArguments` 属性と一致する値に設定します。

`OnPlatform` は [**ScaryColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) サンプルで示されていますが、そのように呼ばれているのはほぼ同一の XAML のブロックが含まれているためです。 この繰り返されるマークアップの存在は、それを減らすために手法を利用できることを示唆しています。

## <a name="the-content-property-attributes"></a>コンテンツ プロパティ属性

`ContentPage` のルート要素の `<ContentPage.Content>` タグや、`StackLayout` の子を囲む `<StackLayout.Children>` タグなど、一部のプロパティ要素は非常に頻繁に発生します。

すべてのクラスでは、そのクラスの [`ContentPropertyAttribute`](xref:Xamarin.Forms.ContentPropertyAttribute) で 1 つのプロパティを示すことができます。 このプロパティの場合、プロパティ要素タグは必要ありません。 `ContentPage` ではコンテンツ プロパティは `Content` と定義されており、`Layout<T>` (`StackLayout` の派生元クラス) ではコンテンツ プロパティは `Children` と定義されています。 これらのプロパティ要素タグは必要ありません。

`Text` のプロパティ要素は `Label` です。

## <a name="formatted-text"></a>書式付きテキスト

[**TextVariations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) サンプルには、`Label` の `Text` プロパティと `FormattedText` プロパティを設定する複数の例が含まれています。 XAML では、`Span` オブジェクトは `FormattedString` オブジェクトの子として出現します。

 複数行文字列が `Text` プロパティに設定されている場合は、行末文字が空白文字に変換されますが、`Label` または `Label.Text` タグの内容として複数行文字列が使用されている場合は、行末文字は保持されます。

 [![テキスト バリエーション共有のトリプル スクリーンショット](images/ch07fg03-small.png "書式設定済みテキストのバリエーション")](images/ch07fg03-large.png#lightbox "書式設定済みテキストのバリエーション")

## <a name="related-links"></a>関連リンク

- [第 7 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [第 7 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [第 7 章の F# サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML の基礎](~/xamarin-forms/xaml/xaml-basics/index.md)
