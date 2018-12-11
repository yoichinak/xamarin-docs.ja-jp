---
title: Xamarin.Forms のクイック スタート
description: この記事では、Xamarin.Forms を使用して、シンプルなクロスプラットフォームの Notes アプリケーションを作成する方法について説明します。これにより、メモを入力して、デバイス ストレージに保持できるようになります。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/21/2018
ms.openlocfilehash: 880fa527d91098cf8c5f8c46cd9c5cce9ec9a489
ms.sourcegitcommit: 744c0a50420bb091fca8b92a84c20e61c741cf9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2018
ms.locfileid: "52743139"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms のクイック スタート

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

このクイック スタートでは、Xamarin.Forms を使用して、シンプルなクロスプラットフォームの Notes アプリケーションを作成する方法について説明します。これにより、メモを入力して、デバイス ストレージに保持できるようになります。 最終的なアプリケーションは、次のとおりです。

[![](quickstart-experimental-images/screenshots-sml.png "Notes アプリケーション")](quickstart-experimental-images/screenshots.png#lightbox "Notes アプリケーション")

::: zone pivot="windows"

## <a name="get-started-with-visual-studio"></a>Visual Studio 入門

1. **[スタート]** 画面で、Visual Studio を起動します。 スタート ページが開きます。

    ![](quickstart-experimental-images/vs/start-page.png "Visual Studio")

2. Visual Studio で、**[新しいプロジェクトの作成]** をクリックして新しいプロジェクトを作成します。

    ![](quickstart-experimental-images/vs/new-solution.png "新しいプロジェクト")

3. **[新しいプロジェクト]** ダイアログで、**[クロスプラットフォーム]** をクリックして、**[モバイル アプリ (Xamarin.Forms)]** テンプレートを選択し、[名前] を「**Notes**」に設定し、プロジェクトの適切な場所を選んで **[OK]** ボタンをクリックします。

    ![](quickstart-experimental-images/vs/new-project.png "クロスプラットフォームのプロジェクト テンプレート")

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、**Notes** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのクイック スタートからソリューションにコピーするときに、ビルド エラーが発生します。

4. **[新しいクロスプラットフォーム アプリ]** ダイアログで、**[空のアプリケーション]** をクリックして、コード共有方法として **[.NET Standard]** を選択し、**[OK]** ボタンをクリックします。

    ![](quickstart-experimental-images/vs/new-app.png "新しいクロスプラット フォーム アプリ")

5. **ソリューション エクスプローラー**の **Notes** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。

    ![](quickstart-experimental-images/vs/open-mainpage-xaml.png "MainPage.xaml を開く")

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
            <Editor x:Name="_editor"
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

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。このページは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。

    **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **ソリューション エクスプローラー**の **Notes** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![](quickstart-experimental-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

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
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。

    **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio で、**[ビルド]、[ソリューションのビルド]** メニュー項目の順に選択します (または F6 キーを押します)。 ソリューションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

      ![](quickstart-experimental-images/vs/build-succeeded.png "ビルドに成功しました")

    エラーがある場合は、ソリューションが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーターでアプリケーションを起動します。

    ![](quickstart-experimental-images/vs/android-start.png "Visual Studio Android ツール バー")

    ![](quickstart-experimental-images/vs/notes-android.png "Android Emulator の Notes")

    メモを入力して **[保存]** ボタンを押します。

    > [!NOTE]
    > 次の手順は、Xamarin.Forms 開発のシステム要件を満たしている、[ペアリングされた Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) がある場合にのみ実行する必要があります。

3. Visual Studio ツール バーで、**[Notes.iOS]** プロジェクトを右クリックして、**[スタートアップ プロジェクトに設定]** を選択します。

      ![](quickstart-experimental-images/vs/set-as-startup-project-ios.png "iOS をスタートアップ プロジェクトとして設定")

4. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)でアプリケーションを起動します。

    ![](quickstart-experimental-images/vs/ios-start.png "Visual Studio iOS ツール バー")

    ![](quickstart-experimental-images/vs/notes-ios.png "iOS シミュレーターの Notes")

    メモを入力して **[保存]** ボタンを押します。

::: zone-end
::: zone pivot="macos"

## <a name="get-started-with-visual-studio-for-mac"></a>Visual Studio for Mac の概要

1. Visual Studio for Mac を起動し、スタート ページで **[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。

    ![](quickstart-experimental-images/vsmac/new-project.png "新しいソリューション")

2. **[新しいプロジェクト用のテンプレートを選びます]** ダイアログで、**[マルチプラットフォーム]、[アプリ]** の順にクリックし、**[空白フォームのアプリ]** テンプレートを選択して、**[次へ]** ボタンをクリックします。

    ![](quickstart-experimental-images/vsmac/choose-template.png "テンプレートを選択します")

3. **[Configure your Blank Forms app]\(空白フォームのアプリの構成\)** ダイアログで、新しいアプリに **Notes** という名前を付け、**[.NET Standard を使用する]** ラジオ ボタンがオンになっていることを確認し、**[次へ]** ボタンをクリックします。    

    ![](quickstart-experimental-images/vsmac/configure-app.png "フォーム アプリケーションの構成")

4. **[Configure your new Blank Forms app]\(新しい空白フォームのアプリの構成\)** ダイアログでは、ソリューションとプロジェクトの名前は **Notes** に設定したままにし、プロジェクトに適切な場所を選択し、**[作成]** をクリックしてプロジェクトを作成します。

    ![](quickstart-experimental-images/vsmac/configure-project.png "フォーム プロジェクトの構成")

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、ソリューションとプロジェクトの両方の名前が **Notes** である必要があります。 別の名前を使用すると、コードをこのクイック スタートからプロジェクトにコピーするときに、ビルド エラーが発生します。

5. **Solution Pad** の **Notes** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。

    ![](quickstart-experimental-images/vsmac/mainpage-xaml.png "MainPage.xaml")

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
            <Editor x:Name="_editor"
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

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。このページは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad** の **Notes** プロジェクトで、**[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![](quickstart-experimental-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

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
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio for Mac で、**[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 プロジェクトがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

      ![](quickstart-experimental-images/vsmac/build-successful.png "ビルドに成功しました")

    エラーがある場合は、プロジェクトが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. **Solution Pad** で **Notes.iOS** プロジェクトを選択して右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。

      ![](quickstart-experimental-images/vsmac/set-startup-project-ios.png "iOS をスタートアップ プロジェクトとして設定")

3. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した iOS シミュレーター内でアプリケーションを起動します。

      ![](quickstart-experimental-images/vsmac/start.png "Visual Studio for Mac ツール バー")

      ![](quickstart-experimental-images/vsmac/notes-ios.png "iOS シミュレーターの Notes")

    メモを入力して **[保存]** ボタンを押します。

4. **Solution Pad** で **Notes.Droid** プロジェクトを選択して右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。

      ![](quickstart-experimental-images/vsmac/set-startup-project-android.png "Android をスタートアップ プロジェクトとして設定")

5. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーター内でアプリケーションを起動します。

      ![](quickstart-experimental-images/vsmac/notes-android.png "Android Emulator の Notes")

    メモを入力して **[保存]** ボタンを押します。

::: zone-end

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
