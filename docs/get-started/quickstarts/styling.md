---
title: クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する
description: この記事では、XAML スタイルで、クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する方法を説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/07/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 099c95af190fc9b43c8e8497eebdeec44aed36cd
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368441"
---
# <a name="style-a-cross-platform-no-locxamarinforms-application"></a>クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

このクイックスタートでは、次の方法について学習します。

- XAML スタイルを使用して Xamarin.Forms アプリケーションのスタイルを設定する。

このクイックスタートでは、XAML スタイルを使用して、クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する方法を説明します。 最終的なアプリケーションは、次のとおりです。

[![Notes ページ](styling-images/screenshots1-sml.png)](styling-images/screenshots1.png#lightbox "Notes ページ")
[![Note Entry ページ](styling-images/screenshots2-sml.png)](styling-images/screenshots2.png#lightbox "Note Entry ページ")

### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試みるには、[前のクイックスタート](database.md)を正常に完了しておく必要があります。 または、[前のクイックスタートのサンプル](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)をダウンロードし、それをこのクイックスタートの出発点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、Notes ソリューションを開きます。

2. **ソリューション エクスプローラー** で **Notes** プロジェクト内の **App.xaml** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    このコードにより、[`Thickness`](xref:Xamarin.Forms.Thickness) 値、一連の [`Color`](xref:Xamarin.Forms.Color) 値、および [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) と [`ContentPage`](xref:Xamarin.Forms.ContentPage) の暗黙的なスタイルが定義されます。 アプリケーション レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) にあるこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **CTRL + S** を押し、 **App.xaml** への変更内容を保存してから、ファイルを閉じます。

3. **ソリューション エクスプローラー** で **Notes** プロジェクト内の **NotesPage.xaml** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    このコードにより、[`ListView`](xref:Xamarin.Forms.ListView) に対する暗黙的なスタイルがページ レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`ListView.Margin` プロパティがアプリケーション レベル `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `ListView` は、`NotesPage` によってのみ使用されるため、ページ レベル `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **Ctrl + S** キーを押して **NotesPage.xaml** への変更内容を保存してから、ファイルを閉じます。

4. **ソリューション エクスプローラー** で **Notes** プロジェクト内の、 **NoteEntryPage.xaml** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
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

    このコードにより、[`Editor`](xref:Xamarin.Forms.Editor) ビューと [`Button`](xref:Xamarin.Forms.Button) ビューに対する暗黙的なスタイルがページ レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`StackLayout.Margin` プロパティがアプリケーション レベル `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `Editor` と `Button` は、`NoteEntryPage` によってのみ使用されるため、ページ レベル `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **Ctrl + S** キーを押して **NoteEntryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

5. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[+]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 各ページで、前のクイックスタートからスタイルがどのように変更されたかを確認します。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動し、Notes プロジェクトを開きます。

2. **Solution Pad** で **Notes** プロジェクト内の **App.xaml** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    このコードにより、[`Thickness`](xref:Xamarin.Forms.Thickness) 値、一連の [`Color`](xref:Xamarin.Forms.Color) 値、および [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) と [`ContentPage`](xref:Xamarin.Forms.ContentPage) の暗黙的なスタイルが定義されます。 アプリケーション レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) にあるこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、 **App.xaml** への変更内容を保存してから、ファイルを閉じます。

3. **Solution Pad** で **Notes** プロジェクト内の **NotesPage.xaml** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    このコードにより、[`ListView`](xref:Xamarin.Forms.ListView) に対する暗黙的なスタイルがページ レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`ListView.Margin` プロパティがアプリケーション レベル `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `ListView` は、`NotesPage` によってのみ使用されるため、ページ レベル `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、 **NotesPage.xaml** への変更内容を保存してから、ファイルを閉じます。

4. **Solution Pad** で **Notes** プロジェクト内の **NoteEntryPage.xaml** をダブルクリックして開きます。 次に、既存のコードを次のコードに置き換えます。

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
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
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

    このコードにより、[`Editor`](xref:Xamarin.Forms.Editor) ビューと [`Button`](xref:Xamarin.Forms.Button) ビューに対する暗黙的なスタイルがページ レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`StackLayout.Margin` プロパティがアプリケーション レベル `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `Editor` と `Button` は、`NoteEntryPage` によってのみ使用されるため、ページ レベル `ResourceDictionary` に追加されていることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、 **NoteEntryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

5. 各プラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](single-page.md#building-the-quickstart)」を参照してください。

    **NotesPage** で **[+]** ボタンを押して **NoteEntryPage** に移動し、メモを入力します。 各ページで、前のクイックスタートからスタイルがどのように変更されたかを確認します。

::: zone-end

## <a name="next-steps"></a>次の手順

このクイックスタートでは、次の方法について学習しました。

- XAML スタイルを使用して Xamarin.Forms アプリケーションのスタイルを設定する。

Xamarin.Forms を使用したアプリケーション開発の基礎の詳細については、クイックスタート Deep Dive に進みます。

> [!div class="nextstepaction"]
> [次へ](deepdive.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin.Forms クイックスタート Deep Dive](deepdive.md)