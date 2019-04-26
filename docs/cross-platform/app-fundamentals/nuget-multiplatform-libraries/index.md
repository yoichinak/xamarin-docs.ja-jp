---
title: NuGet のマルチプラット フォーム ライブラリ プロジェクト (Nugetizer 3000 とも呼ばれます)
description: このドキュメントでは、Nugetizer 3000 ツールを使用して、プラットフォーム間でコードを共有する NuGet パッケージを自動的に作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 6d3f7b316e397705ecb9bd404007dcd9ef5aa183
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266790"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>マルチプラット フォーム ライブラリ プロジェクトの NuGet (Nugetizer 3000)

_Nugetizer 3000 を使用してプラットフォーム間でコードを共有する NuGet パッケージを自動的に作成します。_

使用してプラットフォーム間でコードを共有する NuGet パッケージを自動的に作成することは、 _Nugetizer 3000_します。 これにより、新しいまたは既存のライブラリ プロジェクトから NuGet パッケージを作成することができます**マルチプラット フォーム ライブラリ プロジェクト**します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Nugetizer 3000 は Visual Studio for Mac に含まれている&ndash;を探して、**ライブラリ > Mulitplatform ライブラリ**プロジェクトの種類で、**ファイル > 新規**ウィンドウ。

[![](images/mulitplatform-library-sml.png "マルチプラット フォーム ライブラリの新しいウィンドウを作成します。")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で Nugetizer 3000 を使用してください。[をダウンロードして、VSIX インストーラーを実行](http://bit.ly/nugetizer-2017)します。

-----

## <a name="building-nuget-packages"></a>NuGet パッケージをビルドします。

プロジェクトのすべてのビルド出力コードを内部的にその他のアプリを共有するために使用またはアップロードすることができますが、完全な NuGet パッケージで構成されると、 [NuGet.org](https://www.nuget.org)します。

この機能を使用するための 3 つのシナリオがあります。

- [既存のライブラリ プロジェクト](existing-library.md)

  既存の PCL (または .NET Standard) のプロジェクトから NuGet パッケージを作成します。

- [新しいマルチプラット フォーム ライブラリ プロジェクトを作成します。](single-codebase.md)

  PCL または .NET Standard を使用して NuGet を使用して一般的なコードを共有する新しいライブラリを作成します。

- [新しいプラットフォームに固有のライブラリ プロジェクトの作成](platform-specific.md)

  IOS と Android のプラットフォームに固有のコードを含む、共有プロジェクトを使用して、一般的なコードと iOS または Android 固有の機能をサポートするプラットフォームに固有のプロジェクトを含めることが、NuGet、新しいライブラリを作成します。

参照してください、[メタデータ ガイド](metadata.md)詳細については、必須および省略可能なメタデータ、NuGet パッケージに追加する必要があります。

## <a name="further-nuget-information"></a>NuGet の詳細

詳細をご覧ください[Xamarin 用 Nuget を手動で作成する](~/cross-platform/app-fundamentals/nuget-manual.md)する方法と[アプリで NuGet パッケージを含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)します。

Microsoft の[NuGet のドキュメント](https://docs.microsoft.com/nuget/)でより詳細な情報が含まれています、 **.nupkg**形式と Visual Studio で NuGet パッケージを使用します。

NuGet パッケージのプロジェクトのデザインのディスカッション (別名。 NuGetizer 3000) は、 [NuGet GitHub リポジトリ](https://github.com/NuGet/Home/wiki/NuGetizer-3000)します。

## <a name="related-links"></a>関連リンク

- [NuGetizer 3000 ユース ケース](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Xamarin 用 NuGet パッケージを手動で作成します。](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet のドキュメント](https://docs.microsoft.com/nuget/)
