---
title: 複数のプラットフォームでコードの共有
description: このドキュメントは、ポータブル クラス ライブラリ、共有プロジェクト、.NET Standard、および NuGet など、コードを共有する方法について説明するさまざまなガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: bfca620848bef174e78d9d34b6fdc497dda8f1de
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233225"
---
# <a name="sharing-code-on-multiple-platforms"></a>複数のプラットフォームでコードの共有

これらの記事では、Windows、Android、iOS、および詳細を含む、プラットフォームでコードを共有するために使用できるさまざまなオプションについて説明します。

## <a name="code-sharing-overviewcode-sharingmd"></a>[コード共有の概要](code-sharing.md)

Xamarin プロジェクトでは、.NET Standard ライブラリおよび共有プロジェクトを含む使用可能なオプションを共有する別のコードについて説明します。 ポータブル クラス ライブラリがサポートされても、非推奨の .NET Standard ためただしと見なされます。

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard はプラットフォーム間でコードを共有するための推奨されるオプションです。 特定のバージョンに対してコードをビルド (2.0 では、既存の .NET Framework コードで最適な API の互換性を提供します) と、そのレベルをサポートするその他のプロジェクトで使用またはより高いことができます。 For mac。 .NET standard プロジェクトが Visual Studio 2017 と Visual Studio の両方でサポートされています

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)

共有プロジェクトでは、複数の異なるアプリケーション プロジェクトによって参照される共通のコードを記述できます。 コードでは、各参照元のプロジェクトの一部としてコンパイルされ、共有コード ベースでプラットフォーム固有の機能を組み込むためのコンパイラ ディレクティブを含めることができます。 この記事では、共有プロジェクトのしくみ、および作成し、Xamarin プロジェクトで使用する方法について説明します。

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)

ポータブル クラス ライブラリ プロジェクトをビルドし、複数のプラットフォームで実行する共有コードが含まれているアセンブリを配布できます。 ポータブル クラス ライブラリ (または"の PCL") を作成するには、は、まずを対象とし、これらのプラットフォームに対して定義されているプロファイルで使用できる .NET Framework のサブセットに対してコードを記述するプラットフォームを選択します。 Pcl は、最新バージョンの Visual Studio; で非推奨となると見なされます開発者は、代わりに .NET Standard 2.0 を使用することが推奨されます。

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet プロジェクト:コードを共有するためのマルチプラット フォーム ライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet パッケージを PCL または .NET standard プロジェクトから自動的に生成できます。および共有プロジェクトを別の NuGet プロジェクトの種類を使用して、「おとり」NuGet パッケージにパッケージ化できます。 このセクションでは、各コード共有のシナリオ用の NuGet パッケージを作成する方法について説明します。

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin 用 NuGet パッケージを手動で作成します。](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin プラットフォームで動作する NuGet パッケージの作成に関するヒント。

## <a name="use-cc-libraries-in-cross-platform-xamarin-projectscross-platformcppindexmd"></a>[クロス プラットフォーム Xamarin プロジェクトで C/C++ ライブラリを使用します。](~/cross-platform/cpp/index.md)

この手法を使用すると、C と C++ のライブラリが進化したものを切り離す、 C# NuGet、および Xamarin アプリケーションでバインドします。 機能、ネイティブ プラットフォームの C/C++ ライブラリによって提供されますが、すべてのプラットフォーム固有のコードはコードの重複を指定できる最大のパフォーマンスをできるように、Xamarin の最終的なアプリケーションから分離されます。 
