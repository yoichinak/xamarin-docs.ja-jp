---
title: NuGet 用の新しいプラットフォーム固有のライブラリプロジェクトを作成する
description: このドキュメントでは、複数のプラットフォームに対応したプラットフォーム固有のコードを含む単一の NuGet パッケージを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: b1c32d76f4f25298a8f0643e2e29707df7775736
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939242"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet 用の新しいプラットフォーム固有のライブラリプロジェクトを作成する

IOS や Android など、特定のプラットフォームを対象とするマルチプラットフォームライブラリプロジェクトは、共有プロジェクトで最適に動作します。

NuGet には、iOS と Android 固有の両方のコードに加え、両方に共通の .NET コードを含めることができます。

複数のアセンブリが作成され、1つの NuGet パッケージに組み込まれます。 NuGet 標準では、Xamarin. iOS プロジェクトや Android プロジェクトなど、サポートされているすべてのプロジェクトの種類にパッケージを追加することができます。

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>クロスプラットフォームライブラリの NuGet を作成する手順

1. [**ファイル > 新しいソリューション**を選択します (または、既存のソリューションを右クリックし、[ **> 新しいプロジェクトの追加**] を選択します)。

2. [**マルチプラットフォーム > ライブラリ**] セクションで、[**マルチプラットフォームライブラリ**] を選択します。

    [![1つのコードベースに対してマルチプラットフォームライブラリを構成する](platform-specific-images/mulitplatform-library-sml.png)](platform-specific-images/multiplatform-library.png#lightbox)

3. **名前**と**説明**を入力し、[**プラットフォーム固有**] を選択します。

    [![IOS および Android 用のプラットフォーム固有のライブラリを構成する](platform-specific-images/specific-configure-sml.png)](platform-specific-images/specific-configure.png#lightbox)

4. ウィザードを完了します。 ソリューションには、次のプロジェクトが追加されます。

    - **Android プロジェクト**– android 固有のコードは、必要に応じてこのプロジェクトに追加できます。
    - **Ios プロジェクト**– ios 固有のコードは、必要に応じてこのプロジェクトに追加できます。
    - **NuGet プロジェクト**-このプロジェクトにコードは追加されません。 他のプロジェクトを参照し、NuGet パッケージの出力のメタデータ構成を格納します。
    - **共有プロジェクト**–コンパイラディレクティブ内のプラットフォーム固有のコードを含め、このプロジェクトに共通コードを追加する必要があり `#if` ます。

5. NuGet プロジェクトを右クリックし、[**オプション**] を選択します。次に、[ **Nuget パッケージ > メタデータ**] セクションを開き、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(およびオプションのメタデータ) を入力します。

    [![必須のメタデータを入力してください](platform-specific-images/specific-metadata-sml.png)](platform-specific-images/specific-metadata.png#lightbox)

6. また、[**プロジェクトオプション**] ウィンドウで、[**参照アセンブリ**] セクションを開き、共有ライブラリが "bait とスイッチ" を使用してサポートする PCL プロファイルを選択します。

    ![また、[プロジェクトオプション] ウィンドウで、[参照アセンブリ] セクションを開き、共有ライブラリが bait とスイッチによってサポートする PCL プロファイルを選択します。](platform-specific-images/specific-reference-assemblies.png)

    > [!NOTE]
    > "Bait とスイッチ" とは、PCL アセンブリに、ライブラリによって公開されている API のみを含めることを意味します (プラットフォーム固有のコードを含めることはできません)。 NuGet を Xamarin プロジェクトに追加すると、PCL に対して共有ライブラリがコンパイルされますが、プラットフォーム固有のアセンブリには、iOS または Android プロジェクトによって実際に使用されるコードが含まれます。

7. プロジェクトを右クリックし、[ **NuGet パッケージの作成**] (または [ソリューションのビルドまたは配置]) を選択し**ます。 nupkg** NuGet パッケージファイルは、構成に応じて、[デバッグ] または [リリース **] のいずれかのフォルダーに**保存されます。

    ![NuGet パッケージファイルは、構成に応じて、デバッグまたはリリースのいずれかの bin フォルダーに保存されます。](platform-specific-images/create-nuget-package.png)

## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは ZIP ファイルでもあるため、生成されたパッケージの内部構造を調べることができます。

このスクリーンショットは、iOS と Android をサポートし、2つの参照アセンブリが選択されているプラットフォーム固有の NuGet の内容を示しています。

![NuGet パッケージに含まれるファイル](platform-specific-images/nuget-output.png)

## <a name="related-links"></a>関連リンク

- [メタデータガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
