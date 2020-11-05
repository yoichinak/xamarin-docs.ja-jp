---
title: 暗黙的なスタイル Xamarin.Forms
description: 暗黙的なスタイルとは、同じ TargetType のすべてのコントロールで使用されるスタイルで、各コントロールがスタイルを参照する必要がありません。
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d986d66b9b83bb1034c9e635c3a35f2a0ac3dfda
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93369065"
---
# <a name="implicit-styles-in-no-locxamarinforms"></a>暗黙的なスタイル Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_暗黙的なスタイルとは、同じ TargetType のすべてのコントロールで使用されるスタイルで、各コントロールがスタイルを参照する必要がありません。_

## <a name="create-an-implicit-style-in-xaml"></a>XAML で暗黙的なスタイルを作成する

ページレベルでを宣言するには、 [`Style`](xref:Xamarin.Forms.Style) を [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ページに追加し、1つ以上の宣言をに含める必要があり `Style` `ResourceDictionary` ます。 は、 `Style` 属性を指定しないことによって *暗黙的* に作成され `x:Key` ます。 スタイルは、厳密に一致するものの、 `TargetType` 値から派生した要素には適用されません `TargetType` 。

次のコード例では *implicit* 、ページの `ResourceDictionary` インスタンスに適用される、XAML で宣言された暗黙的なスタイルを示してい [`Entry`](xref:Xamarin.Forms.Entry) ます。

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

は、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ページのインスタンスに適用される単一の *暗黙的* なスタイルを定義し [`Entry`](xref:Xamarin.Forms.Entry) ます。 は、 `Style` 青いテキストを黄色の背景に表示するために使用されます。また、他の外観オプションも設定します。 は、 `Style` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 属性を指定せずにページのに追加され `x:Key` ます。 したがって、は、の `Style` プロパティと完全に一致するすべてのインスタンスに暗黙的に適用され `Entry` [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` ます。 ただし、は、サブクラス化された `Style` インスタンスには適用されません `CustomEntry` `Entry` 。 この結果、次のスクリーンショットに示すような外観が表示されます。

[![暗黙的なスタイルの例](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

さらに、4番目のは、 [`Entry`](xref:Xamarin.Forms.Entry) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) 暗黙的なスタイルのプロパティとプロパティを異なる値にオーバーライドし `Color` ます。

### <a name="create-an-implicit-style-at-the-control-level"></a>コントロールレベルでの暗黙的なスタイルの作成

次のコード例に示すように、ページレベルで *暗黙的* なスタイルを作成するだけでなく、コントロールレベルで作成することもできます。

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

この例では、 *implicit* [`Style`](xref:Xamarin.Forms.Style) コントロールのコレクションに暗黙的なが割り当てられてい [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。 その後、 *暗黙的* なスタイルをコントロールとその子に適用できます。

アプリケーションのでスタイルを作成する方法の詳細につい [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ては、「 [グローバルスタイル](~/xamarin-forms/user-interface/styles/application.md)」を参照してください。

## <a name="create-an-implicit-style-in-c35"></a>C&#35; で暗黙のスタイルを作成する

[`Style`](xref:Xamarin.Forms.Style) C# のページコレクションにインスタンスを追加するには、 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 次の `Style` `ResourceDictionary` コード例に示すように、新しいを作成し、インスタンスをに追加します。

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

コンストラクターは、ページのインスタンスに適用される単一の *暗黙的* なスタイルを定義し [`Entry`](xref:Xamarin.Forms.Entry) ます。 は、 `Style` 青いテキストを黄色の背景に表示するために使用されます。また、他の外観オプションも設定します。 は、 `Style` [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 文字列を指定せずにページのに追加され `key` ます。 したがって、は、の `Style` プロパティと完全に一致するすべてのインスタンスに暗黙的に適用され `Entry` [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` ます。 ただし、は、サブクラス化された `Style` インスタンスには適用されません `CustomEntry` `Entry` 。

## <a name="apply-a-style-to-derived-types"></a>派生型へのスタイルの適用

プロパティを使用すると、 [`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) プロパティによって参照される基本型から派生したコントロールにスタイルを適用でき [`TargetType`](xref:Xamarin.Forms.Style.TargetType) ます。 したがって、このプロパティをに設定すると、 `true` 型がプロパティで指定された基本型から派生していれば、1つのスタイルで複数の型を対象にすることができ `TargetType` ます。

次の例は、インスタンスの背景色を赤に設定する暗黙的なスタイルを示してい [`Button`](xref:Xamarin.Forms.Button) ます。

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

このスタイルをページレベルで配置する [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) と、ページ上のすべてのインスタンスに適用され、 [`Button`](xref:Xamarin.Forms.Button) から派生したコントロールにも適用され `Button` ます。 ただし、プロパティが未設定のままである場合、 [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) スタイルはインスタンスにのみ適用され `Button` ます。

これに相当する C# コードを次に示します。

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
- [基本スタイル (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [スタイルの使用 (サンプル)](/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)