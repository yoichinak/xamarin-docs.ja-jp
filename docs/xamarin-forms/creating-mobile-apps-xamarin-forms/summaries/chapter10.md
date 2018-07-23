---
title: 第 10 章の概要です。 XAML マークアップ拡張機能
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 10 章の概要。 XAML マークアップ拡張機能'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 74f7e2846a9e8d8390a8322c57db0845718bbba7
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157004"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>第 10 章の概要です。 XAML マークアップ拡張機能

通常は、XAML パーサーが .NET の基本データ型の標準変換に基づいてプロパティの型に属性値として設定する任意の文字列に変換しますまたは[ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter)派生物が、プロパティまたはでオブジェクトの種類にアタッチします[`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute)。

ディクショナリ、または静的プロパティまたはフィールドの値の項目など、さまざまなソースから、または何らかの計算から属性を設定する便利な場合があります。

これは、ジョブの*XAML マークアップ拡張機能*します。 XAML マークアップ拡張機能は、その名前にかかわらず*いない*to XML 拡張機能。 XAML は、常に有効な XML です。

## <a name="the-code-infrastructure"></a>コードのインフラストラクチャ

XAML マークアップ拡張機能が実装するクラス、 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension)インターフェイス。 このようなクラスは、多くの場合、word が`Extension`にその名前の末尾が、通常が表示されます XAML でそのサフィックスが付いていません。

XAML のすべての実装では、次の XAML マークアップ拡張機能がサポートされています。

- `x:Static` サポートされています。 [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` サポートされています。 [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` サポートされています。 [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` サポートされています。 [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` サポートされています。 [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

これら 4 つの XAML マークアップ拡張機能は、Xamarin.Forms を含む XAML の多くの実装でサポートされます。

- `StaticResource` サポートされています。 [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` サポートされています。 [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` サポートされている[ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension)&mdash;で説明した[16 章です。データ バインディング](#chapter16)
- `TemplateBinding` サポートされている[ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension)&mdash;書籍では説明しません

Xamarin.Forms での他の XAML マークアップ拡張機能が含まれている[ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;この書籍では説明しません

## <a name="accessing-static-members"></a>静的メンバーへのアクセス

使用して、 [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension)パブリック静的プロパティ、フィールド、または列挙型メンバーの値に属性を設定する要素。 設定、 [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member)プロパティを静的なメンバーにします。 指定する方が簡単`x:Static`と中かっこでメンバー名。 名前、`Member`プロパティが含まれる必要はありません、メンバーだけです。 この一般的な構文、 [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics)サンプル。 静的フィールド自体がで定義されている、 [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs)クラス。 この手法をプログラムで使用される定数を確立することができます。

追加の XML 名前空間宣言で参照できますパブリック静的プロパティ、フィールド、または、.NET framework で定義された列挙体のメンバーで示した、 [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics)サンプル.

## <a name="resource-dictionaries"></a>リソース ディクショナリ

`VisualElement`クラスという名前のプロパティを定義する[ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)型のオブジェクトに設定できる[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 XAML、内には、このディクショナリに項目を格納およびでそれらを指定することができます、`x:Key`属性。 リソース ディクショナリに格納されている項目は、項目に対するすべての参照の間で共有されます。

### <a name="staticresource-for-most-purposes"></a>ほとんどの目的で StaticResource

使用するほとんどの場合、 [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension)に示すように、リソース ディクショナリから項目を参照するマークアップ拡張機能、 [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing)サンプル. 使用することができます、`StaticResourceExtension`要素または`StaticResource`中かっこ内で。

[![リソースの共有の 3 倍になるスクリーン ショット](images/ch10fg03-small.png "リソースの共有")](images/ch10fg03-large.png#lightbox "リソースの共有")

混同しないでください、`x:Static`マークアップ拡張機能と`StaticResource`マークアップ拡張機能。

### <a name="a-tree-of-dictionaries"></a>ディクショナリのツリー

XAML パーサーが検出した場合、 `StaticResource`、一致するキーの場合、ビジュアル ツリーの検索を開始し、内を検索、`ResourceDictionary`アプリケーションの`App`クラス。 これにより、ビジュアル ツリー内の上位のリソース ディクショナリをオーバーライドするビジュアル ツリーの下位のリソース ディクショナリの項目です。 これは、方法については、 [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees)サンプル。

### <a name="dynamicresource-for-special-purposes"></a>特別な目的で DynamicResource

`StaticResource`マークアップ拡張機能により、アイテムは、ビジュアル ツリーの中にビルド時に、ディクショナリから取得する、`InitializeComponent`呼び出します。 代わりに`StaticResource`は[ `DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)ディクショナリのキーへのリンクを維持され、キーの変更によって、項目が参照されている場合は、ターゲットを更新します。

間の差`StaticResource`と`DynamicResource`方法については、 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic)サンプル。

設定するプロパティ`DynamicResource`で説明したように、バインド可能なプロパティでバックアップする必要があります[第 11 章バインド可能なインフラストラクチャ](chapter11.md)します。

## <a name="lesser-used-markup-extensions"></a>使用されていないマークアップ拡張機能

使用して、 [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension)プロパティを設定するマークアップ拡張機能`null`します。

使用して、 [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) .net プロパティを設定するマークアップ拡張機能`Type`オブジェクト。

使用[ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension)配列を定義します。 配列メンバーの種類を指定を設定して、[`Type`] プロパティを`x:Type`マークアップ拡張機能。

## <a name="a-custom-markup-extension"></a>カスタム マークアップ拡張機能

実装するクラスを記述することで、独自の XAML マークアップ拡張機能を作成することができます、 [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension)とのインターフェイスを[ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider))メソッド。

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs)クラスは、これらの要件を満たします。 型の値を作成します`Color`という名前のプロパティの値に基づいて`H`、 `S`、 `L`、および`A`します。 このクラスは、Xamarin.Forms ライブラリをという名前の最初の項目[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)構築およびこの本の過程で使用します。

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo)サンプルは、このライブラリを参照およびカスタム マークアップ拡張機能を使用する方法を示します。

## <a name="related-links"></a>関連リンク

- [第 10 章 – フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [第 10 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML マークアップ拡張](~/xamarin-forms/xaml/markup-extensions/index.md)
