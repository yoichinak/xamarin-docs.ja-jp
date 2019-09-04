---
title: Xamarin 形式のスタイルクラス
description: Xamarin. Forms スタイルクラスを使用すると、スタイルの継承を使用せずに、コントロールに複数のスタイルを適用できます。
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: 4a353d64f0e7e29da6c64f93b8554c3661f4d389
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228126"
---
# <a name="xamarinforms-style-classes"></a>Xamarin 形式のスタイルクラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Xamarin. Forms スタイルクラスを使用すると、スタイルの継承を使用せずに、コントロールに複数のスタイルを適用できます。_

## <a name="create-style-classes"></a>スタイルクラスの作成

スタイルクラスを作成するには、の[`Class`](xref:Xamarin.Forms.Style.Class) [`Style`](xref:Xamarin.Forms.Style) `string`プロパティをクラス名を表すに設定します。 この機能の利点は、 `x:Key`属性を使用して明示的なスタイルを定義するよりも、複数のスタイルクラスを[`VisualElement`](xref:Xamarin.Forms.VisualElement)に適用できることです。

> [!IMPORTANT]
> 複数のスタイルが異なる型をターゲットにしている場合は、同じクラス名を共有できます。 これにより、同じ名前の複数のスタイルクラスを使用して、異なる型をターゲットにすることができます。

次の例は、 [`BoxView`](xref:Xamarin.Forms.BoxView) 3 つのスタイルクラス[`VisualElement`](xref:Xamarin.Forms.VisualElement)とスタイルクラスを示しています。

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

、 `Separator` 、`Rounded` [`BoxView`](xref:Xamarin.Forms.BoxView)およびスタイルクラスはそれぞれ、プロパティを特定の値に設定します。`Circle`

スタイルクラスには、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)の[`VisualElement`](xref:Xamarin.Forms.VisualElement)があります。これは、インスタンスに`VisualElement`のみ適用できることを意味します。 `Rotated` ただし、その[`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)プロパティはに`true`設定され[`BoxView`](xref:Xamarin.Forms.BoxView)ます。これにより、など、から`VisualElement`派生したコントロールに適用できるようになります。 派生型にスタイルを適用する方法の詳細については、「[スタイルを派生型に適用する](implicit.md#apply-a-style-to-derived-types)」を参照してください。

同等の C# コードに示します。

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

スタイルクラスは、コントロールの[`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)プロパティ (型`IList<string>`) をスタイルクラス名のリストに設定することによって使用できます。 コントロールの型がスタイルクラス[`TargetType`](xref:Xamarin.Forms.Style.TargetType)のと一致する場合、スタイルクラスが適用されます。

次の例は、 [`BoxView`](xref:Xamarin.Forms.BoxView)それぞれが異なるスタイルクラスに設定された3つのインスタンスを示しています。

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

この例では、最初[`BoxView`](xref:Xamarin.Forms.BoxView)のは行の区切り記号としてスタイルを`BoxView`設定し、3番目の行は円で示しています。 2つ`BoxView`目のスタイルクラスが適用されます。これにより、角が丸くなり、45度回転します。

![BoxViews スタイルクラスでスタイルを適用する](style-class-images/boxviews.png)

> [!IMPORTANT]
> [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)プロパティが型`IList<string>`であるため、複数のスタイルクラスをコントロールに適用できます。 この場合、スタイルクラスは昇順のリストの順序で適用されます。 したがって、複数のスタイルクラスが同一のプロパティを設定すると、リストの最上位の位置にあるスタイルクラスのプロパティが優先されます。

同等の C# コードに示します。

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

- [基本的なスタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
