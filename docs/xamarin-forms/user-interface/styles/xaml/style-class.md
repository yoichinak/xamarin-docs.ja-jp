---
title: Xamarin.Forms のスタイル クラス
description: Xamarin.Forms のスタイル クラスには、スタイルの継承を使用しなくても、コントロールに適用する複数のスタイルが有効にします。
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: dd749a4a78adbab5317f1ae5ca6334caa009b9b3
ms.sourcegitcommit: 9dcb7377dc92ad921285fbb857b0be13030bbea3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668551"
---
# <a name="xamarinforms-style-classes"></a>Xamarin.Forms のスタイル クラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Xamarin.Forms のスタイル クラスには、スタイルの継承を使用しなくても、コントロールに適用する複数のスタイルが有効にします。_

## <a name="create-style-classes"></a>スタイル クラスを作成します。

スタイル クラスを設定して作成できます、 [ `Class` ](xref:Xamarin.Forms.Style.Class)プロパティを[ `Style` ](xref:Xamarin.Forms.Style)を`string`クラスの名前を表します。 これにより、明示的なスタイルを使用して、定義した場合に利用、`x:Key`属性は、複数のスタイル クラスに適用できること、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。

> [!IMPORTANT]
> さまざまな種類を対象に提供される、複数のスタイルは、同じクラス名を共有できます。 これにより、さまざまな種類の対象に、同じ名前を複数のスタイル クラスができます。

次の例では 3 つ[ `BoxView` ](xref:Xamarin.Forms.BoxView)クラスのスタイル設定、 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)スタイル クラス。

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

`Separator`、 `Rounded`、および`Circle`スタイル クラスの各セット[ `BoxView` ](xref:Xamarin.Forms.BoxView)プロパティを特定の値にします。

`Rotated`スタイル クラスには、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)の[ `VisualElement` ](xref:Xamarin.Forms.VisualElement)、つまりのみに適用できる`VisualElement`インスタンス。 ただし、その[ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)プロパティに設定されて`true`から派生したコントロールに適用できることが保証`VisualElement`など[ `BoxView`](xref:Xamarin.Forms.BoxView)します。 派生型にスタイルを適用する方法についての詳細については、[派生型にスタイルを適用](implicit.md#apply-a-style-to-derived-types)を参照してください。

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

## <a name="consume-style-classes"></a>スタイル クラスを使用します。

設定でスタイル クラスを使用できる、 [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass)プロパティの型であるコントロールの`IList<string>`、スタイル クラス名の一覧にします。 コントロールの型と一致すること、スタイル クラスが適用されます、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)スタイルのクラス。

次の例では 3 つ[ `BoxView` ](xref:Xamarin.Forms.BoxView)インスタンスでは、それぞれ異なるスタイル クラスに設定します。

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

この例では、最初の[ `BoxView` ](xref:Xamarin.Forms.BoxView)する 3 番目の行区切り記号のスタイルが`BoxView`が循環します。 2 番目の`BoxView`が 2 つのスタイル クラスに適用し、it が丸められますの角を与えるし、45 度回転します。

![](style-class-images/boxviews.png "BoxViews スタイル クラスのスタイル設定")

> [!IMPORTANT]
> コントロールに複数のスタイル クラスを適用できます、 [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass)プロパティの型は`IList<string>`します。 この場合、一覧の順序の昇順でスタイル クラスが適用されます。 そのため、複数のスタイル クラスでは、同じプロパティを設定するときに最高のリストの位置にあるスタイル クラスでプロパティが優先されます。

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

- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
