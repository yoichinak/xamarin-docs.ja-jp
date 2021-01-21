---
title: NuGet 用の新しいマルチプラットフォームライブラリの作成
description: このドキュメントでは、NuGet で使用するマルチプラットフォームライブラリを作成する方法について説明します。 この手法は、.NET 基底クラスライブラリで完全に表現できるビジネスロジックとアルゴリズムに適しているため、プラットフォーム固有のコードを使用せずにすべてのターゲットプラットフォームで実行されます。
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: ca874149874c420801c839fcde7afac11f46c5e1
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628918"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet 用の新しいマルチプラットフォームライブラリの作成

PCL または .NET Standard を使用するマルチプラットフォームライブラリプロジェクトを作成すると、結果として得られる NuGet は、ASP.NET プロジェクトや、WinForms、WPF、UWP を使用したデスクトップアプリなど、ターゲットプロファイルをサポートする任意の .NET プロジェクトに追加できます。

ライブラリには、選択した PCL または .NET Standard プロファイルでサポートされているコード、および追加されたその他の Nuget のみを含めることができます。
これは、.NET 基底クラスライブラリで完全に表現できるビジネスロジックとアルゴリズムに適しています。

1つのアセンブリが作成され、NuGet パッケージに組み込まれます。

後でプラットフォーム固有の機能が必要な場合は、 [プラットフォーム固有のプロジェクトを追加でき](#add-platforms)ます。

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>マルチプラットフォームライブラリ NuGet を作成する手順

1. [ **ファイル > 新しいソリューション** を選択します (または、既存のソリューションを右クリックし、[ **> 新しいプロジェクトの追加**] を選択します)。

2. [**マルチプラットフォーム > ライブラリ**] セクションで、[**マルチプラットフォームライブラリ**] を選択します。

   [![スクリーンショットでは、マルチプラットフォームライブラリが選択されているテンプレートを選択しています。](single-codebase-images/mulitplatform-library-sml.png)](single-codebase-images/mulitplatform-library.png#lightbox)

3. **名前** と **説明** を入力し、[**すべてのプラットフォーム] で [Single**] を選択します。

   [![[名前]、[説明]、および [実装] に入力した値がスクリーンショットに表示されます。](single-codebase-images/single-configure-sml.png)](single-codebase-images/single-configure.png#lightbox)

4. ウィザードを完了します。 ソリューション内に1つのライブラリプロジェクトが作成されます。

5. 新しいライブラリプロジェクトを右クリックし、[ **オプション**] を選択します。 **[ビルド > 全般**] セクションでは、**ターゲットフレームワーク** を設定できます。 .net ポータブル PCL プロファイルまたは .NET Standard バージョンを選択します。

   [![ライブラリの種類に PCL または .NET Standard を選択する](single-codebase-images/single-choose-type-sml.png)](single-codebase-images/single-choose-type.png#lightbox)

6. また、[ **プロジェクトオプション** ] ウィンドウで、[ **NuGet パッケージ > メタデータ** ] セクションを開き、 [必要なメタデータ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (および任意のメタデータ) を入力します。

   [![必須のメタデータを入力してください](single-codebase-images/single-metadata-sml.png)](single-codebase-images/single-metadata.png#lightbox)

7. ライブラリプロジェクトを右クリックし、[ **NuGet パッケージの作成** ] (またはソリューションのビルドまたは配置) を選択すると、 **. nupkg** NuGet パッケージファイルが **/ビン/** フォルダーに保存されます (構成によっては、デバッグまたはリリースのいずれか)。

   ![NuGet パッケージファイルは、構成に応じて、デバッグまたはリリースのいずれかの bin フォルダーに保存されます。](single-codebase-images/create-nuget-package.png)

## <a name="verifying-the-output"></a>出力を確認しています

NuGet パッケージは ZIP ファイルでもあるため、生成されたパッケージの内部構造を調べることができます。

このスクリーンショットは、PCL ベースの NuGet の内容を示しています。1つの PCL アセンブリだけが含まれています。

![NuGet パッケージに含まれるファイル](single-codebase-images/nuget-output.png)

<a name="add-platforms"></a>

## <a name="adding-platform-specific-code"></a>Platform-Specific コードの追加

PCL ベースのプロジェクトと .NET Standard ベースのプロジェクトには、プラットフォーム固有の参照 (iOS や Android の機能など) を含めることはできません。

既存の PCL プロジェクトまたは .NET Standard プロジェクトを展開してプラットフォーム固有のコードを含める必要がある場合は、プロジェクトを右クリックし、[ **追加] > [プラットフォームの実装の追加**] の順に選択します。

[![[プラットフォーム実装の追加] メニュー](single-codebase-images/add-later-sml.png)](single-codebase-images/add-later.png#lightbox)

ソリューションには1つまたは複数のプラットフォームプロジェクトを追加できます。また、既存の PCL または .NET Standard ライブラリは、必要に応じて共有プロジェクトに変換できます。

[![IOS、Android、共有プロジェクトなどのプラットフォームオプションを追加する](single-codebase-images/add-later-platforms-sml.png)](single-codebase-images/add-later-platforms-sml.png#lightbox)

共有プロジェクトに変換した後、[**プロジェクトオプション > nuget パッケージ > 参照アセンブリ** 
 [] セクション](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md)に移動し、必要なすべてのプロファイルが選択されていることを確認します (nuget は、以前に使用されていたプロジェクトと互換性があることを確認します)。

## <a name="related-links"></a>関連リンク

- [メタデータガイド](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
