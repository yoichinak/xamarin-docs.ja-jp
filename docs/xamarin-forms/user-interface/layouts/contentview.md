---
title: Xamarin.FormsContentView
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46d2abf895ffe31bd1dc1c22caf36440c54b331c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130118"
---
# <a name="xamarinforms-contentview"></a>Xamarin.FormsContentView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)

クラスは、 Xamarin.Forms [`ContentView`](xref:Xamarin.Forms.ContentView) 単一の子要素を格納する型であり、 `Layout` 通常は、カスタムの再利用可能なコントロールを作成するために使用されます。 クラスは、 `ContentView` から継承さ [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) れます。 この記事および関連するサンプルでは、クラスに基づいてカスタムコントロールを作成する方法について説明し `CardView` `ContentView` ます。

次のスクリーンショットは、 `CardView` クラスから派生したコントロールを示してい `ContentView` ます。

[![CardView サンプルアプリケーションのスクリーンショット](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

クラスは、 `ContentView` 1 つのプロパティを定義します。

* [`Content`](xref:Xamarin.Forms.ContentView.Content)は `View` オブジェクトです。 このプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとして使用できるように、オブジェクトによってサポートされます。

また、は、 `ContentView` クラスからプロパティを継承し `TemplatedView` ます。

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)は、 `ControlTemplate` コントロールの外観を定義またはオーバーライドできるです。

プロパティの詳細につい `ControlTemplate` ては、「 [ControlTemplate を使用して外観をカスタマイズする](#customize-appearance-with-a-controltemplate)」を参照してください。

## <a name="create-a-custom-control"></a>カスタムコントロールを作成する

クラスは、 `ContentView` 単独ではほとんど機能を提供しませんが、カスタムコントロールを作成するために使用できます。 サンプルプロジェクトでは、 `CardView` 画像、タイトル、および説明をカードのようなレイアウトで表示する UI 要素を定義します。

カスタムコントロールを作成するには、次の手順を実行します。

1. Visual Studio 2019 でテンプレートを使用して、新しいクラスを作成し `ContentView` ます。
1. 新しいカスタムコントロールの分離コードファイルで、一意のプロパティまたはイベントを定義します。
1. カスタムコントロールの UI を作成します。

> [!NOTE]
> XAML ではなくコードで定義されたレイアウトを持つカスタムコントロールを作成することができます。 わかりやすくするために、サンプルアプリケーションでは、XAML レイアウトを持つ1つのクラスのみを定義し `CardView` ます。 ただし、サンプルアプリケーションには、コード内でカスタムコントロールを使用するプロセスを示す**Cardviewcodepage**クラスが含まれています。

### <a name="create-code-behind-properties"></a>分離コードプロパティの作成

`CardView`カスタムコントロールは、次のプロパティを定義します。

* `CardTitle`: `string` カードに表示されるタイトルを表すオブジェクト。
* `CardDescription`: `string` カードに表示される説明を表すオブジェクト。
* `IconImageSource`: `ImageSource` カードに表示されるイメージを表すオブジェクト。
* `IconBackgroundColor`: `Color` カードに表示されるイメージの背景色を表すオブジェクト。
* `BorderColor`: `Color` カードの境界線、画像の境界線、および区分線の色を表すオブジェクト。
* `CardColor`: `Color` カードの背景色を表すオブジェクト。

> [!NOTE]
> プロパティは、 `BorderColor` デモンストレーションの目的で複数の項目に影響します。 このプロパティは、必要に応じて3つのプロパティに分けることができます。

各プロパティは、インスタンスによってサポートされ `BindableProperty` ます。 バッキングでは、 `BindableProperty` MVVM パターンを使用して、各プロパティのスタイル設定とバインドを行うことができます。

次の例は、バッキングを作成する方法を示してい `BindableProperty` ます。

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

カスタムプロパティは、メソッドとメソッドを使用して `GetValue` `SetValue` 、オブジェクトの値を取得および設定し `BindableProperty` ます。

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

オブジェクトの詳細について `BindableProperty` は、「バインド可能な[プロパティ](~/xamarin-forms/xaml/bindable-properties.md)」を参照してください。

### <a name="define-ui"></a>UI の定義

カスタムコントロール UI では、 `ContentView` コントロールのルート要素としてを使用し `CardView` ます。 XAML の例を次に示し `CardView` ます。

```XAML
<ContentView ...
             x:Name="this"
             x:Class="CardViewDemo.Controls.CardView">
    <Frame BindingContext="{x:Reference this}"
            BackgroundColor="{Binding CardColor}"
            BorderColor="{Binding BorderColor}"
            ...>
        <Grid>
            ...
            <Frame BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Grey'}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle, FallbackValue='Card Title'}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     ... />
            <Label Text="{Binding CardDescription, FallbackValue='Card description text.'}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

要素は、 `ContentView` `x:Name` プロパティをこのに設定します。**これ**を使用して、インスタンスにバインドされたオブジェクトにアクセスでき `CardView` ます。 プロパティのバインドセットの要素は、バインドされたオブジェクトで定義されている値に設定されます。

データバインディングの詳細については、「 [ Xamarin.Forms データバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)」を参照してください。

> [!NOTE]
> プロパティは、 `FallbackValue` バインディングがの場合に既定値を提供し `null` ます。 これにより、Visual Studio の[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)でコントロールを表示することもでき `CardView` ます。

## <a name="instantiate-a-custom-control"></a>カスタムコントロールのインスタンス化

カスタムコントロールの名前空間への参照は、カスタムコントロールをインスタンス化するページに追加する必要があります。 次の例は、XAML のインスタンスに追加された**コントロール**と呼ばれる名前空間参照を示してい `ContentPage` ます。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

参照が追加されると、は `CardView` XAML でインスタンス化でき、そのプロパティは次のように定義されます。

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

は、 `CardView` コードでインスタンス化することもできます。

```csharp
CardView card = new CardView
{
    BorderColor = Color.DarkGray,
    CardTitle = "Slavko Vlasic",
    CardDescription = "Lorem ipsum dolor sit...",
    IconBackgroundColor = Color.SlateGray,
    IconImageSource = ImageSource.FromFile("user.png")
};
```

## <a name="customize-appearance-with-a-controltemplate"></a>ControlTemplate を使用して外観をカスタマイズする

クラスから派生したカスタムコントロールは、 `ContentView` XAML やコードを使用して外観を定義したり、外観をまったく定義したりすることはできません。 外観がどのように定義されているかに関係なく、 `ControlTemplate` オブジェクトはカスタムレイアウトで外観をオーバーライドできます。

`CardView`レイアウトによっては、一部のユースケースに対して多くの領域が占有される場合があります。 では、レイアウトをオーバーライドして `ControlTemplate` `CardView` 、よりコンパクトなビューを提供できます。これは、要約されたリストに適しています。

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="100*" />
                </Grid.ColumnDefinitions>
                <Image Grid.Row="0"
                       Grid.Column="0"
                       Source="{TemplateBinding IconImageSource}"
                       BackgroundColor="{TemplateBinding IconBackgroundColor}"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <StackLayout Grid.Row="0"
                             Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

のデータバインディングでは `ControlTemplate` 、 `TemplateBinding` マークアップ拡張機能を使用してバインディングを指定します。 その後、プロパティの値を使用して、定義された `ControlTemplate` ControlTemplate オブジェクトにプロパティを設定でき `x:Key` ます。 次の例は、インスタンスに設定されたプロパティを示してい `ControlTemplate` `CardView` ます。

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

次のスクリーンショットは、 `CardView` `CardView` がオーバーライドされた標準のインスタンスを示してい `ControlTemplate` ます。

[![CardView ControlTemplate スクリーンショット](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

コントロールテンプレートの詳細については、「 [ Xamarin.Forms コントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [ContentView サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)
* [Xamarin.Formsデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [バインド](~/xamarin-forms/xaml/bindable-properties.md)可能なプロパティ。
* [Xamarin.Formsコントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)
