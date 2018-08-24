---
title: アーキテクチャの概要
description: このドキュメントでは、Xamarin のブック、対話型のエージェントと対話型のクライアントを連携させる方法を調べるのアーキテクチャについて説明します。
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 3cd0d3e8cc47dd7fa43dfa0b910c9f09740fcc9a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794008"
---
# <a name="architecture-overview"></a>アーキテクチャの概要

Xamarin のブック機能を互いに連携して動作する 2 つの主要なコンポーネント:_エージェント_と_クライアント_です。

## <a name="interactive-agent"></a>対話型のエージェント

エージェント コンポーネントは、プラットフォーム固有の小さなアセンブリ .NET アプリケーションのコンテキストで実行するためです。

Xamarin のブックでは、さまざまなプラットフォームでは、iOS、Android、Mac、および WPF などの既成の"empty"アプリケーションを提供します。 これらのアプリケーションは、明示的にエージェントをホストします。

ライブの検査 (Xamarin インスペクター)、中に通常の開発およびデバッグのワークフローの一部として既存のアプリケーションに IDE デバッガーを使用して、エージェントが挿入されます。

## <a name="interactive-client"></a>対話型のクライアント

クライアントは、ネイティブ シェル (Cocoa Mac で、Windows での WPF) ブック/REPL インターフェイスを表すための web ブラウザーの画面をホストします。 SDK の観点からは、すべてのクライアント統合は、JavaScript、CSS で実装されます。

クライアントが (Roslyn) を使用して、小規模のアセンブリにソース コードをコンパイルすると、接続されているエージェントの実行に経由で送信します。 実行の結果は、表示するため、クライアントに送信されます。 ブック内の各セルには、前のセルのアセンブリを参照している 1 つのアセンブリが生成されます。

エージェントは、任意の型の .NET プラットフォームで実行されていることができ、何もへのアクセスを実行中のアプリケーションが、ために注意が必要、プラットフォームに依存しない方法で結果をシリアル化します。
