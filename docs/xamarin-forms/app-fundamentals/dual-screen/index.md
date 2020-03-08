---
title: Xamarin.Forms のデュアル画面
description: このガイドでは、Xamarin Forms でデュアル画面デバイス用のアプリを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 344b6293090ffa4281ea6351f7f176a5be37e5bd
ms.sourcegitcommit: 0e35d3eafad833d3f19768b001bd804ddda8b69b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78165554"
---
# <a name="xamarinforms-dual-screen"></a>Xamarin.Forms のデュアル画面

![](~/media/shared/preview.png "This API is currently pre-release")

Surface Duo (Android) と Surface Neo (Windows 10X) では、タッチ アプリケーションの新しいパターンが導入されています。 Xamarin.Forms には `TwoPaneView` クラスと `DualScreenInfo` クラスが含まれているため、これらのデバイス用のアプリを開発できます。

## <a name="dual-screen-design-patterns"></a>[デュアル画面の設計パターン](design-patterns.md)

デュアル画面のデバイスで複数の画面を最適に使用する方法を検討する場合は、このパターンに関するガイダンスを参照して、アプリケーション インターフェイスに最適なものを見つけてください。

## <a name="dual-screen-layout"></a>[デュアル画面のレイアウト](twopaneview.md)

同じ名前の UWP コントロールからインスパイアされた Xamarin.Forms の `TwoPaneView` クラスは、デュアル画面デバイス用に最適化されたクロスプラットフォームのレイアウトです。

## <a name="dual-screen-device-capabilities"></a>[デュアル画面のデバイスの機能](dual-screen-info.md)

`DualScreenInfo` で、表示されるペイン、その大きさ、デバイスがどのようなものか、ヒンジの角度などを決定できるようになります。

## <a name="dual-screen-platform-helpers"></a>[デュアル画面のプラットフォーム ヘルパー](dual-screen-helper.md)

`DualScreenHelper` クラスでは、プラットフォームでピクチャ イン ピクチャとして新しいウィンドウを開くことができるかどうかを確認できます。 Neo では、これによりデバイスが作成モードのときに Wonderbar に表示されるウィンドウを開くことができるようになります。

## <a name="dual-screen-triggers"></a>[デュアル画面のトリガー](triggers.md)

[`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) 名前空間には、アタッチされたレイアウト (ウィンドウ) の表示モードが変更されたときに [`VisualState`](xref:Xamarin.Forms.VisualState) 変更をトリガーする 2 つの状態トリガーが含まれています。
