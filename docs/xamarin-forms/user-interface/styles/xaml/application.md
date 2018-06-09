---
title: Xamarin.Forms でのグローバルのスタイル
description: スタイルが利用できるグローバルに、アプリケーションのリソース ディクショナリに追加します。 これにより、ページまたはコントロールにわたってスタイルの重複を回避します。
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c5ce0f3e4ff906f9bdef06e605e71d4ed64d2a68
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245349"
---
# <a name="global-styles-in-xamarinforms"></a>Xamarin.Forms でのグローバルのスタイル

_スタイルが利用できるグローバルに、アプリケーションのリソース ディクショナリに追加します。これにより、ページまたはコントロールにわたってスタイルの重複を回避します。_

## <a name="creating-a-global-style-in-xaml"></a>XAML でグローバル スタイルの作成

既定では、テンプレートから作成されたすべての Xamarin.Forms アプリケーションを使用して、**アプリ**を実装するクラス、 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)サブクラスです。 宣言する、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)アプリケーションのアプリケーション レベルで[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) XAML、既定値を使用して**アプリ**クラスを XAML で置き換える必要があります**アプリ**クラスと関連付けられているコードの分離。 詳細については、次を参照してください。 [App クラスの使用](~/xamarin-forms/app-fundamentals/application-class.md)です。

次のコード例に示す、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)アプリケーション レベルで宣言します。

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

これは、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 、1 つの定義*明示的な*スタイル、`buttonStyle`の外観の設定に使用される[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)インスタンス。 ただし、グローバルのスタイルは、*明示的な*または*暗黙*です。

次のコード例は、適用する XAML ページを示しています、`buttonStyle`をページの[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)インスタンス。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

これは、結果、次のスクリーン ショットに示すように表示されます。

[![](application-images/application-styles-1.png "グローバル スタイル例")](application-images/application-styles-1-large.png#lightbox "グローバル スタイルの例")

ページのスタイルを作成する方法について[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を参照してください[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)と[暗黙的なスタイル](~/xamarin-forms/user-interface/styles/implicit.md)です。

### <a name="overriding-styles"></a>スタイルをオーバーライドします。

ビュー階層内の下位のスタイルをそれ以上定義されているものよりも優先されます。 たとえば、設定、 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)が設定された[ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)に`Red`アプリケーションで、レベルは、ページ レベルのスタイルで設定する上書きされている`Button.TextColor`に`Green`. 同様に、ページ レベルのスタイルは、コントロール レベル スタイルによってオーバーライドされます。 さらに場合、`Button.TextColor`が直接コントロールのプロパティにこれは優先任意のスタイルを設定します。 この優先順位は、次のコード例について説明します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
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

元の`buttonStyle`、アプリケーション レベルで定義されている、によってオーバーライドされる、`buttonStyle`ページ レベルで定義されているインスタンス。 さらに、ページ レベルのスタイルは、制御レベルによってオーバーライド`buttonStyle`です。 したがって、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)次のスクリーン ショットに示すように、青のテキストのインスタンスが表示されます。

[![](application-images/application-styles-2.png "スタイルの例をオーバーライドする")](application-images/application-styles-2-large.png#lightbox "スタイルの例をオーバーライドします。")

## <a name="creating-a-global-style-in-c35"></a>C では、グローバルなスタイルを作成します。&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) アプリケーションのインスタンスを追加することができます[ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)新しいを作成して c# でのコレクション[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)、追加してから、`Style`インスタンスを`ResourceDictionary`、として次のコード例に示します。

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

コンス トラクターは、1 つの定義*明示的な*スタイルを適用するため[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)アプリケーション全体でのインスタンス。 *明示的な* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)にインスタンスが追加される、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)を使用して、 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) を指定して、メソッド`key`を参照する文字列、`Style`インスタンス。 `Style`インスタンス アプリケーションで適切な型の任意のコントロールに適用できます。 ただし、グローバルのスタイルは、*明示的な*または*暗黙*です。

次のコード例は、C# の場合 ページの適用を示しています、`buttonStyle`をページの[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)インスタンス。

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

`buttonStyle`に適用される、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)を設定して、インスタンス、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティの外観を制御し、`Button`インスタンス。

## <a name="summary"></a>まとめ

スタイルが利用できるグローバルにそれらをアプリケーションの追加[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。 これにより、ページまたはコントロールにわたってスタイルの重複を回避します。



## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [スタイル](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Set アクセス操作子](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
