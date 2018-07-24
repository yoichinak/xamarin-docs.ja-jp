---
title: C# に移植 Java
description: Xamarin.Android アプリケーションで Java を使用するための 3 番目のオプションは Java ソース コードを c# に移植することです。
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: c2d05b101c627dab42dc1343eab2a408d1bd010f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762598"
---
# <a name="porting-java-to-c"></a>C# に移植 Java

_Xamarin.Android アプリケーションで Java を使用するための 3 番目のオプションは Java ソース コードを c# に移植することです。_

## <a name="overview"></a>概要

この方法は、組織を対象します。

-  **テクノロジ スタック Java からに切り替える (C#)。**
-  **C# と、同じ製品の Java バージョンを維持する必要があります。**
-  **一般的な Java ライブラリの .NET バージョンが必要です。**


これには Java コードを c# に移植する 2 つの方法があります。 最初の方法では、コードを手動でポートです。 これには、.NET と Java の両方を理解し、各言語の適切な表現方法に慣れているスキルを持つ開発者が含まれます。 このアプローチでは、少量のコード、または c# Java から完全に移行したい組織にとって最適なにより、します。

2 番目の移植方法論がコンバーターを使用して、コードなど、プロセスを自動化する[シャープ](https://github.com/mono/sharpen)です。 [シャープ](https://github.com/mono/sharpen)はオープン ソースのコンバーターが、コードの移植に使用されていた Versant から*db4o* c# Java からです。 db4o は Versant Java では、開発し、.NET に移植し、オブジェクト指向データベースです。 コードのコンバーターを使用してが有意義の両方の言語に存在する必要があり、2 つの間のパリティを必要とするプロジェクトです。

自動化されたコード変換ツールがした場合は意味の例がわかるように、 [ngit](https://github.com/mono/ngit)プロジェクト。
Ngit は Java プロジェクトの移植[jgit](http://eclipse.org/)です。
Jgit 自体は、Java で実装、 [Git](http://git-scm.com/)ソース コード管理システムです。 Java から c# コードを生成するには、ngit プログラマは、jgit から Java コードを抽出、変換プロセスに合わせて一部の修正プログラムを適用し、シャープ ツールは、c# コードを生成するカスタムの自動化されたシステムを使用します。 これにより、jgit で実行される継続で継続的な作業から利点を得る ngit プロジェクトです。

多くの場合、重要な作業量、自動化されたコード変換ツールのブートス トラップに関連して、これを使用する妨げ証明可能性があります。 多くの場合がありますシンプルかつ c# ポート Java を容易に手動で。



## <a name="related-links"></a>関連リンク

- [シャープ変換ツール](https://github.com/mono/sharpen)
