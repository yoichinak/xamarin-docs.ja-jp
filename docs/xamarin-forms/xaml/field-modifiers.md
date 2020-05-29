---
title: XAML フィールド修飾子Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: db00f522b71a8993ef0f7f6cf5070813ce07396a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138126"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>XAML フィールド修飾子Xamarin.Forms

`x:FieldModifier`名前空間属性は、名前付き XAML 要素に対して生成されるフィールドのアクセスレベルを指定します。 属性の有効な値は次のとおりです。

- `private`– XAML 要素に対して生成されたフィールドは、そのフィールドが宣言されているクラスの本体内でのみアクセス可能であることを指定します。
- `public`– XAML 要素に対して生成されたフィールドにアクセス制限がないことを指定します。
- `protected`– XAML 要素の生成されたフィールドに、そのクラス内および派生クラスのインスタンスからアクセスできることを指定します。
- `internal`– XAML 要素の生成されたフィールドが、同じアセンブリ内の型内でのみアクセス可能であることを指定します。
- `notpublic`– XAML 要素の生成されたフィールドが、同じアセンブリ内の型内でのみアクセス可能であることを指定します。

既定では、属性の値が設定されていない場合、要素に対して生成されるフィールドはになり `private` ます。

> [!NOTE]
> 属性の値は、によって小文字に変換されるため、任意の大文字小文字を使用でき Xamarin.Forms ます。

属性を処理するには、次の条件を満たす必要があり `x:FieldModifier` ます。

- 最上位の XAML 要素は有効なである必要があり `x:Class` ます。
- 現在の XAML 要素には、指定されたがあり `x:Name` ます。

次の XAML は、属性の設定例を示しています。

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> 属性を使用して、 `x:FieldModifier` XAML クラスのアクセスレベルを指定することはできません。
