---
title: ''
description: この章では、を使用したエンタープライズアプリケーションパターンの概要について説明し Xamarin.Forms ます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b587bd8cacda9024103757a585c11fba2ed09fda
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130833"
---
# <a name="preface-to-enterprise-app-development"></a>はじめにエンタープライズアプリ開発

この電子ブックでは、を使用してクロスプラットフォームのエンタープライズアプリを構築するためのガイダンスを提供 Xamarin.Forms します。 Xamarin.Formsは、iOS、Android、ユニバーサル Windows プラットフォーム (UWP) などのプラットフォーム間で共有できるネイティブユーザーインターフェイスレイアウトを開発者が簡単に作成できるようにする、クロスプラットフォーム UI ツールキットです。 これにより、企業間 (B2E)、企業間 (B2B)、および B2C (B2C) アプリ向けの包括的なソリューションが提供され、すべてのターゲットプラットフォームでコードを共有し、総保有コスト (TCO) を削減することができます。

このガイドでは、適応性、保守性、およびテスト可能なエンタープライズアプリを開発するためのアーキテクチャガイダンスを提供し Xamarin.Forms ます。 MVVM、依存関係の注入、ナビゲーション、検証、および構成管理を実装する方法についてのガイダンスが提供されていますが、疎結合を維持しています。 また、認証と承認を実行する方法についても説明します。また、コンテナー化されたマイクロサービスからのデータへのアクセス、および単体テストについても説明します。

このガイドには、 [eShopOnContainers モバイルアプリ](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)のソースコードと、 [eShopOnContainers reference アプリ](https://github.com/dotnet-architecture/eShopOnContainers)のソースコードが付属しています。 EShopOnContainers モバイルアプリは、を使用して開発されたクロスプラットフォームエンタープライズアプリで、 Xamarin.Forms eShopOnContainers reference アプリと呼ばれる一連のコンテナー化されたマイクロサービスに接続します。 ただし、eShopOnContainers モバイルアプリは、コンテナー化されたマイクロサービスのデプロイを避ける必要があるユーザーのために、モックサービスからのデータを使用するように構成できます。

## <a name="whats-left-out-of-this-guides-scope"></a>このガイドの範囲の残りの部分

このガイドは、に既に慣れている読者を対象としてい Xamarin.Forms ます。 の詳細については、の Xamarin.Forms [ Xamarin.Forms ドキュメント](~/xamarin-forms/index.yml)を参照し、を使用[ Xamarin.Forms ](https://aka.ms/xamebook)して Mobile Apps を作成してください。

このガイドは、コンテナー化されたマイクロサービスの開発と展開に重点を置いた、 [.Net マイクロサービス: コンテナー化された .Net アプリケーションのアーキテクチャ](https://aka.ms/microservicesebook)を補完しています。 参考になるその他のガイドには、 [ASP.NET Core と Microsoft Azure を使用した最新の Web アプリケーションの設計と開発](https://aka.ms/WebAppEbook)、 [microsoft のプラットフォームとツールを使用](https://aka.ms/dockerlifecycleebook)したコンテナー化された Docker アプリケーションのライフサイクル、[モバイルアプリ開発用の microsoft プラットフォームとツール](https://aka.ms/MobAppDev/StndPDF)が含まれます。

## <a name="who-should-use-this-guide"></a>このガイドを使用するユーザー

このガイドの対象読者は、主に、を使用してクロスプラットフォームのエンタープライズアプリを設計および実装する方法を学習したい開発者とアーキテクトです Xamarin.Forms 。

2番目のユーザーは、を使用してクロスプラットフォームのエンタープライズアプリ開発のために選択する方法を決定する前に、アーキテクチャとテクノロジの概要を取得する技術意思決定者です Xamarin.Forms 。

## <a name="how-to-use-this-guide"></a>このガイドの使用方法

このガイドでは、を使用してクロスプラットフォームのエンタープライズアプリを構築する方法について説明 Xamarin.Forms します。 そのため、このようなアプリとその技術的な考慮事項を理解するための基盤を提供する必要があります。 このガイドは、サンプルアプリと共に、新しいエンタープライズアプリを作成するための開始点または参照としても機能します。 関連するサンプルアプリを新しいアプリのテンプレートとして使用するか、アプリのコンポーネントパーツを整理する方法を確認します。 次に、アーキテクチャのガイダンスについては、このガイドに戻ってください。

を使用したクロスプラットフォームエンタープライズアプリ開発についてよく理解できるように、このガイドをチームメンバーにお送りください Xamarin.Forms 。 共通の一連の用語と基本原則を使用して、すべての人が作業を行うことにより、アーキテクチャのパターンとプラクティスを一貫して適用することができます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
