---
title: Xamarin.Forms での暗黙的なスタイル
description: 暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: cdbfaafdac8f965adaf4b840b568154e40ef7e10
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228184"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms での暗黙的なスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_暗黙的なスタイルは、いずれかのスタイルを参照するには、各コントロールを必要とせずに同じの TargetType のすべてのコントロールによって使用されます。_

## <a name="create-an-implicit-style-in-xaml"></a>XAML で暗黙的なスタイルを作成する

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

[![暗黙的なスタイルの例](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

さらに、4 番目[ `Entry` ](xref:Xamarin.Forms.Entry)オーバーライド、 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)と[ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor)異なるする暗黙的なスタイルのプロパティ`Color`値。

### <a name="create-an-implicit-style-at-the-control-level"></a>コントロールレベルでの暗黙的なスタイルの作成

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

## <a name="create-an-implicit-style-in-c35"></a>C での暗黙的なスタイルの作成&#35;

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

## <a name="apply-a-style-to-derived-types"></a>派生型へのスタイルの適用

プロパティを使用すると、 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)プロパティによって参照される基本型から派生したコントロールにスタイルを適用できます。 [`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) したがって、このプロパティを`true`に設定すると、型が`TargetType`プロパティで指定された基本型から派生していれば、1つのスタイルで複数の型を対象にすることができます。

次の例は、インスタンスの[`Button`](xref:Xamarin.Forms.Button)背景色を赤に設定する暗黙的なスタイルを示しています。

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

ページごとにこのスタイルを配置する[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)すべてに適用されていることになります[ `Button` ](xref:Xamarin.Forms.Button)インスタンスページで、またから派生したコントロールに`Button`します。 ただし、プロパティが[`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)未設定のままである場合、スタイルはインスタンス`Button`にのみ適用されます。

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
- [基本的なスタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [スタイル (サンプル) を使用します。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
