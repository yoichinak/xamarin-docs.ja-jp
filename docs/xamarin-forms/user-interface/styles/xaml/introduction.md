---
title: "スタイルの概要 Xamarin.Forms " 説明: "スタイルを使用すると、ビジュアル要素の外観をカスタマイズできます。 スタイルは特定の型に対して定義され、その型で使用可能なプロパティの値を含んでいます。 "
ms. 製品: xamarin ms. assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 04/27/2016 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="introduction-to-xamarinforms-styles"></a>スタイルの概要 Xamarin.Forms

_スタイルを使用すると、ビジュアル要素の外観をカスタマイズできます。スタイルは特定の型に対して定義され、その型で使用可能なプロパティの値を含んでいます。_

Xamarin.Forms多くの場合、アプリケーションには同じ外観を持つ複数のコントロールが含まれています。 たとえば、 [`Label`](xref:Xamarin.Forms.Label) 次の XAML コード例に示すように、同じフォントオプションとレイアウトオプションを持つ複数のインスタンスをアプリケーションに含めることができます。

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

各 [`Label`](xref:Xamarin.Forms.Label) インスタンスには、によって表示されるテキストの外観を制御するためのプロパティ値が同一 `Label` です。 この結果、次のスクリーンショットに示すような外観が表示されます。

[![スタイルを使用しないラベルの外観](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

個々のコントロールの外観を設定すると、繰り返しが発生し、エラーが発生しやすくなります。 代わりに、外観を定義し、必要なコントロールに適用するスタイルを作成できます。

## <a name="create-a-style"></a>スタイルの作成

[`Style`](xref:Xamarin.Forms.Style) クラスは、プロパティ値のコレクションを 1 つのオブジェクトにグループ化して、複数のビジュアル要素インスタンスに適用できるようにします。 これにより、繰り返し表示されるマークアップを減らし、アプリケーションの外観を簡単に変更できるようになります。

スタイルは主に XAML ベースのアプリケーション向けに設計されていますが、C# でも作成できます。

- [`Style`](xref:Xamarin.Forms.Style)XAML で作成されたインスタンスは、通常、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) コントロール、ページ、またはアプリケーションのコレクションに割り当てられたで定義され [`Resources`](xref:Xamarin.Forms.Application.Resources) ます。
- [`Style`](xref:Xamarin.Forms.Style)C# で作成されたインスタンスは、通常、ページのクラス、またはグローバルにアクセスできるクラスで定義されます。

[`Style`](xref:Xamarin.Forms.Style) を定義する場所の選択は、使用できる場所に影響があります。

- [`Style`](xref:Xamarin.Forms.Style)コントロールレベルで定義されたインスタンスは、コントロールとその子にのみ適用できます。
- [`Style`](xref:Xamarin.Forms.Style)ページレベルで定義されたインスタンスは、ページとその子にのみ適用できます。
- アプリケーション レベルで定義された [`Style`](xref:Xamarin.Forms.Style) インスタンスは、アプリケーション全体に適用できます。

各 [`Style`](xref:Xamarin.Forms.Style) インスタンスには、1 つ以上の [`Setter`](xref:Xamarin.Forms.Setter) オブジェクトのコレクションが含まれています。各 `Setter` には、[`Property`](xref:Xamarin.Forms.Setter.Property)と[`Value`](xref:Xamarin.Forms.Setter.Value)があります。 `Property` は、スタイルが適用される要素のバインド可能なプロパティの名前で、`Value` はプロパティに適用される値です。

各 [`Style`](xref:Xamarin.Forms.Style) インスタンスは、*明示的*または*暗黙的*に指定できます。

- *明示的*な [`Style`](xref:Xamarin.Forms.Style) インスタンスは、との値を指定 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) し、 `x:Key` ターゲット要素のプロパティを参照に設定することによって定義され [`Style`](xref:Xamarin.Forms.NavigableElement.Style) `x:Key` ます。 *明示的*スタイルの詳細については、「[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)」を参照してください。
- *暗黙* [`Style`](xref:Xamarin.Forms.Style) のインスタンスは、のみを指定することによって定義され [`TargetType`](xref:Xamarin.Forms.Style.TargetType) ます。 その `Style` 後、インスタンスは、その型のすべての要素に自動的に適用されます。 のサブクラスに `TargetType` は、が自動的に適用されないことに注意して `Style` ください。 *暗黙的*なスタイルの詳細については、「[暗黙的なスタイル](~/xamarin-forms/user-interface/styles/implicit.md)」を参照してください。

[`Style`](xref:Xamarin.Forms.Style) を作成する場合は、[`TargetType`](xref:Xamarin.Forms.Style.TargetType) プロパティが常に必要です。 次のコード例は、XAML で作成された*明示的*なスタイル (に注意してください) を示してい `x:Key` ます。

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

を適用するには、 `Style` [`VisualElement`](xref:Xamarin.Forms.VisualElement) 次の [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` XAML コード例に示すように、ターゲットオブジェクトはのプロパティ値と一致するである必要があります。

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

ビュー階層の下位にあるスタイルは、上位に定義されているスタイルよりも優先されます。 たとえば、アプリケーションレベルでをに設定するを設定すると、が [`Style`](xref:Xamarin.Forms.Style) [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) `Red` に設定されたページレベルスタイルによってオーバーライドされ `Label.TextColor` `Green` ます。 同様に、ページレベルのスタイルは、コントロールレベルのスタイルによってオーバーライドされます。 また、 `Label.TextColor` がコントロールプロパティに直接設定されている場合は、すべてのスタイルよりも優先されます。

このセクションの記事では、*明示的*なスタイルと*暗黙的*なスタイルを作成して適用する方法、グローバルスタイルを作成する方法、スタイルを継承する方法、実行時にスタイルの変更に応答する方法、に含まれる組み込みのスタイルを使用する方法について説明し Xamarin.Forms ます。

> [!NOTE]
> **スタイル Id とは**
>
> 2.2 より前 Xamarin.Forms の場合、プロパティは、 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) UI テストで識別するアプリケーション内の個々の要素や、Pixate などのテーマエンジンを識別するために使用されていました。 ただし、2.2 では、プロパティが置き換えられたプロパティが導入され Xamarin.Forms [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) ました [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
