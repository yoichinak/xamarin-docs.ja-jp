---
title: "NuGet を使用する NUnit 2.6.4 のインストール"
description: "このガイドでは、NuGet を使用して NUnit 3.0 を NUnit 2.6.4 にダウングレードする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: b7f42d6e36638bf5c7e98b9363295e37997ee067
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="installing-nunit-264-using-nuget"></a>NuGet を使用する NUnit 2.6.4 のインストール

_このガイドでは、NuGet を使用して NUnit 3.0 を NUnit 2.6.4 にダウングレードする方法について説明します。_

Visual Studio for Mac または Xamarin.UITest を使用してテストを作成する開発者は [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4) を使用する必要があります。NUnit 3.0 以上は Visual Studio for Mac や Xamarin.UITest と互換性がないためです。 Visual Studio for Mac または Xamarin.UITests で NUnit 3.0 を使用して単体テストを実行しようとすると失敗します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

このガイドでは、Visual Studio for Mac 用の NuGet を使用して NUnit 2.6.4 をインストールする方法について説明します。 次の手順では、必要に応じて NUnit 3.0 のアンインストールも行います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

このガイドでは、Visual Studio 2015 で NuGet を使用して NUnit 3.0 を NUnit 2.6.4 にダウングレードする方法を説明します。

-----

## <a name="requirements"></a>必要条件

このガイドでは、モバイル アプリ プロジェクトとテスト プロジェクトについて既存のソリューションがあることを前提とします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>Visual Studio for Mac での NUnit 2.6.4 のインストール

次の手順では、NUnit 2.6.4 のインストール方法について説明します。


1. **パッケージ マネージャーを開く** - **[パッケージ]** を右クリックし、ポップアップ メニューから **[パッケージの追加]** を選択します。

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "[パッケージ] を右クリックし、ポップアップ メニューから [パッケージの追加] を選択します。")](installing-nunit-using-nuget-images/add-packages-xs.png#lightbox)
    
1. **`NUnit version:2.6.4` を検索する** - Visual Studio for Mac では、(必要に応じて) NUnit 3.0 をアンインストールしてから NUnit 2.6.4 をダウンロードしてインストールします。 **[パッケージの追加]** ダイアログで、右上隅にある**検索**フィールドに `nunit version:2.6.4` というテキストを入力します。 検索結果から **NUnit** を選択して、**[パッケージの追加]** ボタンをクリックします。

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "検索結果から NUnit を選択して、[パッケージの追加] ボタンをクリックします。")](installing-nunit-using-nuget-images/nunit-search-xs.png#lightbox)


Solution Pad で NUnit パッケージのバージョン番号を調べ、NUnit 2.6.4 がインストールされていることを確認できます。

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "Solution Pad で NUnit パッケージのバージョン番号を調べます。")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png#lightbox)

## <a name="summary"></a>まとめ

このガイドでは、Visual Studio for Mac でパッケージ マネージャー コンソールを使用して、NUnit 3.0 を NUnit 2.6.4 にダウングレードする方法を説明しました。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>Visual Studio での NUnit 2.6.4 のインストール

このセクションでは、Visual Studio 2015 で _NuGet パッケージ マネージャー コンソール_を使用して、NUnit 3.0 をアンインストールし、NUnit 2.6.4 をインストールする方法に焦点を当てています。


1. **NuGet パッケージ マネージャー コンソールを起動する** - **[ツール]、[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。

    [![](installing-nunit-using-nuget-images/package-manager-console.png "NuGet パッケージ マネージャー コンソールを起動する - [ツール]、[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール] の順に選択します。")](installing-nunit-using-nuget-images/package-manager-console.png#lightbox)
    
1. **NUnit のバージョンを確認する** - 必要に応じて、`Get-Package -Project <UITEST PROJECT>` というコマンドを実行してインストールされている NUnit のバージョンを確認することができます。

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

NUnit 3.0 以上が表示された場合は、NUnit 2.6.4 にダウングレードする必要があります。

1. **NUnit 3.0 をアンインストールする** - `Uninstall-Package` コマンドレットを使用して、NUnit 3.0 をアンインストールします。

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **NUnit 2.6.4 をインストールする** - 次のスニペットに示されているように、`Install-Package` コマンドレットを使用して NUnit 2.6.4 をインストールします。

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>まとめ

このガイドでは、Visual Studio 2015 でパッケージ マネージャー コンソールを使用して、NUnit 3.0 を NUnit 2.6.4 にダウングレードする方法を説明しました。

-----

## <a name="related-links"></a>関連リンク

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [NUnit 2.6.4 NuGet パッケージ](https://www.nuget.org/packages/NUnit/2.6.4)
