---
title: CocosSharp
description: このドキュメントは、CocosSharp を使用したゲームの開発に関するさまざまな記事にリンクしています。
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: 2772af61dfc60b7ecd0fd5f7ecdfe5701d2504f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp は、c# および f# を使用する 2D のゲームを作成するためのライブラリです。人気のある Cocos2D エンジンの .NET 移植することをお勧めします。_

## <a name="introduction-to-cocossharp"></a>CocosSharp の概要

CocosSharp 2D のゲーム エンジンは、クロスプラット フォームのゲームを行うためのテクノロジを提供します。 サポートされているプラットフォームの完全な一覧を参照してください、 [github CocosSharp wiki](https://github.com/mono/CocosSharp/wiki)です。
これらのガイドを使用して c# コード サンプルについては CocosSharp はも f# で完全に機能します。

CocosSharp のコアがによって提供される、 [MonoGame framework](http://www.monogame.net/)、クロスプラット フォームは、ハードウェア資産をインポートするため、画像、オーディオ、ゲームの状態管理、入力、およびコンテンツのパイプラインを提供する API を高速化します。
CocosSharp は、2 D ゲームに適した効率的な抽象化レイヤーです。
さらより大きなゲームはゲームが複雑さの増大に応じて、コア ライブラリの外部で独自の最適化を実行できます。 つまり、CocosSharp 提供使いやすさと、パフォーマンスのミックス ゲーム規模や複雑さを制限することがなくすぐに開始する開発者を有効にします。

このハンズオン ビデオは、ゲームを単純なクロスプラット フォーム CocosSharp を作成する方法を示しています。

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

このガイドでは、BouncingGame、ゲームの内容、ゲーム、追加のゲーム ロジックの作成に使用されるさまざまな視覚的要素を使用する方法などについて説明します。

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Fruity 滝ゲーム](~/graphics-games/cocossharp/fruity-falls.md)

![Fruity がゲームのスクリーン ショット](images/fruity-falls.png "Fruity がゲームのスクリーン ショット")

このガイドは、一般的な CocosSharp および物理学、コンテンツの管理、ゲームの状態とゲームのデザインなどのゲーム開発の概念をカバーする Fruity がゲームをについて説明します。  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[コイン時間ゲーム](~/graphics-games/cocossharp/cointime.md)

![コイン時間ゲーム スクリーン ショット](images/cointime.png "コイン時間ゲーム スクリーン ショット")

コインは iOS および Android 用ゲーム完全 platformer です。 ゲームの目的は、コイン レベル内のすべてを収集し、敵および障害物を回避しながら、終了ドアに到達してです。

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[CCDrawNode ジオメトリの描画](~/graphics-games/cocossharp/ccdrawnode.md)

![CCDrawNode で描画される図形](images/ccdrawnode.png "CCDrawNode で描画される図形")

CCDrawNode は、線、円、および三角形などの描画プリミティブ オブジェクトに対してメソッドを提供します。

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[CCAction によるアニメーション](~/graphics-games/cocossharp/ccaction.md)

![CCAction アニメーション](images/ccaction.png "A CCAction アニメーション")

`CCAction` CocosSharp オブジェクトをアニメーション化するために使用する基本クラスです。 このガイドには、組み込みがについて説明します`CCAction`配置、スケーリング、および回転などの一般的なタスクの実装です。 継承することによってカスタム実装を作成する方法についても説明`CCAction`です。

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[CocosSharp によるタイル表示の使用](~/graphics-games/cocossharp/tiled.md)

![ゲームのレベル](images/tiled.png "ゲーム内のレベル")

並べて表示は強力で柔軟なされ、ゲームの直交と等角 タイルを作成するための完成度の高いアプリケーションがマップされます。 CocosSharp は、並べてのネイティブのファイル形式の組み込みの統合を提供します。

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[CocosSharp のエンティティ](~/graphics-games/cocossharp/entities.md)

![ゲームから宇宙船](images/entities.png "ゲームから宇宙船")

エンティティのパターンは、ゲーム コードを整理する強力な方法です。 読みやすさを向上、維持するため、コードを容易にし、組み込みの親/子の機能を活用します。

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[CocosSharp で複数の解像度の処理](~/graphics-games/cocossharp/resolutions.md)

![画面の解像度を表すグリッド](images/resolutions.png "画面の解像度を表すグリッド")

このガイドでは、さまざまな解像度のデバイスに正しく表示するゲームを開発する CocosSharp を操作する方法を説明します。

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp のコンテンツ パイプライン](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

コンテンツのパイプラインは、コンテンツを最適化し、読み込むことができますまたは特定のハードウェアに特定のゲーム開発フレームワークとなるように書式を設定してゲームの開発でよく使用されます。

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[CCSpriteSheet フレーム レートの向上](~/graphics-games/cocossharp/ccspritesheet.md)

![CCSpriteSheet からツリー](images/ccspritesheet.png "CCSpriteSheet からツリー")

`CCSpriteSheet` 結合および 1 つのテクスチャで多数のイメージ ファイルを使用するための機能を提供します。 テクスチャ数を減らすと、ゲームの読み込み時間とフレーム レートを向上させることができます。

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[テクスチャ キャッシュ CCTextureCache を使用します。](~/graphics-games/cocossharp/texture-cache.md)

![CocosSharp でイメージをキャッシュする方法の表現](images/texture-cache.png "CocosSharp でイメージをキャッシュする方法の表現")

CocosSharp の`CCTextureCache`クラスに整理して、キャッシュ、コンテンツをアンロードする標準的な方法が用意されています。 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[CocosSharp による 2D 数学的演算](~/graphics-games/cocossharp/math.md)

![回転されているイメージ](images/math.png "回転されている画像")

このガイドでは、ゲームの開発のための 2D 数学について説明します。 CocosSharp 使用ゲーム開発の一般的なタスクを実行する方法について説明と、これらのタスクの背後にある数値演算について説明します。

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[パフォーマンスと CCRenderTexture に視覚効果](~/graphics-games/cocossharp/ccrendertexture.md)

![ゲームからスプライト](images/ccrendertexture.png "ゲームからスプライト")

`CCRenderTexture`クラスには、1 つのテクスチャに複数の CocosSharp オブジェクトを表示するための機能が用意されています。 作成すると、`CCRenderTexture`グラフィックスを効率的にレンダリングして、視覚効果を実装するインスタンスを使用できます。
