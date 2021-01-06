---
title: イージング関数 Xamarin.Forms
description: Xamarin.Forms にはイージングクラスが含まれています。このクラスを使用すると、アニメーションの実行速度を制御する転送関数を指定できます。 この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ec4c16249aadce668b9fe33dad661e1f7e7ee9e
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939018"
---
# <a name="easing-functions-in-no-locxamarinforms"></a>イージング関数 Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin.Forms にはイージングクラスが含まれています。このクラスを使用すると、アニメーションの実行速度を制御する転送関数を指定できます。この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。_

クラスは、 [`Easing`](xref:Xamarin.Forms.Easing) アニメーションで使用できるイージング関数の数を定義します。

- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn)イージング関数は、アニメーションを先頭にバウンスします。
- [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)イージング関数は、アニメーションを最後にバウンスします。
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)イージング関数は、アニメーションをゆっくりと高速化します。
- イージング関数は、最初のアニメーションを加速させると共に、 [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut) 最後にアニメーションを減速します。
- [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut)イージング関数は、アニメーションを迅速に減速します。
- [`Linear`](xref:Xamarin.Forms.Easing.Linear)イージング関数は、一定の速度を使用します。これは、既定のイージング関数です。
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)イージング関数は、アニメーションをスムーズに加速します。
- [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)イージング関数は、アニメーションを開始時にスムーズに加速させると共に、最終的にアニメーションを減速します。
- [`SinOut`](xref:Xamarin.Forms.Easing.SinOut)イージング関数は、アニメーションをスムーズに減速します。
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)イージング関数を使うと、アニメーションが短時間で終了します。
- [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)イージング関数を使うと、アニメーションは最後にすばやく減速します。

`In`サフィックスと `Out` サフィックスは、イージング関数によって提供される効果が、アニメーションの先頭、末尾、またはその両方で目立つかどうかを示します。

さらに、カスタムイージング関数を作成することもできます。 詳細については、「 [カスタムイージング関数](#custom-easing-functions)」を参照してください。

## <a name="consuming-an-easing-function"></a>イージング関数の使用

クラスのアニメーション拡張メソッドを使用すると、 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 次のコード例に示すように、イージング関数を final メソッドパラメーターとして指定できます。

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

アニメーションのイージング関数を指定することにより、アニメーションベロシティが非線形になり、イージング関数によって提供される効果が生成されます。 アニメーションを作成するときにイージング関数を省略すると、アニメーションは既定の [`Linear`](xref:Xamarin.Forms.Easing.Linear) イージング関数を使用します。これにより、線形速度が生成されます。

> [!NOTE]
> Xamarin.Forms 5.0 には、イージング関数の文字列形式を適切な列挙メンバーに変換する型コンバーターが含まれてい [`Easing`](xref:Xamarin.Forms.Easing) ます。 この型コンバーターは、 `Easing` XAML で設定されている型のプロパティに対して自動的に呼び出されます。

クラスでアニメーション拡張メソッドを使用する方法の詳細については [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 、「 [単純なアニメーション](~/xamarin-forms/user-interface/animation/simple.md)」を参照してください。 イージング関数は、クラスでも使用でき [`Animation`](xref:Xamarin.Forms.Animation) ます。 詳細については、「 [カスタムアニメーション](~/xamarin-forms/user-interface/animation/custom.md)」を参照してください。

## <a name="custom-easing-functions"></a>カスタムイージング関数

カスタムイージング関数を作成するには、主に次の3つの方法があります。

1. 引数を受け取り、結果を返すメソッドを作成 `double` し `double` ます。
1. `Func<double, double>` を作成します。
1. コンストラクターの引数としてイージング関数を指定し [`Easing`](xref:Xamarin.Forms.Easing) ます。

3つのすべてのケースで、カスタムイージング関数は、引数が0の場合は0を返し、1の引数には1を返します。 ただし、0と1の引数値の間には任意の値を返すことができます。 ここでは、それぞれのアプローチについて説明します。

### <a name="custom-easing-method"></a>カスタムイージングメソッド

`double` `double` 次のコード例に示すように、カスタムイージング関数を引数を受け取るメソッドとして定義し、結果を返すことができます。

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

この `CustomEase` メソッドは、入力された値を0、0.2、0.4、0.6、0.8、および1の値に切り捨てます。 このため、 [`Image`](xref:Xamarin.Forms.Image) インスタンスはスムーズに移動されるのではなく、不連続のジャンプで変換されます。

### <a name="custom-easing-func"></a>カスタムイージング Func

`Func<double, double>`次のコード例に示すように、カスタムイージング関数をとして定義することもできます。

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

は、 `CustomEaseFunc` 高速で開始し、コースを反転して反転するイージング関数を表し、もう一度コースを反転してすぐに終了します。 このため、インスタンスの全体的な移動 [`Image`](xref:Xamarin.Forms.Image) は下に向かって行われますが、アニメーションの途中でコースが一時的に反転されます。

### <a name="custom-easing-constructor"></a>カスタムイージングコンストラクター

[`Easing`](xref:Xamarin.Forms.Easing)次のコード例に示すように、カスタムイージング関数をコンストラクターの引数として定義することもできます。

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

カスタムイージング関数は、コンストラクターへのラムダ関数引数として指定され、 [`Easing`](xref:Xamarin.Forms.Easing) メソッドを使用し `Math.Cos` て、メソッドによって抑制される低速のドロップ効果を作成し `Math.Exp` ます。 そのため、 [`Image`](xref:Xamarin.Forms.Image) インスタンスは変換され、最終的な場所にドロップされたように見えます。

## <a name="summary"></a>まとめ

この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。 Xamarin.Forms には、実行中の [`Easing`](xref:Xamarin.Forms.Easing) アニメーションの速度と速度を制御する転送関数を指定できるクラスが含まれています。

## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [イージング関数 (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [イージング](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)