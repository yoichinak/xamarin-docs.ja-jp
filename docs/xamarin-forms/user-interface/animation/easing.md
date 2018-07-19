---
title: Xamarin.Forms でのイージング関数
description: Xamarin.Forms には、アニメーションの速度または実行されているように遅く、そのコントロールを使用すると、転送関数を指定するイージング クラスが含まれています。 この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を示します。
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 1c75771173d94a18c7c1cc5100c64d45bdc32078
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998122"
---
# <a name="easing-functions-in-xamarinforms"></a>Xamarin.Forms でのイージング関数

_Xamarin.Forms には、アニメーションの速度または実行されているように遅く、そのコントロールを使用すると、転送関数を指定するイージング クラスが含まれています。この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を示します。_


[ `Easing` ](xref:Xamarin.Forms.Easing)クラスは、さまざまなアニメーションで使用できるイージング関数を定義します。

- [ `BounceIn` ](xref:Xamarin.Forms.Easing.BounceIn)先頭にアニメーションを bounces イージング機能。
- [ `BounceOut` ](xref:Xamarin.Forms.Easing.BounceOut)最後にアニメーションを bounces イージング機能。
- [ `CubicIn` ](xref:Xamarin.Forms.Easing.CubicIn)アニメーションを加速するイージング関数を緩やかに変化します。
- [ `CubicInOut` ](xref:Xamarin.Forms.Easing.CubicInOut)イージング関数を開始時に、アニメーションを加速および減速のアニメーションを最後にします。
- [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut)減速のアニメーションのイージング関数を簡単にします。
- [ `Linear` ](xref:Xamarin.Forms.Easing.Linear)一定の速度を使用するイージング機能し、イージング関数を既定値は。
- [ `SinIn` ](xref:Xamarin.Forms.Easing.SinIn)イージング機能をスムーズにアニメーションを加速します。
- [ `SinInOut` ](xref:Xamarin.Forms.Easing.SinInOut)イージング機能をスムーズに開始時に、アニメーションを加速し、最後にアニメーションをスムーズに減速します。
- [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut)イージング機能をスムーズにアニメーションを減速します。
- [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn)と、非常に迅速に終了するころに高速化、アニメーションのイージング機能。
- [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut)により迅速に終了するころに減速するアニメーションのイージング機能。

`In`と`Out`サフィックスは、イージング関数によって提供される効果が最後に、またはその両方で、アニメーションの先頭に顕著なを指定します。

さらに、カスタム イージング関数を作成できます。 詳細については、次を参照してください。[カスタム イージング関数](#customeasing)します。

## <a name="consuming-an-easing-function"></a>イージング関数を使用

アニメーションの拡張メソッドにおいて、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスの次のコード例に示す最後のメソッドのパラメーターとして指定するイージング関数を許可します。

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

アニメーションにイージング関数を指定するは、アニメーションの速度は、非線形のようになり、イージング関数によって提供される効果を生成します。 アニメーションを作成するときに、イージング関数を省略すると、既定値を使用するアニメーションが[ `Linear` ](xref:Xamarin.Forms.Easing.Linear)イージング機能で、線形の速度が生成されます。

アニメーションの拡張メソッドを使用しての詳細については、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスを参照してください[単純なアニメーション](~/xamarin-forms/user-interface/animation/simple.md)します。 イージング関数を使用できますが、 [ `Animation` ](xref:Xamarin.Forms.Animation)クラス。 詳細については、次を参照してください。[アニメーション](~/xamarin-forms/user-interface/animation/custom.md)します。

<a name="customeasing" />

## <a name="custom-easing-functions"></a>カスタム イージング関数

カスタム イージング関数を作成する 3 つの主な方法はあります。

1. 受け取るメソッドを作成、`double`引数を返します、`double`結果。
1. `Func<double, double>` を作成します。
1. イージング関数を指定する引数として、 [ `Easing` ](xref:Xamarin.Forms.Easing)コンス トラクター。

すべての 3 つのケースでカスタム イージング関数は 1 の引数の 0 ~ 1 の引数の場合は 0 を返します。 ただし、引数の値を 0 ~ 1 の間の値が返されます。 それぞれの方法でさらに説明するようになりました。

### <a name="custom-easing-method"></a>カスタム イージング メソッド

カスタム イージング関数を使用するメソッドとして定義できますを`double`引数を返します、`double`結果、次のコード例で示した。

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase`メソッドには、0、0.2、0.4、0.6、0.8、および 1 の値を受信した値が切り捨てられます。 そのため、 [ `Image` ](xref:Xamarin.Forms.Image)インスタンスは、不連続なジャンプではなくスムーズに変換されます。

### <a name="custom-easing-func"></a>カスタム イージング関数

としてカスタム イージング関数を定義することも、`Func<double, double>`次のコード例に示すように、します。

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` 2018 年 2 月末日まで、急速に加速をもう一度コースのイージング関数を開始する高速で遅くとコースでは、元に戻し、反転しを表します。 そのため、全体の移動中に、 [ `Image` ](xref:Xamarin.Forms.Image)インスタンスが下方向には、アニメーションの途中でコースを一時的に反転します。

### <a name="custom-easing-constructor"></a>カスタム コンス トラクターを簡略化

引数としてカスタム イージング関数を定義することも、 [ `Easing` ](xref:Xamarin.Forms.Easing)コンス トラクターは、次のコード例で示した。

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

カスタム イージング関数は、ラムダ関数の引数として指定された、 [ `Easing` ](xref:Xamarin.Forms.Easing)コンス トラクターを使用して、`Math.Cos`によってを抑制する低速のドロップ効果を作成する方法、`Math.Exp`メソッド。 そのため、 [ `Image` ](xref:Xamarin.Forms.Image)インスタンスがその最終的な保存場所に表示されるように変換されます。

## <a name="summary"></a>まとめ

この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を説明します。 Xamarin.Forms を含む、 [ `Easing` ](xref:Xamarin.Forms.Easing)クラスをアニメーション高速化する方法を制御する転送関数を指定するか、実行されているように、速度が低下することができます。



## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [イージング関数 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [簡略化](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
