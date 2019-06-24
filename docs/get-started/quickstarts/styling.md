---
title: クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する
description: この記事では、XAML スタイルを使用してクロスプラット フォーム-Xamarin.Forms アプリケーションのスタイルを設定する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 53d2a0aae3398ea31676fbc3dab95d500dfe2104
ms.sourcegitcommit: e45f0cd6d7d4a77dba5ecaad4d7894025005a2dc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67309501"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>スタイルのクロスプラット フォーム-Xamarin.Forms アプリケーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Styled/)

このクイック スタートでは、学習する方法。

- XAML スタイルを使用した Xamarin.Forms アプリケーションのスタイルを設定します。

このクイック スタート XAML スタイルを使用してクロスプラット フォーム-Xamarin.Forms アプリケーションのスタイルを設定する方法について説明します。 最終的なアプリケーションは、次のとおりです。

[![](styling-images/screenshots1-sml.png "ページをノート")](styling-images/screenshots1.png#lightbox "ページをノート")
[![](styling-images/screenshots2-sml.png "エントリ ページに注意してください")](styling-images/screenshots2.png#lightbox "に注意してくださいエントリ ページ")

### <a name="prerequisites"></a>必須コンポーネント

正常に完了する必要があります、[前のクイック スタート](database.md)このクイック スタートを試行する前にします。 または、ダウンロード、[前のクイック スタート サンプル](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)し、このクイック スタートの開始点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、ノートのソリューションを開きます。

2. **ソリューション エクスプ ローラー**の**ノート**プロジェクトで、ダブルクリックして**App.xaml**を開きます。 既存のコードを次のコードに置き換えます。

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

    このコードを定義、 [ `Thickness` ](xref:Xamarin.Forms.Thickness)値、一連の[ `Color` ](xref:Xamarin.Forms.Color)値、および暗黙的なスタイルを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)と[ `ContentPage`](xref:Xamarin.Forms.ContentPage). アプリケーション レベルでは、これらをスタイル設定、注[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、アプリケーション全体で使用できます。 XAML スタイルの詳細については、次を参照してください。[スタイル](deepdive.md#styling)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    変更を保存**App.xaml**キーを押して**CTRL + S**ファイルを閉じます。

3. **ソリューション エクスプ ローラー**の**ノート**プロジェクトで、ダブルクリックして**NotesPage.xaml**を開きます。 既存のコードを次のコードに置き換えます。

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

    このコードの暗黙的なスタイルの追加、 [ `ListView` ](xref:Xamarin.Forms.ListView)ページごとに[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、設定と、`ListView.Margin`プロパティをアプリケーション レベルで定義されている値に`ResourceDictionary`. なお、`ListView`ページ レベルに追加された暗黙的なスタイル`ResourceDictionary`はのみによって使用されるため、`NotesPage`します。 XAML スタイルの詳細については、次を参照してください。[スタイル](deepdive.md#styling)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    変更を保存**NotesPage.xaml**キーを押して**CTRL + S**ファイルを閉じます。

5. **ソリューション エクスプ ローラー**の**ノート**プロジェクトで、ダブルクリックして**NoteEntryPage.xaml**を開きます。 既存のコードを次のコードに置き換えます。

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

    このコードは、暗黙的なスタイルを追加、 [ `Editor` ](xref:Xamarin.Forms.Editor)と[ `Button` ](xref:Xamarin.Forms.Button)ページ レベルのビュー [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、設定と、 `StackLayout.Margin`プロパティをアプリケーション レベルで定義されている値に`ResourceDictionary`します。 なお、`Editor`と`Button`暗黙的スタイルは、ページごとに追加された`ResourceDictionary`はのみによって使用されるため、`NoteEntryPage`します。 XAML スタイルの詳細については、次を参照してください。[スタイル](deepdive.md#styling)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    変更を保存**NoteEntryPage.xaml**キーを押して**CTRL + S**ファイルを閉じます。

6. 構築し、各プラットフォームで、プロジェクトを実行します。 詳細については、次を参照してください。[クイック スタートを構築](single-page.md#building-the-quickstart)します。

    **NotesPage**キーを押して、 **+** に移動するボタン、 **NoteEntryPage**メモを入力します。 各ページで、前のクイック スタートからのスタイルがどのように変更されたかを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac 起動し、ノートのプロジェクトを開きます。

2. **Solution Pad**の**ノート**プロジェクトで、ダブルクリックして**App.xaml**を開きます。 既存のコードを次のコードに置き換えます。

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

    このコードを定義、 [ `Thickness` ](xref:Xamarin.Forms.Thickness)値、一連の[ `Color` ](xref:Xamarin.Forms.Color)値、および暗黙的なスタイルを[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)と[ `ContentPage`](xref:Xamarin.Forms.ContentPage). アプリケーション レベルでは、これらをスタイル設定、注[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、アプリケーション全体で使用できます。 XAML スタイルの詳細については、次を参照してください。[スタイル](deepdive.md#styling)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    変更を保存**App.xaml**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

3. **Solution Pad**の**ノート**プロジェクトで、ダブルクリックして**NotesPage.xaml**を開きます。 既存のコードを次のコードに置き換えます。

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

    このコードの暗黙的なスタイルの追加、 [ `ListView` ](xref:Xamarin.Forms.ListView)ページごとに[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、設定と、`ListView.Margin`プロパティをアプリケーション レベルで定義されている値に`ResourceDictionary`. なお、`ListView`ページ レベルに追加された暗黙的なスタイル`ResourceDictionary`はのみによって使用されるため、`NotesPage`します。 XAML スタイルの詳細については、次を参照してください。[スタイル](deepdive.md#styling)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    変更を保存**NotesPage.xaml**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

5. **Solution Pad**の**ノート**プロジェクトで、ダブルクリックして**NoteEntryPage.xaml**を開きます。 既存のコードを次のコードに置き換えます。

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

    このコードは、暗黙的なスタイルを追加、 [ `Editor` ](xref:Xamarin.Forms.Editor)と[ `Button` ](xref:Xamarin.Forms.Button)ページ レベルのビュー [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、設定と、 `StackLayout.Margin`プロパティをアプリケーション レベルで定義されている値に`ResourceDictionary`します。 なお、`Editor`と`Button`暗黙的スタイルは、ページごとに追加された`ResourceDictionary`はのみによって使用されるため、`NoteEntryPage`します。 XAML スタイルの詳細については、次を参照してください。[スタイル](deepdive.md#styling)で、 [Xamarin.Forms クイック スタートの Deep Dive](deepdive.md)します。

    変更を保存**NoteEntryPage.xaml**を選択して**ファイル > 保存**(またはキーを押して **&#8984; + S**)、ファイルを閉じます。

6. 構築し、各プラットフォームで、プロジェクトを実行します。 詳細については、次を参照してください。[クイック スタートを構築](single-page.md#building-the-quickstart)します。

    **NotesPage**キーを押して、 **+** に移動するボタン、 **NoteEntryPage**メモを入力します。 各ページで、前のクイック スタートからのスタイルがどのように変更されたかを確認します。

::: zone-end


## <a name="next-steps"></a>次の手順

このクイック スタートでは説明した方法。

- XAML スタイルを使用した Xamarin.Forms アプリケーションのスタイルを設定します。

Xamarin.Forms を使用したアプリケーション開発の基礎に関する詳細については、クイック スタートの詳細に進んでください。

> [!div class="nextstepaction"]
> [次へ](deepdive.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Styled/)
- [Xamarin.Forms のクイック スタートの詳細情報](deepdive.md)
