---
title: 単一ページの Xamarin.Forms アプリケーションを作成する
description: この記事では、単一ページのクロスプラットフォーム Xamarin.Forms アプリケーションを作成する方法について説明します。このアプリにより、メモを入力し、デバイス ストレージに保持できるようになります。
zone_pivot_groups: platform-dev16
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 96b3e6bd055c0bc89ae7bcbb66c8b3f48b21ad17
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436231"
---
# <a name="create-a-single-page-no-locxamarinforms-application"></a>単一ページの Xamarin.Forms アプリケーションを作成する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)

このクイックスタートでは、次の方法について学習します。

- クロスプラットフォーム Xamarin.Forms インターフェイスを作成する。
- Extensible Application Markup Language (XAML) を使用して、ページのユーザー インターフェイスを定義する。
- コードから XAML ユーザー インターフェイス要素を操作する。

このクイックスタートでは、クロスプラットフォーム Xamarin.Forms アプリケーションを作成する方法について説明します。このアプリにより、メモを入力し、デバイス ストレージに保持できるようになります。 最終的なアプリケーションは、次のとおりです。

[![Notes アプリケーション](single-page-images/screenshots-sml.png)](single-page-images/screenshots.png#lightbox "Notes アプリケーション")

::: zone pivot="windows"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio 2019 (最新リリース) と、 **.NET によるモバイル開発**ワークロードがインストールされている。
- C# に関する知識。
- (必要に応じて) iOS 上でアプリケーションをビルドするためのペアリング済みの Mac。

これらの前提条件の詳細については、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

## <a name="get-started-with-visual-studio-2019"></a>Visual Studio 2019 の使用を開始する

1. Visual Studio 2019 を起動し、スタート ウィンドウで **[新しいプロジェクトを作成する]** をクリックして新しいプロジェクトを作成します。

    ![新しいプロジェクト](single-page-images/vs/new-solution-2019.png)

2. **[新しいプロジェクトを作成する]** ウィンドウの **[プロジェクト タイプ]** ドロップ ダウンで **[モバイル]** を選択し、 **[モバイル アプリ (Xamarin.Forms]** テンプレートを選択して、 **[次へ]** ボタンをクリックします。

    ![クロスプラットフォームのプロジェクト テンプレート](single-page-images/vs/new-project-2019.png)

3. **[新しいプロジェクトを構成します]** ウィンドウで、 **[プロジェクト名]** を **[Notes]** に設定し、プロジェクトに適切な場所を選択し、 **[作成]** ボタンをクリックします。

    ![プロジェクトを構成する](single-page-images/vs/configure-project.png)

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、**Notes** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのクイック スタートからソリューションにコピーするときに、ビルド エラーが発生します。

4. **[新しいクロス プラットフォーム アプリ]** ダイアログで、 **[空のアプリケーション]** をクリックし、 **[OK]** ボタンをクリックします。

    ![新しいクロスプラット フォーム アプリ](single-page-images/vs/new-app-2019.png)

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](deepdive.md) の「[Xamarin.Forms アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)」を参照してください。

5. **ソリューション エクスプローラー**の **Notes** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。

    ![MainPage.xaml を開く](single-page-images/vs/open-mainpage-xaml-2019.png)

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

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、およびファイルの保存または削除をアプリケーションに指示する 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 ユーザー インターフェイスの作成の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **ソリューション エクスプローラー**の **Notes** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![MainPage.xaml.cs を開く](single-page-images/vs/open-mainpage-codebehind-2019.png)

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

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio で、 **[ビルド]、[ソリューションのビルド]** メニュー項目の順に選択します (または F6 キーを押します)。 ソリューションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

      ![ビルドに成功しました](single-page-images/vs/build-succeeded.png)

    エラーがある場合は、ソリューションが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーターでアプリケーションを起動します。

    ![Visual Studio Android ツール バー](single-page-images/vs/android-start.png)

    [![Android Emulator での Notes](single-page-images/vs/notes-android.png)](single-page-images/vs/notes-android-large.png#lightbox "Android シミュレーターでの Notes")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)」を参照してください。

    > [!NOTE]
    > 次の手順は、[ 開発のシステム要件を満たしている、](~/ios/get-started/installation/windows/connecting-to-mac/index.md)ペアリングされた MacXamarin.Forms がある場合にのみ実行する必要があります。

3. Visual Studio ツール バーで、 **[Notes.iOS]** プロジェクトを右クリックして、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![iOS をスタートアップ プロジェクトとして設定](single-page-images/vs/set-as-startup-project-ios.png)

4. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)でアプリケーションを起動します。

    ![Visual Studio iOS ツール バー](single-page-images/vs/ios-start.png)

    [![iOS シミュレーターでの Notes](single-page-images/vs/notes-ios.png)](single-page-images/vs/notes-ios-large.png#lightbox "iOS シミュレーターでの Notes")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)」を参照してください。

::: zone-end
::: zone pivot="win-vs2017"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio 2017 と、 **.NET によるモバイル開発**ワークロードがインストールされている。
- C# に関する知識。
- (必要に応じて) iOS 上でアプリケーションをビルドするためのペアリング済みの Mac。

これらの前提条件の詳細については、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

## <a name="get-started-with-visual-studio-2017"></a>Visual Studio 2017 の使用を開始する

1. Visual Studio 2017 を起動し、スタート ページで **[新しいプロジェクトの作成]** をクリックして新しいプロジェクトを作成します。

    ![新しいプロジェクト](single-page-images/vs/new-solution.png)

2. **[新しいプロジェクト]** ダイアログで、 **[クロスプラットフォーム]** をクリックして、 **[モバイル アプリ (Xamarin.Forms)]** テンプレートを選択し、[名前] を "**Notes**" に設定し、プロジェクトの適切な場所を選んで **[OK]** ボタンをクリックします。

    ![クロスプラットフォームのプロジェクト テンプレート](single-page-images/vs/new-project.png)

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、**Notes** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのクイック スタートからソリューションにコピーするときに、ビルド エラーが発生します。

3. **[新しいクロスプラットフォーム アプリ]** ダイアログで、 **[空のアプリケーション]** をクリックして、コード共有方法として **[.NET Standard]** を選択し、 **[OK]** ボタンをクリックします。

    ![新しいクロスプラット フォーム アプリ](single-page-images/vs/new-app.png)

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](deepdive.md) の「[Xamarin.Forms アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)」を参照してください。

4. **ソリューション エクスプローラー**の **Notes** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。

    ![MainPage.xaml を開く](single-page-images/vs/open-mainpage-xaml.png)

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

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、およびファイルの保存または削除をアプリケーションに指示する 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 ユーザー インターフェイスの作成の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

6. **ソリューション エクスプローラー**の **Notes** プロジェクトで **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![MainPage.xaml.cs を開く](single-page-images/vs/open-mainpage-codebehind.png)

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

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio で、 **[ビルド]、[ソリューションのビルド]** メニュー項目の順に選択します (または F6 キーを押します)。 ソリューションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

      ![ビルドに成功しました](single-page-images/vs/build-succeeded.png)

    エラーがある場合は、ソリューションが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーターでアプリケーションを起動します。

    ![Visual Studio Android ツール バー](single-page-images/vs/android-start.png)

    [![Android Emulator での Notes](single-page-images/vs/notes-android.png)](single-page-images/vs/notes-android-large.png#lightbox "Android シミュレーターでの Notes")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)」を参照してください。

    > [!NOTE]
    > 次の手順は、[ 開発のシステム要件を満たしている、](~/ios/get-started/installation/windows/connecting-to-mac/index.md)ペアリングされた MacXamarin.Forms がある場合にのみ実行する必要があります。

3. Visual Studio ツール バーで、 **[Notes.iOS]** プロジェクトを右クリックして、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![iOS をスタートアップ プロジェクトとして設定](single-page-images/vs/set-as-startup-project-ios.png)

4. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)でアプリケーションを起動します。

    ![Visual Studio iOS ツール バー](single-page-images/vs/ios-start.png)

    [![iOS シミュレーターでの Notes](single-page-images/vs/notes-ios.png)](single-page-images/vs/notes-ios-large.png#lightbox "iOS シミュレーターでの Notes")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)」を参照してください。

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio for Mac (最新リリース) と、iOS と Android プラットフォームのサポートがインストールされている。
- Xcode (最新リリース)。
- C# に関する知識。

これらの前提条件の詳細については、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

## <a name="get-started-with-visual-studio-for-mac"></a>Visual Studio for Mac の概要

1. Visual Studio for Mac を起動し、スタート ウィンドウで **[新規]** をクリックして新しいプロジェクトを作成します。

    ![新しいソリューション](single-page-images/vsmac/new-project.png)

2. **[新しいプロジェクト用のテンプレートを選びます]** ダイアログで、 **[マルチプラットフォーム]、[アプリ]** の順にクリックし、 **[空白フォームのアプリ]** テンプレートを選択して、 **[次へ]** ボタンをクリックします。

    ![テンプレートを選択する](single-page-images/vsmac/choose-template.png)

3. **[Configure your Blank Forms app]\(空白フォームのアプリの構成\)** ダイアログで、新しいアプリに **Notes** という名前を付け、 **[.NET Standard を使用する]** ラジオ ボタンがオンになっていることを確認し、 **[次へ]** ボタンをクリックします。    

    ![Forms アプリケーションを構成する](single-page-images/vsmac/configure-app.png)

4. **[Configure your new Blank Forms app]\(新しい空白フォームのアプリの構成\)** ダイアログでは、ソリューションとプロジェクトの名前は **Notes** に設定したままにし、プロジェクトに適切な場所を選択し、 **[作成]** をクリックしてプロジェクトを作成します。

    ![Forms プロジェクトを構成する](single-page-images/vsmac/configure-project.png)

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、ソリューションとプロジェクトの両方の名前が **Notes** である必要があります。 別の名前を使用すると、コードをこのクイック スタートからプロジェクトにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](deepdive.md) の「[Xamarin.Forms アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)」を参照してください。

5. **Solution Pad** の **Notes** プロジェクトで、 **[MainPage.xaml]** をダブルクリックして開きます。

    ![MainPage.xaml](single-page-images/vsmac/mainpage-xaml.png)

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

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキストを表示する [`Label`](xref:Xamarin.Forms.Label)、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor)、およびファイルの保存または削除をアプリケーションに指示する 2 つの [`Button`](xref:Xamarin.Forms.Button) インスタンスで構成されます。 この 2 つの `Button` インスタンスは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) に垂直に配置されている `Label`、`Editor`、`Grid` と共に、[`Grid`](xref:Xamarin.Forms.Grid) に水平に配置されます。 ユーザー インターフェイスの作成の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad** の **Notes** プロジェクトで、 **[MainPage.xaml]** を展開し、 **[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![MainPage.xaml.cs](single-page-images/vsmac/mainpage-xaml-cs.png)

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

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio for Mac で、 **[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 プロジェクトがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

      ![ビルドに成功しました](single-page-images/vsmac/build-successful.png)

    エラーがある場合は、プロジェクトが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. **Solution Pad** で **[Notes.iOS]** プロジェクトを選択して右クリックし、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![iOS をスタートアップ プロジェクトとして設定](single-page-images/vsmac/set-startup-project-ios.png)

3. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した iOS シミュレーター内でアプリケーションを起動します。

      ![Visual Studio for Mac ツール バー](single-page-images/vsmac/start.png)

      [![iOS シミュレーターでの Notes](single-page-images/vsmac/notes-ios.png)](single-page-images/vsmac/notes-ios-large.png#lightbox "iOS シミュレーターでの Notes")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)」を参照してください。

4. **Solution Pad** で **[Notes.Droid]** プロジェクトを選択して右クリックし、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![Android をスタートアップ プロジェクトとして設定](single-page-images/vsmac/set-startup-project-android.png)

5. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーター内でアプリケーションを起動します。

      [![Android Emulator での Notes](single-page-images/vsmac/notes-android.png)](single-page-images/vsmac/notes-android-large.png#lightbox "Android シミュレーターでの Notes")

    メモを入力して **[保存]** ボタンを押します。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launching-the-application-on-each-platform)」を参照してください。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- クロスプラットフォーム Xamarin.Forms インターフェイスを作成する。
- Extensible Application Markup Language (XAML) を使用して、ページのユーザー インターフェイスを定義する。
- コードから XAML ユーザー インターフェイス要素を操作する。

この単一ページ アプリケーションを複数ページ アプリケーションにするには、次のクイックスタートに進んでください。

> [!div class="nextstepaction"]
> [次へ](multi-page.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)
- [Xamarin.Forms クイックスタート Deep Dive](deepdive.md)