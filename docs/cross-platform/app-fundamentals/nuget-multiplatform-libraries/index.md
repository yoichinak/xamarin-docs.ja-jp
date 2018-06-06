---
title: NuGet のプロジェクト (Nugetizer 3000)
description: このドキュメントでは、Nugetizer 3000 ツールを使用してプラットフォーム間でコードを共有する NuGet パッケージを自動的に作成する方法について説明します。
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f79ed775173e05dd850fbc74c53127b63c0d5f34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780771"
---
# <a name="nuget-projects-nugetizer-3000"></a>NuGet のプロジェクト (Nugetizer 3000)

_' Nugetizer 3000' を使用してプラットフォーム間でコードを共有する NuGet パッケージを自動的に作成してください。_

使用してプラットフォーム間でコードを共有する NuGet パッケージを自動的に作成することは、 _Nugetizer 3000_です。 これにより、作成することは NuGet パッケージの既存のライブラリ プロジェクトから、または新しいを作成して**マルチプラット フォーム ライブラリ プロジェクト**です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nugetizer 3000 は Mac 6.2 用の Visual Studio に含まれています。

[![](images/mulitplatform-library-sml.png "マルチプラット フォーム ライブラリの新しいウィンドウを作成します。")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio で Nugetizer 3000 を使用する次のようにしてください。 [VSIX インストーラーをダウンロードして](http://bit.ly/nugetizer-2017)です。

-----

## <a name="building-nuget-packages"></a>NuGet パッケージの構築

その他のアプリを使用して内部的にコードを共有するために使用したりにアップロードする、完全な NuGet パッケージのプロジェクトのすべてのビルドの出力構成されると、 [NuGet.org](https://www.nuget.org)です。

この機能を使用する 3 つのシナリオがあります。

- [既存のライブラリ プロジェクト](existing-library.md)

  PCL (または .NET 標準) の既存のプロジェクトの NuGet パッケージを作成します。

- [マルチプラット フォーム ライブラリ プロジェクトを新規作成](single-codebase.md)

  PCL または .NET 標準を使用して NuGet 経由での一般的なコードを共有する新しいライブラリを作成します。

- [新しいプラットフォーム固有のライブラリ プロジェクトの作成](platform-specific.md)

  新しいライブラリと iOS および Android、プラットフォーム固有のコードを含む、共通コードおよび iOS または Android に固有の機能をサポートするプラットフォーム固有のプロジェクトを格納する共有プロジェクトを使用して NuGet を作成します。

参照してください、[メタデータ ガイド](metadata.md)詳細については、必須および省略可能なメタデータ、NuGet パッケージに追加する必要があります。


## <a name="further-nuget-information"></a>NuGet を詳細します。

詳細について[Xamarin 用 NuGets を手動で作成する](~/cross-platform/app-fundamentals/nuget-manual.md)する方法と[アプリ内で NuGet パッケージが含まれて](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)です。

Microsoft の[NuGet のドキュメント](https://docs.microsoft.com/nuget/)でより詳細な情報が含まれています、**これは .nupkg**形式と Visual Studio で NuGet パッケージを使用します。

NuGet パッケージのプロジェクトの設計に関する話し合い (ルール サブタイプ) NuGetizer 3000) は、 [NuGet GitHub リポジトリ](https://github.com/NuGet/Home/wiki/NuGetizer-3000)です。


## <a name="related-links"></a>関連リンク

- [NuGetizer 3000 ユース ケース](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Xamarin の NuGet パッケージを手動で作成します。](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet のドキュメント](https://docs.microsoft.com/nuget/)
- [リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
