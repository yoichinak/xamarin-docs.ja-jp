---
title: エンタープライズアプリケーションの開発にあたって
description: この章では、Xamarin.Forms を使用したエンタープライズアプリケーション開発にあたっての前提事項について説明します。
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 4ce04ec5216872cb56424e8847eec357a5b3ac0e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770739"
---
# <a name="preface-to-enterprise-app-development"></a>エンタープライズアプリケーションの開発にあたって

この電子書籍では、Xamarin.Forms を使用してクロス プラットフォームのエンタープライズアプリケーションを構築するためのガイダンスについて説明します。 Xamarin.Forms は容易に iOS、Android、およびユニバーサル Windows プラットフォーム（UWP）のネイティブなユーザーインターフェースレイアウトをプラットフォーム間で共有して開発できるクロスプラットフォーム UI ツールキットです。 この電子書籍では、従業員向け（B2E）、対企業向け（B2B）、およびコンシューマー向け（B2C）アプリの全てに対して適用可能な総保有コスト（TCO）削減が出来るクロスプラットフォーム間でのソースコード共有の包括的なソリューションを提供します。

このガイドでは、柔軟で保守性の高い、テスト可能な Xamarin. Forms enterprise アプリを開発するためのアーキテクチャガイダンスを提供します。 疎結合を維持しながら MVVM、依存性の注入、ナビゲーション、検証、および構成の管理を実装する方法についてガイダンスを示します。 さらに、このガイダンスでは認証サーバーを使用した認証と認可、コンテナー化されたマイクロサービスへのデータアクセス、そしてユニットテストについても記載しています。

このガイドには [eShopOnContainers モバイル アプリ](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)と、[eShopOnContainers リファレンスアプリケーション](https://github.com/dotnet-architecture/eShopOnContainers)のソースコードがあります。 eShopOnContainers モバイル アプリは一連の eShopOnContainers リファレンスアプリケーションとして知られるコンテナ化されたマイクロサービスと接続して動作する Xamarin.Forms で開発されたクロスプラットフォーム エンタープライズ アプリケーションです。 コンテナー化されたマイクロサービスの展開をしなくても、モックサービスからデータを取得するようにも構成できます。

## <a name="whats-left-out-of-this-guides-scope"></a>このガイドで触れない事項について

このガイドは、既に Xamarin.Forms について熟知している読者を対象としています。 Xamarin.Forms の詳細な概要については [Xamarin.Forms ドキュメント](~/xamarin-forms/index.yml)、および[Xamarin.Forms を使用したモバイル アプリを作成する](https://aka.ms/xamebook)を参照してください。

このガイドは、.net [マイクロサービスを補完しています。コンテナー化された](https://aka.ms/microservicesebook)マイクロサービスの開発と展開に重点を置いた、コンテナー化された .net アプリケーションのアーキテクチャ。 その他の関連ドキュメントとして[ASP.NET Core と Microsoft Azure を使用したモダン Web アプリケーションの設計と開発](https://aka.ms/WebAppEbook)、 [Microsoft プラットフォームとツールを使用したコンテナー化された Docker アプリケーションのライフサイクル](https://aka.ms/dockerlifecycleebook)、および[モバイル アプリ開発用の Microsoft プラットフォームとツール](https://aka.ms/MobAppDev/StndPDF)があります。

## <a name="who-should-use-this-guide"></a>対象読者

このガイドの主な対象読者は、Xamarin.Forms を使ったクロスプラットフォーム エンタープライズ アプリケーションの設計と実装方法について学びたいと思っている開発者とアーキテクトです。

2 番目の対象読者は、Xamarin.Forms を使用したクロスプラットフォーム エンタープライズ アプリケーション開発をするにあたってアプローチを選択する前に、設計と技術の概要を知りたい技術面での意思決定者です。

## <a name="how-to-use-this-guide"></a>このガイドの使用方法

このガイドは、Xamarin.Forms を使用したクロスプラット フォーム エンタープライズ アプリケーションの構築に焦点を当てています。 そのため、それらのアプリケーションと技術検討の基礎の理解を得るには全体を読む必要があります。 このガイドとサンプルプログラムは、新しいエンタープライズ アプリケーションを作成するときの開始地点やリファレンスとしても役に立ちます。 このガイドは、関連サンプル アプリケーションを新しいアプリケーションのテンプレートとして使ったり、アプリケーションのコンポーネントのパーツをどのように組み立てるかの参考にしたり、前述の通り、設計のガイダンスに使用したりできます。 次に、アーキテクチャのガイダンスについては、このガイドに戻ってください。

Xamarin.Forms を使用したクロスプラットフォーム エンタープライズ アプリケーションの共通理解を確実にするために、チームメンバーにこのドキュメントを共有してください。 共通の用語セットと基本的な原理原則を持って作業することは、一貫したアプリケーションの設計のパターンと実践を確実にするための助けとなるでしょう。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
