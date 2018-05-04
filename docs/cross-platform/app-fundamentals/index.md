---
title: コードの共有
description: 中核となるアプリケーションの概念
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 01116a35dca80cd92ea16232a2abb127f60d9f0a
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="sharing-code"></a>コードの共有

このセクションでは、いくつかの一般的な処理タスクまたはモバイル アプリケーションを開発するときに注意する必要がある開発者の概念のガイドを提供します。

## <a name="code-sharing-overviewcode-sharingmd"></a>[コード共有の概要](code-sharing.md)

Xamarin プロジェクト、ポータブル クラス ライブラリ (Pcl)、共有プロジェクト、および標準の .NET ライブラリを含む使用可能なオプションを共有する別のコードについて説明します。


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)

ポータブル クラス ライブラリ プロジェクトでは、ビルドし、複数のプラットフォームで実行する共有コードを含むアセンブリを配布できます。 ポータブル クラス ライブラリ (または"PCL") を作成するには、最初を対象とし、設定は、これらのプラットフォームに対して定義されているプロファイルで使用可能な .NET Framework のサブセットに対してコードを記述するプラットフォームを選択します。 このドキュメントでは、作成して、Xamarin を使用した Pcl を使用する方法について説明します。

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)

共有プロジェクトでは、別のアプリケーション プロジェクトの数によって参照されている共通のコードを記述できます。 コードでは、各参照元のプロジェクトの一部としてコンパイルされ、共有コード ベースのプラットフォーム固有の機能を組み込むためのコンパイラ ディレクティブを含めることができます。 この記事では、共有プロジェクトのしくみ、および作成して、Xamarin のプロジェクトでそれらを使用する方法について説明します。

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET 標準は、プラットフォーム間でコードを共有するための新しいオプションです。 ポータブル クラス ライブラリに同様の方法で動作します。コードでは、特定のバージョン (現在 1.0 1.6 ~) に対しては構築され、以降そのレベルをサポートするその他のプロジェクトで使用できます。 標準の .NET プロジェクトでは Xamarin Studio 6.2、Visual Studio for Windows、および Visual Studio for mac

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[コードを共有するための NuGet プロジェクト: マルチプラット フォーム ライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet パッケージを PCL プロジェクトまたは .NET 標準プロジェクト; から自動的に生成できます。共有プロジェクトを個別の NuGet のプロジェクトの種類を使用して「お連絡とり」NuGet パッケージにパッケージ化できます。 このセクションでは、各コードを共有するシナリオ用の NuGet パッケージを作成する方法について説明します。

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin の NuGet パッケージを手動で作成します。](~/cross-platform/app-fundamentals/nuget-manual.md)

NuGet パッケージを作成するためのヒントは、Xamarin プラットフォームで動作します。
