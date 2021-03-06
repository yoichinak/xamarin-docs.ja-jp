---
title: アニメーションXamarin.Forms
description: Xamarin.Formsには、単純なアニメーションを作成するための簡単な独自のアニメーションインフラストラクチャが用意されています。また、複雑なアニメーションを作成するのに十分な汎用性もあります。
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 88a671c4d28d62a5f73e90a7b2fa9c45b7dbe8b1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84129000"
---
# <a name="animation-in-xamarinforms"></a>アニメーションXamarin.Forms

_Xamarin. フォームには、単純なアニメーションを作成するための簡単な独自のアニメーションインフラストラクチャが含まれています。また、複雑なアニメーションを作成するのに十分な汎用性もあります。_

Xamarin.Formsアニメーションクラスは、ビジュアル要素のさまざまなプロパティをターゲットにします。一般的なアニメーションでは、一定期間にわたってプロパティがある値から別の値に徐々に変化します。 アニメーションクラスには XAML インターフェイスがないことに注意 Xamarin.Forms してください。 ただし、アニメーションは[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)にカプセル化し、XAML から参照できます。

## <a name="simple-animations"></a>[単純なアニメーション](simple.md)

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)クラスには、インスタンスを回転、拡大縮小、変換、およびフェードする単純なアニメーションを作成するために使用できる拡張メソッドが用意されて [`VisualElement`](xref:Xamarin.Forms.VisualElement) います。 この記事では、クラスを使用してアニメーションを作成およびキャンセルする方法について説明し `ViewExtensions` ます。

## <a name="easing-functions"></a>[イージング関数](easing.md)

Xamarin.Formsには、実行中の [`Easing`](xref:Xamarin.Forms.Easing) アニメーションの速度と速度を制御する転送関数を指定できるクラスが含まれています。 この記事では、定義済みのイージング関数を使用する方法と、カスタムイージング関数を作成する方法について説明します。

## <a name="custom-animations"></a>[カスタム アニメーション](custom.md)

クラスは、 [`Animation`](xref:Xamarin.Forms.Animation) すべてのアニメーションのビルドブロックであり Xamarin.Forms 、クラスの拡張メソッドを使用して [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 1 つ以上のオブジェクトを作成し `Animation` ます。 この記事では、クラスを使用して `Animation` アニメーションを作成およびキャンセルする方法、複数のアニメーションを同期する方法、および既存のアニメーションメソッドでアニメーション化されていないプロパティをアニメーション化するカスタムアニメーションを作成する方法について説明します。
