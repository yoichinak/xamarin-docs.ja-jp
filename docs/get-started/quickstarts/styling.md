---
title: クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する
description: この記事では、XAML スタイルを使用してクロスプラットフォームの Xamarin.Forms アプリケーションのスタイルを適用する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 688b0e87bb6281923d3099c0d269b1c2554b6c7a
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "70756756"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>クロスプラットフォームの Xamarin.Forms アプリケーションのスタイルを適用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

このクイックスタートでは、次の方法について説明します。

- XAML スタイルを使用して Xamarin アプリケーションのスタイルを適用します。

クイックスタートでは、XAML スタイルを使用してクロスプラットフォームの Xamarin.Forms アプリケーションのスタイルを適用する方法を説明します。 最終的なアプリケーションは、次のとおりです。

[![](styling-images/screenshots1-sml.png "メモ")](styling-images/screenshots1.png#lightbox "[メモ] ページ")ページ
[(styling-images/screenshots2-sml.png "メモの入力")ページ![]](styling-images/screenshots2.png#lightbox "メモの入力ページ")

### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試行する前に、[前のクイックスタート](database.md)を正常に完了している必要があります。 または、[前のクイックスタートサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)をダウンロードし、このクイックスタートの出発点として使用してください。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、Note ソリューションを開きます。

2. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **app.xaml**をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    このコードは[`Thickness`](xref:Xamarin.Forms.Thickness) 、値、一連の[`Color`](xref:Xamarin.Forms.Color)値、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)および[`ContentPage`](xref:Xamarin.Forms.ContentPage)の暗黙的なスタイルを定義します。 アプリケーションレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)のこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **CTRL + S**キーを押して**app.xaml**への変更を保存し、ファイルを閉じます。

3. **ソリューションエクスプローラー**の**Notes**プロジェクトで、[注釈 **]** プロジェクトをダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    このコードは[`ListView`](xref:Xamarin.Forms.ListView) 、の暗黙的なスタイルをページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、 `ListView.Margin`プロパティをアプリケーションレベル`ResourceDictionary`で定義された値に設定します。 によって`ListView`のみ使用`NotesPage`されるため、暗黙的なスタイル`ResourceDictionary`はページレベルに追加されていることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **CTRL + S**キーを押して、変更内容を [**ノート] ページ**に保存し、ファイルを閉じます。

4. **ソリューションエクスプローラー**の**Notes**プロジェクトで、 **NoteEntryPage**をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
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
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    このコードは[`Editor`](xref:Xamarin.Forms.Editor) 、ビュー `ResourceDictionary`および[`Button`](xref:Xamarin.Forms.Button)ビューの暗黙的なスタイルをページ`StackLayout.Margin`レベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、プロパティをアプリケーションレベルで定義された値に設定します。 `Editor`と`ResourceDictionary` `NoteEntryPage`の暗黙的なスタイルは、によってのみ使用されるため、ページレベルに追加されていることに注意してください。 `Button` XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **CTRL + S**キーを押して**NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

5. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、 **+** ボタンを押して**NoteEntryPage**に移動し、メモを入力します。 各ページで、前のクイックスタートでスタイルがどのように変更されたかを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動し、Notes プロジェクトを開きます。

2. **Solution Pad**の**Notes**プロジェクトで、 **app.xaml**をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    このコードは[`Thickness`](xref:Xamarin.Forms.Thickness) 、値、一連の[`Color`](xref:Xamarin.Forms.Color)値、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)および[`ContentPage`](xref:Xamarin.Forms.ContentPage)の暗黙的なスタイルを定義します。 アプリケーションレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)のこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **[ファイル > 保存]** を選択して (または **&#8984; + S**キーを押して)、 **app.xaml**への変更内容を保存し、ファイルを閉じます。

3. **Solution Pad**の**Notes**プロジェクトで、[注釈 **]** プロジェクトをダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    このコードは[`ListView`](xref:Xamarin.Forms.ListView) 、の暗黙的なスタイルをページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、 `ListView.Margin`プロパティをアプリケーションレベル`ResourceDictionary`で定義された値に設定します。 によって`ListView`のみ使用`NotesPage`されるため、暗黙的なスタイル`ResourceDictionary`はページレベルに追加されていることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    [ファイル **]** **> [保存**] を選択して (または **&#8984; + S**キーを押して)、ファイルを閉じて、変更内容を保存します。

4. **Solution Pad**の**Notes**プロジェクトで、 **NoteEntryPage**をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
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
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    このコードは[`Editor`](xref:Xamarin.Forms.Editor) 、ビュー `ResourceDictionary`および[`Button`](xref:Xamarin.Forms.Button)ビューの暗黙的なスタイルをページ`StackLayout.Margin`レベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、プロパティをアプリケーションレベルで定義された値に設定します。 `Editor`と`ResourceDictionary` `NoteEntryPage`の暗黙的なスタイルは、によってのみ使用されるため、ページレベルに追加されていることに注意してください。 `Button` XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **[ファイル > 保存]** を選択し (または、  **&#8984; + S**キーを押して)、 **NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

5. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、 **+** ボタンを押して**NoteEntryPage**に移動し、メモを入力します。 各ページで、前のクイックスタートでスタイルがどのように変更されたかを確認します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- XAML スタイルを使用して Xamarin アプリケーションのスタイルを適用します。

Xamarin.Forms を使用したアプリケーション開発の基礎について詳しくは、クイックスタートの詳細に関するページをご覧ください。

> [!div class="nextstepaction"]
> [次へ](deepdive.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin.Forms のクイックスタートの詳細](deepdive.md)
