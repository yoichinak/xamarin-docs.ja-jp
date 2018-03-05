---
title: eXtensible Application Markup Language (XAML)
description: "XAML は、ユーザー インターフェイスを定義するために使用する宣言型マークアップ言語です。 ユーザー インターフェイスは、実行時の動作が、分離コード ファイルで定義されているときに、XAML 構文を使用して XML ファイルで定義されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/24/2016
ms.openlocfilehash: a6e31fa9da7a5764d9a7fd04aa73d7d246143384
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML は、ユーザー インターフェイスを定義するために使用する宣言型マークアップ言語です。ユーザー インターフェイスは、実行時の動作が、分離コード ファイルで定義されているときに、XAML 構文を使用して XML ファイルで定義されます。_

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**XAML マスターになる 2016 を展開します。**

> [!NOTE]
> 試してみてから、 [XAML Standard プレビュー](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML の基礎](xaml-basics/index.md)

XAML では、開発者はコードではなく、マークアップを使用して Xamarin.Forms アプリケーションでユーザー インターフェイスを定義します。 Xamarin.Forms プログラムでは、XAML は必要はありませんが、使いやすい、方が視覚的に一貫したと同等のコードよりも簡潔です。 XAML は、人気のあるモデル View-viewmodel (MVVM) アプリケーションのアーキテクチャで使用する場合に特に適して: XAML を XAML ベースのデータ バインディングによって ViewModel コードにリンクされているビューを定義します。

## <a name="xaml-compilationxamlcmd"></a>[XAML のコンパイル](xamlc.md)

XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。 この記事では、XAMLC と利点を使用する方法について説明します。

## <a name="xaml-previewerxaml-previewermd"></a>[XAML プレビューアー](xaml-previewer.md)

[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer.md)で発表された Xamarin 進化 2016 はアルファ チャネルでのテストに使用します。

## <a name="xaml-namespacesnamespacesmd"></a>[XAML 名前空間](namespaces.md)

XAML を使用して、`xmlns`名前空間の宣言の XML 属性です。 この記事では、XAML 名前空間の構文を紹介し、型にアクセスする XAML 名前空間を宣言する方法を示します。

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML マークアップ拡張](markup-extensions/index.md)

XAML には属性の値または単純な文字列で表現できる内容を超えるオブジェクトを設定するマークアップ拡張機能が含まれます。 定数、静的なプロパティとフィールド、リソース ディクショナリ、およびデータ バインドの参照が含まれます。

## <a name="passing-argumentspassing-argumentsmd"></a>[引数の受け渡し](passing-arguments.md)

XAML は、既定以外のコンス トラクターまたはファクトリ メソッドへの引数を渡すために使用できます。 この記事では、ファクトリ メソッドを呼び出すと、汎用引数の型を指定する、コンス トラクターに引数を渡すために使用できる XAML 属性の使用方法を示します。

## <a name="bindable-propertiesbindable-propertiesmd"></a>[連結可能プロパティ](bindable-properties.md)

Xamarin.Forms では、共通言語ランタイム (CLR) のプロパティの機能がバインド可能なプロパティで拡張されます。 バインド可能なプロパティは、特殊な型、プロパティの Xamarin.Forms プロパティ システムによって、プロパティの値を追跡する場所です。 この記事では、バインド可能なプロパティは、の概要について説明し、作成し、それらを使用する方法を示します。

## <a name="attached-propertiesattached-propertiesmd"></a>[関連付けられたプロパティ](attached-properties.md)

添付プロパティは、特殊な種類のバインド可能なプロパティを 1 つのクラスで定義されているが、その他のオブジェクトにアタッチされているを含むフォルダーを属性として XAML で認識可能なクラスおよびプロパティ名、ピリオドで区切られます。 この記事では、アタッチされるプロパティは、の概要について説明し、作成し、それらを使用する方法を示します。

## <a name="resource-dictionariesresource-dictionariesmd"></a>[リソース ディクショナリ](resource-dictionaries.md)

XAML リソースは、2 回以上使用できるオブジェクトの定義です。 A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)リソースを 1 つの場所で定義されている、Xamarin.Forms アプリケーション全体で再利用を許可します。 この記事の内容を作成および使用する方法を示しています、 `ResourceDictionary`、いずれかのマージする方法と`ResourceDictionary`別にします。
