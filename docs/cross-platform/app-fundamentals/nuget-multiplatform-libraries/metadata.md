---
title: "NuGet のメタデータを編集"
description: "プロジェクト オプションを使用して、マルチプラット フォーム ライブラリの NuGet メタデータの編集"
ms.topic: article
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8afce6021c2816f354e26ccecd7d0c40ceb2a9bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="editing-nuget-metadata"></a>NuGet のメタデータを編集

_プロジェクト オプションを使用して、マルチプラット フォーム ライブラリの NuGet メタデータの編集_

ライブラリ (PCL または .NET Standard、または新しい NuGet プロジェクトの種類) などのプロジェクトの種類が、 **NuGet パッケージ**」の「、**プロジェクト オプション**ウィンドウです。

**メタデータ**セクションで使用する値を構成する、 [ **.nuspec** NuGet パッケージのマニフェスト ファイル](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)です。

## <a name="required-information"></a>必要な情報

**全般** タブには、NuGet パッケージを生成する入力する必要がある 4 つのフィールドが含まれています。

[ ![](metadata-images/metadata-general-sml.png "NuGet パッケージの必要なメタデータ ウィンドウ")](metadata-images/metadata-general.png)

- **ID** – パッケージの識別子、Nuget.org (または、パッケージを配布する任意の場所) 内で一意である必要があります。 この後に[ガイダンス](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)のみ URL に無効な文字が使用する (スペースなしは、ほとんどの特殊文字を回避してください)。
- **バージョン**– で一貫性のあるバージョン番号を選択[NuGet のバージョン管理規則](https://docs.microsoft.com/en-us/nuget/create-packages/dependency-versions)です。
- **作成者**– 名のコンマ区切りの一覧です。
- **説明**– ユーザーがパッケージを選択するときに表示されるパッケージの機能の概要です。

> [!NOTE]
> NuGet または他のユーザーに配布するための新しいバージョンを作成するときに、バージョン番号をインクリメントしてください。

詳細については、次を参照してください、[要素参照のために必要な](https://docs.microsoft.com/en-us/nuget/schema/nuspec#required-metadata-elements)詳細については、同様に、これらの手順の詳細な[一意のパッケージ識別子を選択すると、バージョン番号を設定](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)と。[パッケージの種類を設定](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package#setting-a-package-type)です。

> [!IMPORTANT]
> このタブのすべてのフィールドを入力する必要があります。それ以外の場合、エラー メッセージが表示されます: _"プロジェクト メタデータがない NuGet のため、NuGet パッケージは作成されません。NuGet パッケージのメタデータは、プロジェクトのオプションのメタデータで指定することができます"_

## <a name="optional-metadata"></a>省略可能なメタデータ

**詳細** タブには、NuGet パッケージのマニフェスト ファイルに含まれる省略可能なフィールドが含まれています。

[ ![](metadata-images/metadata-detail-sml.png "NuGet パッケージの省略可能なメタデータ ウィンドウ")](metadata-images/metadata-detail.png)

参照してください、[省略可能な要素のリファレンス](https://docs.microsoft.com/en-us/nuget/schema/nuspec#optional-metadata-elements)詳細については、必須および省略可能なフィールドです。

> [!NOTE]
> NuGet パッケージが配布されている場合[NuGet.org](https://www.nuget.org)できるだけ多くの情報を指定することをお勧めします。


## <a name="related-links"></a>関連リンク

- [.nuspec 参照](https://docs.microsoft.com/en-us/nuget/schema/nuspec#general-form-and-schema)
