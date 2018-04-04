---
title: イージング関数
description: Xamarin.Forms には、アニメーションの速度または速度が低下するように実行していること、そのコントロールを使用すると、転送関数を指定するイージング クラスが含まれています。 この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を示します。
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: e9171b885bdf5958b6969719301a1d7dad51d95b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="easing-functions"></a>イージング関数

_Xamarin.Forms には、アニメーションの速度または速度が低下するように実行していること、そのコントロールを使用すると、転送関数を指定するイージング クラスが含まれています。この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を示します。_


[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)クラスは、多数のアニメーションで使用できるイージング関数を定義します。

- [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/)先頭のアニメーションを bounces イージング機能。
- [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)最後に、アニメーションを bounces イージング機能。
- [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/)アニメーションを加速イージング関数を緩やかに変化します。
- [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)開始時に、アニメーションを短縮し、最後に、アニメーションを減速イージング機能。
- [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/)アニメーションを減速イージング関数を簡単にします。
- [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/)定数の速度を使用するイージング関数とイージング関数を既定値は。
- [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/)アニメーション イージング関数をスムーズに高速化します。
- [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)イージング関数をスムーズに開始時に、アニメーションを短縮し、最後に、アニメーションをスムーズに減速します。
- [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/)アニメーションを減速イージング関数をスムーズに動作します。
- [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/)アニメーション、終了するころに迅速に非常に高速化するイージング機能させます。
- [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)アニメーション、終了するころに迅速に減速するイージング機能させます。

`In`と`Out`イージング関数によって提供される効果が最後に、またはその両方に、アニメーションの先頭に顕著なサフィックスを示すです。

また、カスタム イージング関数を作成することができます。 詳細については、次を参照してください。[カスタム イージング関数](#customeasing)です。

## <a name="consuming-an-easing-function"></a>イージング関数を使用

アニメーションの拡張メソッドにおいて、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスは、次のコード例に示すように、最終的なメソッドのパラメーターとして指定するイージング関数を許可します。

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

アニメーションにイージング関数を指定すると、アニメーションの速度、非線形なりイージング関数によって提供される効果が生成されます。 既定値を使用するアニメーション アニメーションを作成するときにイージング関数を省略すると、 [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/)イージング機能で、線形の速度が生成されます。

アニメーションの拡張メソッドの使用の詳細については、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスを参照してください[単純なアニメーション](~/xamarin-forms/user-interface/animation/simple.md)です。 イージング関数を使用できますが、 [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)クラスです。 詳細については、次を参照してください。[アニメーション](~/xamarin-forms/user-interface/animation/custom.md)です。

<a name="customeasing" />

## <a name="custom-easing-functions"></a>カスタム イージング関数

カスタム イージング関数を作成する 3 つの主な方法はあります。

1. 受け取るメソッドを作成、`double`引数を返す、`double`結果。
1. `Func<double, double>` を作成します。
1. イージング関数を指定の引数として、 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)コンス トラクターです。

カスタム イージング関数は、3 つすべてのケース 1 の引数には、0、および 1 の引数の場合は 0 を返す必要があります。 ただし、引数の値を 0 と 1 の間で任意の値を返すことができます。 それぞれの方法を順番に説明するようになりましたはします。

### <a name="custom-easing-method"></a>カスタム イージング メソッド

カスタム イージング関数を受け取るメソッドとして定義できる、`double`引数、および返します、`double`結果、次のコード例で示したように。

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase`メソッドには、0、0.2、0.4、0.6、0.8、および 1 の値を受信した値が切り捨てられます。 したがって、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)インスタンスは、不連続なジャンプではなくスムーズに変換されます。

### <a name="custom-easing-func"></a>カスタム イージング Func

としてカスタム イージング関数を定義することも、`Func<double, double>`次のコード例に示すように、します。

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func`終了するころに加速をもう一度コースの高速、速度が低下しおよびコースを反転させます、反転を開始するイージング関数を表します。 そのため、全体の移動中に、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)インスタンスが下方向には、アニメーションの途中でコースを一時的に反転します。

### <a name="custom-easing-constructor"></a>カスタム イージング コンス トラクター

引数としてカスタム イージング関数を定義することも、 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)コンス トラクターは、次のコード例で示したようにします。

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

カスタム イージング関数は、ラムダ関数の引数として指定された、 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)コンス トラクターを使用して、`Math.Cos`によって抑制される低速なドロップ効果を作成する方法、`Math.Exp`メソッドです。 したがって、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)インスタンスがその最終的な保存場所に表示されるように変換されます。

## <a name="summary"></a>まとめ

この記事には、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法が示されています。 Xamarin.Forms を含む、 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)クラスを使用すると、アニメーションが高速化する方法を制御する転送関数を指定するか、速度が低下するように実行していることです。



## <a name="related-links"></a>関連リンク

- [非同期サポートの概要](~/cross-platform/platform/async.md)
- [イージング関数 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [イージング](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
