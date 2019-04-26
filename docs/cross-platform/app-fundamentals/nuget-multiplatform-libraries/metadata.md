---
title: NuGet メタデータの編集
description: このドキュメントでは、プロジェクトのオプションを使用して、マルチプラット フォーム ライブラリの NuGet メタデータを編集する方法について説明します。 必須およびオプションの両方のメタデータがについて説明します。
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266883"
---
# <a name="editing-nuget-metadata"></a>NuGet メタデータの編集

_プロジェクト オプションを使用して、マルチプラット フォーム ライブラリの NuGet メタデータの編集_

ライブラリ (PCL または .NET Standard、または新しい NuGet プロジェクトの種類) などのプロジェクトの種類が、 **NuGet パッケージ**セクション、**プロジェクト オプション**ウィンドウ。

**メタデータ**セクションで使用する値を構成する、 [ **.nuspec** NuGet パッケージのマニフェスト ファイル](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file)します。

## <a name="required-information"></a>必要な情報

**全般** タブには、NuGet パッケージを生成する入力する必要がある 4 つのフィールドが含まれています。

[![](metadata-images/metadata-general-sml.png "NuGet パッケージに必要なメタデータ ウィンドウ")](metadata-images/metadata-general.png#lightbox)

- **ID** – パッケージの識別子。 Nuget.org (または、パッケージを配布する任意の場所) 内で一意である必要があります。 この後に[ガイダンス](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)URL で有効な文字だけを使用して (スペースなしとほとんどの特殊文字を避けるため)。
- **バージョン**– で一貫性のあるバージョン番号を選択[NuGet のバージョン管理規則](https://docs.microsoft.com/nuget/create-packages/dependency-versions)します。
- **作成者**– 名のコンマ区切りリスト。
- **説明**– ユーザーがパッケージを選択するときに表示されるパッケージの機能の概要。

> [!NOTE]
> NuGet または他のユーザーに配布するための新しいバージョンを構築するときに、バージョン番号をインクリメントする注意してください。

詳細については、次を参照してください、[要素参照のために必要な](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements)詳細については、同様に、これらの手順の詳細な[一意のパッケージ識別子を選択し、バージョン番号を設定する](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)と。[パッケージの種類を設定](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type)します。

> [!IMPORTANT]
> このタブのすべてのフィールドを入力する必要があります。それ以外の場合、エラー メッセージが表示されます。_"プロジェクト メタデータがない NuGet のため、NuGet パッケージは作成されません。プロジェクト オプションのメタデータ セクションでは、NuGet パッケージのメタデータを指定することができます"_

## <a name="optional-metadata"></a>省略可能なメタデータ

**詳細** タブには、NuGet パッケージのマニフェスト ファイルに含まれる省略可能なフィールドが含まれています。

[![](metadata-images/metadata-detail-sml.png "NuGet パッケージの省略可能なメタデータ ウィンドウ")](metadata-images/metadata-detail.png#lightbox)

参照してください、[省略可能な要素のリファレンス](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements)詳細については、必須および省略可能なフィールド。

> [!NOTE]
> NuGet パッケージが分散される場合[NuGet.org](https://www.nuget.org)できるだけ多くの情報を指定することをお勧めします。


## <a name="related-links"></a>関連リンク

- [.nuspec 参照](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
