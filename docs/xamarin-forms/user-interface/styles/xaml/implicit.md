---
title: Xamarin.Forms での暗黙的なスタイル
description: 暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: 0be5c788b5be3d01234cc9a3124fa6a01ded2394
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971141"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms での暗黙的なスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。_

## <a name="create-an-implicit-style-in-xaml"></a>XAML での暗黙的なスタイルを作成します。

宣言する、 [ `Style` ](xref:Xamarin.Forms.Style)ページ レベル、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ページと、1 つ以上を追加する必要があります`Style`に宣言を含めることができます、`ResourceDictionary`します。 A`Style`される*暗黙的な*されませんを指定して、`x:Key`属性。 一致するビジュアル要素に適用されるスタイル、`TargetType`正確から派生した要素へ、`TargetType`値。

次のコード例は、*暗黙的な*スタイルがページの XAML で宣言された`ResourceDictionary`に、ページの適用と[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス。

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

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 、1 つを定義します。*暗黙的な*をページの適用されるスタイル[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス。 `Style`も他の外観のオプションの設定中に、黄色の背景に青色のテキストを表示するために使用します。 `Style`をページの追加は[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を指定せず、`x:Key`属性。 そのため、`Style`すべてに適用される、`Entry`に合うように暗黙的にインスタンス、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)のプロパティ、`Style`正確にします。 ただし、`Style`に適用されない、`CustomEntry`インスタンスの場合、サブクラス化された`Entry`します。 次のスクリーン ショットに示すように外観が発生します。

[![](implicit-images/implicit-styles.png "暗黙的なスタイル例")](implicit-images/implicit-styles-large.png#lightbox "暗黙的スタイルの例")

さらに、4 番目[ `Entry` ](xref:Xamarin.Forms.Entry)オーバーライド、 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)と[ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor)異なるする暗黙的なスタイルのプロパティ`Color`値。

### <a name="create-an-implicit-style-at-the-control-level"></a>コントロールのレベルで暗黙的なスタイルを作成します。

作成するだけでなく*暗黙的な*ページ レベルでスタイルを作成することも制御レベルでは、次のコード例に示すようにします。

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

この例で、*暗黙的な* [ `Style` ](xref:Xamarin.Forms.Style)に割り当てられている、 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)のコレクション、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)コントロール。 *暗黙的な*スタイル、コントロールとその子に適用できます。

アプリケーションのスタイルを作成する方法について[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を参照してください[グローバル スタイル](~/xamarin-forms/user-interface/styles/application.md)します。

## <a name="create-an-implicit-style-in-c35"></a>C での暗黙的なスタイルを作成します。&#35;

[`Style`](xref:Xamarin.Forms.Style) ページのインスタンスを追加することができます[ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)新しいを作成して C# でのコレクション[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、追加してから、`Style`インスタンスを`ResourceDictionary`ように、次のコード例:

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

コンストラクターは、1 つを定義します。*暗黙的な*をページの適用されるスタイル[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス。 `Style`も他の外観のオプションの設定中に、黄色の背景に青色のテキストを表示するために使用します。 `Style`をページの追加は[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を指定せず、`key`文字列。 そのため、`Style`すべてに適用される、`Entry`に合うように暗黙的にインスタンス、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)のプロパティ、`Style`正確にします。 ただし、`Style`に適用されない、`CustomEntry`インスタンスの場合、サブクラス化された`Entry`します。

## <a name="apply-a-style-to-derived-types"></a>派生型にスタイルを適用します。

[ `Style.ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)プロパティによって参照される基本型から派生したコントロールに適用されるスタイルを使用する、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)プロパティ。 そのため、このプロパティを設定`true`型がで指定された基本型から派生されている複数の種類を対象とする 1 つのスタイルを有効、`TargetType`プロパティ。

次の例では、暗黙的なスタイルの背景色を設定する[ `Button` ](xref:Xamarin.Forms.Button)赤インスタンス。

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

ページごとにこのスタイルを配置する[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)すべてに適用されていることになります[ `Button` ](xref:Xamarin.Forms.Button)インスタンスページで、またから派生したコントロールに`Button`します。 ただし場合、 [ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)プロパティが設定を解除したまま、スタイルのみに適用される`Button`インスタンス。

同等の C# コードに示します。

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
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
