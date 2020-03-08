---
title: Xamarin.Forms での暗黙的なスタイル
description: 暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: d58ba81596cccf470b7246514d71f35968599880
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918127"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms での暗黙的なスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_暗黙的なスタイルとは、同じ TargetType のすべてのコントロールで使用されるスタイルで、各コントロールがスタイルを参照する必要がありません。_

## <a name="create-an-implicit-style-in-xaml"></a>XAML で暗黙的なスタイルを作成する

ページレベルで[`Style`](xref:Xamarin.Forms.Style)を宣言するには、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)をページに追加する必要があります。その後、1つまたは複数の `Style` 宣言を `ResourceDictionary`に含めることができます。 `Style` は、`x:Key` 属性を指定しないことによって*暗黙的*に行われます。 次に、`TargetType` に一致するビジュアル要素にスタイルが適用されますが、`TargetType` 値から派生した要素には適用されません。

次のコード例は、ページの `ResourceDictionary`の XAML で宣言され、ページの[`Entry`](xref:Xamarin.Forms.Entry)インスタンスに適用される*暗黙的*なスタイルを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)は、ページの[`Entry`](xref:Xamarin.Forms.Entry)インスタンスに適用される単一の*暗黙的*なスタイルを定義します。 `Style` は、青いテキストを黄色の背景に表示するために使用されます。また、その他の外観オプションも設定します。 `Style` は、`x:Key` 属性を指定せずにページの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加されます。 したがって、`Style` は `Style` の[`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティと完全に一致するため、すべての `Entry` インスタンスに暗黙的に適用されます。 ただし、`Style` は、サブクラス化された `Entry`である `CustomEntry` インスタンスには適用されません。 これで、次のスクリーンショットのような結果になります。

[![の暗黙的なスタイルの例](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

また、4番目の[`Entry`](xref:Xamarin.Forms.Entry)は、暗黙的なスタイルの[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)と[`TextColor`](xref:Xamarin.Forms.InputView.TextColor)プロパティを異なる `Color` 値にオーバーライドします。

### <a name="create-an-implicit-style-at-the-control-level"></a>コントロールレベルでの暗黙的なスタイルの作成

次のコード例に示すように、ページレベルで*暗黙的*なスタイルを作成するだけでなく、コントロールレベルで作成することもできます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

この例では、*暗黙的*な[`Style`](xref:Xamarin.Forms.Style)が[`StackLayout`](xref:Xamarin.Forms.StackLayout)コントロールの[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)コレクションに割り当てられています。 その後、*暗黙的*なスタイルをコントロールとその子に適用できます。

アプリケーションの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)でスタイルを作成する方法の詳細については、「[グローバルスタイル](~/xamarin-forms/user-interface/styles/application.md)」を参照してください。

## <a name="create-an-implicit-style-in-c35"></a>C での暗黙的なスタイルの作成&#35;

次のコード例に示すように、新しい[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)をC#作成し、`Style` インスタンスを `ResourceDictionary`に追加することによって、のページの[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)コレクションに[`Style`](xref:Xamarin.Forms.Style)インスタンスを追加できます。

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

コンストラクターは、ページの[`Entry`](xref:Xamarin.Forms.Entry)インスタンスに適用される単一の*暗黙的*なスタイルを定義します。 `Style` は、青いテキストを黄色の背景に表示するために使用されます。また、その他の外観オプションも設定します。 `Style` は、`key` 文字列を指定せずにページの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に追加されます。 したがって、`Style` は `Style` の[`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティと完全に一致するため、すべての `Entry` インスタンスに暗黙的に適用されます。 ただし、`Style` は、サブクラス化された `Entry`である `CustomEntry` インスタンスには適用されません。

## <a name="apply-a-style-to-derived-types"></a>派生型へのスタイルの適用

[`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)プロパティを使用すると、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティによって参照される基本型から派生したコントロールにスタイルを適用できます。 したがって、このプロパティを `true` に設定すると、型が `TargetType` プロパティで指定された基本型から派生していれば、1つのスタイルで複数の型を対象にすることができます。

次の例は、 [`Button`](xref:Xamarin.Forms.Button)インスタンスの背景色を赤に設定する暗黙的なスタイルを示しています。

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

このスタイルをページレベルの[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)に配置すると、ページ上のすべての[`Button`](xref:Xamarin.Forms.Button)インスタンス、および `Button`から派生したコントロールにも適用されます。 ただし、 [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)プロパティが未設定のままになっている場合、スタイルは `Button` インスタンスにのみ適用されます。

同等の C# コードを次に示します。

```csharp
var buttonStyle = new Style(typeof(Button))
{
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.Red
        }
    }
};

Resources = new ResourceDictionary { buttonStyle };
```

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [メソッド](xref:Xamarin.Forms.Setter)
