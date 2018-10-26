---
title: Java からの移植C#
description: 3 つ目のオプションが Java ソース コードを移植する Xamarin.Android アプリケーションで Java を使用してをC#します。
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/05/2016
ms.openlocfilehash: 9beb6d59c9376a404c06af7f0cd1efd985929843
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104272"
---
# <a name="porting-java-to-c"></a>Java からの移植C#

_3 つ目のオプションが Java ソース コードを移植する Xamarin.Android アプリケーションで Java を使用してをC#します。_

## <a name="overview"></a>概要

この方法は、組織に関心のあります。

-  **テクノロジ スタックを Java から切り替えるC#します。**
-  **維持する必要があります、C#と同じ製品の Java バージョン。**
-  **Java 人気のあるライブラリの .NET バージョンがあることにします。**


Java コードを移植する 2 つの方法はありますC#します。 最初の方法は、手動でコードの移植です。 これには、.NET と Java の両方を理解し、各言語の適切な手法に精通した開発者のスキルを持つ必要があります。 このアプローチにより、最も意味の小規模なコード、またはから Java を完全に移行したい組織C#します。

2 番目の移植方法論がコード コンバーターを使用して、プロセスを自動化し、[シャープ](https://github.com/mono/sharpen)します。 [シャープ](https://github.com/mono/sharpen)は、オープン ソースのコンバーターが、コードの移植に使用されていた Versant から*db4o*を Java からC#します。 db4o は、Versant は Java で開発し、.NET に移植し、オブジェクト指向のデータベースです。 コード コンバーターを使用するうえで適したプロジェクトの両方の言語でに存在する必要があり、2 つの間のパリティを必要とする可能性があります。

ときに、自動コード変換ツールが意味の例で確認できます、 [ngit](https://github.com/mono/ngit)プロジェクト。
Java プロジェクトのポートである Ngit [jgit](http://eclipse.org/)します。
Jgit 自体はの Java 実装、 [Git](http://git-scm.com/)ソース コード管理システム。 生成するC#コードから Java、自動化された jgit から Java コードを抽出、変換処理に対応するためにいくつかの修正プログラムを適用してから、シャープを生成するを実行するシステムのカスタム ngit プログラマの使用、C#コード。 これにより、継続的かつ継続的な作業は、jgit で行われるからメリットを得る ngit プロジェクトです。

多くの場合、作業量が自明でない自動コード変換ツールでは、ブートス トラップに関連する、これを使用する妨げ証明可能性があります。 多くの場合、可能性がある単純かつ容易にポート JavaC#を手動でします。



## <a name="related-links"></a>関連リンク

- [変換ツールを高めましょう](https://github.com/mono/sharpen)
