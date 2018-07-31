---
title: NuGet パッケージをダウン グレードする方法
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 72fdf7246b148fa95ea312284957072ecda47121
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351217"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet パッケージをダウン グレードする方法

Visual Studio for Mac と Visual Studio の以前のバージョンのパッケージを選択して、自動的にインストールする機能があります。パッケージの更新の動作に似ています。 次の手順を以下に示します。

## <a name="visual-studio"></a>Visual Studio
1. 移動して**ツール > NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**
2. プロジェクト設定**既定のプロジェクト**
3. この構文を使用します。

    > パッケージのインストール [PackageName]-[バージョン] メニューのタブのバージョン

できますもコピー/貼り付けするパッケージの NuGet ページから正確なコマンド。 Xamarin.Forms の例: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. プロジェクトで、パッケージ フォルダーを右クリックし、選択**パッケージの追加**
2. Searchbar では、必要なパッケージを検索する次の構文を使用できます。

    `[PackageName] version:*`

### <a name="examples"></a>使用例 
- すべての Xamarin.Forms パッケージを一覧表示されます。 

    `Xamarin.Forms version:`
- すべての Xamarin.Forms 1.4.x パッケージを一覧表示されます。 

    `Xamarin.Forms version:1.4`

*注: の間にスペースを追加する場合`version:`(&)、バージョン番号を検索場合と同様にバージョンが指定されていません。*

