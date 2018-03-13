---
title: "スタイルの概要"
description: "スタイルは、カスタマイズする視覚要素の外観を許可します。 スタイルは、特定の種類に対して定義されてし、型に使用できるプロパティの値を含むです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: ce5a7976f5bac68ca01b30a8d437aa83b8360580
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-styles"></a>スタイルの概要

_スタイルは、カスタマイズする視覚要素の外観を許可します。スタイルは、特定の種類に対して定義されてし、型に使用できるプロパティの値を含むです。_

Xamarin.Forms アプリケーションは、多くの場合と同じ外観を持つ複数のコントロールを含んでいます。 たとえば、アプリケーションの複数がある[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)を次の XAML コードの例で示すように、同じフォント オプションとレイアウト オプションを持つインスタンス。

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

次のコード例は、c# で作成された同等のページを示しています。

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

各[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスによって表示されるテキストの外観を制御するのと同じプロパティ値には、`Label`です。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](introduction-images/no-styles.png "表示スタイルをせずにラベルを付ける")](introduction-images/no-styles-large.png#lightbox "スタイルせず外観にラベルを付ける")

繰り返し発生することができます個々 のコントロールの外観を設定し、エラーが発生します。 代わりに、スタイルを作成できます、外観を定義し、必要なコントロールに適用します。

## <a name="creating-a-style"></a>スタイルを作成します。

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)クラスが複数のビジュアル要素のインスタンスに適用できるし、1 つのオブジェクトにプロパティ値のコレクションをグループ化します。 これで、反復的なマークアップを削減し、アプリケーションの外観を変更するより簡単にできます。

スタイルは、主に XAML ベース アプリケーション用に設計された、C# の場合、作成できます。

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) XAML で作成されたインスタンスが通常で定義されている、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)に割り当てられている、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 、コントロールのコレクションページで、または、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) 、アプリケーションのコレクション。
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) c# で作成されたインスタンスには、ページのクラス、またはグローバルにアクセス可能なクラスで通常定義されます。

定義する場所を選択する、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インパクトを使用できます。

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) コントロールのレベルで定義されているインスタンスは、コントロールとその子にのみ適用できます。
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ページ レベルで定義されているインスタンスは、ページとその子にのみ適用できます。
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) アプリケーション レベルで定義されているインスタンスは、アプリケーション全体に適用できます。

各[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンスには、1 つまたは複数のコレクションが含まれています[ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)オブジェクトは、各`Setter`を持つ、 [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/)および[`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/)。 `Property` 、スタイルを適用する要素のバインド可能なプロパティの名前を指定し、`Value`プロパティに適用される値です。

各[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンスできます*明示的な*、または*暗黙的な*:

- *明示的な* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンスが指定することで定義されている、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)と`x:Key`値に設定して、ターゲット要素の設定[`Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティを`x:Key`参照します。 詳細については*明示的な*スタイルを参照してください[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)です。
- *暗黙的な* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)のみを指定してインスタンスが定義されている、 [ `TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)です。 `Style`インスタンスに自動的に適用されるその型のすべての要素にします。 注のサブクラス、`TargetType`は自動的にはありません、`Style`適用します。 詳細については*暗黙的な*スタイルを参照してください[暗黙的なスタイル](~/xamarin-forms/user-interface/styles/implicit.md)です。

作成するときに、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)プロパティは常に必要です。 次のコード例は、*明示的な*スタイル (注、 `x:Key`) XAML で作成します。

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

適用する、 `Style`、ターゲット オブジェクトである必要があります、 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)に一致する、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)のプロパティの値、`Style`次の XAML コードの例のように。

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

ビュー階層内の下位のスタイルをそれ以上定義されているものよりも優先されます。 たとえば、設定、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)が設定された[ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)に`Red`アプリケーションで、レベルは、ページ レベルのスタイルで設定する上書きされている`Label.TextColor`に`Green`. 同様に、ページ レベルのスタイルは、コントロール レベル スタイルによってオーバーライドされます。 さらに場合、`Label.TextColor`が直接コントロールのプロパティでよりも優先、スタイルが設定されています。

このセクションの記事で試行して、作成して適用する方法について説明して*明示的な*と*暗黙的な*スタイル、グローバルのスタイルを作成、継承、スタイルを設定する方法、実行時にスタイルの変更に応答する方法Xamarin.Forms で含まれている組み込みのスタイルを使用する方法とします。

> [!NOTE]
> **StyleId とは何ですか。**
>
> Xamarin.Forms 2.2 より前、 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) UI テスト、および Pixate などテーマ エンジンでの識別用のアプリケーションで個々 の要素を識別するプロパティが使用されました。 ただし、Xamarin.Forms 2.2 が導入され、 [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/)プロパティは置き換え、 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/)プロパティです。 詳細については、次を参照してください。 [Xamarin.UITest と Test Cloud でテストする自動化 Xamarin.Forms](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md)です。

## <a name="summary"></a>まとめ

Xamarin.Forms アプリケーションは、多くの場合と同じ外観を持つ複数のコントロールを含んでいます。 繰り返し発生することができます個々 のコントロールの外観を設定し、エラーが発生します。 代わりに、スタイルを作成できますコントロールの外観をカスタマイズしてグループ化と設定のプロパティ、コントロール型で使用できます。


## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
