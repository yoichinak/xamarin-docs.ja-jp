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
ms.openlocfilehash: 1b4732e87fb09a4846bfe12b7a476dfef2d6f4f9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647227"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Xamarin.Forms での動的なスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_スタイルはないプロパティの変更に応答し、アプリケーションの実行中は変更されません。たとえば、スタイル ビジュアル要素、1 つの set アクセス操作子インスタンスが変更された場合、削除、または追加された新しい set アクセス操作子インスタンスに割り当てると、変更は視覚要素に適用されません。ただし、アプリケーションは、動的リソースを使用して実行時に動的にスタイルの変更に応答することができます。_

`DynamicResource`マークアップ拡張機能に似ています、`StaticResource`から値をフェッチする辞書のキーを使用して両方のマークアップ拡張機能、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 ただし、中に、`StaticResource`は 1 つのディクショナリ参照を実行、`DynamicResource`ディクショナリ キーへのリンクを保持します。 そのため、キーに関連付けられているディクショナリ エントリが置き換えられている場合、変更は、ビジュアル要素に適用されます。 これにより、アプリケーションで作成するランタイム スタイルの変更ができます。

次のコード例に示します*動的*XAML ページのスタイル。

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar)インスタンスを使用して、`DynamicResource`を参照するマークアップ拡張機能、 [ `Style` ](xref:Xamarin.Forms.Style)という`searchBarStyle`、XAML で定義されていません。 ただし、ため、 [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style)のプロパティ、`SearchBar`を使用してインスタンスが設定されて、 `DynamicResource`、ディクショナリ キーがないことはない例外がスローされます。

代わりに、分離コード ファイルでコンストラクターは、作成、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)キーを持つエントリ`searchBarStyle`次のコード例のように。

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

ときに、`OnButtonClicked`イベント ハンドラーを実行すると、`searchBarStyle`間の切り替えは`blueSearchBarStyle`と`greenSearchBarStyle`します。 次のスクリーン ショットに示すように外観が発生します。

[![](dynamic-images/dynamic-style-blue.png "動的なスタイルの例の青")](dynamic-images/dynamic-style-blue-large.png#lightbox "動的スタイルの例を青")
[![](dynamic-images/dynamic-style-green.png "緑の動的なスタイルの使用例")](dynamic-images/dynamic-style-green-large.png#lightbox "緑の動的なスタイルの例")

次のコード例では、C# で最初のページを示しています。

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

C# で、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)インスタンスを使用して、 [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*)参照メソッドを`searchBarStyle`します。 `OnButtonClicked`イベント ハンドラーのコードは、XAML の例では、および実行すると、同じです。`searchBarStyle`間の切り替えは`blueSearchBarStyle`と`greenSearchBarStyle`します。

## <a name="dynamic-style-inheritance"></a>動的スタイルの継承

動的なスタイルのスタイルを派生することを実現できないを使用して、 [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn)プロパティ。 代わりに、 [ `Style` ](xref:Xamarin.Forms.Style)クラスが含まれています、 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey)値を持つにディクショナリのキーに設定できるプロパティを動的に変更可能性があります。

次のコード例に示します*動的*XAML ページで継承をスタイル設定します。

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar)インスタンスを使用して、`StaticResource`を参照するマークアップ拡張機能、 [ `Style` ](xref:Xamarin.Forms.Style)という`tealSearchBarStyle`します。 これは、`Style`いくつか追加のプロパティを設定しを使用して、 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティ参照を`searchBarStyle`します。 `DynamicResource`マークアップ拡張機能は必要ありませんので`tealSearchBarStyle`は変更されませんを除き、`Style`から派生します。 そのため、`tealSearchBarStyle`へのリンクを維持`searchBarStyle`が、基本のスタイルが変更されたときに変更されるとします。

コンストラクターを作成、分離コード ファイルで、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)キーを持つエントリ`searchBarStyle`、動的なスタイルを示す前の例のようです。 ときに、`OnButtonClicked`イベント ハンドラーを実行すると、`searchBarStyle`間の切り替えは`blueSearchBarStyle`と`greenSearchBarStyle`します。 次のスクリーン ショットに示すように外観が発生します。

[![](dynamic-images/dynamic-style-inheritance-blue.png "動的なスタイル継承の例の青")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "動的スタイル継承の例の青")
[![](dynamic-images/dynamic-style-inheritance-green.png "緑の動的なスタイル継承の例")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "緑の動的なスタイル継承の例")

次のコード例では、C# で最初のページを示しています。

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

`tealSearchBarStyle`に直接割り当てられている、 [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style)のプロパティ、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)インスタンス。 これは、`Style`いくつか追加のプロパティを設定しを使用して、 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティ参照を`searchBarStyle`します。 [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*)メソッドは必要ありませんここため`tealSearchBarStyle`は変更されませんを除き、`Style`から派生します。 そのため、`tealSearchBarStyle`へのリンクを維持`searchBarStyle`が、基本のスタイルが変更されたときに変更されるとします。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的なスタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [スタイル (サンプル) を使用します。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
