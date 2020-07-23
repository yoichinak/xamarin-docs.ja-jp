---
title: NuGet メタデータの編集
description: このドキュメントでは、プロジェクトオプションを使用して、マルチプラットフォームライブラリの NuGet メタデータを編集する方法について説明します。 必須のメタデータと省略可能なメタデータの両方について説明します。
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 858d2a2399e1d294767b8afad36502b809955224
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933280"
---
# <a name="editing-nuget-metadata"></a>NuGet メタデータの編集

_プロジェクトオプションを使用して、マルチプラットフォームライブラリの NuGet メタデータを編集する_

ライブラリプロジェクトの種類 (PCL、.NET Standard、新しい NuGet プロジェクトの種類など) には、[**プロジェクトオプション**] ウィンドウに [ **nuget パッケージ**] セクションがあります。

**メタデータ**セクションでは、 [ **nuspec** NuGet パッケージマニフェストファイル](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)で使用される値を構成します。

## <a name="required-information"></a>必要な情報

**[全般**] タブには、NuGet パッケージを生成するために入力する必要がある4つのフィールドがあります。

[![NuGet パッケージに必要なメタデータウィンドウ](metadata-images/metadata-general-sml.png)](metadata-images/metadata-general.png#lightbox)

- **ID** –パッケージ識別子。 NuGet.org (またはパッケージが配布されるすべての場所) 内で一意である必要があります。 この[ガイダンス](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)に従って、URL で有効な文字のみを使用します (スペースは不要で、ほとんどの特殊文字は使用しないでください)。
- [**バージョン**] – [NuGet のバージョン管理ルール](https://docs.microsoft.com/nuget/create-packages/dependency-versions)と一致するバージョン番号を選択します。
- **作成者**–名前のコンマ区切りのリスト。
- **説明**–パッケージを選択しているときに表示される、パッケージの機能の概要です。

> [!NOTE]
> NuGet または他のユーザーに配布するための新しいバージョンをビルドする場合は、バージョン番号を必ず増やしてください。

詳細については、「[必須要素リファレンス](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements)」を参照してください。[また、一意のパッケージ識別子を選択し、バージョン番号を設定](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)し、[パッケージの種類を設定する](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type)詳細な手順についても説明します。

> [!IMPORTANT]
> このタブのすべてのフィールドを入力する必要があります。それ以外の場合は、 _"プロジェクトに nuget メタデータが含まれていないため、nuget パッケージは作成されません。" というエラーメッセージが表示されます。NuGet パッケージのメタデータは、[プロジェクトオプション] の [メタデータ] セクションで指定できます_。

## <a name="optional-metadata"></a>省略可能なメタデータ

[**詳細**] タブには、NuGet パッケージマニフェストファイルに含めるオプションのフィールドが表示されます。

[![NuGet パッケージのオプションのメタデータウィンドウ](metadata-images/metadata-detail-sml.png)](metadata-images/metadata-detail.png#lightbox)

必須フィールドとオプションフィールドの詳細については、[省略可能な要素のリファレンス](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements)を参照してください。

> [!NOTE]
> NuGet パッケージが[NuGet.org](https://www.nuget.org)に配布されている場合は、できるだけ多くの情報を提供することをお勧めします。

## <a name="related-links"></a>関連リンク

- [. nuspec リファレンス](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
