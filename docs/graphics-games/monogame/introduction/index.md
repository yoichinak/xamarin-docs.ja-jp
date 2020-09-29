---
title: モノゲームによるゲーム開発の概要
description: このマルチパートチュートリアルでは、モノゲームを使用して簡単な2D アプリケーションを作成する方法について説明します。  グラフィックス、入力、ゲームエンティティ、物理など、ゲームのプログラミングに関する一般的な概念について説明します。
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: bdbce0b001d5baf5ef751e092a2083efdf3ca992
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436275"
---
# <a name="introduction-to-game-development-with-monogame"></a>モノゲームによるゲーム開発の概要

_このマルチパートチュートリアルでは、モノゲームを使用して簡単な2D アプリケーションを作成する方法について説明します。 グラフィックス、入力、ゲームエンティティ、物理など、ゲームのプログラミングに関する一般的な概念について説明します。_

この記事では、クロスプラットフォームのゲームを作成するためのモノゲーム API テクノロジについて説明します。 プラットフォームの完全な一覧については、「 [モノゲーム」 web サイト](http://www.monogame.net/)を参照してください。 このチュートリアルでは、C# をコードサンプルに使用しますが、モノゲームも F # で完全に機能します。

モノゲームは、アセットをインポートするためのグラフィックス、オーディオ、ゲーム状態管理、入力、およびコンテンツパイプラインを提供する、クロスプラットフォームのハードウェアアクセラレーション API です。 ほとんどのゲームエンジンとは異なり、モノゲームでは、パターンやプロジェクトの構造を提供したり、強制したりすることはありません。  これは、開発者が自由にコードを整理できることを意味しますが、最初に新しいプロジェクトを開始するときにはセットアップコードが少し必要になります。

このチュートリアルの最初のセクションでは、空のプロジェクトを設定する方法について説明します。 最後のセクションでは、すべてのゲームロジックとコンテンツの作成について説明します。ほとんどはクロスプラットフォームです。

このチュートリアルを終了すると、プレーヤーがタッチ入力でアニメーション化された文字を制御できる単純なゲームが作成されます。  これは技術的には完全なゲームではありませんが (獲得した条件がないため)、多数のゲーム開発の概念を示し、さまざまな種類のゲームの基盤として使用できます。

このチュートリアルの結果を次に示します。

![マウスの後のサンプルゲーム文字のアニメーション](images/image1.gif)

## <a name="monogame-and-xna"></a>モノゲームと XNA

モノゲームライブラリは、構文と機能の両方で Microsoft の XNA ライブラリを模倣することを目的としています。  すべてのモノゲームオブジェクトは、Microsoft の Xna 名前空間に存在します。ほとんどの XNA コードは、変更せずにモノゲームで使用できます。

XNA に精通している開発者は、モノゲームの構文について既に理解しており、モノゲームの操作に関する追加情報を探している開発者は、既存のオンライン XNA チュートリアル、API ドキュメント、およびディスカッションを参照できます。

## <a name="walkthrough-parts"></a>チュートリアルのパート

- [パート 1-クロスプラットフォームのモノゲームプロジェクトの作成](~/graphics-games/monogame/introduction/part1.md)
- [パート2–ゲームの実装](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>関連リンク

- [プレイ Inggame のモノゲームプロジェクト (サンプル)](/samples/xamarin/mobile-samples/walkinggamemg/)
- [NuGet でのモノゲーム Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet でのモノゲーム iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [モノゲーム API ドキュメント](http://www.monogame.net/documentation/?page=main)