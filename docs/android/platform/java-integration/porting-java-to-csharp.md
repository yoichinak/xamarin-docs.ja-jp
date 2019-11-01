---
title: Java を Xamarin C# Android に移植する
description: Xamarin Android アプリケーションで Java を使用するための3つ目のオプションは、Java ソースコードC#をに移植することです。
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/05/2016
ms.openlocfilehash: 8f96fcc4aadcd8f082d55dc568b2517f048edaf2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027190"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>Java を Xamarin C# Android に移植する

このアプローチは、次のような組織にとって重要な場合があります。

- **テクノロジスタックを Java からにC#切り替えています。**
- **C#とは、同じ製品の Java バージョンを維持する必要があります。**
- **一般的な Java ライブラリの .NET バージョンを用意する必要があります。**

Java コードをにC#移植するには、次の2つの方法があります。 最初の方法は、コードを手動で移植することです。 これには、.NET と Java の両方を理解し、各言語の適切な表現方法に精通している、熟練した開発者が関与します。 この方法は、少量のコード、または Java からにC#完全に移行する組織にとって最も効果的です。

2番目の移植方法では、[シャープ](https://github.com/mono/sharpen)などのコードコンバーターを使用してプロセスを自動化します。 [シャープ](https://github.com/mono/sharpen)は、Versant からのオープンソースコンバーターでC#あり、当初は Java から*db4o*のコードを移植するために使用されていました。 db4o は、Java で開発した後、.NET に移植されたオブジェクト指向データベースです。 コードコンバーターの使用は、両方の言語に存在する必要があり、2つの間に何らかのパリティを必要とするプロジェクトに適しています。

たとえば、自動化されたコード変換ツールを使用すると、 [ngit](https://github.com/mono/ngit)プロジェクトでそのようなことがわかります。
Ngit は、Java プロジェクト[jgit](https://eclipse.org/)のポートです。
Jgit 自体は、 [Git](https://git-scm.com/)ソースコード管理システムの Java 実装です。 Java からC#コードを生成するには、ngit プログラマーはカスタム自動化システムを使用して Jgit から java コードを抽出し、変換プロセスに対応するためにいくつかの修正プログラムC#を適用してから、コードを生成するシャープを実行します。 これにより、ngit プロジェクトは、jgit で実行される継続的な継続的な作業の恩恵を受けることができます。

多くの場合、自動化されたコード変換ツールのブートストラップに関連する作業量はごくわずかであり、これは使用する障壁であることが証明されます。 多くの場合、Java C#を手動で移植する方が簡単で簡単です。

## <a name="related-links"></a>関連リンク

- [シャープ変換ツール](https://github.com/mono/sharpen)
