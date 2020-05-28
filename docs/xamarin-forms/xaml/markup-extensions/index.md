---
title: ''
description: この記事では、 Xamarin.Forms xaml マークアップ拡張機能を使用して、リテラルテキスト文字列以外のソースから要素属性を設定できるようにすることで、xaml のパワーと柔軟性を拡張する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 568cffc335f28b1a47f3278ad061d851ebef84b6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130391"
---
# <a name="xaml-markup-extensions"></a>XAML マークアップ拡張

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML マークアップ拡張機能は、リテラルテキスト文字列以外のソースから要素属性を設定できるようにすることで、XAML のパワーと柔軟性を拡張するのに役立ちます。

たとえば、通常、次 `Color` のようにのプロパティを設定し `BoxView` ます。

```xaml
<BoxView Color="Blue" />
```

または、16進数の RGB 色の値に設定することもできます。

```xaml
<BoxView Color="#FF0080" />
```

どちらの場合も、属性に設定されたテキスト文字列 `Color` は、 `Color` クラスによって値に変換され [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) ます。

代わりに、 `Color` リソースディクショナリに格納されている値、または作成したクラスの静的プロパティの値、またはページ上の別の要素の型のプロパティから、また `Color` は別の色相、鮮やかさ、明度の値から構築された属性を設定することをお勧めします。

これらのオプションはすべて、XAML マークアップ拡張機能を使用して実行できます。 しかし、"マークアップ拡張機能" という語句を除いて、XAML マークアップ拡張機能は XML の拡張機能では*ありません*。 Xaml マークアップ拡張機能を使用している場合でも、XAML は常に有効な XML です。

マークアップ拡張機能は、実際には要素の属性を表す別の方法にすぎません。 通常、XAML マークアップ拡張機能は、中かっこで囲まれた属性設定で識別できます。

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

中かっこ内のすべての属性設定は、*常に*XAML マークアップ拡張機能です。 ただし、このように、中かっこを使用せずに XAML マークアップ拡張機能を参照することもできます。

この記事は、次の2つの部分で構成されています。

## <a name="consuming-xaml-markup-extensions"></a>[XAML マークアップ拡張の使用](consuming.md)  

「」で定義されている XAML マークアップ拡張機能を使用し Xamarin.Forms ます。

## <a name="creating-xaml-markup-extensions"></a>[XAML マークアップ拡張の作成](creating.md)

独自のカスタム XAML マークアップ拡張機能を作成します。

## <a name="related-links"></a>関連リンク

- [マークアップ拡張機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [本の XAML マークアップ拡張機能の章 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [動的なスタイル](~/xamarin-forms/user-interface/styles/dynamic.md)
- [データ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
