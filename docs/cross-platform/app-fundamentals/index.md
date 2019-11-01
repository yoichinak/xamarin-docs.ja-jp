---
title: 複数のプラットフォームでコードを共有する
description: このドキュメントでは、ポータブルクラスライブラリ、共有プロジェクト、.NET Standard、NuGet など、コードを共有するための手法について説明しているさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: davidortinau
ms.author: daortin
ms.date: 07/18/2018
ms.openlocfilehash: a91fba3cd1fba3bcf2317e8f9cb25631c62491cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016828"
---
# <a name="sharing-code-on-multiple-platforms"></a>複数のプラットフォームでコードを共有する

これらの記事では、Windows、Android、iOS など、プラットフォーム間でコードを共有するために使用できるさまざまなオプションについて説明します。

## <a name="code-sharing-overviewcode-sharingmd"></a>[コード共有の概要](code-sharing.md)

.NET Standard ライブラリや共有プロジェクトなど、Xamarin プロジェクトで使用できるさまざまなコード共有オプションについて説明します。 ポータブルクラスライブラリもサポートされていますが、.NET Standard を優先するため非推奨と見なされます。

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET Standard は、プラットフォーム間でコードを共有する場合に推奨されるオプションです。 コードは特定のバージョンに対してビルドされ (2.0 は、既存の .NET Framework コードとの最高の API 互換性を提供します)、そのレベル以上をサポートする他のプロジェクトで使用できます。 .NET Standard プロジェクトは、Visual Studio 2019 と Visual Studio 2019 for Mac の両方でサポートされています。

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)

共有プロジェクトを使用すると、さまざまなアプリケーションプロジェクトによって参照される共通のコードを記述できます。 このコードは、参照している各プロジェクトの一部としてコンパイルされ、共有コードベースにプラットフォーム固有の機能を組み込むためのコンパイラディレクティブを含めることができます。 この記事では、共有プロジェクトの動作と、Xamarin プロジェクトで共有プロジェクトを作成して使用する方法について説明します。

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)

ポータブルクラスライブラリプロジェクトを使用すると、複数のプラットフォームで実行する共有コードを含むアセンブリをビルドおよび配布できます。 ポータブルクラスライブラリ ("PCL") を作成するには、まずターゲットにするプラットフォームを選択してから、それらのプラットフォーム用に定義されているプロファイルで使用可能な .NET Framework のサブセットに対してコードを記述します。 PCLs は、最新バージョンの Visual Studio では非推奨と見なされます。開発者は、代わりに .NET Standard 2.0 を使用することをお勧めします。

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet プロジェクト: コード共有用のマルチプラットフォームライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet パッケージは、PCL または .NET standard プロジェクトから自動的に生成されます。と共有プロジェクトは、別個の NuGet プロジェクトの種類を使用して "bait および switch" NuGet パッケージにパッケージ化できます。 このセクションでは、コード共有シナリオごとに NuGet パッケージを作成する方法について説明します。

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin の NuGet パッケージを手動で作成する](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin プラットフォームで動作する NuGet パッケージを作成するためのヒントです。

## <a name="use-cc-libraries-in-cross-platform-xamarin-projectscross-platformcppindexmd"></a>[クロスプラットフォーム XamarinC++プロジェクトで C/ライブラリを使用する](~/cross-platform/cpp/index.md)

この手法を使用すると、C/C++ライブラリの進化、NuGet でC#のバインド、および Xamarin アプリケーションを分離できます。 機能はネイティブプラットフォームの C/C++ライブラリによって提供されますが、プラットフォーム固有のすべてのコードは、最終的な Xamarin アプリケーションから分離されているため、コードを複製しなくても最高のパフォーマンスを実現できます。 
