---
title: XAML Standard (プレビュー)
description: この記事では、Xamarin.Forms では、XAML Standard プレビューの調査を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 61e0fa2587ce9a8794dbd32ff9de1f13da857342
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38838011"
---
# <a name="xaml-standard-preview"></a>XAML Standard (プレビュー)

![[プレビュー]](~/media/shared/preview.png)

XAML Standard で Xamarin.Forms を使用した実験を次の手順に従います。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダウンロード、[ここに NuGet パッケージのプレビュー](https://aka.ms/xf-xamlstandard-nuget)します。
2. 追加、 **Xamarin.Forms.Alias** Xamarin.Forms .NET Standard とプラットフォームのプロジェクトに NuGet パッケージ。
3. 使用して、パッケージを初期化します。 `Alias.Init()`
4. 追加、`xmlns:a`リファレンス `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML の型の使用 - を参照してください、[コントロールのリファレンス](controls.md)詳細についてはします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダウンロード、[ここに NuGet パッケージのプレビュー](https://aka.ms/xf-xamlstandard-nuget)します。
2. 追加、 **Xamarin.Forms.Alias** Xamarin.Forms .NET Standard とプラットフォームのプロジェクトに NuGet パッケージ。
3. 使用して、パッケージを初期化します。 `Alias.Init()`
4. 追加、`xmlns:a`リファレンス `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML の型の使用 - を参照してください、[コントロールのリファレンス](controls.md)詳細についてはします。

-----

次の XAML は、Xamarin.Forms で使用されているいくつかの標準的な XAML コントロールを示します`ContentPage`:

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
> Xmlns を必要とする`a:`XAML 標準コントロールでのプレフィックスは現在プレビューの制限です。


## <a name="related-links"></a>関連リンク

- [NuGet のプレビュー](https://aka.ms/xf-xamlstandard-nuget)
- [コントロールのリファレンス](controls.md)
