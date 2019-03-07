# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動し、**WebServiceTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**WebServiceTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー**で、**[WebServiceTutorial]** プロジェクトを選択し、右クリックして **[NuGet パッケージの管理...]** を選びます。

    ![[Add NuGet Packages...]\(NuGet パッケージの追加...\) メニュー項目が選択されているスクリーンショット](../images/vs/add-nuget-packages.png "[Add NuGet Packages...]\(NuGet パッケージの追加...\) メニュー項目")

1. **NuGet パッケージ マネージャー**で、**[参照]** タブを選択し、**Newtonsoft.Json** NuGet パッケージを検索して選択し、**[インストール]** ボタンをクリックしてプロジェクトに追加します。

    ![NuGet パッケージ マネージャーの Newtonsoft.Json NuGet パッケージのスクリーンショット](../images/vs/add-package.png "Newtonsoft.Json NuGet パッケージ")

    このパッケージは、JSON のシリアル化解除をアプリケーションに組み込むために使用されます。

1. エラーがないようにソリューションを構築してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、**WebServiceTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。 共有コード メカニズムとして .NET Standard がアプリで使用されていることを確認します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**WebServiceTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** で、**[WebServiceTutorial]** プロジェクトを選び、右クリックして **[追加] > [Add NuGet Packages...]\(NuGet パッケージの追加...\)** の順に選択します。

    ![[Add NuGet Packages...]\(NuGet パッケージの追加...\) メニュー項目が選択されているスクリーンショット](../images/vsmac/add-nuget-packages.png "[Add NuGet Packages...]\(NuGet パッケージの追加...\) メニュー項目")

1. **[パッケージを追加]** ウィンドウで、**Newtonsoft.Json** NuGet パッケージを検索して選択し、**[パッケージを追加]** ボタンをクリックしてプロジェクトに追加します。

    ![NuGet パッケージ マネージャーの Newtonsoft.Json NuGet パッケージのスクリーンショット](../images/vsmac/add-package.png "Newtonsoft.Json NuGet パッケージ")

    このパッケージは、JSON のシリアル化解除をアプリケーションに組み込むために使用されます。

1. エラーがないようにソリューションを構築してください。
