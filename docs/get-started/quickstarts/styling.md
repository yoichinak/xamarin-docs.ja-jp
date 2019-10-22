---
title: クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する
description: この記事では、XAML スタイルを使用してクロスプラットフォームの Xamarin. フォームアプリケーションのスタイルを適用する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 688b0e87bb6281923d3099c0d269b1c2554b6c7a
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70756756"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>クロスプラットフォームの Xamarin. フォームアプリケーションのスタイルを適用する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

このクイックスタートでは、次の方法について説明します。

- XAML スタイルを使用して Xamarin アプリケーションのスタイルを適用します。

クイックスタートでは、XAML スタイルを使用してクロスプラットフォームの Xamarin. フォームアプリケーションのスタイルを適用する方法を説明します。 最終的なアプリケーションは、次のとおりです。

[![](styling-images/screenshots1-sml.png "Notes Page")](styling-images/screenshots1.png#lightbox "Notes Page")
[![](styling-images/screenshots2-sml.png "Note Entry Page")](styling-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>必要条件

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

    このコードは、 [`Thickness`](xref:Xamarin.Forms.Thickness)値、一連の[`Color`](xref:Xamarin.Forms.Color)値、および[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)と[`ContentPage`](xref:Xamarin.Forms.ContentPage)の暗黙的なスタイルを定義します。 アプリケーションレベルの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)にあるこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

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

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)の暗黙的なスタイルをページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、`ListView.Margin` プロパティをアプリケーションレベルの `ResourceDictionary` で定義されている値に設定します。 @No__t_0 暗黙的スタイルは、`NotesPage` によってのみ使用されるため、ページレベル `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

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

    このコードは、 [`Editor`](xref:Xamarin.Forms.Editor)および[`Button`](xref:Xamarin.Forms.Button)ビューの暗黙的なスタイルをページレベルの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、`StackLayout.Margin` プロパティをアプリケーションレベルの `ResourceDictionary` で定義されている値に設定します。 @No__t_0 および `Button` の暗黙的なスタイルは、`NoteEntryPage` によってのみ使用されるため、ページレベルの `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **CTRL + S**キーを押して**NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

5. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、[ **+** ] ボタンをクリックして**NoteEntryPage**に移動し、メモを入力します。 各ページで、前のクイックスタートでスタイルがどのように変更されたかを確認します。

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

    このコードは、 [`Thickness`](xref:Xamarin.Forms.Thickness)値、一連の[`Color`](xref:Xamarin.Forms.Color)値、および[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)と[`ContentPage`](xref:Xamarin.Forms.ContentPage)の暗黙的なスタイルを定義します。 アプリケーションレベルの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)にあるこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

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

    このコードは、 [`ListView`](xref:Xamarin.Forms.ListView)の暗黙的なスタイルをページレベル[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、`ListView.Margin` プロパティをアプリケーションレベルの `ResourceDictionary` で定義されている値に設定します。 @No__t_0 暗黙的スタイルは、`NotesPage` によってのみ使用されるため、ページレベル `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

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

    このコードは、 [`Editor`](xref:Xamarin.Forms.Editor)および[`Button`](xref:Xamarin.Forms.Button)ビューの暗黙的なスタイルをページレベルの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加し、`StackLayout.Margin` プロパティをアプリケーションレベルの `ResourceDictionary` で定義されている値に設定します。 @No__t_0 および `Button` の暗黙的なスタイルは、`NoteEntryPage` によってのみ使用されるため、ページレベルの `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「 [Xamarin のクイックスタート](deepdive.md)」の「[スタイル](deepdive.md#styling)処理」を参照してください。

    **[ファイル > 保存]** を選択し (または、  **&#8984; + S**キーを押して)、 **NoteEntryPage**への変更内容を保存し、ファイルを閉じます。

5. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    [**ノート] ページ**で、[ **+** ] ボタンをクリックして**NoteEntryPage**に移動し、メモを入力します。 各ページで、前のクイックスタートでスタイルがどのように変更されたかを確認します。

::: zone-end

## <a name="next-steps"></a>次のステップ

このクイックスタートでは、次の方法について学習しました。

- XAML スタイルを使用して Xamarin アプリケーションのスタイルを適用します。

Xamarin. Forms を使用したアプリケーション開発の基礎について詳しくは、クイックスタートの詳細に関するページをご覧ください。

> [!div class="nextstepaction"]
> [次へ](deepdive.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin. フォームのクイックスタートの詳細](deepdive.md)
