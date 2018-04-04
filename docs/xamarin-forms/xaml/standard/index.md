---
title: XAML Standard (プレビュー)
description: Xamarin.Forms で XAML Standard プレビューを探索する方法
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 7cb2a79fffda40f09ee75a12d6287b162566741d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-standard-preview"></a>XAML Standard (プレビュー)

![[プレビュー]](~/media/shared/preview.png)

Xamarin.Forms で XAML の標準をテストする手順に従います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダウンロード、 [NuGet パッケージは、ここをプレビュー](https://aka.ms/xf-xamlstandard-nuget)です。
2. 追加、 **Xamarin.Forms.Alias** Xamarin.Forms PCL、.NET Standard、およびプラットフォームのプロジェクトに NuGet パッケージです。
3. 使用してパッケージを初期化します。 `Alias.Init()`
4. 追加、`xmlns:a`参照 `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML の型の使用 - を参照してください、[コントロールのリファレンス](controls.md)詳細についてはします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダウンロード、 [NuGet パッケージは、ここをプレビュー](https://aka.ms/xf-xamlstandard-nuget)です。
2. 追加、 **Xamarin.Forms.Alias** Xamarin.Forms PCL、.NET Standard、およびプラットフォームのプロジェクトに NuGet パッケージです。
3. 使用してパッケージを初期化します。 `Alias.Init()`
4. 追加、`xmlns:a`参照 `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML の型の使用 - を参照してください、[コントロールのリファレンス](controls.md)詳細についてはします。

-----

次の XAML は、Xamarin.Forms で使用されている一部の XAML 標準コントロールを示します`ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Xmlns を必要とする`a:`XAML 標準コントロールでのプレフィックスは、現在のプレビューの制限事項です。


## <a name="related-links"></a>関連リンク

- [NuGet のプレビュー](https://aka.ms/xf-xamlstandard-nuget)
- [コントロールのリファレンス](controls.md)
