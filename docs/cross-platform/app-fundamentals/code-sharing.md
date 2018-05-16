---
title: コード共有のオプション
description: このドキュメントは、クロス プラットフォーム プロジェクト間でコードの共有のさまざまな方法を比較します。 共有プロジェクト、ポータブル クラス ライブラリ、および .NET Standard、長所と短所のそれぞれを含むです。
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: de2e24b1746568510c84fb163efa8562ab47cf00
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="sharing-code-overview"></a>共有コードの概要

_このドキュメントは、クロス プラットフォーム プロジェクト間でコードの共有のさまざまな方法を比較します。 共有プロジェクト、ポータブル クラス ライブラリ、および .NET Standard、長所と短所のそれぞれを含むです。_

クロス プラットフォーム アプリケーション間でコードを共有するための 3 つの代替方法はあります。

-   [**共有プロジェクト**](#Shared_Projects) : 共有アセット プロジェクト タイプを使用して、ソース コードを整理し、必要に応じて #if コンパイラ ディレクティブを使用してプラットフォーム固有の要件を管理します。
-   [**ポータブル クラス ライブラリ**](#Portable_Class_Libraries) –、サポート対象のプラットフォームを対象とするポータブル クラス ライブラリ (PCL) を作成して、プラットフォーム固有の機能を提供するインターフェイスを使用します。
-   [**.NET Standard ライブラリ**](#Net_Standard) – .NET Standardプロジェクトが Pcl、プラットフォーム固有の機能を挿入するインターフェイスの使用を必要とする同様に動作します。

コード共有の戦略の目標は、単一のコードベースを複数のプラットフォームで利用できますが、この図に示すアーキテクチャをサポートするためにです。

 ![](code-sharing-images/conceptualarchitecture.png "共有コード アプリケーションのアーキテクチャ")

この記事では、アプリケーションの適切なプロジェクトの種類を選択するための 3 つの方法を比較します。

<a name="Shared_Projects" />

## <a name="shared-projects"></a>共有プロジェクト

コード ファイルを共有する最も簡単な方法は、使用する、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)です。

この画面で (Android、iOS および Windows Phone) の 3 つのアプリケーション プロジェクトを含むソリューション ファイルを示しています、 **Shared**を一般的な c# ソース コード ファイルを含むプロジェクト。

 ![](code-sharing-images/sharedsolution.png "共有プロジェクトのソリューション")

概念的なアーキテクチャは、各プロジェクトがすべての共有されているソース ファイルに含まれる場合、次の図で示されます。

 ![](code-sharing-images/sharedassetproject.png "共有プロジェクトの図")


### <a name="example"></a>例

クロスプラットフォーム アプリケーションを iOS、Android、Windows Phone をサポートするには、各プラットフォームのアプリケーション プロジェクトが必要です。 共通のコードは、共有プロジェクトに住んでいます。

ソリューション例にはは、以下のフォルダー (プロジェクトの名前が選択されている、表現力のプロジェクトは、これらの名前付けガイドラインに従う必要はありません) プロジェクトが含まれます。

-   **共有**– すべてのプロジェクトに共通のコードを含む共有プロジェクト。
-   **AppAndroid** – Xamarin.Android アプリケーション プロジェクト。
-   **AppiOS** – Xamarin.iOS アプリケーション プロジェクト。
-   **AppWinPhone** – Windows Phone アプリケーション プロジェクト。


この方法で次の 3 つのアプリケーション プロジェクトが同じソース コード (c# 内のファイル共有) を共有します。 共有コードへの編集は、3 つすべてのプロジェクト間で共有されます。


### <a name="benefits"></a>利点

-  複数のプロジェクト間でコードを共有できます。
-  共有コードがコンパイラ ディレクティブ (を使用してプラットフォームに基づいて分岐できます。 使用して`#if __ANDROID__`で説明したように、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)ドキュメント)。
-  アプリケーション プロジェクトで共有コードが利用できるプラットフォーム固有の参照を含めることができます (を使用してなど`Community.CsharpSqlite.WP7`Windows Phone 用 Tasky のサンプルで)。



### <a name="disadvantages"></a>短所

-  他のほとんどのプロジェクト タイプとは異なり、共有プロジェクトには 'output' アセンブリがありません。 コンパイル時に、ファイルが参照元のプロジェクトの一部として扱われ、そのアセンブリにコンパイルします。 アセンブリとしてコードを共有する場合、ポータブル クラス ライブラリまたは .NET 標準がより優れたソリューションです。
-  リファクタリング 'inactive' コンパイラ ディレクティブ内のコードに影響を与えるには、コードは更新されません。


 <a name="Shared_Remarks" />

### <a name="remarks"></a>コメント

アプリケーション開発者がコードのみ対象読者は、アプリケーション内の共有 (およびその他の開発者に配布するのでない) を記述するのに適してします。

 <a name="Portable_Class_Libraries" />


## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ


ポータブル クラス ライブラリは[ここでは詳しく説明した](~/cross-platform/app-fundamentals/pcl.md)です。

 ![](code-sharing-images/portableclasslibrary.png "ポータブル クラス ライブラリのダイアグラム")


### <a name="benefits"></a>利点

-  複数のプロジェクト間でコードを共有できます。
-  リファクタリング操作は、常に影響を受けるすべての参照を更新します。


### <a name="disadvantages"></a>短所

-  コンパイラ ディレクティブを使用することはできません。
-  のみ、.NET framework のサブセットを使用して、選択したプロファイルによって決まります (を参照してください、 [PCL 概要](~/cross-platform/app-fundamentals/pcl.md)の詳細)。


### <a name="remarks"></a>コメント

結果として得られるアセンブリを他の開発者と共有する予定の場合は、適切なソリューションです。



<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET Standard ライブラリ

.NET Standardは[ここでは詳しく説明した](~/cross-platform/app-fundamentals/net-standard.md)です。

![](code-sharing-images/netstandard.png ".NET Standardダイアグラム")

### <a name="benefits"></a>利点

-  複数のプロジェクト間でコードを共有できます。
-  リファクタリング操作は、常に影響を受けるすべての参照を更新します。
-  大きなサーフェイス領域の .NET 基本クラス ライブラリ (BCL) は PCL プロファイルよりも使用できます。

### <a name="disadvantages"></a>短所

 -  コンパイラ ディレクティブを使用することはできません。

### <a name="remarks"></a>コメント

.NET StandardはPCLに似ていますが、プラットフォームサポートのためのモデルがよりシンプルで、BCLのクラス数も増えています。



## <a name="summary"></a>まとめ

コード共有を選択する戦略を対象とするプラットフォームによって推進されます。 プロジェクトの最適な方法を選択します。

PCLまたは.NET Standardが (特に、NuGet で公開する) の共有コード ライブラリを構築するための適切な選択肢です。 共有プロジェクトは、アプリケーション開発者は、クロスプラット フォーム アプリで多くのプラットフォーム固有の機能を使用する計画の適切に動作します。


## <a name="related-links"></a>関連リンク

- [クロス プラットフォーム アプリケーションの構築 (メイン ドキュメント)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)
- [共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [ケース スタディ: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky サンプル (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL (github) を使用してサンプルを tasky](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
