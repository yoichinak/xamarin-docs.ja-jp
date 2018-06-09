---
title: Xamarin.Forms でのアニメーション
description: Xamarin.Forms には、使用したときにも複雑なアニメーションを作成するのに十分な用途の広い、単純なアニメーションを作成するため単純では、自社のアニメーション インフラストラクチャが含まれています。
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 5bc04f638168a10266c20e278481fc0c513afe48
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245027"
---
# <a name="animation-in-xamarinforms"></a>Xamarin.Forms でのアニメーション

_Xamarin.Forms には、使用したときにも複雑なアニメーションを作成するのに十分な用途の広い、単純なアニメーションを作成するため単純では、自社のアニメーション インフラストラクチャが含まれています。_

Xamarin.Forms アニメーション クラスは、徐々 にプロパティを変更する 1 つの値から別の期間の一般的なアニメーションを使用して、ビジュアル要素のさまざまなプロパティを対象します。 Xamarin.Forms アニメーション クラスの XAML インターフェイスがないことに注意してください。 ただしにアニメーションをカプセル化できます[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)し、XAML から参照できます。

## <a name="simple-animationssimplemd"></a>[単純なアニメーション](simple.md)

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスは、回転、拡大縮小、平行移動、およびフェードアウトする単純なアニメーションを構築するために使用できる拡張メソッドを提供[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)インスタンス。 この説明を作成してを使用してアニメーションを取り消し、`ViewExtensions`クラスです。

## <a name="easing-functionseasingmd"></a>[イージング関数](easing.md)

Xamarin.Forms を含む、 [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)クラスを使用すると、アニメーションが高速化する方法を制御する転送関数を指定するか、速度が低下するように実行していることです。 この記事では、定義済みのイージング関数を使用する方法とカスタム イージング関数を作成する方法を示します。

## <a name="custom-animationscustommd"></a>[カスタム アニメーション](custom.md)

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)クラスは、すべての Xamarin.Forms アニメーションでの拡張メソッドでのビルド ブロック、 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)クラスの 1 つまたは複数を作成する`Animation`オブジェクト。 この記事は、使用する方法を示します、`Animation`クラスを作成し、アニメーションをキャンセル、複数のアニメーションを同期およびは既存のアニメーション メソッドでアニメーション化するプロパティをアニメーション化するカスタムのアニメーションを作成します。
