---
title: 'スタイルの継承:::no-loc(Xamarin.Forms):::'
description: 'スタイルを他のスタイルから継承して、重複を減らし、再利用できるようにすることができます。 この記事では、アプリケーションでスタイルの継承を実行する方法について説明 :::no-loc(Xamarin.Forms)::: します。'
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 9b374987ce7741c82c433b2e35261c3a23ef778f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996748"
---
# <a name="style-inheritance-in-no-locxamarinforms"></a>スタイルの継承:::no-loc(Xamarin.Forms):::

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_スタイルを他のスタイルから継承して、重複を減らし、再利用できるようにすることができます。_

## <a name="style-inheritance-in-xaml"></a>XAML でのスタイルの継承

スタイルの継承は、プロパティを既存のに設定することによって行われ [`Style.BasedOn`](xref::::no-loc(Xamarin.Forms):::.Style.BasedOn) [`Style`](xref::::no-loc(Xamarin.Forms):::.Style) ます。 XAML では、 `BasedOn` `StaticResource` 以前に作成したを参照するマークアップ拡張機能にプロパティを設定することによって、これを実現し `Style` ます。 C# では、プロパティをインスタンスに設定することによってこれを実現し `BasedOn` `Style` ます。

基本スタイルを継承するスタイルには [`Setter`](xref::::no-loc(Xamarin.Forms):::.Setter) 、新しいプロパティのインスタンスを含めることも、基本スタイルのスタイルをオーバーライドするために使用することもできます。 また、基本スタイルから継承するスタイルは、同じ型、または基本スタイルの対象となる型から派生した型を対象とする必要があります。 たとえば、基本スタイルがインスタンスを対象とする場合、 [`View`](xref::::no-loc(Xamarin.Forms):::.View) 基本スタイルに基づくスタイルは、インスタンス `View` またはクラスから派生した型 (やなど) をターゲットにすることができ `View` [`Label`](xref::::no-loc(Xamarin.Forms):::.Label) [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) ます。

次のコードは、XAML ページでの*明示的*なスタイルの継承を示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

ターゲットのインスタンスは、および `baseStyle` [`View`](xref::::no-loc(Xamarin.Forms):::.View) プロパティを設定し [`HorizontalOptions`](xref::::no-loc(Xamarin.Forms):::.View.HorizontalOptions) [`VerticalOptions`](xref::::no-loc(Xamarin.Forms):::.View.VerticalOptions) ます。 は、 `baseStyle` コントロールに直接設定されません。 代わりに、 `labelStyle` とを `buttonStyle` 継承し、追加のバインド可能なプロパティ値を設定します。 次に、 `labelStyle` `buttonStyle` プロパティを設定する [`Label`](xref::::no-loc(Xamarin.Forms):::.Label) ことにより、とがインスタンスとインスタンスに適用され [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) [`Style`](xref::::no-loc(Xamarin.Forms):::.NavigableElement.Style) ます。 この結果、次のスクリーンショットに示すような外観が表示されます。

[![スタイルの継承のスクリーンショット](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> 暗黙的なスタイルは、明示的なスタイルから派生できますが、明示的なスタイルは暗黙的なスタイルから派生することはできません。

### <a name="respecting-the-inheritance-chain"></a>継承チェーンを尊重する

スタイルは、ビュー階層内の同じレベル、またはそれ以上のスタイルからしか継承できません。 これは、次のことを意味します。

- アプリケーションレベルのリソースは、他のアプリケーションレベルのリソースからのみ継承できます。
- ページレベルリソースは、アプリケーションレベルのリソース、およびその他のページレベルリソースから継承できます。
- コントロールレベルリソースは、アプリケーションレベルのリソース、ページレベルのリソース、およびその他のコントロールレベルのリソースから継承できます。

この継承チェーンを次のコード例に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

この例では、 `labelStyle` と `buttonStyle` は制御レベルリソースであり、 `baseStyle` はページレベルリソースです。 ただし、 `labelStyle` および `buttonStyle` はから継承 `baseStyle` されますが、 `baseStyle` `labelStyle` `buttonStyle` ビュー階層内のそれぞれの場所によってまたはから継承することはできません。

## <a name="style-inheritance-in-c35"></a>C&#35; でのスタイル継承

[`Style`](xref::::no-loc(Xamarin.Forms):::.Style) [`Style`](xref::::no-loc(Xamarin.Forms):::.NavigableElement.Style) 次のコード例では、インスタンスが必要なコントロールのプロパティに直接割り当てられている同等の C# ページを示します。

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

ターゲットのインスタンスは、および `baseStyle` [`View`](xref::::no-loc(Xamarin.Forms):::.View) プロパティを設定し [`HorizontalOptions`](xref::::no-loc(Xamarin.Forms):::.View.HorizontalOptions) [`VerticalOptions`](xref::::no-loc(Xamarin.Forms):::.View.VerticalOptions) ます。 は、 `baseStyle` コントロールに直接設定されません。 代わりに、 `labelStyle` とを `buttonStyle` 継承し、追加のバインド可能なプロパティ値を設定します。 次に、 `labelStyle` `buttonStyle` プロパティを設定する [`Label`](xref::::no-loc(Xamarin.Forms):::.Label) ことにより、とがインスタンスとインスタンスに適用され [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) [`Style`](xref::::no-loc(Xamarin.Forms):::.NavigableElement.Style) ます。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref::::no-loc(Xamarin.Forms):::.ResourceDictionary)
- [Style](xref::::no-loc(Xamarin.Forms):::.Style)
- [Setter](xref::::no-loc(Xamarin.Forms):::.Setter)
