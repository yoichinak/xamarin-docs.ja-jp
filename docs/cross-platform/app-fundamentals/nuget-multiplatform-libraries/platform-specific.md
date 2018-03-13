---
title: "NuGet の新しいプラットフォーム固有のライブラリ プロジェクトを作成します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6512387738217259067e7b9ae8076f73b4fbeb07
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet の新しいプラットフォーム固有のライブラリ プロジェクトを作成します。

IOS や Android などの特定のプラットフォームを対象とするマルチプラット フォームのライブラリ プロジェクトは、共有プロジェクトに最適です。

NuGet は、.NET コードの両方に共通するだけでなく、iOS および Android に固有のコードを含めることができます。

複数のアセンブリが作成され、1 つの NuGet パッケージに組み込まれています。 NuGet の標準では、Xamarin iOS や Android プロジェクトなど、サポートされているプロジェクトのすべての種類にパッケージを追加できることを確認してください。

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>クロスプラット フォーム ライブラリ NuGet を作成する手順

1. 選択**ファイル > 新しいソリューション**(既存のソリューションを右クリックしを選択または**追加 > 新しいプロジェクト**)。

2. 選択**マルチプラット フォーム ライブラリ**から、**マルチプラット フォーム > ライブラリ**セクション。

  [![](platform-specific-images/mulitplatform-library-sml.png "1 つのコード ベースのマルチプラット フォーム ライブラリを構成します。")](platform-specific-images/multiplatform-library.png#lightbox)

3. 入力、**名前**と**説明**を選択して**プラットフォーム固有**:

  [![](platform-specific-images/specific-configure-sml.png "IOS および Android 用のプラットフォーム固有のライブラリを構成します。")](platform-specific-images/specific-configure.png#lightbox)

4. ウィザードを完了します。 次のプロジェクトがソリューションに追加されます。

  - **Android プロジェクト**– Android 固有のコードは、このプロジェクトに必要に応じて追加できます。
  - **iOS プロジェクト**– iOS に固有のコードは、このプロジェクトに必要に応じて追加できます。
  - **NuGet プロジェクト**– コードがこのプロジェクトに追加されません。 他のプロジェクトを参照し、NuGet パッケージの出力のメタデータの構成が含まれています。
  - **共有プロジェクト**– 一般的なコードは、内のプラットフォーム固有のコードを含む、このプロジェクトに追加する必要があります`#if`コンパイラ ディレクティブです。

5. NuGet のプロジェクトを右クリックし、選択**オプション**を開き、 **NuGet パッケージ > メタデータ**セクションし、入力、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(としてその適切に省略可能なメタデータ):

  [![](platform-specific-images/specific-metadata-sml.png "必要なメタデータを入力してください。")](platform-specific-images/specific-metadata.png#lightbox)

6. また、**プロジェクト オプション**ウィンドウを開いた、**参照アセンブリ**セクションし、共有ライブラリは、「お連絡とり」を通じてサポートするために PCL をプロファイルの選択。

  ![](platform-specific-images/specific-reference-assemblies.png "また、プロジェクトのオプション ウィンドウで、参照アセンブリ セクションを開きおとりを通じて共有ライブラリがサポートするために PCL をプロファイル")

  > [!NOTE]
> 「お連絡とり」では、PCL アセンブリが (プラットフォーム固有のコードを含めることはできません) ライブラリによって公開される API にしか含めることを意味します。 NuGet は、Xamarin のプロジェクトに追加するときに、共有ライブラリは、PCL に対してコンパイルされますが、プラットフォーム固有のアセンブリには、実際には、iOS または Android プロジェクトで使用されるコードが含まれています。

7. プロジェクトを右クリックし、選択**NuGet パッケージの作成**(またはビルドまたはソリューションを展開する) および**これは .nupkg** NuGet パッケージのファイルに保存されます、 **/bin/**フォルダー (Debug または Release、構成によって)。

  ![](platform-specific-images/create-nuget-package.png "NuGet パッケージ ファイルは保存されます bin フォルダーにデバッグまたはリリースでは、いずれかの構成によって")


## <a name="verifying-the-output"></a>出力を確認しています

生成されたパッケージの内部構造を検査することは、NuGet パッケージは ZIP ファイルではもです。

このスクリーン ショットでは iOS と Android をサポートし、2 つの参照アセンブリが存在するプラットフォーム固有の NuGet の内容を選択します。

![](platform-specific-images/nuget-output.png "NuGet パッケージに含まれているファイル")


## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
