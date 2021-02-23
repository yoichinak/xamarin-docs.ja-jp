---
title: Xamarin.Forms アプリケーションの作成クイックスタート
description: この記事では、Xamarin.Forms シェル アプリケーションを作成する方法について説明します。ほとんどのモバイル アプリケーションで必要な基本機能が提供されることで、モバイル アプリケーション開発の複雑さが軽減されます。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 359F5012-4409-42A7-BEDF-C1DB9AF86DED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/26/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4d21daa89322021e80a46753c864da3cb671b2d6
ms.sourcegitcommit: 1f391667869a4541dd9b42d78862dc01d69ed160
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2021
ms.locfileid: "99818630"
---
# <a name="create-a-xamarinforms-application-quickstart"></a>Xamarin.Forms アプリケーションの作成クイックスタート

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/getstarted-notes-app/)

このクイックスタートでは、次の方法について学習します。

- Xamarin.Forms シェル アプリケーションを作成する。
- eXtensible Application Markup Language (XAML) を使用してページのユーザー インターフェイスを定義し、コードから XAML 要素を操作する。
- `Shell` クラスをサブクラス化することによって、シェル アプリケーションのビジュアル階層を記述する。

このクイックスタートでは、クロスプラットフォームの Xamarin.Forms シェル アプリケーションを作成する方法について説明します。このアプリにより、メモを入力し、デバイス ストレージに保持できるようになります。 最終的なアプリケーションは、次のとおりです。

[![Notes アプリケーション](app-images/screenshots1-sml.png)](app-images/screenshots1.png#lightbox)
[![Notes の About ページ](app-images/screenshots2-sml.png)](app-images/screenshots2.png#lightbox)

::: zone pivot="windows"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio 2019 (最新リリース) と、 **.NET によるモバイル開発** ワークロードがインストールされている。
- C# に関する知識。
- (必要に応じて) iOS 上でアプリケーションをビルドするためのペアリング済みの Mac。

これらの前提条件の詳細については、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

## <a name="get-started-with-visual-studio-2019"></a>Visual Studio 2019 の使用を開始する

1. Visual Studio 2019 を起動し、スタート ウィンドウで **[新しいプロジェクトを作成する]** をクリックして新しいプロジェクトを作成します。

    ![新しいソリューション](app-images/vs/new-solution.png)

2. **[新しいプロジェクトの作成]** ウィンドウの **[プロジェクト タイプ]** ドロップダウンで **[モバイル]** を選択し、 **[モバイル アプリ (Xamarin.Forms)]** テンプレートを選択して、 **[次へ]** ボタンをクリックします。

    ![テンプレートを選択する](app-images/vs/new-project.png)

3. **[新しいプロジェクトを構成します]** ウィンドウで、 **[プロジェクト名]** を **[Notes]** に設定し、プロジェクトに適切な場所を選択し、 **[作成]** ボタンをクリックします。   

    ![シェル アプリケーションを構成する](app-images/vs/configure-project.png)

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、ソリューションとプロジェクトの両方の名前が **Notes** である必要があります。 別の名前を使用すると、コードをこのクイック スタートからプロジェクトにコピーするときに、ビルド エラーが発生します。

4. **[新しいモバイル アプリ]** ダイアログで、 **[タブ付き]** テンプレートを選択し、 **[作成]** ボタンをクリックします。

    ![シェル アプリケーションを作成する](app-images/vs/create-project.png)

    プロジェクトが作成されたら、**GettingStarted.txt** ファイルを閉じます。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms シェル クイックスタート Deep Dive](deepdive.md) の [Xamarin.Forms シェル アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)に関するセクションを参照してください。

5. **ソリューション エクスプローラー** で、**Notes** プロジェクトの次のフォルダー (およびその内容) を削除します。

    - **モデル**
    - **サービス**
    - **ViewModel**
    - **ビュー**

6. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **GettingStarted.txt** を削除します。

7. **ソリューション エクスプローラー** の **Notes** プロジェクトで、**Views** という名前の新しいフォルダーを追加します。

8. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **[Views]** フォルダーを選択して右クリックし、 **[追加] > [新しい項目...]** を選択します。 **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム] > [Xamarin.Forms] > [コンテンツ ページ]** を選択し、新しいファイルに **NotesPage** という名前を付け、 **[追加]** ボタンをクリックします。

    ![NotesPage を追加する](app-images/vs/add-notespage.png)

    これにより、**NotesPage** という名前の新しいページがプロジェクトの **[Views]** フォルダーに追加されます。 このページは、アプリケーションのメイン ページになります。

9. **ソリューション エクスプローラー** の **Notes** プロジェクトで、**NotesPage.xaml** をダブルクリックして開きます。

    ![NotesPage.xaml を開く](app-images/vs/open-notespage-xaml.png)

10. **NotesPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Layout children vertically -->
        <StackLayout Margin="20">
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <!-- Layout children in two columns -->
            <Grid ColumnDefinitions="*,*">
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor) と、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) オブジェクトで構成されます。 2 つの `Button` オブジェクトは [`Grid`](xref:Xamarin.Forms.Grid) 内に水平に配置され、`Editor` と `Grid` は [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内に垂直に配置されます。 ユーザー インターフェイスの作成の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **Ctrl + S** キーを押して **NotesPage.xaml** への変更を保存します。  

12. **ソリューション エクスプローラー** の **Notes** プロジェクトで、**NotesPage.xaml.cs** をダブルクリックして開きます。

    ![NotesPage.xaml.cs を開く](app-images/vs/open-notespage-codebehind.png)

12. **NotesPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class NotesPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public NotesPage()
            {
                InitializeComponent();

                // Read the file.
                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                // Save the file.
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                // Delete the file.
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **Ctrl + S** キーを押して **NotesPage.xaml.cs** への変更を保存します。    

13. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **[Views]** フォルダーを選択して右クリックし、 **[追加] > [新しい項目...]** を選択します。 **[新しい項目の追加]** ダイアログで、 **[Visual C# アイテム] > [Xamarin.Forms] > [コンテンツ ページ]** を選択し、新しいファイルに **AboutPage** という名前を付け、 **[追加]** ボタンをクリックします。

    ![AboutPage を追加する](app-images/vs/add-aboutpage.png)

    これにより、**AboutPage** という名前の新しいページがプロジェクトの **[Views]** フォルダーに追加されます。

14. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **AboutPage.xaml** をダブルクリックして開きます。

    ![AboutPage.xaml を開く](app-images/vs/open-aboutpage-xaml.png)

15. **AboutPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{OnPlatform iOS=LightSlateGray, Android=#2196F3}"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="20"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    このコードでは、[`Image`](xref:Xamarin.Forms.Image)、テキストが表示される 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクト、[`Button`](xref:Xamarin.Forms.Button) で構成される、ページのユーザー インターフェイスが宣言によって定義されます。 2 つの `Label` オブジェクトと `Button` は [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内に垂直に配置され、`Image` と `StackLayout` は [`Grid`](xref:Xamarin.Forms.Grid) 内に垂直に配置されます。 ユーザー インターフェイスの作成の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **Ctrl + S** キーを押して **AboutPage.xaml** への変更を保存します。    

16. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **AboutPage.xaml.cs** をダブルクリックして開きます。

    ![AboutPage.xaml.cs を開く](app-images/vs/open-aboutpage-codebehind.png)

17. **AboutPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using Xamarin.Essentials;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class AboutPage : ContentPage
        {
            public AboutPage()
            {
                InitializeComponent();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                // Launch the specified URL in the system browser.
                await Launcher.OpenAsync("https://aka.ms/xamarin-quickstart");
            }
        }
    }
    ```

    このコードで定義されている `OnButtonClicked` イベント ハンドラーは、 **[Learn more]** [`Button`](xref:Xamarin.Forms.Button) が押されると実行されます。 このボタンを押すと、Web ブラウザーが起動され、`OpenAsync` メソッドへの URI 引数によって表されるページが表示されます。 ユーザーの操作の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **Ctrl + S** キーを押して **AboutPage.xaml.cs** への変更を保存します。

18. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **AppShell.xaml** をダブルクリックして開きます。

    ![AppShell.xaml を開く](app-images/vs/open-appshell-xaml.png)

19. **AppShell.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">
        <!-- Display a bottom tab bar containing two tabs -->   
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```    

    このコードで宣言によって定義されているアプリケーションのビジュアル階層は、2 つの `ShellContent` オブジェクトを含む `TabBar`で構成されています。 これらのオブジェクトによって表されているのは、ユーザー インターフェイス要素ではなく、アプリケーションのビジュアル階層の編成です。 シェルによってこれらのオブジェクトが取得され、コンテンツ用のユーザー インターフェイスが生成されます。 ユーザー インターフェイスの作成の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **Ctrl + S** キーを押して **AppShell.xaml** への変更を保存します。    

20. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **AppShell.xaml** を展開し、**AppShell.xaml.cs** をダブルクリックして開きます。

    ![AppShell.xaml.cs を開く](app-images/vs/open-appshell-codebehind.png)

21. **AppShell.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
            }
        }
    }
    ```

    **Ctrl + S** キーを押して **AppShell.xaml.cs** への変更を保存します。

22. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **App.xaml** をダブルクリックして開きます。

    ![App.xaml を開く](app-images/vs/open-app-xaml.png)

23. **App.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

    </Application>
    ```

    このコードで宣言によって定義されている `App` クラスにより、アプリケーションのインスタンス化が行われます。

    **Ctrl + S** キーを押して **App.xaml** への変更を保存します。    

24. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **App.xaml** を展開し、**App.xaml.cs** をダブルクリックして開きます。

    ![App.xaml.cs を開く](app-images/vs/open-app-codebehind.png)

25. **App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {

            public App()
            {
                InitializeComponent();
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    このコードで定義されている `App` クラスのコードビハインドにより、アプリケーションのインスタンス化が行われます。 それによって、[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティがサブクラス化された `Shell` オブジェクトに初期化されます。

    **Ctrl + S** キーを押して **App.xaml.cs** への変更を保存します。    

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio で、 **[ビルド]、[ソリューションのビルド]** メニュー項目の順に選択します (または F6 キーを押します)。 ソリューションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

      ![ビルドに成功しました](app-images/vs/build-successful.png)

    エラーがある場合は、プロジェクトが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーターでアプリケーションを起動します。

      ![Visual Studio Android ツール バー](app-images/vs/android-start.png)

      ![Android Emulator での Notes](app-images/vs/notes1-android.png)

    メモを入力して **[保存]** ボタンを押します。 次に、アプリケーションを閉じて再起動し、入力したメモが再度読み込まれることを確認します。

    **[About]** タブ アイコンを押して、`AboutPage` に移動します。

      ![Android Emulator での Notes の About ページ](app-images/vs/notes2-android.png)

    **[Learn more]** ボタンをクリックすると、クイックスタートの Web ページが開きます。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launch-the-application-on-each-platform)」を参照してください。

    > [!NOTE]
    > 次の手順は、[ 開発のシステム要件を満たしている、](~/ios/get-started/installation/windows/connecting-to-mac/index.md)ペアリングされた MacXamarin.Forms がある場合にのみ実行する必要があります。    

3. Visual Studio ツール バーで、 **[Notes.iOS]** プロジェクトを右クリックして、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![Notes.iOS をスタートアップ プロジェクトとして設定する](app-images/vs/set-startup-project-ios.png)

4. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)でアプリケーションを起動します。

      ![Visual Studio iOS ツール バー](app-images/vs/ios-start.png)

      [![iOS シミュレーターでの Notes](app-images/vs/notes1-ios.png)](app-images/vs/notes1-ios-large.png#lightbox)

    メモを入力して **[保存]** ボタンを押します。 次に、アプリケーションを閉じて再起動し、入力したメモが再度読み込まれることを確認します。

    **[About]** タブ アイコンを押して、`AboutPage` に移動します。

      [![iOS シミュレーターでの Notes の About ページ](app-images/vs/notes2-ios.png)](app-images/vs/notes2-ios-large.png#lightbox)

    **[Learn more]** ボタンをクリックすると、クイックスタートの Web ページが開きます。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launch-the-application-on-each-platform)」を参照してください。

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio for Mac (最新リリース) と、iOS と Android プラットフォームのサポートがインストールされている。
- Xcode (最新リリース)。
- C# に関する知識。

これらの前提条件の詳細については、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

## <a name="get-started-with-visual-studio-for-mac"></a>Visual Studio for Mac の概要

1. Visual Studio for Mac を起動し、スタート ウィンドウで **[新規]** をクリックして新しいプロジェクトを作成します。

    ![新しいソリューション](app-images/vsmac/new-project.png)

2. **[新しいプロジェクト用のテンプレートを選択する]** ダイアログで、 **[マルチプラットフォーム] > [アプリ]** の順にクリックし、 **[Shell Forms App]\(シェル フォーム アプリ\)** テンプレートを選択して、 **[次へ]** ボタンをクリックします。

    ![テンプレートを選択する](app-images/vsmac/choose-template.png)

3. **[Configure your Shell Forms app]\(シェル フォーム アプリの構成\)** ダイアログで、新しいアプリに **Notes** という名前を設定して、 **[次へ]** ボタンをクリックします。    

    ![シェル アプリケーションを構成する](app-images/vsmac/configure-app.png)

4. **[Configure your new Shell Forms app]\(新しいシェル フォーム アプリの構成\)** ダイアログでは、ソリューションとプロジェクトの名前は **Notes** に設定したままにし、プロジェクトに適切な場所を選択し、 **[作成]** をクリックしてプロジェクトを作成します。

    ![シェル プロジェクトを構成する](app-images/vsmac/configure-project.png)

    > [!IMPORTANT]
    > このクイックスタートの C# スニペットと XAML スニペットでは、ソリューションとプロジェクトの両方の名前が **Notes** である必要があります。 別の名前を使用すると、コードをこのクイック スタートからプロジェクトにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms シェル クイックスタート Deep Dive](deepdive.md) の [Xamarin.Forms シェル アプリケーションの構造](deepdive.md#anatomy-of-a-xamarinforms-application)に関するセクションを参照してください。

5. **Solution Pad** で、**Notes** プロジェクトの次のフォルダー (およびその内容) を削除します。

    - **モデル**
    - **サービス**
    - **ViewModel**
    - **ビュー**

6. **Solution Pad** で、**Notes** プロジェクトの **GettingStarted.txt** を削除します。

7. **Solution Pad** の **Notes** プロジェクトで、**Views** という名前の新しいフォルダーを追加します。

8. **Solution Pad** で、**Notes** プロジェクトの **[Views]** フォルダーを選択して右クリックし、 **[追加] > [新しいファイル...]** を選択します。 **[新しいファイル]** ダイアログで、 **[フォーム] > [フォーム ContentPage XAML]** の順に選択して、新しいファイルに **NotesPage** という名前を付け、 **[新規]** ボタンをクリックします。

    ![NotesPage を追加する](app-images/vsmac/add-notespage.png)

    これにより、**NotesPage** という名前の新しいページがプロジェクトの **[Views]** フォルダーに追加されます。 このページは、アプリケーションのメイン ページになります。

9. **Solution Pad** で **Notes** プロジェクトの **NotesPage.xaml** をダブルクリックして開きます。

    ![NotesPage.xaml を開く](app-images/vsmac/open-notespage-xaml.png)

10. **NotesPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">
        <!-- Layout children vertically -->
        <StackLayout Margin="20">
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <!-- Layout children in two columns -->
            <Grid ColumnDefinitions="*,*">
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、ページにユーザー インターフェイスが宣言的に定義されます。これは、テキスト入力用の [`Editor`](xref:Xamarin.Forms.Editor) と、アプリケーションへのファイルの保存または削除の指示が出される 2 つの [`Button`](xref:Xamarin.Forms.Button) オブジェクトで構成されます。 2 つの `Button` オブジェクトは [`Grid`](xref:Xamarin.Forms.Grid) 内に水平に配置され、`Editor` と `Grid` は [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内に垂直に配置されます。 ユーザー インターフェイスの作成の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NotesPage.xaml** への変更を保存します。

12. **Solution Pad** で **Notes** プロジェクトの **NotesPage.xaml.cs** をダブルクリックして開きます。

    ![NotesPage.xaml.cs を開く](app-images/vsmac/open-notespage-codebehind.png)

12. **NotesPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class NotesPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public NotesPage()
            {
                InitializeComponent();

                // Read the file.
                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                // Save the file.
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                // Delete the file.
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    このコードでは、`notes.txt` という名前のファイルを参照する `_fileName` フィールドを定義します。このファイルで、アプリケーション用のローカル アプリケーション データ フォルダーにメモ データが保存されます。 ページ コンストラクターが実行されると、ファイルが存在する場合は読み取られ、[`Editor`](xref:Xamarin.Forms.Editor) に表示されます。 **[保存]** [`Button`](xref:Xamarin.Forms.Button) が押されると、`OnSaveButtonClicked` イベント ハンドラーが実行されます。これにより、`Editor` のコンテンツがファイルに保存されます。 **[削除]** `Button` が押されると、`OnDeleteButtonClicked` イベント ハンドラーが実行されます。これにより、ファイルが存在する場合は削除され、`Editor` から任意のテキストが削除されます。 ユーザーの操作の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NotesPage.xaml.cs** への変更を保存します。

13. **Solution Pad** で、**Notes** プロジェクトの **[Views]** フォルダーを選択して右クリックし、 **[追加] > [新しいファイル...]** を選択します。 **[新しいファイル]** ダイアログで、 **[フォーム] > [フォーム ContentPage XAML]** の順に選択して、新しいファイルに **AboutPage** という名前を付け、 **[新規]** ボタンをクリックします。

    ![AboutPage を追加する](app-images/vsmac/add-aboutpage.png)

14. **Solution Pad** で **Notes** プロジェクトの **AboutPage.xaml** をダブルクリックして開きます。

    ![AboutPage.xaml を開く](app-images/vsmac/open-aboutpage-xaml.png)

    これにより、**AboutPage** という名前の新しいページがプロジェクトの **[Views]** フォルダーに追加されます。

15. **AboutPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{OnPlatform iOS=LightSlateGray, Android=#2196F3}"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="20"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    このコードでは、[`Image`](xref:Xamarin.Forms.Image)、テキストが表示される 2 つの [`Label`](xref:Xamarin.Forms.Label) オブジェクト、[`Button`](xref:Xamarin.Forms.Button) で構成される、ページのユーザー インターフェイスが宣言によって定義されます。 2 つの `Label` オブジェクトと `Button` は [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内に垂直に配置され、`Image` と `StackLayout` は [`Grid`](xref:Xamarin.Forms.Grid) 内に垂直に配置されます。 ユーザー インターフェイスの作成の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AboutPage.xaml** への変更を保存します。

16. **Solution Pad** で **Notes** プロジェクトの **AboutPage.xaml.cs** をダブルクリックして開きます。

    ![AboutPage.xaml.cs を開く](app-images/vsmac/open-aboutpage-codebehind.png)

17. **AboutPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using Xamarin.Essentials;
    using Xamarin.Forms;

    namespace Notes.Views
    {
        public partial class AboutPage : ContentPage
        {
            public AboutPage()
            {
                InitializeComponent();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                // Launch the specified URL in the system browser.
                await Launcher.OpenAsync("https://aka.ms/xamarin-quickstart");
            }
        }
    }
    ```

    このコードで定義されている `OnButtonClicked` イベント ハンドラーは、 **[Learn more]** [`Button`](xref:Xamarin.Forms.Button) が押されると実行されます。 このボタンを押すと、Web ブラウザーが起動され、`OpenAsync` メソッドへの URI 引数によって表されるページが表示されます。 ユーザーの操作の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー操作に対する応答](deepdive.md#responding-to-user-interaction)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AboutPage.xaml.cs** への変更を保存します。

18. **Solution Pad** で **Notes** プロジェクトの **AppShell.xaml** をダブルクリックして開きます。

    ![AppShell.xaml を開く](app-images/vsmac/open-appshell-xaml.png)

19. **AppShell.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">
        <!-- Display a bottom tab bar containing two tabs -->
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```    

    このコードで宣言によって定義されているアプリケーションのビジュアル階層は、2 つの `ShellContent` オブジェクトを含む `TabBar`で構成されています。 これらのオブジェクトによって表されているのは、ユーザー インターフェイス要素ではなく、アプリケーションのビジュアル階層の編成です。 シェルによってこれらのオブジェクトが取得され、コンテンツ用のユーザー インターフェイスが生成されます。 ユーザー インターフェイスの作成の詳細については、[Xamarin.Forms シェル クイック スタート Deep Dive](deepdive.md) に関するページの「[ユーザー インターフェイス](deepdive.md#user-interface)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AppShell.xaml** への変更を保存します。

20. **Solution Pad** で、**Notes** プロジェクトの **AppShell.xaml** を展開し、**AppShell.xaml.cs** をダブルクリックして開きます。

    ![AppShell.xaml.cs を開く](app-images/vsmac/open-appshell-codebehind.png)

21. **AppShell.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class AppShell : Shell
        {
            public AppShell()
            {
                InitializeComponent();
            }
        }
    }
    ```

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AppShell.xaml.cs** への変更を保存します。

22. **Solution Pad** で **Notes** プロジェクトの **App.xaml** をダブルクリックして開きます。

    ![App.xaml を開く](app-images/vsmac/open-app-xaml.png)

23. **App.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

    </Application>
    ```

    このコードで宣言によって定義されている `App` クラスにより、アプリケーションのインスタンス化が行われます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**App.xaml** への変更を保存します。

24. **Solution Pad** で、**Notes** プロジェクトの **App.xaml** を展開し、**App.xaml.cs** をダブルクリックして開きます。

    ![App.xaml.cs を開く](app-images/vsmac/open-app-codebehind.png)

25. **App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {

            public App()
            {
                InitializeComponent();
                MainPage = new AppShell();
            }

            protected override void OnStart()
            {
            }

            protected override void OnSleep()
            {
            }

            protected override void OnResume()
            {
            }
        }
    }
    ```

    このコードで定義されている `App` クラスのコードビハインドにより、アプリケーションのインスタンス化が行われます。 それによって、[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティがサブクラス化された `Shell` オブジェクトに初期化されます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**App.xaml.cs** への変更を保存します。

### <a name="building-the-quickstart"></a>クイック スタートのビルド

1. Visual Studio for Mac で、 **[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 プロジェクトがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

      ![ビルドに成功しました](app-images/vsmac/build-successful.png)

    エラーがある場合は、プロジェクトが正常にビルドされるまで、前の手順を繰り返して誤りを修正します。

2. **Solution Pad** で **[Notes.iOS]** プロジェクトを選択して右クリックし、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![iOS をスタートアップ プロジェクトとして設定](app-images/vsmac/set-startup-project-ios.png)

3. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した iOS シミュレーター内でアプリケーションを起動します。

      ![Visual Studio for Mac ツール バー](app-images/vsmac/start.png)

      ![iOS シミュレーターでの Notes](app-images/vsmac/notes1-ios.png)

    メモを入力して **[保存]** ボタンを押します。 次に、アプリケーションを閉じて再起動し、入力したメモが再度読み込まれることを確認します。

    **[About]** タブ アイコンを押して、`AboutPage` に移動します。

      ![iOS シミュレーターでの Notes の About ページ](app-images/vsmac/notes2-ios.png)

    **[Learn more]** ボタンをクリックすると、クイックスタートの Web ページが開きます。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launch-the-application-on-each-platform)」を参照してください。

4. **Solution Pad** で **[Notes.Droid]** プロジェクトを選択して右クリックし、 **[スタートアップ プロジェクトに設定]** を選択します。

      ![Android をスタートアップ プロジェクトとして設定](app-images/vsmac/set-startup-project-android.png)

5. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、選択した Android エミュレーター内でアプリケーションを起動します。

      ![Android Emulator での Notes](app-images/vsmac/notes1-android.png)

    メモを入力して **[保存]** ボタンを押します。 次に、アプリケーションを閉じて再起動し、入力したメモが再度読み込まれることを確認します。

    **[About]** タブ アイコンを押して、`AboutPage` に移動します。

      ![Android Emulator での Notes の About ページ](app-images/vsmac/notes2-android.png)

    **[Learn more]** ボタンをクリックすると、クイックスタートの Web ページが開きます。

    各プラットフォームでアプリケーションを起動する方法の詳細については、「[Xamarin.Forms クイック スタート Deep Dive](deepdive.md)」の「[各プラットフォームでアプリケーションを起動する](deepdive.md#launch-the-application-on-each-platform)」を参照してください。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- Xamarin.Forms シェル アプリケーションを作成する。
- eXtensible Application Markup Language (XAML) を使用してページのユーザー インターフェイスを定義し、コードから XAML 要素を操作する。
- `Shell` クラスをサブクラス化することによって、シェル アプリケーションのビジュアル階層を記述する。

次のクイックスタートに進み、この Xamarin.Forms シェル アプリケーションにさらにページを追加します。

> [!div class="nextstepaction"]
> [次へ](navigation.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-app/)
- [Xamarin.Forms シェル クイックスタート Deep Dive](deepdive.md)
