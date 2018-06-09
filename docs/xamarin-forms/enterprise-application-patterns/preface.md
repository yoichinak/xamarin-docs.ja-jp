---
title: エンタープライズ アプリの開発に「はじめに」
description: この章では、Xamarin.Forms を使用してエンタープライズ アプリケーションのパターンに語句を提供します。
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fd085d2fb12e82233f6d3be2e2773a84539f837b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242369"
---
# <a name="preface-to-enterprise-app-development"></a>エンタープライズ アプリの開発に「はじめに」

この電子では、Xamarin.Forms を使用してクロスプラット フォームのエンタープライズ アプリの構築についてを説明します。 Xamarin.Forms は iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) を含む、プラットフォームで共有できるインターフェイス レイアウトにネイティブ ユーザーを簡単に作成するための開発者ができるクロスプラット フォームの UI ツールキットです。 従業員 (B2E)、企業間取引 (B2B)、およびすべてのターゲット プラットフォーム間でコードを共有する機能を提供して、総保有コスト (tco) の削減に役立ちますコンシューマー (間 B2C) アプリにビジネスの包括的なソリューションを提供します。

このガイドでは、適応性、保守、およびテストが容易な Xamarin.Forms のエンタープライズ アプリケーションを開発するためのアーキテクチャに関するガイダンスを提供します。 疎結合を維持しながら MVVM、依存関係の挿入、ナビゲーション、検証、および構成の管理を実装する方法についてガイダンスを示します。 さらもガイダンスにある認証と IdentityServer、コンテナー化 microservices、および単体テストからのデータ アクセスの承認を実行します。

番組ガイドが付属してソース コードを[eShopOnContainers モバイル アプリ](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)、およびソース コードの[eShopOnContainers 参照アプリ](https://github.com/dotnet-architecture/eShopOnContainers)です。 EShopOnContainers モバイル アプリは、開発、eShopOnContainers アプリの参照と呼ばれるコンテナー化 microservices の系列に接続する、Xamarin.Forms を使用してクロスプラット フォームのエンタープライズ アプリです。 ただし、eShopOnContainers モバイル アプリを構成して、コンテナー化 microservices の展開を避けたいと管理者向けのモック サービスからデータを使用できます。

## <a name="whats-left-out-of-this-guides-scope"></a>このガイドのスコープから除外されます新機能

このガイドの目的は、Xamarin.Forms に慣れている読者です。 Xamarin.Forms の詳細な概要については、次を参照してください。、 [Xamarin.Forms ドキュメント](~/xamarin-forms/index.yml)、および[Xamarin.Forms を使用したモバイル アプリを作成する](https://aka.ms/xamebook)です。

このガイドに補完するものです[.NET Microservices: .NET アプリケーションのコンテナーをアーキテクチャ](https://aka.ms/microservicesebook)、開発と配置のコンテナー化 microservices に焦点を合わせたです。 読み取りの価値があるその他のガイドが含まれます[を設計および ASP.NET Core と Microsoft Azure で最新の Web アプリケーションの開発](http://aka.ms/WebAppEbook)、 [Microsoft プラットフォームとツールがDockerアプリケーションのライフサイクルのコンテナー](http://aka.ms/dockerlifecycleebook)、および[Microsoft プラットフォームとモバイル アプリ開発用ツール](http://aka.ms/MobAppDev/StndPDF)です。

## <a name="who-should-use-this-guide"></a>このガイドを使用する必要があります。

このガイドの対象ユーザーは、開発者およびように設計および Xamarin.Forms を使用してエンタープライズのクロスプラット フォーム アプリを実装する方法を説明する設計者では主にです。

セカンダリの対象ユーザーは、Xamarin.Forms を使用してクロスプラット フォームのエンタープライズ アプリの開発を選択する方法を決める前に、アーキテクチャとテクノロジの概要を受信する技術の意思決定者です。

## <a name="how-to-use-this-guide"></a>このガイドを使用する方法

このガイドでは、Xamarin.Forms を使用してクロスプラット フォームのエンタープライズ アプリの構築に焦点を当てています。 そのため、このようなアプリと、技術的な考慮事項を理解するための基盤を提供するには、その全体に読み取る必要があります。 サンプル アプリとともに、ガイドは、開始ポイントまたは新しいエンタープライズ アプリを作成するための参照としても使用できます。 新しいアプリ、またはのアプリのコンポーネント部分を整理する方法については、テンプレートとして、関連するサンプル アプリを使用します。 次に、参照アーキテクチャに関するガイダンスについては、このガイドをします。

自由に、Xamarin.Forms を使用してクロスプラット フォームのエンタープライズ アプリ開発の共通理解を確保するチーム メンバーには、このガイドを転送してください。 用語の共通セットを使用して作業との基本原則を基になるすべてのユーザーを持つには、一貫性のあるアプリケーションのアーキテクチャのパターンとプラクティスをすることができます。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
