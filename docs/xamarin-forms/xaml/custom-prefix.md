---
title: XAML Namespace を Xamarin.Forms でプレフィックスをお勧めします。
description: XAML の使用量の XAML 名前空間に関連付ける推奨プレフィックスを指定するコントロールの作成者によって XmlnsPrefixAttribute クラスを使用できます。
ms.prod: xamarin
ms.assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: 33f18b3f9c9ddb6ab31ca92e2f192ffad783ec0c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075103"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>XAML Namespace を Xamarin.Forms でプレフィックスをお勧めします。

`XmlnsPrefixAttribute`クラスは、XAML の使用量の XAML 名前空間に関連付ける推奨プレフィックスを指定するコントロールの作成者によって使用できます。 プレフィックスは、XAML へのオブジェクト ツリーのシリアル化をサポートしている場合に役立ちます。 または XAML 編集機能を持つデザイン環境と対話するときにします。 例えば:

- XAML テキスト エディターを使用できます、`XmlnsPrefixAttribute`初期の XAML 名前空間のヒントとして`xmlns`マッピングします。
- XAML デザイン環境を使用できます、`XmlnsPrefixAttribute`オブジェクトをツールボックスから、およびビジュアル デザイン サーフェイスにドラッグしたときに、XAML にマッピングを追加します。

名前空間プレフィックスを使用して、アセンブリ レベルで定義する必要がありますを推奨、`XmlnsPrefixAttribute`を 2 つの引数を受け取るコンス トラクター: XAML 名前空間の識別子を指定する文字列と推奨されるプレフィックスを指定する文字列。

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

プレフィックスは、プレフィックスは、通常 XAML 名前空間に由来するすべてのシリアル化された要素に適用するために、短い文字列を使用してください。 そのため、プレフィックス文字列の長さには、シリアル化された XAML 出力のサイズに影響が及ぶことができます。

> [!NOTE]
> 1 つ以上`XmlnsPrefixAttribute`をアセンブリに適用できます。 たとえば、1 つ以上の XAML 名前空間の種類を定義するアセンブリがあれば、XAML 名前空間ごとに別のプレフィックス値を定義できます。

## <a name="related-links"></a>関連リンク

- [XAML カスタム名前空間スキーマ](custom-namespace-schemas.md)
- [Xamarin.Forms の XAML 名前空間](namespaces.md)
