---
title: Xamarin. Forms ContentView
description: この記事では、ContentView クラスを使用して、例 CardView などのカスタムコントロールを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 638402E7-CA44-456B-863B-791F6B6B561D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/14/2019
ms.openlocfilehash: 69f3311834fd438af97b3d2fa527572f02d2b0cb
ms.sourcegitcommit: fa2898d95b35fcee05503f3829351ba5a7d4a44d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2019
ms.locfileid: "74955080"
---
# <a name="xamarinforms-contentview"></a>Xamarin. Forms ContentView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)

Xamarin [`ContentView`](xref:Xamarin.Forms.ContentView)クラスは、単一の子要素を含む型の `Layout` であり、通常は、カスタムの再利用可能なコントロールを作成するために使用されます。 `ContentView` クラスは[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)から継承されます。 この記事および関連するサンプルでは、`ContentView` クラスに基づいてカスタム `CardView` コントロールを作成する方法について説明します。

次のスクリーンショットは、`ContentView` クラスから派生した `CardView` コントロールを示しています。

[![CardView サンプルアプリケーションのスクリーンショット](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

`ContentView` クラスは、1つのプロパティを定義します。

* [`Content`](xref:Xamarin.Forms.ContentView.Content)は `View` オブジェクトです。 このプロパティは、データバインディングのターゲットにできるように、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによってサポートされます。

`ContentView` は、`TemplatedView` クラスからもプロパティを継承します。

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)は、コントロールの外観を定義またはオーバーライドできる `ControlTemplate` です。

`ControlTemplate` プロパティの詳細については、「 [ControlTemplate を使用して外観をカスタマイズする](#customize-appearance-with-a-controltemplate)」を参照してください。

## <a name="create-a-custom-control"></a>カスタムコントロールを作成する

`ContentView` クラスは、それ自体ではほとんど機能を提供しませんが、カスタムコントロールを作成するために使用できます。 サンプルプロジェクトでは、`CardView` コントロールが定義されています。これは、画像、タイトル、説明をカードのようなレイアウトで表示する UI 要素です。

カスタムコントロールを作成するには、次の手順を実行します。

1. Visual Studio 2019 の `ContentView` テンプレートを使用して、新しいクラスを作成します。
1. 新しいカスタムコントロールの分離コードファイルで、一意のプロパティまたはイベントを定義します。
1. カスタムコントロールの UI を作成します。

> [!NOTE]
> XAML ではなくコードで定義されたレイアウトを持つカスタムコントロールを作成することができます。 わかりやすくするために、サンプルアプリケーションでは、XAML レイアウトを持つ単一の `CardView` クラスのみを定義しています。 ただし、サンプルアプリケーションには、コード内でカスタムコントロールを使用するプロセスを示す**Cardviewcodepage**クラスが含まれています。

### <a name="create-code-behind-properties"></a>分離コードプロパティの作成

`CardView` カスタムコントロールは、次のプロパティを定義します。

* `CardTitle`: カードに表示されるタイトルを表す `string` オブジェクト。
* `CardDescription`: カードに表示される説明を表す `string` オブジェクト。
* `IconImageSource`: カードに表示されるイメージを表す `ImageSource` オブジェクト。
* `IconBackgroundColor`: カードに表示されるイメージの背景色を表す `Color` オブジェクト。
* `BorderColor`: カードの境界線、画像の境界線、および区分線の色を表す `Color` オブジェクト。
* `CardColor`: カードの背景色を表す `Color` オブジェクト。

> [!NOTE]
> `BorderColor` プロパティは、デモンストレーションの目的で複数の項目に影響します。 このプロパティは、必要に応じて3つのプロパティに分けることができます。

各プロパティは、`BindableProperty` インスタンスによってサポートされます。 バッキング `BindableProperty` では、MVVM パターンを使用して、各プロパティのスタイル設定とバインドを行うことができます。

次の例は、バッキング `BindableProperty`を作成する方法を示しています。

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

カスタムプロパティは、`GetValue` メソッドと `SetValue` メソッドを使用して、`BindableProperty` オブジェクトの値を取得および設定します。

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

`BindableProperty` オブジェクトの詳細については、「バインド可能な[プロパティ](~/xamarin-forms/xaml/bindable-properties.md)」を参照してください。

### <a name="define-ui"></a>UI の定義

カスタムコントロール UI では、`CardView` コントロールのルート要素として `ContentView` が使用されます。 `CardView` XAML の例を次に示します。

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

`ContentView` 要素は、`x:Name` プロパティをこのに設定します。**この**プロパティを使用して、`CardView` インスタンスにバインドされたオブジェクトにアクセスできます。 プロパティのバインドセットの要素は、バインドされたオブジェクトで定義されている値に設定されます。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

> [!NOTE]
> `FallbackValue` プロパティは、バインディングが `null`場合に既定値を提供します。 これにより、Visual Studio の[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)で `CardView` コントロールを表示することもできます。

## <a name="instantiate-a-custom-control"></a>カスタムコントロールのインスタンス化

カスタムコントロールの名前空間への参照は、カスタムコントロールをインスタンス化するページに追加する必要があります。 次の例は、XAML の `ContentPage` インスタンスに追加された**コントロール**と呼ばれる名前空間参照を示しています。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

参照が追加されると、XAML で `CardView` をインスタンス化し、そのプロパティを定義できます。

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

`CardView` は、コードでインスタンス化することもできます。

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

`ContentView` クラスから派生したカスタムコントロールは、XAML やコードを使用して外観を定義したり、外観をまったく定義したりすることはできません。 外観がどのように定義されているかに関係なく、`ControlTemplate` オブジェクトはカスタムレイアウトで外観をオーバーライドできます。

`CardView` レイアウトでは、一部のユースケースに対して多くの領域が占有される可能性があります。 `ControlTemplate` では、`CardView` レイアウトをオーバーライドして、よりコンパクトなビューを提供できます。これは、要約されたリストに適しています。

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

`ControlTemplate` でのデータバインディングでは、`TemplateBinding` マークアップ拡張機能を使用してバインディングを指定します。 `ControlTemplate` プロパティは、`x:Key` 値を使用して、定義された ControlTemplate オブジェクトに設定できます。 次の例は、`CardView` インスタンスに設定された `ControlTemplate` プロパティを示しています。

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

次のスクリーンショットは、標準の `CardView` インスタンスと、`ControlTemplate` がオーバーライドされた `CardView` を示しています。

[![CardView ControlTemplate スクリーンショット](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

コントロール テンプレートの詳細については、「[Xamarin.Forms のコントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [ContentView サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)
* [Xamarin. フォームデータバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [バインド](~/xamarin-forms/xaml/bindable-properties.md)可能なプロパティ。
* [Xamarin.Forms のコントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)
