---
title: 動的なスタイル
description: せずスタイルにプロパティの変更に応答して、アプリケーションの実行中は変更されません。 たとえば、視覚的要素を 1 つの set アクセス操作子インスタンスが変更された場合、削除、または追加された新しい set アクセス操作子インスタンスに割り当てると、スタイル、変更は視覚要素に適用されません。 ただし、アプリケーションは、動的なリソースを使用して、実行時に動的にスタイルの変更に応答できます。
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: e0cfcbaef70f58622a21315637279740f568ada8
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848071"
---
# <a name="dynamic-styles"></a>動的なスタイル

_せずスタイルにプロパティの変更に応答して、アプリケーションの実行中は変更されません。たとえば、視覚的要素を 1 つの set アクセス操作子インスタンスが変更された場合、削除、または追加された新しい set アクセス操作子インスタンスに割り当てると、スタイル、変更は視覚要素に適用されません。ただし、アプリケーションは、動的なリソースを使用して、実行時に動的にスタイルの変更に応答できます。_

`DynamicResource`マークアップ拡張機能がに似ていますが、`StaticResource`から値をフェッチする辞書のキーを使用して両方のマークアップ拡張機能、 [ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。 ただし、while、 `StaticResource` 1 つのディクショナリの参照を実行、`DynamicResource`ディクショナリ キーへのリンクを維持します。 そのため、キーに関連付けられているディクショナリ エントリが置き換えられた場合、変更が視覚要素に適用されます。 これにより、アプリケーションで実行するランタイムのスタイル変更できます。

次のコード例を示します*動的*XAML ページのスタイル。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
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

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)インスタンスを使用して、`DynamicResource`を参照するマークアップ拡張機能、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)という名前`searchBarStyle`、これが、XAML で定義されていません。 ただし、ため、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)のプロパティ、`SearchBar`インスタンスを使用して設定されて、 `DynamicResource`、不足しているディクショナリのキーが返されないと例外がスローされます。

代わりに、分離コード ファイルで、コンス トラクターを作成、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)キーを持つエントリ`searchBarStyle`の次のコード例に示すようにします。

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

ときに、`OnButtonClicked`イベント ハンドラーを実行すると、`searchBarStyle`切り替えることは`blueSearchBarStyle`と`greenSearchBarStyle`です。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](dynamic-images/dynamic-style-blue.png "青の動的スタイル例")](dynamic-images/dynamic-style-blue-large.png#lightbox "青の動的なスタイルの使用例")
[![](dynamic-images/dynamic-style-green.png "緑色の動的スタイル例")](dynamic-images/dynamic-style-green-large.png#lightbox "緑色の動的なスタイルの例")

次のコード例では、C# の場合と同じページを示しています。

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

C# で、 [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)インスタンスを使用して、 [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/)メソッドを参照する`searchBarStyle`です。 `OnButtonClicked`イベント ハンドラーのコードは XAML の例と、実行時に同じ`searchBarStyle`切り替えることは`blueSearchBarStyle`と`greenSearchBarStyle`です。

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>動的なスタイルの継承

動的なスタイルのスタイルを派生することを使用して、 [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/)プロパティです。 代わりに、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)クラスが含まれています、 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/)値を持つにディクショナリのキーを設定できるプロパティを動的に変更があります。

次のコード例を示します*動的*XAML ページのスタイルを継承します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
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

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)インスタンスを使用して、`StaticResource`を参照するマークアップ拡張機能、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)という`tealSearchBarStyle`です。 これは、`Style`追加のプロパティを設定し、使用して、 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/)プロパティ参照を`searchBarStyle`です。 `DynamicResource`マークアップ拡張機能は必要ありませんので`tealSearchBarStyle`変更されませんが、を除き、`Style`から派生します。 したがって、`tealSearchBarStyle`へのリンクを維持`searchBarStyle`が、基本のスタイルが変更されたときに変更されるとします。

分離コード ファイルでは、コンス トラクターを作成、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)キーを持つエントリ`searchBarStyle`、動的なスタイルを例示されている前の例としてごとです。 ときに、`OnButtonClicked`イベント ハンドラーを実行すると、`searchBarStyle`切り替えることは`blueSearchBarStyle`と`greenSearchBarStyle`です。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](dynamic-images/dynamic-style-inheritance-blue.png "青の動的なスタイルの継承の例")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "動的スタイルの継承の例の青")
[![](dynamic-images/dynamic-style-inheritance-green.png "緑色の動的なスタイル継承の例")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "緑色の動的なスタイルの継承の例")

次のコード例では、C# の場合と同じページを示しています。

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

`tealSearchBarStyle`に直接割り当ては、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)のプロパティ、 [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)インスタンス。 これは、`Style`いくつか追加のプロパティを設定し、使用して、 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/)プロパティ参照を`searchBarStyle`です。 [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/)メソッドは必要ありませんここため`tealSearchBarStyle`変更されませんが、を除き、`Style`から派生します。 したがって、`tealSearchBarStyle`へのリンクを維持`searchBarStyle`が、基本のスタイルが変更されたときに変更されるとします。

## <a name="summary"></a>まとめ

せずスタイルにプロパティの変更に応答して、アプリケーションの実行中は変更されません。 ただし、アプリケーションは、動的なリソースを使用して、実行時に動的にスタイルの変更に応答できます。 さらに、*動的*のスタイルから派生することができます、 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/)プロパティです。



## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [動的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
