---
title: Xamarin.Forms スタイルクラス
description: Xamarin.Forms スタイルクラスを使用すると、スタイルの継承を使用せずに、コントロールに複数のスタイルを適用できます。
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f100f98418b7e3cb82939bf67dda61b66cb5864e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557752"
---
# <a name="no-locxamarinforms-style-classes"></a>Xamarin.Forms スタイルクラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Xamarin.Forms スタイルクラスを使用すると、スタイルの継承を使用せずに、コントロールに複数のスタイルを適用できます。_

## <a name="create-style-classes"></a>スタイルクラスの作成

スタイルクラスを作成するには [`Class`](xref:Xamarin.Forms.Style.Class) 、のプロパティをクラス名を表すに設定し [`Style`](xref:Xamarin.Forms.Style) `string` ます。 この機能の利点は、属性を使用して明示的なスタイルを定義するより `x:Key` も、複数のスタイルクラスをに適用できることです [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。

> [!IMPORTANT]
> 複数のスタイルが異なる型をターゲットにしている場合は、同じクラス名を共有できます。 これにより、同じ名前の複数のスタイルクラスを使用して、異なる型をターゲットにすることができます。

次の例は、3つの [`BoxView`](xref:Xamarin.Forms.BoxView) スタイルクラスとスタイルクラスを示してい [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

`Separator`、 `Rounded` 、および `Circle` スタイルクラスはそれぞれ、 [`BoxView`](xref:Xamarin.Forms.BoxView) プロパティを特定の値に設定します。

スタイルクラスには、のがあります。これは、 `Rotated` [`TargetType`](xref:Xamarin.Forms.Style.TargetType) [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンスにのみ適用できることを意味し `VisualElement` ます。 ただし、その [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) プロパティはに設定されます。これにより、など `true` 、から派生したコントロールに適用できるようになり `VisualElement` [`BoxView`](xref:Xamarin.Forms.BoxView) ます。 派生型にスタイルを適用する方法の詳細については、「 [スタイルを派生型に適用する](implicit.md#apply-a-style-to-derived-types)」を参照してください。

これに相当する C# コードを次に示します。

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>スタイルクラスを使用する

スタイルクラスは [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) 、コントロールのプロパティ (型 `IList<string>` ) をスタイルクラス名のリストに設定することによって使用できます。 コントロールの型がスタイルクラスのと一致する場合、スタイルクラスが適用され [`TargetType`](xref:Xamarin.Forms.Style.TargetType) ます。

次の例は [`BoxView`](xref:Xamarin.Forms.BoxView) 、それぞれが異なるスタイルクラスに設定された3つのインスタンスを示しています。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

この例では、最初のは [`BoxView`](xref:Xamarin.Forms.BoxView) 行の区切り記号としてスタイルを設定し、3番目の行は円で示してい `BoxView` ます。 2つ目の `BoxView` スタイルクラスが適用されます。これにより、角が丸くなり、45度回転します。

![BoxViews スタイルクラスでスタイルを適用する](style-class-images/boxviews.png)

> [!IMPORTANT]
> プロパティが型であるため、複数のスタイルクラスをコントロールに適用でき [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) `IList<string>` ます。 この場合、スタイルクラスは昇順のリストの順序で適用されます。 したがって、複数のスタイルクラスが同一のプロパティを設定すると、リストの最上位の位置にあるスタイルクラスのプロパティが優先されます。

これに相当する C# コードを次に示します。

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>関連リンク

- [基本スタイル (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)