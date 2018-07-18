---
title: Xamarin.Forms のスタイルの概要
description: スタイルをカスタマイズするビジュアル要素の外観を許可します。 スタイルは、特定の種類に対して定義されてし、その型で使用できるプロパティの値を含みます。
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 8f84c960f17f56fce2a1bba143a215ce930f6f4e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996111"
---
# <a name="introduction-to-xamarinforms-styles"></a>Xamarin.Forms のスタイルの概要

_スタイルをカスタマイズするビジュアル要素の外観を許可します。スタイルは、特定の種類に対して定義されてし、その型で使用できるプロパティの値を含みます。_

Xamarin.Forms アプリケーションには、複数のコントロール同一の外観を持つには多くの場合が含まれます。 たとえば、アプリケーションの複数がある[ `Label` ](xref:Xamarin.Forms.Label)を次の XAML コード例に示すように、同じフォントのオプションとレイアウトのオプションを持つインスタンス。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
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

次のコード例では、c# で作成した同等のページを示します。

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
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

各[ `Label` ](xref:Xamarin.Forms.Label)インスタンスが同じプロパティ値によって表示されるテキストの外観を制御するため、`Label`します。 次のスクリーン ショットに示すように外観が発生します。

[![](introduction-images/no-styles.png "ラベルの外観のスタイルなし")](introduction-images/no-styles-large.png#lightbox "外観のスタイルなしのラベル")

個々 のコントロールの外観の設定は繰り返し発生することがあります、エラーが発生します。 代わりに、スタイルを作成できる外観を定義し、必要なコントロールに適用されます。

## <a name="creating-a-style"></a>スタイルを作成します。

[ `Style` ](xref:Xamarin.Forms.Style)クラスにグループ化し、複数のビジュアル要素のインスタンスに適用できる 1 つのオブジェクトにプロパティ値のコレクション。 これで、繰り返しのマークアップを削減し、アプリケーションの外観を変更するより簡単にできます。

スタイルは、主に XAML ベース アプリケーション用に設計された、c# では、作成できます。

- [`Style`](xref:Xamarin.Forms.Style) XAML で作成されたインスタンスが通常で定義されている、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)に割り当てられている、 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) 、コントロールのコレクションページで、または、 [ `Resources` ](xref:Xamarin.Forms.Application.Resources) 、アプリケーションのコレクション。
- [`Style`](xref:Xamarin.Forms.Style) ページのクラス、またはグローバルにアクセスできるクラスで、c# で作成されたインスタンスが通常定義されます。

定義する場所を選択する、 [ `Style` ](xref:Xamarin.Forms.Style)に及ぼす影響を使用できます。

- [`Style`](xref:Xamarin.Forms.Style) コントロールのレベルで定義されているインスタンスは、コントロールとその子にのみ適用できます。
- [`Style`](xref:Xamarin.Forms.Style) ページ レベルで定義されているインスタンスは、ページとその子にのみ適用できます。
- [`Style`](xref:Xamarin.Forms.Style) アプリケーション レベルで定義されているインスタンスは、アプリケーション全体で適用できます。

各[ `Style` ](xref:Xamarin.Forms.Style)インスタンスには、1 つまたは複数のコレクションが含まれています[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクトは、各`Setter`を持つ、 [ `Property` ](xref:Xamarin.Forms.Setter.Property)および[`Value`](xref:Xamarin.Forms.Setter.Value)。 `Property`にスタイルを適用するのには、要素のバインド可能なプロパティの名前を指定し、`Value`プロパティに適用される値です。

各[ `Style` ](xref:Xamarin.Forms.Style)インスタンスは*明示的な*、または*暗黙的な*:

- *明示的な* [ `Style` ](xref:Xamarin.Forms.Style)インスタンスが指定することで定義されている、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)と`x:Key`値し、ターゲット要素の設定[`Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティを`x:Key`参照。 詳細については*明示的な*スタイルを参照してください[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)します。
- *暗黙的な* [ `Style` ](xref:Xamarin.Forms.Style)のみを指定してインスタンスが定義されている、 [ `TargetType`](xref:Xamarin.Forms.Style.TargetType)します。 `Style`は、自動的にその型のすべての要素に適用されますし、インスタンス。 注そのサブクラスの`TargetType`が自動的にない、`Style`適用します。 詳細については*暗黙的な*スタイルを参照してください[暗黙的スタイル](~/xamarin-forms/user-interface/styles/implicit.md)します。

作成するときに、 [ `Style` ](xref:Xamarin.Forms.Style)、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティは常に必要です。 次のコード例は、*明示的な*スタイル (注、 `x:Key`) XAML で作成します。

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

適用する、 `Style`、ターゲット オブジェクトである必要があります、 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)と一致する、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティの値、`Style`次の XAML コード例のように。

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

スタイルのビュー階層の下位には、アップ以上定義されているものよりも優先されます。 設定など、 [ `Style` ](xref:Xamarin.Forms.Style)設定[ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor)に`Red`アプリケーションでレベルを設定ページのレベルのスタイルによってオーバーライドされます`Label.TextColor`に`Green`. 同様に、ページ レベルのスタイルはコントロールのレベルのスタイルによってオーバーライドされます。 さらに場合、`Label.TextColor`が直接コントロール プロパティをこれよりも優先任意のスタイルを設定します。

このセクションの記事に説明し、作成して適用する方法について説明します*明示的な*と*暗黙的な*スタイル、スタイルの継承、グローバルなスタイルを作成する方法、実行時にスタイルの変更に応答する方法Xamarin.Forms で含まれている組み込みのスタイルを使用する方法とします。

> [!NOTE]
> **StyleId とは何ですか。**
>
> Xamarin.Forms 2.2 では、前に、 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId)プロパティは、UI テスト、およびテーマ エンジン Pixate などの識別用のアプリケーションの個々 の要素を識別するために使用されました。 ただし、Xamarin.Forms 2.2 が導入された、 [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId)は置き換えられて、プロパティ、 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId)プロパティ。 詳細については、次を参照してください。 [Xamarin.UITest と Test Cloud による Xamarin.Forms の自動化テスト](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md)します。

## <a name="summary"></a>まとめ

Xamarin.Forms アプリケーションには、複数のコントロール同一の外観を持つには多くの場合が含まれます。 個々 のコントロールの外観の設定は繰り返し発生することがあります、エラーが発生します。 代わりに、スタイルを作成できますコントロールの外観をカスタマイズしてグループ化とコントロールの種類に使用できる設定プロパティ。


## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
