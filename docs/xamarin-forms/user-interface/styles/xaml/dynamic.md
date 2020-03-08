---
title: Xamarin.Forms での動的なスタイル
description: この記事では、動的リソースを使用して、Xamarin.Forms アプリケーションを実行時に動的にスタイルの変更に応答できる方法について説明します。
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
ms.custom: video
ms.openlocfilehash: 9a26532d13b843b812da94739be071c7accac212
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918686"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Xamarin.Forms での動的なスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_スタイルは、プロパティの変更に応答せず、アプリケーションの継続中は変更されません。たとえば、視覚要素にスタイルを割り当てた後、Setter インスタンスのいずれかが変更、削除された場合、または新しい Setter インスタンスが追加された場合、変更はビジュアル要素に適用されません。ただし、アプリケーションは動的リソースを使用して、実行時に動的にスタイルの変更に応答できます。_

`DynamicResource` マークアップ拡張機能は、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)から値をフェッチするためにディクショナリキーを使用するの `StaticResource` マークアップ拡張機能に似ています。 ただし、`StaticResource` は単一の辞書検索を実行しますが、`DynamicResource` は辞書キーへのリンクを保持します。 そのため、キーに関連付けられているディクショナリ エントリが置き換えられている場合、変更は、ビジュアル要素に適用されます。 これにより、アプリケーションで作成するランタイム スタイルの変更ができます。

次のコード例は、XAML ページの*動的*スタイルを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar)インスタンスは、`DynamicResource` マークアップ拡張機能を使用して、XAML で定義されていない `searchBarStyle`という名前の[`Style`](xref:Xamarin.Forms.Style)を参照します。 ただし、`SearchBar` インスタンスの[`Style`](xref:Xamarin.Forms.NavigableElement.Style)プロパティは `DynamicResource`を使用して設定されるため、不足しているディクショナリキーによって例外がスローされることはありません。

代わりに、分離コードファイルで、コンストラクターは次のコード例に示すように、キー `searchBarStyle`を使用して[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)エントリを作成します。

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

`OnButtonClicked` イベントハンドラーが実行されると、`searchBarStyle` は `blueSearchBarStyle` と `greenSearchBarStyle`を切り替えます。 これで、次のスクリーンショットのような結果になります。

[![青の動的スタイルの例](dynamic-images/dynamic-style-blue.png)](dynamic-images/dynamic-style-blue-large.png#lightbox)
[![緑色の動的スタイルの例](dynamic-images/dynamic-style-green.png)](dynamic-images/dynamic-style-green-large.png#lightbox)

次のコード例では、c# で最初のページを示しています。

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

でC#は、 [`SearchBar`](xref:Xamarin.Forms.SearchBar)インスタンスは、 [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*)メソッドを使用して `searchBarStyle`を参照します。 `OnButtonClicked` イベントハンドラーのコードは、XAML の例と同じです。実行すると、`searchBarStyle` は `blueSearchBarStyle` と `greenSearchBarStyle`を切り替えます。

## <a name="dynamic-style-inheritance"></a>動的スタイルの継承

動的スタイルからのスタイルの派生は、 [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn)プロパティを使用して実現することはできません。 代わりに、 [`Style`](xref:Xamarin.Forms.Style)クラスに[`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティが含まれています。このプロパティは、値が動的に変更される可能性のあるディクショナリキーに設定できます。

次のコード例は、XAML ページでの*動的*スタイルの継承を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar)インスタンスは、`StaticResource` マークアップ拡張機能を使用して、`tealSearchBarStyle`という名前の[`Style`](xref:Xamarin.Forms.Style)を参照します。 この `Style` はいくつかの追加のプロパティを設定し、 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティを使用して `searchBarStyle`を参照します。 `DynamicResource` マークアップ拡張機能は、派生元の `Style` を除き、`tealSearchBarStyle` は変更されないため、必須ではありません。 したがって、`tealSearchBarStyle` は `searchBarStyle` へのリンクを保持し、基本スタイルが変更されたときに変更されます。

分離コードファイルでは、コンストラクターは、前の例の動的スタイルの例に従って、キー `searchBarStyle`を持つ[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)エントリを作成します。 `OnButtonClicked` イベントハンドラーが実行されると、`searchBarStyle` は `blueSearchBarStyle` と `greenSearchBarStyle`を切り替えます。 これで、次のスクリーンショットのような結果になります。

[![青の動的スタイルの継承の例](dynamic-images/dynamic-style-inheritance-blue.png)](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox)
[![緑色の動的スタイルの継承の例](dynamic-images/dynamic-style-inheritance-green.png)](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox)

次のコード例では、c# で最初のページを示しています。

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle` は、 [`SearchBar`](xref:Xamarin.Forms.SearchBar)インスタンスの[`Style`](xref:Xamarin.Forms.NavigableElement.Style)プロパティに直接割り当てられます。 この `Style` はいくつかの追加のプロパティを設定し、 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティを使用して `searchBarStyle`を参照します。 ここでは[`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*)メソッドは必要ありません。これは、派生元の `Style` を除き `tealSearchBarStyle` は変更されないためです。 したがって、`tealSearchBarStyle` は `searchBarStyle` へのリンクを保持し、基本スタイルが変更されたときに変更されます。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [メソッド](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
