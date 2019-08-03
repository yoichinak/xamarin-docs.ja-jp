---
title: Xamarin. Forms の XAML フィールド修飾子
description: X:FieldModifier namespace 属性は、名前付き XAML 要素に対して生成されるフィールドのアクセスレベルを指定します。
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/02/2019
ms.openlocfilehash: 0f6050de943ca9878cf41b448d44bf222689be56
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739446"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin. Forms の XAML フィールド修飾子

名前`x:FieldModifier`空間属性は、名前付き XAML 要素に対して生成されるフィールドのアクセスレベルを指定します。 属性の有効な値は次のとおりです。

- `private`– XAML 要素に対して生成されたフィールドは、そのフィールドが宣言されているクラスの本体内でのみアクセス可能であることを指定します。
- `public`– XAML 要素に対して生成されたフィールドにアクセス制限がないことを指定します。
- `protected`– XAML 要素の生成されたフィールドに、そのクラス内および派生クラスのインスタンスからアクセスできることを指定します。
- `internal`– XAML 要素の生成されたフィールドが、同じアセンブリ内の型内でのみアクセス可能であることを指定します。
- `notpublic`– XAML 要素の生成されたフィールドが、同じアセンブリ内の型内でのみアクセス可能であることを指定します。

既定では、属性の値が設定されていない場合、要素`private`に対して生成されるフィールドはになります。

> [!NOTE]
> 属性の値は、形式によって小文字に変換されるため、任意の大文字小文字を使用できます。

`x:FieldModifier`属性を処理するには、次の条件を満たす必要があります。

- 最上位の XAML 要素は有効`x:Class`なである必要があります。
- 現在の XAML 要素には`x:Name` 、指定されたがあります。

次の XAML は、属性の設定例を示しています。

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> `x:FieldModifier`属性を使用して、XAML クラスのアクセスレベルを指定することはできません。
