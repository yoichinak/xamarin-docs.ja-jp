---
title: Xamarin.Forms アプリケーションの 1 つのページを作成します。
description: この記事では、メモを入力し、デバイスのストレージに保存することができます、1 つのページのクロス プラットフォームの Xamarin.Forms アプリケーションを作成する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 7696ece64da28f05bb15866214de4a7f1103d06f
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293314"
---
# <a name="create-a-single-page-xamarinforms-application"></a>ページの 1 つの Xamarin.Forms アプリケーションを作成します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

このクイック スタートでは、学習する方法。

- クロスプラット フォーム-Xamarin.Forms アプリケーションを作成します。
- EXtensible Application Markup Language (XAML) を使用してページのユーザー インターフェイスを定義します。
- コードから XAML ユーザー インターフェイス要素とやり取りします。

このクイック スタートでは、メモを入力し、デバイスのストレージに保存することができます、クロスプラット フォーム-Xamarin.Forms アプリケーションを作成する方法をについて説明します。 最終的なアプリケーションは、次のとおりです。

[![](single-page-images/screenshots-sml.png "Notes アプリケーション")](single-page-images/screenshots.png#lightbox "Notes アプリケーション")

::: zone pivot="windows"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio 2017 (最新リリース) で、 **.NET によるモバイル開発**ワークロードをインストールします。
- サポート技術情報のC#します。
- (省略可能)Ios アプリケーションの構築にペアリングした Mac。

これらの前提条件の詳細については、次を参照してください。[インストール Xamarin](~/cross-platform/get-started/installation/index.md)します。 Visual Studio 2017 を Mac ビルド ホストに接続する方法については、次を参照してください。 [Xamarin.iOS 開発用 Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)します。

## <a name="get-started-with-visual-studio"></a>Visual Studio 入門

1. Visual Studio を起動して、スタート ページで次のようにクリックします**新しいプロジェクトの作成...** 新しいプロジェクトを作成します。

    ![](single-page-images/vs/new-solution.png "新しいプロジェクト")

2. **[新しいプロジェクト]** ダイアログで、**[クロスプラットフォーム]** をクリックして、**[モバイル アプリ (Xamarin.Forms)]** テンプレートを選択し、[名前] を「**Notes**」に設定し、プロジェクトの適切な場所を選んで **[OK]** ボタンをクリックします。

    ![](single-page-images/vs/new-project.png "クロスプラットフォームのプロジェクト テンプレート")

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、**Notes** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのクイック スタートからソリューションにコピーするときに、ビルド エラーが発生します。

3. **[新しいクロスプラットフォーム アプリ]** ダイアログで、**[空のアプリケーション]** をクリックして、コード共有方法として **[.NET Standard]** を選択し、**[OK]** ボタンをクリックします。

    ![](single-page-images/vs/new-app.png "新しいクロスプラット フォーム アプリ")

    作成される、.NET Standard ライブラリの詳細については、次を参照してください。 [Xamarin.Forms アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

4. **ソリューション エクスプローラー**の **Notes** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。

    ![](single-page-images/vs/open-mainpage-xaml.png "MainPage.xaml を開く")

5. **MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。このページは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 ユーザー インターフェイスの作成の詳細については、次を参照してください。[ユーザー インターフェイス](deepdive.md#user-interface)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

6. **ソリューション エクスプローラー**の **Notes** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![](single-page-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

7. **MainPage.xaml.cs** で、テンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、次を参照してください。[ユーザーとの対話に応答して](deepdive.md#responding-to-user-interaction)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio で、**[ビルド]、[ソリューションのビルド]** メニュー項目の順に選択します (または F6 キーを押します)。 ソリューションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

      ![](single-page-images/vs/build-succeeded.png "ビルドに成功しました")

    エラーがある場合は、ソリューションが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーターでアプリケーションを起動します。

    ![](single-page-images/vs/android-start.png "Visual Studio Android ツール バー")

    [![](single-page-images/vs/notes-android.png "Android エミュレーターでのノート")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、次を参照してください。[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    > [!NOTE]
    > 次の手順は、Xamarin.Forms 開発のシステム要件を満たしている、[ペアリングされた Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) がある場合にのみ実行する必要があります。

3. Visual Studio ツール バーで、**[Notes.iOS]** プロジェクトを右クリックして、**[スタートアップ プロジェクトに設定]** を選択します。

      ![](single-page-images/vs/set-as-startup-project-ios.png "iOS をスタートアップ プロジェクトとして設定")

4. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)でアプリケーションを起動します。

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS ツール バー")

    [![](single-page-images/vs/notes-ios.png "IOS シミュレーターでのノート")](single-page-images/vs/notes-ios-large.png#lightbox "iOS シミュレーターでのノート")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、次を参照してください。[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio for Mac (最新リリース)、iOS と Android プラットフォームのサポートがインストールされています。
- Xcode (最新リリース)。
- サポート技術情報のC#します。

これらの前提条件の詳細については、次を参照してください。[インストール Xamarin](~/cross-platform/get-started/installation/index.md)します。

## <a name="get-started-with-visual-studio-for-mac"></a>Visual Studio for Mac の概要

1. Visual Studio for Mac を起動し、スタート ページで **[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。

    ![](single-page-images/vsmac/new-project.png "新しいソリューション")

2. **[新しいプロジェクト用のテンプレートを選びます]** ダイアログで、**[マルチプラットフォーム]、[アプリ]** の順にクリックし、**[空白フォームのアプリ]** テンプレートを選択して、**[次へ]** ボタンをクリックします。

    ![](single-page-images/vsmac/choose-template.png "テンプレートを選択します")

3. **[Configure your Blank Forms app]\(空白フォームのアプリの構成\)** ダイアログで、新しいアプリに **Notes** という名前を付け、**[.NET Standard を使用する]** ラジオ ボタンがオンになっていることを確認し、**[次へ]** ボタンをクリックします。    

    ![](single-page-images/vsmac/configure-app.png "フォーム アプリケーションの構成")

4. **[Configure your new Blank Forms app]\(新しい空白フォームのアプリの構成\)** ダイアログでは、ソリューションとプロジェクトの名前は **Notes** に設定したままにし、プロジェクトに適切な場所を選択し、**[作成]** をクリックしてプロジェクトを作成します。

    ![](single-page-images/vsmac/configure-project.png "フォーム プロジェクトの構成")

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、ソリューションとプロジェクトの両方の名前が **Notes** である必要があります。 別の名前を使用すると、コードをこのクイック スタートからプロジェクトにコピーするときに、ビルド エラーが発生します。

    作成される、.NET Standard ライブラリの詳細については、次を参照してください。 [Xamarin.Forms アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

5. **Solution Pad** の **Notes** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。

    ![](single-page-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. **MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。このページは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 ユーザー インターフェイスの作成の詳細については、次を参照してください。[ユーザー インターフェイス](deepdive.md#user-interface)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad** の **Notes** プロジェクトで、**[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![](single-page-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. **MainPage.xaml.cs** で、テンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、次を参照してください。[ユーザーとの対話に応答して](deepdive.md#responding-to-user-interaction)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio for Mac で、**[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 プロジェクトがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

      ![](single-page-images/vsmac/build-successful.png "ビルドに成功しました")

    エラーがある場合は、プロジェクトが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. **Solution Pad**を選択、 **Notes.iOS**プロジェクトを右クリックし、**スタートアップ プロジェクトとして設定**:

      ![](single-page-images/vsmac/set-startup-project-ios.png "iOS をスタートアップ プロジェクトとして設定")

3. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した iOS シミュレーター内でアプリケーションを起動します。

      ![](single-page-images/vsmac/start.png "Visual Studio for Mac ツール バー")

      [![](single-page-images/vsmac/notes-ios.png "IOS シミュレーターでのノート")](single-page-images/vsmac/notes-ios-large.png#lightbox "iOS シミュレーターでのノート")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、次を参照してください。[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

4. **Solution Pad**を選択、 **Notes.Droid**プロジェクトを右クリックし、**スタートアップ プロジェクトとして設定**:

      ![](single-page-images/vsmac/set-startup-project-android.png "Android をスタートアップ プロジェクトとして設定")

5. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーター内でアプリケーションを起動します。

      [![](single-page-images/vsmac/notes-android.png "Android エミュレーターでのノート")](single-page-images/vsmac/notes-android-large.png#lightbox "Notes in the Android Simulator")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、次を参照してください。[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイック スタートでは説明した方法。

- クロスプラット フォーム-Xamarin.Forms アプリケーションを作成します。
- EXtensible Application Markup Language (XAML) を使用してページのユーザー インターフェイスを定義します。
- コードから XAML ユーザー インターフェイス要素とやり取りします。

複数ページのアプリケーションには、このシングル ページ アプリケーションを有効にするには、次のクイック スタートに進みます。

> [!div class="nextstepaction"]
> [次へ](multi-page.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
- [Xamarin.Forms のクイック スタートの詳細情報](deepdive.md)
