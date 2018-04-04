---
title: MonoGame を使用したゲームの開発の概要
description: このマルチパートのチュートリアルでは、MonoGame を使用して単純な 2D アプリケーションを作成する方法を示します。  一般的なゲーム内容は入力ですが、グラフィックなどのプログラミングの概念と、エンティティ、および物理学のゲームです。
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 9fb19b86ca303f8be3506d267dd75dc9db6cfca6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame を使用したゲームの開発の概要

_このマルチパートのチュートリアルでは、MonoGame を使用して単純な 2D アプリケーションを作成する方法を示します。一般的なゲーム内容は入力ですが、グラフィックなどのプログラミングの概念と、エンティティ、および物理学のゲームです。_

この記事では、クロスプラット フォームのゲームを行うための MonoGame API テクノロジについて説明します。 プラットフォームの一覧については、次を参照してください。、 [MonoGame web サイト](http://www.monogame.net/)です。 このチュートリアルは使用 (C#) コード サンプル、MonoGame はも f# で完全に機能します。

MonoGame はプラットフォーム間で、ハードウェア アクセラレーション資産をインポートするため、画像、オーディオ、ゲームの状態管理、入力、およびコンテンツのパイプラインを提供する API。 ほとんどのゲーム エンジンとは異なり MonoGame やは提供しません、パターン、またはプロジェクトの構造を強制します。  つまり、開発者は自由に好きなように、コードの編成、中にセットアップ コードのビットが最初に、新しいプロジェクトを開始するときに必要であることも意味します。

このチュートリアルの最初のセクションでは、空のプロジェクトの設定について説明します。 最後のセクションでは、ゲーム ロジックとコンテンツ – 最もうちはクロス プラットフォームのすべての書き込みについて説明します。

このチュートリアルの目的が作成されました簡単なゲーム、プレーヤーがタッチ入力のアニメーションの文字を制御できます。  これは技術的には、完全ゲームがあるため win または条件が失われるなし) にではありませんが、さまざまなゲーム開発の概念を示していて、さまざまな種類のゲームの基盤として使用できます。 

このチュートリアルの結果を次に示します。

![](images/image1.gif "このチュートリアルで作成するアプリ")

# <a name="monogame-and-xna"></a>Monogame および XNA

MonoGame ライブラリは、構文と機能の両方に Microsoft の XNA ライブラリを模倣するために対象としています。  まま変更せずに MonoGame で使用されるほとんどの XNA のコードを許可する – Microsoft.Xna 名前空間 MonoGame のすべてのオブジェクトに存在します。 

XNA に慣れている開発者に既に習熟 MonoGame の構文と MonoGame の操作に関する追加情報を探している開発者は既存のオンライン XNA チュートリアル、API のドキュメント、およびディスカッションを参照することになります。


# <a name="walkthrough-parts"></a>チュートリアルのパーツ

- [クロス プラットフォーム MonoGame プロジェクトを作成する – 第 1 部](~/graphics-games/monogame/introduction/part1.md)
- [パート 2 –、WalkingGame を実装します。](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>関連リンク

- [WalkingGame MonoGame プロジェクト (サンプル)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB フォント iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB フォント Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [NuGet で MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet で MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API ドキュメント](http://www.monogame.net/documentation/?page=main)
