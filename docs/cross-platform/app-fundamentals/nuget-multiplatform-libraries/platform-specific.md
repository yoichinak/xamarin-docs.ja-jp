---
title: NuGet 用の新しいプラットフォーム固有のライブラリプロジェクトを作成する
description: このドキュメントでは、複数のプラットフォームに対応したプラットフォーム固有のコードを含む単一の NuGet パッケージを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 925e08c600c695640c927ada26df376a252b3927
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016726"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet 用の新しいプラットフォーム固有のライブラリプロジェクトを作成する

IOS や Android など、特定のプラットフォームを対象とするマルチプラットフォームライブラリプロジェクトは、共有プロジェクトで最適に動作します。

NuGet には、iOS と Android 固有の両方のコードに加え、両方に共通の .NET コードを含めることができます。

複数のアセンブリが作成され、1つの NuGet パッケージに組み込まれます。 NuGet 標準では、Xamarin. iOS プロジェクトや Android プロジェクトなど、サポートされているすべてのプロジェクトの種類にパッケージを追加することができます。

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>クロスプラットフォームライブラリの NuGet を作成する手順

1. **ファイル > 新しいソリューション**を選択します (または、既存のソリューションを右クリックし、 **> 新しいプロジェクトの追加** を選択します)。

2. **[マルチプラットフォーム > ライブラリ]** セクションで、 **[マルチプラットフォームライブラリ]** を選択します。

    [![](platform-specific-images/mulitplatform-library-sml.png "Configure multi-platform library for a single code base")](platform-specific-images/multiplatform-library.png#lightbox)

3. **名前**と**説明**を入力し、 **[プラットフォーム固有]** を選択します。

    [![](platform-specific-images/specific-configure-sml.png "Configure platform-specific library for iOS and Android")](platform-specific-images/specific-configure.png#lightbox)

4. ウィザードを完了します。 ソリューションには、次のプロジェクトが追加されます。

    - **Android プロジェクト**– android 固有のコードは、必要に応じてこのプロジェクトに追加できます。
    - **Ios プロジェクト**– ios 固有のコードは、必要に応じてこのプロジェクトに追加できます。
    - **NuGet プロジェクト**-このプロジェクトにコードは追加されません。 他のプロジェクトを参照し、NuGet パッケージの出力のメタデータ構成を格納します。
    - **共有プロジェクト**-`#if` コンパイラディレクティブ内のプラットフォーム固有のコードを含め、このプロジェクトに共通コードを追加する必要があります。

5. NuGet プロジェクトを右クリックし、 **[オプション]** を選択します。次に、 **[Nuget パッケージ > メタデータ]** セクションを開き、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(およびオプションのメタデータ) を入力します。

    [![](platform-specific-images/specific-metadata-sml.png "Enter required metadata")](platform-specific-images/specific-metadata.png#lightbox)

6. また、 **[プロジェクトオプション]** ウィンドウで、 **[参照アセンブリ]** セクションを開き、共有ライブラリが "bait とスイッチ" を使用してサポートする PCL プロファイルを選択します。

    ![](platform-specific-images/specific-reference-assemblies.png "Also in the Project Options window, open the Reference Assemblies section and choose   which PCL profiles the shared library will support via bait and switch")

    > [!NOTE]
    > "Bait とスイッチ" とは、PCL アセンブリに、ライブラリによって公開されている API のみを含めることを意味します (プラットフォーム固有のコードを含めることはできません)。 NuGet を Xamarin プロジェクトに追加すると、PCL に対して共有ライブラリがコンパイルされますが、プラットフォーム固有のアセンブリには、iOS または Android プロジェクトによって実際に使用されるコードが含まれます。

7. プロジェクトを右クリックし、**NuGet パッケージの作成** (または ソリューションのビルドまたは配置) を選択し**ます。 nupkg** NuGet パッケージファイルは、構成に応じて、デバッグ または リリース **のいずれかのフォルダーに**保存されます。

    ![](platform-specific-images/create-nuget-package.png "NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration")

## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは ZIP ファイルでもあるため、生成されたパッケージの内部構造を調べることができます。

このスクリーンショットは、iOS と Android をサポートし、2つの参照アセンブリが選択されているプラットフォーム固有の NuGet の内容を示しています。

![](platform-specific-images/nuget-output.png "Files contained in the NuGet package")

## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
