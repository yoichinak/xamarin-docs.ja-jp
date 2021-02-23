---
title: クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する
description: この記事では、XAML スタイルを使用してクロスプラットフォームの Xamarin.Forms シェル アプリケーションのスタイルを設定し、XAML ホット リロードを使用してこれらの変更を表示する方法について説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 1C6ED8AF-41B4-4D6C-9BD8-C72D3F05E541
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/26/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0424f3c9cdcde98cef4c414ada33046cd8ce7ee6
ms.sourcegitcommit: 1f391667869a4541dd9b42d78862dc01d69ed160
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2021
ms.locfileid: "99818097"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

このクイックスタートでは、次の方法について学習します。

- XAML スタイルを使用して Xamarin.Forms シェル アプリケーションのスタイルを設定する。
- XAML ホット リロードを使用して、アプリケーションをリビルドせずに UI の変更を確認する。

このクイックスタートでは、XAML スタイルを使用して、クロスプラットフォーム Xamarin.Forms アプリケーションのスタイルを設定する方法を説明します。 また、このクイックスタートでは、XAML ホット リロードを使用して、実行中のアプリケーションの UI を更新します。アプリケーションをリビルドする必要はありません。 XAML ホット リロードの詳細については、「[Xamarin.Forms の XAML ホット リロード](~/xamarin-forms/xaml/hot-reload.md)」を参照してください。

最終的なアプリケーションは、次のとおりです。

[![Notes ページ](styling-images/screenshots1-sml.png)](styling-images/screenshots1.png#lightbox)
[![Note Entry ページ](styling-images/screenshots2-sml.png)](styling-images/screenshots2.png#lightbox)
[![About ページ](styling-images/screenshots3-sml.png)](styling-images/screenshots3.png#lightbox)

### <a name="prerequisites"></a>必須コンポーネント

このクイックスタートを試みるには、[前のクイックスタート](database.md)を正常に完了しておく必要があります。 または、[前のクイックスタートのサンプル](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)をダウンロードし、それをこのクイックスタートの出発点として使用します。

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動し、Notes ソリューションを開きます。

2. 選択したプラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](app.md#building-the-quickstart)」を参照してください。

    アプリケーションを実行したままにして、Visual Studio に戻ります。

3. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **App.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

        <!-- Resources used by multiple pages in the application -->         
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppPrimaryColor">#1976D2</Color>
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="PrimaryColor">Black</Color>
            <Color x:Key="SecondaryColor">White</Color>
            <Color x:Key="TertiaryColor">Silver</Color>

            <!-- Implicit styles -->
            <Style TargetType="ContentPage"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button">
                <Setter Property="FontSize"
                        Value="Medium" />
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="TextColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="CornerRadius"
                        Value="5" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    このコードにより、[`Thickness`](xref:Xamarin.Forms.Thickness) 値、一連の [`Color`](xref:Xamarin.Forms.Color) 値、および [`ContentPage`](xref:Xamarin.Forms.ContentPage) 型と [`Button`](xref:Xamarin.Forms.Button) 型の暗黙的なスタイルが定義されます。 アプリケーション レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) にあるこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **Ctrl + S** キーを押して **App.xaml** への変更を保存します。 XAML ホット リロードにより、アプリケーションをリビルドせずに、実行中のアプリの UI が更新されます。 具体的には、各ページの背景色が変更されます。

4. **ソリューション エクスプローラー** で、**Notes** プロジェクトの **AppShell.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">

        <Shell.Resources>
            <!-- Style Shell elements -->
            <Style x:Key="BaseStyle"
                   TargetType="Element">
                <Setter Property="Shell.BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="Shell.ForegroundColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TitleColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TabBarUnselectedColor"
                        Value="#95FFFFFF"/>
            </Style>
            <Style TargetType="TabBar"
                   BasedOn="{StaticResource BaseStyle}" />
        </Shell.Resources>

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

    このコードにより、アプリケーションによって使用される一連の [`Color`](xref:Xamarin.Forms.Color) 値が定義されている 2 つのスタイルが、`Shell` リソース ディクショナリに追加されます。

    **Ctrl + S** キーを押して **AppShell.xaml** への変更を保存します。 XAML ホット リロードにより、アプリケーションをリビルドせずに、実行中のアプリの UI が更新されます。 具体的には、シェル chrome の背景色が変更されます。

5. **ソリューション エクスプローラー** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **NotesPage.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">

        <ContentPage.Resources>
            <!-- Define a visual state for the Selected state of the CollectionView -->
            <Style TargetType="StackLayout">
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal" />
                            <VisualState x:Name="Selected">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="{StaticResource AppPrimaryColor}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>
        </ContentPage.Resources>

        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="{StaticResource PageMargin}"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium" />
                        <Label Text="{Binding Date}"
                               TextColor="{StaticResource TertiaryColor}"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    このコードにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で選択された各項目の外観が定義されている [`StackLayout`](xref:Xamarin.Forms.StackLayout) の暗黙的なスタイルがページ レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`CollectionView.Margin` プロパティと `Label.TextColor`プロパティがアプリケーション レベルの `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `StackLayout` は、`NotesPage` によってのみ使用されるため、ページ レベル `ResourceDictionary` に追加されていることに注意してください。

    **Ctrl + S** キーを押して **NotesPage.xaml** への変更を保存します。 XAML ホット リロードにより、アプリケーションをリビルドせずに、実行中のアプリの UI が更新されます。 具体的には、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で選択された項目の色が変更されます。

6. **ソリューション エクスプローラー** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **NoteEntryPage.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>         
        </ContentPage.Resources>

        <!-- Layout children vertically -->
        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid ColumnDefinitions="*,*">
                <!-- Layout children in two columns -->
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    このコードにより、[`Editor`](xref:Xamarin.Forms.Editor) に対する暗黙的なスタイルがページ レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`StackLayout.Margin` プロパティがアプリケーション レベル `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `Editor` は、`NoteEntryPage` によってのみ使用されるため、ページ レベルの `ResourceDictionary` に追加されていることに注意してください。

7. 実行中のアプリケーションで、`NoteEntryPage` に移動します。

8. Visual Studio で、 **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NoteEntryPage.xaml** への変更を保存します。

    XAML ホット リロードにより、リビルドなしで、アプリケーションの UI が更新されます。 具体的には、実行中のアプリケーションで [`Editor`](xref:Xamarin.Forms.Editor) の背景色が、[`Button`](xref:Xamarin.Forms.Button) オブジェクトの外観と同様に変更されます。

9. **ソリューション エクスプローラー** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **AboutPage.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{StaticResource AppPrimaryColor}"
                   Opacity="0.85"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->       
            <StackLayout Grid.Row="1"
                         Margin="{StaticResource PageMargin}"
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

    このコードにより、`Image.BackgroundColor` プロパティと `StackLayout.Margin` プロパティが、アプリケーション レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) で定義されている値に設定されます。

10. 実行中のアプリケーションで、`AboutPage` に移動します。

11. Visual Studio で、**Ctrl + S** キーを押して **AboutPage.xaml** への変更を保存します。

    XAML ホット リロードにより、リビルドなしで、アプリケーションの UI が更新されます。 具体的には、実行中のアプリケーションで [`Image`](xref:Xamarin.Forms.Editor) の背景色が変更されます。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動し、Notes プロジェクトを開きます。

2. 選択したプラットフォームでプロジェクトをビルドして実行します。 詳細については、「[クイックスタートのビルド](app.md#building-the-quickstart)」を参照してください。

    アプリケーションを実行したままにして、Visual Studio for Mac に戻ります。

3. **Solution Pad** で、**Notes** プロジェクトの **App.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

        <!-- Resources used by multiple pages in the application -->                 
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppPrimaryColor">#1976D2</Color>
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="PrimaryColor">Black</Color>
            <Color x:Key="SecondaryColor">White</Color>
            <Color x:Key="TertiaryColor">Silver</Color>

            <!-- Implicit styles -->
            <Style TargetType="ContentPage"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button">
                <Setter Property="FontSize"
                        Value="Medium" />
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="TextColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="CornerRadius"
                        Value="5" />
            </Style>  
        </Application.Resources>
    </Application>
    ```

    このコードにより、[`Thickness`](xref:Xamarin.Forms.Thickness) 値、一連の [`Color`](xref:Xamarin.Forms.Color) 値、および [`ContentPage`](xref:Xamarin.Forms.ContentPage) 型と [`Button`](xref:Xamarin.Forms.Button) 型の暗黙的なスタイルが定義されます。 アプリケーション レベル [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) にあるこれらのスタイルは、アプリケーション全体で使用できることに注意してください。 XAML のスタイル設定の詳細については、「[Xamarin.Forms クイックスタート Deep Dive](deepdive.md)」の「[スタイル設定](deepdive.md#styling)」を参照してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**App.xaml** への変更を保存します。 XAML ホット リロードにより、アプリケーションをリビルドせずに、実行中のアプリの UI が更新されます。 具体的には、各ページの背景色が変更されます。

4. **Solution Pad** で、**Notes** プロジェクトの **AppShell.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">

        <Shell.Resources>
            <!-- Style Shell elements -->
            <Style x:Key="BaseStyle"
                   TargetType="Element">
                <Setter Property="Shell.BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="Shell.ForegroundColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TitleColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TabBarUnselectedColor"
                        Value="#95FFFFFF"/>
            </Style>
            <Style TargetType="TabBar"
                   BasedOn="{StaticResource BaseStyle}" />
        </Shell.Resources>

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

    このコードにより、アプリケーションによって使用される一連の [`Color`](xref:Xamarin.Forms.Color) 値が定義されている 2 つのスタイルが、`Shell` リソース ディクショナリに追加されます。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AppShell.xaml** への変更を保存します。 XAML ホット リロードにより、アプリケーションをリビルドせずに、実行中のアプリの UI が更新されます。 具体的には、シェル chrome の背景色が変更されます。

5. **Solution Pad** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **NotesPage.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">

        <ContentPage.Resources>
            <!-- Define a visual state for the Selected state of the CollectionView -->
            <Style TargetType="StackLayout">
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal" />
                            <VisualState x:Name="Selected">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="{StaticResource AppPrimaryColor}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>
        </ContentPage.Resources>

        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="{StaticResource PageMargin}"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium" />
                        <Label Text="{Binding Date}"
                               TextColor="{StaticResource TertiaryColor}"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    このコードにより、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で選択された各項目の外観が定義されている [`StackLayout`](xref:Xamarin.Forms.StackLayout) の暗黙的なスタイルがページ レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`CollectionView.Margin` プロパティと `Label.TextColor`プロパティがアプリケーション レベルの `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `StackLayout` は、`NotesPage` によってのみ使用されるため、ページ レベル `ResourceDictionary` に追加されていることに注意してください。

    **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NotesPage.xaml** への変更を保存します。 XAML ホット リロードにより、アプリケーションをリビルドせずに、実行中のアプリの UI が更新されます。 具体的には、[`CollectionView`](xref:Xamarin.Forms.CollectionView) で選択された項目の色が変更されます。

6. **Solution Pad** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **NoteEntryPage.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>       
        </ContentPage.Resources>

        <!-- Layout children vertically -->
        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
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

    このコードにより、[`Editor`](xref:Xamarin.Forms.Editor) の暗黙的なスタイルがページ レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) に追加され、`StackLayout.Margin` プロパティがアプリケーション レベルの `ResourceDictionary` で定義されている値に設定されます。 暗黙的なスタイル `Editor` は、`NoteEntryPage` によってのみ使用されるため、ページ レベルの `ResourceDictionary` に追加されていることに注意してください。

7. 実行中のアプリケーションで、`NoteEntryPage` に移動します。

8. Visual Studio for Mac で、 **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**NoteEntryPage.xaml** への変更を保存します。

    XAML ホット リロードにより、リビルドなしで、アプリケーションの UI が更新されます。 具体的には、実行中のアプリケーションで [`Editor`](xref:Xamarin.Forms.Editor) の背景色が、[`Button`](xref:Xamarin.Forms.Button) オブジェクトの外観と同様に変更されます。

9. **Solution Pad** の **Notes** プロジェクトで、 **[Views]** フォルダーにある **AboutPage.xaml** を開きます。 次に、既存のコードを次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{StaticResource AppPrimaryColor}"
                   Opacity="0.85"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="{StaticResource PageMargin}"
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

    このコードにより、`Image.BackgroundColor` プロパティと `StackLayout.Margin` プロパティが、アプリケーション レベルの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) で定義されている値に設定されます。

10. 実行中のアプリケーションで、`AboutPage` に移動します。

11. Visual Studio for Mac で、 **[ファイル] > [保存]** を選択して (または **&#8984; + S** キーを押して)、**AboutPage.xaml** への変更を保存します。

    XAML ホット リロードにより、リビルドなしで、アプリケーションの UI が更新されます。 具体的には、実行中のアプリケーションで [`Image`](xref:Xamarin.Forms.Editor) の背景色が変更されます。

::: zone-end

## <a name="next-steps"></a>次のステップ

このクイックスタートでは、次の方法について学習しました。

- XAML スタイルを使用して Xamarin.Forms シェル アプリケーションのスタイルを設定する。
- XAML ホット リロードを使用して、アプリケーションをリビルドせずに UI の変更を確認する。

Xamarin.Forms シェルを使用したアプリケーション開発の基礎の詳細について詳しく学習するには、クイックスタート Deep Dive に進んでください。

> [!div class="nextstepaction"]
> [次へ](deepdive.md)

## <a name="related-links"></a>関連リンク

- [Notes (サンプル)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin.Forms の XAML ホット リロード](~/xamarin-forms/xaml/hot-reload.md)
- [Xamarin.Forms クイックスタート Deep Dive](deepdive.md)
