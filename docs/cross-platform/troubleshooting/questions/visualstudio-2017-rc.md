---
title: "Visual Studio 2017 リリース候補を使用して、Xamarin を使用できますか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 745e532a9543029f13e5fdffcc15153d780278ec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Visual Studio 2017 リリース候補を使用して、Xamarin を使用できますか。

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Visual Studio 2017 リリース候補を使用して、Xamarin を使用できますか。

はい。 Visual Studio 2017 Release Candidate で Xamarin を活用することができます。 しかし、なお、現在、Visual Studio 2017 RC に Xamarin をインストールすると Visual Studio 2015/2013 をインストールする Xamarin の以前のバージョンはアンインストールします。 Xamarin for Visual Studio 2017 をできますインストールされませんで Xamarin と同時の Visual Studio 2015/2013、Visual Studio インストーラー システムの使用率に向けたの MSI パッケージ離れて Visual Studio 2017 の結果として。

チームは現在この想定される動作をバイパスする方法をお探し、中にユーザーのニーズに基づいて、開発環境の選択はお勧めします。 

> [!IMPORTANT]
> Xamarin.iOS プロジェクトを作成している場合は、Xamarin.iOS チャネルの同じバージョンでは、ペアの Mac コンピューター上の Mac 用の Visual Studio を確認します。

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate に Xamarin をインストールする方法

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>Visual Studio 2017 RC インストーラー時にインストールします。

* 選択、 **Xamarin**新しいの一部としてコンポーネント**Visual Studio インストーラー**

  [ ![](visualstudio-2017-rc-images/install1-sml.png "Visual Studio 2017 RC インストーラー画面")](visualstudio-2017-rc-images/install1-orig.png)

これにより、Xamarin.iOS と Xamarin.Android 開発用 Visual Studio 拡張機能がインストールされます。

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Visual Studio 2017 年 1 RC の既存のインストールで Xamarin をインストールまたは再インストールします。

#### <a name="using-the-visual-studio-installer"></a>Visual Studio インストーラーを使用します。

1. Visual Studio インストーラー アプリケーションを検索します。

  [ ![](visualstudio-2017-rc-images/reinstall1-sml.png "Visual Studio インストーラー アプリケーションの検索結果")](visualstudio-2017-rc-images/reinstall1-orig.png)

2. 選択: します。 **.NET (プレビュー) を使用したモバイル開発**ワークロード タブで、または

  [ ![](visualstudio-2017-rc-images/reinstall2-sml.png "VS インストーラー ワークロード タブ")](visualstudio-2017-rc-images/reinstall2-orig.png) b します。 **Xamarin**で、**個々 のコンポーネント** タブ

  [ ![](visualstudio-2017-rc-images/reinstall3-sml.png "VS インストーラー コンポーネント タブ")](visualstudio-2017-rc-images/reinstall3-orig.png)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>Visual Studio 内で Visual Studio インストーラーを使用します。
1. Visual Studio 2017 のスタート ページに移動します。
2. をクリックして**プロジェクト テンプレートが複数**下にある、**新しいプロジェクト**セクション

    [ ![](visualstudio-2017-rc-images/reinstall4-sml.png "Visual Studio スタート ページ")](visualstudio-2017-rc-images/reinstall4-orig.png)
3. をクリックして`Open Visual Studio Installer`左側のウィンドウで

    [ ![](visualstudio-2017-rc-images/reinstall5-sml.png "[新しいプロジェクト] 画面")](visualstudio-2017-rc-images/reinstall5-orig.png)
4. 次のいずれかを選択します。
    
    a.  **.NET (プレビュー) を使用したモバイル開発**ワークロード タブで、または

    [ ![](visualstudio-2017-rc-images/reinstall2-sml.png "VS インストーラー ワークロード タブ")](visualstudio-2017-rc-images/reinstall2-orig.png) b します。 **Xamarin**で、**個々 のコンポーネント** タブ

    [ ![](visualstudio-2017-rc-images/reinstall3-sml.png "VS インストーラー コンポーネント タブ")](visualstudio-2017-rc-images/reinstall3-orig.png)
