---
title: Xamarin.Forms で XAML フィールド修飾子
description: X:fieldmodifier 名前空間属性では、名前付き XAML 要素の生成されたフィールドのアクセス レベルを指定します。
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209507"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin.Forms で XAML フィールド修飾子

_`x:FieldModifier`名前空間属性が名前付き XAML 要素の生成されたフィールドのアクセス レベルを指定します。_

## <a name="overview"></a>概要

属性の有効な値は次のとおりです。

- `Public` – 指定した XAML 要素の生成されたフィールドを`public`です。
- `NotPublic` – 生成された XAML 要素のフィールドを指定`internal`アセンブリにします。

属性の値が設定されていない場合、生成されたフィールドの要素になります`private`です。

次の条件を満たす必要があります、`x:FieldModifier`処理する属性。

- 最上位の XAML 要素を有効にする必要があります`x:Class`です。
- 現在の XAML 要素が、`x:Name`指定します。

次の XAML では、属性の設定の例を示します。

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` XAML クラスのアクセス レベルを指定する属性を使用することはできません。
