---
title: コードの共有の概要
description: このドキュメントでは、クロスプラットフォームプロジェクト間でコードを共有するさまざまな方法 (共有プロジェクト、ポータブルクラスライブラリ、および .NET Standard) を比較します。それぞれの利点と欠点を示します。
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: davidortinau
ms.author: daortin
ms.date: 08/06/2018
ms.openlocfilehash: 78b849434a087cf7951fe36345688251885ea00b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016898"
---
# <a name="sharing-code-overview"></a>コードの共有の概要

_このドキュメントでは、クロスプラットフォームプロジェクト (.NET Standard、共有プロジェクト、およびポータブルクラスライブラリ) 間でコードを共有するさまざまな方法 (それぞれの利点と欠点を含む) を比較します。_

クロスプラットフォームアプリケーション間でコードを共有するには、次の3つの方法があります。

- [ **.NET Standard ライブラリ**](#Net_Standard)– .NET Standard プロジェクトは、複数のプラットフォーム間で共有されるコードを実装できます。また、多数の .net api にアクセスできます (バージョンによって異なります)。 .NET Standard 1.0-1.6 は、徐々に大きくなる Api のセットを実装しますが、.NET Standard 2.0 は .NET BCL (Xamarin アプリで利用可能な .NET Api を含む) の最適な範囲を提供します。
- [**共有プロジェクト**](#Shared_Projects)–共有アセットプロジェクトタイプを使用してソースコードを整理し、必要に応じて `#if` コンパイラディレクティブを使用してプラットフォーム固有の要件を管理します。
- [**ポータブルクラスライブラリ**](#Portable_Class_Libraries)(非推奨) –ポータブルクラスライブラリ (pcl) は、共通の API サーフェイスを使用して複数のプラットフォームを対象とし、インターフェイスを使用してプラットフォーム固有の機能を提供できます。 PCLs は、Visual Studio の最新バージョンでは非推奨とされており、代わりに .NET Standard を使用 &ndash; ます。

コード共有戦略の目的は、この図に示すアーキテクチャをサポートすることです。ここでは、単一のコードベースを複数のプラットフォームで使用できます。

 ![共有コードアプリケーションのアーキテクチャ](code-sharing-images/conceptualarchitecture.png "共有コードアプリケーションのアーキテクチャ")

この記事では、アプリケーションに適したプロジェクトの種類を選択するために使用できる方法を比較します。

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET Standard ライブラリ

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)ライブラリは、さまざまな種類のプロジェクトで参照できる、適切に定義された基本クラスライブラリのセットを提供します。これには、xamarin Android や xamarin などのクロスプラットフォームプロジェクトが含まれます。 既存の .NET Framework コードとの互換性を最大にするには、.NET Standard 2.0 をお勧めします。

![.NET Standard ダイアグラム](code-sharing-images/netstandard.png ".NET Standard ダイアグラム")

### <a name="benefits"></a>利点

- を使用すると、複数のプロジェクト間でコードを共有できます。
- リファクタリング操作は、常に、影響を受けるすべての参照を更新します。
- .NET 基底クラスライブラリ (BCL) の大きな領域は、PCL プロファイルで使用できます。 特に、.NET Standard 2.0 は、.NET Framework とほぼ同じ API サーフェイスを備えており、新しいアプリには、既存の PCLs を移植することをお勧めします。

### <a name="disadvantages"></a>短所

- `#if __IOS__`のようなコンパイラディレクティブは使用できません。

### <a name="remarks"></a>Remarks

.NET Standard は[PCL に似](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries)ていますが、プラットフォームのサポートと BCL のクラスの数がより単純なモデルになっています。

<a name="Shared_Projects" />

## <a name="shared-projects"></a>共有プロジェクト

[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)には、コードファイルと、それらを参照するプロジェクトに含まれるアセットが含まれています。 プロジェクトの共有は、コンパイルされた出力を独自に生成しません。

このスクリーンショットは、次の3つのアプリケーションプロジェクト (Android、iOS、および Windows 用) を含むソリューションファイルを示しC#ています。共有プロジェクトには、共通のソースコードファイルが含まれています。

![共有プロジェクトソリューション](code-sharing-images/sharedsolution.png "共有プロジェクトソリューション")

概念アーキテクチャは次の図のようになります。各プロジェクトには、すべての共有ソースファイルが含まれています。

![共有プロジェクトダイアグラム](code-sharing-images/sharedassetproject.png "共有プロジェクトダイアグラム")

### <a name="example"></a>例

IOS、Android、および Windows をサポートするクロスプラットフォームアプリケーションでは、プラットフォームごとにアプリケーションプロジェクトが必要になります。 共通コードは共有プロジェクトに存在します。

ソリューションの例には、次のフォルダーとプロジェクトが含まれています (表現力のためにプロジェクト名が選択されています。プロジェクトでは、これらの名前付けガイドラインに従う必要はありません)。

- **共有**–すべてのプロジェクトに共通のコードを含む共有プロジェクト。
- **Appandroid** – Xamarin アプリケーションプロジェクト。
- **Appios** – Xamarin ios アプリケーションプロジェクト。
- **Appwindows** – windows アプリケーションプロジェクト。

この方法では、3つのアプリケーションプロジェクトが同じソースコード ( C#共有内のファイル) を共有しています。 共有コードの編集は、3つのプロジェクトすべてで共有されます。

### <a name="benefits"></a>利点

- を使用すると、複数のプロジェクト間でコードを共有できます。
- 共有コードは、コンパイラディレクティブを使用してプラットフォームに基づいて分岐できます ( 「[クロスプラットフォームアプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)」ドキュメントで説明されているように、`#if __ANDROID__` の使用。
- アプリケーションプロジェクトには、共有コードが利用できるプラットフォーム固有の参照 (Windows Phone の Tasky サンプルでの `Community.CsharpSqlite.WP7` の使用など) を含めることができます。

### <a name="disadvantages"></a>短所

- ' Inactive ' コンパイラディレクティブ内のコードに影響を与えるリファクタリングは、これらのディレクティブ内のコードを更新しません。
- 他のほとんどのプロジェクトの種類とは異なり、共有プロジェクトには ' output ' アセンブリがありません。 コンパイル時に、ファイルは参照元のプロジェクトの一部として処理され、そのアセンブリにコンパイルされます。 コードをアセンブリとして共有する場合は、.NET Standard またはポータブルクラスライブラリを使用することをお勧めします。

<a name="Shared_Remarks" />

### <a name="remarks"></a>Remarks

アプリケーション開発者向けの優れたソリューションで、アプリ内での共有のみを目的としたコードを作成できます (他の開発者に配布することはできません)。

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

> [!TIP]
> ポータブルクラスライブラリよりも .NET Standard 2.0 ライブラリをお勧めします。

ポータブルクラスライブラリの[詳細につい](~/cross-platform/app-fundamentals/pcl.md)ては、こちらを参照してください。

![ポータブルクラスライブラリの図](code-sharing-images/portableclasslibrary.png "ポータブルクラスライブラリの図")

### <a name="benefits"></a>利点

- を使用すると、複数のプロジェクト間でコードを共有できます。
- リファクタリング操作は、常に、影響を受けるすべての参照を更新します。

### <a name="disadvantages"></a>短所

- Visual Studio の最新バージョンで非推奨とされました。代わりに .NET Standard ライブラリをお勧めします。 PCL と .NET Standard[の違いについては、](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries)こちらを参照してください。
- コンパイラディレクティブは使用できません。
- 使用できる .NET framework のサブセットは、選択したプロファイルによって決まります (詳細については、「 [PCL の概要」を](~/cross-platform/app-fundamentals/pcl.md)参照してください)。

### <a name="remarks"></a>Remarks

PCL テンプレートは、最新バージョンの Visual Studio では非推奨と見なされます。

## <a name="summary"></a>まとめ

選択するコード共有戦略は、対象とするプラットフォームによって決まります。 プロジェクトに最適な方法を選択します。

.NET Standard は、共有可能なコードライブラリ (特に NuGet での公開) を構築する場合に最適な選択肢です。 共有プロジェクトは、クロスプラットフォームアプリでプラットフォーム固有の多くの機能を使用することを計画しているアプリケーション開発者に適しています。

PCL プロジェクトは Visual Studio で引き続きサポートされますが、新しいプロジェクトには .NET Standard をお勧めします。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームアプリケーションのビルド (メインドキュメント)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)
- [共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [ケース スタディ: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky サンプル (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL を使用した tasky のサンプル (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
