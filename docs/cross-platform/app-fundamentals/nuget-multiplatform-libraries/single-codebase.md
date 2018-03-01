---
title: "NuGet の新しいマルチプラット フォーム ライブラリを作成します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e95cf18c281732c85c2029e4ff35e8dd8be0f5e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet の新しいマルチプラット フォーム ライブラリを作成します。

PCL を使用する、マルチプラット フォーム ライブラリ プロジェクトを作成または .NET 標準結果 NuGet を ASP.NET プロジェクト、または WinForms、WPF では、または UWP を使用して、デスクトップ アプリを含め、ターゲット プロファイルをサポートする任意の .NET プロジェクトに追加できることを意味します。

ライブラリには、選択した PCL または標準的な .NET プロファイルと追加されるその他の NuGets でサポートされているコードのみを含めることができます。
これは、ビジネス ロジックと .NET の基本クラス ライブラリの完全に表現できるアルゴリズムに適しています。

1 つのアセンブリが作成され、NuGet パッケージに組み込まれています。

プラットフォーム固有の機能を後で必要がある場合[プラットフォーム固有のプロジェクトを追加できる](#add-platforms)です。

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>マルチプラット フォーム ライブラリ NuGet を作成する手順

1. 選択**ファイル > 新しいソリューション**(既存のソリューションを右クリックしを選択または**追加 > 新しいプロジェクト**)。

2. 選択**マルチプラット フォーム ライブラリ**から、**マルチプラット フォーム > ライブラリ**セクション。

  [ ![](single-codebase-images/mulitplatform-library-sml.png "1 つのコード ベースのマルチプラット フォーム ライブラリを構成します。")](single-codebase-images/mulitplatform-library.png)

3. 入力、**名前**と**説明**を選択して**すべてのプラットフォームの 1 つ**:

  [ ![](single-codebase-images/single-configure-sml.png "1 つのコード ベースのマルチプラット フォーム ライブラリを構成します。")](single-codebase-images/single-configure.png)

4. ウィザードを完了します。 1 つのライブラリ プロジェクトはソリューションに作成されます。

5. 新しいライブラリ プロジェクトを右クリックし、**オプション**です。 **ビルド > 全般**セクションでは、**ターゲット フレームワーク**– 設定する .NET ポータブル PCL プロファイルまたは .NET の Standard バージョンを選択します。

  [ ![](single-codebase-images/single-choose-type-sml.png "PCL または .NET 標準ライブラリの種類を選択します。")](single-codebase-images/single-choose-type.png)

6. また、**プロジェクト オプション**ウィンドウを開いた、 **NuGet パッケージ > メタデータ**セクションし、入力、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(および省略可能なメタデータ)。

  [ ![](single-codebase-images/single-metadata-sml.png "必要なメタデータを入力してください。")](single-codebase-images/single-metadata.png)

7. ライブラリ プロジェクトを右クリックし、選択**NuGet パッケージの作成**(またはビルドまたはソリューションを展開する) および**これは .nupkg** NuGet パッケージのファイルに保存されます、 **/bin/**フォルダー (デバッグまたはリリースでは、構成によっては):

  ![](single-codebase-images/create-nuget-package.png "NuGet パッケージのファイルが保存 bin フォルダーにデバッグまたはリリースでは、いずれかの構成によって")


## <a name="verifying-the-output"></a>出力を確認しています

生成されたパッケージの内部構造を検査することは、NuGet パッケージは ZIP ファイルではもです。

このスクリーン ショットは、1 つの PCL アセンブリのみが含まれる – PCL に基づく NuGet の内容を示しています。

![](single-codebase-images/nuget-output.png "NuGet パッケージに含まれているファイル")

<a name="add-platforms" />

# <a name="adding-platform-specific-code"></a>プラットフォーム固有のコードを追加します。

PCL ベースのプロジェクトと標準 .NET ベースのプロジェクトは、(iOS または Android の機能) などのプラットフォーム固有の参照を含めることはできません。

既存の PCL プロジェクトまたは .NET 標準プロジェクトは、展開すると、プラットフォーム固有のコードを追加する必要があります場合、これ行うプロジェクトを右クリックしを選択すると**追加 > プラットフォームの実装を追加しています.**:

[ ![](single-codebase-images/add-later-sml.png "プラットフォームの実装のメニューに追加します。")](single-codebase-images/add-later.png)

1 つまたは複数のプラットフォーム プロジェクトをソリューションに追加できるし、既存の PCL または標準の .NET ライブラリは、共有プロジェクトに必要に応じて変換できます。

[ ![](single-codebase-images/add-later-platforms-sml.png "IOS、Android、およびプロジェクトの共有などのプラットフォームのオプションを追加します。")](single-codebase-images/add-later-platforms-sml.png)

共有プロジェクトへの変換後に、次を参照してください、**プロジェクトのオプション > NuGet パッケージ > 参照アセンブリ**
[セクション](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md)いずれかの必要なプロファイルが選択されていることを確認してください (できるように、。NuGet は引き続き使用されていたプロジェクトと互換性がある)。


## <a name="related-links"></a>関連リンク

- [メタデータのガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
