---
title: サンプルの統合
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 8c46fa05a8ffcedfa7fa9d1344946fac518b5036
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="sample-integrations"></a>サンプルの統合

参照してください、[調理シンク][ KitchenSink]との統合の実際の例のサンプルです。 単にビルド`KitchenSink.sln`Mac または Visual Studio とし、開くの Visual Studio で`KitchenSink.workbook`です。

[![台所シンク統合スクリーン ショット](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

台所シンク サンプルでは、両方の概念のセットを示します。

* 表現の部分が使用する方法をデモンストレーション`RepresentationManager`の組み込み形式を使用して、レンダリングを強化するためにします。
* `Person`オブジェクトとその関連付けられた JavaScript レンダラーは、使用方法を示します`ISerializableObject`表現プロバイダーを経由せずします。

参照してください[SkiaSharp] [ skiasharp]既存を使用するとの統合の実際の例については[表現](~/tools/workbooks/sdk/representations.md)その型を表示するために、Xamarin ブックによって提供されます。

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks