---
title: NuGet 用の新しいプラットフォーム固有のライブラリプロジェクトを作成する
description: このドキュメントでは、複数のプラットフォームに対応したプラットフォーム固有のコードを含む単一の NuGet パッケージを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: d3f756b1a551c7b6bcbe48129235d537312edff6
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282148"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet 用の新しいプラットフォーム固有のライブラリプロジェクトを作成する

IOS や Android など、特定のプラットフォームを対象とするマルチプラットフォームライブラリプロジェクトは、共有プロジェクトで最適に動作します。

NuGet には、iOS と Android 固有の両方のコードに加え、両方に共通の .NET コードを含めることができます。

複数のアセンブリが作成され、1つの NuGet パッケージに組み込まれます。 NuGet 標準では、Xamarin. iOS プロジェクトや Android プロジェクトなど、サポートされているすべてのプロジェクトの種類にパッケージを追加することができます。

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>クロスプラットフォームライブラリの NuGet を作成する手順

1. **ファイル > 新しいソリューション**を選択します (または、既存のソリューションを右クリックし、 **> 新しいプロジェクトの追加** を選択します)。

2. **[マルチプラットフォーム > ライブラリ]** セクションで、 **[マルチプラットフォームライブラリ]** を選択します。

    [![](platform-specific-images/mulitplatform-library-sml.png "1つのコードベースに対してマルチプラットフォームライブラリを構成する")](platform-specific-images/multiplatform-library.png#lightbox)

3. **名前**と**説明**を入力し、 **[プラットフォーム固有]** を選択します。

    [![](platform-specific-images/specific-configure-sml.png "IOS および Android 用のプラットフォーム固有のライブラリを構成する")](platform-specific-images/specific-configure.png#lightbox)

4. ウィザードを完了します。 ソリューションには、次のプロジェクトが追加されます。

    - **Android プロジェクト**– android 固有のコードは、必要に応じてこのプロジェクトに追加できます。
    - **Ios プロジェクト**– ios 固有のコードは、必要に応じてこのプロジェクトに追加できます。
    - **NuGet プロジェクト**-このプロジェクトにコードは追加されません。 他のプロジェクトを参照し、NuGet パッケージの出力のメタデータ構成を格納します。
    - **共有プロジェクト**–コンパイラディレクティブ内`#if`のプラットフォーム固有のコードを含め、このプロジェクトに共通コードを追加する必要があります。

5. NuGet プロジェクトを右クリックし、 **[オプション]** を選択します。次に、 **[Nuget パッケージ > メタデータ]** セクションを開き、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(およびオプションのメタデータ) を入力します。

    [![](platform-specific-images/specific-metadata-sml.png "必須のメタデータを入力してください")](platform-specific-images/specific-metadata.png#lightbox)

6. また、 **[プロジェクトオプション]** ウィンドウで、 **[参照アセンブリ]** セクションを開き、共有ライブラリが "bait とスイッチ" を使用してサポートする PCL プロファイルを選択します。

    ![](platform-specific-images/specific-reference-assemblies.png "また、[プロジェクトオプション] ウィンドウで、[参照アセンブリ] セクションを開き、共有ライブラリが bait とスイッチによってサポートする PCL プロファイルを選択します。")

    > [!NOTE]
    > "Bait とスイッチ" とは、PCL アセンブリに、ライブラリによって公開されている API のみを含めることを意味します (プラットフォーム固有のコードを含めることはできません)。 NuGet を Xamarin プロジェクトに追加すると、PCL に対して共有ライブラリがコンパイルされますが、プラットフォーム固有のアセンブリには、iOS または Android プロジェクトによって実際に使用されるコードが含まれます。

7. プロジェクトを右クリックし、**NuGet パッケージの作成** (または ソリューションのビルドまたは配置) を選択し**ます。 nupkg** NuGet パッケージファイルは、構成に応じて、デバッグ または リリース **のいずれかのフォルダーに**保存されます。

    ![](platform-specific-images/create-nuget-package.png "NuGet パッケージファイルは、構成に応じて、デバッグまたはリリースのいずれかの bin フォルダーに保存されます。")


## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは ZIP ファイルでもあるため、生成されたパッケージの内部構造を調べることができます。

このスクリーンショットは、iOS と Android をサポートし、2つの参照アセンブリが選択されているプラットフォーム固有の NuGet の内容を示しています。

![](platform-specific-images/nuget-output.png "NuGet パッケージに含まれるファイル")


## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
