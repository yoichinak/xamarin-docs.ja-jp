---
title: 既存のライブラリ プロジェクトから NuGet の作成
description: このドキュメントでは、コードを他の開発者と共有できるように、既存のライブラリ プロジェクトから NuGet パッケージを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779984"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>既存のライブラリ プロジェクトから NuGet の作成

既存の PCL または標準の .NET ライブラリをを介して NuGets に変換できる、**プロジェクト オプション**ウィンドウ。

1. ライブラリ プロジェクトを右クリックし、**ソリューション パッド**選択**オプション**です。

2. 移動して、 **NuGet パッケージ > メタデータ**セクションし、すべてを入力、[必要情報](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)で、**全般** タブ。

  [![](existing-library-images/existing-metadata-sml.png "必要なメタデータを入力してください。")](existing-library-images/existing-metadata.png#lightbox)

3. 必要に応じて、[メタデータを追加](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)で、**詳細**タブです。

4. メタデータが構成されると、プロジェクトを右クリックしてを選択する**NuGet パッケージの作成**と**これは .nupkg** NuGet パッケージのファイルに保存されますが、 **/bin/** フォルダー (デバッグまたはリリースでは、構成によって異なります)。

  ![](existing-library-images/create-nuget-package.png "NuGet パッケージの作成を右クリック メニューから選択します。")

5. NuGet パッケージを作成する_すべて_ビルドまたは配置に移動して、 **NuGet パッケージ > ビルド**セクションおよび目盛り**プロジェクトのビルド時に NuGet パッケージを作成する**:

    [![](existing-library-images/existing-tickbox-sml.png "NuGet パッケージを作成するティック")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet の構築 パッケージ ビルド処理速度が低下します。 場合は、このボックスは、チェック マークがない、することができますも NuGet パッケージを手動で生成 (上記の手順 4 で表示) プロジェクトのコンテキスト メニューからいつでもです。

## <a name="verifying-the-output"></a>出力を確認しています

生成されたパッケージの内部構造を検査することは、NuGet パッケージは ZIP ファイルではもです。

このスクリーン ショットは、1 つの PCL アセンブリのみが含まれる – PCL に基づく NuGet の内容を示しています。

![](existing-library-images/nuget-output.png "NuGet パッケージに含まれているファイル")


## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
