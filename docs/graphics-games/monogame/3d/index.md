---
title: モノゲームを使用した3D グラフィックスの概要
description: モノゲームには、リアルタイム3D グラフィックスを表示するための柔軟で効率的な API が用意されています。 これには、レンダリングのための上位レベルのコンストラクトが含まれ、下位レベルのグラフィックスリソースにもアクセスできます。
ms.prod: xamarin
ms.assetid: 8706826E-8BA5-4E00-A7D6-4072626E3292
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: d5deef3056b3ce645c509b9754306214704ed7b9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763849"
---
# <a name="introduction-to-3d-graphics-with-monogame"></a>モノゲームを使用した3D グラフィックスの概要

_モノゲームには、リアルタイム3D グラフィックスを表示するための柔軟で効率的な API が用意されています。これには、レンダリングのための上位レベルのコンストラクトが含まれ、下位レベルのグラフィックスリソースにもアクセスできます。_

モノゲーム API は、3D ゲームおよびアプリケーションを開発するための広範なクラスのセットを提供します。 これにより、ハードウェアへの直接アクセスが可能になり、さまざまなプラットフォームで同じ構文を維持しながら、パフォーマンスが最大になります。

モノゲームは Microsoft の XNA とほぼ同じであるため、XNA を経験した開発者は、モノのゲーム開発を理解していることがわかります。 XNA を使用していないものの、3D ゲームで DirectX または OpenGL を使用している開発者は、多くのクラスと概念を理解しています。

最初のセクションでは、fbx ファイルからゲームに3D モデルを追加する方法について説明します。 次のセクションでは、移動や検索などの一般的なコントロールを含む3D カメラの作成方法について説明します。 最後のセクションでは、クラスについ`VertexBuffer`てさらに詳しく説明します。これにより、fbx ファイルから読み込まれたレンダリングモデルと比較して、3d レンダリングをより詳細に制御できるようになります。

## <a name="topics"></a>トピック

- [モデルクラスの使用](~/graphics-games/monogame/3d/part1.md)
- [3D グラフィックスを頂点で描画する](~/graphics-games/monogame/3d/part2.md)
- [3D 座標](~/graphics-games/monogame/3d/part3.md)
