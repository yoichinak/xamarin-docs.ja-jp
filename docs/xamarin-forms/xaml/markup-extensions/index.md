---
title: XAML マークアップ拡張機能
description: 属性が設定されている XAML からのソースの範囲を拡張します。
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: b81bc4b31edd1d8b8f5f43f97885c38e889dd32c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-markup-extensions"></a>XAML マークアップ拡張機能

XAML マークアップ拡張機能では、リテラル テキスト文字列以外のソースから設定する要素の属性を許可することで、電源と XAML の柔軟性を拡張するのに役立ちます。

たとえば、通常、設定、`Color`のプロパティ`BoxView`次のようにします。

```xaml
<BoxView Color="Blue" />
```

または、RGB 色の 16 進数の値に設定することができます。

```xaml
<BoxView Color="#FF0080" />
```

どちらの場合、テキスト文字列に設定、`Color`属性に変換されます、`Color`値に、 [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/)クラスです。

設定する代わりにたい場合があります、`Color`属性リソース ディクショナリに格納されている値、作成したクラスの静的プロパティの値からまたは型のプロパティから`Color` ページで、別の要素から構築された、または色合い、鮮やかさ、明るさの値を区切ります。

これらすべてのオプションは、XAML マークアップ拡張機能の使用可能です。 でも、語句「マークアップ拡張機能」がコンピューターを保護する: XAML マークアップ拡張機能は*いない*to XML 拡張機能です。 XAML マークアップ拡張機能を使用しても XAML は常に有効な XML です。 

マークアップ拡張機能は、要素の属性を表すさまざまな方法では実際にです。 XAML マークアップ拡張機能は、中かっこで囲まれた属性の設定によって通常識別は。

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

中かっこで属性の設定が*常に*XAML マークアップ拡張機能です。 ただし、説明するように、XAML マークアップ拡張機能参照することも、中かっこを使用せずします。

この記事は、2 つの部分に分かれています。

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[XAML マークアップ拡張の使用](consuming.md)  

Xamarin.Forms で定義されている XAML マークアップ拡張機能を使用します。

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[XAML マークアップ拡張の作成](creating.md) 

独自のカスタム XAML マークアップ拡張を書き込みます。



## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 帳から XAML マークアップ拡張機能の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
