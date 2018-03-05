---
title: "CocosSharp を使用したゲームの開発の概要"
description: "このマルチパートのチュートリアルでは、CocosSharp を使用して単純な 2 D ゲームを作成する方法を示します。 グラフィックス、入力、および物理学などの一般的なゲーム プログラミングの概念を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 3b1bb45ab87c85dff42b4f7ea5297eb3596b81a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>CocosSharp を使用したゲームの開発の概要

_このマルチパートのチュートリアルでは、CocosSharp を使用して単純な 2 D ゲームを作成する方法を示します。グラフィックス、入力、および物理学などの一般的なゲーム プログラミングの概念を説明します。_

CocosSharp 2D のゲーム エンジンは、クロスプラット フォームのゲームを行うためのテクノロジを提供します。 サポートされているプラットフォームの完全な一覧を参照してください、 [github CocosSharp wiki](https://github.com/mono/CocosSharp/wiki)です。 このチュートリアルは使用 (C#) コード サンプル、CocosSharp はも f# で完全に機能します。

CocosSharp のコアがによって提供される、 [MonoGame framework](http://www.monogame.net/)、クロスプラット フォームは、ハードウェア資産をインポートするため、画像、オーディオ、ゲームの状態管理、入力、およびコンテンツのパイプラインを提供する API を高速化します。 CocosSharp は、2 D ゲームに適した効率的な抽象化レイヤーです。 さらより大きなゲームはゲームが複雑さの増大に応じて、コア ライブラリの外部で独自の最適化を実行できます。 つまり、CocosSharp 提供使いやすさと、パフォーマンスのミックス ゲーム規模や複雑さを制限することがなくすぐに開始する開発者を有効にします。

このチュートリアルは、空のプロジェクトの設定に注力しています。 最初のセクションです。  2 番目の部分は、ゲーム ロジックのすべての書き込みについて説明します。 

このチュートリアルの最後では作成されましたが画面のボールを保持しようとしています。 水平方向にパドルをスライドにプレーヤーの目標がここでは単純なゲーム。 各バウンスには、1 つのポイントでプレイヤーのスコアが増加します。

![](images/image1.png "各バウンドでは、1 つのポイントでプレイヤーのスコアが増えます")

# <a name="walkthrough-parts"></a>チュートリアルのパーツ

* [パート 1 – CocosSharp プロジェクトを作成します。](~/graphics-games/cocossharp/first-game/part1.md)
* [パート 2 –、BouncingGame を実装します。](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>関連リンク

- [ゲームの内容 (サンプル)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [完成したプロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [NuGet で CocosSharp PCL](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [CocosSharp API ドキュメント](http://developer.xamarin.comhttps://developer.xamarin.com/api/namespace/CocosSharp/)
