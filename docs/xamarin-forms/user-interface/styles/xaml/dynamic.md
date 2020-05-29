---
title: 動的スタイルXamarin.Forms
description: この記事では、 Xamarin.Forms 動的リソースを使用して、アプリケーションが実行時に動的にスタイル変更に応答する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: ''
ms.openlocfilehash: d40ca3423cca68757cf458faf5cca1138aec5461
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140089"
---
# <a name="dynamic-styles-in-xamarinforms"></a>動的スタイルXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_スタイルは、プロパティの変更に応答せず、アプリケーションの継続中は変更されません。たとえば、視覚要素にスタイルを割り当てた後、Setter インスタンスのいずれかが変更、削除された場合、または新しい Setter インスタンスが追加された場合、変更はビジュアル要素に適用されません。ただし、アプリケーションは動的リソースを使用して、実行時に動的にスタイルの変更に応答できます。_

`DynamicResource`マークアップ拡張機能は、 `StaticResource` から値をフェッチするためにディクショナリキーを使用するという点で、マークアップ拡張機能に似てい [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ます。 ただし、では、 `StaticResource` 単一の辞書参照が実行されますが、 `DynamicResource` ディクショナリキーへのリンクが保持されます。 そのため、キーに関連付けられているディクショナリエントリが置換された場合、変更はビジュアル要素に適用されます。 これにより、アプリケーションで実行時のスタイルの変更を行うことができます。

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

インスタンスは、 [`SearchBar`](xref:Xamarin.Forms.SearchBar) `DynamicResource` マークアップ拡張機能を使用して [`Style`](xref:Xamarin.Forms.Style) 、XAML で定義されていない、という名前のを参照し `searchBarStyle` ます。 ただし、 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) インスタンスのプロパティは `SearchBar` を使用して設定されるため、不足しているディクショナリキーによって `DynamicResource` 例外がスローされることはありません。

代わりに、分離コードファイルで、コンストラクターは [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `searchBarStyle` 次のコード例に示すように、キーを使用してエントリを作成します。

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

`OnButtonClicked`イベントハンドラーが実行されると、 `searchBarStyle` はとの間を切り替え `blueSearchBarStyle` `greenSearchBarStyle` ます。 この結果、次のスクリーンショットに示すような外観が表示されます。

[ ![ 青の動的スタイルの例](dynamic-images/dynamic-style-blue.png)](dynamic-images/dynamic-style-blue-large.png#lightbox)( 
 [ ![ 緑の動的スタイル](dynamic-images/dynamic-style-green.png)](dynamic-images/dynamic-style-green-large.png#lightbox)の例)

次のコード例は、C# の対応するページを示しています。

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

C# では、インスタンスはメソッドを使用してを [`SearchBar`](xref:Xamarin.Forms.SearchBar) [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) 参照 `searchBarStyle` します。 `OnButtonClicked`イベントハンドラーコードは、XAML の例と同じです。実行すると、 `searchBarStyle` とが `blueSearchBarStyle` 切り替わり `greenSearchBarStyle` ます。

## <a name="dynamic-style-inheritance"></a>動的スタイルの継承

動的スタイルからのスタイルの派生は、プロパティを使用して実現できません [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) 。 代わりに、クラスには [`Style`](xref:Xamarin.Forms.Style) プロパティが含まれてい [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) ます。このプロパティは、値が動的に変更される可能性のあるディクショナリキーに設定できます。

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

インスタンスは、 [`SearchBar`](xref:Xamarin.Forms.SearchBar) マークアップ拡張機能を使用して、 `StaticResource` という名前のを参照し [`Style`](xref:Xamarin.Forms.Style) `tealSearchBarStyle` ます。 これ `Style` により、いくつかの追加のプロパティが設定され、プロパティを使用してが [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 参照さ `searchBarStyle` れます。 `DynamicResource`マークアップ拡張機能は、 `tealSearchBarStyle` から派生したを除き、変更されないため、必要ありません `Style` 。 したがって、は `tealSearchBarStyle` へのリンクを保持 `searchBarStyle` し、基本スタイルが変更されると変更されます。

分離コードファイルでは、コンストラクターは、前の [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 例の動的スタイルの例に従って、キーを使用してエントリを作成し `searchBarStyle` ます。 `OnButtonClicked`イベントハンドラーが実行されると、 `searchBarStyle` はとの間を切り替え `blueSearchBarStyle` `greenSearchBarStyle` ます。 この結果、次のスクリーンショットに示すような外観が表示されます。

[ ![ 青の動的スタイルの継承例](dynamic-images/dynamic-style-inheritance-blue.png)](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox)( 
 [ ![ 緑の動的スタイルの継承](dynamic-images/dynamic-style-inheritance-green.png)](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox)の例)

次のコード例は、C# の対応するページを示しています。

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

は、 `tealSearchBarStyle` インスタンスのプロパティに直接割り当てられ [`Style`](xref:Xamarin.Forms.NavigableElement.Style) [`SearchBar`](xref:Xamarin.Forms.SearchBar) ます。 これ `Style` により、いくつかの追加のプロパティが設定され、プロパティを使用してが参照され [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) `searchBarStyle` ます。 [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*)ここでは、が変更されないため、メソッドは必要 `tealSearchBarStyle` ありません。これは、から派生したを除き `Style` ます。 したがって、は `tealSearchBarStyle` へのリンクを保持 `searchBarStyle` し、基本スタイルが変更されると変更されます。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
