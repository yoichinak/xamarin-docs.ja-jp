---
title: Xamarin.Forms でのアニメーション
description: Xamarin.Forms には、複雑なアニメーションを作成するのに十分な汎用性の高いさらに、単純なアニメーションを作成するために簡単ですが独自のアニメーションのインフラストラクチャが含まれています。
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998305"
---
# <a name="animation-in-xamarinforms"></a>Xamarin.Forms でのアニメーション

_Xamarin.Forms には、複雑なアニメーションを作成するのに十分な汎用性の高いさらに、単純なアニメーションを作成するために簡単ですが独自のアニメーションのインフラストラクチャが含まれています。_

Xamarin.Forms のアニメーション クラスは、段階的にプロパティを変更する 1 つの値から別に一定時間の一般的なアニメーションでのビジュアル要素は、さまざまなプロパティを対象します。 Xamarin.Forms のアニメーション クラスの XAML インターフェイスがないことに注意してください。 ただし、アニメーションでカプセル化できます[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)し、XAML から参照します。

## <a name="simple-animationssimplemd"></a>[単純なアニメーション](simple.md)

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスは、回転、拡大縮小、平行移動、およびフェードする単純なアニメーションの作成に使用できる拡張メソッドを提供します。 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)インスタンス。 この記事は、作成してを使用してアニメーションをキャンセルでは、`ViewExtensions`クラス。

## <a name="easing-functionseasingmd"></a>[イージング関数](easing.md)

Xamarin.Forms を含む、 [ `Easing` ](xref:Xamarin.Forms.Easing)クラスをアニメーション高速化する方法を制御する転送関数を指定するか、実行されているように、速度が低下することができます。 この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を示します。

## <a name="custom-animationscustommd"></a>[カスタム アニメーション](custom.md)

[ `Animation` ](xref:Xamarin.Forms.Animation)クラスは拡張メソッドで、すべての Xamarin.Forms アニメーションのビルディング ブロック、 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions)クラスの 1 つまたは複数作成`Animation`オブジェクト。 この記事では、使用する方法を示します、`Animation`クラスを作成し、アニメーションをキャンセル、複数のアニメーションを同期するには、既存のアニメーションの方法でアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成します。
