---
title: アーキテクチャの概要
description: このドキュメントでは、Xamarin Workbooks のアーキテクチャについて説明し、対話型エージェントと対話型クライアントがどのように連携して動作するかを調べます。
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 7b3f2613e315bc05fedfb5b2fa70d11c2874ba65
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029616"
---
# <a name="architecture-overview"></a>アーキテクチャの概要

Xamarin Workbooks には、_エージェント_と_クライアント_と連携して動作する必要がある2つの主要コンポーネントがあります。

## <a name="interactive-agent"></a>対話型エージェント

エージェントコンポーネントは、.NET アプリケーションのコンテキストで実行される小規模なプラットフォーム固有のアセンブリです。

Xamarin Workbooks には、iOS、Android、Mac、WPF など、多数のプラットフォーム用の既成の "空の" アプリケーションが用意されています。 これらのアプリケーションは、エージェントを明示的にホストします。

ライブ検査 (Xamarin Inspector) では、エージェントは、通常の開発 & デバッグワークフローの一部として、IDE デバッガーを使用して既存のアプリケーションに挿入されます。

## <a name="interactive-client"></a>対話型クライアント

クライアントは、ブック/REPL インターフェイスを表示するための web ブラウザー画面をホストするネイティブシェル (Mac では Cocoa、Windows では WPF) です。 SDK の観点からは、すべてのクライアント統合が JavaScript と CSS で実装されています。

クライアントは、ソースコードを小さなアセンブリにコンパイルし、接続されているエージェントに送信して実行するために、(Roslyn 経由で) を担当します。 実行結果は、レンダリングのためにクライアントに送り返されます。 ブック内の各セルは、前のセルのアセンブリを参照する1つのアセンブリを生成します。

エージェントは任意の種類の .NET プラットフォームで実行でき、実行中のアプリケーションのあらゆるものにアクセスできます。そのため、プラットフォームに依存しない方法で結果をシリアル化する必要があります。
