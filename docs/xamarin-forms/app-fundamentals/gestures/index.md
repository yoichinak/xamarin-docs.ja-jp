---
title: Xamarin.Forms のジェスチャ
description: このガイドでは、Xamarin.Forms のジェスチャ認識エンジンを使って、Xamarin.Forms アプリケーションでユーザーとビューのやり取りを検出する方法について説明します。
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
ms.openlocfilehash: 33968fb935e8b69736ac338bfa0479e4f278e64a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106183"
---
# <a name="xamarinforms-gestures"></a>Xamarin.Forms のジェスチャ

_ジェスチャ認識エンジンを使って、Xamarin.Forms アプリケーションでユーザーとビューのやり取りを検出できます。_

Xamarin.Forms の [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) クラスでは、[`View`](xref:Xamarin.Forms.View) インスタンス上でのタップ、ピンチ、パン、スワイプのジェスチャがサポートされています。

## <a name="adding-a-tap-gesture-recognizertapmd"></a>[タップ ジェスチャ認識エンジンの追加](tap.md)

タップ ジェスチャはタップの検出に使われ、[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) クラスを使って認識されます。

## <a name="adding-a-pinch-gesture-recognizerpinchmd"></a>[ピンチ ジェスチャ認識エンジンの追加](pinch.md)

ピンチ ジェスチャは対話型のズームを実行するために使われ、[`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) クラスを使って認識されます。

## <a name="adding-a-pan-gesture-recognizerpanmd"></a>[パン ジェスチャ認識エンジンの追加](pan.md)

パン ジェスチャは、画面周辺の指の動きを検出し、その動きをコンテンツに適用するために使われ、[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) クラスを使って認識されます。

## <a name="adding-a-swipe-gesture-recognizerswipemd"></a>[スワイプ ジェスチャ認識エンジンの追加](swipe.md)

スワイプ ジェスチャが発生するのは、指が画面に沿って水平または垂直方向に動かされたときで、多くの場合コンテンツのナビゲーションを開始するために使われます。 スワイプ ジェスチャは [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) クラスを使って認識されます。
