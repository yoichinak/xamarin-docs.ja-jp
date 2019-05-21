---
title: Xamarin.Forms でグローバル スタイル
description: スタイル利用できるグローバルに、アプリケーションのリソース ディクショナリに追加することで。 これは、ページまたはコントロールの間でスタイルの重複を回避するのに役立ちます。
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 665f5d1653b74997519149cef68e0882f476179d
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924607"
---
# <a name="global-styles-in-xamarinforms"></a>Xamarin.Forms でグローバル スタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_スタイル利用できるグローバルに、アプリケーションのリソース ディクショナリに追加することで。これは、ページまたはコントロールの間でスタイルの重複を回避するのに役立ちます。_

## <a name="create-a-global-style-in-xaml"></a>XAML でグローバルなスタイルを作成します。

既定では、テンプレートから作成したすべての Xamarin.Forms アプリケーションでは、**App** クラスを使用して [`Application`](xref:Xamarin.Forms.Application) サブクラスが実装されます。 宣言する、 [ `Style` ](xref:Xamarin.Forms.Style)アプリケーションのアプリケーション レベルで[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) XAML、既定値を使用して**アプリ**クラスを XAML で置き換える必要があります**アプリ**クラスと関連付けられているコード ビハインドです。 詳細については、[アプリ クラスで操作して](~/xamarin-forms/app-fundamentals/application-class.md)を参照してください。

次のコード例は、 [ `Style` ](xref:Xamarin.Forms.Style)アプリケーション レベルで宣言されています。

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

これは、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 、1 つを定義します。*明示的な*スタイル、 `buttonStyle`、の外観を設定に使用される[ `Button` ](xref:Xamarin.Forms.Button)インスタンス。 ただし、グローバルなスタイルは、*明示的な*または*暗黙的な*します。

次のコード例に示します、XAML ページを適用する、`buttonStyle`をページの[ `Button` ](xref:Xamarin.Forms.Button)インスタンス。

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

次のスクリーン ショットに示すように外観が発生します。

[![](application-images/application-styles-1.png "グローバル スタイル例")](application-images/application-styles-1-large.png#lightbox "グローバル スタイルの例")

ページのスタイルを作成する方法について[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)を参照してください[明示的なスタイル](~/xamarin-forms/user-interface/styles/explicit.md)と[暗黙的スタイル](~/xamarin-forms/user-interface/styles/implicit.md)します。

### <a name="override-styles"></a>スタイルをオーバーライドします。

スタイルのビュー階層の下位には、アップ以上定義されているものよりも優先されます。 設定など、 [ `Style` ](xref:Xamarin.Forms.Style)設定[ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor)に`Red`アプリケーションでレベルを設定 ページのレベルのスタイルによってオーバーライドされます`Button.TextColor`に`Green`. 同様に、ページ レベルのスタイルはコントロールのレベルのスタイルによってオーバーライドされます。 さらに場合、`Button.TextColor`が直接コントロール プロパティにこれは優先任意のスタイルを設定します。 この優先順位は次のコード例について説明します。

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

元の`buttonStyle`、アプリケーション レベルで定義されている、によってオーバーライドされる、`buttonStyle`ページ レベルで定義されているインスタンス。 コントロールのレベルでページ レベルのスタイルをオーバーライドするさらに、`buttonStyle`します。 そのため、 [ `Button` ](xref:Xamarin.Forms.Button)の次のスクリーン ショットに示すように青色のテキストのインスタンスが表示されます。

[![](application-images/application-styles-2.png "例のスタイルをオーバーライドする")](application-images/application-styles-2-large.png#lightbox "スタイルの例をオーバーライドします。")

## <a name="create-a-global-style-in-c35"></a>C でグローバルなスタイルを作成します。&#35;

[`Style`](xref:Xamarin.Forms.Style) アプリケーションのインスタンスを追加することができます[ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)によって新しいを作成する (C#) コレクション[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)、追加してから、`Style`インスタンスを`ResourceDictionary`、として次のコード例に示します。

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

コンストラクターは、1 つを定義します。*明示的な*に適用するためのスタイル[ `Button` ](xref:Xamarin.Forms.Button)アプリケーション全体でのインスタンス。 *明示的な* [ `Style` ](xref:Xamarin.Forms.Style)にインスタンスが追加される、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)を使用して、 [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) を指定して、メソッド`key`を参照する文字列、`Style`インスタンス。 `Style`インスタンス、アプリケーションで適切な種類のコントロールに適用できます。 ただし、グローバルなスタイルは、*明示的な*または*暗黙的な*します。

次のコード例に示します、C# のページを適用する、`buttonStyle`をページの[ `Button` ](xref:Xamarin.Forms.Button)インスタンス。

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

`buttonStyle`に適用される、 [ `Button` ](xref:Xamarin.Forms.Button)インスタンスを設定して、 [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style)プロパティの外観を制御し、`Button`インスタンス。

## <a name="related-links"></a>関連リンク

- [XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [基本的なスタイル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [スタイル (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [スタイル](xref:Xamarin.Forms.Style)
- [Set アクセス操作子](xref:Xamarin.Forms.Setter)
