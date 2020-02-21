---
title: Xamarin.Forms のデュアル画面
description: このガイドでは、Xamarin Forms でデュアル画面デバイス用のアプリを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 41a7ef4e447bb71582264b4e73629566d3ffd4e7
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77480507"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms のデュアル画面

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo (Android) と Surface Neo (Windows 10X) では、タッチ アプリケーションの新しいパターンが導入されています。 Xamarin.Forms には `TwoPaneView` クラスと `DualScreenInfo` クラスが含まれているため、これらのデバイス用のアプリを開発できます。

## <a name="dual-screen-patterns"></a>[デュアル画面のパターン](design-patterns.md)

デュアル画面のデバイスで複数の画面を最適に使用する方法を検討する場合は、このパターンに関するガイダンスを参照して、アプリケーション インターフェイスに最適なものを見つけてください。

## <a name="twopaneview"></a>[TwoPaneView](twopaneview.md)

同じ名前の UWP コントロールからインスパイアされた Xamarin.Forms の `TwoPaneView` クラスは、デュアル画面デバイス用に最適化されたクロスプラットフォームのレイアウトです。

## <a name="dualscreeninfo"></a>[DualScreenInfo](dual-screen-info.md)

`DualScreenInfo` で、表示されるペイン、その大きさ、デバイスがどのようなものか、ヒンジの角度などを決定できるようになります。

## <a name="dualscreenhelper"></a>[DualScreenHelper](dual-screen-helper.md)

`DualScreenHelper` クラスでは、プラットフォームでピクチャ イン ピクチャとして新しいウィンドウを開くことができるかどうかを確認できます。 Neo では、これによりデバイスが作成モードのときに Wonderbar に表示されるウィンドウを開くことができるようになります。
