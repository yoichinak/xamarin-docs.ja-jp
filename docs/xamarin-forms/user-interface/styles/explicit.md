---
title: "明示的なスタイル"
description: "明示的なスタイルは、そのスタイル プロパティを設定して、コントロールに適用される選択的にいずれかです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 43a1a5ee6a8bd9d53f6fd44be935ae7573db6812
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="explicit-styles"></a>明示的なスタイル

_明示的なスタイルは、そのスタイル プロパティを設定して、コントロールに適用される選択的にいずれかです。_

## <a name="creating-an-explicit-style-in-xaml"></a>XAML での明示的なスタイルの作成

宣言する、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)ページ レベルで、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ページと、1 つまたは複数を追加する必要があります`Style`に宣言を含めることができます、`ResourceDictionary`です。 A`Style`が行われます*明示的な*その宣言を付けることで、`x:Key`属性は、ことのわかりやすいキーを`ResourceDictionary`です。 *明示的な*を設定して特定の visual 要素にスタイルが適用し、必要があります、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。

次のコード例に示す*明示的な*スタイルがページの XAML で宣言された`ResourceDictionary`をページの適用と[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンス。

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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)定義 3*明示的な*をページの適用されるスタイル[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンス。 各`Style`も、フォント サイズと水平および垂直方向のレイアウト オプションを設定中に別の色でテキストを表示するために使用します。 各`Style`別に適用される`Label`を設定してその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティを使用して、`StaticResource`マークアップ拡張機能です。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](explicit-images/explicit-styles.png "明示的なスタイル例")](explicit-images/explicit-styles-large.png "明示的なスタイルの例")

さらに、最終的な[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)が、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)適用されているもオーバーライド、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)プロパティをさまざまな`Color`値。

### <a name="creating-an-explicit-style-at-the-control-level"></a>コントロールでの明示的なスタイルを作成するレベル

作成するだけでなく*明示的な*ページ レベルのスタイル、それらも作成できますコントロール レベルでは、次のコード例に示すようにします。

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

この例では、*明示的な* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)に割り当てられているインスタンス、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)のコレクション、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)コントロール。 スタイルは、コントロールとその子に適用できます。

アプリケーションのスタイルを作成する方法について[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を参照してください[グローバル スタイル](~/xamarin-forms/user-interface/styles/application.md)です。

## <a name="creating-an-explicit-style-in-c35"></a>C &#35;の明示的なスタイルを作成します。

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ページのインスタンスを追加することができます[ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)新しいを作成して c# でのコレクション[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、追加してから、`Style`インスタンスを`ResourceDictionary`のように、次のコード例:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red  }
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
                new Label { Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

3 つのコンス トラクターは、定義*明示的な*をページの適用されるスタイル[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンス。 各*明示的な* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)に追加、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を使用して、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/)メソッドを指定する、`key`を参照する文字列、`Style`インスタンス。 各`Style`別に適用される`Label`を設定して、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。

ただし、使用する利点はありません、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ここです。 代わりに、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)インスタンスに直接割り当てることができる、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)に必要なビジュアル要素のプロパティと`ResourceDictionary`次に示すように、削除することができますコードの例:

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

3 つのコンス トラクターは、定義*明示的な*をページの適用されるスタイル[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンス。 各`Style`も、フォント サイズと水平および垂直方向のレイアウト オプションを設定中に別の色でテキストを表示するために使用します。 各`Style`別に適用される`Label`を設定してその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。 さらに、最終的な`Label`が、`Style`適用されているもオーバーライド、`TextColor`を別のプロパティ`Color`値。

## <a name="summary"></a>まとめ

A [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)が行われます*明示的な*その宣言を付けることで、`x:Key`属性があり、選択的に適用するコントロールを設定して、 [ `Style`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。



## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
