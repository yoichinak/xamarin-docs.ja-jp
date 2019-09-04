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
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228165"
---
# <a name="introduction-to-xamarinforms-styles"></a>Xamarin. Forms スタイルの概要

_スタイルを使用すると、ビジュアル要素の外観をカスタマイズできます。スタイルは特定の型に対して定義され、その型で使用可能なプロパティの値を含んでいます。_

多くの場合、Xamarin アプリケーションには、同じ外観を持つ複数のコントロールが含まれています。 たとえば、次の XAML コード例に[`Label`](xref:Xamarin.Forms.Label)示すように、同じフォントオプションとレイアウトオプションを持つ複数のインスタンスをアプリケーションに含めることができます。

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

各[`Label`](xref:Xamarin.Forms.Label)インスタンスには、 `Label`によって表示されるテキストの外観を制御するためのプロパティ値が同一です。 次のスクリーン ショットに示すように外観が発生します。

[![スタイルを使用しないラベルの外観](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

個々のコントロールの外観を設定すると、繰り返しが発生し、エラーが発生しやすくなります。 代わりに、外観を定義し、必要なコントロールに適用するスタイルを作成できます。

## <a name="create-a-style"></a>スタイルの作成

クラス[`Style`](xref:Xamarin.Forms.Style)は、プロパティ値のコレクションを1つのオブジェクトにグループ化して、複数のビジュアル要素インスタンスに適用できるようにします。 これにより、繰り返し表示されるマークアップを減らし、アプリケーションの外観を簡単に変更できるようになります。

スタイルは主に XAML ベースのアプリケーション向けに設計されていますがC#、次のように作成することもできます。

- [`Style`](xref:Xamarin.Forms.Style) XAML で作成されたインスタンスが通常で定義されている、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)に割り当てられている、 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 、コントロールのコレクションページで、または、 [ `Resources` ](xref:Xamarin.Forms.Application.Resources) 、アプリケーションのコレクション。
- [`Style`](xref:Xamarin.Forms.Style)でC#作成されたインスタンスは、通常、ページのクラス、またはグローバルにアクセスできるクラスで定義されます。

[`Style`](xref:Xamarin.Forms.Style) を定義する場所の選択は、使用できる場所に影響があります。

- [`Style`](xref:Xamarin.Forms.Style)コントロールレベルで定義されたインスタンスは、コントロールとその子にのみ適用できます。
- [`Style`](xref:Xamarin.Forms.Style)ページレベルで定義されたインスタンスは、ページとその子にのみ適用できます。
- [`Style`](xref:Xamarin.Forms.Style)アプリケーションレベルで定義されたインスタンスは、アプリケーション全体で適用できます。

各[ `Style` ](xref:Xamarin.Forms.Style)インスタンスには、1 つまたは複数のコレクションが含まれています[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクトは、各`Setter`を持つ、 [ `Property` ](xref:Xamarin.Forms.Setter.Property)および[`Value`](xref:Xamarin.Forms.Setter.Value)。 は、スタイルが適用される要素のバインド可能なプロパティの名前です。は、 `Value`プロパティに適用される値です。 `Property`

各[`Style`](xref:Xamarin.Forms.Style)インスタンスは、*明示的*または*暗黙的*に指定できます。

- *明示的* [`Style`](xref:Xamarin.Forms.Style)なインスタンスは、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)との`x:Key`値を指定`x:Key`し、ターゲット要素の[`Style`](xref:Xamarin.Forms.NavigableElement.Style)プロパティを参照に設定することによって定義されます。 *明示的*スタイルの詳細については、「[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)」を参照してください。
- [`TargetType`](xref:Xamarin.Forms.Style.TargetType)*暗黙* [`Style`](xref:Xamarin.Forms.Style)のインスタンスは、のみを指定することによって定義されます。 その`Style`後、インスタンスは、その型のすべての要素に自動的に適用されます。 の`TargetType`サブクラスには、 `Style`が自動的に適用されないことに注意してください。 *暗黙的*なスタイルの詳細については、「[暗黙的なスタイル](~/xamarin-forms/user-interface/styles/implicit.md)」を参照してください。

を作成[`Style`](xref:Xamarin.Forms.Style)する場合[`TargetType`](xref:Xamarin.Forms.Style.TargetType) 、プロパティは常に必須です。 次のコード例は、XAML で作成され`x:Key`た*明示的*なスタイル (に注意してください) を示しています。

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

を適用`Style`するには、次の XAML コード例に示すように、ターゲット`Style`オブジェクトはの[`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティ値と一致[`VisualElement`](xref:Xamarin.Forms.VisualElement)するである必要があります。

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

スタイルのビュー階層の下位には、アップ以上定義されているものよりも優先されます。 設定など、 [ `Style` ](xref:Xamarin.Forms.Style)設定[ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor)に`Red`アプリケーションでレベルを設定 ページのレベルのスタイルによってオーバーライドされます`Label.TextColor`に`Green`. 同様に、ページ レベルのスタイルはコントロールのレベルのスタイルによってオーバーライドされます。 また、がコントロール`Label.TextColor`プロパティに直接設定されている場合は、すべてのスタイルよりも優先されます。

このセクションの記事では、*明示的*なスタイルと*暗黙的*なスタイルを作成して適用する方法、グローバルスタイルを作成する方法、スタイルを継承する方法、実行時にスタイルの変更に応答する方法、およびに含まれる組み込みのスタイルを使用する方法について説明します。Xamarin. フォーム。

> [!NOTE]
> **スタイル Id とは**
>
> Xamarin. Forms 2.2 より前のプロパティ[`StyleId`](xref:Xamarin.Forms.Element.StyleId)は、UI テストで識別するアプリケーション内の個々の要素、および Pixate などのテーマエンジンを識別するために使用されていました。 ただし2.2、プロパティが[`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) [`StyleId`](xref:Xamarin.Forms.Element.StyleId)置き換えられたプロパティが導入されました。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
