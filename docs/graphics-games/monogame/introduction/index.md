---
title: MonoGame を使用したゲーム開発の概要
description: このマルチパート チュートリアルでは、MonoGame を使用して、単純な 2D アプリケーションを作成する方法を示します。  一般的なゲームでは、入力、グラフィックスなどのプログラミングの概念エンティティ、および物理学のゲームです。
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4ab98d59bc74672f9531f4dbd3c33a6270582612
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386272"
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame を使用したゲーム開発の概要

_このマルチパート チュートリアルでは、MonoGame を使用して、単純な 2D アプリケーションを作成する方法を示します。一般的なゲームでは、入力、グラフィックスなどのプログラミングの概念エンティティ、および物理学のゲームです。_

この記事では、クロス プラットフォーム ゲームを行うための MonoGame API テクノロジについて説明します。 プラットフォームの一覧については、次を参照してください。、 [MonoGame の web サイト](http://www.monogame.net/)します。 このチュートリアルでは使用C#コード サンプル、MonoGame が完全に機能をF#もします。

MonoGame はクロス プラットフォームで、ハードウェア資産をインポートするためのグラフィック、オーディオ、ゲームの状態管理、入力、およびコンテンツ パイプラインを提供する API を高速です。 ほとんどのゲーム エンジンとは異なり MonoGame やは提供しません、パターンまたはプロジェクトの構造を強制します。  これにより、開発者は自由に好きなように、コードを整理、一方のセットアップ コードが最初に新しいプロジェクトを開始するときに必要であることも意味します。

このチュートリアルの最初のセクションでは、空のプロジェクトの設定について説明します。 最後のセクションでは、ゲーム ロジックとコンテンツ – 最もうちはクロス プラットフォームのすべての書き込みについて説明します。

このチュートリアルの目的は、単純なゲーム プレイヤーがタッチ入力のアニメーションの文字を制御できます作りされます。  これは完全なゲームがあるため獲得したり、条件が失われるなし) に技術的ではありませんが、多くのゲーム開発の概念を示し、さまざまな種類のゲームの基盤として使用できます。 

このチュートリアルの結果を次に示します。

![次のマウス サンプル ゲーム キャラクターのアニメーション](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame および XNA

MonoGame ライブラリは、構文と機能の両方で Microsoft の XNA ライブラリを模倣するものです。  MonoGame のまま変更せずに使用するほとんどの XNA コード – Microsoft.Xna 名前空間の下 MonoGame のすべてのオブジェクトが存在します。 

XNA に慣れている開発者を MonoGame の構文に精通することが既にと MonoGame を使用した作業に関する追加情報を探している開発者は既存のオンライン XNA チュートリアル、API のドキュメント、およびディスカッションを参照できます。


## <a name="walkthrough-parts"></a>チュートリアルのパーツ

- [パート 1-クロスプラット フォーム MonoGame プロジェクトを作成します。](~/graphics-games/monogame/introduction/part1.md)
- [パート 2-Walkinggame の実装](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>関連リンク

- [WalkingGame MonoGame プロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB フォント iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB フォント Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [NuGet で MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet での iOS の MonoGame](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API ドキュメント](http://www.monogame.net/documentation/?page=main)
