---
title: XAML マークアップ拡張機能
description: この記事では、Xamarin.Forms の XAML マークアップ拡張機能を使用して、リテラル テキスト文字列以外のソースから設定する要素の属性を許可することで、電源と XAML の柔軟性を拡張する方法について説明します。
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 40a50ecf93c8b77577e8e74df2e134b0ce3f80b6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767539"
---
# <a name="xaml-markup-extensions"></a>XAML マークアップ拡張機能

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML マークアップ拡張機能では、リテラル テキスト文字列以外のソースから設定する要素の属性を許可することで、電源と XAML の柔軟性を拡張するのに役立ちます。

設定する通常など、`Color`プロパティの`BoxView`次のように。

```xaml
<BoxView Color="Blue" />
```

または、16 進数の RGB 色の値に設定することができます。

```xaml
<BoxView Color="#FF0080" />
```

いずれの場合も、テキスト文字列の設定`Color`属性に変換を`Color`値を[ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter)クラス。

設定する方が、`Color`属性、リソース ディクショナリに格納されている値か、作成したクラスの静的プロパティの値または型のプロパティから`Color` ページで、別の要素から構築された、または色合い、鮮やかさ、および明るさの値を区切ります。

これらすべてのオプションは、XAML マークアップ拡張機能を使用できます。 しかし、"マークアップ拡張機能" という語句を使用しないようにしてください。XAML マークアップ拡張機能は、XML の拡張機能では*ありません*。 XAML マークアップ拡張機能でも XAML は常に有効な XML です。

マークアップ拡張機能は、要素の属性を表現する別の方法にすぎません。 XAML マークアップ拡張機能は、通常は、中かっこで囲まれた属性の設定で識別できます。

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

属性の設定中かっこが*常に*XAML マークアップ拡張機能。 ただし、わかりますが、XAML マークアップ拡張機能もせずに参照できる中かっこを使用します。

この記事では、2 つの部分に分かれています。

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[XAML マークアップ拡張の使用](consuming.md)  

Xamarin.Forms で定義されている XAML マークアップ拡張機能を使用します。

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[XAML マークアップ拡張の作成](creating.md)

独自のカスタム XAML マークアップ拡張を記述します。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Xamarin.Forms book から XAML マークアップ拡張機能の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
