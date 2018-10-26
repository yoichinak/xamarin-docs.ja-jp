---
title: CocosSharp の 2D ゲーム エンジン
description: このドキュメントは、CocosSharp によるゲーム開発に関するさまざまな記事にリンクしています。 リンクされたコンテンツでは、サンプル アプリ、描画、アニメーション、および詳細について説明します。
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: add73360ea98d8c516e413f0cc0264f68c58d79d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107015"
---
# <a name="cocossharp-2d-game-engine"></a>CocosSharp の 2D ゲーム エンジン

_CocosSharp を使用して 2D ゲームを構築するためのライブラリは、C#とF#します。人気のある Cocos2D エンジンの .NET への移植することをお勧めします。_

## <a name="introduction-to-cocossharp"></a>CocosSharp の概要

CocosSharp の 2D ゲーム エンジンは、クロス プラットフォーム ゲームを行うためのテクノロジを提供します。 サポートされているプラットフォームの完全な一覧については、次を参照してください。、 [GitHub の wiki を CocosSharp](https://github.com/mono/CocosSharp/wiki)します。
これらのガイドを使用して、C#コード サンプル、CocosSharp で完全に機能がF#もします。

CocosSharp のコアがによって提供される、[フレームワークの MonoGame](http://www.monogame.net/)、クロス プラットフォームは、ハードウェア アクセラレーション アセットをインポートするためのグラフィック、オーディオ、ゲームの状態管理、入力、およびコンテンツ パイプラインを提供する API。
CocosSharp は、2 D ゲームに適した、効率的な抽象化レイヤーです。
さらに、大規模なゲームはゲームの複雑さが増すと、コア ライブラリの外部で独自の最適化を実行できます。 つまり、CocosSharp 提供使いやすく、パフォーマンス、さまざまなゲームのサイズや複雑さを制限することがなくすばやく開始する開発者を有効にします。

この実践的なビデオは、ゲームの単純なクロスプラット フォームで CocosSharp を作成する方法を示します。

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

このガイドでは、BouncingGame、ゲームの内容をゲームや追加のゲーム ロジックを構築するために使用するさまざまなビジュアル要素を使用する方法などについて説明します。

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Fruity Falls game](~/graphics-games/cocossharp/fruity-falls.md)

![Fruity Falls game のスクリーン ショット](images/fruity-falls.png "Fruity Falls game のスクリーン ショット")

このガイドでは、一般的な CocosSharp と物理学、コンテンツ管理、ゲームの状態、およびゲームの設計などのゲーム開発の概念を説明、Fruity がゲームをについて説明します。  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Coin Time game](~/graphics-games/cocossharp/cointime.md)

![Coin Time game スクリーン ショット](images/cointime.png "コインにゲームのスクリーン ショット")

コインは完全なプラットフォーム向けの iOS および Android 用のゲームです。 ゲームの目的をすべてのレベルで収集して敵や障害物を回避しながら出口のドアに到達してです。

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[CCDrawNode によるジオメトリの描画](~/graphics-games/cocossharp/ccdrawnode.md)

![CCDrawNode によるジオメトリに描画される形状](images/ccdrawnode.png "CCDrawNode によるジオメトリに描画される図形")

CCDrawNode では、線、円、三角形などの描画プリミティブ オブジェクトのメソッドを提供します。

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[CCAction によるアニメーション](~/graphics-games/cocossharp/ccaction.md)

![CCAction アニメーション](images/ccaction.png "A CCAction アニメーション")

`CCAction` CocosSharp オブジェクトをアニメーション化するために使用する基本クラスです。 このガイドは、組み込み`CCAction`配置、スケーリング、および回転などの一般的なタスクを実装します。 継承することによってカスタム実装を作成する方法についても説明`CCAction`します。

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[CocosSharp によるタイル表示の使用](~/graphics-games/cocossharp/tiled.md)

![ゲーム内のレベル](images/tiled.png "ゲーム内のレベル")

並べて表示された、強力な柔軟性が高くは、ゲームのマップを直交と等角 タイルを作成するための完成度の高いアプリケーション。 CocosSharp では、並べて表示のネイティブなファイル形式の組み込みの統合を提供します。

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[CocosSharp のエンティティ](~/graphics-games/cocossharp/entities.md)

![ゲームからの宇宙船](images/entities.png "ゲームからの宇宙船")

エンティティのパターンは、ゲームのコードを整理する強力な方法です。 読みやすく、コードを簡単に維持するには、親/子の組み込み機能を活用しているとします。

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[CocosSharp で複数の解像度の処理](~/graphics-games/cocossharp/resolutions.md)

![画面の解像度を表すグリッド](images/resolutions.png "画面の解像度を表すグリッド")

このガイドでは、さまざまな解像度のデバイスに正しく表示するゲームを開発する CocosSharp を操作する方法を示します。

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp のコンテンツ パイプライン](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

コンテンツ パイプラインは、コンテンツを最適化し、書式を特定のハードウェアまたは特定のゲーム開発フレームワークで読み込むことができるようにするゲーム開発でよく使用されます。

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[CCSpriteSheet でフレーム レートの向上](~/graphics-games/cocossharp/ccspritesheet.md)

![ツリー、CCSpriteSheet から](images/ccspritesheet.png "CCSpriteSheet からツリー")

`CCSpriteSheet` 結合して 1 つのテクスチャで多くのイメージ ファイルを使用して機能を提供します。 テクスチャの数を減らすと、ゲームの読み込み時間とフレーム レートを改善できます。

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[CCTextureCache を使用して、テクスチャ キャッシュ](~/graphics-games/cocossharp/texture-cache.md)

![CocosSharp でイメージをキャッシュする方法の表現を](images/texture-cache.png "CocosSharp でイメージをキャッシュする方法の表現")

CocosSharp の`CCTextureCache`クラスには、整理、キャッシュ、およびコンテンツをアンロードする標準的な方法が用意されています。 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[CocosSharp による 2 次元数値演算](~/graphics-games/cocossharp/math.md)

![回転されているイメージ](images/math.png "回転されているイメージ")

このガイドでは、ゲーム開発用の 2D 数学について説明します。 CocosSharp を使用して、ゲーム開発の一般的なタスクを実行する方法について説明し、これらのタスクの背後にある数学について説明します。

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[パフォーマンスと CCRenderTexture で視覚効果](~/graphics-games/cocossharp/ccrendertexture.md)

![ゲームからスプライト](images/ccrendertexture.png "ゲームからのスプライト")

`CCRenderTexture`クラスは、1 つのテクスチャに複数の CocosSharp オブジェクトをレンダリングするための機能を提供します。 作成すると、`CCRenderTexture`効率的にグラフィックスを描画し、視覚効果を実装するインスタンスを使用できます。
