---
title: での明示的なスタイルXamarin.Forms
description: 明示的なスタイルとは、スタイルプロパティを設定することによってコントロールに選択的に適用されるスタイルです。 この記事では、アプリケーションで明示的なスタイルを使用する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 62b84a5028c17c28a69a887a832028c2064fa78d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136267"
---
# <a name="explicit-styles-in-xamarinforms"></a>での明示的なスタイルXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_明示的なスタイルとは、スタイルプロパティを設定することによってコントロールに選択的に適用されるスタイルです。_

## <a name="create-an-explicit-style-in-xaml"></a>XAML での明示的なスタイルの作成

ページレベルでを宣言するには、 [`Style`](xref:Xamarin.Forms.Style) を [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ページに追加し、1つ以上の宣言をに含める必要があり `Style` `ResourceDictionary` ます。 は、の `Style` 宣言に属性を指定することによって*明示的*に作成されます。これにより `x:Key` 、に説明的なキーが与えられ `ResourceDictionary` ます。 その後、プロパティを設定することによって、*明示的*なスタイルを特定のビジュアル要素に適用する必要があり [`Style`](xref:Xamarin.Forms.NavigableElement.Style) ます。

次のコード例は、ページのインスタンスに適用される XAML で宣言された*明示的*なスタイル `ResourceDictionary` を示してい [`Label`](xref:Xamarin.Forms.Label) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

は、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ページのインスタンスに適用される3つの*明示的*なスタイルを定義し [`Label`](xref:Xamarin.Forms.Label) ます。 各 `Style` は、テキストを別の色で表示するために使用されます。また、フォントサイズ、水平および垂直方向のレイアウトオプションも設定します。 各 `Style` は、 `Label` [`Style`](xref:Xamarin.Forms.NavigableElement.Style) マークアップ拡張機能を使用してプロパティを設定することによって、別のに適用され `StaticResource` ます。 この結果、次のスクリーンショットに示すような外観が表示されます。

[![明示的スタイルの例](explicit-images/explicit-styles.png)](explicit-images/explicit-styles-large.png#lightbox)

さらに、最終的なにはが [`Label`](xref:Xamarin.Forms.Label) [`Style`](xref:Xamarin.Forms.Style) 適用されますが、プロパティが別の値にオーバーライドされることもあり [`TextColor`](xref:Xamarin.Forms.Label.TextColor) `Color` ます。

### <a name="create-an-explicit-style-at-the-control-level"></a>コントロールレベルでの明示的なスタイルの作成

次のコード例に示すように、*明示的*なスタイルをページレベルで作成するだけでなく、コントロールレベルで作成することもできます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

この例では、*明示的* [`Style`](xref:Xamarin.Forms.Style) なインスタンスがコントロールのコレクションに割り当てられてい [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 スタイルは、コントロールとその子に適用できます。

アプリケーションのでスタイルを作成する方法の詳細につい [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ては、「[グローバルスタイル](~/xamarin-forms/user-interface/styles/application.md)」を参照してください。

## <a name="create-an-explicit-style-in-c35"></a>C&#35; での明示的なスタイルの作成

[`Style`](xref:Xamarin.Forms.Style)C# のページコレクションにインスタンスを追加するには、 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 次の `Style` `ResourceDictionary` コード例に示すように、新しいを作成し、インスタンスをに追加します。

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

コンストラクターは、ページのインスタンスに適用される3つの*明示的*なスタイルを定義し [`Label`](xref:Xamarin.Forms.Label) ます。 各*明示的* [`Style`](xref:Xamarin.Forms.Style) なは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) メソッドを使用して、 [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) `key` インスタンスを参照する文字列を指定してに追加され `Style` ます。 それぞれの `Style` プロパティを設定する `Label` ことによって、それぞれが異なるに適用され [`Style`](xref:Xamarin.Forms.NavigableElement.Style) ます。

ただし、ここではを使用する利点はありません [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 。 代わりに、 [`Style`](xref:Xamarin.Forms.Style) [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 次のコード例に示すように、インスタンスを必要なビジュアル要素のプロパティに直接割り当てることができ、を `ResourceDictionary` 削除できます。

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

コンストラクターは、ページのインスタンスに適用される3つの*明示的*なスタイルを定義し [`Label`](xref:Xamarin.Forms.Label) ます。 各 `Style` は、テキストを別の色で表示するために使用されます。また、フォントサイズ、水平および垂直方向のレイアウトオプションも設定します。 各 `Style` は、 `Label` そのプロパティを設定することによって、別のに適用され [`Style`](xref:Xamarin.Forms.NavigableElement.Style) ます。 さらに、最終的なにはが `Label` `Style` 適用されますが、プロパティが別の値にオーバーライドされることもあり `TextColor` `Color` ます。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
