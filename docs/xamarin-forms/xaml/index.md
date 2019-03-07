---
title: eXtensible Application Markup Language (XAML)
description: XAML は、ユーザー インターフェイスの定義に使用できる宣言型マークアップ言語です。 ユーザー インターフェイスは、実行時の動作が分離コード ファイルで定義されているときに、XAML 構文を使用して XML ファイルで定義されます。
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
---

# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML は、ユーザー インターフェイスの定義に使用できる宣言型マークアップ言語です。ユーザー インターフェイスは、実行時の動作が分離コード ファイルで定義されているときに、XAML 構文を使用して XML ファイルで定義されます。_

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016:XAML のマスターになります。**

> [!NOTE]
> お試し、 [XAML Standard プレビュー](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML の基礎](xaml-basics/index.md)

XAML では、コードではなく、マークアップを使用して、Xamarin.Forms アプリケーションのユーザー インターフェイスを定義できます。 Xamarin.Forms アプリケーションで XAML が必要なことはありませんが、理解できる使いやすい、あり方が視覚的に一貫性のあると同等のコードよりも簡潔です。 XAML は、一般的なモデル-ビュー-ビューモデル (MVVM) アプリケーションのアーキテクチャで使用するため特に適しています。XAML では、XAML ベースのデータ バインディングによって、ビューモデルのコードにリンクされているビューを定義します。

## <a name="xaml-compilationxamlcmd"></a>[XAML のコンパイル](xamlc.md)

XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。 この記事では、XAMLC、およびその特典を使用する方法について説明します。

## <a name="xaml-previewerxaml-previewermd"></a>[XAML プレビューアー](xaml-previewer.md)

[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer.md)ページのサイドを入力するとレンダリングされますが、ユーザー インターフェイスを確認できるように、XAML マークアップでのライブ プレビューを表示します。

## <a name="xaml-namespacesnamespacesmd"></a>[XAML 名前空間](namespaces.md)

XAML を使用して、 `xmlns` XML 名前空間宣言属性。 この記事では、XAML 名前空間の構文を紹介し、型にアクセスする XAML 名前空間を宣言する方法を示します。

## <a name="xaml-custom-namespace-schemascustom-namespace-schemasmd"></a>[XAML カスタム名前空間スキーマ](custom-namespace-schemas.md)

XAML 名前空間のカスタム スキーマを定義すること、`XmlnsDefinitionAttribute`クラスは、カスタムの URL と 1 つまたは複数の CLR 名前空間の間のマッピングを指定します。 カスタムの名前空間のスキーマは、XAML 名前空間の宣言で使用できます。

## <a name="xaml-namespace-recommended-prefixescustom-prefixmd"></a>[XAML Namespace プレフィックスをお勧めします](custom-prefix.md)

`XmlnsPrefixAttribute`クラスは、XAML の使用量の XAML 名前空間に関連付ける推奨プレフィックスを指定するコントロールの作成者によって使用できます。

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML マークアップ拡張](markup-extensions/index.md)

XAML には、値または単純な文字列で表現できる内容を超えるオブジェクトに属性を設定するためのマークアップ拡張機能が含まれています。 これらは、定数、静的なプロパティとフィールド、リソース ディクショナリ、およびデータ バインドの参照が含まれます。

## <a name="field-modifiersfield-modifiersmd"></a>[フィールド修飾子](field-modifiers.md)

`x:FieldModifier`名前空間属性が生成されたフィールドの名前付き XAML 要素のアクセス レベルを指定します。

## <a name="passing-argumentspassing-argumentsmd"></a>[引数の受け渡し](passing-arguments.md)

既定以外のコンス トラクターまたはファクトリ メソッドへの引数を渡すには、XAML を使用できます。 この記事では、工場出荷時のメソッドを呼び出すと、ジェネリック引数の型を指定する、コンス トラクターに引数を渡すに使用できる XAML 属性の使用を示します。

## <a name="bindable-propertiesbindable-propertiesmd"></a>[連結可能プロパティ](bindable-properties.md)

Xamarin.Forms では、共通言語ランタイム (CLR) のプロパティの機能は、バインド可能なプロパティが拡張されます。 バインド可能なプロパティは、特殊な種類のプロパティ、プロパティの値が Xamarin.Forms プロパティ システムによって追跡されます。 この記事では、バインド可能なプロパティは、概要を示し、作成し、これらを使用する方法を示します。

## <a name="attached-propertiesattached-propertiesmd"></a>[添付プロパティ](attached-properties.md)

添付プロパティは、特殊な種類のバインド可能なプロパティ、1 つのクラスで定義されているが、他のオブジェクトにアタッチされ、クラスを含む属性として XAML で認識可能なプロパティ名はピリオドで区切られました。 この記事では、添付プロパティは、概要を示し、作成し、これらを使用する方法を示します。

## <a name="resource-dictionariesresource-dictionariesmd"></a>[リソース ディクショナリ](resource-dictionaries.md)

XAML リソースは、2 回以上使用できるオブジェクトの定義です。 A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用を許可します。 この記事で作成および使用する方法を示します、 `ResourceDictionary`、いずれかのマージする方法と`ResourceDictionary`別にします。

## <a name="loading-xaml-at-runtimeruntime-loadmd"></a>[実行時の XAML の読み込み](runtime-load.md)

XAML の読み込みおよび実行時に解析できる、 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)拡張メソッド。
