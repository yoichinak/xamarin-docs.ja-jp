---
title: NuGet パッケージをダウングレードする方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 0c70859845915a821bb83b0f9d29528634b1a5de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013611"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet パッケージをダウングレードする方法を教えてください

Visual Studio for Mac & Visual Studio には、古いバージョンのパッケージを選択して自動的にインストールする機能があります。更新パッケージの動作と同様です。 これらの手順については、以下で説明します。

## <a name="visual-studio"></a>Visual Studio

1. [**ツール] > [NuGet パッケージマネージャー] > [パッケージマネージャーコンソール**] にアクセス
2. **[既定のプロジェクト]** でプロジェクトを設定する
3. 次の構文を使用します。

    > インストールパッケージ [PackageName]-バージョン [バージョン] メニューのタブ]

また、パッケージの NuGet ページから正確なコマンドをコピーして貼り付けることもできます。 Xamarin. Forms の例: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. プロジェクトで、パッケージ フォルダーを右クリックし & **パッケージの追加** を選択します。
2. Searchbar では、次の構文を使用して、必要なパッケージを検索できます。

    `[PackageName] version:*`

### <a name="examples"></a>使用例 

- すべての Xamarin. Forms パッケージを一覧表示します。 

    `Xamarin.Forms version:`

- すべての Xamarin .x パッケージが一覧表示されます。 

    `Xamarin.Forms version:1.4`

> [!NOTE]
> `version:` & バージョン番号の間に空白を追加すると、バージョンが指定されていないかのように検索が実行されます。
