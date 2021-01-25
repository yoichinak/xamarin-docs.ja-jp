---
ms.openlocfilehash: e4c0e1500e6e445580a971c6af0af0fd267ee0f7
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690084"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

このチュートリアルを完了するには、 **.NET によるモバイル開発** ワークロードがインストールされた、Visual Studio 2019 (最新リリース) が必要です。 さらに、iOS でチュートリアル アプリケーションを構築するには、ペアリング済みの Mac が必要になります。 Xamarin プラットフォームのインストールについては、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

1. Visual Studio を起動し、**LocalDatabaseTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**LocalDatabaseTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー** で、**LocalDatabaseTutorial** プロジェクトを選択し、右クリックして **[NuGet パッケージの管理...]** を選択します。

    ![選択されている [NuGet パッケージの管理] メニュー項目のスクリーンショット](../images/vs/add-nuget-packages.png "[Add NuGet Packages]\(NuGet パッケージの追加) メニュー項目")

1. **NuGet パッケージ マネージャー** で、 **[参照]** タブを選択し、**pcl-sqlite-net** NuGet パッケージを検索して選択し、 **[インストール]** ボタンをクリックしてプロジェクトに追加します。

    ![NuGet パッケージ マネージャーの SQLite.NET NuGet パッケージのスクリーンショット](../images/vs/add-package.png "SQLite.NET NuGet Package")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** SQLite-net
    > - **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

1. エラーがないようにソリューションを構築してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

このチュートリアルを完了するには、iOS と Android のプラットフォームのサポートがインストールされた Visual Studio for Mac (最新リリース) が必要です。 さらに、Xcode (最新リリース) も必要になります。 Xamarin プラットフォームのインストールについて詳しくは、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

1. Visual Studio for Mac を起動し、**LocalDatabaseTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**LocalDatabaseTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** で、**LocalDatabaseTutorial** プロジェクトを選択し、右クリックして **[NuGet パッケージの管理...]** を選択します。

    ![選択されている [Add NuGet Packages]\(NuGet パッケージの追加\) メニュー項目のスクリーンショット](../images/vsmac/add-nuget-packages.png "[Add NuGet Packages]\(NuGet パッケージの追加) メニュー項目")

1. **[NuGet パッケージの管理]** ウィンドウで、**sqlite-net-pcl** NuGet パッケージを検索して選択し、 **[パッケージを追加]** ボタンをクリックしてプロジェクトに追加します。

    ![NuGet パッケージ マネージャーの SQLite.NET NuGet パッケージのスクリーンショット](../images/vsmac/add-package.png "SQLite.NET NuGet Package")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **ID:** sqlite-net-pcl
    > - **作成者:** SQLite-net
    > - **NuGet リンク:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

1. エラーがないようにソリューションを構築してください。
