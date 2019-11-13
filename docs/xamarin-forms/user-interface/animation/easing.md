---
title: Xamarin のイージング関数
description: Xamarin. フォームにはイージングクラスが含まれています。このクラスを使用すると、アニメーションの実行速度を制御する転送関数を指定できます。 この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 56ea31d1e1be8bbad4a27dd7ffd844aa03f75bbb
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842828"
---
# <a name="easing-functions-in-xamarinforms"></a>Xamarin のイージング関数

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin. フォームにはイージングクラスが含まれています。このクラスを使用すると、アニメーションの実行速度を制御する転送関数を指定できます。この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。_

[`Easing`](xref:Xamarin.Forms.Easing)クラスは、アニメーションで使用できるイージング関数の数を定義します。

- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn)イージング関数は、アニメーションを先頭にバウンスします。
- [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)イージング関数は、最後にアニメーションをバウンスします。
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)イージング関数は、アニメーションを遅く加速します。
- [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)イージング関数は、最初のアニメーションを加速させると共に、最後にアニメーションを減速します。
- [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut)イージング関数は、アニメーションをすばやく減速します。
- [`Linear`](xref:Xamarin.Forms.Easing.Linear)イージング関数は、一定の速度を使用します。これは、既定のイージング関数です。
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)イージング関数は、アニメーションをスムーズに加速します。
- [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)イージング関数は、アニメーションを開始時にスムーズに加速させると共に、最後にアニメーションを減速します。
- [`SinOut`](xref:Xamarin.Forms.Easing.SinOut)イージング関数は、アニメーションをスムーズに減速します。
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn)イージング関数を使うと、アニメーションが短時間で終了します。
- [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)イージング関数を使うと、アニメーションは最終的にすばやく減速します。

`In` と `Out` のサフィックスは、イージング関数によって提供される効果が、アニメーションの先頭、末尾、またはその両方で目立つかどうかを示します。

さらに、カスタムイージング関数を作成することもできます。 詳細については、「[カスタムイージング関数](#customeasing)」を参照してください。

## <a name="consuming-an-easing-function"></a>イージング関数の使用

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスのアニメーション拡張メソッドを使用すると、次のコード例に示すように、イージング関数を final メソッドパラメーターとして指定できます。

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

アニメーションのイージング関数を指定することにより、アニメーションベロシティが非線形になり、イージング関数によって提供される効果が生成されます。 アニメーションを作成するときにイージング関数を省略すると、アニメーションでは、既定の[`Linear`](xref:Xamarin.Forms.Easing.Linear)イージング関数が使用されます。これにより、線形速度が生成されます。

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスでアニメーション拡張メソッドを使用する方法の詳細については、「[単純なアニメーション](~/xamarin-forms/user-interface/animation/simple.md)」を参照してください。 イージング関数は、 [`Animation`](xref:Xamarin.Forms.Animation)クラスでも使用できます。 詳細については、「[カスタムアニメーション](~/xamarin-forms/user-interface/animation/custom.md)」を参照してください。

<a name="customeasing" />

## <a name="custom-easing-functions"></a>カスタムイージング関数

カスタムイージング関数を作成するには、主に次の3つの方法があります。

1. `double` 引数を受け取り、`double` 結果を返すメソッドを作成します。
1. `Func<double, double>` を作成します。
1. [`Easing`](xref:Xamarin.Forms.Easing)コンストラクターの引数としてイージング関数を指定します。

3つのすべてのケースで、カスタムイージング関数は、引数が0の場合は0を返し、1の引数には1を返します。 ただし、0と1の引数値の間には任意の値を返すことができます。 ここでは、それぞれのアプローチについて説明します。

### <a name="custom-easing-method"></a>カスタムイージングメソッド

次のコード例に示すように、カスタムイージング関数は、`double` 引数を受け取るメソッドとして定義し、`double` 結果を返すことができます。

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

`CustomEase` メソッドは、入力値を0、0.2、0.4、0.6、0.8、および1の値に切り捨てます。 したがって、 [`Image`](xref:Xamarin.Forms.Image)インスタンスはスムーズに移動されるのではなく、不連続のジャンプで変換されます。

### <a name="custom-easing-func"></a>カスタムイージング Func

次のコード例に示すように、カスタムイージング関数を `Func<double, double>`として定義することもできます。

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

`CustomEaseFunc` は、迅速に開始し、コースの速度を低下させて反転するイージング関数を表し、もう一度コースを反転してすぐに終了します。 したがって、 [`Image`](xref:Xamarin.Forms.Image)インスタンスの全体的な移動は下向きになりますが、アニメーションの途中でコースが一時的に反転されます。

### <a name="custom-easing-constructor"></a>カスタムイージングコンストラクター

次のコード例に示すように、カスタムイージング関数は、 [`Easing`](xref:Xamarin.Forms.Easing)コンストラクターの引数として定義することもできます。

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

カスタムイージング関数は、 [`Easing`](xref:Xamarin.Forms.Easing)コンストラクターのラムダ関数引数として指定され、`Math.Cos` メソッドを使用して、`Math.Exp` メソッドによって抑制される低速のドロップ効果を作成します。 したがって、 [`Image`](xref:Xamarin.Forms.Image)インスタンスは変換され、最終的な場所にドロップしたように見えます。

## <a name="summary"></a>まとめ

この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。 Xamarin. Forms には[`Easing`](xref:Xamarin.Forms.Easing)クラスが含まれています。このクラスを使用すると、アニメーションの実行速度を制御する転送関数を指定できます。

## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [イージング関数 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [イージング](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
