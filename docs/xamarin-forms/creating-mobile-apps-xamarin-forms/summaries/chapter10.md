---
title: '第 10 章の概要: XAML マークアップ拡張'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 10 章の概要: XAML マークアップ拡張'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 076e9f5155492e5a69d906c587b24495fe39d3f1
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334327"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>第 10 章の概要: XAML マークアップ拡張

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

通常、XAML パーサーでは、基本的な .NET データ型の標準変換に基づいて、あるいはプロパティまたはその型に [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute) でアタッチされている [`TypeConverter`](xref:Xamarin.Forms.TypeConverter) 派生物に基づいて、属性値として設定されている文字列が、プロパティの型に変換されます。

しかし、ディクショナリ内の項目、静的なプロパティやフィールドの値、または何らかの種類の計算など、別のソースから属性を設定すると便利な場合があります。

これは、"*XAML マークアップ拡張*" の仕事です。 その名前にかかわらず、XAML マークアップ拡張は XML に対する拡張機能では "*ありません*"。 XAML は常に正規の XML です。

## <a name="the-code-infrastructure"></a>コード インフラストラクチャ

XAML マークアップ拡張は、[`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) インターフェイスを実装するクラスです。 そのようなクラスでは、多くの場合、名前の末尾に `Extension` という単語が付いていますが、XAML では通常はそのサフィックスなしで使用されます。

次の XAML マークアップ拡張は、XAML のすべての実装でサポートされています。

- [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) によってサポートされる `x:Static`
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) によってサポートされる `x:Reference`
- [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) によってサポートされる `x:Type`
- [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) によってサポートされる `x:Null`
- [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) によってサポートされる `x:Array`

次の 4 つの XAML マークアップ拡張は、Xamarin.Forms を含む XAML の多くの実装でサポートされています。

- [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) によってサポートされる `StaticResource`
- [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) によってサポートされる `DynamicResource`
- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) によってサポートされる `Binding` &mdash; 以下を参照: [第 16 章データ バインディング](chapter16.md)
- [`TemplateBindingExtension`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) によってサポートされる `TemplateBinding` &mdash; 本書では説明されていません

Xamarin.Forms には、[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) に関連する追加の XAML マークアップ拡張が含まれます。

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) &mdash; 本書では説明されていません

## <a name="accessing-static-members"></a>静的メンバーへのアクセス

属性を、パブリック静的プロパティ、フィールド、または列挙型のメンバーの値に設定するには、[`x:Static`](xref:Xamarin.Forms.Xaml.StaticExtension) 要素を使用します。 [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) プロパティを静的メンバーに設定します。 通常は、中かっこで `x:Static` とメンバー名を指定する方が簡単です。 `Member` プロパティの名前を含める必要はありません。、メンバー自体だけで十分です。 この一般的な構文については、[**SharedStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) サンプルを参照してください。 静的フィールド自体は、[`AppConstants`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) クラスで定義されています。 この手法を使用すると、プログラム全体で使用される定数を設定できます。

XML 名前空間の宣言を追加することで、.NET Framework で定義されているパブリック静的プロパティ、フィールド、または列挙メンバーを参照できます。これについては、[**SystemStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) サンプルを参照してください。

## <a name="resource-dictionaries"></a>リソース ディクショナリ

`VisualElement` クラスで定義されている [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) という名前のプロパティは、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 型のオブジェクトに設定できます。 XAML 内では、このディクショナリに項目を格納し、`x:Key` 属性で識別できます。 リソース ディクショナリに格納されている項目は、その項目へのすべての参照間で共有されます。

### <a name="staticresource-for-most-purposes"></a>ほとんどの目的に対する StaticResource

ほとんどの場合、[`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) マークアップ拡張を使用して、リソース ディクショナリから項目を参照します。これについては、[**ResourceSharing**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) サンプルを参照してください。 中かっこ内では `StaticResourceExtension` 要素または `StaticResource` を使用できます。

[![リソース共有のトリプル スクリーンショット](images/ch10fg03-small.png "リソースの共有")](images/ch10fg03-large.png#lightbox "リソースの共有")

`x:Static` マークアップ拡張と `StaticResource` マークアップ拡張を混同しないようにしてください。

### <a name="a-tree-of-dictionaries"></a>ディクショナリのツリー

XAML パーサーでは、`StaticResource` が検出されると、ビジュアル ツリーでの一致するキーの検索が開始され、その後アプリケーションの `App` クラスの `ResourceDictionary` が検索されます。 これにより、ビジュアル ツリー内のでより深いリソース ディクショナリの項目で、ビジュアル ツリーの上位のリソース ディクショナリをオーバーライドできます。 これについては、[**ResourceTrees**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) サンプルを参照してください。

### <a name="dynamicresource-for-special-purposes"></a>特別な目的に対する DynamicResource

`InitializeComponent` の呼び出しの間にビジュアル ツリーが作成されるときに、`StaticResource` マークアップ拡張によってディクショナリから項目が取得されます。 `StaticResource` の代わりに [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) を使用すると、ディクショナリ キーへのリンクが保持され、キーによって参照されている項目が変更されるとターゲットが更新されます。

`StaticResource` と `DynamicResource` の違いについては、[**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) サンプルを参照してください。

「[第11章バインド可能なインフラストラクチャ](chapter11.md)」で説明されているように、`DynamicResource` によって設定されるプロパティは、バインド可能なプロパティによって裏付けられている必要があります。

## <a name="lesser-used-markup-extensions"></a>あまり使用されないマークアップ拡張

プロパティを `null` に設定するには、[`x:Null`](xref:Xamarin.Forms.Xaml.NullExtension) マークアップ拡張を使用します。

プロパティを .NET の `Type` オブジェクトに設定するには、[`x:Type`](xref:Xamarin.Forms.Xaml.TypeExtension) マークアップ拡張を使用します。

配列を定義するには、[`x:Array`](xref:Xamarin.Forms.Xaml.ArrayExtension) を使用します。 [`Type`] プロパティを `x:Type` マークアップ拡張に設定することで、配列メンバーの型を指定します。

## <a name="a-custom-markup-extension"></a>カスタム マークアップ拡張

[`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) インターフェイスと [`ProvideValue`](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) メソッドを実装するクラスを記述することで、独自の XAML マークアップ拡張を作成できます。

それらの要件は、[`HslColorExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) クラスによって満たされます。 それでは、`H`、`S`、`L`、`A` という名前のプロパティの値に基づいて、`Color` 型の値が作成されます。 このクラスは、本書を通して構築されて使用される [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) という名前の Xamarin.Forms ライブラリの最初の項目です。

[**CustomExtensionDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) サンプルでは、このライブラリを参照し、カスタム マークアップ拡張を使用する方法が示されています。

## <a name="related-links"></a>関連リンク

- [第 10 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [第 10 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
