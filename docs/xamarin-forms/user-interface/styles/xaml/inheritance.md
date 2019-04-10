---
title: Xamarin.Forms でスタイル継承
description: スタイルは、重複を削減し、再利用を有効にするには、その他のスタイルを継承できます。 この記事では、Xamarin.Forms アプリケーションでスタイルの継承を実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: bef48db93ae76346802b6569080bb1e54e3e51b3
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55291948"
---
# <a name="style-inheritance-in-xamarinforms"></a>Xamarin.Forms でスタイル継承

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_スタイルは、重複を削減し、再利用を有効にするには、その他のスタイルを継承できます。_

## <a name="style-inheritance-in-xaml"></a>XAML でスタイル継承

スタイルの継承を設定して実行、 [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn)プロパティを既存[ `Style`](xref:Xamarin.Forms.Style)します。 XAML に設定してこれは、`BasedOn`プロパティを`StaticResource`以前に作成した参照するマークアップ拡張機能`Style`します。 C# での設定でこれは、`BasedOn`プロパティを`Style`インスタンス。

基本のスタイルを継承するスタイルを含めることができます[ `Setter` ](xref:Xamarin.Forms.Setter) 、新しいプロパティのインスタンスか、それらを使用して基本のスタイルのスタイルをオーバーライドします。 さらに、基本のスタイルを継承するスタイルでは、同じ型、または基本のスタイルの対象となる型から派生した型をターゲットする必要があります。 たとえば、基本のスタイルのターゲット[ `View` ](xref:Xamarin.Forms.View)インスタンス、基本のスタイルに基づくスタイルを対象にできます`View`インスタンスまたはから派生する型、`View`クラスなど、 [ `Label`](xref:Xamarin.Forms.Label)と[ `Button` ](xref:Xamarin.Forms.Button)インスタンス。

次のコード例*明示的な*XAML ページで継承をスタイル設定します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
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

`baseStyle`ターゲット[ `View` ](xref:Xamarin.Forms.View)インスタンス、および設定、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティ。 `baseStyle`がコントロール上で直接設定されていません。 代わりに、`labelStyle`と`buttonStyle`バインド可能な追加のプロパティ値の設定をそれを継承します。 `labelStyle`と`buttonStyle`に適用される、 [ `Label` ](xref:Xamarin.Forms.Label)インスタンスと[ `Button` ](xref:Xamarin.Forms.Button)を設定して、インスタンス、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティ。 次のスクリーン ショットに示すように外観が発生します。

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> 暗黙的なスタイルは、明示的なスタイルから派生できますが、明示的なスタイルは暗黙的なスタイルから派生することはできません。

### <a name="respecting-the-inheritance-chain"></a>継承チェーンを尊重し

スタイルは、同じレベル以上のスタイルからのみ継承できますで階層を表示します。 これによって、次のことが起こります。

- アプリケーション レベルのリソースは、その他のアプリケーション レベルのリソースからのみ継承できます。
- ページ レベルのリソースは、アプリケーション レベルのリソース、およびその他のページ レベル リソースから継承できます。
- コントロールのレベルのリソースは、アプリケーション レベルのリソース、ページ レベル リソース、およびその他のコントロール レベルのリソースから継承できます。

この継承チェーンは次のコード例について説明します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
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

この例で`labelStyle`と`buttonStyle`レベルのリソースをコントロールには中に`baseStyle`はページ レベル リソースです。 ただし、`labelStyle`と`buttonStyle`継承`baseStyle`のことはできません`baseStyle`から継承する`labelStyle`または`buttonStyle`はそれぞれの場所で階層を表示している。

## <a name="style-inheritance-in-c35"></a>C スタイルの継承&#35;

同等の C# ページで、 [ `Style` ](xref:Xamarin.Forms.Style)インスタンスに直接割り当てられた、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 、必要なコントロールのプロパティが次のコード例に示すように。

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

`baseStyle`ターゲット[ `View` ](xref:Xamarin.Forms.View)インスタンス、および設定、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティ。 `baseStyle`がコントロール上で直接設定されていません。 代わりに、`labelStyle`と`buttonStyle`バインド可能な追加のプロパティ値の設定をそれを継承します。 `labelStyle`と`buttonStyle`に適用される、 [ `Label` ](xref:Xamarin.Forms.Label)インスタンスと[ `Button` ](xref:Xamarin.Forms.Button)を設定して、インスタンス、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティ。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
