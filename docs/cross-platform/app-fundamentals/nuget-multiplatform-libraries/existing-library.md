---
title: 既存のライブラリプロジェクトから NuGet を作成する
description: このドキュメントでは、既存のライブラリプロジェクトから NuGet パッケージを作成して、コードを他の開発者と共有できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: davidortinau
ms.author: daortin
ms.date: 04/20/2017
ms.openlocfilehash: 30158056b8adbdd5aba4cc311220a22502ea9968
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016786"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>既存のライブラリプロジェクトから NuGet を作成する

既存の PCL または .NET Standard ライブラリは、 **[プロジェクトオプション]** ウィンドウで nuget に切り替えることができます。

1. **Solution Pad**でライブラリプロジェクトを右クリックし、 **[オプション]** を選択します。

2. **[NuGet パッケージ > メタデータ]** セクションにアクセスし、[必要な](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)すべての情報を **[全般**] タブに入力します。

   [![](existing-library-images/existing-metadata-sml.png "Enter required metadata")](existing-library-images/existing-metadata.png#lightbox)

3. 必要に応じて、 **[詳細]** タブで[メタデータを追加](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)します。

4. メタデータを構成したら、プロジェクトを右クリックし、 **[Nuget パッケージの作成]** を選択し**ます。 nupkg** NuGet パッケージファイルは、構成に応じて、[デバッグ] または [リリース **] のいずれかのフォルダーに**保存されます。

   ![](existing-library-images/create-nuget-package.png "Choose Create NuGet Package from the right-click menu")

5. _すべて_のビルドまたは配置で nuget パッケージを作成するには、 **Nuget パッケージ > ビルド**セクションにアクセスし、プロジェクトをビルド**するときに nuget パッケージを作成**します。

    [![](existing-library-images/existing-tickbox-sml.png "Tick to create a NuGet package")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> NuGet パッケージをビルドすると、ビルドプロセスの速度が低下する可能性があります。 このボックスが表示されていない場合でも、プロジェクトのコンテキストメニュー (上記の手順 4. を参照) からいつでも手動で NuGet パッケージを生成できます。

## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは ZIP ファイルでもあるため、生成されたパッケージの内部構造を調べることができます。

このスクリーンショットは、PCL ベースの NuGet の内容を示しています。1つの PCL アセンブリだけが含まれています。

![](existing-library-images/nuget-output.png "Files contained in the NuGet package")

## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
