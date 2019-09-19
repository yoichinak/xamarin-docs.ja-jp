---
title: NuGet パッケージをダウングレードする方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: daa8159dd7cac1f727904ca5de08908fd3ca1af9
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105667"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet パッケージをダウングレードする方法を教えてください

Visual Studio for Mac & Visual Studio には、古いバージョンのパッケージを選択して自動的にインストールする機能があります。更新パッケージの動作と同様です。 これらの手順については、以下で説明します。

## <a name="visual-studio"></a>Visual Studio

1. [**ツール] > [NuGet パッケージマネージャー] > [パッケージマネージャーコンソール**] にアクセス
2. **[既定のプロジェクト]** でプロジェクトを設定する
3. 次の構文を使用します。

    > パッケージのインストール [PackageName]-[バージョン] メニューのタブのバージョン

また、パッケージの NuGet ページから正確なコマンドをコピーして貼り付けることもできます。 Xamarin. Forms の例:[https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

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
> バージョン番号 & の間`version:`に空白を追加すると、バージョンが指定されていないように検索が動作します。
