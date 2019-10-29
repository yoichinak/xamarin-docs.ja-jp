---
title: サンプルの統合
description: このドキュメントでは、Xamarin Workbooks 統合を示すサンプルへのリンクを示します。 リンクされたサンプルは、表現レンダリングおよび SkiaSharp と連携します。
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: fa66ee2b2b469900381e2d31dc5dec49eb42c4f6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018121"
---
# <a name="sample-integrations"></a>サンプルの統合

統合の実際の例については、[キッチンのシンク][KitchenSink]サンプルを参照してください。 Visual Studio for Mac または Visual Studio で `KitchenSink.sln` をビルドし、`KitchenSink.workbook`を開くだけです。

[![キッチンシンク統合のスクリーンショット](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

キッチンのシンクサンプルでは、両方の概念のセットを示しています。

* この表現は、`RepresentationManager` を使用して、組み込み表現を使用してレンダリングを強化する方法を示しています。
* `Person` オブジェクトとそれに関連付けられた JavaScript レンダラーは、表現プロバイダーを介さずに `ISerializableObject` を使用する方法を示しています。

Xamarin Workbooks によって提供される既存の[表現](~/tools/workbooks/sdk/representations.md)を使用して型をレンダリングする実際の統合例については、「 [SkiaSharp][skiasharp] 」を参照してください。

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
