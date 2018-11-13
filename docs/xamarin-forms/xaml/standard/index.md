---
title: XAML Standard (プレビュー)
description: この記事では、Xamarin.Forms では、XAML Standard プレビューの調査を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
ms.openlocfilehash: d9fb19fb233c25e5dd525c7c157013ef66f4a4f3
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51562758"
---
# <a name="xaml-standard-preview"></a>XAML Standard (プレビュー)

![[プレビュー]](~/media/shared/preview.png)

XAML Standard で Xamarin.Forms を使用した実験を次の手順に従います。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ダウンロード、[ここに NuGet パッケージのプレビュー](https://aka.ms/xf-xamlstandard-nuget)します。
2. 追加、 **Xamarin.Forms.Alias** Xamarin.Forms .NET Standard とプラットフォームのプロジェクトに NuGet パッケージ。
3. 使用して、パッケージを初期化します。 `Alias.Init()`
4. 追加、`xmlns:a`リファレンス `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML の型の使用 - を参照してください、[コントロールのリファレンス](controls.md)詳細についてはします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

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
