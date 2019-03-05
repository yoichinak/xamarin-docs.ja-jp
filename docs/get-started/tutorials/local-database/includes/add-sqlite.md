# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**LocalDatabaseTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**LocalDatabaseTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**で、**LocalDatabaseTutorial** プロジェクトを選択し、右クリックして **[NuGet パッケージの管理...]** を選択します。

    ![選択されている [NuGet パッケージの管理] メニュー項目のスクリーンショット](../images/vs/add-nuget-packages.png "NuGet パッケージの追加メニュー項目")

1. **NuGet パッケージ マネージャー**で、**[参照]** タブを選択し、**pcl-sqlite-net** NuGet パッケージを検索して選択し、**[インストール]** ボタンをクリックしてプロジェクトに追加します。

    ![NuGet パッケージ マネージャーの SQLite.NET NuGet パッケージのスクリーンショット](../images/vs/add-package.png "SQLite.NET NuGet パッケージ")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

1. エラーが発生しないようにソリューションを構築してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**LocalDatabaseTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**LocalDatabaseTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。
    
    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** で、**LocalDatabaseTutorial** プロジェクトを選択し、右クリックして **[追加] > [Add NuGet Packages...]\(NuGet パッケージの追加...\)** を選択します。

    ![[Add NuGet Packages...]\(NuGet パッケージの追加...\) メニュー項目が選択されているスクリーンショット](../images/vsmac/add-nuget-packages.png "[Add NuGet Packages...]\(NuGet パッケージの追加...\) メニュー項目")

1. **[パッケージを追加]** ウィンドウで、**sqlite-net-pcl** NuGet パッケージを検索して選択し、**[パッケージを追加]** ボタンをクリックしてプロジェクトに追加します。

    ![NuGet パッケージ マネージャーの SQLite.NET NuGet パッケージのスクリーンショット](../images/vsmac/add-package.png "SQLite.NET NuGet パッケージ")

    > [!NOTE]
    > 類似した名前を持つ NuGet パッケージが多数あります。 正しいパッケージには、次の属性があります。
    > - **作成者:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet リンク:**  [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > パッケージ名に関係なく、この NuGet パッケージは .NET Standard プロジェクトで使用できます。

    このパッケージは、データベース操作をアプリケーションに組み込むために使用されます。

1. エラーが発生しないようにソリューションを構築してください。
