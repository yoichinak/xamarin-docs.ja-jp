---
title: アーキテクチャの概要
description: このドキュメントでは、対話型のエージェントと対話型のクライアントを連携させる方法を調べて、Xamarin Workbooks のアーキテクチャについて説明します。
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 352e8d0191512184ffd7d82fa0dfa0bc79fa24ca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61257426"
---
# <a name="architecture-overview"></a>アーキテクチャの概要

Xamarin Workbooks は機能で相互に連携して動作する 2 つの主要なコンポーネントです。_エージェント_と_クライアント_します。

## <a name="interactive-agent"></a>対話型のエージェント

エージェント コンポーネントは、.NET アプリケーションのコンテキストで実行されるプラットフォーム固有の小さなアセンブリです。

Xamarin Workbooks は、さまざまなプラットフォーム、iOS、Android、Mac、WPF などの既成の「空の」アプリケーションを提供します。 これらのアプリケーションは、明示的にエージェントをホストします。

ライブ インスペクション (Xamarin Inspector) 中に、エージェントは通常の開発とデバッグのワークフローの一部として既存のアプリケーションに IDE のデバッガーを使用して挿入されます。

## <a name="interactive-client"></a>対話型のクライアント

クライアントはネイティブ シェル (Cocoa Mac 上で、Windows での WPF) ブック/REPL インターフェイスを表示するための web ブラウザー画面をホストします。 SDK の観点からは、すべてのクライアント統合は、JavaScript と CSS で実装されます。

クライアントが (Roslyn) を使用して、ソース コードの小さなアセンブリにコンパイルおよび実行のために接続されているエージェントに送信します。 実行の結果は、レンダリングのクライアントに送信されます。 ブック内の各セルは、前のセルのアセンブリを参照する 1 つのアセンブリを生成します。

エージェントが任意の種類の .NET プラットフォームで実行されていることができます、実行中のアプリケーションで何もへのアクセスを持つため、注意が必要なプラットフォームに依存しない方法で結果をシリアル化します。
