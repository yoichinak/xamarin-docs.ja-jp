---
title: "NuGet パッケージをダウン グレードする方法は?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 98d12bd93c50690909ac902a6f2498bcdb94960f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet パッケージをダウン グレードする方法は?

Visual Studio for Mac と Visual Studio の両方を古いバージョンのパッケージを選択し、それらを自動的にインストールする機能があります。パッケージの更新の動作に似ています。 次の手順は次のとおりです。

## <a name="visual-studio"></a>Visual Studio
1. 移動して**ツール > NuGet Package Manager > パッケージ マネージャー コンソール**
2. プロジェクト設定**既定のプロジェクト**
3. この構文を使用します。

    > パッケージのインストール [PackageName]-[バージョン] メニューのタブのバージョン

ことができますもコピー/貼り付け、パッケージの NuGet ページから正確なコマンド。 Xamarin.Forms の使用例: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. プロジェクトで、[パッケージ] フォルダーを右クリックし、選択**パッケージの追加**
2. Searchbar では、次の構文を使用して、必要なパッケージを検索することができます。

    `[PackageName] version:*`

### <a name="examples"></a>使用例 
- Xamarin.Forms のすべてのパッケージの一覧です。 

    `Xamarin.Forms version:`
- Xamarin.Forms 1.4.x のすべてのパッケージの一覧です。 


    `Xamarin.Forms version:1.4`

*注: 間に空白を追加する場合`version:`(&)、バージョン番号、検索場合と同様のバージョンが指定されていません。*

