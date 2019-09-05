---
title: アーキテクチャの概要
description: このドキュメントでは、Xamarin Workbooks のアーキテクチャについて説明し、対話型エージェントと対話型クライアントがどのように連携して動作するかを調べます。
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 7129d0bedddb272ef87e3d209cb05c2ca0c0acf4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285282"
---
# <a name="architecture-overview"></a>アーキテクチャの概要

Xamarin Workbooks は、互いに連携して動作する必要がある2つの主要なコンポーネントを備えています。_エージェント_と_クライアント_。

## <a name="interactive-agent"></a>対話型エージェント

エージェントコンポーネントは、.NET アプリケーションのコンテキストで実行される小規模なプラットフォーム固有のアセンブリです。

Xamarin Workbooks には、iOS、Android、Mac、WPF など、多数のプラットフォーム用の既成の "空の" アプリケーションが用意されています。 これらのアプリケーションは、エージェントを明示的にホストします。

ライブ検査 (Xamarin Inspector) では、エージェントは、通常の開発 & デバッグワークフローの一部として、IDE デバッガーを使用して既存のアプリケーションに挿入されます。

## <a name="interactive-client"></a>対話型クライアント

クライアントは、ブック/REPL インターフェイスを表示するための web ブラウザー画面をホストするネイティブシェル (Mac では Cocoa、Windows では WPF) です。 SDK の観点からは、すべてのクライアント統合が JavaScript と CSS で実装されています。

クライアントは、ソースコードを小さなアセンブリにコンパイルし、接続されているエージェントに送信して実行するために、(Roslyn 経由で) を担当します。 実行結果は、レンダリングのためにクライアントに送り返されます。 ブック内の各セルは、前のセルのアセンブリを参照する1つのアセンブリを生成します。

エージェントは任意の種類の .NET プラットフォームで実行でき、実行中のアプリケーションのあらゆるものにアクセスできます。そのため、プラットフォームに依存しない方法で結果をシリアル化する必要があります。
