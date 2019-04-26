---
title: NuGet の新しいプラットフォームに固有のライブラリ プロジェクトを作成します。
description: このドキュメントでは、複数のプラットフォームのプラットフォームに固有のコードを含む 1 つの NuGet パッケージを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 00a02973d6016ad63e4317279515acc2b4e2e81b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61267243"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet の新しいプラットフォームに固有のライブラリ プロジェクトを作成します。

IOS や Android などの特定のプラットフォームを対象とするマルチプラット フォーム ライブラリ プロジェクトは、共有プロジェクトに最適です。

NuGet は、iOS と Android 固有のコードと .NET コードの両方に共通の両方に含めることができます。

複数のアセンブリが作成され、1 つの NuGet パッケージに組み込まれています。 NuGet の標準では、Xamarin.iOS と Android のプロジェクトなど、あらゆる型のサポートされているプロジェクトにパッケージを追加できることを確認します。

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>クロス プラットフォーム ライブラリの NuGet を作成する手順

1. 選択**ファイル > 新しいソリューション**(または既存のソリューションを右クリックし、選択**追加 > 新しいプロジェクト**)。

2. 選択**マルチプラット フォーム ライブラリ**から、**マルチプラット フォーム > ライブラリ**セクション。

  [![](platform-specific-images/mulitplatform-library-sml.png "1 つのコード ベースのマルチプラット フォーム ライブラリを構成します。")](platform-specific-images/multiplatform-library.png#lightbox)

3. 入力、**名前**と**説明**、選択**プラットフォーム固有**:

  [![](platform-specific-images/specific-configure-sml.png "IOS および Android 用のプラットフォームに固有のライブラリを構成します。")](platform-specific-images/specific-configure.png#lightbox)

4. ウィザードを完了します。 次のプロジェクトは、ソリューションに追加されます。

  - **Android プロジェクト**– Android 固有のコードは、このプロジェクトに必要に応じて追加できます。
  - **iOS プロジェクト**– iOS 固有のコードは、このプロジェクトに必要に応じて追加できます。
  - **NuGet プロジェクト**– コードはこのプロジェクトに追加されません。 他のプロジェクトを参照し、NuGet パッケージの出力のメタデータの構成が含まれています。
  - **共有プロジェクト**– 一般的なコードは、内部でプラットフォーム固有のコードを含む、このプロジェクトに追加する必要があります`#if`コンパイラ ディレクティブ。

5. NuGet プロジェクトを右クリックし、選択**オプション**を開き、 **NuGet パッケージ > メタデータ**セクションし、入力、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(省略可能なあらゆるとしてメタデータ):

  [![](platform-specific-images/specific-metadata-sml.png "必要なメタデータを入力します。")](platform-specific-images/specific-metadata.png#lightbox)

6. さらに、**プロジェクト オプション**ウィンドウを開いて、**参照アセンブリ**セクションし、「おとり」を使用して、共有ライブラリがサポートされる PCL プロファイルの選択。

  ![](platform-specific-images/specific-reference-assemblies.png "プロジェクト オプション ウィンドウで、参照アセンブリのセクションを開いてまた bait and スイッチを使用して、共有ライブラリがサポートされる PCL プロファイルを選択")

  > [!NOTE]
> 「おとり」では、PCL アセンブリでは (プラットフォーム固有のコードを含めることはできません)、ライブラリによって公開される API だけを格納することを意味します。 Xamarin プロジェクトに NuGet を追加するときに共有ライブラリは、PCL に対してコンパイルされますが、プラットフォーム固有のアセンブリには、iOS または Android プロジェクトで実際に使用されるコードが含まれます。

7. プロジェクトを右クリックし、選択**NuGet パッケージの作成**(またはビルドまたはソリューションを展開する) および **.nupkg** NuGet パッケージのファイルに保存されます、 **/bin/** フォルダー (デバッグまたはリリースでは、構成に応じて)。

  ![](platform-specific-images/create-nuget-package.png "NuGet パッケージ ファイルは保存されます bin フォルダーにデバッグまたはリリースでは、構成に応じて")


## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは、ZIP ファイルではもおり、生成されたパッケージの内部構造を検査することができます。

このスクリーン ショットでは iOS と Android をサポートし、2 つの参照アセンブリがプラットフォーム固有の NuGet の内容を選択します。

![](platform-specific-images/nuget-output.png "NuGet パッケージに含まれるファイル")


## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
