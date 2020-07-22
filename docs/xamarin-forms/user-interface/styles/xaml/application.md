---
title: グローバルスタイルXamarin.Forms
description: スタイルは、アプリケーションのリソースディクショナリに追加することで、グローバルに使用できます。 これは、ページまたはコントロール間でスタイルが重複しないようにするために役立ちます。
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a222c3ee2234904cce94b52a14654728a1aa6d1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140128"
---
# <a name="global-styles-in-xamarinforms"></a>グローバルスタイルXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_スタイルは、アプリケーションのリソースディクショナリに追加することで、グローバルに使用できます。これは、ページまたはコントロール間でスタイルが重複しないようにするために役立ちます。_

## <a name="create-a-global-style-in-xaml"></a>XAML でグローバルスタイルを作成する

既定では、 Xamarin.Forms テンプレートから作成されたすべてのアプリケーションは、 **App**クラスを使用してサブクラスを実装し [`Application`](xref:Xamarin.Forms.Application) ます。 アプリケーションレベルでを宣言するには [`Style`](xref:Xamarin.Forms.Style) 、アプリケーションの [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) xaml を使用して、既定の**アプリ**クラスを xaml**アプリ**クラスおよび関連する分離コードに置き換える必要があります。 詳細については、「 [App クラス](~/xamarin-forms/app-fundamentals/application-class.md)の使用」を参照してください。

次のコード例は、アプリケーションレベルで宣言されたを示してい [`Style`](xref:Xamarin.Forms.Style) ます。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

これに [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) より、インスタンスの外観を設定するために使用される、1つの*明示的*なスタイル () が定義され `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) ます。 ただし、グローバルスタイルは*明示的*または*暗黙的*にすることができます。

次のコード例は、をページのインスタンスに適用する XAML ページを示してい `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

この結果、次のスクリーンショットに示すような外観が表示されます。

[![](application-images/application-styles-1.png "Global Styles Example")](application-images/application-styles-1-large.png#lightbox "Global Styles Example")

ページ内でスタイルを作成する方法の詳細については [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 、「[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)と[暗黙的なスタイル](~/xamarin-forms/user-interface/styles/implicit.md)」を参照してください。

### <a name="override-styles"></a>スタイルのオーバーライド

ビュー階層の下位にあるスタイルは、上位に定義されているスタイルよりも優先されます。 たとえば、アプリケーションレベルでをに設定するを設定すると、が [`Style`](xref:Xamarin.Forms.Style) [`Button.TextColor`](xref:Xamarin.Forms.Button.TextColor) `Red` に設定されたページレベルスタイルによってオーバーライドされ `Button.TextColor` `Green` ます。 同様に、ページレベルのスタイルは、コントロールレベルのスタイルによってオーバーライドされます。 また、 `Button.TextColor` がコントロールプロパティに直接設定されている場合は、すべてのスタイルよりも優先されます。 この優先順位を次のコード例に示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`buttonStyle`アプリケーションレベルで定義されている元のは、 `buttonStyle` ページレベルで定義されているインスタンスによってオーバーライドされます。 また、ページレベルのスタイルは、コントロールレベルによってオーバーライドされ `buttonStyle` ます。 そのため、 [`Button`](xref:Xamarin.Forms.Button) 次のスクリーンショットに示すように、インスタンスは青色のテキストと共に表示されます。

[![](application-images/application-styles-2.png "Overriding Styles Example")](application-images/application-styles-2-large.png#lightbox "Overriding Styles Example")

## <a name="create-a-global-style-in-c35"></a>C&#35; でグローバルスタイルを作成する

[`Style`](xref:Xamarin.Forms.Style)インスタンスは、 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 次の `Style` `ResourceDictionary` コード例に示すように、新しいを作成し、インスタンスをに追加することによって、アプリケーションのコレクションに追加できます。

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

コンストラクターは、アプリケーション全体のインスタンスに適用するための1つの*明示的*なスタイルを定義し [`Button`](xref:Xamarin.Forms.Button) ます。 *明示的* [`Style`](xref:Xamarin.Forms.Style)インスタンスは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) `key` インスタンスを参照する文字列を指定して、メソッドを使用してに追加され `Style` ます。 この `Style` インスタンスは、アプリケーション内の正しい型のコントロールに適用できます。 ただし、グローバルスタイルは*明示的*または*暗黙的*にすることができます。

次のコード例は、をページのインスタンスに適用する C# ページを示してい `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) ます。

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

は、 `buttonStyle` [`Button`](xref:Xamarin.Forms.Button) プロパティを設定することによってインスタンスに適用され、 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) インスタンスの外観を制御し `Button` ます。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本スタイル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [スタイルの使用 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Style](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
