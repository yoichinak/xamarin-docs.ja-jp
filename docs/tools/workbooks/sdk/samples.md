---
title: サンプルの統合
description: このドキュメントでは、Xamarin Workbooks 統合を示すサンプルへのリンクを示します。 リンクされたサンプルは、表現レンダリングおよび SkiaSharp と連携します。
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: fbe471aa7f08d85a870d68505cf2c983b7e442e9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292810"
---
# <a name="sample-integrations"></a>サンプルの統合

統合の実際の例については、[キッチンのシンク][KitchenSink]サンプルを参照してください。 Visual Studio for Mac また`KitchenSink.sln`は Visual Studio でビルドし、を`KitchenSink.workbook`開くだけです。

[![キッチンシンク統合のスクリーンショット](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

キッチンのシンクサンプルでは、両方の概念のセットを示しています。

* この表現は、を使用`RepresentationManager`して、組み込み表現を使用してレンダリングを強化する方法を示しています。
* オブジェクト`Person`とそれに関連付けられた`ISerializableObject` JavaScript レンダラーは、表現プロバイダーを経由せずにを使用する方法を示しています。

Xamarin Workbooks によって提供される既存の[表現](~/tools/workbooks/sdk/representations.md)を使用して型をレンダリングする実際の統合例については、「 [SkiaSharp][skiasharp] 」を参照してください。

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
