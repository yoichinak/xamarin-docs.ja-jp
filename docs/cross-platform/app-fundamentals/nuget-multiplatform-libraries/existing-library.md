---
title: 既存のライブラリプロジェクトから NuGet を作成する
description: このドキュメントでは、既存のライブラリプロジェクトから NuGet パッケージを作成して、コードを他の開発者と共有できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: davidortinau
ms.author: daortin
ms.date: 04/20/2017
ms.openlocfilehash: 058904236e100a670e8a38417dcaec805178d6d2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936803"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>既存のライブラリプロジェクトから NuGet を作成する

既存の PCL または .NET Standard ライブラリは、[**プロジェクトオプション**] ウィンドウで nuget に切り替えることができます。

1. **Solution Pad**でライブラリプロジェクトを右クリックし、[**オプション**] を選択します。

2. [ **NuGet パッケージ > メタデータ**] セクションにアクセスし、[必要な](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)すべての情報を **[全般**] タブに入力します。

   [![必須のメタデータを入力してください](existing-library-images/existing-metadata-sml.png)](existing-library-images/existing-metadata.png#lightbox)

3. 必要に応じて、[**詳細**] タブで[メタデータを追加](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)します。

4. メタデータを構成したら、プロジェクトを右クリックし、[ **Nuget パッケージの作成**] を選択し**ます。 nupkg** NuGet パッケージファイルは、構成に応じて、[デバッグ] または [リリース **] のいずれかのフォルダーに**保存されます。

   ![右クリックメニューから [NuGet パッケージの作成] を選択します。](existing-library-images/create-nuget-package.png)

5. _すべて_のビルドまたは配置で nuget パッケージを作成するには、 **Nuget パッケージ > ビルド**セクションにアクセスし、プロジェクトをビルド**するときに nuget パッケージを作成**します。

    [![NuGet パッケージを作成するためのティック](existing-library-images/existing-tickbox-sml.png)](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet パッケージをビルドすると、ビルドプロセスの速度が低下する可能性があります。 このボックスが表示されていない場合でも、プロジェクトのコンテキストメニュー (上記の手順 4. を参照) からいつでも手動で NuGet パッケージを生成できます。

## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは ZIP ファイルでもあるため、生成されたパッケージの内部構造を調べることができます。

このスクリーンショットは、PCL ベースの NuGet の内容を示しています。1つの PCL アセンブリだけが含まれています。

![NuGet パッケージに含まれるファイル](existing-library-images/nuget-output.png)

## <a name="related-links"></a>関連リンク

- [メタデータガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
