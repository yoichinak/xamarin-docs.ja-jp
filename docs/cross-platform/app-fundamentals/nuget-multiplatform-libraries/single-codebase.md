---
title: NuGet の新しいマルチプラット フォーム ライブラリを作成します。
description: このドキュメントでは、NuGet を使用するためのマルチプラット フォーム ライブラリを作成する方法について説明します。 この手法は、ビジネス ロジックと、.NET 基本クラス ライブラリ全体で表すことができますし、プラットフォーム固有のコードを使用せずにすべてのターゲット プラットフォームで実行するためのアルゴリズムに適しています。
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6d695df9c59a5f95441092d6d7b44d5feda941bd
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172055"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet の新しいマルチプラット フォーム ライブラリを作成します。

PCL を使用するマルチプラット フォーム ライブラリ プロジェクトを作成または .NET Standard 結果として得られる NuGet ターゲット プロファイルは、asp.net、WinForms、WPF、UWP を使用してデスクトップ アプリなどをサポートする任意の .NET プロジェクトに追加できることを意味します。

ライブラリには、選択した PCL または .NET Standard プロファイルと追加されるその他の Nuget でサポートされているコードのみを含めることができます。
これは、ビジネス ロジックと .NET の基本クラス ライブラリの完全に表現できるアルゴリズムに適しています。

1 つのアセンブリが作成され、NuGet パッケージに組み込まれています。

プラットフォーム固有の機能を後で必要な場合[プラットフォームに固有のプロジェクトを追加できる](#add-platforms)します。

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>マルチプラット フォーム ライブラリの NuGet を作成する手順

1. 選択**ファイル > 新しいソリューション**(または既存のソリューションを右クリックし、選択**追加 > 新しいプロジェクト**)。

2. 選択**マルチプラット フォーム ライブラリ**から、**マルチプラット フォーム > ライブラリ**セクション。

  [![](single-codebase-images/mulitplatform-library-sml.png "1 つのコード ベースのマルチプラット フォーム ライブラリを構成します。")](single-codebase-images/mulitplatform-library.png#lightbox)

3. 入力、**名前**と**説明**、選択**すべてのプラットフォームの 1 つ**:

  [![](single-codebase-images/single-configure-sml.png "1 つのコード ベースのマルチプラット フォーム ライブラリを構成します。")](single-codebase-images/single-configure.png#lightbox)

4. ウィザードを完了します。 1 つのライブラリ プロジェクトは、ソリューションに作成されます。

5. 新しいライブラリ プロジェクトを右クリックし、**オプション**します。 **ビルド > 全般**セクションでは、**ターゲット フレームワーク**設定 – .NET ポータブル PCL プロファイルまたは .NET Standard バージョンを選択。

  [![](single-codebase-images/single-choose-type-sml.png "PCL または .NET Standard ライブラリの種類を選択します。")](single-codebase-images/single-choose-type.png#lightbox)

6. また、**プロジェクト オプション**ウィンドウを開いて、 **NuGet パッケージ > メタデータ**セクションし、入力、[必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)(省略可能なメタデータ) と同様に。

  [![](single-codebase-images/single-metadata-sml.png "必要なメタデータを入力します。")](single-codebase-images/single-metadata.png#lightbox)

7. ライブラリ プロジェクトを右クリックし、選択**NuGet パッケージの作成**(またはビルドまたはソリューションを展開する) および **.nupkg** NuGet パッケージのファイルに保存されます、 **/bin/** フォルダー (デバッグまたはリリースでは、構成に応じて):

  ![](single-codebase-images/create-nuget-package.png "NuGet パッケージのファイルが保存 bin フォルダーにデバッグまたはリリースでは、構成に応じて")


## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは、ZIP ファイルではもおり、生成されたパッケージの内部構造を検査することができます。

このスクリーン ショットは、1 つの PCL アセンブリのみが含まれる、PCL ベース NuGet – の内容を示しています。

![](single-codebase-images/nuget-output.png "NuGet パッケージに含まれるファイル")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>プラットフォーム固有のコードを追加します。

PCL ベースのプロジェクトと .NET Standard ベースのプロジェクト (iOS、Android の機能) などのプラットフォームに固有の参照を含めることはできません。

プロジェクトを右クリックして完了場合、既存の PCL プロジェクトまたは .NET Standard プロジェクトを展開すると、プラットフォーム固有のコードを含める必要がある、この**追加 > プラットフォームの実装を追加しています.**:

[![](single-codebase-images/add-later-sml.png "プラットフォームの実装 メニューを追加します。")](single-codebase-images/add-later.png#lightbox)

1 つまたは複数のプラットフォーム プロジェクトをソリューションに追加できるし、既存の PCL または .NET Standard ライブラリは、共有プロジェクトに必要に応じて変換できます。

[![](single-codebase-images/add-later-platforms-sml.png "IOS、Android、および共有プロジェクトなどのプラットフォーム オプションを追加します。")](single-codebase-images/add-later-platforms-sml.png#lightbox)

共有プロジェクトに変換してから、次を参照してください、**プロジェクト オプション > NuGet パッケージ > 参照アセンブリ**
[セクション](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md)をいずれかの必要なプロファイルが選択されていることを確認してください (ように、。NuGet は引き続き使用されていたプロジェクトと互換性がある)。


## <a name="related-links"></a>関連リンク

- [メタデータ ガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
