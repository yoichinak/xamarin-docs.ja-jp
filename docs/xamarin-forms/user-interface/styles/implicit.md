---
title: "暗黙的なスタイル"
description: "暗黙的なスタイルは、スタイルを参照するには、各コントロールを必要とせず、同じ TargetType のすべてのコントロールで使用される 1 つです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: b96b306c882eb30aaf8c81604afb9b6a547d715b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="implicit-styles"></a>暗黙的なスタイル

_暗黙的なスタイルは、スタイルを参照するには、各コントロールを必要とせず、同じ TargetType のすべてのコントロールで使用される 1 つです。_

## <a name="creating-an-implicit-style-in-xaml"></a>XAML での暗黙的なスタイルの作成

宣言する、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)ページ レベルで、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ページと、1 つまたは複数を追加する必要があります`Style`に宣言を含めることができます、`ResourceDictionary`です。 A`Style`が行われます*暗黙的な*されませんを指定して、`x:Key`属性。 一致する要素を視覚的に適用されるスタイル、`TargetType`正確がから派生した要素には、`TargetType`値。

次のコード例に示す、*暗黙的な*スタイルがページの XAML で宣言された`ResourceDictionary`、ページの適用と[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)インスタンス。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 、1 つの定義*暗黙的な*をページの適用されるスタイル[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)インスタンス。 `Style`も他の外観のオプションを設定中に、黄色の背景に青色のテキストを表示するために使用します。 `Style`をページの追加は[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を指定せず、`x:Key`属性。 したがって、`Style`すべてに適用されます、`Entry`インスタンスを暗黙的に一致している、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)のプロパティ、`Style`正確にします。 ただし、`Style`にも適用されず、 `CustomEntry` 、サブクラスは、このインスタンスは`Entry`します。 これは、結果、次のスクリーン ショットに示すように表示されます。

[![](implicit-images/implicit-styles.png "暗黙的なスタイル例")](implicit-images/implicit-styles-large.png#lightbox "暗黙的なスタイルの例")

さらに、4 番目[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)よりも優先、 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)と[ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/)異なるする暗黙的なスタイルのプロパティ`Color`値。

### <a name="creating-an-implicit-style-at-the-control-level"></a>コントロールでの暗黙的なスタイルを作成するレベル

作成するだけでなく*暗黙的な*ページ レベルのスタイル、それらも作成できますコントロール レベルでは、次のコード例に示すようにします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
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

この例では、*暗黙的な* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)に割り当てられている、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)のコレクション、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)コントロール。 *暗黙的な*スタイルのコントロールとその子を適用できます。

アプリケーションのスタイルを作成する方法について[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を参照してください[グローバル スタイル](~/xamarin-forms/user-interface/styles/application.md)です。

## <a name="creating-an-implicit-style-in-c35"></a>C での暗黙的なスタイルの作成&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ページのインスタンスを追加することができます[ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)新しいを作成して c# でのコレクション[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、追加してから、`Style`インスタンスを`ResourceDictionary`のように、次のコード例:

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

コンス トラクターは、1 つの定義*暗黙的な*をページの適用されるスタイル[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)インスタンス。 `Style`も他の外観のオプションを設定中に、黄色の背景に青色のテキストを表示するために使用します。 `Style`をページの追加は[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を指定せず、`key`文字列。 したがって、`Style`すべてに適用されます、`Entry`インスタンスを暗黙的に一致している、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)のプロパティ、`Style`正確にします。 ただし、`Style`にも適用されず、 `CustomEntry` 、サブクラスは、このインスタンスは`Entry`します。

## <a name="summary"></a>まとめ

*暗黙的な*スタイルは、いずれかの同一のすべてのビジュアル要素によって使用される[ `TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)スタイルを参照するには、各コントロールを必要とせずします。 A`Style`が行われます*暗黙的な*されませんを指定して、`x:Key`属性。 代わりに、`x:Key`属性は、の値に自動的になりますが、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)プロパティです。



## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
