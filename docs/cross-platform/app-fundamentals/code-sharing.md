---
title: 共有コードの概要
description: このドキュメントは、クロス プラットフォーム プロジェクト間でコードを共有するためのさまざまな方法を比較します。その方法には、共有プロジェクト、ポータブル クラス ライブラリ、.NET Standard があり、それぞれの長所と短所についても取り上げます。
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 82a73619e4c0507e8857cc91d88ababa870013de
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270473"
---
# <a name="sharing-code-overview"></a>共有コードの概要

_このドキュメントは、クロス プラットフォーム プロジェクト間でコードの共有のさまざまな方法を比較します。 .NET Standard、共有プロジェクト、およびポータブル クラス ライブラリ、長所と短所の各など。_

クロスプラット フォーム対応のアプリケーション間でコードを共有するための 3 つの方法はあります。

- [**.NET standard ライブラリ**](#Net_Standard) – .NET Standard プロジェクトを複数のプラットフォームで共有できるコードを実装することができ、(バージョン) によって多数の .NET Api にアクセスできます。 .NET Standard 2.0 の最適なカバレッジを提供しますが、Api の .NET standard 1.0 ~ 1.6 の実装が徐々 に大きくなるを設定します。
- [**共有プロジェクト**](#Shared_Projects) – 共有アセット プロジェクトの種類を使用して、ソース コードを整理し、使用`#if`コンパイラ ディレクティブのプラットフォームに固有の要件を管理するために必要とします。
- [**ポータブル クラス ライブラリ**](#Portable_Class_Libraries) (非推奨)-ポータブル クラス ライブラリ (Pcl) ことができます、共通 API サーフェイスで複数のプラットフォームをターゲットし、プラットフォーム固有の機能を提供するインターフェイスを使用します。 最新バージョンの Visual Studio での Pcl は非推奨&ndash;代わりに .NET Standard を使用します。

コード共有戦略の目標は、単一のコードベースを複数のプラットフォームで利用できます、この図に示したアーキテクチャをサポートするためにです。

 ![アプリケーション アーキテクチャのコードを共有](code-sharing-images/conceptualarchitecture.png "アプリケーション アーキテクチャのコードを共有")

この記事では、アプリケーションの適切なプロジェクトの種類を選択するために使用できるメソッドを比較します。

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET Standard ライブラリ

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md)ライブラリが適切に定義された一連の Xamarin.Android と Xamarin.iOS のようなクロス プラットフォーム プロジェクトなど、別のプロジェクトの種類で参照できる基本クラス ライブラリを提供します。 既存の .NET Framework コードとの最大限の互換性には、.NET standard 2.0 を使用することをお勧めします。

![.NET standard のダイアグラム](code-sharing-images/netstandard.png ".NET Standard のダイアグラム")

### <a name="benefits"></a>利点

- 複数のプロジェクトでコードを共有することができます。
- リファクタリング操作は、常に影響を受けるすべての参照を更新します。
- .NET 基本クラス ライブラリ (BCL) のより大きなサーフェイス領域は PCL プロファイルよりも使用できます。 具体的には、.NET Standard 2.0 では、.NET Framework とほぼ同じ API サーフェスを持つし、新しいアプリを移植の既存の Pcl をお勧めします。

### <a name="disadvantages"></a>短所

- ようなコンパイラ ディレクティブを使用することはできません`#if __IOS__`します。

### <a name="remarks"></a>Remarks

.NET standard は[PCL のような](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries)がプラットフォームのサポートより多くの BCL からクラスの単純なモデルです。

<a name="Shared_Projects" />

## <a name="shared-projects"></a>共有プロジェクト

[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)コード ファイルとそれらを参照する任意のプロジェクトに含まれている資産が含まれます。 共有プロジェクトでは、独自のコンパイル済みの出力は生成されません。

このスクリーン ショットは、使用 (Android、iOS、および Windows) の 3 つのアプリケーション プロジェクトを含むソリューション ファイルを示しています、 **Shared**一般的な c# ソース コード ファイルを含むプロジェクト。

![プロジェクトのソリューションを共有](code-sharing-images/sharedsolution.png "プロジェクト ソリューションの共有")

概念的なアーキテクチャは、各プロジェクトがすべての共有ソース ファイルに含まれる場合、次の図に示されます。

![共有プロジェクト ダイアグラム](code-sharing-images/sharedassetproject.png "共有プロジェクトのダイアグラム")

### <a name="example"></a>例

IOS、Android、および Windows をサポートするクロス プラットフォーム アプリケーションは、プラットフォームごとに、アプリケーション プロジェクトを必要となります。 一般的なコードは、共有プロジェクトに住んでいます。

ソリューションの例は、次のフォルダーとプロジェクト (プロジェクト名に選択されている表現力には、プロジェクトは、これらの名前付けのガイドラインに従う必要はありません) が含まれます。

- **共有**– すべてのプロジェクトに共通のコードを含む、共有プロジェクト。
- **AppAndroid** – Xamarin.Android アプリケーションのプロジェクト。
- **AppiOS** – Xamarin.iOS アプリケーションのプロジェクト。
- **AppWindows** – Windows アプリケーション プロジェクト。

この方法で 3 つのアプリケーション プロジェクトが同じソース コード (c# 内のファイル共有) を共有します。 共有コードへの編集は、次の 3 つすべてのプロジェクト間で共有されます。

### <a name="benefits"></a>利点

- 複数のプロジェクトでコードを共有することができます。
- 共有コードをコンパイラ ディレクティブ (例: を使用するプラットフォームに基づいて分岐できます。 使用して`#if __ANDROID__`で説明したように、[クロス プラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)ドキュメント)。
- アプリケーション プロジェクトで共有コードを使用するプラットフォームに固有の参照を含めることができます (を使用してなど`Community.CsharpSqlite.WP7`Tasky サンプル Windows Phone の)。

### <a name="disadvantages"></a>短所

- 'Inactive' のコンパイラ ディレクティブ内のコードに影響を与えるリファクタリングでは、それらのディレクティブ内のコードは更新されません。
- その他のほとんどのプロジェクト タイプとは異なり、共有プロジェクトには 'output' アセンブリがありません。 コンパイル時に、ファイルが参照元のプロジェクトの一部として扱われます、そのアセンブリにコンパイルします。 アセンブリとしてコードを共有する場合、.NET Standard またはポータブル クラス ライブラリがより優れたソリューションです。

<a name="Shared_Remarks" />

### <a name="remarks"></a>Remarks

コードを作成するアプリケーション開発者に最適なソリューションは、アプリケーション内の共有 (および他の開発者に配布しない) のみです。

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

> [!TIP]
> .NET standard 2.0 ライブラリは、ポータブル クラス ライブラリよりも推奨されます。

ポータブル クラス ライブラリは[詳細はここで説明した](~/cross-platform/app-fundamentals/pcl.md)します。

![ポータブル クラス ライブラリのダイアグラム](code-sharing-images/portableclasslibrary.png "ポータブル クラス ライブラリのダイアグラム")

### <a name="benefits"></a>利点

- 複数のプロジェクトでコードを共有することができます。
- リファクタリング操作は、常に影響を受けるすべての参照を更新します。

### <a name="disadvantages"></a>短所

- 最新バージョンの Visual Studio では非推奨、代わりに .NET Standard ライブラリが推奨されます。 これを参照してください[の相違点について](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries)PCL および .NET Standard の間。
- コンパイラ ディレクティブを使用することはできません。
- のみ、.NET framework のサブセットが使用するには使用可能な選択したプロファイルによって決まります (を参照してください、 [PCL の概要](~/cross-platform/app-fundamentals/pcl.md)の詳細)。

### <a name="remarks"></a>Remarks

PCL のテンプレートでは、最新バージョンの Visual Studio では非推奨と見なされます。

## <a name="summary"></a>まとめ

コード共有方法を選択するは対象とするプラットフォームに基づいて構築します。 プロジェクトの最適な方法を選択します。

.NET standard には (特に、NuGet に発行する) に共有できるコード ライブラリの構築に最適な選択肢を示します。 共有プロジェクトは、計画のクロス プラットフォーム アプリで多くのプラットフォームに固有の機能を使用するアプリケーション開発者にとっても作業します。

PCL プロジェクトは引き続き Visual Studio でサポートされるは新しいプロジェクトの .NET Standard がお勧めします。

## <a name="related-links"></a>関連リンク

- [クロス プラットフォーム アプリケーションの構築 (メイン文書)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)
- [共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [ケース スタディ: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky サンプル (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL (github) を使用してサンプルを tasky](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
