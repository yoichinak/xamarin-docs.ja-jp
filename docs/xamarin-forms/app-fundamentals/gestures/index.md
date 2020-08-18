---
title: Xamarin.Forms のジェスチャ
description: このガイドでは、Xamarin.Forms のジェスチャ認識エンジンを使って、Xamarin.Forms アプリケーションでユーザーとビューのやり取りを検出する方法について説明します。
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/04/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7528afd0971cf06eb69df4ed7c08c3fd6dcc9e22
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917828"
---
# <a name="no-locxamarinforms-gestures"></a>Xamarin.Forms のジェスチャ

_ジェスチャ認識エンジンを使って、Xamarin.Forms アプリケーションでユーザーとビューのやり取りを検出できます。_

Xamarin.Forms の [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) クラスでは、[`View`](xref:Xamarin.Forms.View) インスタンス上でのタップ、ピンチ、パン、スワイプ、ドラッグ アンド ドロップの各ジェスチャがサポートされています。

## <a name="add-a-tap-gesture-recognizer"></a>[タップ ジェスチャ認識エンジンを追加する](tap.md)

タップ ジェスチャはタップの検出に使われ、[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) クラスを使って認識されます。

## <a name="add-a-pinch-gesture-recognizer"></a>[ピンチ ジェスチャ認識エンジンを追加する](pinch.md)

ピンチ ジェスチャは対話型のズームを実行するために使われ、[`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) クラスを使って認識されます。

## <a name="add-a-pan-gesture-recognizer"></a>[パン ジェスチャ認識エンジンを追加する](pan.md)

パン ジェスチャは、画面周辺の指の動きを検出し、その動きをコンテンツに適用するために使われ、[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) クラスを使って認識されます。

## <a name="add-a-swipe-gesture-recognizer"></a>[スワイプ ジェスチャ認識エンジンを追加する](swipe.md)

スワイプ ジェスチャが発生するのは、指が画面に沿って水平または垂直方向に動かされたときで、多くの場合コンテンツのナビゲーションを開始するために使われます。 スワイプ ジェスチャは [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) クラスを使って認識されます。

## <a name="add-a-drag-and-drop-gesture-recognizer"></a>[ドラッグ アンド ドロップ ジェスチャ認識エンジンを追加する](drag-and-drop.md)

ドラッグ アンド ドロップ ジェスチャを使用すると、項目とそれに関連付けられているデータ パッケージを、連続するジェスチャを使用して、画面上のある位置から別の位置にドラッグできます。 ドラッグ ジェスチャは `DragGestureRecognizer` クラスによって認識され、ドロップ ジェスチャは `DropGestureRecognizer` クラスによって認識されます。
