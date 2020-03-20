---
title: Java を Xamarin.Android 用の C# に移植する
description: Xamarin.Android アプリケーションで Java を使用するための 3 つ目のオプションは、Java ソース コードを C# に移植することです。
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/05/2016
ms.openlocfilehash: 8f96fcc4aadcd8f082d55dc568b2517f048edaf2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027190"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>Java を Xamarin.Android 用の C# に移植する

このアプローチは、次のような組織にとって重要な場合があります。

- **テクノロジ スタックを Java から C# に切り替えている。**
- **同じ製品の C# バージョンと Java バージョンを維持する必要がある。**
- **人気のある Java ライブラリの .NET バージョンを用意する必要がある。**

Java コードを C# に移植するには、2 つの方法があります。 最初の方法は、コードを手作業で移植することです。 これには、.NET と Java の両方を理解し、各言語の適切な構文に精通している、熟練した開発者が必要です。 この方法は、コードが少量の場合、または組織が Java から C# に完全に移行しようとしている場合に、最も効果的です。

2 番目の移植方法は、[Sharpen](https://github.com/mono/sharpen) などのコード コンバーターを使用してプロセスを試し、自動化することです。 [Sharpen](https://github.com/mono/sharpen) は Versant によるオープンソースのコンバーターであり、もともとは *db4o* 用のコードを Java から C# に移植するために使用されていました。 db4o は、Versant が Java で開発したオブジェクト指向データベースであり、後に .NET に移植されました。 コード コンバーターの使用は、両方の言語で存在する必要があり、2 つの間に何らかの類似性を必要とするプロジェクトに、適している場合があります。

自動化されたコード変換ツールが有効である場合の例は、[ngit](https://github.com/mono/ngit) プロジェクトで確認できます。
ngit は、Java プロジェクトの [jgit](https://eclipse.org/) を移植したものです。
jgit 自体は、[Git](https://git-scm.com/) ソース コード管理システムの Java による実装です。 Java から C# コードを生成するため、ngit のプログラマーは、自動化されたカスタム システムを使用して jgit から Java コードを抽出し、変換プロセスに対応するためのいくつかのパッチを適用してから、C# コードを生成する Sharpen を実行しています。 これにより、ngit プロジェクトでは、jgit で現在も行われている作業から継続的に恩恵を受けることができます。

自動コード変換ツールの使用開始時にはかなりの作業が伴うことがよくあり、それによって使用が妨げらる場合があります。 多くの場合、手作業で Java を C# に移植する方が単純で簡単です。

## <a name="related-links"></a>関連リンク

- [Sharpen 変換ツール](https://github.com/mono/sharpen)
