---
title: Xamarin.Forms での暗黙的なスタイル
description: 暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 277be51c242521f52e9b1e162226ae8137e7b133
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995515"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms での暗黙的なスタイル

_暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。_

## <a name="creating-an-implicit-style-in-xaml"></a>XAML での暗黙的なスタイルの作成

宣言する、 [ `Style` ](xref:Xamarin.Forms.Style)ページ レベル、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ページと、1 つ以上を追加する必要があります`Style`に宣言を含めることができます、`ResourceDictionary`します。 A`Style`される*暗黙的な*されませんを指定して、`x:Key`属性。 一致するビジュアル要素に適用されるスタイル、`TargetType`正確から派生した要素へ、`TargetType`値。

次のコード例は、*暗黙的な*スタイルがページの XAML で宣言された`ResourceDictionary`に、ページの適用と[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス。

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

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 、1 つを定義します。*暗黙的な*をページの適用されるスタイル[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス。 `Style`も他の外観のオプションの設定中に、黄色の背景に青色のテキストを表示するために使用します。 `Style`をページの追加は[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を指定せず、`x:Key`属性。 そのため、`Style`すべてに適用される、`Entry`に合うように暗黙的にインスタンス、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)のプロパティ、`Style`正確にします。 ただし、`Style`に適用されない、`CustomEntry`インスタンスの場合、サブクラス化された`Entry`します。 次のスクリーン ショットに示すように外観が発生します。

[![](implicit-images/implicit-styles.png "暗黙的なスタイル例")](implicit-images/implicit-styles-large.png#lightbox "暗黙的スタイルの例")

さらに、4 番目[ `Entry` ](xref:Xamarin.Forms.Entry)オーバーライド、 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)と[ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor)異なるする暗黙的なスタイルのプロパティ`Color`値。

### <a name="creating-an-implicit-style-at-the-control-level"></a>コントロールでの暗黙的なスタイルを作成するレベル

作成するだけでなく*暗黙的な*ページ レベルでスタイルを作成することも制御レベルでは、次のコード例に示すようにします。

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

この例で、*暗黙的な* [ `Style` ](xref:Xamarin.Forms.Style)に割り当てられている、 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)のコレクション、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)コントロール。 *暗黙的な*スタイル、コントロールとその子に適用できます。

アプリケーションのスタイルを作成する方法について[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を参照してください[グローバル スタイル](~/xamarin-forms/user-interface/styles/application.md)します。

## <a name="creating-an-implicit-style-in-c35"></a>C での暗黙的なスタイルの作成&#35;

[`Style`](xref:Xamarin.Forms.Style) ページのインスタンスを追加することができます[ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)新しいを作成して c# でのコレクション[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、追加してから、`Style`インスタンスを`ResourceDictionary`ように、次のコード例:

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

コンス トラクターは、1 つを定義します。*暗黙的な*をページの適用されるスタイル[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス。 `Style`も他の外観のオプションの設定中に、黄色の背景に青色のテキストを表示するために使用します。 `Style`をページの追加は[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を指定せず、`key`文字列。 そのため、`Style`すべてに適用される、`Entry`に合うように暗黙的にインスタンス、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)のプロパティ、`Style`正確にします。 ただし、`Style`に適用されない、`CustomEntry`インスタンスの場合、サブクラス化された`Entry`します。

## <a name="summary"></a>まとめ

*暗黙的な*スタイルは、同一のすべてのビジュアル要素で使用される 1 つ[ `TargetType`](xref:Xamarin.Forms.Style.TargetType)スタイルを参照するには、各コントロールを必要とせずします。 A`Style`される*暗黙的な*されませんを指定して、`x:Key`属性。 代わりに、`x:Key`属性の値を自動的に、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティ。



## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
