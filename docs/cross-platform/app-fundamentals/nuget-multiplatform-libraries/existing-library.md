---
title: 既存のライブラリ プロジェクトから NuGet を作成します。
description: このドキュメントでは、他の開発者と共有するコードをできるように、既存のライブラリ プロジェクトから NuGet パッケージを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 6e043334d3ca45a573423ebdfdf1ec9149167b55
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864690"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>既存のライブラリ プロジェクトから NuGet を作成します。

既存の PCL または .NET Standard ライブラリを使用して Nuget に変換できます、**プロジェクト オプション**ウィンドウ。

1. ライブラリ プロジェクトを右クリックし、 **Solution Pad**選択**オプション**します。

2. 移動して、 **NuGet パッケージ > メタデータ**セクションし、すべてを入力、[必須情報](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)で、**全般** タブ。

   [![](existing-library-images/existing-metadata-sml.png "必要なメタデータを入力します。")](existing-library-images/existing-metadata.png#lightbox)

3. 必要に応じて、[メタデータを追加](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)で、**詳細**タブ。

4. メタデータを構成すると、プロジェクトを右クリックして選択**NuGet パッケージの作成**と **.nupkg** NuGet パッケージのファイルに保存されます、 **/bin/** フォルダー (デバッグまたはリリースでは、構成によって異なります)。

   ![](existing-library-images/create-nuget-package.png "NuGet パッケージの作成を右クリック メニューから選択します。")

5. NuGet パッケージを作成する_すべて_ビルドまたは配置に移動して、 **NuGet パッケージ > ビルド**セクションとティック**プロジェクトのビルド時に NuGet パッケージを作成**:

    [![](existing-library-images/existing-tickbox-sml.png "NuGet パッケージを作成するティック")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet のビルド パッケージは、ビルド プロセスを低下することができます。 このボックスがオンになっていない場合でも生成できます NuGet パッケージを手動で (上記の手順 4 に示すように) プロジェクトのコンテキスト メニューから、いつでも。

## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは、ZIP ファイルではもおり、生成されたパッケージの内部構造を検査することができます。

このスクリーン ショットは、1 つの PCL アセンブリのみが含まれる、PCL ベース NuGet – の内容を示しています。

![](existing-library-images/nuget-output.png "NuGet パッケージに含まれるファイル")


## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
