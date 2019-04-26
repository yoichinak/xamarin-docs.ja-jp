---
title: Xamarin.Forms の XAML フィールド修飾子
description: X:fieldmodifier 名前空間の属性には、生成されたフィールドの名前付き XAML 要素のアクセス レベルを指定します。
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075299"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin.Forms の XAML フィールド修飾子

_`x:FieldModifier`名前空間属性が生成されたフィールドの名前付き XAML 要素のアクセス レベルを指定します。_

## <a name="overview"></a>概要

属性の有効な値は次のとおりです。

- `Public` – 指定する XAML 要素の生成されたフィールドが`public`します。
- `NotPublic` – 生成された XAML 要素のフィールドを指定します`internal`アセンブリ。

属性の値が設定されていない場合、要素の生成されたフィールドになります`private`します。

次の条件を満たす必要があります、`x:FieldModifier`処理する属性。

- 最上位の XAML 要素を有効にする必要があります`x:Class`します。
- 現在の XAML 要素が、`x:Name`指定します。

次の XAML は、属性の設定の例を示します。

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier`属性を使用して XAML クラスのアクセス レベルを指定することはできません。
