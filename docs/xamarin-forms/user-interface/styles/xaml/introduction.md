---
title: Xamarin. Forms スタイルの概要
description: スタイルを使用すると、ビジュアル要素の外観をカスタマイズできます。 スタイルは特定の型に対して定義され、その型で使用可能なプロパティの値を含んでいます。
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 35f8dad3590c07ceb3c93aa735b8c02d75098498
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70228165"
---
# <a name="introduction-to-xamarinforms-styles"></a>Xamarin. Forms スタイルの概要

_スタイルを使用すると、ビジュアル要素の外観をカスタマイズできます。スタイルは特定の型に対して定義され、その型で使用可能なプロパティの値を含んでいます。_

多くの場合、Xamarin アプリケーションには、同じ外観を持つ複数のコントロールが含まれています。 たとえば、次の XAML コード例に示すように、同じフォントオプションとレイアウトオプションを持つ複数の[`Label`](xref:Xamarin.Forms.Label)インスタンスをアプリケーションに含めることができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

次に示すのは、C# で作成された同等のページのコード例です。

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

各[`Label`](xref:Xamarin.Forms.Label)インスタンスには、`Label`によって表示されるテキストの外観を制御するためのプロパティ値が同じです。 これで、次のスクリーンショットのような結果になります。

[スタイルを使用しないラベルの外観の![](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

個々のコントロールの外観を設定すると、繰り返しが発生し、エラーが発生しやすくなります。 代わりに、外観を定義し、必要なコントロールに適用するスタイルを作成できます。

## <a name="create-a-style"></a>スタイルの作成

[`Style`](xref:Xamarin.Forms.Style)クラスは、プロパティ値のコレクションを1つのオブジェクトにグループ化して、複数のビジュアル要素インスタンスに適用できるようにします。 これにより、繰り返し表示されるマークアップを減らし、アプリケーションの外観を簡単に変更できるようになります。

スタイルは主に XAML ベースのアプリケーション向けに設計されていますがC#、次のように作成することもできます。

- XAML で作成された[`Style`](xref:Xamarin.Forms.Style)インスタンスは、通常、コントロール、ページ、またはアプリケーションの[`Resources`](xref:Xamarin.Forms.Application.Resources)コレクション[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)に割り当てられた[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)で定義されます。
- でC#作成された[`Style`](xref:Xamarin.Forms.Style)インスタンスは、通常、ページのクラス、またはグローバルにアクセスできるクラスで定義されます。

[`Style`](xref:Xamarin.Forms.Style) を定義する場所の選択は、使用できる場所に影響があります。

- コントロールレベルで定義されている[`Style`](xref:Xamarin.Forms.Style)インスタンスは、コントロールとその子にのみ適用できます。
- ページレベルで定義されている[`Style`](xref:Xamarin.Forms.Style)インスタンスは、ページとその子にのみ適用できます。
- アプリケーションレベルで定義されている[`Style`](xref:Xamarin.Forms.Style)インスタンスは、アプリケーション全体で適用できます。

各[`Style`](xref:Xamarin.Forms.Style)インスタンスには、1つまたは複数の[`Setter`](xref:Xamarin.Forms.Setter)オブジェクトのコレクションが含まれており、各 `Setter` には[`Property`](xref:Xamarin.Forms.Setter.Property)と[`Value`](xref:Xamarin.Forms.Setter.Value)があります。 `Property` は、スタイルが適用される要素のバインド可能なプロパティの名前であり、`Value` はプロパティに適用される値です。

各[`Style`](xref:Xamarin.Forms.Style)インスタンスは、*明示的*または*暗黙的*に指定できます。

- *明示的*な[`Style`](xref:Xamarin.Forms.Style)インスタンスは、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)と `x:Key` 値を指定し、ターゲット要素の[`Style`](xref:Xamarin.Forms.NavigableElement.Style)プロパティを `x:Key` 参照に設定することによって定義されます。 *明示的*スタイルの詳細については、「[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)」を参照してください。
- *暗黙*の[`Style`](xref:Xamarin.Forms.Style)インスタンスは、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)のみを指定することによって定義されます。 その後、`Style` インスタンスは、その型のすべての要素に自動的に適用されます。 `TargetType` のサブクラスには `Style` 適用されないことに注意してください。 *暗黙的*なスタイルの詳細については、「[暗黙的なスタイル](~/xamarin-forms/user-interface/styles/implicit.md)」を参照してください。

[`Style`](xref:Xamarin.Forms.Style)を作成する場合、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティは常に必須です。 次のコード例は、XAML で作成された*明示的*なスタイル (`x:Key`に注意してください) を示しています。

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

`Style`を適用するには、次の XAML コード例に示すように、ターゲットオブジェクトは `Style`の[`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティ値に一致する[`VisualElement`](xref:Xamarin.Forms.VisualElement)である必要があります。

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

ビュー階層の下位にあるスタイルは、上位に定義されているスタイルよりも優先されます。 たとえば、 [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor)をアプリケーションレベルで `Red` に設定する[`Style`](xref:Xamarin.Forms.Style)の設定は、`Label.TextColor` を `Green`に設定するページレベルのスタイルによってオーバーライドされます。 同様に、ページレベルのスタイルは、コントロールレベルのスタイルによってオーバーライドされます。 また、コントロールプロパティで `Label.TextColor` が直接設定されている場合は、どのスタイルよりも優先されます。

このセクションの記事では、*明示的*なスタイルと*暗黙的*なスタイルを作成して適用する方法、グローバルスタイルを作成する方法、スタイルを継承する方法、実行時にスタイルの変更に応答する方法、およびに含まれる組み込みのスタイルを使用する方法について説明します。Xamarin. フォーム。

> [!NOTE]
> **スタイル Id とは**
>
> Pixate プロパティ[`StyleId`](xref:Xamarin.Forms.Element.StyleId)は、Xamarin. Forms 2.2 より前に、UI テストで識別するアプリケーション内の個々の要素を識別するために使用されていました。また、などのテーマエンジンでも使用されていました。 ただし2.2、 [`StyleId`](xref:Xamarin.Forms.Element.StyleId)プロパティを置き換えた[`AutomationId`](xref:Xamarin.Forms.Element.AutomationId)プロパティが導入されました。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [スタイル](xref:Xamarin.Forms.Style)
- [メソッド](xref:Xamarin.Forms.Setter)
