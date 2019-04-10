---
title: Xamarin.Forms での明示的なスタイル
description: 明示的なスタイルでは、コントロールにスタイル プロパティを設定して選択的に適用される 1 つです。 この記事では、Xamarin.Forms アプリケーションで明示的なスタイルを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: fcbdeac5ebceccddee68fcca635a3935944ecac8
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55291935"
---
# <a name="explicit-styles-in-xamarinforms"></a>Xamarin.Forms での明示的なスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_明示的なスタイルでは、コントロールにスタイル プロパティを設定して選択的に適用される 1 つです。_

## <a name="create-an-explicit-style-in-xaml"></a>XAML での明示的なスタイルを作成します。

宣言する、 [ `Style` ](xref:Xamarin.Forms.Style)ページ レベル、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ページと、1 つ以上を追加する必要があります`Style`に宣言を含めることができます、`ResourceDictionary`します。 A`Style`される*明示的な*宣言の提供することにより、 `x:Key` 、属性のわかりやすいキーを与える、 `ResourceDictionary`。 *明示的な*を設定して固有のビジュアル要素にスタイルが適用しする必要があります、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティ。

次のコード例に示す*明示的な*スタイルがページの XAML で宣言された`ResourceDictionary`をページの適用と[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)定義 3*明示的な*をページの適用されるスタイル[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 各`Style`も、フォント サイズと水平方向および垂直方向のレイアウト オプションを設定中には、異なる色でテキストを表示するために使用します。 各`Style`別に適用される`Label`を設定してその[ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティを使用して、`StaticResource`マークアップ拡張機能。 次のスクリーン ショットに示すように外観が発生します。

[![](explicit-images/explicit-styles.png "明示的なスタイル例")](explicit-images/explicit-styles-large.png#lightbox "明示的なスタイルの例")

さらに、最終的な[ `Label` ](xref:Xamarin.Forms.Label)が、 [ `Style` ](xref:Xamarin.Forms.Style)それに適用されますもオーバーライド、 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)プロパティをさまざまな`Color`値。

### <a name="create-an-explicit-style-at-the-control-level"></a>コントロールのレベルで明示的なスタイルを作成します。

作成するだけでなく*明示的な*ページ レベルでスタイルを作成することも制御レベルでは、次のコード例に示すようにします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

この例で、*明示的な* [ `Style` ](xref:Xamarin.Forms.Style)に割り当てられているインスタンス、 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)のコレクション、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)コントロール。 スタイルは、コントロールとその子に適用できます。

アプリケーションのスタイルを作成する方法について[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を参照してください[グローバル スタイル](~/xamarin-forms/user-interface/styles/application.md)します。

## <a name="create-an-explicit-style-in-c35"></a>C での明示的なスタイルを作成します。&#35;

[`Style`](xref:Xamarin.Forms.Style) ページのインスタンスを追加することができます[ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)新しいを作成して C# でのコレクション[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、追加してから、`Style`インスタンスを`ResourceDictionary`ように、次のコード例:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

3 つのコンス トラクターが定義*明示的な*をページの適用されるスタイル[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 各*明示的な* [ `Style` ](xref:Xamarin.Forms.Style)に追加されます、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を使用して、 [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object))メソッドを指定する、`key`を参照する文字列、`Style`インスタンス。 各`Style`別に適用される`Label`を設定して、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティ。

ただし、使用する利点はありません、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ここです。 代わりに、 [ `Style` ](xref:Xamarin.Forms.Style)インスタンスに直接割り当てることができる、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 、必要なビジュアル要素のプロパティと`ResourceDictionary`次に示すように、削除することができますコード例:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

3 つのコンス トラクターが定義*明示的な*をページの適用されるスタイル[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 各`Style`も、フォント サイズと水平方向および垂直方向のレイアウト オプションを設定中には、異なる色でテキストを表示するために使用します。 各`Style`別に適用される`Label`を設定してその[ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティ。 さらに、最終的な`Label`が、`Style`それに適用されますもオーバーライド、`TextColor`プロパティを異なる`Color`値。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
