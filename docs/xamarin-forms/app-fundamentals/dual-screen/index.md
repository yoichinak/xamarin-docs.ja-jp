---
title: Xamarin.Forms のデュアル画面機能
description: このガイドでは、Xamarin Forms でデュアル画面デバイス用のアプリを簡単に活用する方法について説明します。
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 1f648297a608c592d90f2c70494ae4496fe73c0f
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145516"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms のデュアル画面

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo (Android) と Surface Neo (Windows 10X) では、タッチ アプリケーションの新しいパターンが導入されています。 これらの新しいエクスペリエンスを最大限に活用するため、Xamarin.Forms では `TwoPaneView` と `DualScreenInfo` が導入されています。

## <a name="dual-screen-patternsdesign-patternsmd"></a>[デュアル画面のパターン](design-patterns.md)

デュアル画面のデバイスで複数の画面を最適に使用する方法を検討する場合は、パターンに関するガイダンスを参照して、アプリケーション インターフェイスに最適なものを見つけてください。

## <a name="twopaneviewtwopaneviewmd"></a>[TwoPaneView](twopaneview.md)

同じ名前の UWP コントロールからインスパイアされた Xamarin.Forms の `TwoPaneView` クラスでは、クロスプラットフォーム方式のデュアル画面デバイス用に最適化されたレイアウトを導入しています。

## <a name="dualscreeninfodual-screen-infomd"></a>[DualScreenInfo](dual-screen-info.md)

デュアル画面のデバイスを最大限に活用するには、表示されるペイン、その大きさ、デバイスがどのようなものか、ヒンジの角度などを把握しておく必要があります。 `DualScreenInfo` でこの情報が提供されます。

## <a name="dualscreenhelperdual-screen-helpermd"></a>[DualScreenHelper](dual-screen-helper.md)
`DualScreenHelper` では、プラットフォームでピクチャ イン ピクチャとして新しいウィンドウを開くことができるかどうかを確認できます。 Neo では、これによりデバイスが作成モードのときに Wonderbar に表示されるウィンドウを開くことができるようになります。