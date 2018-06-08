---
title: スタイルの継承
description: スタイルは、重複を減らすし、再利用できるようにするには、他のスタイルを継承できます。
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7b489e90daec2659a6d11b2776731582bdf368ff
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848422"
---
# <a name="style-inheritance"></a>スタイルの継承

_スタイルは、重複を減らすし、再利用できるようにするには、他のスタイルを継承できます。_

## <a name="style-inheritance-in-xaml"></a>XAML でのスタイルの継承

スタイルの継承を設定して実行、 [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/)既存プロパティ[ `Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)です。 XAML では、これを実現するには、`BasedOn`プロパティを`StaticResource`以前に作成したを参照するマークアップ拡張機能`Style`します。 C# の場合、これを実現するには、`BasedOn`プロパティを`Style`インスタンス。

基本のスタイルを継承するスタイルを含めることができます[ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) 、新しいプロパティのインスタンスか、または基本スタイルのスタイルをオーバーライドに使用します。 さらに、スタイル、基本のスタイルを継承する必要がありますまたは対象に、同じ型で基本スタイルの対象となる型から派生する型。 たとえば、基本スタイル ターゲット[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)インスタンス、基本のスタイルに基づくスタイルを対象にできます`View`インスタンスまたはから派生した型、`View`クラスなど[ `Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)と[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)インスタンス。

次のコード例*明示的な*XAML ページのスタイルを継承します。

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

`baseStyle`ターゲット[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)インスタンスし、設定、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティです。 `baseStyle`が直接コントロールに対して設定されていません。 代わりに、`labelStyle`と`buttonStyle`追加のバインド可能なプロパティ値の設定をそれを継承します。 `labelStyle`と`buttonStyle`が適用されます、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスと[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)を設定して、インスタンス、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> 暗黙的なスタイルは、明示的なスタイルから派生することができますが、明示的なスタイルは、暗黙的なスタイルから派生することはできません。

### <a name="respecting-the-inheritance-chain"></a>継承チェーンを考慮し

スタイルは、同じレベル以上のスタイルからのみ継承できます階層の表示にします。 これによって、次のことが起こります。

- アプリケーション レベルのリソースは、その他のアプリケーション レベル リソースからのみ継承できます。
- ページ レベル リソースをアプリケーション レベル リソース、およびその他のページ レベル リソースから継承できます。
- コントロール レベルのリソースは、アプリケーション レベルのリソース、ページ レベル リソース、およびその他のコントロール レベルのリソースから継承できます。

この継承チェーンの例は次のコード例を示します。

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

この例では`labelStyle`と`buttonStyle`コントロール レベルのリソースは、中に`baseStyle`ページ レベル リソースがします。 しかし、`labelStyle`と`buttonStyle`から継承`baseStyle`のことはできません`baseStyle`から継承する`labelStyle`または`buttonStyle`ビュー階層内ではそれぞれの場所にします。

## <a name="style-inheritance-in-c35"></a>C スタイルの継承&#35;

等価 (C#) ページで、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンスに直接割り当てられた、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)次のコード例に、必要なコントロールのプロパティが表示されます。

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

`baseStyle`ターゲット[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)インスタンスし、設定、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティです。 `baseStyle`が直接コントロールに対して設定されていません。 代わりに、`labelStyle`と`buttonStyle`追加のバインド可能なプロパティ値の設定をそれを継承します。 `labelStyle`と`buttonStyle`が適用されます、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスと[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)を設定して、インスタンス、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。

## <a name="summary"></a>まとめ

スタイルは、重複を減らすし、再利用できるようにするには、他のスタイルを継承できます。 スタイルの継承を設定して実行、 [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/)既存プロパティ[ `Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)です。


## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
