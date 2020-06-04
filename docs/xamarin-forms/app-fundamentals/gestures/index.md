---
title: Xamarin.Forms のジェスチャ
description: このガイドでは、Xamarin.Forms のジェスチャ認識エンジンを使って、Xamarin.Forms アプリケーションでユーザーとビューのやり取りを検出する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e1e93f74ab8ef6d63213a8fbdc7ec45a794cf55
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137878"
---
# <a name="xamarinforms-gestures"></a>Xamarin.Forms のジェスチャ

_ジェスチャ認識エンジンを使って、Xamarin.Forms アプリケーションでユーザーとビューのやり取りを検出できます。_

Xamarin.Forms の [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) クラスでは、[`View`](xref:Xamarin.Forms.View) インスタンス上でのタップ、ピンチ、パン、スワイプのジェスチャがサポートされています。

## <a name="adding-a-tap-gesture-recognizer"></a>[タップ ジェスチャ認識エンジンの追加](tap.md)

タップ ジェスチャはタップの検出に使われ、[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) クラスを使って認識されます。

## <a name="adding-a-pinch-gesture-recognizer"></a>[ピンチ ジェスチャ認識エンジンの追加](pinch.md)

ピンチ ジェスチャは対話型のズームを実行するために使われ、[`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) クラスを使って認識されます。

## <a name="adding-a-pan-gesture-recognizer"></a>[パン ジェスチャ認識エンジンの追加](pan.md)

パン ジェスチャは、画面周辺の指の動きを検出し、その動きをコンテンツに適用するために使われ、[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) クラスを使って認識されます。

## <a name="adding-a-swipe-gesture-recognizer"></a>[スワイプ ジェスチャ認識エンジンの追加](swipe.md)

スワイプ ジェスチャが発生するのは、指が画面に沿って水平または垂直方向に動かされたときで、多くの場合コンテンツのナビゲーションを開始するために使われます。 スワイプ ジェスチャは [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) クラスを使って認識されます。
