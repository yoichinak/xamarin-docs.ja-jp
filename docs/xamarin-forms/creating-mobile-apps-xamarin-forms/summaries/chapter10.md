---
title: 10 章の概要です。 XAML マークアップ拡張機能
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 10 章の概要です。 XAML マークアップ拡張機能'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cc6c3154b7e6535fa7528032fb7a91ad90a0a7f8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241101"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>10 章の概要です。 XAML マークアップ拡張機能

通常は、XAML パーサーが属性値として設定され、基本の .NET データ型の標準の変換に基づくプロパティの型に任意の文字列に変換しますまたは[ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/)派生またはアタッチするプロパティでオブジェクトの種類、[`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

ディクショナリ、または静的プロパティまたはフィールドの値内の項目など、別のソースから、または何らかの計算から属性を設定すると便利な場合があります。

これは、ジョブの*XAML マークアップ拡張機能*します。 XAML マークアップ拡張機能は、名前にかかわらず、*いない*to XML 拡張機能です。 XAML は、常に有効な XML です。

## <a name="the-code-infrastructure"></a>コードのインフラストラクチャ

XAML マークアップ拡張機能が実装するクラス、 [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/)インターフェイスです。 このようなクラスには、単語がよくあります`Extension`その名前の末尾が、通常なしに記述されて XAML そのサフィックス。

XAML のすべての実装では、次の XAML マークアップ拡張機能がサポートされています。

- `x:Static` サポートされています。 [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` サポートされています。 [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` サポートされています。 [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` サポートされています。 [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` サポートされています。 [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

これら 4 つの XAML マークアップ拡張機能は、Xamarin.Forms をなど、XAML の多くの実装でサポートされます。

- `StaticResource` サポートされています。 [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` サポートされています。 [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` サポートされている[ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/)&mdash;で説明した[章 16。データ バインディング](#chapter16)
- `TemplateBinding` サポートされている[ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/)&mdash;ブックで説明されていません。

Xamarin.Forms での他の XAML マークアップ拡張機能が含まれている[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;ブックで説明されていません。

## <a name="accessing-static-members"></a>静的メンバーへのアクセス

使用して、 [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)パブリック静的プロパティ、フィールド、または列挙型メンバーの値に属性を設定する要素。 設定、 [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/)プロパティ、静的メンバーにします。 指定する方が簡単`x:Static`と中かっこでメンバーの名前。 名前、`Member`プロパティが含まれる必要はありません、メンバーだけです。 この一般的な構文、 [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics)サンプルです。 静的フィールド自体がで定義されている、 [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs)クラスです。 この手法をプログラムで使用される定数を確立することができます。

その他の XML 名前空間宣言を参照できますパブリック静的プロパティ、フィールド、または .NET framework で定義された列挙体メンバーで示したように、 [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics)サンプル.

## <a name="resource-dictionaries"></a>リソース ディクショナリ

`VisualElement`クラスという名前のプロパティを定義する[ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)型のオブジェクトに設定できる[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。 XAML 内には、このディクショナリに項目を格納およびでそれらを指定することができます、`x:Key`属性。 リソース ディクショナリに格納されている項目は、項目に対するすべての参照の間で共有されます。

### <a name="staticresource-for-most-purposes"></a>ほとんどの目的で StaticResource

使用するほとんどの場合、 [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)に示すように、リソース ディクショナリから項目を参照するマークアップ拡張機能、 [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing)サンプル. 使用することができます、`StaticResourceExtension`要素または`StaticResource`中かっこ内。

[![リソースの共有のトリプル スクリーン ショット](images/ch10fg03-small.png "リソースの共有")](images/ch10fg03-large.png#lightbox "リソースの共有")

混同しないでください、`x:Static`マークアップ拡張機能と`StaticResource`マークアップ拡張機能です。

### <a name="a-tree-of-dictionaries"></a>辞書のツリー

XAML パーサーを検出した場合、 `StaticResource`、一致するキーのビジュアル ツリーを検索を開始し、内を検索、`ResourceDictionary`アプリケーションの`App`クラスです。 これは、ビジュアル ツリー内の上位のリソース ディクショナリをオーバーライドするビジュアル ツリーの下位のリソース ディクショナリの項目です。 これに示されている、 [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees)サンプルです。

### <a name="dynamicresource-for-special-purposes"></a>特殊な用途で DynamicResource

`StaticResource`マークアップ拡張機能により、ビジュアル ツリーの中にビルド時にディクショナリから取得する項目、`InitializeComponent`呼び出します。 代わりに`StaticResource`は[ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)、ディクショナリ キーへのリンクを保持し、キーの変更によって、項目が参照されている場合にターゲットを更新します。

違い`StaticResource`と`DynamicResource`に示されているが、 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic)サンプルです。

設定されたプロパティ`DynamicResource`で説明したようにバインド可能なプロパティでバックアップする必要があります[第 11 章バインド可能なインフラストラクチャ](chapter11.md)です。

## <a name="lesser-used-markup-extensions"></a>いずれか小さいほうに使用されるマークアップ拡張機能

使用して、 [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)マークアップ拡張機能プロパティに設定を`null`です。

使用して、 [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)マークアップ拡張機能プロパティを設定する、.net`Type`オブジェクト。

使用して[ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)配列を定義します。 配列メンバーの種類を指定するには、設定、[`Type`] プロパティを`x:Type`マークアップ拡張機能です。

## <a name="a-custom-markup-extension"></a>カスタム マークアップ拡張機能

実装するクラスを記述して、独自の XAML マークアップ拡張機能を作成することができます、 [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/)とのインターフェイス、 [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/)メソッドです。

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs)クラスは、これらの要件を満たします。 型の値を作成`Color`という名前のプロパティの値に基づいて`H`、 `S`、 `L`、および`A`です。 このクラスは、Xamarin.Forms ライブラリという名前の最初の項目[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)構築および本書の過程で使用します。

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo)サンプルは、このライブラリを参照して、カスタム マークアップ拡張機能を使用する方法を示します。



## <a name="related-links"></a>関連リンク

- [章 10 のフル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [10 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
